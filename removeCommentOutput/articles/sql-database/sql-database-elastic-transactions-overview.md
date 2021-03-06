<properties
   pageTitle="Overview of Elastic Database Transactions with Azure SQL Database (in preview)"
   description="Overview of Elastic Database Transactions with Azure SQL Database (in preview)"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jeffreyg"
   editor="sidneyh"/>

<tags
	ms.service="sql-database"
	ms.date="11/02/2015"
	wacn.date=""/>

# Overview of Elastic Database Transactions with Azure SQL Database (in preview)

Elastic database transactions for Azure SQL Database (SQL DB) allow you to run transactions that span several databases in SQL DB. Elastic database transactions for SQL DB are available for .NET applications using ADO .NET and integrate with the familiar programming experience using the [System.Transaction](https://msdn.microsoft.com/zh-cn/library/system.transactions.aspx) classes. 

On premises, such a scenario usually required running Microsoft Distributed Transaction Coordinator (MSDTC). Since MSDTC is not available for Platform-as-a-Service application in Azure, the ability to coordinate distributed transactions has now been directly integrated into SQL DB. Applications can connect to any SQL Database to launch distributed transactions, and one of the databases will transparently coordinate the distributed transaction, as shown in the following figure. 

  ![Distributed transactions with Azure SQL Database using elastic database transactions ][1]

## Common scenarios

Elastic database transactions for SQL DB enable applications to make atomic changes to data stored in several different SQL Databases. The preview focuses on client-side development experiences in C# and .NET. A server-side experience using T-SQL is planned for a later time.  
Elastic database transactions targets the following scenarios:

* Multi-database applications in Azure: With this scenario, data is vertically partitioned across several databases in SQL DB such that different kinds of data reside on different databases. Some operations require changes to data which is kept in two or more databases. The application uses elastic database transactions to coordinate the changes across databases and ensure atomicity.

* Sharded database applications in Azure: With this scenario, the data tier uses the elastic database client library or [self-sharding](http://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx) to horizontally partition the data across many databases in SQL DB. One prominent use case is the need to perform atomic changes for a sharded multi-tenant application when changes span tenants. Think for instance of a transfer from one tenant to another, both residing on different databases. A second case is fine-grained sharding to accommodate capacity needs for a large tenant which in turn typically implies that some atomic operations needs to stretch across several databases used for the same tenant. A third case is atomic updates to reference data that are replicated across databases. Atomic, transacted, operations along these lines can now be coordinated across several databases using the preview.
Elastic database transactions use two-phase commit to ensure transaction atomicity across databases. It is a good fit for transactions that involve less than 100 databases at a time within a single transaction. These limits are not enforced, but one should expect performance and success rates for elastic database transactions to suffer when exceeding these limits.


## Installation and migration

The capabilities for elastic database transactions in SQL DB are provided through updates to the .NET libraries System.Data.dll and System.Transactions.dll. The DLLs ensure that two-phase commit is used where necessary to ensure atomicity. To start developing applications using elastic database transactions, install [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) or a later version. When running on an earlier version of the .NET framework, transactions will fail to promote to a distributed transaction and an exception will be raised.

After installation, you can use the distributed transaction APIs in System.Transactions with connections to SQL DB. If you have existing MSDTC applications using these APIs, simply rebuild your existing applications for .NET 4.6 after installing the 4.6.1 Framework. If your projects target .NET 4.6, they will automatically use the updated DLLs from the new Framework version and distributed transaction API calls in combination with connections to SQL DB will now succeed.

Remember that elastic database transactions do not require installing MSDTC. Instead, elastic database transactions are directly managed by and within SQL DB. This significantly simplifies cloud scenarios since a deployment of MSDTC is not necessary to use distributed transactions with SQL DB. Section 4 explains in more detail how to deploy elastic database transactions and the required .NET framework together with your cloud applications to Azure.

## Development experience

###	Multi-database applications

The following sample code uses the familiar programming experience with .NET System.Transactions. The TransactionScope class establishes an ambient transaction in .NET. (An âambient transactionâ is one that lives in the current thread.) All connections opened within the TransactionScope participate in the transaction. If different databases participate, the transaction is automatically elevated to a distributed transaction. The outcome of the transaction is controlled by setting the scope to complete to indicate a commit.

	using (var scope = new TransactionScope())
	{
		using (var conn1 = new SqlConnection(connStrDb1))
		{
			conn1.Open();
			SqlCommand cmd1 = conn1.CreateCommand();
			cmd1.CommandText = string.Format("insert into T1 values(1)");
			cmd1.ExecuteNonQuery();
		}

		using (var conn2 = new SqlConnection(connStrDb2))
		{
			conn2.Open();
			var cmd2 = conn2.CreateCommand();
			cmd2.CommandText = string.Format("insert into T2 values(2)");
			cmd2.ExecuteNonQuery();
		}

		scope.Complete();
	}

### Sharded database applications
 
Elastic database transactions for SQL DB also support coordinating distributed transactions where you use the OpenConnectionForKey method of the elastic database client library to open connections for a scaled out data tier. Consider cases where you need to guarantee transactional consistency for changes across several different sharding key values. Connections to the shards hosting the different sharding key values are brokered using OpenConnectionForKey. In the general case, the connections can be to different shards such that ensuring transactional guarantees requires a distributed transaction. 
The following code sample illustrates this approach. It assumes that a variable called shardmap is used to represent a shard map from the elastic database client library:

	using (var scope = new TransactionScope())
	{
		using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
		{
			conn1.Open();
			SqlCommand cmd1 = conn1.CreateCommand();
			cmd1.CommandText = string.Format("insert into T1 values(1)");
			cmd1.ExecuteNonQuery();
		}
		
		using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
		{
			conn2.Open();
			var cmd2 = conn2.CreateCommand();
			cmd2.CommandText = string.Format("insert into T1 values(2)");
			cmd2.ExecuteNonQuery();
		}

		scope.Complete();
	}


## Setup for Azure worker roles

You can automate the installation and deployment of the .NET version and libraries needed for elastic database transactions to Azure (into the guest OS of your cloud service). For Azure worker roles, use the startup tasks. The concepts and steps are documented in [Install .NET on a Cloud Service Role](/documentation/articles/cloud-services-dotnet-install-dotnet).  

Note that the installer for .NET 4.6.1 requires more temporary storage during the bootstrapping process on Azure cloud services than the installer for .NET 4.6. To ensure a successful installation, you need to increase temporary storage for your Azure cloud service in your ServiceDefinition.csdef file in the LocalResources section and the environment settings of your startup task, as shown in the following sample:

	<LocalResources>
	...
		<LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
		<LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
	</LocalResources>
	<Startup>
		<Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
			<Environment>
		...
				<Variable name="TEMP">
					<RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
				</Variable>
				<Variable name="TMP">
					<RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
				</Variable>
			</Environment>
		</Task>
	</Startup>

## Monitoring transaction status

Use Dynamic Management Views (DMVs) in SQL DB to monitor status and progress of your ongoing elastic database transactions. All DMVs related to transactions are relevant for distributed transactions in SQL DB. You can find the corresponding list of DMVs here: [Transaction Related Dynamic Management Views and Functions (Transact-SQL)](https://msdn.microsoft.com/zh-cn/library/ms178621.aspx).
 
These DMVs are particularly useful:

* **sys.dm\_tran\_active\_transactions**: Lists currently active transactions and their status. The UOW (Unit Of Work) column can identify the different child transactions that belong to the same distributed transaction. All transactions within the same distributed transaction carry the same UOW value. See the [DMV documentation](https://msdn.microsoft.com/zh-cn/library/ms174302.aspx) for more details.
* **sys.dm\_tran\_database\_transactions**: Provides additional information about transactions, such as placement of the transaction in the log. See the [DMV documentation](https://msdn.microsoft.com/zh-cn/library/ms186957.aspx) for more details.
* **sys.dm\_tran\_locks**: Provides information about the locks that are currently held by ongoing transactions. See the [DMV documentation](https://msdn.microsoft.com/zh-cn/library/ms190345.aspx) for more details.

## Limitations 

The following limitations currently apply to elastic database transactions in SQL DB:

* Only transactions across databases in SQL DB are supported. Other [X/Open XA](https://en.wikipedia.org/wiki/X/Open_XA) resource providers and databases outside of SQL DB cannot participate in elastic database transactions. That means that elastic database transactions cannot stretch across on premises SQL Server and Azure SQL Databases. For distributed transactions on premises, continue to use MSDTC. 
* Only client-coordinated transactions from a .NET application are supported. Server-side support for T-SQL such as BEGIN DISTRIBUTED TRANSACTION is planned, but not yet available. 
* Only databases on Azure SQL DB V12 are supported.
* Only databases that belong to the same logical server in SQL DB are supported.

## Learn more

Not yet using elastic database capabilities for your Azure applications? Check out our [Documentation Map](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). For questions, please reach out to us on the [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them to the [SQL Database feedback forum](http://feedback.azure.com/forums/217321-sql-database).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png




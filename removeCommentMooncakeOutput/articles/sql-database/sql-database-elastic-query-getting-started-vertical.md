<properties
	pageTitle="Getting started with cross-database queries (vertical partitioning) | Windows Azure"	
	description="how to use elastic database query with vertically partitioned databases"
	services="sql-database"
	documentationCenter=""  
	manager="jeffreyg"
	authors="sidneyh"/>

<tags
	ms.service="sql-database"
	ms.date="10/19/2015"
	wacn.date="" />

# Getting started with cross-database queries (vertical partitioning) 

Elastic database query (preview) for Azure SQL Database allows you to run T-SQL queries that span multiple databases using a single connection point. This topic applies to [vertically partitioned databases](/documentation/articles/sql-database-elastic-query-vertical-partitioning).  

When completed, you will: learn how to configure and use an Azure SQL Database to perform queries that span multiple related databases. 

For more information about the elastic database query feature, please see  [Azure SQL Database elastic database query overview](/documentation/articles/sql-database-elastic-query-overview). 

## Create the sample databases

To start with, we need to create two databases, **Customers** and **Orders**, either in the same or different logical servers.  

Execute the following queries on the **Orders** database to create the **OrderInformation** table and input the sample data. 

	CREATE TABLE [dbo].[OrderInformation]( 
		[OrderID] [int] NOT NULL, 
		[CustomerID] [int] NOT NULL 
		) 
	INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
	INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
	INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
	INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
	INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Now, execute following query on the Customers database to create the CustomerInformation table and input the sample data. 

	CREATE TABLE [dbo].[CustomerInformation]( 
		[CustomerID] [int] NOT NULL, 
		[CustomerName] [varchar](50) NULL, 
		[Company] [varchar](50) NULL 
		CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
	) 
	INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
	INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
	INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## Create database objects
### Database scoped master key and credentials


These are used to connect to the shard map manager and the shards: 

1. Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.
2. Connect to the Orders database and execute the following T-SQL commands:

		CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
		CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
		WITH IDENTITY = '<username>', 
		SECRET = '<password>';  

	The "username" and "password" should be the username and password used to login into the Customers database.

### External data sources
To create an external data source, execute the following command on the Orders database: 

	CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
		(TYPE = RDBMS, 
		LOCATION = '<server_name>.database.chinacloudapi.cn', 
		DATABASE_NAME = 'Customers', 
		CREDENTIAL = ElasticDBQueryCred, 
	) ;

### External tables
Create an external table on the Orders database, which matches the definition of the CustomerInformation table:

	CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
	( [CustomerID] [int] NOT NULL, 
	  [CustomerName] [varchar](50) NOT NULL, 
	  [Company] [varchar](50) NOT NULL) 
	WITH 
	( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## Execute a sample elastic database T-SQL query

Once you have defined your external data source and your external tables you can now use T-SQL to query your external tables. Execute this query on the Orders database: 

	SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
	FROM OrderInformation 
	INNER JOIN CustomerInformation 
	ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## Cost

Currently, the elastic database query feature is included into the cost of your Azure SQL Database.  

For pricing information see [SQL Database Pricing](/home/features/sql-database/#price). 


[AZURE.INCLUDE [elastic-scale-include](../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->

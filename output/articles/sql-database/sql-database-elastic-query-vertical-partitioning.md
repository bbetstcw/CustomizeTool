<properties
    pageTitle="Elastic database query for cross-database queries (vertical partitioning) | Windows Azure"
    description="how to set up cross-database queries over vertical partitions"    
    services="sql-database"
    documentationCenter=""  
    manager="jeffreyg"
    authors="torsteng"/>

<tags
	ms.service="sql-database"
	ms.date="01/06/2016"
	wacn.date=""/>

# Elastic database query for cross-database queries (vertical partitioning)

This document explains how to setup elastic query for cross-database querying scenarios (vertical partitioning) and how to perform your queries. For a definition of the vertical partitioning scenario, see [Azure SQL Database elastic database query overview (preview)](/documentation/articles/sql-database-elastic-query-overview).

![Query across tables in different databases][1]

## Creating database objects

For vertically partitioned scenarios, elastic query extends the current T-SQL DDL to refer to tables that are stored on remote databases. This section provides an overview of the DDL statements to configure elastic query for transparent access to remote tables. These DDL statements allow to create the metadata representation of your remote tables in the local database.  

**NOTE**:  Unlike with horizontal partitioning, these DDL statements do not depend on defining a data tier with a shard map through the elastic database client library. 

Defining the database objects for elastic database query relies on the following T-SQL statements which are explained further for the vertical partitioning scenario below: 

* [CREATE MASTER KEY](https://msdn.microsoft.com/zh-cn/library/ms174382.aspx) 

* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/zh-cn/library/mt270260.aspx)

* [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/zh-cn/library/dn935022.aspx)  

* [CREATE/DROP EXTERNAL TABLE](https://msdn.microsoft.com/zh-cn/library/dn935021.aspx) 

### 1.1 Database scoped master key and credentials 

A credential represents the user ID and password that elastic query will use to connect to your remote databases in Azure SQL DB. To create the required master key and credential use the following syntax: 

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
    
To delete the credential:
    
    DROP DATABASE SCOPED CREDENTIAL <credential_name>;  
    DROP MASTER KEY;   

 
Ensure that the *< username>* does not include any *"@servername"* suffix. 

### 1.2 External data sources

You provide the information about your remote databases to elastic query by defining external data sources. The syntax for creating and dropping external data sources is defined as follows: 

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = '<fully_qualified_server_name>',
                DATABASE_NAME = '<remote_database_name>',  
                CREDENTIAL = <credential_name> 
                ) [;] 

Note the TYPE parameter which defines this data source as RDBMS. 

You can use the following statement to drop an external data source: 

    DROP EXTERNAL DATA SOURCE <data_source_name>[;]

#### Permissions for CREATE/DROP EXTERNAL DATA SOURCE 

The user must possess ALTER ANY EXTERNAL DATA SOURCE permission. This permission is included into the ALTER DATABASE permission. 

**Example** 

The following example illustrates the use of the CREATE statement for external data sources. 

	CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
	WITH 
	( 
		TYPE=RDBMS, 
		LOCATION='myserver.database.chinacloudapi.cn', 
		DATABASE_NAME='ReferenceData', 
		CREDENTIAL= SqlUser 
	); 
 
To retrieve the list of current external data sources from the following catalog view: 

    select * from sys.external_data_sources; 

### 1.3 External Tables 

Elastic query extends the existing external table syntax to define external tables that use external data sources of type RDBMS. An external table definition for vertical partitioning covers the following aspects: 

* **Schema**: The external table DDL defines a schema that your queries can use. The schema provided in your external table definition needs to match the schema of the tables in the remote database where the actual data is stored. 

* **Remote database reference**: The external table DDL refers to an external data source. The external data source specifies the logical server name and database name of the remote database where the actual table data is stored. 

Using an external data source as outlined in the previous section, the syntax to create external tables is as follows: 

	CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
	{ WITH ( <rdbms_external_table_options> ) } 
	)[;] 
	
	<rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

The DATA_SOURCE clause defines the external data source (i.e. the remote database in case of vertical partitioning) that is used for the external table.  

The SCHEMA_NAME and OBJECT_NAME clauses provide the ability to map the external table definition to a table in a different schema on the remote database, or to a table with a different name, respectively. This is useful is you want to define an external table to a catalog view or DMV on your remote database - or any other situation where the remote table name is already taken locally.  

The following DDL statement drops an existing external table definition from the local catalog. It does not impact the remote database. 

	DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]  

**Permissions for CREATE/DROP EXTERNAL TABLE**: ALTER ANY EXTERNAL DATA SOURCE permissions are needed for external table DDL which is also needed to refer to the underlying data source.  

**Security considerations**: Users with access to the external table automatically gain access to the underlying remote tables under the credential given in the external data source definition. You should carefully manage access to the external table in order to avoid undesired elevation of privileges through the credential of the external data source. Regular SQL permissions can be used to GRANT or REVOKE access to an external table just as though it were a regular table.  


 **Example**: The following example illustrates how to create an external table:  

	CREATE EXTERNAL TABLE [dbo].[customer]( 
		[c_id] int NOT NULL, 
		[c_firstname] nvarchar(256) NULL, 
		[c_lastname] nvarchar(256) NOT NULL, 
		[street] nvarchar(256) NOT NULL, 
		[city] nvarchar(256) NOT NULL, 
		[state] nvarchar(20) NULL, 
		[country] nvarchar(50) NOT NULL, 
	) 
	WITH 
	( 
	       DATA_SOURCE = RemoteReferenceData 
	); 

The following example shows how to retrieve the list of external tables from the current database: 

	select * from sys.external_tables; 

## Querying

### 2.1 Full fidelity T-SQL queries 

Once you have defined your external data source and your external tables, you can now use full T-SQL over your external tables. 

**Example for vertical partitioning**: The following query performs a three-way join between the two local tables for orders and order lines and the remote table for customers. This is an example of the reference data use case for elastic query: 

	SELECT  	
	 c_id as customer,
	 c_lastname as customer_name,
	 count(*) as cnt_orderline, 
	 max(ol_quantity) as max_quantity,
	 avg(ol_amount) as avg_amount,
	 min(ol_delivery_d) as min_deliv_date
	FROM customer 
	JOIN orders 
	ON c_id = o_c_id
	JOIN  order_line 
	ON o_id = ol_o_id and o_c_id = ol_c_id
	WHERE c_id = 100

  
## Connectivity for tools

You can use regular SQL Server connection strings to connect your BI and data integration tools to databases on the SQL DB server that has elastic query enabled and external tables defined. Make sure that SQL Server is supported as a data source for your tool. Then refer to the elastic query database and its external tables just like any other SQL Server database that you would connect to with your tool. 

## Best practices 
 
* Make sure that the elastic query endpoint database has been given access to the remote database by enabling access for Azure Services in its SQL DB firewall configuration. Also ensure that the credential provided in the external data source definition can successfully log into the remote database and has the permissions to access the remote table.  

* Elastic query works best for queries where most of the computation can be done on the remote databases. You typically get the best query performance with selective filter predicates that can be evaluated on the remote databases or joins that can be performed completely on the remote database. Other query patterns may need to load large amounts of data from the remote database and may perform poorly. 


[AZURE.INCLUDE [elastic-scale-include](../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->

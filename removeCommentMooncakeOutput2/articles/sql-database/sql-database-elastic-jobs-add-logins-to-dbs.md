<properties 
	title="How to add a users to an elastic database pool" 
	pageTitle="How to add a users to an elastic database pool" 
	description="You must add a user with privileges to each db in the pool" 
	metaKeywords="azure sql database elastic databases credentials" 
	services="sql-database" documentationCenter=""  
	manager="jeffreyg" 
	authors="sidneyh"/>

<tags
	ms.service="sql-database"
	ms.date="07/27/2015"
	wacn.date=""/>

# How to add users to an Elastic Database pool

The **Elastic Database jobs** feature (preview) enables you to run a Transact-SQL script across a group of databases including custom-defined collection of databases, an **Elastic Database pool** or an **Elastic Database shard set** in Azure SQL Database. To run the script, a user with the appropriate permissions must be added to every database in which the job will execute. For more information, see [Managing Databases and Logins in Azure SQL Database](https://msdn.microsoft.com/zh-cn/library/azure/ee336235.aspx) or [Adding Users to Your SQL Azure Database](http://azure.microsoft.com/blog/2010/06/21/adding-users-to-your-sql-azure-database/)

## Prerequisites
* Install the [elastic job components](/documentation/articles/sql-database-elastic-jobs-service-installation). 

## How to add the users to the databases

1.	First connect to **master** of the Azure SQL Database server where the databases in your elastic database pool reside and create a new login using the same credentials provided when installing **elastic database jobs**.

		CREATE LOGIN login1 WITH password='<ProvidePassword>';

2. Log into each database in the pool and create a user using the same name and password. The user must have sufficient permissions to execute the job. This code must be run on each database.

		CREATE USER admin1 FROM LOGIN login1;
		
3. The user must have permissions as well, sufficient to execute the script specified for the job. Use the **sp_addrolemember** procedure to provide the user with the minimum required permissions for the script to execute succesfully. 

## Next steps

Create and manage jobs, see [Creating and managing elastic database jobs](/documentation/articles/sql-database-elastic-jobs-create-and-manage).

[AZURE.INCLUDE [elastic-scale-include](../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->

 
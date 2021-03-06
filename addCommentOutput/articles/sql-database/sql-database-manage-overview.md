<properties 
	pageTitle="Overview: management tools for SQL Database" 
	description="Compares tools and options for managing Azure SQL Database" 
	services="sql-database" 
	documentationCenter="" 
	authors="TigerMint" 
	manager="" 
	editor=""/>

<tags
	ms.service="sql-database"
	ms.date="04/15/2015"
	wacn.date=""/>

# Overview: management tools for SQL Database

This topic explores and compares tools and options for managing SQL <!-- deleted by customization databases --><!-- keep by customization: begin --> Databases <!-- keep by customization: end --> so you can pick the right tool for the job, your business, and you. Choosing the right tool depends on how many databases you manage, the task, and how often a task is performed.



## Azure Management Portal


The [Azure Management <!-- deleted by customization Portal](http://manage.windowsazure.cn) --><!-- keep by customization: begin --> Portal](https://manage.windowsazure.cn) <!-- keep by customization: end --> is a web-based <!-- deleted by customization Management Portal --><!-- keep by customization: begin --> management portal <!-- keep by customization: end --> where you can create, update, and delete <!-- deleted by customization databases --><!-- keep by customization: begin --> Azure SQL Databases <!-- keep by customization: end --> and logical servers and monitor database <!-- deleted by customization activity --><!-- keep by customization: begin --> resources <!-- keep by customization: end -->. This tool is great <!-- keep by customization: begin --> is <!-- keep by customization: end --> if you're just getting started with Azure, managing a small number of databases, or need to quickly do something.

For more in-depth information about using the portal see [Manage SQL Databases using the Azure Management Portal](/documentation/articles/sql-database-manage-portal).

## SQL Server Management Studio and SQL Server Data Tools in Visual Studio


SQL Server Management Studio (SSMS) and SQL Server Data Tools (SSDT) in Visual Studio are client tools that run on your computer and allow you to connect to, manage, and develop your database in the cloud. If you're an application developer familiar with Visual Studio or other integrated development environments (IDEs), <!-- deleted by customization [try --><!-- keep by customization: begin --> try <!-- keep by customization: end --> using SSDT in Visual <!-- deleted by customization Studio](https://msdn.microsoft.com/zh-cn/library/mt204009.aspx) --><!-- keep by customization: begin --> Studio <!-- keep by customization: end -->. If you need advanced SQL capabilities that are not already <!-- deleted by customization available --><!-- keep by customization: begin --> capable <!-- keep by customization: end --> in SSDT <!-- deleted by customization, --> such as managing SQL Server Databases in hybrid environments, <!-- deleted by customization you can --> use SSMS.

For more information on managing your Azure SQL Databases with SSMS <!-- keep by customization: begin --> and SSDT <!-- keep by customization: end -->, [Manage SQL Databases using SSMS](/documentation/articles/sql-database-manage-azure-ssms)


## Command line tools

<!-- deleted by customization
You can use command line tools such as PowerShell to manage databases and elastic database pools, and to automate Azure resource deployments. Microsoft recommends this tool for managing a large number of databases and automating deployment and resource changes in a production environment. 

For more information on managing your Azure SQL Databases with command line tools, [Manage SQL Database with PowerShell](/documentation/articles/sql-database-command-line-tools)

-->
<!-- keep by customization: begin -->
You can use command line tools such as PowerShell to manage Azure SQL Databases and automate Azure resource deployments. Microsoft Recommends this tool for managing a large number of Azure SQL Database and deploying resource changes in a production environment. 

For more information on managing your Azure SQL Databases with command line tools, [click here](/documentation/articles/sql-database-command-line-tools)
 
<!-- keep by customization: end -->
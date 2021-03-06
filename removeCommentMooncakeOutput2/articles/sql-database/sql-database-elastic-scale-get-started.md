<properties 
	pageTitle="Get started with elastic database tools" 
	description="Basic explanation of elastic database tools feature of Azure SQL Database, including easy to run sample app." 
	services="sql-database" 
	documentationCenter="" 
	manager="jeffreyg" 
	authors="sidneyh" 
	editor=""/>

<tags
	ms.service="sql-database"
	ms.date="07/24/2015"
	wacn.date=""/>

#Get started with Elastic Database tools

Growing and shrinking capacity on demand is one of the key cloud computing promises. For database applications, an important technique for building such scalable solutions is the pattern known as sharding – where data is physically partitioned across a number of identically structured databases.  But traditionally, building and managing applications that use sharding has required significant coding outside of the application’s business logic. 

Elastic database tools simplify creating and managing applications using database sharding in Azure SQL DB.  The tools include the elastic database client library and split-merge tool.   Together, they implement the infrastructure aspects of sharding and allow you to focus instead on the business logic of your application. 

This document introduces you to the developer experience using the elastic database client library. 

For more information about how elastic database tools work, see [Elastic Database tools overview](/documentation/articles/sql-database-elastic-scale-introduction).

For a list of all topics on elastic database tools, see the [learning map](/documentation/articles/sql-database-elastic-scale-documentation-map)

## The Elastic Database sample application

The sample creates a simple sharded application and explores key capabilities of elastic database tools. To download and run the application, follow the steps shown below or in the video [Elastic Scale - Get Started video](http://channel9.msdn.com/Blogs/Windows-Azure/Elastic-Scale-with-Azure-SQL-Database-Getting-Started). 

## Prerequisites
To run the sample app, you must use Visual Studio, and you must have access to an Azure SQL Database running on Azure. If you do not already have a subscription to Azure, sign up for a [trial subscription](/pricing/1rmb-trial/).
### Visual Studio and Nuget

1. Visual Studio 2012 or higher with C# is required. Download a free version at [Visual Studio Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2.7 or higher. To get the latest version, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)
### Create an Azure SQL database

* See [Getting Started with Windows Azure SQL Database](/documentation/articles/sql-database-get-started).

## Download and run the sample app

The **Elastic Database with Azure SQL— Getting Started** sample application illustrates the most important aspects of the development experience for sharded applications using Azure SQL elastic database tools. It focuses on key use cases for [shard map management](/documentation/articles/sql-database-elastic-scale-shard-map-management), [data dependent routing](/documentation/articles/sql-database-elastic-scale-data-dependent-routing) and [multi-shard querying](/documentation/articles/sql-database-elastic-scale-multishard-querying). To download and run the sample, follow these steps: 

1. Open Visual Studio and select **File -> New -> Project**.
2. In the dialog, click **Online**.

    ![New Project>Online][2]
3. Then click **Visual C#** under **Samples**.

    ![Click Visual C#][3]
4. In the search box, type **elastic db** to search for the sample.The title **Elastic DB Tools for Azure SQL - Getting Started** appears.

    ![Search Box][1]
 
5. Select the sample, choose a name and a location for the new project and press **OK** to create the project.
6. Open the **app.config** file in the solution for the sample project and follow the instructions in the file to add your Azure SQL database server name and your login information (user name and password).
7. Build and run the application. When asked, please allow Visual Studio to restore the NuGet packages of the solution. This will download the latest version of the elastic database client library from NuGet.
8. Play with the different options to learn more about the client library capabilities. Note the steps the application takes in the console output and feel free to explore the code behind the scenes.

    ![progress][4]

Congratulations – you have successfully built and run your first sharded application using elastic database tools on Azure SQL Database. Take a quick look at the shards that the sample created by connecting with Visual Studio or SQL Server Management Studio to your Azure DB Server. You will notice new sample shard databases and a shard map manager database that the sample has created.

**Note**   If you do not have SQL Server Management Studio, see [Managing Azure SQL Database using SQL Server Management Studio](/documentation/articles/sql-database-manage-azure-ssms), which includes instructions for getting the tool.  

### Key pieces of the code sample

1. **Managing Shards and Shard Maps**: The code illustrates how to work with shards, ranges, and mappings in file **ShardMapManagerSample.cs**. You can find more information about this topic here: [Shard Map Management](/documentation/articles/sql-database-elastic-scale-shard-map-management/).  
2. **Data Dependent Routing**: Routing of transactions to the right shard is shown in **DataDependentRoutingSample.cs**. For more details, see [Data Dependent Routing](/documentation/articles/sql-database-elastic-scale-data-dependent-routing/). 
3. **Querying over Multiple Shards**: Querying across shards is illustrated in the file **MultiShardQuerySample.cs**. For more details, see [Multi-Shard Querying](/documentation/articles/sql-database-elastic-scale-multishard-querying/).
4. **Adding empty shards**: The iterative adding of new empty shards is performed by the code in
file **AddNewShardsSample.cs**. Details of this topic are covered here: [Shard Map Management](/documentation/articles/sql-database-elastic-scale-shard-map-management/).

### Other elastic scale operations

1. **Splitting an existing shard**: The capability to split shards is provided through the **split-merge tool**. You can find more information on this tool here: [split-merge tool overview](/documentation/articles/sql-database-elastic-scale-overview-split-and-merge).
2. **Merging existing shards**: Shard merges are also performed using the **split-merge tool**. For more information, refer to: [split-merge tool overview](/documentation/articles/sql-database-elastic-scale-overview-split-and-merge).   


## Cost

The elastic database tools are free of charge. Elastic database tools does not impose additional charges on top of the cost for your Azure usage. 

For example, the sample application creates new databases. The cost depends on the Azure SQL DB database edition you choose and the Azure usage of your application.

For pricing information see [SQL Database Pricing Details](/home/features/sql-database/#price).

## Next steps
For more information about the elastic database tools, see:

* [Elastic database tools documentation map](/documentation/articles/sql-database-elastic-scale-documentation-map) 
-    Code Samples: 
    -    [Elastic DB with Azure SQL - Getting Started](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Elastic DB with Azure SQL - Integrating with Entity Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Shard Elasticity on Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [Elastic Scale Announcement](http://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Channel 9: [Elastic Scale Overview Video](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Discussion Forum: [Azure SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
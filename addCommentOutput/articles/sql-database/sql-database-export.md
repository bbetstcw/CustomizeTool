<properties
	pageTitle="Create and export a BACPAC of an Azure SQL database"
	description="Create and export a BACPAC of an Azure SQL database"
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jeffreyg"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.date="12/01/2015"
	wacn.date=""/>


# Create and export a BACPAC of an Azure SQL database

**Single database**

> [AZURE.SELECTOR]
- [Azure Management Portal](/documentation/articles/sql-database-export)
- [PowerShell](/documentation/articles/sql-database-export-powershell)

This article provides directions for exporting a BACPAC of your Azure SQL database with the [Azure Management Portal](https://manage.windowsazure.cn).

A [BACPAC](https://msdn.microsoft.com/zh-cn/library/ee210546.aspx#Anchor_4) is a .bacpac file that contains a database schema and data. The primary use case for a BACPAC is to move a database from one server to another, to [migrate a local database to the cloud](/documentation/articles/sql-database-cloud-migrate), and for archiving an existing database in an open format.

> [AZURE.NOTE] BACPACs are not intended to be used for backup and restore operations. Azure SQL Database automatically creates backups for every user database. For details, see [Business Continuity Overview](/documentation/articles/sql-database-business-continuity).


The BACPAC is exported into an Azure storage blob container that you can download once the operation successfully completes.

To complete this article you need the following:

- An Azure subscription. If you need an Azure subscription simply click **FREE TRIAL** at the top of this page, and then come back to finish this article.
- An Azure SQL Database. If you do not have a SQL database, create one following the steps in this article: [Create your first Azure SQL Database](/documentation/articles/sql-database-get-started).
- An [Azure Storage account](/documentation/articles/storage-create-storage-account) with a blob container to store the BACPAC. Currently the storage account must use the classic deployment model so choose **Classic** when creating a storage account. 


## Export your database

Open the SQL Database blade for the database you want to export.

> [AZURE.IMPORTANT] To guarantee a transactionally consistent BACPAC file you should first [create a copy of your database](/documentation/articles/sql-database-copy) and then export the database copy. 

1.	Go to the [Azure Management Portal](https://manage.windowsazure.cn).
2.	Click **BROWSE ALL**.
3.	Click **SQL databases**.
2.	Click the database you want to export as a BACPAC.
3.	In the SQL Database blade click **Export** to open the **Export database** blade:

    ![export button][1]

1.  Click **Storage** and select your storage account and blob container where the BACPAC will be stored:

    ![export database][2]

1.  Enter the **Server admin login** and **Password** for the Azure SQL server containing the database you are exporting.
1.  Click **Create** to export the database.

Clicking **Create** creates an export database request and submits it to the service. Depending on the size of your database the export operation may take some time to complete.

## Monitor the progress of the export operation

2.	Click **BROWSE ALL**.
3.	Click **SQL servers**.
2.	Click the server containing the original (source) database you just exported.
3.	In the SQL server blade click **Import/Export history**:

    ![import export history][3]
    ![import export history][4]

## Verify the BACPAC is in your storage container

2.	Click **BROWSE ALL**.
3.	Click **Storage accounts (classic)**.
2.	Click the storage account where you stored the BACPAC.
3.	Click **Containers** and select the container you exported the database into for details (you can download and save the BACPAC from here).

    ![.bacpac file details][5]	


## Next steps

- [Import an Azure SQL database](/documentation/articles/sql-database-import)



## Additional resources

- [Business Continuity Overview](/documentation/articles/sql-database-business-continuity)
- [Disaster Recovery Drills](/documentation/articles/sql-database-disaster-recovery-drills)
<!-- deleted by customization
- [SQL Database documentation](/documentation/services/sql-database/)
-->
<!-- keep by customization: begin -->
- [SQL Database documentation](/documentation/services/sql-databases)
<!-- keep by customization: end -->


<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-history.png
[4]: ./media/sql-database-export/export-status.png
[5]: ./media/sql-database-export/bacpac-details.png

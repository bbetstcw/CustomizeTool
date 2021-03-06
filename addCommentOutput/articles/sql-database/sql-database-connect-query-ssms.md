<properties
	pageTitle="Connect to SQL database by using SSMS | Windows Azure"
	description="Learn how to connect to an Azure SQL database by using SQL Server Management Studio (SSMS). Then, run a sample query using Transact-SQL (T-SQL)."
	metaCanonical=""
	keywords="connect to sql database,sql server management studio"
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jeffreyg"
	editor="" />

<tags
	ms.service="sql-database"
	ms.date="10/09/2015"
	wacn.date=""/>

<!-- deleted by customization
# Connect to SQL Database with SQL Server Management Studio and perform a sample T-SQL query
-->
<!-- keep by customization: begin -->
# Connect with SQL Server Management Studio
<!-- keep by customization: end -->

> [AZURE.SELECTOR]
- [C#](/documentation/articles/sql-database-connect-query)
- [SSMS](/documentation/articles/sql-database-connect-query-ssms)
- [Excel](/documentation/articles/sql-database-connect-excel)

<!-- deleted by customization
This article shows you how to connect to an Azure SQL database using SQL Server Management Studio (SSMS) and perform a simple query using Transact-SQL (T-SQL) statements.
-->
<!-- keep by customization: begin -->
This article shows you how to install SQL Server Management Studio (SSMS), connect to a database server in Azure, and then perform a simple query using Transact-SQL statements.
<!-- keep by customization: end -->

You'll need a SQL database in Azure first. You can create one quickly with the instructions in [Getting Started with Windows Azure SQL Database](/documentation/articles/sql-database-get-started). The examples here are based on the AdventureWorks sample database you create in that article, but the same steps, up until you perform the query, apply to any SQL database.

## Install and start SQL Server Management Studio (SSMS)

When working with SQL Database, you should use the most recent version of SSMS. See [Download SQL Server Management Studio](https://msdn.microsoft.com/zh-cn/library/mt238290.aspx) to get it. With the most recent version, SSMS automatically notifies you when the most recent update is available.

## Start SSMS and connect to your SQL database server

1. Type "Microsoft SQL Server Management Studio" in the Windows search box, and then click the desktop app to start SSMS.
2. In the **Connect to Server** dialog box, in the **Server name** box, type the name of the server that hosts your SQL database in the format *&lt;servername>*.**database.chinacloudapi.cn**.
3. Choose **SQL Server Authentication** from the **Authentication** list.
4. Type the **Login** and **Password** you set up when you created the server, and then click **Connect** <!-- deleted by customization to connect to SQL Database -->.

<!-- deleted by customization
	![SQL Server Management Studio: Connect to a SQL Database server](./media/sql-database-connect-query-ssms/1-connect.png)

### If the connection to SQL Database fails
-->
<!-- keep by customization: begin -->
	![SSMS Connect to Azure SQL Database server](./media/sql-database-connect-query-ssms/1-connect.png)

### If the connection fails
<!-- keep by customization: end -->

The most common reason for connection failures are mistakes in the server name, user name, or password, as well as the server not allowing connections for security reasons. Make sure that the firewall settings of the server allow connections from your local computer's IP address and the IP address that the SSMS client uses. Sometimes they're different.

If the connection fails because of firewall settings, the latest version of SSMS will create the firewall rule for you after asking. To get it, see [Download SSMS](https://msdn.microsoft.com/zh-cn/library/mt238290.aspx). If you're using an earlier version, the IP address is reported in an error message and you need to add this IP address to the server firewall rule. For more information, see [How to: Configure Firewall Settings (Azure SQL Database)](/documentation/articles/sql-database-configure-firewall-settings).

## Run sample queries

After you connect <!-- deleted by customization to SQL Database -->, you can run a sample query. If you didn't create the database using the AdventureWorks sample in <!-- deleted by customization [Get started --><!-- keep by customization: begin --> [Getting Started <!-- keep by customization: end --> with Windows Azure SQL Database](/documentation/articles/sql-database-get-started), this query won't work. Skip straight to Next Steps to learn more.

1. In **Object Explorer**, navigate to the **AdventureWorks** database.
2. Right-click the database and then select **New Query**.

	![New query](./media/sql-database-connect-query-ssms/4-run-query.png)

3. In the query window, copy and paste the following code.

		SELECT
		CustomerId
		,Title
		,FirstName
		,LastName
		,CompanyName
		FROM SalesLT.Customer;

4. Click the **Execute** button.  The following screen shot shows a successful query.

	![Sucess](./media/sql-database-connect-query-ssms/5-success.png)

## Next steps

You can use <!-- deleted by customization T-SQL --><!-- keep by customization: begin --> Transact-SQL <!-- keep by customization: end --> statements to create and manage databases in Azure in much the same way you can with SQL Server. If you're familiar with using <!-- deleted by customization T-SQL --><!-- keep by customization: begin --> Transact-SQL <!-- keep by customization: end --> with SQL Server, see [Azure SQL Database Transact-SQL information)](/documentation/articles/sql-database-transact-sql-information) for a summary of differences.

If you're new to <!-- deleted by customization T-SQL --><!-- keep by customization: begin --> Transact-SQL <!-- keep by customization: end -->, see [Tutorial: Writing Transact-SQL Statements](https://msdn.microsoft.com/zh-cn/library/ms365303.aspx) and the [Transact-SQL Reference (Database Engine)](https://msdn.microsoft.com/zh-cn/library/bb510741.aspx).
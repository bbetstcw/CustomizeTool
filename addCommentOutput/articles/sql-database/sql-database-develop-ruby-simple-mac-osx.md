<properties
	pageTitle="Connect to SQL Database by using Ruby with TinyTDS on Mac OS X (Yosemite)"
	description="Give a Ruby code sample you can run on Mac OS X (Yosemite) to connect to Azure SQL Database."
	services="sql-database"
	documentationCenter=""
	authors="ajlam"
	manager="jeffreyg"
	editor=""/>


<tags
	ms.service="sql-database"
	ms.date="12/17/2015"
	wacn.date=""/>


# Connect to SQL Database by using Ruby on Mac OS X (Yosemite)


> [AZURE.SELECTOR]
- [Node.js](/documentation/articles/sql-database-develop-nodejs-simple-mac)
- [Python](/documentation/articles/sql-database-develop-python-simple-mac-osx)
- [Ruby](/documentation/articles/sql-database-develop-ruby-simple-mac-osx)


This topic presents a Ruby code sample that runs on Mac computer running Yosemite to connect to an Azure SQL Database database.

## Prerequisites

### Install the required modules

Open your terminal and install the following:

**1) Homebrew**: Run the following command from your terminal. This will download the Homebrew package manager on your machine.

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

**2) FreeTDS:** Run the following command from your terminal. This will install FreeTDS on your machine and is required for TinyTDS to work.

    brew install FreeTDS

**3) TinyTDS:** Run the following command from your terminal. This will install TinyTDS on your machine.

    gem install tiny_tds

### A SQL database

See the [getting started page](/documentation/articles/sql-database-get-started) to learn how to create a sample database.  It is important you follow the guide to create an **AdventureWorks database template**. The samples shown below only work with the **AdventureWorks schema**.


## Step 1: Get Connection Details

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## Step 2:  Connect

The [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) function is used to connect to SQL Database.

    require 'tiny_tds'
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',
    host: 'yourserver.database.chinacloudapi.cn', port: 1433,
    database: 'AdventureWorks', azure:true

## Step 3:  Execute a query

The [TinyTds::Result](https://github.com/rails-sqlserver/tiny_tds) function is used to retrieve a result set from a query against SQL Database. This function accepts a query and returns a result set. The results set is iterated over by using [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds).

    require 'tiny_tds'  
    print 'test'     
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',
    host: 'yourserver.database.chinacloudapi.cn', port: 1433,
    database: 'AdventureWorks', azure:true
    results = client.execute("select * from SalesLT.Product")
    results.each do |row|
    puts row
    end

## Step 4:  Insert a row

In this example you will see how to execute an [INSERT](https://msdn.microsoft.com/zh-cn/library/ms174335.aspx) statement safely, pass parameters which protect your application from [SQL injection](https://technet.microsoft.com/zh-cn/library/ms161953(v=sql.105).aspx) vulnerability, and retrieve the auto-generated [Primary Key](https://msdn.microsoft.com/zh-cn/library/ms179610.aspx) value.  


To use TinyTDS with Azure, it is recommended that you execute several `SET` statements to change how the current session handles specific information. Recommended `SET` statements are provided in the code sample. For example, `SET ANSI_NULL_DFLT_ON` will allow new columns created to allow null values even if the nullability status of the column is not explicitly stated.

To align with the Microsoft SQL Server [datetime](http://msdn.microsoft.com/zh-cn/library/ms187819.aspx) format, use the [strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) function to cast to the corresponding datetime format.

    require 'tiny_tds'
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',
    host: 'yourserver.database.chinacloudapi.cn', port: 1433,
    database: 'AdventureWorks', azure:true
    results = client.execute("SET ANSI_NULLS ON")
    results = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
    results = client.execute("SET ANSI_NULL_DFLT_ON ON")
    results = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
    results = client.execute("SET ANSI_PADDING ON")
    results = client.execute("SET QUOTED_IDENTIFIER ON")
    results = client.execute("SET ANSI_WARNINGS ON")
    results = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")
    require 'date'
    t = Time.now
    curr_date = t.strftime("%Y-%m-%d %H:%M:%S.%L")
    results = client.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)
    OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, '#{curr_date}' )")
    results.each do |row|
    puts row
    end

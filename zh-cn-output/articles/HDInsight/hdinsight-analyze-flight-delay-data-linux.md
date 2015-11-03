<properties 
	pageTitle="使用 HDInsight 中的 Hadoop 分析航班延误数据 | Windows Azure" 
	description="了解如何使用一个 Windows PowerShell 脚本来设置 HDInsight 群集、运行 Hive 作业、运行 Sqoop 作业和删除群集。" 
	services="hdinsight" 
	documentationCenter="" 
	authors="Blackmist" 
	manager="paulettm" 
	editor="cgronlun"
	tags="azure-portal"/>

<tags 
	ms.service="hdinsight" 
	ms.date="08/04/2015" 
	wacn.date=""/>

#使用 HDInsight 中的 Hive 分析航班延误数据

了解如何在基于 Linux 的 HDInsight（预览版）上使用 Hive 分析航班延误数据，然后使用 Sqoop 将数据导出到 Azure SQL 数据库中。

> [AZURE.NOTE]尽管可以将本文档的各个部分（例如，Python 和 Hive）用于基于 Windows 的 HDInsight 群集，但很多步骤特定于基于 Linux 的群集。有关适用于基于 Windows 的群集的步骤，请参阅[在 HDInsight 中使用 Hive 分析航班延误数据](/documentation/articles/hdinsight-analyze-flight-delay-data)

###先决条件

在开始阅读本教程前，你必须具有：

- **一个 Azure 订阅**。请参阅[获取 Azure 免费试用版](/pricing/1rmb-trial/)。

- __一个 HDInsight 群集__。有关创建新的基于 Linux 的 HDInsight 群集的步骤，请参阅 [在 Linux 上的 HDInsight 中开始将 Hadoop 与 Hive 配合使用](/documentation/articles/hdinsight-hadoop-linux-tutorial-get-started)。

- __Azure SQL 数据库__。你将使用 Azure SQL 数据库作为目标数据存储。如果你还没有 SQL 数据库，请参阅[如何创建和配置 Azure SQL 数据库](/documentation/articles/sql-database-create-configure)以创建一个

- __Azure CLI__。如果你尚未安装 Azure CLI，请参阅[安装和配置 Azure CLI](/documentation/articles/xplat-cli) 了解详细步骤。


##下载航班数据

1. 浏览到[美国研究与技术创新管理部门 - 运输统计局][rita-website]。
2. 在该页面上，选择以下值：

	|名称 |值 |
	| 筛选年份 | 2013 |
	| 筛选期间 | 1 月 |
	| 字段 | Year、FlightDate、UniqueCarrier、Carrier、FlightNum、OriginAirportID、Origin、OriginCityName、OriginState、DestAirportID、Dest、DestCityName、DestState、DepDelayMinutes、ArrDelay、ArrDelayMinutes、CarrierDelay、WeatherDelay、NASDelay、SecurityDelay、LateAircraftDelay。清除所有其他字段 |

3. 单击“下载”。

##上载数据

1. 使用以下命令将该 zip 文件上载到 HDInsight 群集头节点：

		scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.cn:

	将 __FILENAME__ 替换为 zip 文件的名称。将 __USERNAME__ 替换为 HDInsight 群集的 SSH 登录名。将 CLUSTERNAME 替换为 HDInsight 群集的名称。
	
	> [AZURE.NOTE]如果你使用密码对 SSH 登录名进行身份验证，则系统将提示你输入密码。如果你使用了公钥，则可能需要使用 `-i` 参数并指定匹配私钥的路径。例如，`scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.cn:`。

2. 上载完成后，使用 SSH 连接到群集：

		ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.cn
		
	有关将 SSH 与基于 Linux 的 HDInsight 配合使用的详细信息，请参阅以下文章：
	
	* [在 Linux、Unix 或 OS X 中的 HDInsight 上将 SSH 与基于 Linux 的 Hadoop 配合使用](/documentation/articles/hdinsight-hadoop-linux-use-ssh-unix)

	* [在 Windows 中的 HDInsight 上将 SSH 与基于 Linux 的 Hadoop 配合使用](/documentation/articles/hdinsight-hadoop-linux-use-ssh-windows)
	
3. 连接后，使用以下命令来解压缩 .zip 文件：

		unzip FILENAME.zip
	
	它将提取大小约为 60MB 的 .csv 文件。
	
4. 使用以下命令在 WASB（由 HDInsight 使用的分布式数据存储）上创建一个新目录并复制该文件：

	hadoop fs -mkdir -p /tutorials/flightdelays/data
	hadoop fs -copyFromLocal FILENAME.csv /tutorials/flightdelays/data/FILENAME.csv
	
##创建并运行 HiveQL

使用以下步骤将 CSV 文件中的数据导入到名为 __Delays__ 的 Hive 表中。

1. 使用以下项创建名为 __flightdelays.hql__ 的新文件并编辑它：

		nano flightdelays.hql
		
	使用以下项作为此文件的内容：
	
		DROP TABLE delays_raw;
		-- Creates an external table over the csv file
		CREATE EXTERNAL TABLE delays_raw (
			YEAR string,
			FL_DATE string,
			UNIQUE_CARRIER string,
			CARRIER string,
			FL_NUM string,
			ORIGIN_AIRPORT_ID string,
			ORIGIN string,
			ORIGIN_CITY_NAME string,
			ORIGIN_CITY_NAME_TEMP string,
			ORIGIN_STATE_ABR string,
			DEST_AIRPORT_ID string,
			DEST string,
			DEST_CITY_NAME string,
			DEST_CITY_NAME_TEMP string,
			DEST_STATE_ABR string,
			DEP_DELAY_NEW float,
			ARR_DELAY_NEW float,
			CARRIER_DELAY float,
			WEATHER_DELAY float,
			NAS_DELAY float,
			SECURITY_DELAY float,
			LATE_AIRCRAFT_DELAY float)
		-- The following lines describe the format and location of the file
		ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
		LINES TERMINATED BY '\n'
		STORED AS TEXTFILE
		LOCATION '/tutorials/flightdelays/data';
		
		-- Drop the delays table if it exists
		DROP TABLE delays;
		-- Create the delays table and populate it with data
		-- pulled in from the CSV file (via the external table defined previously)
		CREATE TABLE delays AS
		SELECT YEAR AS year,
		    FL_DATE AS flight_date,
		    substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
		    substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
		    substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
		    ORIGIN_AIRPORT_ID AS origin_airport_id,
		    substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
		    substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
		    substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
		    DEST_AIRPORT_ID AS dest_airport_id,
		    substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
		    substring(DEST_CITY_NAME,2) AS dest_city_name,
		    substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
		    DEP_DELAY_NEW AS dep_delay_new,
		    ARR_DELAY_NEW AS arr_delay_new,
		    CARRIER_DELAY AS carrier_delay,
		    WEATHER_DELAY AS weather_delay,
		    NAS_DELAY AS nas_delay,
		    SECURITY_DELAY AS security_delay,
		    LATE_AIRCRAFT_DELAY AS late_aircraft_delay
		FROM delays_raw;
		
2. 使用 __Ctrl + X__，然后单击 __Y__ 以保存该文件。

3. 使用以下命令启动 Hive 并运行 __flightdelays.hql__ 文件：

		hive -i flightdelays.hql
		
	这将运行该文件，然后返回 `hive>` 提示。

4. 当你收到 `hive>` 提示时，请使用以下命令从导入的航班延误数据中检索数据。

		INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
		SELECT regexp_replace(origin_city_name, '''', ''),
			avg(weather_delay)
		FROM delays
		WHERE weather_delay IS NOT NULL
		GROUP BY origin_city_name;

	这将检索遇到天气延迟的城市的列表，以及平均延迟时间并将其保存到 `/tutorials/flightdelays/output` 中。稍后，Sqoop 将从该位置读取数据并将其导出到 Azure SQL 数据库中。

##创建 SQL 数据库

按照以下步骤创建 Azure SQL 数据库。这将用于保存通过 Sqoop 从 HDInsight 导出的数据。

1. 从安装了 Azure CLI 的开发环境，使用以下命令创建新的 Azure SQL 数据库：

		azure sql server create <adminLogin> <adminPassword> <region>

	例如，`azure sql server create admin password "China North"`。

	该命令完成后，你将收到如下响应：

		info:    Executing command sql server create
		+ Creating SQL Server
		data:    Server Name i1qwc540ts
		info:    sql server create command OK

> [AZURE.IMPORTANT]请记下此命令返回的服务器名称。这是已创建的 SQL 数据库服务器的短名称。完全限定域名 (FQDN) 是 `<shortname>.database.chinacloudapi.cn`。

2. 使用以下命令在 SQL 数据库服务器上创建一个名为 **sqooptest** 的数据库：

        azure sql db create [options] <serverName> sqooptest <adminLogin> <adminPassword>

    此命令在其完成后将返回“OK”消息。

	> [AZURE.NOTE]如果你收到指示无权访问的错误，则可能需要使用以下命令将客户端工作站的 IP 地址添加到 SQL 数据库防火墙：
	>
	> `sql firewallrule create [options] <serverName> <ruleName> <startIPAddress> <endIPAddress>`

##创建 SQL 数据库表

> [AZURE.NOTE]有多种方法可连接到 SQL 数据库以创建表。以下步骤从 HDInsight 群集使用 [FreeTDS](http://www.freetds.org/)。

1. 使用 SSH 连接到基于 Linux 的 HDInsight 群集，并从 SSH 会话运行以下步骤。

3. 使用以下命令安装 FreeTDS：

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. 安装 FreeTDS 后，使用以下命令连接到你先前创建的 SQL 数据库服务器：

        TDSVER=8.0 tsql -H <serverName>.database.chinacloudapi.cn -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    你将收到如下输出：

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. 在 `1>` 提示符下，输入以下行：

        CREATE TABLE [dbo].[delays](
		[origin_city_name] [nvarchar](/documentation/articles/50) NOT NULL,
		[weather_delay] float,
		CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
		([origin_city_name] ASC))
		GO

    输入 `GO` 语句后，将评估前面的语句。这将创建一个名为 __delays__ 且具有聚集索引（SQL 数据库所必需）的新表。

    使用以下命令验证是否已创建该表：

        SELECT * FROM information_schema.tables
        GO

    您应该会看到与下面类似的输出：

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     delays      BASE TABLE

8. 在 `1>` 提示符下输入 `exit` 以退出 tsql 实用工具。
	
##使用 Sqoop 导出数据

1. 使用以下命令从 Sqoop lib 目录创建到 SQL Server JDBC 驱动程序的链接。这样一来，Sqoop 就可以使用该驱动程序与 SQL 数据库通信：

		sudo ln /usr/share/java/sqljdbc_4.1/enu/sqljdbc4.jar /usr/hdp/current/sqoop-client/lib/sqljdbc4.jar

2. 使用以下命令验证 Sqoop 是否可以看到你的 SQL 数据库：

		sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.chinacloudapi.cn:1433 --username <adminLogin> --password <adminPassword>

	这将返回数据库列表，其中包括你先前创建的 sqooptest 数据库。

3. 使用以下命令将 hivesampletable 中的数据导出到 mobiledata 表：

		sqoop export --connect 'jdbc:sqlserver://<serverName>.database.chinacloudapi.cn:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasb:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

	此命令将指示 Sqoop 连接到 SQL 数据库中的 sqooptest 数据库，并将数据从 wasb:///tutorials/flightdelays/output（我们先前存储 hive 查询输出的位置）导出到 delays 表。

4. 该命令完成后，使用以下命令通过 TSQL 连接到数据库：

		TDSVER=8.0 tsql -H <serverName>.database.chinacloudapi.cn -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

	连接成功以后，使用以下语句验证数据是否已导出到 mobiledata 表：
	
		SELECT * FROM delays
		GO

	你会在表中看到一系列数据。键入 `exit` 退出 tsql 实用程序。

##<a id="nextsteps"></a>后续步骤
现在你已了解如何执行以下操作：将文件上载到 Azure Blob 存储、使用 Azure Blob 存储中的数据填充 Hive 表、运行 Hive 查询以及使用 Sqoop 将数据从 HDFS 导出到 Azure SQL 数据库。若要了解更多信息，请参阅下列文章：

* [HDInsight 入门][hdinsight-get-started]
* [将 Hive 与 HDInsight 配合使用][hdinsight-use-hive]
* [将 Oozie 与 HDInsight 配合使用][hdinsight-use-oozie]
* [将 Sqoop 与 HDInsight 配合使用][hdinsight-use-sqoop]
* [将 Pig 与 HDInsight 配合使用][hdinsight-use-pig]
* [为 HDInsight 开发 Java MapReduce 程序][hdinsight-develop-mapreduce]
* [为 HDInsight 开发 Python Hadoop 流式处理程序][hdinsight-develop-streaming]



[azure-purchase-options]: /pricing/overview/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: /pricing/1rmb-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
[powershell-install-configure]: /documentation/articles/install-configure-powershell
[hdinsight-use-oozie]: /documentation/articles/hdinsight-use-oozie-linux-mac
[hdinsight-use-hive]: /documentation/articles/hdinsight-use-hive
[hdinsight-provision]: /documentation/articles/hdinsight-provision-clusters
[hdinsight-storage]: /documentation/articles/hdinsight-hadoop-use-blob-storage
[hdinsight-upload-data]: /documentation/articles/hdinsight-upload-data
[hdinsight-get-started]: /documentation/articles/hdinsight-hadoop-linux-tutorial-get-started
[hdinsight-use-sqoop]: /documentation/articles/hdinsight-use-sqoop
[hdinsight-use-pig]: /documentation/articles/hdinsight-use-pig
[hdinsight-develop-streaming]: /documentation/articles/hdinsight-hadoop-streaming-python
[hdinsight-develop-mapreduce]: /documentation/articles/hdinsight-develop-deploy-java-mapreduce-linux
[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 

<!---HONumber=74-->
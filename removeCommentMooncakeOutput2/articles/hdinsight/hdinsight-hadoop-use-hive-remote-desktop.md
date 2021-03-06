<properties
   pageTitle="Use Hadoop Hive and Remote Desktop in HDInsight | Windows Azure"
   description="Learn how to connect to Hadoop cluster in HDInsight by using Remote Desktop, and then run Hive queries by using the Hive Command-Line Interface."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.date="10/09/2015"
	wacn.date=""/>

# Use Hive with Hadoop on HDInsight with Remote Desktop

[AZURE.INCLUDE [hive-selector](../includes/hdinsight-selector-use-hive.md)]

In this article, you will learn how to connect to an HDInsight cluster by using Remote Desktop, and then run Hive queries by using the Hive Command-Line Interface (CLI).

> [AZURE.NOTE] This document does not provide a detailed description of what the HiveQL statements that are used in the examples do. For information about the HiveQL that is used in this example, see [Use Hive with Hadoop on HDInsight](/documentation/articles/hdinsight-use-hive).

##<a id="prereq"></a>Prerequisites

To complete the steps in this article, you will need the following:

* A Windows-based HDInsight (Hadoop on HDInsight) cluster

* A client computer running Windows 10, Window 8, or Windows 7

##<a id="connect"></a>Connect with Remote Desktop

Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at [Connect to HDInsight clusters using RDP](/documentation/articles/hdinsight-administer-use-management-portal-v1#rdp).

##<a id="hive"></a>Use the Hive command

When you have connected to the desktop for the HDInsight cluster, use the following steps to work with Hive:

1. From the HDInsight desktop, start the **Hadoop Command Line**.

2. Enter the following command to start the Hive CLI:

        %hive_home%\bin\hive

    When the CLI has started, you will see the Hive CLI prompt: `hive>`.

3. Using the CLI, enter the following statements to create a new table named **log4jLogs** using sample data:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    These statements perform the following actions:

    * **DROP TABLE**: Deletes the table and the data file if the table already exists.

    * **CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive. External tables store only the table definition in Hive (the data is left in the original location).

		> [AZURE.NOTE] External tables should be used when you expect the underlying data to be updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries to use the latest data.
    	>
    	> Dropping an external table does **not** delete the data, only the table definition.

	* **ROW FORMAT**: Tells Hive how the data is formatted. In this case, the fields in each log are separated by a space.

    * **STORED AS TEXTFILE LOCATION**: Tells Hive where the data is stored (the example/data directory) and that it is stored as text.

    * **SELECT**: Selects a count of all rows where column **t4** contains the value **[ERROR]**. This should return a value of **3** because there are three rows that contain this value.

    * **INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log. This restricts the search to the sample.log file that contains the data, and keeps it from returning data from other example data files that do not match the schema we defined.


4. Use the following statements to create a new 'internal' table named **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    These statements perform the following actions:

    * **CREATE TABLE IF NOT EXISTS**: Creates a table if it does not already exist. Because the **EXTERNAL** keyword is not used, this is an internal table, which is stored in the Hive data warehouse and is managed completely by Hive.

		> [AZURE.NOTE] Unlike **EXTERNAL** tables, dropping an internal table also deletes the underlying data.

    * **STORED AS ORC**: Stores the data in optimized row columnar (ORC) format. This is a highly optimized and efficient format for storing Hive data.

    * **INSERT OVERWRITE ... SELECT**: Selects rows from the **log4jLogs** table that contain **[ERROR]**, then inserts the data into the **errorLogs** table.

    To verify that only rows that contain **[ERROR]** in column t4 were stored to the **errorLogs** table, use the following statement to return all the rows from **errorLogs**:

        SELECT * from errorLogs;

    Three rows of data should be returned, all containing **[ERROR]** in column t4.

##<a id="summary"></a>Summary

As you can see, the the Hive command provides an easy way to interactively run Hive queries on an HDInsight cluster, monitor the job status, and retrieve the output.

##<a id="nextsteps"></a>Next steps

For general information about Hive in HDInsight:

* [Use Hive with Hadoop on HDInsight](/documentation/articles/hdinsight-use-hive)

For information about other ways you can work with Hadoop on HDInsight:

* [Use Pig with Hadoop on HDInsight](/documentation/articles/hdinsight-use-pig)

* [Use MapReduce with Hadoop on HDInsight](/documentation/articles/hdinsight-use-mapreduce)


[1]: /documentation/articles/hdinsight-hadoop-visual-studio-tools-get-started
[hdinsight-sdk-documentation]: http://msdn.microsoft.com/zh-cn/library/dn479185.aspx

[azure-purchase-options]: /pricing/overview/
[azure-member-offers]: /pricing/member-offers/
[azure-trial]: /pricing/1rmb-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: /documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: /documentation/articles/hdinsight-use-oozie
[hdinsight-analyze-flight-data]: /documentation/articles/hdinsight-analyze-flight-delay-data
[hdinsight-storage]: /documentation/articles/hdinsight-use-blob-storage
[hdinsight-provision]: /documentation/articles/hdinsight-provision-clusters
[hdinsight-submit-jobs]: /documentation/articles/hdinsight-submit-hadoop-jobs-programmatically
[hdinsight-upload-data]: /documentation/articles/hdinsight-upload-data
[hdinsight-get-started]: /documentation/articles/hdinsight-get-started
[Powershell-install-configure]: /documentation/articles/powershell-install-configure
[powershell-here-strings]: http://technet.microsoft.com/zh-cn/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png

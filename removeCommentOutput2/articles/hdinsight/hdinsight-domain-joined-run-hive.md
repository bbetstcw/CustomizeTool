<!-- not suitable for Mooncake -->

<properties
    pageTitle="Configure Hive policies in Domain-joined HDInsight | Azure"
    description="Learn ...."
    services="hdinsight"
    documentationcenter=""
    author="saurinsh"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal" />
<tags
    ms.assetid="3fade1e5-c2e1-4ad5-b371-f95caea23f6d"
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/25/2016"
    wacn.date=""
    ms.author="saurinsh" />

# Configure Hive policies in Domain-joined HDInsight (Preview)
Learn how to configure Apache Ranger policies for Hive. In this article, you create two Ranger policies to restrict access to the hivesampletable. The hivesampletable comes with HDInsight clusters. After you have configured the policies, you use Excel and ODBC driver to connect to Hive tables in HDInsight.

## Prerequisites
* A Domain-joined HDInsight cluster. See [Configure Domain-joined HDInsight clusters](/documentation/articles/hdinsight-domain-joined-configure/).
* A workstation with Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.

## Connect to Apache Ranger Admin UI
**To connect to Ranger Admin UI**

1. From a browser, connect to Ranger Admin UI. The URL is https://&lt;ClusterName>.azurehdinsight.cn/Ranger/. 
   
   > [AZURE.NOTE]
   > Ranger uses different credentials than Hadoop cluster. To prevent browsers using cached Hadoop credentials, use new inprivate browser window to connect to the Ranger Admin UI.
   > 
   > 
2. Log in using the cluster administrator domain user name and password:
   
    ![HDInsight Domain-joined Ranger home page](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)
   
    Currently, Ranger only works with Yarn and Hive.

## Create Domain users
In [Configure Domain-joined HDInsight clusters](/documentation/articles/hdinsight-domain-joined-configure/#create-and-configure-azure-ad-ds-for-your-azure-ad), you have created hiveruser1 and hiveuser2. You will use the two user account in this tutorial.

## Create Ranger policies
In this section, you will create two Ranger policies for accessing hivesampletable. You give select permission on different set of columns. Both users were created in [Configure Domain-joined HDInsight clusters](/documentation/articles/hdinsight-domain-joined-configure/#create-and-configure-azure-ad-ds-for-your-azure-ad).  In the next section, you will test the two policies in Excel.

**To create Ranger policies**

1. Open Ranger Admin UI. See [Connect to Apache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).
2. Click **&lt;ClusterName>_hive**, under **Hive**. You shall see two pre-configure policies.
3. Click **Add New Policy**, and then enter the following values:
   
   * Policy name: read-hivesampletable-all
   * Hive Database: default
   * table: hivesampletable
   * Hive column: *
   * Select User: hiveuser1
   * Permissions: select
     
     ![HDInsight Domain-joined Ranger Hive policy configure](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).
     
     > [AZURE.NOTE]
     > If a domain user is not populated in Select User, wait a few moments for Ranger to sync with AAD.
     > 
     > 
4. Click **Add** to save the policy.
5. Repeat the last two steps to create another policy with the following properties:
   
   * Policy name: read-hivesampletable-devicemake
   * Hive Database: default
   * table: hivesampletable
   * Hive column: clientid, devicemake
   * Select User: hiveuser2
   * Permissions: select

## Create Hive ODBC data source
The instructions can be found in [Create Hive ODBC data source](/documentation/articles/hdinsight-connect-excel-hive-odbc-driver/).  

    Property|Description
    ---|---
    Data Source Name|Give a name to your data source
    Host|Enter &lt;HDInsightClusterName>.azurehdinsight.cn. For example, myHDICluster.azurehdinsight.cn
    Port|Use <strong>443</strong>. (This port has been changed from 563 to 443.)
    Database|Use <strong>Default</strong>.
    Hive Server Type|Select <strong>Hive Server 2</strong>
    Mechanism|Select <strong>Azure HDInsight Service</strong>
    HTTP Path|Leave it blank.
    User Name|Enter hiveuser1@contoso158.partner.onmschina.cn. Update the domain name if it is different.
    Password|Enter the password for hiveuser1.
    </table>

Make sure to click **Test** before saving the data source.

## Import data into Excel from HDInsight
In the last section, you have configured two policies.  hiveuser1 has the select permission on all the columns, and hiveuser2 has the select permission on two columns. In this section, you impersonate the two users to import data into Excel.

1. Open a new or existing workbook in Excel.
2. From the **Data** tab, click **From Other Data Sources**, and then click **From Data Connection Wizard** to launch the **Data Connection Wizard**.
   
    ![Open data connection wizard][img-hdi-simbahiveodbc.excel.dataconnection]
3. Select **ODBC DSN** as the data source, and then click **Next**.
4. From ODBC data sources, select the data source name that you created in the previous step, and then  click **Next**.
5. Re-enter the password for the cluster in the wizard, and then click **OK**. Wait for the **Select Database and Table** dialog to open. This can take a few seconds.
6. Select **hivesampletable**, and then click **Next**. 
7. Click **Finish**.
8. In the **Import Data** dialog, you can change or specify the query. To do so, click **Properties**. This can take a few seconds. 
9. Click the **Definition** tab. The command text is:
   
       SELECT * FROM "HIVE"."default"."hivesampletable"
   
   By the Ranger policies you defined,  hiveuser1 has select permission on all the columns.  So this query works with hiveuser1's credentials, but this query does not not work with hiveuser2's credentials.
   
   ![Connection Properties][img-hdi-simbahiveodbc-excel-connectionproperties]
10. Click **OK** to close the Connection Properties dialog.
11. Click **OK** to close the **Import Data** dialog.  
12. Reenter the password for hiveuser1, and then click **OK**. It takes a few seconds before data gets imported to Excel. When it is done, you shall see 11 columns of data.

To test the second policy (read-hivesampletable-devicemake) you created in the last section

1. Add a new sheet in Excel.
2. Follow the last procedure to import the data.  The only change you will make is to use hiveuser2's credentials instead of hiveuser1's. This will fail because hiveuser2 only has permission to see two columns. You shall get the following error:
   
        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. Follow the same procedure to import data. This time, use hiveuser2's credentials, and also modify the select statement from:
   
        SELECT * FROM "HIVE"."default"."hivesampletable"
   
    to:
   
        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"
   
    When it is done, you shall see two columns of data imported.

## Next steps
* For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](/documentation/articles/hdinsight-domain-joined-configure/).
* For managing a Domain-joined HDInsight clusters, see [Manage Domain-joined HDInsight clusters](/documentation/articles/hdinsight-domain-joined-manage/).
* For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with Linux-based Hadoop on HDInsight from Linux, Unix, or OS X](/documentation/articles/hdinsight-hadoop-linux-use-ssh-unix/#connect-to-a-domain-joined-hdinsight-cluster).
* For Connecting Hive using Hive JDBC, see [Connect to Hive on Azure HDInsight using the Hive JDBC driver](/documentation/articles/hdinsight-connect-hive-jdbc-driver/)
* For connecting Excel to Hadoop using Hive ODBC, see [Connect Excel to Hadoop with the Microsoft Hive ODBC drive](/documentation/articles/hdinsight-connect-excel-hive-odbc-driver/)
* For connecting Excel to Hadoop using Power Query, see [Connect Excel to Hadoop by using Power Query](/documentation/articles/hdinsight-connect-excel-power-query/)


<!-- not suitable for Mooncake -->

<properties
	pageTitle="Create a Spark cluster on Azure HDInsight and use Spark SQL from Zeppelin and Jupyter for interactive analysis | Azure"
	description="Step-by-step instructions on how to quickly create an Apache Spark cluster in HDInsight and then use Spark SQL from Zeppelin and Jupyter notebooks to run interactive queries."
	services="hdinsight"
	documentationCenter=""
	authors="nitinme"
	manager="paulettm"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.date="03/07/2016"
	wacn.date=""/>


# Get started: Create Apache Spark on HDInsight Windows and run interactive queries using Spark SQL (Preview)

> [AZURE.NOTE] HDInsight now provides Spark clusters on Linux. For information on how to get started with Spark clusters on HDInsight Linux, see [Get started: create Apache Spark on Azure HDInsight (Linux)](/documentation/articles/hdinsight-apache-spark-jupyter-spark-sql).

Learn how to create an Apache Spark cluster in HDInsight using the Quick Create option and then use the web-based [Zeppelin](https://zeppelin.incubator.apache.org) and [Jupyter](https://jupyter.org) notebooks to run Spark SQL interactive queries on the Spark cluster.


   ![Get started using Apache Spark in HDInsight](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.getstartedflow.png  "Get started using Apache Spark in HDInsight tutorial. Steps illustrated: create a storage account; create a cluster; run Spark SQL statements")

[AZURE.INCLUDE [delete-cluster-warning](../includes/hdinsight-delete-cluster-warning.md)]

**Prerequisites:**

Before you begin this tutorial, you must have an Azure subscription. See [Get Azure trial](/pricing/1rmb-trial/).


## Create an HDInsight Spark cluster

In this section, you create an HDInsight version 3.2 cluster, which is based on Spark version 1.3.1. For information about HDInsight versions and their SLAs, see [HDInsight component versioning](/documentation/articles/hdinsight-component-versioning-v1).

>[AZURE.NOTE] The steps in this article create an Apache Spark cluster in HDInsight by using basic configuration settings. For information about other cluster configuration settings (such as using additional storage, an Azure virtual network, or a metastore for Hive), see [Create HDInsight clusters using custom options](/documentation/articles/hdinsight-apache-spark-provision-clusters).


**To create a Spark cluster**

1. Sign in to the [Azure Portal](https://portal.azure.cn/).

2. Click **NEW**, click **Data + Analytics**, and then click **HDInsight**.

    ![Creating a new cluster in the Azure Portal](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.createcluster.1.png "Creating a new cluster in the Azure Portal")

3. Enter a **Cluster Name**, select **Spark** for the **Cluster Type**, and from the **Cluster Operating System** drop-down menu, select **Windows Server 2012 R2 Datacenter**. A green check appears beside the cluster name if it is available.

	![Enter cluster name and type](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.createcluster.2.png "Enter cluster name and type")

4. If you have more than one subscription, click the **Subscription** entry to select the Azure subscription to use for the cluster.

5. Click **Resource Group** to see a list of existing resource groups and select where to create the cluster. Or, you can click **Create New** and then enter the name of the new resource group. A green check appears to indicate if the new group name is available.

	> [AZURE.NOTE] This entry defaults to one of your existing resource groups, if any are available.

6. Click **Credentials**, and then enter a **Cluster Login Username** and **Cluster Login Password**. If you want to enable Remote Desktop on the cluster node, for **Enable Remote Desktop**, click **Yes**, and then specify the required values. This tutorial does not require remote desktop so you can skip this. Click **Select** at the bottom to save the credentials configuration.

	![Provide cluster credentials](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.createcluster.3.png "Provide cluster credentials")

7. Click **Data Source** to choose an existing data source for the cluster, or create a new one. When you create a Hadoop cluster in HDInsight, you specify an Azure Storage account. A specific Blob storage container from that account is designated as the default file system, like in the Hadoop distributed file system (HDFS). By default, the HDInsight cluster is created in the same data center as the storage account you specify. For more information, see [Use Azure Blob storage with HDInsight][hdinsight-storage]

	![Data source blade](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.createcluster.4.png "Provide data source configuration")

	Currently you can select an Azure Storage Account as the data source for an HDInsight cluster. Use the following to understand the entries on the **Data Source** blade.

	- **Selection Method**: Set this to **From all subscriptions** to enable browsing of storage accounts from all your subscriptions. Set this to **Access Key** if you want to enter the **Storage Name** and **Access Key** of an existing storage account.

	- **Select storage account / Create New**: Click **Select storage account** to browse and select an existing storage account you want to associate with the cluster. Or, click **Create New** to create a new storage account. Use the field that appears to enter the name of the storage account. A green check appears if the name is available.

	- **Choose Default Container**: Use this to enter the name of the default container to use for the cluster. While you can enter any name here, we recommend using the same name as the cluster so that you can easily recognize that the container is used for this specific cluster.

	- **Location**: The geographic region that the storage account is in, or will be created in.

		> [AZURE.IMPORTANT] Selecting the location for the default data source also sets the location of the HDInsight cluster. The cluster and default data source must be located in the same region.

	Click **Select** to save the data source configuration.

8. Click **Node Pricing Tiers** to display information about the nodes that will be created for this cluster. Set the number of worker nodes that you need for the cluster. The estimated cost of the cluster will be shown within the blade.

	![Node pricing tiers blade](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.createcluster.5.png "Specify number of cluster nodes")

	Click **Select** to save the node pricing configuration.

9. On the **New HDInsight Cluster** blade, ensure that **Pin to Startboard** is selected, and then click **Create**. This creates the cluster and adds a tile for it to the Startboard of your Azure portal. The icon will indicate that the cluster is creating, and will change to display the HDInsight icon once creation has completed.

	| While creating | Creation complete |
	| ------------------ | --------------------- |
	| ![Creating indicator on startboard](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/provisioning.png) | ![Created cluster tile](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/provisioned.png) |

	> [AZURE.NOTE] It will take some time for the cluster to be created, usually around 15 minutes. Use the tile on the Startboard, or the **Notifications** entry on the left of the page to check on the creation process.

10. Once the creation is complete, click the tile for the Spark cluster from the Startboard to launch the cluster blade.


## <a name="zeppelin"></a>Run interactive Spark SQL queries using a Zeppelin notebook

After you have created a cluster, you can use a web-based Zeppelin notebook to run Spark SQL interactive queries against the Spark HDInsight cluster. In this section, we will use a sample data file (hvac.csv) available by default on the cluster to run some interactive Spark SQL queries.

>[AZURE.NOTE] The notebook you create by following the instructions below is also available by default on the cluster. After you have launched Zeppelin, you will find this notebook by the name **Zeppelin HVAC tutorial**.

1. From the [Azure Portal](https://portal.azure.cn/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard). You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.   

2. From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Zeppelin Notebook**. If prompted, enter the admin credentials for the cluster.

	> [AZURE.NOTE] You may also reach the Zeppelin Notebook for your cluster by opening the following URL in your browser. Replace __CLUSTERNAME__ with the name of your cluster:
	>
	> `https://CLUSTERNAME.azurehdinsight.cn/zeppelin`

2. Create a new notebook. From the header pane, click **Notebook**, and then click **Create New Note**.

	![Create a new Zeppelin notebook](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.createnewnote.png "Create a new Zeppelin notebook")

	On the same page, under the **Notebook** heading, you should see a new notebook with the name starting with **Note XXXXXXXXX**. Click the new notebook.

3. On the webpage for the new notebook, click the heading, and change the name of the notebook if you want to. Press ENTER to save the name change. Also, make sure the notebook header shows a **Connected** status in the top-right corner.

	![Zeppelin notebook status](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.newnote.connected.png "Zeppelin notebook status")

4. Load sample data into a temporary table. When you create a Spark cluster in HDInsight, the sample data file, **hvac.csv**, is copied to the associated storage account under **\HdiSamples\SensorSampleData\hvac**.

	In the empty paragraph that is created by default in the new notebook, paste the following code:

		// Create an RDD using the default Spark context, sc
		val hvacText = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")

		// Define a schema
		case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)

		// Map the values in the .csv file to the schema
		val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
    		s => Hvac(s(0),
            		s(1),
            		s(2).toInt,
            		s(3).toInt,
            		s(6)
        	)
		).toDF()

		// Register as a temporary table called "hvac"
		hvac.registerTempTable("hvac")

	Press **SHIFT + ENTER** on the keyboard or click the **Play** button for the paragraph to run the code. The status in the upper right corner of the paragraph should progress from READY, PENDING, RUNNING to FINISHED. The output appears at the bottom of the same paragraph. The screenshot looks like the following:

	![Create a temporary table from raw data](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.note.loaddataintotable.png "Create a temporary table from raw data")

	You can also provide a title to each paragraph. From the right-hand corner, click the **Settings** icon, and then click **Show title**.

5. You can now run Spark SQL statements on the **hvac** table. Paste the following query in a new paragraph. The query retrieves the building ID and the difference between the target and actual temperatures for each building on a given date. Press **SHIFT + ENTER**.

		%sql
		select buildingID, (targettemp - actualtemp) as temp_diff, date
		from hvac
		where date = "6/1/13"

	The **%sql** statement at the beginning tells the notebook to use the Spark  SQL interpreter. You can look at the defined interpreters from the **Interpreter** tab in the notebook header.

	The following screenshot shows the output.

	![Run a Spark SQL statement using the notebook](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.note.sparksqlquery1.png "Run a Spark SQL statement using the notebook")

	Click the display options (highlighted in rectangle) to switch between different representations for the same output. Click **Settings** to choose what consitutes the key and values in the output. The screen capture above uses **buildingID** as the key and the average of **temp_diff** as the value.

6. You can also run Spark SQL statements using variables in the query. The next code example shows how to define a variable, **Temp**, in the query with the possible values you want to query with. When you first run the query, a drop-down is automatically populated with the values you specified for the variable.

		%sql
		select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff
		from hvac
		where targettemp > "${Temp = 65,65|75|85}"

	Paste this code example in a new paragraph and press **SHIFT + ENTER**. The following screenshot shows the output.

	![Run a Spark SQL statement using the notebook](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.note.sparksqlquery2.png "Run a Spark SQL statement using the notebook")

	For subsequent queries, you can select a new value from the drop-down and run the query again. Click **Settings** to choose what consitutes the key and values in the output. The screen capture above uses **buildingID** as the key, the average of **temp_diff** as the value, and **targettemp** as the group.

7. Restart the Spark SQL interpreter to exit the application. Click the **Interpreter** tab at the top, and for the Spark interpreter, click **Restart**.

	![Restart the Zeppelin intepreter](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.zeppelin.restart.interpreter.png "Restart the Zeppelin intepreter")

## <a name="jupyter"></a>Run Spark SQL queries using a Jupyter notebook

In this section, you use a Jupyter notebook to run Spark SQL queries against a Spark cluster.

>[AZURE.NOTE] The notebook you create by following the instructions below is also available by default on the cluster. After you have launched Jupyter, you will find this notebook by the name **HVACTutorial.ipynb**.

1. From the [Azure Portal](https://portal.azure.cn/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard). You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.   

2. From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Jupyter Notebook**. If prompted, enter the admin credentials for the cluster.

	> [AZURE.NOTE] You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser. Replace __CLUSTERNAME__ with the name of your cluster:
	>
	> `https://CLUSTERNAME.azurehdinsight.cn/jupyter`

2. Create a new notebook. Click **New**, and then click **Python2**.

	![Create a new Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Create a new Jupyter notebook")

3. A new notebook is created and opened with the name Untitled.pynb. Click the notebook name at the top, and enter a friendly name.

	![Provide a name for the notebook](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Provide a name for the notebook")

4. Import the required modules and create the Spark and SQL contexts. Paste the following code example in an empty cell, and then press **SHIFT + ENTER**.

		from pyspark import SparkContext
		from pyspark.sql import SQLContext
		from pyspark.sql.types import *

		# Create Spark and SQL contexts
		sc = SparkContext('spark://headnodehost:7077', 'pyspark')
		sqlContext = SQLContext(sc)

	Every time you run a job in Jupyter, your web browser window title will show a **(Busy)** status along with the notebook title. You will also see a solid circle next to the **Python 2** text in the top-right corner. After the job is completed, this will change to a hollow circle.

	 ![Status of a Jupyter notebook job](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status of a Jupyter notebook job")

4. Load sample data into a temporary table. When you create a Spark cluster in HDInsight, the sample data file, **hvac.csv**, is copied to the associated storage account under **\HdiSamples\SensorSampleData\hvac**.

	In an empty cell, paste the following code example and press **SHIFT + ENTER**. This code example registers the data into a temporary table called **hvac**.

		# Load the data
		hvacText = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")

		# Create the schema
		hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

		# Parse the data in hvacText
		hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

		# Create a data frame
		hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

		# Register the data fram as a table to run queries against
		hvacdf.registerAsTable("hvac")

		# Run queries against the table and display the data
		data = sqlContext.sql("select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = \"6/1/13\"")
		data.show()

5. Once the job is completed successfully, the following output is displayed.

		buildingID temp_diff date  
		4          8         6/1/13
		3          2         6/1/13
		7          -10       6/1/13
		12         3         6/1/13
		7          9         6/1/13
		7          5         6/1/13
		3          11        6/1/13
		8          -7        6/1/13
		17         14        6/1/13
		16         -3        6/1/13
		8          -8        6/1/13
		1          -1        6/1/13
		12         11        6/1/13
		3          14        6/1/13
		6          -4        6/1/13
		1          4         6/1/13
		19         4         6/1/13
		19         12        6/1/13
		9          -9        6/1/13
		15         -10       6/1/13

6. Restart the kernel to exit the application. From the top menu bar, click **Kernel**, click **Restart**, and then click **Restart** again at the prompt.

	![Restart the Jupyter Kernel](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/hdispark.jupyter.restart.kernel.png "Restart the Jupyter Kernel")

##Delete the cluster

[AZURE.INCLUDE [delete-cluster-warning](../includes/hdinsight-delete-cluster-warning.md)]


## <a name="seealso"></a>See also


* [Overview: Apache Spark on Azure HDInsight](/documentation/articles/hdinsight-apache-spark-overview)
* [Create a Spark on HDInsight cluster](/documentation/articles/hdinsight-apache-spark-provision-clusters)
* [Perform interactive data analysis using Spark in HDInsight with BI tools](/documentation/articles/hdinsight-apache-spark-use-bi-tools)
* [Use Spark in HDInsight for building machine learning applications](/documentation/articles/hdinsight-apache-spark-ipython-notebook-machine-learning)
* [Use Spark in HDInsight for building real-time streaming applications](/documentation/articles/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming)
* [Manage resources for the Apache Spark cluster in Azure HDInsight](/documentation/articles/hdinsight-apache-spark-resource-manager)


[hdinsight-versions]: /documentation/articles/hdinsight-component-versioning-v1
[hdinsight-upload-data]: /documentation/articles/hdinsight-upload-data
[hdinsight-storage]: /documentation/articles/hdinsight-hadoop-use-blob-storage

[azure-purchase-options]: /pricing/overview/
[azure-member-offers]: /pricing/member-offers/
[azure-trial]: /pricing/1rmb-trial/
[azure-management-portal]: https://manage.windowsazure.cn/
[azure-create-storageaccount]: /documentation/articles/storage-create-storage-account

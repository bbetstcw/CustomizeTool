<properties
    pageTitle="Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight | Azure"
    description="Step-by-step instructions on how to configure Jupyter notebooks available with HDInsight Spark clusters to use external Spark packages."
    services="hdinsight"
    documentationcenter=""
    author="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal" />
<tags
    ms.assetid="2a8bc545-064e-436f-8b5f-e67c26cfbf98"
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/31/2016"
    wacn.date=""
    ms.author="nitinme" />

# Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight Linux
Learn how to configure a Jupyter notebook in Apache Spark cluster on HDInsight (Linux) to use external, community-contributed packages that are not included out-of-the-box in the cluster. 

You can search the [Maven repository](http://search.maven.org/) for the complete list of packages that are available. You can also get a list of available packages from other sources. For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).

In this article, you will learn how to use the [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with the Jupyter notebook.

## Prerequisites
You must have the following:

* An Azure subscription. See [Get Azure trial](/pricing/1rmb-trial/).
* An Apache Spark cluster on HDInsight Linux. For instructions, see [Create Apache Spark clusters in Azure HDInsight](/documentation/articles/hdinsight-apache-spark-jupyter-spark-sql/).

## Use external packages with Jupyter notebooks
1. From the [Azure Portal](https://portal.azure.cn/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard). You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.   
2. From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Jupyter Notebook**. If prompted, enter the admin credentials for the cluster.
   
   > [AZURE.NOTE]
   > You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser. Replace **CLUSTERNAME** with the name of your cluster:
   > 
   > `https://CLUSTERNAME.azurehdinsight.cn/jupyter`
   > 
   > 
3. Create a new notebook. Click **New**, and then click **Spark**.
   
    ![Create a new Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Create a new Jupyter notebook")
4. A new notebook is created and opened with the name Untitled.pynb. Click the notebook name at the top, and enter a friendly name.
   
    ![Provide a name for the notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Provide a name for the notebook")
5. You will use the `%%configure` magic to configure the notebook to use an external package. In notebooks that use external packages, make sure you call the `%%configure` magic in the first code cell. This ensures that the kernel is configured to use the package before the session starts.

    **For HDInsight 3.3 and HDInsight 3.4** 

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    **For HDInsight 3.5**

        %%configure
        { "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}


    >[AZURE.IMPORTANT] If you forget to configure the kernel in the first cell, you can use the `%%configure` with the `-f` parameter, but that will restart the session and all progress will be lost.

1. The snippet above expects the maven coordinates for the external package in Maven Central Repository. In this snippet, `com.databricks:spark-csv_2.10:1.4.0` is the maven coordinate for **spark-csv** package. Here's how you construct the coordinates for a package.
   
    a. Locate the package in the Maven Repository. For this tutorial, we use [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. From the repository, gather the values for **GroupId**, **ArtifactId**, and **Version**.
   
    ![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")
   
    c. Concatenate the three values, separated by a colon (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0
2. Run the code cell with the `%%configure` magic. This will configure the underlying Livy session to use the package you provided. In the subsequent cells in the notebook, you can now use the package, as shown below.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
3. You can then run the snippets, like shown below, to view the data from the dataframe you created in the previous step.
   
        df.show()
   
        df.select("Time").count()

## <a name="seealso"></a>See also
* [Overview: Apache Spark on Azure HDInsight](/documentation/articles/hdinsight-apache-spark-overview/)

### Scenarios
* [Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools](/documentation/articles/hdinsight-apache-spark-use-bi-tools/)
* [Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data](/documentation/articles/hdinsight-apache-spark-ipython-notebook-machine-learning/)
* [Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results](/documentation/articles/hdinsight-apache-spark-machine-learning-mllib-ipython/)
* [Spark Streaming: Use Spark in HDInsight for building real-time streaming applications](/documentation/articles/hdinsight-apache-spark-eventhub-streaming/)
* [Website log analysis using Spark in HDInsight](/documentation/articles/hdinsight-apache-spark-custom-library-website-log-analysis/)

### Create and run applications
* [Create a standalone application using Scala](/documentation/articles/hdinsight-apache-spark-create-standalone-application/)
* [Run jobs remotely on a Spark cluster using Livy](/documentation/articles/hdinsight-apache-spark-livy-rest-interface/)

### Tools and extensions
* [Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons](/documentation/articles/hdinsight-apache-spark-intellij-tool-plugin/)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely](/documentation/articles/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely/)
* [Use Zeppelin notebooks with a Spark cluster on HDInsight](/documentation/articles/hdinsight-apache-spark-use-zeppelin-notebook/)
* [Kernels available for Jupyter notebook in Spark cluster for HDInsight](/documentation/articles/hdinsight-apache-spark-jupyter-notebook-kernels/)
* [Install Jupyter on your computer and connect to an HDInsight Spark cluster](/documentation/articles/hdinsight-apache-spark-jupyter-notebook-install-locally/)

### Manage resources
* [Manage resources for the Apache Spark cluster in Azure HDInsight](/documentation/articles/hdinsight-apache-spark-resource-manager/)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight](/documentation/articles/hdinsight-apache-spark-job-debugging/)


<properties
    pageTitle="Run Hive queries using HDInsight .NET SDK | Azure"
    description="Learn how to submit Hadoop jobs to Azure HDInsight Hadoop using HDInsight .NET SDK."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationcenter=""
    tags="azure-portal"
    author="mumian" />
<tags
    ms.assetid="4e291890-f8b4-426c-b5e8-d4fd512ff042"
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    wacn.date=""
    ms.author="jgao" />

# Run Hive queries using HDInsight .NET SDK
[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Learn how to submit Hive queries using HDInsight .NET SDK.

> [AZURE.NOTE]
> The steps in this article must be performed from a Windows client. For information on using a Linux, OS X, or Unix client to work with Hive, use the tab selector shown on the top of the article.
> 
> 

## Prerequisites
Before you begin this article, you must have the following:

* **A Hadoop cluster in HDInsight**. See [Get started using Linux-based Hadoop in HDInsight](/documentation/articles/hdinsight-use-sqoop/#create-cluster-and-sql-database).
* **Visual Studio 2012/2013/2015**.

## Submit Hive queries using HDInsight .NET SDK
The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET. 

**To Submit jobs**

1. Create a C# console application in Visual Studio.
2. From the Nuget Package Manager Console, run the following command.
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Use the following code:

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.cn";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
   
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. Press **F5** to run the application.

## Next steps
In this article, you have learned several ways to create an HDInsight cluster. To learn more, see the following articles:

* [Get started with Azure HDInsight][hdinsight-get-started]
* [Create Hadoop clusters in HDInsight][hdinsight-provision]
* [Manage Hadoop clusters in HDInsight by using the Azure Portal](/documentation/articles/hdinsight-administer-use-management-portal-v1/)
* [HDInsight .NET SDK reference](https://msdn.microsoft.com/zh-cn/library/mt271028.aspx)
* [Use Pig with HDInsight](/documentation/articles/hdinsight-use-pig/)
* [Use Sqoop with HDInsight](/documentation/articles/hdinsight-use-sqoop/)
* [Create non-interactive authentication .NET HDInsight applications](/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/)

[hdinsight-provision]: /documentation/articles/hdinsight-provision-clusters-v1/
[hdinsight-get-started]: /documentation/articles/hdinsight-hadoop-tutorial-get-started-windows-v1/



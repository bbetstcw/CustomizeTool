<properties 
	pageTitle="Utilize operation and service logs in Stream Analytics | Windows Azure" 
	description="How-to use Stream Analytics operation logs" 
	keywords="service logs"
	services="stream-analytics" 
	documentationCenter="" 
	authors="jeffstokes72" 
	manager="paulettm" 
	editor="cgronlun"/>

<tags
	ms.service="stream-analytics"
	ms.date="12/04/2015"
	wacn.date=""/>

# Understand Stream Analytics service and operation logs

All Azure services supply operational logging messages to users to record details related to management operations. In Azure Stream Analytics, this information can be used for debugging purposes such as viewing job status, job progress, and failure messages to track the progress of a job over time, from start to processing to output.

## Find operation logs in the Azure Management Portal

Operation Logs can be accessed in two ways:  

- Dashboard of the Stream Analytics job  
- Management Services in the Azure Management Portal  

## Dashboard of the Stream Analytics job

A link to the corresponding logs of a Stream Analytics job is displayed on the jobâs Dashboard tab. If you click on that link, it will set the filters in a way that it shows latest logs for that specific job.

  ![Select Management Services logs](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## Management Services

To manually navigate to the Operation Logs for Stream Analytics and other services in the Azure Management Portal:

1.	Click on **Management Services** in the [Azure Management Portal](https://manage.windowsazure.cn).
2.	Select **Stream Analytics** for **Type** and the name of the job for **Service Name**.  

  ![Select Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## Get log details

You can filter by Time Range and Status to view the logs for your job.

In the Azure Management portal, click on the **Details** button at the bottom of the window to view more details about a selected event. 

  ![Select Details](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

## Debug a failed job

In the Azure Management portal, click on the Search icon and type âfailedâ. This gives a result of all logs with failures. 

  ![Debugging a failed job](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

You can select any one of the failures, and click on the **Details** for more information on the error.  Some error messages also provide information about how the mitigate the issue. 

  ![Operation Details](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

In case you need to contact [Support](/support/contact/) or provide information to the team via the [MSDN forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics), please note the Operation Details, specifically the **Correlation ID**. 

## Get help
For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics)

## Next steps

- [Introduction to Azure Stream Analytics](/documentation/articles/stream-analytics-introduction)
- [Get started using Azure Stream Analytics](/documentation/articles/stream-analytics-get-started)
- [Scale Azure Stream Analytics jobs](/documentation/articles/stream-analytics-scale-jobs)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/zh-cn/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API Reference](https://msdn.microsoft.com/zh-cn/library/azure/dn835031.aspx)

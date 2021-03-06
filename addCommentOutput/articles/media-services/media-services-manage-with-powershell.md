<properties 
	pageTitle="Manage Azure Media Services Accounts with PowerShell" 
	description="Learn how to manage Azure Media Services accounts with PowerShell cmdlets." 
	authors="Juliako" 
	manager="dwrede" 
	editor="" 
	services="media-services" 
	documentationCenter=""/>

<tags
	ms.service="media-services"
	ms.date="12/08/2015"
	wacn.date=""/>


#Manage Azure Media Services Accounts with PowerShell

> [AZURE.SELECTOR]
- [Portal](/documentation/articles/media-services-create-account)
- [PowerShell](/documentation/articles/media-services-manage-with-powershell)
- [REST](http://msdn.microsoft.com/zh-cn/library/azure/dn194267.aspx)

> [AZURE.NOTE] To be able to create an Azure Media Services account, you must have an Azure account. If you don't have an account, you can create a trial account in just a couple of minutes. For details, see <a href="/pricing/1rmb-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Trial</a>.

##Overview 

This articles shows you how to use PowerShell cmdlets to manage Azure Media Services accounts.

>[AZURE.NOTE]
> To complete this tutorial, you need an Azure account. If you don't have an account, you can create a trial account in just a couple of minutes. For details, see <a href="/pricing/1rmb-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Trial</a>.

##Install Windows Azure PowerShell Cmdlets

To install the latest Azure PowerShell cmdlets, see [How to install and configure Azure PowerShell](/documentation/articles/powershell-install-configure)

##Select Azure Subscription

Once you install and configure PowerShell cmdlets, you should specify in which subscription you want to work. 

To get a list of available subscriptions run the following cmdlet:

	PS C:\> Get-AzureSubscription

Then, select one by doing:

	PS C:\> Select-AzureSubscription "TestSubscription"

 
##Get Storage Account Name

Azure Media Services uses Azure Storage to store media content. When you create a new Media Services account, you have to associate it with a Storage account. Storage account must belong to the same subscription as the one you  plan to use for your Media Services account. 

In this example, an existing storage account is used. The [Get-AzureStorageAccount](https://msdn.microsoft.com/zh-cn/library/azure/dn495134.aspx) cmdlet gets storage accounts in the current subscription. Get the name (StorageAccountName) of the storage account you want to associate your Media Account with.

	StorageAccountDescription : 
	AffinityGroup             :
	Location                  : China East
	GeoReplicationEnabled     : True
	GeoPrimaryLocation        : China East
	GeoSecondaryLocation      : China North
	Label                     : storagetest001
	StorageAccountStatus      : Created
	StatusOfPrimary           : Available
	StatusOfSecondary         : Available
	Endpoints                 : {https://storagetest001.blob.core.chinacloudapi.cn/,
	                            https://storagetest001.queue.core.chinacloudapi.cn/,
	                            https://storagetest001.table.core.chinacloudapi.cn/}
	AccountType               : Standard_GRS
	StorageAccountName        : storatetest001
	OperationDescription      : Get-AzureStorageAccount
	OperationId               : e919dd56-7691-96db-8b3c-2ceee891ae5d
	OperationStatus           : Succeeded

##Create New Media Services Account

To create a new Azure Media Services account use the [New-AzureMediaServicesAccount](https://msdn.microsoft.com/zh-cn/library/azure/dn495286.aspx) cmdlet providing Media Services account name, data center location where it will be created, and Storage account name. 


	PS C:\> New-AzureMediaServicesAccount -Name "amstestaccount001" -StorageAccountName "storagetest001" -Location "China East"

##Get Media Services Accounts

Once you created one or more Media Services accounts you can list information by using [Get-AzureMediaServicesAccount](https://msdn.microsoft.com/zh-cn/library/azure/dn495286.aspx)

	
	PS C:\> Get-AzureMediaServicesAccount
	
	AccountId		Name				State
	---------       ----       			 -----
	xxxxxxxxxx      amstestaccount001   Active

By providing Name parameter you will get more detailed information including account keys.

	PS C:\> Get-AzureMediaServicesAccount -Name amstestaccount001

##Re-generate Media Services Access Keys

If you want to update Media Services primary or secondary access key, use [New-AzureMediaServicesKey](https://msdn.microsoft.com/zh-cn/library/azure/dn495215.aspx). 
You need to provide account name and specify which key you want to re-generate (primary or secondary). 

Specify a -Force switch if you do not want for the PowerShell to ask confirmation questions.

	PS C:\> New-AzureMediaServicesKey -Name "amstestaccount001" -KeyType "Primary" -Force

##Remove Media Services Account

When you are ready to delete the Azure Media account, use [Remove-AzureMediaServicesAccount](https://msdn.microsoft.com/zh-cn/library/azure/dn495220.aspx).

	PS C:\> Remove-AzureMediaServicesAccount -Name "amstestaccount001" -Force


<!-- deleted by customization
##Media Services learning paths

[AZURE.INCLUDE [media-services-learning-paths-include](../includes/media-services-learning-paths-include.md)]

##Provide feedback

[AZURE.INCLUDE [media-services-user-voice-include](../includes/media-services-user-voice-include.md)]

-->
 
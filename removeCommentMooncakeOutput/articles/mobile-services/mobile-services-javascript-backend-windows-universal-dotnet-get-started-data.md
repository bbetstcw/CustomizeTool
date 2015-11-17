<properties 
	pageTitle="Add Mobile Services to an existing universal Windows app | Windows Azure" 
	description="Learn how to connect your existing universal Windows app to Azure Mobile Services." 
	services="mobile-services" 
	documentationCenter="windows" 
	authors="ggailey777" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.date="07/22/2015" 
	wacn.date=""/>

# Add Mobile Services to an existing app

[AZURE.INCLUDE [mobile-services-selector-get-started-data](../includes/mobile-services-selector-get-started-data.md)]

##Overview

This topic shows you how to use Azure Mobile Services to leverage data in a universal Windows app. Universal Windows app solutions include projects for both Windows Store 8.1 and Windows Phone Store 8.1 apps and a common shared project. For more information, see [Build universal Windows apps that target Windows and Windows Phone](http://msdn.microsoft.com/zh-cn/library/windows/apps/xaml/dn609832.aspx).

In this tutorial, you will download a Visual Studio 2013 project for a universal Windows app that stores data in memory, create a new mobile service, integrate the mobile service with the app, and then sign-in to the Azure Management Portal to view changes to data made when running the app.

>[AZURE.NOTE]This topic shows you how to use the tooling in Visual Studio Professional 2013 with Update 3 to connect a new mobile service to a universal Windows app. The same steps can be used to connect a mobile service to a Windows Store or Windows Phone Store 8.1 app. To connect a mobile service to an Windows Phone 8.0 or Windows Phone Silverlight 8.1 app, see [Get started with data for Windows Phone](/documentation/articles/mobile-services-windows-phone-get-started-data).

To complete this tutorial, you need the following:

* An active Azure account. If you don't have an account, you can create a trial account in just a couple of minutes. For details, see [Azure  Trial](/pricing/1rmb-trial).
* [Visual Studio Express 2013 for Windows](https://www.visualstudio.com/downloads/download-visual-studio-vs) (Update 2 or a later version). 

##<a name="download-app"></a>Download the GetStartedWithData project

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-download-project](../includes/mobile-services-windows-universal-dotnet-download-project.md)]
 

##<a name="create-service"></a>Create a new mobile service from Visual Studio

[AZURE.INCLUDE [mobile-services-create-new-service-vs2013](../includes/mobile-services-create-new-service-vs2013.md)]

&nbsp;&nbsp;7. In Solution Explorer, open the App.xaml.cs code file in the GetStartedWithData.Shared project folder, and notice the new static field that was added to the **App** class inside a Windows Store app conditional compilation block, which looks like the following example: 

	public static Microsoft.WindowsAzure.MobileServices.MobileServiceClient 
	    todolistClient = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
	        "https://todolist.azure-mobile.cn/",
	        "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX");

&nbsp;&nbsp;This code provides access to your new mobile service in your app by using an instance of the [MobileServiceClient] class. The client is created by supplying the URI and the application key of the new mobile service. This static field is available to all pages in your app.

&nbsp;&nbsp;8. Right-click the Windows Phone app project, click **Add**, click **Connected Service...**, select the mobile service that you just created, and then click **OK**. The same code is added to the shared App.xaml.cs file, but this time within a Windows Phone app conditional compilation block.

At this point, both the Windows Store and Windows Phone Store apps are connected to the new mobile service. The next step is to create a new TodoItem table in the mobile service.

##<a name="add-table"></a>Add a new table to the mobile service

[AZURE.INCLUDE [mobile-services-create-new-table-vs2013](../includes/mobile-services-create-new-table-vs2013.md)]

##<a name="update-app"></a>Update the app to use the mobile service

[AZURE.INCLUDE [mobile-services-windows-dotnet-update-data-app](../includes/mobile-services-windows-dotnet-update-data-app.md)]

##<a name="test-azure-hosted"></a>Test the mobile service hosted in Azure

Now we can test both versions of the universal Windows app against the mobile service hosted in Azure.

[AZURE.INCLUDE [mobile-services-windows-universal-test-app](../includes/mobile-services-windows-universal-test-app.md)]

&nbsp;&nbsp;4. In the [Azure Management Portal], click **Mobile Services**, and then click your mobile service.

&nbsp;&nbsp;5. Click the **Data** > **Browse** and notice that the **TodoItem** table now contains data, with id values generated by Mobile Services, and that columns have been automatically added to the table to match the TodoItem class in the app.
     
This concludes the tutorial.

## <a name="next-steps"> </a>Next steps

This tutorial demonstrated the basics of enabling a universal Windows app to work with data in Mobile Services. Next, consider reading up on one of these other topics:

* [Get started with authentication]
  <br/>Learn how to authenticate users of your app.

* [Get started with push notifications] 
  <br/>Learn how to send a very basic push notification to your app.

* [Mobile Services C# How-to Conceptual Reference](/documentation/articles/mobile-services-windows-dotnet-how-to-use-client-library)
  <br/>Learn more about how to use Mobile Services with .NET.
  
<!-- Anchors. -->

[Get the Windows Store app]: #download-app
[Create the mobile service from Visual Studio]: #create-service
[Add a data table for storage]: #add-table
[Update the app to use the mobile service]: #update-app
[Test the app against Mobile Services]: #test-app
[View uploaded data in the Azure Management Portal]: #view-data
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Validate and modify data with scripts]: /documentation/articles/mobile-services-windows-store-dotnet-validate-modify-data-server-scripts
[Refine queries with paging]: /documentation/articles/mobile-services-windows-store-dotnet-add-paging-data
[Get started with Mobile Services]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started
[Get started with data]: /documentation/articles/mobile-services-windows-store-dotnet-get-started-data
[Get started with authentication]: /documentation/articles/mobile-services-windows-store-dotnet-get-started-users
[Get started with push notifications]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started-push
[Mobile Services .NET How-to Conceptual Reference]: /documentation/articles/mobile-services-windows-dotnet-how-to-use-client-library

[Azure Management Portal]: https://manage.windowsazure.cn/
[Management Portal]: https://manage.windowsazure.cn/
[Mobile Services SDK]: http://go.microsoft.com/fwlink/p/?LinkId=257545
[Developer Code Samples site]:  https://code.msdn.microsoft.com:443/Get-Started-with-Data-in-0e863e57

[MobileServiceClient]: https://msdn.microsoft.com/zh-cn/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx
 
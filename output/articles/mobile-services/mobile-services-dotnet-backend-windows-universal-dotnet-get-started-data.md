<properties 
	pageTitle="Add Mobile Services to an existing universal Windows Store app | Windows Azure" 
	description="Learn how to get started using Mobile Services to leverage data in your Windows Store app." 
	services="mobile-services" 
	documentationCenter="windows" 
	authors="ggailey777" 
	manager="dwrede" 
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.date="11/10/2015"
	wacn.date=""/>

# Add Mobile Services to an existing app

[AZURE.INCLUDE [mobile-services-selector-get-started-data](../includes/mobile-services-selector-get-started-data.md)]

##Overview

This topic shows you how to use Azure Mobile Services as a backend data source for a Windows Store app. In this tutorial, you will download a Visual Studio 2013 project for an app that stores data in memory, create a new mobile service, integrate the mobile service with the app, and view the changes to data made when running the app.

The mobile service that you will create in this tutorial is a .NET backend mobile service. .NET backend enables you to use .NET languages and Visual Studio for server-side business logic in the mobile service, and you can run and debug your mobile service on your local computer. To create a mobile service that lets you write your server-side business logic in JavaScript, see the JavaScript backend version of this topic.

>[AZURE.NOTE]This topic shows you how to use the tooling in Visual Studio Professional 2013 with Update 3 to connect a new mobile service to a universal Windows app. The same steps can be used to connect a mobile service to a Windows Store or Windows Phone Store 8.1 app. To connect a mobile service to a Windows Phone 8.0 or Windows Phone Silverlight 8.1 app, see [Get started with data for Windows Phone](/documentation/articles/mobile-services-dotnet-backend-windows-phone-get-started-data).

> If you cannot upgrade to Visual Studio Professional 2013 Update 3 or you prefer manually add your mobile service project to a Windows Store app solution, see [this version](/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data) of the topic.

##Prerequisites

To complete this tutorial, you need the following:

* An active Azure account. If you don't have an account, you can create a trial account in just a couple of minutes. For details, see [Azure Trial](/pricing/1rmb-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-windows-universal-dotnet-get-started-data%2F).
* <a href="https://www.visualstudio.com/downloads/download-visual-studio-vs" target="_blank">Visual Studio 2013</a> (Update 3 or a later version). 

##Download the GetStartedWithData project

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-download-project](../includes/mobile-services-windows-universal-dotnet-download-project.md)]

##Create a new mobile service from Visual Studio

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service-vs2013](../includes/mobile-services-dotnet-backend-create-new-service-vs2013.md)]

&nbsp;&nbsp;7. In Solution Explorer, open the App.xaml.cs code file in the GetStartedWithData.Shared project folder, and notice the new static field that was added to the **App** class inside a Windows Store app conditional compilation block, which looks like the following example: 

	public static Microsoft.WindowsAzure.MobileServices.MobileServiceClient 
	    todolistClient = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
	        "https://todolist.azure-mobile.cn/",
	        "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX");
		

&nbsp;&nbsp;This code provides access to your new mobile service in your app by using an instance of the [MobileServiceClient](https://msdn.microsoft.com/zh-cn/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx) class. The client is created by supplying the URI and the application key of the new mobile service. This static field is available to all pages in your app.

&nbsp;&nbsp;8. Right-click the Windows Phone app project, click **Add**, click **Connected Service...**, select the mobile service that you just created, and then click **OK**. The same code is added to the shared App.xaml.cs file, but this time within a Windows Phone app conditional compilation block.

At this point, both the Windows Store and Windows Phone Store apps are connected to the new mobile service. The next step is to test the new mobile service project.


##Test the mobile service project locally

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-api-documentation](../includes/mobile-services-dotnet-backend-test-local-service-api-documentation.md)]


##Update the app to use the mobile service

In this section you will update the universal Windows app to use the mobile service as a backend service for the application. You only need to make changes to the MainPage.cs project file in the GetStartedWithData.Shared project folder. 

[AZURE.INCLUDE [mobile-services-windows-dotnet-update-data-app](../includes/mobile-services-windows-dotnet-update-data-app.md)]


##Publish the mobile service to Azure

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../includes/mobile-services-dotnet-backend-publish-service.md)]


##Test the mobile service hosted in Azure

Now we can test both versions of the universal Windows app against the mobile service hosted in Azure.

[AZURE.INCLUDE [mobile-services-windows-universal-test-app](../includes/mobile-services-windows-universal-test-app.md)]

##View the data stored in the SQL Database

[AZURE.INCLUDE [mobile-services-dotnet-backend-view-sql-data](../includes/mobile-services-dotnet-backend-view-sql-data.md)]
 
This concludes the tutorial.

##Next steps

This tutorial demonstrated the basics of enabling a universal Windows app project to work with data in Mobile Services. Next, consider reading up on one of these other topics:

* [Get started with authentication]
  <br/>Learn how to authenticate users of your app.

* [Get started with push notifications] 
  <br/>Learn how to send a very basic push notification to your app.

* [Mobile Services C# How-to Conceptual Reference](/documentation/articles/mobile-services-windows-dotnet-how-to-use-client-library)
  <br/>Learn more about how to use Mobile Services with .NET.


<!-- Images. -->



<!-- URLs. -->
[Validate and modify data with scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-dotnet
[Refine queries with paging]: /develop/mobile/tutorials/add-paging-to-data-dotnet
[Get started with Mobile Services]: /documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started
[Get started with authentication]: /documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-users
[Get started with push notifications]: /documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-push

[Get started with offline data sync]: /documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data

[Azure Management Portal]: https://manage.windowsazure.cn/
[Management Portal]: https://manage.windowsazure.cn/
[Mobile Services SDK]: http://go.microsoft.com/fwlink/p/?LinkId=257545
[Developer Code Samples site]:  https://code.msdn.microsoft.com:443/Get-Started-with-Data-in-0e863e57
[Mobile Services .NET How-to Conceptual Reference]: /documentation/articles/mobile-services-windows-dotnet-how-to-use-client-library
[MobileServiceClient class]: https://msdn.microsoft.com/zh-cn/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx
  
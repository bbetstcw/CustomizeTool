<properties 
	pageTitle="Add push notifications to your Xamarin Android app | Windows Azure" 
	description="Learn how to configure push notifications with Google Cloud Messaging for you Xamarin.Android apps using Azure Mobile Services and Azure Notification Hubs." 
	documentationCenter="xamarin" 
	authors="ggailey777" 
	manager="dwrede" 
	services="mobile-services" 
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.date="12/07/2015"
	wacn.date=""/>

# Add push notifications to your Mobile Services app

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../includes/mobile-services-selector-get-started-push.md)]

##Overview
This topic shows you how to use Azure Mobile Services to send push notifications to a Xamarin.Android app. In this tutorial you add push notifications using the Google Cloud Messaging (GCM) service to the [Get started with Mobile Services] project. When complete, your mobile service will send a push notification each time a record is inserted.

This tutorial requires the following:

+ An active Google account.
+ [Google Cloud Messaging Client Component]. You will add this component during the tutorial.

You should already have the [Xamarin.Android] and the [Azure Mobile Services Component] installed in your project from when you completed either [Get started with Mobile Services].

##<a id="register"></a>Enable Google Cloud Messaging

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a id="configure"></a>Configure your mobile service to send push requests

[AZURE.INCLUDE [mobile-services-android-configure-push](../includes/mobile-services-android-configure-push.md)]

##<a id="update-scripts"></a>Update the registered insert script to send notifications

>[AZURE.TIP] The following steps show you how to update the script registered to the insert operation on the TodoItem table in the Azure Management Portal. You can also access and edit this mobile service script directly in Visual Studio, in the Azure node of Server Explorer.

[AZURE.INCLUDE [mobile-services-javascript-backend-android-push-insert-script](../includes/mobile-services-javascript-backend-android-push-insert-script.md)]


##<a id="configure-app"></a>Configure the existing project for push notifications

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Add push notifications code to your app

[AZURE.INCLUDE [mobile-services-xamarin-android-push-add-to-app](../includes/mobile-services-xamarin-android-push-add-to-app.md)]

##<a id="test"></a>Test push notifications in your app

You can test the app by directly attaching an Android phone with a USB cable, or by using a virtual device in the emulator.

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../includes/mobile-services-android-push-notifications-test.md)]

You have successfully completed this tutorial.

## <a name="next-steps"> </a>Next steps

Learn more about Mobile Services and Notification Hubs in the following topics:

* [Get started with authentication](/documentation/articles/mobile-services-android-get-started-users)
  <br/>Learn how to authenticate users of your app with different account types using mobile services.

* [What are Notification Hubs?](/documentation/articles/notification-hubs-overview)
  <br/>Learn more about how Notification Hubs works to deliver notifications to your apps across all major client platforms.

* [Debug Notification Hubs applications](https://msdn.microsoft.com/zh-cn/library/dn530751.aspx)
  </br>Get guidance troubleshooting and debugging Notification Hubs solutions. 

* [How to use the .NET client library for Mobile Services](/documentation/articles/mobile-services-windows-dotnet-how-to-use-client-library)
  <br/>Learn more about how to use Mobile Services with Xamarin C# code.

* [Mobile Services server script reference](/documentation/articles/mobile-services-how-to-use-server-scripts)
  <br/>Learn more about how to implement business logic in your mobile service.

<!-- URLs. -->
[Get started with Mobile Services]: /documentation/articles/mobile-services-ios-get-started

[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Xamarin.Android]: http://xamarin.com/download/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
 
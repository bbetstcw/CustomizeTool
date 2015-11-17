<properties
	pageTitle="Get Started with Azure Mobile Services for Android apps (JavaScript backend)"
	description="Follow this tutorial to get started using Azure Mobile Services for Android development (JavaScript backend)."
	services="mobile-services"
	documentationCenter="android"
	authors="RickSaling"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.date="10/08/2015"
	wacn.date=""/>

# Get started with Mobile Services for Android  (JavaScript backend)

[AZURE.INCLUDE [mobile-services-selector-get-started](../includes/mobile-services-selector-get-started.md)]

<!-- deleted by customization
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../includes/mobile-services-hero-slug.md)]
-->

This tutorial shows you how to add a cloud-based backend service to an Android app using Azure Mobile Services. In this tutorial, you will create both a new mobile service and a simple **To do list** app that stores app data in the new mobile service.
<!-- deleted by customization

> [AZURE.VIDEO mobile-get-started-android]
-->

A screenshot from the completed app is below:
![](./media/mobile-services-android-get-started/mobile-quickstart-completed-android.png)

## Prerequisites

Completing this tutorial requires the [Android Developer Tools](https://developer.android.com/sdk/index.html)), which includes the Android Studio integrated development environment, and the latest Android platform. Android 4.2 or a later version is required.

The downloaded quickstart project contains the Azure Mobile Services SDK for Android.

> [AZURE.IMPORTANT] To complete this tutorial, you need an Azure account. If you don't have an account, you can create a trial account in just a couple of minutes. For details, see [Azure Trial](/pricing/1rmb-trial/?WT.mc_id=AE564AB28).


## Create a new mobile service

[AZURE.INCLUDE [mobile-services-create-new-service](../includes/mobile-services-create-new-service.md)]

## Create a new Android app

Once you have created your mobile service, you can follow an easy quickstart in the Management Portal to either create a new app or modify an existing app to connect to your mobile service.

In this section you will create a new Android app that is connected to your mobile service.

1.  In the Management Portal, click **Mobile Services**, and then click the mobile service that you just created.

2. In the quickstart tab, click **Android** under **Choose platform** and expand **Create a new Android app**.

   	![](./media/mobile-services-android-get-started/mobile-portal-quickstart-android1.png)

   	This displays the three easy steps to create an Android app connected to your mobile service.

  	![](./media/mobile-services-android-get-started/mobile-quickstart-steps-android-AS.png)

3. If you haven't already done so, download and install the [Android Developer Tools](http://developer.android.com/sdk/index.html) on your local computer or virtual machine.

4. Click **Create TodoItem table** to create a table to store app data.


<!-- deleted by customization
5. Now download your app by pressing the **Download** button.
-->
<!-- keep by customization: begin -->
5. Now download your app:
	- The latest app version uses the Mobile Services Android SDK 2.0. You can download that version from <a href="https://github.com/RickSaling/mobile-services-samples/tree/futures">here</a>. Click **Download Zip**, unzip it, and the project is in GettingStarted under the Android folder.
	 
	- An earlier version uses the prior version of the SDK. To use it, under **Download and run your app**, click **Download**. This downloads the project for the sample _To do list_ application that is connected to your mobile service. The project files are compressed, so browse to their location and expand the files on your computer.

<!-- keep by customization: end -->

## Run your Android app

[AZURE.INCLUDE [mobile-services-run-your-app](../includes/mobile-services-android-get-started.md)]

<!-- keep by customization: begin -->
### Look at code (optional)

If you would like to see the source code of the completed app, go [here](https://github.com/RickSaling/mobile-services-samples/tree/androidStudio/GettingStarted/AndroidStudio).


If you would like to see the Eclipse version of this tutorial, go to : [Get started (Eclipse)](/documentation/articles/mobile-services-android-get-started-EC).
<!-- keep by customization: end -->

## <a name="next-steps"> </a>Next Steps
Now that you have completed the quickstart, learn how to perform additional important tasks in Mobile Services:

* [Get started with data]
  <br/>Learn more about storing and querying data using Mobile Services.

* [Get started with authentication]
  <br/>Learn how to authenticate users of your app with an identity provider.

* [Get started with push notifications]
  <br/>Learn how to send a very basic push notification to your app.


<!-- deleted by customization
[AZURE.INCLUDE [app-service-disqus-feedback-slug](../includes/app-service-disqus-feedback-slug.md)]

-->
<!-- keep by customization: begin -->


<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/mobile-services-android-get-started/mobile-portal-quickstart-android.png
[7]: ./media/mobile-services-android-get-started/mobile-quickstart-steps-android-AS.png
[8]: ./media/mobile-services-android-get-started/mobile-eclipse-quickstart.png
[10]: ./media/mobile-services-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/mobile-services-android-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-android-get-started/mobile-data-browse.png
[14]: ./media/mobile-services-android-get-started/mobile-services-import-android-workspace.png
[15]: ./media/mobile-services-android-get-started/mobile-services-import-android-project.png
<!-- keep by customization: end -->

<!-- URLs. -->
[Get started (Eclipse)]: /documentation/articles/mobile-services-android-get-started-ec
[Get started with data]: /documentation/articles/mobile-services-android-get-started-data
[Get started with authentication]: /documentation/articles/mobile-services-android-get-started-users
[Get started with push notifications]: /documentation/articles/mobile-services-javascript-backend-android-get-started-push
<!-- keep by customization: begin -->
[Android SDK]: http://developer.android.com/sdk/index.html
[Android Studio]: https://developer.android.com/sdk/index.html
<!-- keep by customization: end -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[Management Portal]: https://manage.windowsazure.cn/
<properties 
	pageTitle="Add authentication to your Universal Windows 8.1 app | Windows Azure" 
	description="Learn how to use Mobile Services to authenticate users of your Universal Windows 8.1 app by using various identity providers, including Google, Facebook, Twitter, and Microsoft." 
	services="mobile-services" 
	documentationCenter="windows" 
	authors="ggailey777" 
	manager="dwrede" 
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.date="10/05/2015"
	wacn.date=""/>

# Add authentication to your Mobile Services app 

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../includes/mobile-services-selector-get-started-users.md)]

## Overview

This topic shows you how to authenticate users in Azure Mobile Services from your universal Windows app. In this tutorial, you add authentication to the quickstart project using an identity provider that is supported by Mobile Services. After being successfully authenticated and authorized by Mobile Services, the user ID value is displayed.

This tutorial is based on the Mobile Services quickstart. You must also first complete the tutorial [Get started with Mobile Services] or [Add Mobile Services to an existing app](/documentation/articles/mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data). 

>[AZURE.NOTE]This tutorial shows you how to use server-directed authentication for users in Windows Store and Windows Phone Store 8.1 apps. Fore information about client-directed authentication, see [Logging in with Google, Microsoft and Facebook SDKs to Azure Mobile Services](http://azure.microsoft.com/blog/2014/10/27/logging-in-with-google-microsoft-and-facebook-sdks-to-azure-mobile-services/).

##<a name="register"></a>Register your app for authentication and configure Mobile Services

[AZURE.INCLUDE [mobile-services-register-authentication](../includes/mobile-services-register-authentication.md)] 

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../includes/mobile-services-dotnet-backend-aad-server-extension.md)] 

##<a name="permissions"></a>Restrict permissions to authenticated users

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../includes/mobile-services-restrict-permissions-dotnet-backend.md)] 

&nbsp;&nbsp;6. In Visual Studio, right-click the Windows Store project for the TodoList app and click **Set as StartUp Project**.

&nbsp;&nbsp;7. In the shared project, open the App.xaml.cs project file, locate the definition for the [MobileServiceClient](http://msdn.microsoft.com/zh-cn/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx), and make sure that it is configured to connect to the mobile service running in Azure.

>[AZURE.NOTE]When you use Visual Studio tools to connect your app to a Mobile Service, the tool generate two sets of **MobileServiceClient** definitions, one for each client platform. This is a good time to simplify the generated code by unifying the `#if...#endif` wrapped **MobileServiceClient** definitions into a single unwrapped definition used by both versions of the app. You won't need to do this when you downloaded the quickstart app from the Azure Management portal.

&nbsp;&nbsp;8. Press the F5 key to run the Windows store app, and verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.
   
&nbsp;&nbsp;This happens because the app attempts to access Mobile Services as an unauthenticated user, but the *TodoItem* table now requires authentication.

Next, you will update the app to authenticate users before requesting resources from the mobile service.

##<a name="add-authentication"></a>Add authentication to the app

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-authenticate-app](../includes/mobile-services-windows-universal-dotnet-authenticate-app.md)] 

>[AZURE.NOTE]If you registered your Windows Store app package information with Mobile Services, you should call the <a href="https://msdn.microsoft.com/zh-cn/library/azure/microsoft.windowsazure.mobileservices.singlesignonextensions.loginasync.aspx" target="_blank">LoginAsync</a> method by supplying a value of **true** for the *useSingleSignOn* parameter. If you do not do this, your users will continue to be presented with a login prompt every time that the login method is called.

##<a name="tokens"></a>Store the authorization tokens on the client

[AZURE.INCLUDE [mobile-services-windows-store-dotnet-authenticate-app-with-token](../includes/mobile-services-windows-store-dotnet-authenticate-app-with-token.md)] 


## <a name="next-steps"> </a>Next steps

In the next tutorial, [Service-side authorization of Mobile Services users][Authorize users with scripts], you will take the user ID value provided by Mobile Services based on an authenticated user and use it to filter the data returned by Mobile Services. 

##See also

+ [Enhanced users feature](http://azure.microsoft.com/blog/2014/10/02/custom-login-scopes-single-sign-on-new-asp-net-web-api-updates-to-the-azure-mobile-services-net-backend/)<br/>
You can get additional user data maintained by the identity provider in your mobile service by by calling the **ServiceUser.GetIdentitiesAsync()** method in a .NET backend. 

+ [Mobile Services .NET How-to Conceptual Reference]<br/>Learn more about how to use Mobile Services with a .NET client.


<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started
[Get started with data]: /documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data
[Get started with authentication]: /documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-users
[Get started with push notifications]: /documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-push
[Authorize users with scripts]: /documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-authorize-users-in-scripts
[JavaScript and HTML]: /documentation/articles/mobile-services-dotnet-backend-windows-store-javascript-get-started-users

[Azure Management Portal]: https://manage.windowsazure.cn/
[Mobile Services .NET How-to Conceptual Reference]: /documentation/articles/mobile-services-windows-dotnet-how-to-use-client-library
[Register your Windows Store app package for Microsoft authentication]: /documentation/articles/mobile-services-how-to-register-store-app-package-microsoft-authentication
 

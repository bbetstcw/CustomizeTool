<properties
   pageTitle="Azure Active Directory developer's guide | Windows Azure"
   description="This article provides a comprehensive guide to developer-oriented resources for Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
	ms.service="active-directory"
	ms.date="10/16/2015"
	wacn.date=""/>


# Azure Active Directory developer's guide

## Overview
As an identity management as a service (IDMaaS) platform, Azure Active Directory provides developers an effective way to integrate identity management into their applications. The following articles provide overviews on implementation and key features of Azure Active Directory. We suggest that you read them in order, or jump to [Getting started](#getting-started) if you're ready to dig in.


1. [The benefits of Azure Active Directory integration](/documentation/articles/active-directory-how-to-integrate): Discover why integration with Azure Active Directory offers the best solution for secure sign-in and authorization.

1. [Active Directory authentication scenarios](/documentation/articles/active-directory-authentication-scenarios): Take advantage of simplified authentication in Azure Active Directory to provide sign-on to your application.

1. [Integrating applications with Azure Active Directory](/documentation/articles/active-directory-integrating-applications): Learn how to add, update, and remove applications from Azure Active Directory, and about the branding guidelines for integrated apps.

1. [Azure Active Directory Graph API](/documentation/articles/active-directory-graph-api): Use the Azure Active Directory Graph API to programmatically access Azure Active Directory through REST API endpoints.

1. [Azure Active Directory authentication libraries](/documentation/articles/active-directory-authentication-libraries): Easily authenticate users to obtain access tokens by using the Azure authentication libraries.


## Getting started

These tutorials are tailored for multiple platforms and can help you quickly start developing with Azure Active Directory. As a prerequisite, you must [get an Azure Active Directory tenant](/documentation/articles/active-directory-howto-tenant).

### Mobile and PC application quick-start guides

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)| [![Windows Phone](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsphone.md)|[![Windows Store](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:
|[iOS](/documentation/articles/active-directory-devquickstarts-ios)|[Android](/documentation/articles/active-directory-devquickstarts-android)|[.NET](/documentation/articles/active-directory-devquickstarts-dotnet)|[Windows Phone](/documentation/articles/active-directory-devquickstarts-windowsphone)|[Windows Store](/documentation/articles/active-directory-devquickstarts-windowsstore)|[Xamarin](/documentation/articles/active-directory-devquickstarts-xamarin)|[Cordova](/documentation/articles/active-directory-devquickstarts-cordova)

### Web application quick-start guides

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Javascript](./media/active-directory-developers-guide/javascript.png)](active-directory-devquickstarts-angular.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md)
|:--:|:--:|:--:|
|[.NET](/documentation/articles/active-directory-devquickstarts-webapp-dotnet)|[Javascript](/documentation/articles/active-directory-devquickstarts-angular)|[Node.js](/documentation/articles/active-directory-devquickstarts-openidconnect-nodejs)

### Web API quick-start guides

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](/documentation/articles/active-directory-devquickstarts-webapi-dotnet)|[Node.js](/documentation/articles/active-directory-devquickstarts-webapi-nodejs)

### Querying the directory quickstart guide

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph API](/documentation/articles/active-directory-graph-api-quickstart)|

## How-tos

These articles describe how to perform specific tasks by using Azure Active Directory:

- [Get an Azure Active Directory tenant](/documentation/articles/active-directory-howto-tenant)
- [List your application in the Azure Active Directory application gallery](/documentation/articles/active-directory-app-gallery-listing)
- [Understand the Azure Active Directory application manifest](/documentation/articles/active-directory-application-manifest)
- [Create an app with Office 365 APIs](https://msdn.microsoft.com/office/office365/howto/getting-started-Office-365-APIs)
- [Submit web apps for Office 365 to the Seller Dashboard](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Preview: How to build apps that sign users in with both personal & work or school accounts](/documentation/articles/active-directory-appmodel-v2-overview)
- [Preview: How to build apps that sign up & sign in consumers](/documentation/articles/active-directory-b2c-overview)

## Reference

These articles provide a foundation reference for REST and authentication library APIs, protocols, errors, code samples, and endpoints.  

###  Support
- [Tagged questions](http://stackoverflow.com/questions/tagged/azure-active-directory): Find Azure Active Directory solutions on Stack Overflow by searching for the tags [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) and [adal](http://stackoverflow.com/questions/tagged/adal).

### Code

- [Azure Active Directory open-source libraries](http://github.com/AzureAD): The easiest way to find a library’s source is by using our [library list](/documentation/articles/active-directory-authentication-libraries).

- [Azure Active Directory samples](http://github.com/AzureADSamples): The easiest way to navigate the list of samples is by using the [index of code samples](/documentation/articles/active-directory-code-samples).


### Graph API

- [Graph API reference](https://msdn.microsoft.com/zh-cn/library/azure/hh974476.aspx): REST reference for the Azure Active Directory Graph API. [View the interactive Graph API reference experience](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Graph API permission scopes](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/graph-api-permission-scopes): OAuth 2.0 permission scopes that are used to control the access that an app has to directory data in a tenant.


### Authentication protocols

- [SAML 2.0 protocol reference](https://msdn.microsoft.com/zh-cn/library/azure/dn195591.aspx): The SAML 2.0 protocol enables applications to provide a single sign-on experience to their users.


- [OAuth 2.0 protocol reference](https://msdn.microsoft.com/zh-cn/library/azure/dn645545.aspx): You can use the OAuth 2.0 protocol to authorize access to web applications and web APIs in your Azure Active Directory tenant.


- [OpenID Connect 1.0 protocol reference](https://msdn.microsoft.com/zh-cn/library/azure/dn645541.aspx): The OpenID Connect 1.0 protocol extends OAuth 2.0 for use as an authentication protocol.


- [WS-Federation 1.2 protocol reference](https://msdn.microsoft.com/zh-cn/library/azure/dn903702.aspx): The WS-Federation 1.2 protocol is specified in the Web Services Federation Version 1.2 Specification.

- [Supported token and claim types](/documentation/articles/active-directory-token-and-claims): You can use this guide to understand and evaluate the claims in the SAML 2.0 and JSON Web Tokens (JWT) tokens.

## Social

- [Active Directory Team blog](http://blogs.technet.com/b/ad/): The latest developments in the world of Azure Active Directory.

- [Azure Active Directory Graph Team blog](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory information that's specific to the Graph API.

- [Cloud Identity](http://www.cloudidentity.net): Thoughts on identity management as a service, from a principal Azure Active Directory PM.  

- [Azure Active Directory on Twitter](https://twitter.com/azuread): Azure Active Directory announcements in 140 characters or fewer.
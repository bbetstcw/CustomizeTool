<properties 
	pageTitle="Get Started with Azure Active Directory and Visual Studio connected services (WebApi projects) | Windows Azure" 
	description="How to get started using Azure Active Directory in WebApi projects after connecting to or creating an Azure AD using Visual Studio connected services" 
    services="active-directory"
	documentationCenter="" 
	authors="TomArcher" 
	manager="douge" 
	editor=""/>
  
<tags 
	ms.service="active-directory" 
	ms.date="12/18/2015" 
	wacn.date=""/>

# Get Started with Azure Active Directory and Visual Studio connected services (WebApi projects)

> [AZURE.SELECTOR]
> - [Getting Started](/documentation/articles/vs-active-directory-webapi-getting-started)
> - [What Happened](/documentation/articles/vs-active-directory-webapi-what-happened)

##Requiring authentication to access controllers
 
All controllers in your project were adorned with the **Authorize** attribute. This attribute will require the user to be authenticated before accessing the APIs defined by these controllers. To allow the controller to be accessed anonymously, remove this attribute from the controller. If you want to set the permissions at a more granular level, apply the attribute to each method that requires authorization instead of applying it to the controller class.

[Learn more about Azure Active Directory](/documentation/services/identity/)
 
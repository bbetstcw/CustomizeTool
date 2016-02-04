<properties 
	pageTitle="What happened to my Cordova project (Visual Studio Connected Services) | Windows Azure" 
	description="Describes what happened to your Azure Cordova project after adding Azure Mobile Services by using Visual Studio Connected Services " 
	services="mobile-services" 
	documentationCenter="na" 
	authors="TomArcher" 
	manager="douge" 
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.date="01/05/2016"
	wacn.date=""/>

# What happened to my Azure Cordova project after adding Azure Mobile Services by using Visual Studio Connected Services?

##References Added

The Azure Mobile Service Client plugin included with all Multi-Device Hybrid Apps has been enabled.
  
##Connection string values for Mobile Services

Under `services\mobileServices\settings`, a new JavaScript (.js) file with a **MobileServiceClient** was generated containing the selected mobile serviceâs application URL and application key. The file contains the initialization of a mobile service client object, similar to the following code.

	var mobileServiceClient;
	document.addEventListener("deviceready", function() {
            mobileServiceClient = new WindowsAzure.MobileServiceClient(
	        "<your mobile service name>.azure-mobile.cn",
	        "<insert your key>"
	    );

[Learn more about mobile services](/documentation/services/mobile-services) 


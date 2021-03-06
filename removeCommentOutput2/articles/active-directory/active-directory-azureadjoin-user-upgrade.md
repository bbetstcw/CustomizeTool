<properties 
	pageTitle="Set up a Windows 10 device with Azure AD from Settings| Windows Azure" 
	description="Explains how users can join to Azure AD through the settings menu." 
	services="active-directory" 
	documentationCenter="" 
	authors="femila" 
	manager="stevenpo" 
	editor=""
	tags="azure-classic-portal"/>

<tags
	ms.service="active-directory"
	ms.date="11/19/2015"
	wacn.date=""/>

# Set up a Windows 10 device with Azure AD from Settings
If you are already using Windows 7 or 8, and your machine is upgraded to Windows 10, you can join to Azure AD through the Settings menu.

To join to Azure AD from the settings menu
-----------------------------------------------------------------------------------------------

1. From the Start menu, click the Settings charm.
2. From Settings->**System**->**About**->**Join Azure AD**
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center>

3. Click **Continue** on the Azure AD Join message window.
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center>
4. Provide your sign in credentials. This sign in experience will include all steps required to complete authentication. If you are part of a federated tenant, your administrator will provide you federation experience hosted by your organization.
<center>
![](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center>
5. If your organization has configured multi-factor authenticaion for joining to Azure AD, you will have to provide the second factor before being able to proceed.
6. Click **Accept** on the Allow this device to be managed screen.
7. you should see the message "Your device is now joined to your organization in Azure AD".


## Additional Information
* [Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join](/documentation/articles/active-directory-azureadjoin-user-upgrade)
* [Learn about usage scenarios and deployment considerations for Azure AD Join](/documentation/articles/active-directory-azureadjoin-deployment-aadjoindirect)
* [Set up Azure AD Join](/documentation/articles/active-directory-azureadjoin-setup)

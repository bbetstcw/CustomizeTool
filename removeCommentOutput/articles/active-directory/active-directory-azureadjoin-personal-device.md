

<properties 
	pageTitle="Join a personal device to your organization| Windows Azure" 
	description="Explains how users can register their personal Windows 10 computers to their corporate network, provides deployment steps for a BYOD scenario." 
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

# Join a personal device to your organization

To join a Windows 10 device to your organization
--------------------------------------------------------------------------------------------
1.	From the **Start** menu, select **Settings**. 
2.	Select **Accounts**, and then click **Your account**.
3.	Click **Add Work or School account**, and then type in your organizational account.
4.	You will then be taken to the sign-on page for your organization. Enter your username and password and click **OK**.
5.	You will then be prompted for a multi-factor authentication challenge. This is configurable by IT.
6.	Azure AD will then check whether this user/device requires mobile device management (MDM) enrollment. 
7.	Windows will then register the device in the organizationâs directory in Azure AD and enroll it in MDM.
8.	When this is done, if you are a managed user, Windows will wrap up the setup process and take the user to the desktop through the auto-sign on.
9.	If you are a federated user, you will be taken to the Windows sign-on screen and have to enter your credentials.

## Additional Information
* [Windows 10 for the enterprise: Ways to use devices for work](/documentation/articles/active-directory-azureadjoin-windows10-devices-overview)
* [Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join](/documentation/articles/active-directory-azureadjoin-user-upgrade)
* [Authenticating identities without passwords through Microsoft Passport](/documentation/articles/active-directory-azureadjoin-passport)
* [Learn about usage scenarios for Azure AD Join](/documentation/articles/active-directory-azureadjoin-deployment-aadjoindirect)
* [Connect domain-joined devices to Azure AD for Windows 10 experiences](/documentation/articles/active-directory-azureadjoin-devices-group-policy)
* [Set up Azure AD Join](/documentation/articles/active-directory-azureadjoin-setup)

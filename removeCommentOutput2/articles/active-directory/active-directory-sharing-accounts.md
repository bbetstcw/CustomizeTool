<properties
	pageTitle="Sharing accounts using Azure AD |  Windows Azure"
	description="Describes how Azure Active Directory enables organizations to securely share accounts for on-premises apps and consumer cloud services."
	services="active-directory"
	documentationCenter=""
	authors="msStevenPo"
 	manager="stevenpo"
	editor=""/>

 <tags
	ms.service="active-directory"
	ms.date="10/16/2015"
	wacn.date=""/>

# Sharing accounts with Azure AD

## Overview
Sometimes organizations need to use a single username and password for multiple people. This typically happens in two cases:

- When accessing applications that require a unique login and password for each user, whether on-premises apps or consumer cloud services ( e.g. corporate social media accounts).
- When creating multi-user environments. You may have a single, local account that has elevated privileges and can be used to do core setup, administration, and recovery activities (e.g. the local "global administrator" account for Office 365 or the root account in Salesforce).

Traditionally, these accounts would be shared by distributing the credentials (username/password) to the right individuals or storing them in a shared location where multiple trusted agents can access them.

The traditional sharing model has several drawbacks:

- Enabling access to new applications requires you to distribute credentials to everyone that needs access.
- Each shared application may require its own unique set of shared credentials, requiring users to remember multiple sets of credentials. When users have to remember many credentials, the risk increases that they will resort to risky practices. (e.g. writing down passwords).
- You can't tell who has access to an application.
- You can't tell who has *accessed* an application.
- When you need to remove access to an application, you have to update the credentials and re-distribute them to everyone that needs access to that application.

## Azure Active Directory account sharing

Azure AD provides a new approach to using shared accounts that eliminates these drawbacks.

The Azure AD administrator configures which applications a user can access by using the Access Panel and choosing the type of single sign-on best suited for that application. One of those types, *password-based single-sign on*, lets Azure AD act as a kind of "broker" during the sign-on process for that app.

Users log in once with their organizational account. This is the same account that they regularly use to access their desktop or email. They can discover and access only those applications that they are assigned to. With shared accounts, this list of applications can include any number of shared credentials. The end user doesn't need to remember or write down the various accounts they may be using.

Shared accounts not only increase oversight and improve usability, they also enhance your security. Users with permissions to use the credentials don't see the shared password, but rather get permissions to use the password as part of an orchestrated authentication flow. Further, with some password SSO applications, you have the option to have Azure AD periodically rollover (update) the password using large, complex passwords, increasing the account security. The administrator can easily grant or revoke access to an application and also know who has access to the account and who accessed it in the past.

Azure AD supports shared accounts for any Enterprise Mobility Suite (EMS), Premium, or Basic licensed users, across all types of password single sign on applications. You can share accounts for any of thousands of pre-integrated applications in the application gallery and can add your own password-authenticating application with [custom SSO apps](/documentation/articles/active-directory-single-sign-on-newly-acquired-saas-apps).

Azure AD features that enable account sharing include:

- [Password single sign-on](/documentation/articles/active-directory-appssoaccess-whatis#password-based-single-sign-on)
- Password single sign-on agent
- [Group assignment](/documentation/articles/active-directory-accessmanagement-self-service-group-management)
- Custom Password apps
- [App usage dashboard/reports](/documentation/articles/active-directory-passwords-get-insights)
- End user access portals
- [App proxy](/documentation/articles/active-directory-application-proxy-get-started)

## Sharing an account
To use Azure AD to share an account you will need to:

- Configure the application for password Single Sign-On (SSO)
- Use [group based assignment](/documentation/articles/active-directory-accessmanagement-group-saasapps) and select the option to enter a shared credential
- Optional: in some applications, such as Facebook, Twitter, or LinkedIn, you can enable the option for [Azure AD automated password roll-over](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

You can also make your shared account more secure with Multi-Factor Authentication (MFA) (learn more about [securing applications with Azure AD](/documentation/articles/multi-factor-authentication-get-started)) and you can delegate the ability to manage who has access to the application using [Azure AD Self-service](/documentation/articles/active-directory-accessmanagement-self-service-group-management) Group Management.

## Related articles

- [Protecting apps with conditional access](/documentation/articles/active-directory-conditional-access)
- [Self-service group management/SSAA](/documentation/articles/active-directory-accessmanagement-self-service-group-management)

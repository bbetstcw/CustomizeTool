<properties
	pageTitle="How to provide secure remote access to on-premises apps"
	description="Covers how to use Azure AD Application Proxy to provide secure remote access to your on-premises apps."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.date="01/06/2016"
	wacn.date=""/>

# How to provide secure remote access to on-premises applications

> [AZURE.NOTE] Application Proxy is a feature that is available only if you upgraded to the Premium or Basic edition of Azure Active Directory. For more information, see [Azure Active Directory editions](/documentation/articles/active-directory-editions).

You want to provide access for remote users who have all kinds of devices - managed, and unmanaged; tablets, smartphones, and laptops. But providing secure access to a myriad of resources can be complex. In recent years, reverse proxies were a popular way to provide secure remote access, but they needed to be behind firewalls which were hard to secure and hard to make highly available.

## Secure remote access in the cloud
In a modern cloud environment, we take remote access to the next level using Application Proxy in Windows Azure Active Directory (AD). Application Proxy is a feature of Azure AD that is supplied as a service, meaning that it's easy to deploy and use. It integrates with Azure AD, the same identity platform that is used by Office 365.

## What is Azure Active Directory Application Proxy?
Application Proxy provides single sign-on (SSO) and secure remote access for web applications hosted on-premises, such as SharePoint sites and Outlook Web Access. Your on-premises web applications can now be accessed the same way as your SaaS apps in Azure Active Directory, without the need for a VPN or changing the network infrastructure. Application Proxy lets you publish applications, and then employees can log into your apps from home or on their own devices, and authenticate through this cloud-based proxy.

## How does it work?
### Enabling Access
Application Proxy works by installing a slim Windows Server service called the Connector inside your network. The Connector doesn't necessitate opening any inbound ports and you don't have to put anything in the DMZ. If you have a lot of traffic to your apps you can add more Connectors, and the service will take care of the load balancing. The Connectors are stateless and pull everything from the cloud as necessary.
When a user accesses applications remotely, from any device, he's authenticated by Azure Active Directory and gets access to the application.

### Single sign-on
Azure AD Application Proxy provides single sign-on to applications that use Integrated Windows Authentication (IWA), or claims-aware applications. If your application uses IWA, Application Proxy impersonates the user using Kerberos Constrained Delegation to provide SSO. If you have a claims-aware application that trusts Azure Active Directory, SSO is achieved because the user was already authenticated by Azure AD.

## How to get started
Make sure you have an Azure AD basic or premium subscription and an Azure AD directory for which you are a global administrator. You also need Azure AD basic or premium licenses for the directory administrator and users accessing the apps. Take a look at  [Azure Active Directory editions](/documentation/articles/active-directory-editions) for more information.

### Getting started enabling remote access to on-premises applications
Setting up Application Proxy is accomplished in two steps:

1. [Enable Application Proxy and configure the Connector](/documentation/articles/active-directory-application-proxy-enable)  
2. [Publish applications](/documentation/articles/active-directory-application-proxy-publish) - use the quick and easy wizard to get your on-premises apps published and accessible remotely.

## What's next?
There's a lot more you can do with Application Proxy:

- [Publish applications using your own domain name](/documentation/articles/active-directory-application-proxy-custom-domains)
- [Enable single-sign on](/documentation/articles/active-directory-application-proxy-sso-using-kcd)
- [Working with claims aware applications](/documentation/articles/active-directory-application-proxy-claims-aware-apps)
- [Enable conditional access](/documentation/articles/active-directory-application-proxy-conditional-access)


### Learn more about Application Proxy
- [Take a look at our online help](/documentation/articles/active-directory-application-proxy-enable)
- [Check out the Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)
- [Watch our videos on Channel 9!](http://channel9.msdn.com/events/Ignite/2015/BRK3864)

## Additional resources
* [Sign up for Azure as an organization](/documentation/articles/sign-up-organization)
* [Azure Identity](/documentation/articles/fundamentals-identity)

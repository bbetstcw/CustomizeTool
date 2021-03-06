<properties 
	pageTitle="Managing access to resources with Azure Active Directory groups| Windows Azure" 
	description="How to use groups in  Azure Active Directory for access management to on-premises and cloud applications and resources."
	services="active-directory" 
	documentationCenter="" 
	authors="curtand"
	manager="stevenpo"
	editor=""/>

<tags 
	ms.service="active-directory" 
	ms.date="08/14/2015" 
	wacn.date="" />


# Managing access to resources with Azure Active Directory groups

Azure Active Directory (Azure AD) is a comprehensive identity and access management solution that provides a robust set of capabilities to manage access to on-premises and cloud applications and resources including Microsoft online services like Office 365 and a world of non-Microsoft SaaS applications.


> [AZURE.NOTE] To use Azure Active Directory, you need an Azure account. If you don't have an account, you can [sign up for a free Azure account](/pricing/1rmb-trial/).


Within Azure Active Directory, one of the major features is the ability to manage access to resources. These resources can be part of the directory, as in the case of permissions to manage objects through roles in the directory, or resources that are external to the directory, such as SaaS applications, Azure services, and SharePoint sites or on premise resources. There are 4 ways a user can be assigned access rights to a resource:


1\.	Direct assignment

Users can be assigned directly to a resource by the owner of that resource.

2\.	Group membership

A group can be assigned to a resource by the resource owner, and by doing so, granting the members of that group access to the resource. Membership of the group can then be managed by the owner of the group. Effectively, the resource owner delegates the permission to assign users to their resource to the owner of the group.

3\.	Rule based

The resource owner can use a rule to express which users should be assigned access to a resource. The outcome of the rule depends on the attributes used in that rule and their values for specific users, and by doing so, the resource owner effectively delegates the right to manage access to their resource to the authoritative source for the attributes that are used in the rule. Note that the resource owner still manages the rule itself and determines which attributes and values provide access to their resource.

4\.	External authority

The access to a resource is derived from an external source; for example, a group that is synchronized from an authoritative source such as an on premise directory or a SaaS app such as WorkDay. The resource owner assigns the group to provide access to the resource, and the external source manages the members of the group.

  ![Overview of access management diagram](./media/active-directory-access-management-groups/access-management-overview.png)


###Watch a video that explains Access Management

You can watch a short video that explains more about this:

[Azure AD : Introduction to Dynamic Memberships for Groups](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups)

##How does Access Management in Azure Active Directory work?
At the center of Azure Active Directoryâs access management solution is the security group. Using a security group to manage access to resources is a well-known paradigm, which allows for a flexible and easily understood way to provide access to a resource for the intended group of users. The resource owner (or the administrator of the directory) can assign a group to provide a certain access right to the resources they own. The members of the group will be provided the access, and the resource owner can delegate the right to manage the members list of a group to someone else â such as a department manager or a helpdesk administrator.

![Azure Active Directory access management diagram](./media/active-directory-access-management-groups/active-directory-access-management-works.png)
The owner of a group can also make that group available for self service requests. In doing so, an end user can search for and find the group and make a request to join, effectively seeking permission to access the resources that are managed through the group. The owner of the group can set up the group so that join requests are approved automatically or require approval by the owner of the group. When a user makes a request to join a group, the join request is forwarded to the owners of the group. If one of the owners approves the request, the requesting user is notified and the user is joined to the group. If one of the owners denies the request, the requesting user is notified but not joined to the group.


## Getting started with access management
Ready to get started? You should try out some of the basic tasks you can do with Azure AD groups. Use these capabilities to provide specialized access to different groups of people for different resources in your organization. A list of basic first steps are listed below.


* [Creating a simple rule to configure dynamic memberships for a group](/documentation/articles/active-directory-accessmanagement-simplerulegroup)

* [Using a group to manage access to SaaS applications](/documentation/articles/active-directory-accessmanagement-group-saasapps)

* [Making a group available for end user self-service](/documentation/articles/active-directory-accessmanagement-self-service-group-management)

* [Syncing an on-premise group to  Azure using Azure AD Connect](/documentation/articles/active-directory-aadconnect)

* [Managing Owners to a Group](/documentation/articles/active-directory-accessmanagement-managing-group-owners)


## Next steps for access management
Now that you have understood the basics of access management, here are some additional advanced capabilities available in Azure Active Directory for managing access to your applications and resources.

* [Using a simple rule to create a group](/documentation/articles/active-directory-accessmanagement-simplerulegroup)

* [Using Attributes to Create Advanced Rules](/documentation/articles/active-directory-accessmanagement-groups-with-advanced-rules)

* [Managing Security groups in Azure Active Directory](/documentation/articles/active-directory-accessmanagement-manage-groups)

* [Setting up Dedicated Groups in Azure Active Directory](/documentation/articles/active-directory-accessmanagement-dedicated-groups)


## Learn more
These articles provide additional information on Azure Active Directory.

* [What is Azure Active Directory?](/documentation/articles/active-directory-whatis)

* [Integrating your on-premises identities with Azure Active Directory](/documentation/articles/active-directory-aadconnect)

* [Graph API Reference for Groups](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

<properties 
	pageTitle="Dedicated groups in Azure Active Directory | Windows Azure" 
	description="Overview of dedicated groups in Azure AD and how they are created." 
	services="active-directory" 
	documentationCenter="" 
	authors="femila" 
	manager="stevenpo" 
	editor=""
	tags="azure-classic-portal"/>

<tags 
	ms.service="active-directory" 
	ms.date="10/09/2015" 
	wacn.date=""/>

# Dedicated groups in Azure Active Directory

In Azure Active Directory, dedicated groups are created automatically and group membership for the dedicated groups is also automatic. You cannot add or remove members to and from dedicated groups through the Azure Management portal, Windows PowerShell cmdlets, or programmatically. To enable dedicated groups, In the Azure Management Portal, on the Configure tab, set the Enable Dedicated Groups switch to Yes.

In the current public preview release, once the Enable Dedicated Groups switch is set to Yes, you can further enable the directory to automatically create the All Users dedicated group by setting the Enable “All Users” Group switch to Yes. You can then also edit the name of this dedicated group by typing it in the Display Name for “All Users” Group field.

The All Users dedicated group can be useful if you want to assign the same permissions to all the users in your directory. For example, you can grant all users in your directory access to a SaaS application by assigning access for the All Users dedicated group to this application.

Here are some topics that will provide some additional information on Azure Active Directory 

* [Managing access to resources with Azure Active Directory groups](/documentation/articles/active-directory-manage-groups)

* [What is Azure Active Directory?](/documentation/articles/active-directory-whatis)

* [Integrating your on-premises identities with Azure Active Directory](/documentation/articles/active-directory-aadconnect)

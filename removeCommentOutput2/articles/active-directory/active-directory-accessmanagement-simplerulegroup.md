<properties
	pageTitle="Creating a simple rule to configure dynamic memberships for a group| Windows Azure"
	description="Explains how to create a simple rule to configure dynamic memberships for a group."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.date="11/17/2015"
	wacn.date=""/>


# Creating a simple rule to configure dynamic memberships for a group

**To enable dynamic membership for a particular group, perform the following steps:** 

1. In the Azure Management Portal, under the **Groups** tab, select the group you want to edit, and then in this groupâs **Configure** tab, set the **Enable Dynamic Memberships** switch to **Yes**.


2. You can now set up a simple single rule for the group that will control how dynamic membership for this group functions. Make sure the **Add users where** radio button is selected and then select a user property from the pull-down menu (for example, department, jobTitle, etc.), 

3. Next, select a condition (Not Equals, Equals, Not Starts With, Starts With, Not Contains, Contains, Not Match, Match), and finally specify a value for the selected user property. For example, if a group is assigned to a SaaS application and you enable dynamic memberships for this group by setting a rule whereby **Add users where** is set to the jobTitle that Equals(-eq)Sales Rep, all users within your Azure AD directory whose job titles are set to Sales Rep, will have access to this SaaS application.

Here are some topics that will provide some additional information on Azure Active Directory 

* [Managing access to resources with Azure Active Directory groups](/documentation/articles/active-directory-manage-groups)
* [What is Azure Active Directory?](/documentation/articles/active-directory-whatis)
* [Integrating your on-premises identities with Azure Active Directory](/documentation/articles/active-directory-aadconnect)

<properties
	pageTitle="Assigning administrator roles in Azure Active Directory | Windows Azure"
	description="Explains what administrator roles are available with Azure Active Directory and how to assign them."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.date="12/01/2015"
	wacn.date=""/>

# Assigning administrator roles in Azure Active Directory (Azure AD)

Depending on the size of your company, you may want to designate several administrators who serve different functions. These administrators will have access to various features in the Azure Management Portal or Azure <!-- deleted by customization Management Portal --><!-- keep by customization: begin --> Managemwnt portal <!-- keep by customization: end --> and, depending on their role, will be able to create or edit users, assign administrative roles to others, reset user passwords, manage user licenses, and manage domains, among other things.

It's  important to understand that a user who is assigned an admin role will have the same permissions across all of the cloud services that your organization has subscribed to, regardless of whether you assign the role in the Office 365 portal, or in the Azure Management Portal, or by using the Azure AD module for Windows PowerShell.

The following administrator roles are available:

- **Billing administrator**: Makes purchases, manages subscriptions, manages support tickets, and monitors service health.
- **Global administrator**: Has access to all administrative features. The person who signs up for the Azure account becomes a global administrator. Only global administrators can assign other administrator roles. There can be more than one global administrator at your company.
- **Password administrator**: Resets passwords, manages service requests, and monitors service health. Password administrators can reset passwords only for users and other password administrators.
- **Service administrator**: Manages service requests and monitors service health.
> [AZURE.NOTE]
> To assign the service administrator role to a user, the global administrator must first assign administrative permissions to the user in the service, such as Exchange Online, and then assign the service administrator role to the user in the Azure Management Portal.
- **User administrator**: Resets passwords, monitors service health, and manages user accounts, user groups, and service requests. Some limitations apply to the permissions of a user management administrator. For example, they cannot delete a global administrator or create other administrators. Also, they cannot reset passwords for billing, global, and service administrators.

## Administrator permissions

### Billing administrator

Can do | Cannot do
------------- | -------------
<p>View company and user information</p><p>Manage Office support tickets</p><p>Perform billing and purchasing operations for Office products</p> | <p>Reset user passwords</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p>

### Global administrator

Can do | Cannot do
------------- | -------------
<p>View company and user information</p><p>Manage Office support tickets</p><p>Perform billing and purchasing operations for Office products</p> <p>Reset user passwords</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p><p>Enable or disable multi-factor authentication</p> | N/A

### Password administrator

Can do | Cannot do
------------- | -------------
<p>View company and user information</p><p>Manage Office support tickets</p><p>Reset user passwords</p> | <p>Perform billing and purchasing operations for Office products</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p>

### Service administrator

Can do | Cannot do
------------- | -------------
<p>View company and user information</p><p>Manage Office support tickets</p> | <p>Reset user passwords</p><p>Perform billing and purchasing operations for Office products</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p>

### User administrator

Can do | Cannot do
------------- | -------------
<p>View company and user information</p><p>Manage Office support tickets</p><p>Reset user passwords, with limitations. He or she cannot reset passwords for billing, global, and service administrators.</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses, with limitations. He or she cannot delete a global administrator or create other administrators.</p> | <p>Perform billing and purchasing operations for Office products</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p><p>Enable or disable multi-factor authentication</p>

## Details about the global administrator role

The global administrator has access to all administrative features. By default, the person who signs up for an Azure subscription is assigned  the global administrator role for the directory. Only global administrators can assign other administrator roles.

## Assign or remove administrator roles


1. In the <!-- deleted by customization Azure --> Management Portal, click **Active Directory**, and then click the name of your organizationâs directory.
2. On the **Users** page, click the display name of the user you want to edit.
3. In the **Organizational Role** list, select the administrator role that you want to assign to this user, or select **User** if you want to remove an existing administrator role.
4. In the **Alternate Email Address** box, type an email address. This email address is used for important notifications, including password self-reset, so the user must be able to access the email account whether or not the user can access Azure.
5. Select **Allow** or **Block** to specify whether to allow the user to sign in and access services.
6. Specify a location from the **Usage Location** drop-down list.
7. When you have finished, click **Save**.

## What's next

- [Manage users](/documentation/articles/active-directory-manage-users)
- [Manage passwords](/documentation/articles/active-directory-manage-passwords)
- [Manage groups](/documentation/articles/active-directory-manage-groups)

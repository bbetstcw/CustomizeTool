<properties
   pageTitle="Azure Active Directory business-to-business (B2B) collaboration"
   description="Azure Active Directory B2B collaboration enables business partners to access your corporate applications, with each of their users represented by a single Azure AD account"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="msStevenPo"
   editor=""/>

<tags
	ms.service="active-directory"
	ms.date="09/17/2015"
	wacn.date=""/>

# Azure Active Directory B2B collaboration

Azure Active Directory B2B collaboration lets you enable access to your corporate applications from partner-managed identities. You can create cross-company relationships by inviting and authorizing users from partner companies to access your resources. Complexity is reduced because each company federates once with Azure Active Directory (Azure AD) and each user is represented by a single Azure AD account. Security is increased because access is revoked when partner users are terminated from their organizations, and unintended access via membership in internal directories is prevented. For business partners who don't already have Azure AD, B2B collaboration has a streamlined sign-up experience to provide Azure AD accounts to your business partners.

-   Your business partners use their own sign-in credentials, which frees you from managing an external partner directory, and from the need to remove access when users leave the partner organization.

-   You manage access to your apps independently of your business partner's account lifecycle. This means, for example, that you can revoke access without having to ask the IT department of your business partner to do anything.

## Capabilities

B2B collaboration simplifies management and improves security of partner access to corporate resources including SaaS apps such as Office 365, Salesforce, Azure Services, and every mobile, cloud and on-premises claims-aware application. B2B collaboration enables partners manage their own accounts and enterprises can apply security policies to partner access.

Azure Active Directory B2B collaboration is easy to configure with simplified sign-up for partners of all sizes even if they don’t have their own Azure Active Directory via an email-verified process. It is also easy to maintain with no external directories or per partner federation configurations.

The process:

1. Azure AD B2B collaboration allows a company administrator to invite and authorize a set of external users by uploading a comma-separated values (CSV) file of no more than 2000 lines to the B2B collaboration portal.

  ![CSV file upload dialog](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. The portal will send email invitations to these external users.

3. The invited user will either sign in to an existing work account with Microsoft (managed in Azure AD), or get a new work account in Azure AD.

4. Once signed in, the user will be redirected to the app that was shared with them.

Invitations to consumer email addresses (for example, gmail or [*comcast.net*](http://comcast.net/)) are not currently supported.

For more on how B2B collaboration works, check out [this video](https://channel9.msdn.com/Series/Azure-AD-Identity/AzureADB2B).

## CSV File Format

The CSV file follows the format below. Add all required commas even if you don't specify one or more options.

**Email:** Email address for invited user.<br/>
**DisplayName:** Display name for invited user (typically, first and last name).<br/>
**InviteAppID:**  The ID for the application to use for branding the email invite and acceptance pages.<br/>
**InviteReplyURL:** URL to which to direct an invited user after invite acceptance. This should be a company-specific URL (such as [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). If this optional field is not specified, the inviting company's Access Panel URL is generated (this URL is of the form  `https://account.activedirectory.windowsazure.cn/applications/default.aspx?tenantId=<TenantID>`).<br/>
**InviteAppResources:** AppIDs to which applications can assign users. AppIDs are retrievable by calling `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InviteGroupResources:** ObjectIDs for groups to add user to. ObjectIDs are retrievable by calling `Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteContactUsUrl:** "Contact Us" URL to include in email invitations in case the invited user wants to contact your organization.<br/>

## Sample CSV file
Here is a sample CSV you can modify for your purposes. Save it to any file name you prefer, but ensure that it has a '.csv' file extension.

```
Email,DisplayName,InviteAppID,InviteReplyUrl,InviteAppResources,InviteGroupResources,InviteContactUsUrl
wharp@contoso.com,Walter Harp,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,/home/features/identity/,,,/home/features/identity/
jsmith@contoso.com,Jeff Smith,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,/home/features/identity/,,,/home/features/identity/
bsmith@contoso.com,Ben Smith,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,/home/features/identity/,,,/home/features/identity/
```

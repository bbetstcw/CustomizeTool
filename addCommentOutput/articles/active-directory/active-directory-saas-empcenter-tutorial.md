<properties 
    pageTitle="Tutorial: Azure Active Directory integration with EmpCenter | Windows Azure" 
    description="Learn how to use EmpCenter with Azure Active Directory to enable single sign-on, automated provisioning, and more!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags
	ms.service="active-directory"
	ms.date="12/18/2015"
	wacn.date=""/>

#Tutorial: Azure Active Directory integration with EmpCenter
<!-- keep by customization: begin -->
>[AZURE.TIP]For feedback, click [here](http://go.microsoft.com/fwlink/?LinkId=615281).
<!-- keep by customization: end -->
<!-- keep by customization: end -->
  
The objective of this tutorial is to show the integration of Azure and EmpCenter.  
The scenario outlined in this tutorial assumes that you already have the following items:

-   A valid Azure subscription
-   An EmpCenter single sign-on enabled subscription
  
After completing this tutorial, the Azure AD users you have assigned to EmpCenter will be able to single sign into the application at your EmpCenter company site (service provider initiated sign on), or using the [Introduction to the Access <!-- keep by customization: begin --><!-- deleted by customization <!-- keep by customization: end --> Panel](/documentation/articles/active-directory-saas-access-panel-introduction). <!-- keep by customization: begin --> --><!-- keep by customization: begin --> Panel](https://msdn.microsoft.com/zh-cn/library/dn308586) <!-- keep by customization: end --><!-- keep by customization: end -->
  
The scenario outlined in this tutorial consists of the following building blocks:

1.  Enabling the application integration for EmpCenter
2.  Configuring single sign-on
3.  Configuring user provisioning
4.  Assigning users

![Scenario](./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenario")
##Enabling the application integration for EmpCenter
  
The objective of this section is to outline how to enable the application integration for EmpCenter.

###To enable the application integration for EmpCenter, perform the following steps:

1.  In the Azure Management Portal, on the left navigation pane, click **Active Directory**.

    ![Active Directory](./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.  From the **Directory** list, select the directory for which you want to enable directory integration.

3.  To open the applications view, in the directory view, click **Applications** in the top menu.

    ![Applications](./media/active-directory-saas-empcenter-tutorial/IC700994.png "Applications")

4.  Click **Add** at the bottom of the page.

    ![Add application](./media/active-directory-saas-empcenter-tutorial/IC749321.png "Add application")

5.  On the **What do you want to do** dialog, click **Add an application from the gallery**.

    ![Add an application from gallerry](./media/active-directory-saas-empcenter-tutorial/IC749322.png "Add an application from gallerry")

6.  In the **search box**, type **EmpCenter**.

    ![Application Gallery](./media/active-directory-saas-empcenter-tutorial/IC802917.png "Application Gallery")

7.  In the results pane, select **EmpCenter**, and then click **Complete** to add the application.

    ![EmpCentral](./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##Configuring single sign-on
  
The objective of this section is to outline how to enable users to authenticate to EmpCenter with their account in Azure AD using federation based on the SAML protocol.

###To configure single sign-on, perform the following steps:

1.  In the Azure AD portal, on the **EmpCenter** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.

    ![Configure Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802919.png "Configure Single Sign-On")

2.  On the **How would you like users to sign on to EmpCenter** page, select **Windows Azure AD Single Sign-On**, and then click **Next**.

    ![Configure Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802920.png "Configure Single Sign-On")

3.  On the **Configure App Settings** page, perform the following steps:

    ![Configure App Settings](./media/active-directory-saas-empcenter-tutorial/IC802921.png "Configure App Settings")

    1.  In the **Sign On URL** textbox, type the URL used by your users to sign-on to your EmpCenter application (e.g.: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Click **Next**

4.  On the **Configure single sign-on at EmpCenter** page, to download your metadata, click **Download metadata**, and then save the metadata file on your computer.

    ![Configure Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802922.png "Configure Single Sign-On")

5.  Send the downloaded metadata file to your EmpCenter support team.

    >[AZURE.NOTE] Your EmpCenter support team has to do the actual SSO configuration.
    You will get a notification when SSO has been enabled for your subscription.

6.  On the Azure AD portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.

    ![Configure Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802923.png "Configure Single Sign-On")
##Configuring user provisioning
  
In order to enable Azure AD users to log into EmpCenter, they must be provisioned into EmpCenter.  
In the case of EmpCenter, the user accounts need to be created by your EmpCenter support team.

>[AZURE.NOTE] You can use any other EmpCenter user account creation tools or APIs provided by EmpCenter to provision Azure Active Directory user accounts.

##Assigning users
  
To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.

###To assign users to EmpCenter, perform the following steps:

1.  In the Azure AD portal, create a test account.

2.  On the **EmpCenter **application integration page, click **Assign users**.

    ![Assign Users](./media/active-directory-saas-empcenter-tutorial/IC802924.png "Assign Users")

3.  Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.

    ![Yes](./media/active-directory-saas-empcenter-tutorial/IC767830.png "Yes")
  
If you want to test your single sign-on settings, open the Access Panel. For more details about the Access Panel, see [Introduction to the Access Panel](/documentation/articles/active-directory-saas-access-panel-introduction).
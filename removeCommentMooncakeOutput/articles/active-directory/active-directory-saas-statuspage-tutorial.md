<properties
	pageTitle="Tutorial: Azure Active Directory integration with StatusPage | Windows Azure"
	description="Learn how to configure single sign-on between Azure Active Directory and StatusPage."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="prasannas"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.date="12/01/2015"
	wacn.date=""/>


# Tutorial: Azure Active Directory integration with StatusPage

The objective of this tutorial is to show you how to integrate StatusPage with Azure Active Directory (Azure AD).<br>Integrating StatusPage with Azure AD provides you with the following benefits: 

- You can control in Azure AD who has access to StatusPage 
- You can enable your users to automatically get signed-on to StatusPage (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure Active Directory Portal

If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](/documentation/articles/active-directory-appssoaccess-whatis).

## Prerequisites 

To configure Azure AD integration with StatusPage, you need the following items:

- An Azure AD subscription
- A StatusPage single-sign on enabled subscription


> [AZURE.NOTE] To test the steps in this tutorial, we do not recommend using a production environment.


To test the steps in this tutorial, you should follow these recommendations:

- You should not use your production environment, unless this is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](/pricing/1rmb-trial/). 

 
## Scenario Description
The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment. <br>
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding StatusPage from the gallery 
2. Configuring and testing Azure AD single sign-on


## Adding StatusPage from the gallery
To configure the integration of StatusPage into Azure AD, you need to add StatusPage from the gallery to your list of managed SaaS apps.

**To add StatusPage from the gallery, perform the following steps:**

1. In the **Azure Management Portal**, on the left navigation pane, click **Active Directory**. <br><br>
![Active Directory][1]<br>

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To open the applications view, in the directory view, click **Applications** in the top menu.<br><br>
![Applications][2]<br>
4. Click **Add** at the bottom of the page.<br><br>
![Applications][3]<br>
5. On the **What do you want to do** dialog, click **Add an application from the gallery**.<br><br>
![Applications][4]<br>
6. In the search box, type **StatusPage**.<br><br>
![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_01.png)<br>
7. In the results pane, select **StatusPage**, and then click **Complete** to add the application.
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_02.png)<br>


##  Configuring and testing Azure AD single sign-on
The objective of this section is to show you how to configure and test Azure AD single sign-on with StatusPage based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in StatusPage to an user in Azure AD is. In other words, a link relationship between an Azure AD user and the related user in StatusPage needs to be established.<br>
This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in StatusPage.
 
To configure and test Azure AD single sign-on with StatusPage, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
4. **[Creating a StatusPage test user](#creating-a-statuspage-test-user)** - to have a counterpart of Britta Simon in StatusPage that is linked to the Azure AD representation of her.
5. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD Single Sign-On

The objective of this section is to enable Azure AD single sign-on in the Azure AD portal and to configure single sign-on in your StatusPage application. 



**To configure Azure AD single sign-on with StatusPage, perform the following steps:**

1. In the Azure AD portal, on the **StatusPage** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.
<br><br> ![Configure Single Sign-On][6] <br>

2. On the **How would you like users to sign on to StatusPage** page, select **Azure AD Single Sign-On**, and then click **Next**.
<br><br> ![Configure Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_03.png) <br>

3. On the **Configure App Settings** dialog page, perform the following steps:.
<br><br>![Configure Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_04.png) <br>

    > [AZURE.NOTE] Contact the StatusPage support team at [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io)to request metadata necessary to configure single sign-on.


    a. From the metadata, copy the Issuer value, and then paste it into the **Identifier** textbox.

    b. From the metadata, copy the Reply URL, and then paste it into the **Reply URL** textbox.

    c. Click **Next**.
 
 
4. On the **Configure single sign-on at StatusPage** page, perform the following steps:
<br><br>![Configure Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_05.png) <br>

    a. Click **Download certificate**, and then save the file on your computer.

    b. Click **Next**.


1. In another browser window, sign on to your StatusPage company site as an administrator.

1. In the main toolbar, click **Manage Account**.
<br><br> ![Configure Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) <br>


1. Click the **Single Sign-on** tab. 
<br><br> ![Configure Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) <br>


1. On the SSO Setup page, perform the following steps:
<br><br>![Configure Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) <br>

    a. In the Azure Management Portal, on the **Configure single sign-on at StatusPage** dialog page, copy the **Single Sign-On Service URL** value, and then paste it into the **SSO Target URL** textbox. 

    b. Open your downloaded certificate in Notepad, copy the content, and then paste it into the **Certificate** textbox. 

    c. Click **Save**.


6. In the Azure AD portal, select the single sign-on configuration confirmation, and then click **Next**. 
<br><br>![Azure AD Single Sign-On][10]<br>

7. On the **Single sign-on confirmation** page, click **Complete**.  
  <br><br>![Azure AD Single Sign-On][11]




### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure Management Portal called Britta Simon.<br>
In the Users list, select **Britta Simon**.<br><br>![Create Azure AD User][20]<br>

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure Management Portal**, on the left navigation pane, click **Active Directory**.
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/create_aaduser_09.png) <br> 

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To display the list of users, in the menu on the top, click **Users**.
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) <br>
 
4. To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**. 
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) <br>

5. On the **Tell us about this user** dialog page, perform the following steps: 
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/create_aaduser_05.png) <br> 

    a. As Type Of User, select New user in your organization.

    b. In the User Name **textbox**, type **BrittaSimon**.

    c. Click **Next**.

6.  On the **User Profile** dialog page, perform the following steps: 
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/create_aaduser_06.png) <br>
 
    a. In the **First Name** textbox, type **Britta**.  

    b. In the **Last Name** textbox, type, **Simon**.

    c. In the **Display Name** textbox, type **Britta Simon**.

    d. In the **Role** list, select **User**.
    e. Click **Next**.

7. On the **Get temporary password** dialog page, click **create**.
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/create_aaduser_07.png) <br>
 
8. On the **Get temporary password** dialog page, perform the following steps:
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/create_aaduser_08.png) <br>
  
    a. Write down the value of the **New Password**.

    b. Click **Complete**.   

  
 
### Creating a StatusPage test user

The objective of this section is to create a user called Britta Simon in StatusPage.
StatusPage supports just-in-time provisioning. You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on).


**To create a user called Britta Simon in StatusPage, perform the following steps:**

1. Sign-on to your StatusPage company site as an administrator.

1. In the menu on the top, click **Manage Account**.

1. Click the Team Members tab. <br><br>![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) <br>

1. Click **Add Team Member**. <br><br>![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) <br>

1. Type the **Email Address**, **First Name** and **Sur Name** of a valid user you want to provision into the related textboxes. <br><br>![Creating an Azure AD test user](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) <br>

1. As **Role**, choose **Client Administrator**.

1. Click **Create Account**.

### Assigning the Azure AD test user

The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to StatusPage.
<br><br>![Assign User][200] <br>

**To assign Britta Simon to StatusPage, perform the following steps:**

1. On the Azure Management Portal, to open the applications view, in the directory view, click **Applications** in the top menu.
<br><br>![Assign User][201] <br>

2. In the applications list, select **StatusPage**.
<br><br>![Configure Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_50.png) <br>

1. In the menu on the top, click **Users**.
<br><br>![Assign User][203] <br>

1. In the Users list, select **Britta Simon**.

2. In the toolbar on the bottom, click **Assign**.
<br><br>![Assign User][205]



### Testing Single Sign-On

The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.<br>
When you click the StatusPage tile in the Access Panel, you should get automatically signed-on to your StatusPage application.


## Additional Resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](/documentation/articles/active-directory-saas-tutorial-list)
* [What is application access and single sign-on with Azure Active Directory?](/documentation/articles/active-directory-appssoaccess-whatis)


<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_205.png






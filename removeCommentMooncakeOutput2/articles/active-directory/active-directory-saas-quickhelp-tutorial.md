<properties
	pageTitle="Tutorial: Azure Active Directory integration with QuickHelp | Windows Azure"
	description="Learn how to configure single sign-on between Azure Active Directory and QuickHelp."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.date="10/16/2015"
	wacn.date=""/>


# Tutorial: Azure Active Directory integration with QuickHelp

The objective of this tutorial is to show you how to integrate QuickHelp with Azure Active Directory (Azure AD).<br>Integrating QuickHelp with Azure AD provides you with the following benefits: 

- You can control in Azure AD who has access to QuickHelp 
- You can enable your users to automatically get signed-on to QuickHelp (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure Active Directory Portal

If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](/documentation/articles/active-directory-appssoaccess-whatis).

## Prerequisites 

To configure Azure AD integration with QuickHelp, you need the following items:

- An Azure AD subscription
- A QuickHelp single-sign on enabled subscription


> [AZURE.NOTE] To test the steps in this tutorial, we do not recommend using a production environment.


To test the steps in this tutorial, you should follow these recommendations:

- You should not use your production environment, unless this is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](/pricing/1rmb-trial/). 

 
## Scenario Description
The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment. <br>
The scenario outlined in this tutorial consists of three main building blocks:

1. Adding QuickHelp from the gallery 
2. Configuring and testing Azure AD single sign-on


## Adding QuickHelp from the gallery
To configure the integration of QuickHelp into Azure AD, you need to add QuickHelp from the gallery to your list of managed SaaS apps.

**To add QuickHelp from the gallery, perform the following steps:**

1. In the **Azure Management Portal**, on the left navigation pane, click **Active Directory**. 
<br><br>![Active Directory][1]<br>

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To open the applications view, in the directory view, click **Applications** in the top menu.<br><br>
![Applications][2]<br>
4. Click **Add** at the bottom of the page.<br><br>
![Applications][3]<br>
5. On the **What do you want to do** dialog, click **Add an application from the gallery**.<br><br>
![Applications][4]<br>
6. In the search box, type **QuickHelp**.<br><br>
![Applications][5]<br>
7. In the results pane, select **QuickHelp**, and then click **Complete** to add the application.
<br><br>![Applications][500]<br>


##  Configuring and testing Azure AD single sign-on
The objective of this section is to show you how to configure and test Azure AD single sign-on with QuickHelp based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in QuickHelp to an user in Azure AD is. In other words, a link relationship between an Azure AD user and the related user in QuickHelp needs to be established.<br>
This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in QuickHelp.
 
To configure and test Azure AD single sign-on with QuickHelp, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
4. **[Creating a QuickHelp test user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in QuickHelp that is linked to the Azure AD representation of her.
5. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD Single Sign-On

The objective of this section is to enable Azure AD single sign-on in the Azure AD portal and to configure single sign-on in your QuickHelp application.<br>

**To configure Azure AD single sign-on with QuickHelp, perform the following steps:**

1. In the Azure AD portal, on the **QuickHelp** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.
<br><br> ![Configure Single Sign-On][6] <br>

2. On the **How would you like users to sign on to QuickHelp** page, select **Azure AD Single Sign-On**, and then click **Next**.
<br><br> ![Azure AD Single Sign-On][7] <br>

3. On the **Configure App Settings** dialog page, perform the following steps:
<br><br>![Configure App Settings][8] <br>
 
     a. In the **Sign On URL** textbox, type the URL used by your users to sign-on to your QuickHelp site (e.g.:* https://quickhelp.com/bsiazure/#/home/assignedContent*).

     > [AZURE.NOTE] Please contact your QuickHelp support team if you don't know the value of the Sign On URL.

     b. Click **Next**.

4. Download the **QuickHelp** metadata file and saved it on your computer: [https://quickhelp.blob.core.chinacloudapi.cn/metadata/QuickhelpSamlMetadataBS.xml](https://quickhelp.blob.core.chinacloudapi.cn/metadata/QuickhelpSamlMetadataBS.xml).
 
4. On the **Configure single sign-on at QuickHelp** page, perform the following steps:click **Download metadata**, and then save the metadata file locally on your computer.
<br><br>![What is Azure AD Connect][9] <br>



1. Sign-on to your QuickHelp company site as administrator.

2. In the menu on the top, click **Admin**.
<br><br>![Configure Single Sign-On][21]<br>


1. In the **QuickHelp Admin** menu, click **Settings**.
<br><br>![Configure Single Sign-On][22]<br>

1. Click **Authentication Settings**.

1. On the **Authentication Settings** page, perform the following steps
<br><br>![Configure Single Sign-On][23]<br>

    a. As **SSO Type**, select **WSFederation**.

    b. To upload your downloaded Azure metadata file, click **Browse**, navigate to the file, end then click **Upload Metadata**.

    d. In the **Email** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

6. On the Azure AD portal, select the single sign-on configuration confirmation, and then click **Next**. 
<br><br>![What is Azure AD Connect][10]<br>

7. On the **Single sign-on confirmation** page, click **Complete**.  
  <br><br>![What is Azure AD Connect][11]




### Creating an Azure AD test user
The objective of this section is to create a test user in the Azure Management Portal called Britta Simon.<br>
In the Users list, select **Britta Simon**.<br><br>![Create Azure AD User][20]<br>

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure Management Portal**, on the left navigation pane, click **Active Directory**.<br><br>
![Creating an Azure AD test user](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png)<br> 

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To display the list of users, in the menu on the top, click **Users**.<br><br>![Creating an Azure AD test user](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) <br>
 
4. To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**. <br><br>![Creating an Azure AD test user](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png)<br> 

5. On the **Tell us about this user** dialog page, perform the following steps: <br><br>![Creating an Azure AD test user](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_05.png) <br>

    a. As Type Of User, select New user in your organization.

    b. In the User Name **textbox**, type **BrittaSimon**.

    c. Click **Next**.

6.  On the **User Profile** dialog page, perform the following steps: 
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_06.png) <br>
 
    a. In the **First Name** textbox, type **Britta**.  

    b. In the **Last Name** textbox, type, **Simon**.

    c. In the **Display Name** textbox, type **Britta Simon**.

    d. In the **Role** list, select **User**.
    e. Click **Next**.

7. On the **Get temporary password** dialog page, click **create**.
<br><br> ![Creating an Azure AD test user](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_07.png) <br>
 
8. On the **Get temporary password** dialog page, perform the following steps:
<br><br>![Creating an Azure AD test user](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_08.png) <br>
  
    a. Write down the value of the **New Password**.

    b. Click **Complete**.   

  
 
### Creating a QuickHelp test user

The objective of this section is to create a user called Britta Simon in QuickHelp.

In this tutorial, new users are imported from a CSV file with the following structure:

|FirstName|LastName|Email|Department|Title|
|---|---|---|---|---|
|Britta|Simon|BritaSimon@Fabrikam.com|||

<br><br>![Create a QuickHelp test user][26]<br>

You need to create a CSV File with this structure that has as values the values of **Britta Simon** in your Azure Active Directory test environment. 



**To create a user called Britta Simon in QuickHelp, perform the following steps:**

1. Create a CSV file following the instructions above. 
 
2. Sign on to your QuickHelp company site as administrator.
   <br><br>![Create a QuickHelp test user][21]<br>


3. In the **QuickHelp Admin** menu, click **Users**, and then **New**.
<br><br>![Create a QuickHelp test user][24]<br>


4. As **Content**, select **User**, and then click **Import**. 
<br><br>![Create a QuickHelp test user][25]<br>

5. To import your CSV file, click **Browse**, navigate to your file, and then click **Next**. 
<br><br>![Create a QuickHelp test user][26]<br>

6. On the summary page, review the status, and then click **Finish**. 
<br><br>![Create a QuickHelp test user][27]<br>


If Britta was successfully imported, you can see her in the list of users. 
<br><br>![Create a QuickHelp test user][28]<br>



### Assigning the Azure AD test user

The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to QuickHelp.
<br><br>![Assign User][200] <br>

**To assign Britta Simon to QuickHelp, perform the following steps:**

1. On the Azure Management Portal, to open the applications view, in the directory view, click **Applications** in the top menu.
<br><br>![Assign User][201] <br>

2. In the applications list, select **QuickHelp**.
<br><br>![Assign User][202] <br>

1. In the menu on the top, click **Users**.
<br><br>![Assign User][203] <br>

1. In the Users list, select **Britta Simon**.

2. In the toolbar on the bottom, click **Assign**.
<br><br>![Assign User][205]



### Testing Single Sign-On

The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.<br>
When you click the QuickHelp tile in the Access Panel, you should get automatically signed-on to your QuickHelp application.


## Additional Resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](/documentation/articles/active-directory-saas-tutorial-list)
* [What is application access and single sign-on with Azure Active Directory?](/documentation/articles/active-directory-appssoaccess-whatis)


<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_01.png
[500]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_14.png


[6]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_02.png
[8]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_03.png
[9]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_04.png
[10]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
[24]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_08.png
[25]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_09.png
[26]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_10.png
[27]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_11.png
[28]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_12.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_13.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_400.png
[401]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_401.png
[402]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_402.png





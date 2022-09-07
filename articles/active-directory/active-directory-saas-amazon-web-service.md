<properties
	pageTitle="Tutorial: Azure Active Directory integration with Amazon Web Service (AWS) | Microsoft Azure"
	description="Learn how to configure single sign-on between Azure Active Directory and Amazon Web Service (AWS)."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/27/2015"
	ms.author="markvi"/>


# Tutorial: Azure Active Directory integration with Amazon Web Service (AWS)

The objective of this tutorial is to show you how to integrate Amazon Web Service (AWS) with Azure Active Directory (Azure AD).<br>Integrating Amazon Web Service (AWS) with Azure AD provides you with the following benefits: 

- You can control in Azure AD who has access to Amazon Web Service (AWS) 
- You can enable your users to automatically get signed-on to Amazon Web Service (AWS) (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure Active Directory Portal

If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisites 

To configure Azure AD integration with Amazon Web Service (AWS), you need the following items:

- An Azure AD subscription
- An Amazon Web Service (AWS) single-sign on enabled subscription


> [AZURE.NOTE] To test the steps in this tutorial, we do not recommend using a production environment.


To test the steps in this tutorial, you should follow these recommendations:

- You should not use your production environment, unless this is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/). 

 
## Scenario Description
The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment. <br>
The scenario outlined in this tutorial consists of three main building blocks:

1. Adding Amazon Web Service (AWS) from the gallery 
2. Configuring and testing Azure AD single sign-on


## Adding Amazon Web Service (AWS) from the gallery
To configure the integration of Amazon Web Service (AWS) into Azure AD, you need to add Amazon Web Service (AWS) from the gallery to your list of managed SaaS apps.

### To add Amazon Web Service (AWS) from the gallery, perform the following steps:

1. In the **Azure Management Portal**, on the left navigation pane, click **Active Directory**. <br><br>
![Active Directory][1]

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To open the applications view, in the directory view, click **Applications** in the top menu.<br><br>
![Applications][2]
4. Click **Add** at the bottom of the page.<br><br>
![Applications][3]
5. On the **What do you want to do** dialog, click **Add an application from the gallery**.<br><br>
![Applications][4]
6. In the search box, type **Amazon Web Service (AWS)**.<br><br>
![Applications][5]
7. In the results pane, select **Amazon Web Service (AWS)**, and then click **Complete** to add the application.<br><br>
![Applications][6]



##  Configuring and testing Azure AD single sign-on
The objective of this section is to show you how to configure and test Azure AD single sign-on with Amazon Web Service (AWS) based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Amazon Web Service (AWS) to an user in Azure AD is. In other words, a link relationship between an Azure AD user and the related user in Amazon Web Service (AWS) needs to be established.<br>
This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Amazon Web Service (AWS).
 
To configure and test Azure AD single sign-on with Amazon Web Service (AWS), you need to complete the following building blocks:

1. **[Configuring Azure AD Single Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
4. **[Creating a Amazon Web Service (AWS) test user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in Amazon Web Service (AWS) that is linked to the Azure AD representation of her.
5. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD Single Single Sign-On

The objective of this section is to enable Azure AD single sign-on in the Azure AD portal and to configure single sign-on in your Amazon Web Service (AWS) application.<br>
Your Amazon Web Service (AWS) application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **saml token attributes** configuration. 
The following screenshot shows an example for this.
<br><br> ![Configure Single Sign-On][27]

**To configure Azure AD single sign-on with Amazon Web Service (AWS), perform the following steps:**

1. In the Azure AD portal, on the **Amazon Web Service (AWS)** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.<br><br>
![Configure Single Sign-On][7]

2. On the **How would you like users to sign on to Amazon Web Service (AWS)** page, select **Azure AD Single Sign-On**, and then click **Next**.<br><br>
![Configure Single Sign-On][8]

3. On the **Configure App Settings** dialog page, click Next. <br><br>![Configure App Settings][9]
 
4. On the **Configure single sign-on at Amazon Web Service (AWS)** page, click **Download metadata**, and then save the metadata file locally on your computer.<br><br>![Configure Single Sign-On][10]

5. In a different browser window, sign-on to your Amazon Web Service (AWS) company site as administrator.

6. Click **Console Home**. <br><br> ![Configure Single Sign-On][11]

7. Click **Identity and Access Management**. <br><br> ![Configure Single Sign-On][12]

8. Click **Identity Providers**, and then click **Create Provider**. <br><br> ![Configure Single Sign-On][13]

9. On the **Configure Provider** dialog page, perform the following steps: <br><br> ![Configure Single Sign-On][14]

     9.1. As **Provider Type**, select **SAML**.

     9.2. In the **Provider Name** textbox, type a provider name (e.g.: *WAAD*).

     9.3. To upload your downloaded metadata file, click **Choose File**.

     9.4. Click **Next Step**.


10. On the **Verify Provider Information** dialog page, click **Create**. <br><br> ![Configure Single Sign-On][15]

11. Go to **Roles > Create New Role**. <br><br> ![Configure Single Sign-On][16]

12. On the **Set Role Name** dialog, perform the following steps: <br><br> ![Configure Single Sign-On][17]
     12.1. In the **Role Name** textbox, type a role name (e.g.: *TestUser*).

     12.2. Click **Next Step**.

13. On the **Select Role Type** dialog, perform the following steps: <br><br> ![Configure Single Sign-On][18]

     13.1. Select **Role For Identity Provider Access**.

     13.2. In the **Grant Web Single Sign-On (WebSSO) access to SAML providers** section, click **Select**.


14. On the Establish Trust dialog, perform the following steps:  <br><br> ![Configure Single Sign-On][19]
     14.1. As SAML provider, select the SAML provider you have created previousley (e.g.: *WAAD*) 



15. On the Azure AD portal, select the single sign-on configuration confirmation, and then click **Next**.<br><br>![What is Azure AD Connect][20]

16. On the **Single sign-on confirmation** page, click **Complete** to close the **Configure single sign-on** dialog.<br><br>![What is Azure AD Connect][22]


17. In the menu on the top, click **Attributes** to open the **SAML Token Attributes** dialog. <br><br> ![Configure Single Sign-On][21]

18. Click **add user attribute**. <br><br> ![Configure Single Sign-On][23]

19. On the Add User Attribute dialog, perform the following steps. <br><br> ![Configure Single Sign-On][24] 

     19.1. In the **Attribute Name** textbox, type **https://aws.amazon.com/SAML/Attributes/Role**.

     19.2. In the **Attribute Value** textbox, type **arn:aws:iam::214510765665:role/Admin,arn:aws:iam::214510765665:saml-provider/WAAD**.

     19.3. Click **Complete** to close the **Add User Attribute** dialog.

20. Click **add user attribute**. <br><br> ![Configure Single Sign-On][23]


21. On the Add User Attribute dialog, perform the following steps. <br><br> ![Configure Single Sign-On][25] 

     21.1. In the **Attribute Name** textbox, type **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     21.2. In the **Attribute Value** list, select **user:mail**.

     21.3. Click **Complete** to close the **Add User Attribute** dialog.


22. Click **Apply Changes**. <br><br> ![Configure Single Sign-On][26]





### Creating an Azure AD test user

The objective of this section is to create a test user in the Azure portal called Britta Simon.<br>
In the Users list, select **Britta Simon**.<br>![Creating an Azure AD test user](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure Management Portal**, on the left navigation pane, click **Active Directory**.<br>
![Creating an Azure AD test user](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To display the list of users, in the menu on the top, click **Users**.<br>![Creating an Azure AD test user](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**. <br>![Creating an Azure AD test user](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. On the **Tell us about this user** dialog page, perform the following steps: <br>![Creating an Azure AD test user](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 
  1. As Type Of User, select New user in your organization.
  2. In the User Name **textbox**, type **BrittaSimon**.
  3. Click Next.

6.  On the **User Profile** dialog page, perform the following steps: <br>![Creating an Azure AD test user](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 
  1. In the **First Name** textbox, type **Britta**.  
  2. In the **Last Name** txtbox, type, **Simon**.
  3. In the **Display Name** textbox, type **Britta Simon**.
  4. In the **Role** list, select **User**.
  5. Click **Next**.

7. On the **Get temporary password** dialog page, click **create**.<br>![Creating an Azure AD test user](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. On the **Get temporary password** dialog page, perform the following steps:<br>![Creating an Azure AD test user](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 
  1. Write down the value of the **New Password**.
  2. Click **Complete**.   
  
 
### Creating a Amazon Web Service (AWS) test user

The objective of this section is to create a user called Britta Simon in Amazon Web Service (AWS).

### To create a user called Britta Simon in Amazon Web Service (AWS), perform the following steps:

1. Log in to your **Amazon Web Service (AWS)** company site as administrator.

2. Click the **Console Home** icon. <br><br> ![Configure Single Sign-On][11]

3. Click Identity and Access Management. <br><br> ![Configure Single Sign-On][28]

4. In the Dashboard, click Users, and then click Create New Users. <br><br> ![Configure Single Sign-On][29]

5. On the Create User dialog, perform the following steps: <br><br> ![Configure Single Sign-On][30]

     5.1. In the **Enter User Names** textboxes, type Brita Simon's user name in Azure AD.

     5.2. Click **Create**.




### Assigning the Azure AD test user

The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to Amazon Web Service (AWS).
<br><br>![Assign User][31]

**To assign Britta Simon to CloudPassage, perform the following steps:**

1. On the Azure portal, to open the applications view, in the directory view, click **Applications** in the top menu.<br>
<br><br>![Assign User][26]
2. In the applications list, select **Amazon Web Service (AWS)**.
<br><br>![Assign User][27]
1. In the menu on the top, click **Users**.<br>
<br><br>![Assign User][25]
1. In the Users list, select **Britta Simon**.

2. In the toolbar on the bottom, click **Assign**.
<br><br>![Assign User][29]

### Testing Single Sign-On

The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.<br>
When you click the Amazon Web Service (AWS) tile in the Access Panel, you should get automatically signed-on to your Amazon Web Service (AWS) application.


## Additional Resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png


























test

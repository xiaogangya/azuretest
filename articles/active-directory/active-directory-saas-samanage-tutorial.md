<properties pageTitle="Tutorial: Azure Active Directory Integration with Samanage | Microsoft Azure" description="Learn how to use Samanage with Azure Active Directory to enable single sign-on, automated provisioning, and more!." services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Tutorial: Azure Active Directory Integration with Samanage
>[AZURE.TIP]For feedback, click [here](http://go.microsoft.com/fwlink/?LinkId=522516).
  
The objective of this tutorial is to show the integration of Azure and Samanage.  
The scenario outlined in this tutorial assumes that you already have the following items:

-   A valid Azure subscription
-   A Samanage tenant
  
After completing this tutorial, the Azure AD users you have assigned to Samanage will be able to single sign into the application at your Samanage company site (service provider initiated sign on), or using the [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586)
  
The scenario outlined in this tutorial consists of the following building blocks:

1.  Enabling the application integration for Samanage
2.  Configuring single sign-on
3.  Configuring user provisioning
4.  Assigning users

![Scenario](./media/active-directory-saas-samanage-tutorial/IC771705.png "Scenario")
##Enabling the application integration for Samanage
  
The objective of this section is to outline how to enable the application integration for Samanage.

###To enable the application integration for Samanage, perform the following steps:

1.  In the Azure Management Portal, on the left navigation pane, click **Active Directory**.

    ![Active Directory](./media/active-directory-saas-samanage-tutorial/IC700993.png "Active Directory")

2.  From the **Directory** list, select the directory for which you want to enable directory integration.

3.  To open the applications view, in the directory view, click **Applications** in the top menu.

    ![Applications](./media/active-directory-saas-samanage-tutorial/IC700994.png "Applications")

4.  Click **Add** at the bottom of the page.

    ![Add application](./media/active-directory-saas-samanage-tutorial/IC749321.png "Add application")

5.  On the **What do you want to do** dialog, click **Add an application from the gallery**.

    ![Add an application from gallerry](./media/active-directory-saas-samanage-tutorial/IC749322.png "Add an application from gallerry")

6.  In the **search box**, type **Samanage**.

    ![Application gallery](./media/active-directory-saas-samanage-tutorial/IC771707.png "Application gallery")

7.  In the results pane, select **Samanage**, and then click **Complete** to add the application.

    ![Samanage](./media/active-directory-saas-samanage-tutorial/IC771708.png "Samanage")
##Configuring single sign-on
  
The objective of this section is to outline how to enable users to authenticate to Samanage with their account in Azure AD using federation based on the SAML protocol.  
As part of this procedure, you are required to create a base-64 encoded certificate file.  
If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).

###To configure single sign-on, perform the following steps:

1.  In the Azure AD portal, on the **Samanage** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.

    ![Configure single sign-on](./media/active-directory-saas-samanage-tutorial/IC771709.png "Configure single sign-on")

2.  On the **How would you like users to sign on to Samanage** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.

    ![Microsoft Azure AD Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC771710.png "Microsoft Azure AD Single Sign-On")

3.  On the **Configure App URL** page, in the **Samanage Sign In URL** textbox, type your URL using the following pattern "*https://\<tenant-name\>.samanage.com*", and then click **Next**.

    ![Configure App URL](./media/active-directory-saas-samanage-tutorial/IC771711.png "Configure App URL")

4.  On the **Configure single sign-on at Samanage** page, click **Download certificate**, and then save the certificate file on your computer.

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC777613.png "Configure Single Sign-On")

5.  In a different web browser window, log into your Samanage company site as an administrator.

6.  Click **Dashboard** and select **Setup** in left navigation pane.

    ![Dashboard](./media/active-directory-saas-samanage-tutorial/IC771712.png "Dashboard")

7.  Click **Single Sign-On**.

    ![Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC771713.png "Single Sign-On")

8.  On the **Login using SAML** dialog page, perform the following steps, and then click **Save Changes**:

    1.  Click **Enable Single Sign-On with SAML**.
        ![Login using SAML](./media/active-directory-saas-samanage-tutorial/IC771719.png "Login using SAML")
    2.  In the Azure portal, on the **Configure single sign-on at Samanage** dialog page, copy the **Identity Provider ID** value, and then paste it into the **Identity Provider URL ** textbox.
        ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC771720.png "Configure Single Sign-On")
    3.  In the Azure portal, on the **Configure single sign-on at Samanage** dialog page, copy the **Remote Login URL** value, and then paste it into the **Login URL** textbox.
    4.  In the Azure portal, on the **Configure single sign-on at Samanage** dialog page, copy the **Remote Logout URL** value, and then paste it into the **Logout URL** textbox.
    5.  Create a **base-64 encoded** file from your downloaded certificate.  

        >[AZURE.TIP] For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)

    6.  Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox
    7.  Click **Create users if they do not exist in Samanage**.
        ![Update](./media/active-directory-saas-samanage-tutorial/IC771722.png "Update")
    8.  Click **Update**.

9.  On the Azure AD portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC771723.png "Configure Single Sign-On")
##Configuring user provisioning
  
In order to enable Azure AD users to log into Samanage, they must be provisioned into Samanage.  
In the case of Samanage, provisioning is a manual task.

###To provision a user accounts, perform the following steps:

1.  Log in to your **Samanage** tenant.

2.  Go to **Dashboard \> Setup**.

    ![Setup](./media/active-directory-saas-samanage-tutorial/IC771724.png "Setup")

3.  Click the **Users** tab

    ![Users](./media/active-directory-saas-samanage-tutorial/IC771725.png "Users")

4.  Click **New User**.

    ![New User](./media/active-directory-saas-samanage-tutorial/IC771726.png "New User")

5.  Type the **Email Address** and the **Name** of an Azure AD account you want to provision and click **Create user**.

    >[AZURE.NOTE]The AAD account holder will receive an email and follow a link to confirm their account before it becomes active.

    ![Creat User](./media/active-directory-saas-samanage-tutorial/IC771727.png "Creat User")

>[AZURE.NOTE]You can use any other Samanage user account creation tools or APIs provided by Samanage to provision AAD user accounts.

##Assigning users
  
To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.

###To assign users to Samanage, perform the following steps:

1.  In the Azure AD portal, create a test account.

2.  On the **Samanage **application integration page, click **Assign users**.

    ![Assign users](./media/active-directory-saas-samanage-tutorial/IC771728.png "Assign users")

3.  Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.

    ![Yes](./media/active-directory-saas-samanage-tutorial/IC767830.png "Yes")
  
If you want to test your single sign-on settings, open the Access Panel. For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).
test

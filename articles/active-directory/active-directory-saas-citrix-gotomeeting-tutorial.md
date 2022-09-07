<properties pageTitle="Tutorial: Azure Active Directory integration with Citrix GoToMeeting | Microsoft Azure" description="Learn how to use Citrix GoToMeeting with Azure Active Directory to enable single sign-on, automated provisioning, and more!." services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Tutorial: Azure Active Directory integration with Citrix GoToMeeting  
Applies To: Azure

>[AZURE.TIP]For feedback, click [here](http://go.microsoft.com/fwlink/?LinkId=522412).

The objective of this tutorial is to show the integration of Azure and Citrix GoToMeeting. The scenario outlined in this tutorial assumes that you already have the following items:

-   A valid Azure subscription
-   A tenant in Citrix GoToMeeting

The scenario outlined in this tutorial consists of the following building blocks:

1.  Enabling the application integration for Citrix GoToMeeting
2.  Configuring single sign-on
3.  Configuring user provisioning
4.  Assigning users

![Configuration](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Configuration")
##Enabling the application integration for Citrix GoToMeeting

The objective of this section is to outline how to enable the application integration for Citrix GoToMeeting.

###To enable the application integration for Citrix GoToMeeting, perform the following steps:

1.  In the Azure Management Portal, on the left navigation pane, click **Active Directory**.

    ![Active Directory](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  From the **Directory** list, select the directory for which you want to enable directory integration.

3.  To open the applications view, in the directory view, click **Applications** in the top menu.

    ![Applications](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Applications")

4.  Click **Add** at the bottom of the page.

    ![Add application](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Add application")

5.  On the **What do you want to do** dialog, click **Add an application from the gallery**.

    ![Add an application from gallerry](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Add an application from gallerry")

6.  In the **search box**, type **Citrix GoToMeeting**.

    ![Citrix GoToMeeting](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  In the results pane, select **Citrix GoToMeeting**, and then click **Complete** to add the application.

    ![Citrix GoToMeeting](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##Configuring single sign-on

The objective of this section is to outline how to enable users to authenticate to Citrix GoToMeeting with their account in Azure AD using federation based on the SAML protocol.  
As part of this procedure, you are required to upload a base-64 encoded certificate to your Citrix GoToMeeting tenant.  
If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).

###To configure single sign-on, perform the following steps:

1.  On the **Citrix GoToMeeting** application integration page, click **Configure single sign-on** to open the **CONFIGURE SINGLE SIGN ON** dialog.

    ![Enable single sign-on](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Enable single sign-on")

2.  On the **How would you like users to sign on to Citrix GoToMeeting** page, select **Microsoft Azure AD Single Sign-On**.

    ![Configure single sign-on](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Configure single sign-on")

3.  On the **Configure single sign-on at Citrix GoToMeeting** page, click **Download certificate**, and then save the certificate file on your computer.

    ![Configure single sign-on](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Configure single sign-on")

4.  In a different browser window, log into your Citrix GoToMeeting tenant as administrator.

5.  Open the [Set up SAML 2.0 single sign-on configuration (SSO)](https://login.citrixonline.com/saml/settings.html) page, and then perform the following steps:

    ![SAML setup](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC689232.png "SAML setup")

    1.  Select **Configure manually**.
    2.  In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialogue page, copy the **Sign-Out Page URL** value, and then paste it into the **Sign-out page URL** textbox.
    3.  In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialogue page, copy the **Sign-In Page URL** value, and then paste it into the **Sign-in page URL** textbox.
    4.  Create a **Base-64 encoded** file from your downloaded certificate.  

        >[AZURE.TIP] For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)

    5.  Click **Replace certificate**, and then upload your **base-64 encoded certificate file**.
    6.  Click **Save**.

6.  On the Azure AD portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.

    ![Configure single sign-on](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Configure single sign-on")
##Configuring user provisioning

The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Citrix GoToMeeting.

###To configure user provisioning, perform the following steps:

1.  In the Azure Management Portal, on the **Citrix GoToMeeting** application integration page, click **Configure user provisioning** to open the **Configure User Provisioning** dialog.

    ![Configure user provisioning](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Configure user provisioning")

2.  In the **Citrix GotoMeeting admin user name** page, type a Citrix GoToMeeting account name that has administrative rights on your Citrix GoToMeeting tenant, and then click **Next**.

    ![Configure user provisioning](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Configure user provisioning")

3.  On the **Confirmation** page, click the checkmark to save your configuration.

4.  Click the **validate** button, to verify your configuration.
##Assigning users

To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.

###To assign users to Citrix GoToMeeting, perform the following steps:

1.  In the Azure AD portal, create a test account.

2.  On the **Citrix GoToMeeting** application integration page, click **Assign users**.

    ![Assign users](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Assign users")

3.  Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.

    ![Yes](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Yes")

You should now wait for 10 minutes and verify that the account has been synchronized to Dropbox for Business.

As a first verification step, you can check the provisioning status, by clicking Dashboard in the D on the **Citrix GoToMeeting** application integration page on the Azure Management Portal.

![Dashboard](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Dashboard")

A successfully completed user provisioning cycle is indicated by a related status:

![Integration status](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Integration status")

If you want to test your single sign-on settings, open the Access Panel.

For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).

test

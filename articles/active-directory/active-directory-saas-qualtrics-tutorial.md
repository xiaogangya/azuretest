<properties pageTitle="Tutorial: Azure Active Directory integration with Qualtrics | Microsoft Azure" description="Learn how to use Qualtrics with Azure Active Directory to enable single sign-on, automated provisioning, and more!." services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Tutorial: Azure Active Directory integration with Qualtrics
>[AZURE.TIP]For feedback, click [here](http://go.microsoft.com/fwlink/?LinkId=528601).
  
The objective of this tutorial is to show the integration of Azure and Qualtrics.  
The scenario outlined in this tutorial assumes that you already have the following items:

-   A valid Azure subscription
-   A Qualtrics single sign-on enabled subscription
  
After completing this tutorial, the Azure AD users you have assigned to Qualtrics will be able to single sign into the application at your Qualtrics company site (service provider initiated sign on), or using the [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586)
  
The scenario outlined in this tutorial consists of the following building blocks:

1.  Enabling the application integration for Qualtrics
2.  Configuring single sign-on
3.  Configuring user provisioning
4.  Assigning users

![Scenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")
##Enabling the application integration for Qualtrics
  
The objective of this section is to outline how to enable the application integration for Qualtrics.

###To enable the application integration for Qualtrics, perform the following steps:

1.  In the Azure Management Portal, on the left navigation pane, click **Active Directory**.

    ![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")

2.  From the **Directory** list, select the directory for which you want to enable directory integration.

3.  To open the applications view, in the directory view, click **Applications** in the top menu.

    ![Applications](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")

4.  Click **Add** at the bottom of the page.

    ![Add application](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")

5.  On the **What do you want to do** dialog, click **Add an application from the gallery**.

    ![Add an application from gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")

6.  In the **search box**, type **Qualtrics**.

    ![Application Gallery](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")

7.  In the results pane, select **Qualtrics**, and then click **Complete** to add the application.

    ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
##Configuring single sign-on
  
The objective of this section is to outline how to enable users to authenticate to Qualtrics with their account in Azure AD using federation based on the SAML protocol.

###To configure single sign-on, perform the following steps:

1.  In the Azure AD portal, on the **Qualtrics** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.

    ![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")

2.  On the **How would you like users to sign on to Qualtrics** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.

    ![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")

3.  On the **Configure App URL** page, in the **Qualtrics Sign On URL** textbox, type your URL (e.g.: “*https://ssotest2ut1.qualtrics.com*"), and then click **Next**.

    ![Configure App URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")

4.  On the **Configure single sign-on at Qualtrics** page, click **Download metadata**, and then save the metadata file on your computer.

    ![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")

5.  Send the metadatafile to the Qualtrics support team.

    >[AZURE.NOTE]The single sign-on configuration has to be performed by the Qualtrics support team. You will get a notification as soon as the configuration has been completed.

6.  On the Azure AD portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.

    ![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")
##Configuring user provisioning
  
There is no action item for you to configure user provisioning to Qualtrics.  
When an assigned user tries to log into Qualtrics using the access panel, Qualtrics checks whether the user exists.  
If there is no user account available yet, it is automatically created by Qualtrics.
##Assigning users
  
To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.

###To assign users to Qualtrics, perform the following steps:

1.  In the Azure AD portal, create a test account.

2.  On the **Qualtrics **application integration page, click **Assign users**.

    ![Assign Users](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")

3.  Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.

    ![Yes](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")
  
If you want to test your single sign-on settings, open the Access Panel. For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).
test

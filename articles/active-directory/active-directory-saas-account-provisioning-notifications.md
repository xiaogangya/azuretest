<properties
	pageTitle="Account provisioning notifications"
	description="Learn how to ensure that you are notified of issues related to user provisioning that require your attention by enabling account provisioning notifications."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/27/2015"
	ms.author="markusvi"/>


# Account Provisioning Notifications

With user provisioning, you can automate the process of managing users in third party SaaS applications. <br>
While this is an automated process, your interaction with this process is from time to time required. <br>
This is, for example the case, when the password of the account you have configured to exchange data with a third party SaaS application has expired. 

By enabling account provisioning notifications, you can ensure that you are notified of issues related to user provisioning that require your attention.

You activate or deactivate account provisioning notifications as part of your user provisioning configuration for a third party SaaS application.

![User Provisioning][1] 



To activate account provisioning notifications, select the related checkbox on the **Confirmation** dialog page, and then type the email alias of the recipient.

![Account Provisioning Notifications][2]
 


You can enter a distribution list as recipient; however, it is important to note that the notification email contains links to reports that are only accessible by the Azure AD administrators.

If you have account provisioning notifications enabled, you will receive emails about critical issues that are related to user provisioning. 
 However, to avoid an email overload, you will only receive one notification email per day for each SaaS application the notification email is enabled for.


[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
test

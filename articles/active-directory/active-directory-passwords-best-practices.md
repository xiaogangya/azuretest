<properties 
	pageTitle="Best Practices: Azure AD Password Management | Microsoft Azure" 
	description="Deployment and usage best practices, sample end-user documentation, and training guides for Password Management in Azure Active Directory." 
	services="active-directory" 
	documentationCenter="" 
	authors="asteen" 
	manager="kbrint" 
	editor="billmath"/>

<tags 
	ms.service="active-directory" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/08/2015" 
	ms.author="asteen"/>

# Deploying Password Management and training users to use it
After enabling password reset, the next step you need to take is to get users using the service in your organization. To do this, you'll need to make sure your users are configured to use the service properly and also that your users have the training they need to be successful in managing their own passwords. This article will explain to you the following concepts:

* [**How to get your users configured for Password Management**](#how-to-get-users-configured-for-password-reset)
  * [What makes an account configured for password reset](#what-makes-an-account-configured)
  * [Ways you can to populate authentication data yourself](#ways-to-populate-authentication-data)
* [**The best ways to roll out password reset to your organization**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [Email-based rollout + sample email communications](#email-based-rollout)
  * [How to use enforced registration to force users to register at sign in](#using-enforced-registration)
  * [How to upload authentication data for user accounts](#uploading-data-yourself)
* [**Sample user and support training materials (coming soon!)**](#sample-training-materials)

## How to get users configured for password reset
This section describes to you various methods by which you can ensure every user in your organization can use self-service password reset effectively in case they forget their password.

### What makes an account configured
Before a user can use password reset, **all** of the following conditions must be met:

1.	Password reset must be enabled in the directory.  Learn how to enable password reset by reading [Enable users to reset their Azure AD Passwords](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) or [Enable users to reset or change their AD Passwords](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
2.	The user must be licensed.
 - For cloud users, the user must have **any paid Office 365 license**, or an **AAD Basic** or **AAD Premium license** assigned.
 - For on-prem users (federated or hash synced), the user **must have an AAD Premium license assigned**.
3.	The user must have the **minimum set of authentication data defined** in accordance with the current password reset policy.
 - Authentication data is considered defined if the corresponding field in the directory contains well-formed data.
 - A minimum set of authentication data is defined as at **least one** of the enabled authentication options if a one gate policy is configured, or at **least two** of the enabled authentication options if a two gate policy is configured.
4.	If the user is using an on-premises account, then [Password Writeback](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) must be enabled and turned on

### Ways to populate authentication data
You have several options on how to specify data for users in your organization to be used for password reset.

- Edit users in the [Azure Management Portal](https://manage.windowsazure.com) or the [Office 365 Admin Portal](https://portal.microsoftonline.com)
- Use AADSync to synchronize user properties into Azure AD from an on-premises Active Directory domain
- Use Windows PowerShell to edit user properties
- Allow users to register their own data by guiding them to the registration portal at [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- Require users to register for password reset when they sign in to the Access Panel at [http://myapps.microsoft.com](http://myapps.microsoft.com) by setting the **Require users to register SSPR** configuration option to **Yes**.

Users need not register for password reset for the system to work.  For example, if you have existing phone numbers in your local directory, you can synchronize them in Azure AD and we will use them for password reset automatically.

## What is the best way to roll out password reset for users?
The following are the general rollout steps for password reset:

1.	Enable password reset in your directory by going to the **Configure** tab in the [Azure Management Portal](https://manage.windowsazure.com) and selecting **Yes** for the **Users Enabled for Password Reset** option.
2.	Assign the appropriate licenses to each user to whom you’d like to offer password reset in the by going to the **Licenses** tab in the [Azure Management Portal](https://manage.windowsazure.com).
3.	Optionally restrict password reset to a group of users to roll out the feature slowly over time by setting the **Restrict Access to Password Reset** toggle to **Yes** and selecting a security group to enable for password reset (note these users must all have licenses assigned to them).
4.	Instruct your users to use password reset by either sending them an email instructing them to register, enabling enforced registration on the access panel, or by uploading the appropriate authentication data for those users yourself via DirSync, PowerShell, or the [Azure Management Portal](https://manage.windowsazure.com).  More details on this are provided below.
5.	Over time, review users registering by navigating to the Reports tab and viewing the [**Password Reset Registration Activity**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) report.
6.	Once a good number of users have registered, watch them use password reset by navigating to the Reports tab and viewing the [**Password Reset Activity**](active-directory-passwords-get-insights.md#view-password-reset-activity) report.

There are several ways to inform your users that they can register for and use password reset in your organization.  They are detailed below.

### Email-based rollout
Perhaps the simplest approach to inform your users about to register for or use password reset is by sending them an email instructing them to do so.  Below is a template you can use to do this.  Feel free to replace the colors / logos with those of your own choosing to customize it to fit your needs.

  ![][001]

You can download the email template [here](http://1drv.ms/1xWFtQM).

### Using enforced registration
If you want your users to register for password reset themselves, you can also force them to register when they sign in to the access panel at [http://myapps.microsoft.com](http://myapps.microsoft.com).  You can enable this option from your directory’s **Configure** tab by enabling the **Require Users to Register when Signing in to the Access Panel** option.  

You can also optionally define whether or not they will be asked to re-register after a configurable period of time by modifying the **Number of days before users must confirm their contact data** option to be a non-zero value. See [Customizing User Password Management Behavior](active-directory-passwords-customize.md#password-management-behavior) for more information.

  ![][002]

After you enable this option, when users sign in to the access panel, they will see a popup that informs them that their administrator has required them to verify their contact information. They can use it to reset their password if they ever lose access to their account.

  ![][003]

Clicking **Verify Now** brings them to the **password reset registration portal** at [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) and requires them to register.  Registration via this method can be dismissed by clicking the **cancel** button or closing the window, but users are reminded every time they sign in if they do not register.

  ![][004]

### Uploading data yourself
If you want to upload authentication data yourself, then users need not register for password reset before being able to reset their passwords.  As long as users have the authentication data defined on their account that meets the password reset policy you have defined, then those users will be able to reset their passwords.

To learn what properties you can set via AAD Connect or Windows PowerShell, see [What data is used by password reset](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

You can upload the authentication data via the [Azure Management Portal](https://manage.windowsazure.com) by following the steps below:

1.	Navigate to your directory in the **Active Directory extension** in the [Azure Management Portal](https://manage.windowsazure.com).
2.	Click on the **Users** tab.
3.	Select the user you are interested in from the list.
4.	On the first tab, you will find **Alternate Email**, which can be used as a property to enable password reset. 

    ![][005]

5.	Click on the **Work Info** tab.
6.	On this page, you will find **Office Phone**, **Mobile Phone**, **Authentication Phone**, and **Authentication Email**.  These properties can also be set to allow a user to reset his or her password. 

    ![][006]

See [What data is used by password reset](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) to see how each of these properties can be used.

## Sample training materials
We are working on sample training materials that you can use to get your IT organization and your users up to speed quickly on how to deploy and use password reset.  Stay tuned!


<br/>
<br/>
<br/>

**Additional Resources**


* [What is Password Management](active-directory-passwords.md)
* [How Password Management works](active-directory-passwords-how-it-works.md)
* [Getting started with Password Mangement](active-directory-passwords-getting-started.md)
* [Customize Password Management](active-directory-passwords-customize.md)
* [How to get Operational Insights with Password Management Reports](active-directory-passwords-get-insights.md)
* [Password Management FAQ](active-directory-passwords-faq.md)
* [Troubleshoot Password Management](active-directory-passwords-troubleshoot.md)
* [Learn More](active-directory-passwords-learn-more.md)
* [Password Management on MSDN](https://msdn.microsoft.com/library/azure/dn510386.aspx)



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
 
test

<properties
	pageTitle="Azure Active Directory Reporting Notifications"
	description="How to use the Azure Active Directory reporting notifications for suspicious sign ins."
	services="active-directory"
	documentationCenter=""
	authors="SSalahAhmed"
	manager="gchander"
	editor="LisaToft"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/17/2015"
	ms.author="saah;kenhoff"/>

# Azure Active Directory Reporting Notifications

## What reports generate email notifications

At this time, only the Irregular Sign in Activity report triggers email notifications.

## What is an "Irregular Sign in"?

Irregular Sign ins are those that have been identified by our machine learning algorithms, on the basis of an "impossible travel" condition combined with an anomalous sign in location and device. This may indicate that a hacker has been trying to sign in using this account. 

## Who receives the email notifications?

The email is sent to all global admins who have been assigned an Active Directory Premium license. To ensure it is delivered, we send it to the admins Alternate Email Address as well. Admins should include aad-alerts-noreply@mail.windowsazure.com in their safe senders list so they don’t miss the email.

## How often are these emails sent?

The email is sent if 10 new Irregular Sign in Activities occur in the last 30 days, or since the last email was sent, whichever is less.

## How do I access the report mentioned in the email?

When you click on the link, you will be redirected to the report page within the Azure Management Portal. In order to access the report, you need to be both:

- An admin or co-admin of your Azure subscription
- A global administrator in the directory, and assigned an Active Directory Premium license. For more information, see Azure Active Directory editions.

## Can I turn off these emails?

Yes, to turn off notifications related to anomalous sign ins within the Azure Management Portal, click **Configure**, and then select **Disabled** under the **Notifications** section.

## What's next
- Curious about what security, audit, and activity reports are available? Check out [Azure AD Security, Audit, and Activity Reports](active-directory-view-access-usage-reports.md)
- [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Add company branding to your Sign In and Access Panel pages](active-directory-add-company-branding.md)

test

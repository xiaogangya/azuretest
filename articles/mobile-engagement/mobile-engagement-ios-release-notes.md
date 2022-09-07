<properties
	pageTitle="Azure Mobile Engagement iOS SDK Release Notes"
	description="Latest updates and procedures for iOS SDK for Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="MehrdadMzfr"
	manager="dwrede"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-ios"
	ms.devlang="objective-c"
	ms.topic="article"
	ms.date="08/05/2015"
	ms.author="MehrdadMzfr" />

#Release notes

##3.1.0 (08/26/2015)

-   Fix iOS 9 compatibility bug with a third party library. It was causing crashes while sending polls results, application information or extra data.

##3.0.0 (06/19/2015)

-   Mobile Engagement uses Silent Push Notifications.
-   Dropped support for iOS 4.X. Starting from this version the deployment target of your application must be at least iOS 6.

##2.2.0 (05/21/2015)

-   The Mobile Engagement device id for devices < iOS 6 is now based on a GUID generated at installation time.

##2.1.0 (04/24/2015)

-   Added Swift compatibility.
-   When clicking on a notification, the action URL is now executed right after the application is opened.
-   Added missing header file in SDK package.
-   Fixed an issue when the Mobile Engagement crash reporter was disabled.

##2.0.0 (02/17/2015)

-   Initial Release of Azure Mobile Engagement
-   appId/sdkKey configuration is replaced by a connection string configuration.
-   Removed API to send and receive arbitrary XMPP messages from arbitrary XMPP entities.
-   Removed API to send and receive messages between devices.
-   Security improvements.
-   SmartAd tracking removed.

test

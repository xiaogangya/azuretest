<properties 
	pageTitle="Azure Mobile Engagement Android SDK Integration" 
	description="Latest updates and procedures for Android SDK for Azure Mobile Engagement"
	services="mobile-engagement" 
	documentationCenter="mobile" 
	authors="piyushjo" 
	manager="dwrede" 
	editor="" />

<tags 
	ms.service="mobile-engagement" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-android" 
	ms.devlang="Java" 
	ms.topic="article" 
	ms.date="08/10/2015" 
	ms.author="piyushjo" />


#Release notes

##4.1.0 (08/25/2015)

- Handle new permission model for Android M.
- Can now configure location features at runtime instead of using  `AndroidManifest.xml`.
- Fix a permission bug: if you use `ACCESS_FINE_LOCATION`, then `ACCESS_COARSE_LOCATION` is not needed anymore.
- Stability improvements.

##4.0.0 (07/06/2015)

-   Internal protocol changes to make analytics and push more reliable.
-   Native push (GCM/ADM) is now also used for in app notifications so you must configure the native push credentials for any type of push campaign.
-   Fix big picture notification: they were displayed only 10s after being pushed.
-   Fix a bug in web view: clicking on a link was also executing the default action URL.
-   Fix a rare crash related to local storage management.
-   Fix dynamic configuration string management.
-   Update EULA.

##3.0.0 (02/17/2015)

-   Initial Release of Azure Mobile Engagement
-   appId configuration is replaced by a connection string configuration.
-   Removed API to send and receive arbitrary XMPP messages from arbitrary XMPP entities.
-   Removed API to send and receive messages between devices.
-   Security improvements.
-   Google Play and SmartAd tracking removed.

 
test

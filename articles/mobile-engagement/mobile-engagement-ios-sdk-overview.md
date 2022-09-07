<properties
	pageTitle="Azure Mobile Engagement iOS SDK Overview"
	description="Latest updates and procedures for iOS SDK for Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
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

#iOS SDK for Azure Mobile Engagement

Start here to get all the details on how to integrate Azure Mobile Engagement in an iOS App. If you'd like to give it a try first, make sure you do our [15 minutes tutorial](mobile-engagement-ios-get-started.md).

Click to see the [SDK Content](mobile-engagement-ios-sdk-content.md)

##Integration procedures
1. Start here: [How to integrate Mobile Engagement in your iOS app](mobile-engagement-ios-integrate-engagement.md)

2. For Notifications: [How to integrate Reach (Notifications) in your iOS app](mobile-engagement-ios-integrate-engagement-reach.md)

3. Tag plan implementation: [How to use the advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md)


##Release notes

###3.1.0 (08/26/2015)

-   Fix iOS 9 compatibility bug with a third party library. It was causing crashes while sending polls results, application information or extra data.

For earlier version please see the [complete release notes](mobile-engagement-ios-release-notes.md)

##Upgrade procedures

If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.

You may have to follow several procedures if you missed several versions of the SDK see the complete [Upgrade Procedures](mobile-engagement-ios-upgrade-procedure.md).

For each new version of the SDK you must first replace (remove and re-import in xcode) the EngagementSDK and EngagementReach folders.

###From 2.0.0 to 3.0.0
Dropped support for iOS 4.X. Starting from this version the deployment target of your application must be at least iOS 6.

If you are using Reach in your application, you must add `remote-notification` value to the `UIBackgroundModes` array in your Info.plist file in order to receive remote notifications.

The method `application:didReceiveRemoteNotification:` needs to be replaced by `application:didReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate.

"AEPushDelegate.h" is deprecated interface and you need to remove all references. This includes removing `[[EngagementAgent shared] setPushDelegate:self]` and the delegate methods from your application delegate:

	-(void)willRetrieveLaunchMessage;
	-(void)didFailToRetrieveLaunchMessage;
	-(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;


test

<properties
	pageTitle="How to configure Twitter authentication for your App Services application"
	description="Learn how to configure Twitter authentication for your App Services application."
	services="app-service\mobile"
	documentationCenter=""
	authors="mattchenderson" 
	manager="dwrede"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="07/27/2015"
	ms.author="mahender"/>

# How to configure your application to use Twitter login

[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

This topic shows you how to configure Azure Mobile Apps to use Twitter as an authentication provider.

To complete the procedure in this topic, you must have a Twitter account that has a verified email address. To create a new Twitter account, go to <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Register your application with Twitter


1. Log on to the [Azure Management Portal], and navigate to your Mobile App. Copy your **URL**. You will use this to configure your Twitter app.

2. Click **Settings**, **User authentication**, and then click **Twitter**. Copy the **Callback URL**. You will use this to configure your Twitter app.

3. Navigate to the [Twitter Developers] website, sign-in with your Twitter account credentials, and click **Create New App**.

4. Type in the **Name** and a **Description** for your new app. Paste in your **Mobile App URL** for the **Website** value. Then, for the **Callback URL**, paste the **Callback URL** you copied earlier. This is your Mobile App gateway appended with the path, _/signin-twitter_. For example, `https://contosogateway.azurewebsites.net/signin-twitter`. Make sure that you are using the HTTPS scheme.

    ![][0]

3.  At the bottom the page, read and accept the terms. Then click **Create your Twitter application**. This registers the app displays the application details.

4. Click the **Settings** tab, check **Allow this application to be used to sign in with Twitter**, then click **Update Settings**.

5. Select the **Keys and Access Tokens** tab. Make a note of the values of **Consumer Key (API Key)** and **Consumer secret (API Secret)**.

    > [AZURE.NOTE] The consumer secret is an important security credential. Do not share this secret with anyone or distribute it with your app.


## <a name="secrets"> </a>Add Twitter information to your Mobile App

1. Back in the [Azure Management Portal] on the Twitter setting blade for your Mobile App, paste in the API Key and API Secret values which you obtained previously. Then click **Save**.

    ![][1]

You are now ready to use Twitter for authentication in your app.

## <a name="related-content"> </a>Related Content

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication-preview/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication-preview/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure Management Portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-dotnet-backend-xamarin-ios-get-started-users-preview.md
 
test

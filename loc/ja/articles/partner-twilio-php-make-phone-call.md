<properties 
	pageTitle="How to make a phone call from Twilio (PHP) - Azure" 
	description="Learn how to make a phone call and send a SMS message with the Twilio API service on Azure. Samples are for PHP application." 
	documentationCenter="php" 
	services="" 
	authors="devinrader" 
	manager="twilio" 
	editor="mollybos"/>

<tags 
	ms.service="multiple" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="PHP" 
	ms.topic="article" 
	ms.date="11/25/2014" 
	ms.author="microsofthelp@twilio.com"/>

# Azure 上の PHP アプリケーションの Twilio を使用して電話をかける方法 

次の例では、Azure でホストされる PHP web ページから呼び出す Twilio を使用する方法を示します。次のスクリーン ショットに示すように、作成されたアプリケーションはユーザーに電話をかける値を求めます。

![Twilio と PHP を使用して紺碧呼び出しフォーム][twilio_php]

このトピックのコードを使用する、次を行う必要があります。

1. Twilio アカウントと認証を取得トークン。Twilio を始める、評価での価格設定 [http://www.twilio.com/pricing][twilio_pricing].トライアル アカウントにサインアップすることができます。 [https://www.twilio.com/try-twilio][try_twilio].Twilio によって提供される API の詳細については、次を参照してください。 [http://www.twilio.com/api][twilio_api].
2. PHP の Twilio ライブラリを取得します。(GitHub からダウンロードできます。[https://github.com/twilio/twilio-php][twilio_php_github]) または PEAR パッケージとしてインストールします。詳細については、次を参照してください。 [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].
3. PHP の Azure SDK をインストールします。SDK とインストール方法の概要については、次を参照してください。 [PHP の Azure SDK を設定します。][setup_php_sdk].

## 電話を行うための web フォームを作成します。

The following HTML code shows how to build a web page (**callform.html**) that retrieves user data for making a call:

    <html>
	<head>
		<title>Automated call form</title>
	</head>
	<body>
	<h1>Automated Call Form</h1>
 	<p>Fill in all fields and click <b>Make this call</b>.</p>
  	<form action="makecall.php" method="post">
   	<table>
     	<tr>
       		<td>To:</td>
       		<td><input type="text" size=50 name="callTo" value=""></td>
     	</tr>
     	<tr>
       		<td>From:</td>
       		<td><input type="text" size=50 name="callFrom" value=""></td>
     	</tr>
     	<tr>
       		<td>Call message:</td>
       		<td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
     	</tr>
     	<tr>
       		<td colspan=2><input type="submit" value="Make this call"></td>
     	</tr>
   	</table>
 	</form>
 	<br/>
	</body>
	</html>

## Create the code to make the call
The following code shows how to build a web page (**makecall.php**) which is called when the user submits the form displayed by **callform.html**. The code shown below creates the call message and generates the call. (Use your Twilio account and authentication token instead of the placeholder values assigned to **$sid** と **$token** in the code below.)

    <html>
	<head><title>Making call...</title></head>
	<body>
	<p>Your call is being made.</p>

	<?php
	require_once 'Services/Twilio.php';

	$sid = "your_account_sid";
	$token = "your_authentication_token";

	$from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
	$to_number = $_POST['callTo'];
	$message = $_POST['callText'];

	$client = new Services_Twilio($sid, $token, "2010-04-01");

	$call = $client->account->calls->create(
		$from_number, 
		$to_number,
  		'http://twimlets.com/message?Message='.urlencode($message)
	);

	echo "Call status: ".$call->status."<br />";
	echo "URI resource: ".$call->uri."<br />";
	?>
	</body>
	</html>

In addition to making the call, **makecall.php** displays some call metadata (example shown in screenshot below). For more information about call metadata, see [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure Call Response Using Twilio and PHP][twilio_php_response]

## Run the application
The next step is to deploy your application to Azure Websites. The following articles contain the information for creating a website and deploying your code with Git, FTP, or WebMatrix (though not all information in each article is relevant):

* [Create a PHP-MySQL Azure Web Site and deploy using Git][website-git]
* [Create a PHP-MySQL Azure Web Site and Deploy Using FTP][website-ftp]
* [Create and deploy a PHP-MySQL Azure Web Site using WebMatrix][website-webmatrix]

## 次のステップ
This code was provided to show you basic functionality using Twilio in PHP on Azure. Before deploying to Azure in production, you may want to add more error handling or other features. For example:

* Instead of using a web form, you could use Azure storage blobs or SQL Database to store phone numbers and call text. For information about using Azure storage blobs in PHP, see [Using Azure Storage with PHP Applications][howto_blob_storage_php]. For information about using SQL Database in PHP, see [Using SQL Database with PHP Applications][howto_sql_azure_php].
* 、 **makecall.php** code uses Twilio-provided URL ([http://twimlets.com/message][twimlet_message_url]) to provide a Twilio Markup Language (TwiML) response that informs Twilio how to proceed with the call. For example, the TwiML that is returned can contain a `<Say>` verb that results in text being spoken to the call recipient. Instead of using the Twilio-provided URL, you could build your own service to respond to Twilio's request; for more information, see [Twilio を音声および SMS 機能を PHP で使用する方法][howto_twilio_voice_sms_php]. More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about `<Say>` and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Read the Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].

For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].

## また見なさい
* [Twilio を音声および SMS 機能を PHP で使用する方法](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_php]: https://github.com/twilio/twilio-php
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/
[website-ftp]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/
[website-webmatrix]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-webmatrix/
[twilio_php_github]: https://github.com/twilio/twilio-php

<properties 
	pageTitle="Upload a Microsoft Azure Management API Certificate into the Portal" 
	description="Learn how to upload athe Management API certficate in Microsoft Azure" 
	services="cloud-services" 
	documentationCenter=".net" 
	authors="Thraka" 
	manager="timlt" 
	editor=""/>

<tags 
	ms.service="na" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/20/2015"
	ms.author="adegeo"/>


# Upload an Azure Management API Management Certificate

Management certificates allow you to authenticate with the Service Management API provided by Azure. Many programs and tools (such as Visual Studio or the Azure SDK) will use these certficates to automate configuration and deployment of various Azure services. These are not really related to cloud services. 

>[AZURE.WARNING] Be careful! These types of certificates allow anyone who authenticates with them to manage the subscription they are associated with. 

More information about Azure certificates (including creating a self-signed certificate) is [available](cloud-services/cloud-services-certs-create.md#what-are-management-certificates) to you if you need it.

You can also use [Azure Active Directory](http://azure.microsoft.com/documentation/services/active-directory/) to authenticate client-code for automation purposes.

## Upload a management certificate

Once you have a management certficate created, (.cer file with only the public key) you can upload it into the portal. When the certificate is available in the portal, anyone with a matching certficiate (private key) can connect through the Management API and access the resources for the associated subscription.

1. Log into the [Azure Management Portal](http://manage.windowsazure.com).
2. Click **Settings** on the left side of the portal (you may need to scroll down). 
    
    ![Settings](./media/azure-api-management-certs/settings.png)

3. Click the **Management Certificates** tab.1

    ![Settings](./media/azure-api-management-certs/certificates-tab.png)
    
4. Click the **Upload** button.

    ![Settings](./media/azure-api-management-certs/upload.png)
    
5. Fill out the dialog information and click the done **Checkmark**.

    ![Settings](./media/azure-api-management-certs/upload-dialog.png)

## Next steps

Now that you have a management certficate associated with a subscription, you can (after you have installed the matching certificate locally) programatically connect to the [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) and automate the various Azure resources that are also associated with that subscription. 
test

write access test

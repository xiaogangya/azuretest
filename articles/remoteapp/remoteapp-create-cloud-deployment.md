<properties 
	pageTitle="How to create a cloud collection of Azure RemoteApp" 
	description="Learn how to create a deployment of Azure RemoteApp that saves data in the Azure cloud." 
	services="remoteapp" 
	documentationCenter="" 
	authors="lizap" 
	manager="mbaldwin" 
	editor=""/>

<tags 
	ms.service="remoteapp" 
	ms.workload="compute" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/02/2015" 
	ms.author="elizapo"/>

# How to create a cloud collection of Azure RemoteApp

There are two kinds of Azure RemoteApp collections: 

- Cloud: resides completely in Azure. You can choose to save all data in the cloud (so a cloud-only collection) or to connect your collection to a VNET and save data there.   
- Hybrid: includes a virtual network for on-premises access - this requires the use of Azure AD and an on-premises Active Directory environment.

This tutorial walks you through the process of creating a cloud collection. There are four steps: 

1.	Create a RemoteApp collection.
2.	Optionally configure directory synchronization. If you are using Azure AD + Active Directory, you have to to synchronize users, contacts, and passwords from your on-premises Active Directory to your Azure AD tenant.
5.	Publish RemoteApp apps.
6.	Configure user access.

**Note** *This topic is in the middle of being reworked. I've updated the steps to reflect the new UI, but I'm not yet able to republish the entire topic. I'm working on a couple of new articles that will make it much easier to figure out your authentication and collection options. So, if you're confused, please know that I know and am working as quickly as I can to get better info to you. Thanks.*

**Before you begin**

You need to do the following before creating the collection:

- [Sign up](http://azure.microsoft.com/services/remoteapp/) for Azure RemoteApp. 
- Gather information about the users that you want to grant access to. This can be either Microsoft account information or Active Directory work account information for users.
- This procedure assumes you are either going to use one of the template images provided as part of your subscription or that you have already uploaded the template image you want to use. If you need to upload a different template image, you can do that from the Template Images page. Just click **upload a template image** and follow the steps in the wizard. 
- Want to use the Office 365 ProPlus image? Check out info [here](remoteapp-officesubscription.md).
- Want to provide custom apps or LOB programs? Create a new [image](remoteapp-imageoptions.md) and use it in your cloud collection.
- Figure out whether you need to connect to a VNET. If you choose to connect to a VNET, make sure it meets the sizing guidelines and that it can connect to RemoteApp.
- If you're using a VNET, decide whether you want to join it to your local Active Directory domain.

## Step 1: Create a cloud collection - with or without a VNET##


Use the following steps to **create a cloud-only collection**:

1. In the management portal, go to the RemoteApp page.
2. Click **New > Quick Create**.
3. Enter a name for your collection, and select your region.
4. Choose the plan that you want to use - standard or basic.
5. Choose the template to use for this collection. 

	**Tip:** Your subscription for RemoteApp comes with [template images](remoteapp-images.md) that contain Office 365 or Office 2013 (for trial use) programs, some published (such as Word) and others ready to publish. You can also create a new [image](remoteapp-imageoptions.md) and use it in your cloud collection.


1. Click **Create RemoteApp collection**.
	
	**Important:** It can take up to 30 minutes to provision your collection.

After your RemoteApp collection has been created, double-click the name of the collection. That will bring up the **Quick Start** page - this is where you finish configuring the collection.

Use the following steps to create a **cloud + VNET collection**:

1. In the management portal, go to the RemoteApp page.
2. Click **New** > **Create with VNET**.
3. Enter a name for your collection.
4. Choose the plan that you want to use - standard or basic.
5. Choose the VNET you already created. Don't know how to do that? For now, the steps are in the [hybrid](remoteapp-create-hybrid-deployment.md) topic.
6. Decide whether you want to join your collection to your domain. If yes, you'll need to use AD Connect to integrate Azure AD and your Active Directory environment. That's covered in below in **Step 2**.
6. Click **Create RemoteApp collection**.


## Step 2: Configure Active Directory directory synchronization (optional) ##

If you want to use Active Directory, RemoteApp requires directory synchronization between Azure Active Directory and your on-premises Active Directory to synchronize users,  contacts, and passwords to your Azure Active Directory tenant. See [Configuring Active Directory for Azure RemoteApp](remoteapp-ad.md) for planning information. You can also go directly to [AD Connect](http://blogs.technet.com/b/ad/archive/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect.aspx) for information.

## Step 3: Publish RemoteApp apps ##

A RemoteApp app is the app or program that you provide to your users. It is located in the template image you uploaded for the collection. When a user accesses a RemoteApp app, the app appears to run in their local environment, but it is really running in Azure. 

Before your users can access apps, you need to publish them to the end-user feed – a list of available apps that your users access through the Remote Desktop client.
 
You can publish multiple apps to your RemoteApp collection. From the RemoteApp publishing page, click **Publish** to add a program. You can either publish from the Start menu of the template image or by specifying the path on the template image for the app. If you choose to add from the Start menu, choose the app to publish. If you choose to provide the path to the app, provide a name for the app and the path to where it is installed on the template image.

## Step 4: Configure user access ##

Now that you have created your RemoteApp collection, you need to add the users that you want to be able to use your remote resources. If you are using Active Directory, the users that you provide access to need to exist in the Active Directory tenant associated with the subscription you used to create this RemoteApp collection.

1.	From the Quick Start page, click **Configure user access**. 
2.	Enter the work account (from Active Directory) or Microsoft account that you want to grant access for.

	**Notes:** 

	Make sure that you use the “user@domain.com” format.

	If you are using Office 365 ProPlus in your collection, you must use the Active Directory identities for your users. This helps validate licensing. 

3.	After the users are validated, click **Save**.


## Next steps ##

That's it - you have successfully created and deployed your RemoteApp cloud collection. The next step is to have your users download and install the Remote Desktop client. You can find the URL for the client on the RemoteApp Quick Start page. Then, have users log into the client and access the apps you published.

 
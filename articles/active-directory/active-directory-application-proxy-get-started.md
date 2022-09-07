<properties
	pageTitle="How to provide secure remote access to on-premises apps"
	description="Covers how to use Azure AD Application Proxy to provide secure remote access to your on-premises apps."
	services="active-directory"
	documentationCenter=""
	authors="rkarlin"
	manager="terrylan"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/07/2015"
	ms.author="rkarlin"/>

# How to provide secure remote access to on-premises applications

You want to provide access for remote users who have all kinds of devices – managed, unmanaged; tablets, smartphones and laptops. And providing secure access to a myriad of resources can be complex. In recent years, reverse proxies were a popular way to provide secure remote access, but they needed to be behind firewalls which were hard to secure and hard to make highly available.

## Secure remote access in the cloud
In a modern cloud environment, we take remote access to the next level using Application Proxy in Microsoft Azure Active Directory. Application Proxy is a feature of Azure AD that is supplied as a service, meaning that it’s easy to deploy and use. It integrates with Azure Active Directory, the same identity platform that is used by Office 365.

## What is Azure Active Directory Application Proxy?
Application Proxy provides single-sign-on and secure remote access for web applications hosted on-premises, such as such as SharePoint sites and Outlook Web Access. Your on-premises web applications can now be accessed the same way as your SaaS apps in Azure Active Directory, without the need for a VPN or changing the network infrastructure. Application Proxy lets you publish applications. Employees can log into your apps from home, on their own devices and authenticate through this cloud-based proxy.

## How does it work?
### Enabling Access
Application Proxy works by installing a slim Windows Server service called the Connector inside your network. The Connector doesn’t necessitate opening any inbound ports and you don’t have to put anything in the DMZ. If you have a lot of traffic to your apps you can add more Connectors, and the service will take care of the load balancing. The Connectors are stateless and pull everything from the cloud as necessary. 
When a user accesses applications remotely, from any device, he’s authenticated by Azure Active Directory and gets access to the application. 

### Single sign-on
Azure AD Application proxy provides single sign-on (SSO) to applications that use IWA or claims-aware applications. If your application uses Integrated Windows Authentication, Application Proxy impersonates the user using Kerberos Constrained Delegation to provide SSO. SSO to claims-aware applications is achieved because the user was already authenticated by Azure Active Directory.

## How to get started
Make sure you have an Azure AD basic or premium subscription and an Azure AD directory for which you are a global administrator. You also need Azure AD basic or premium licenses for the directory administrator and users accessing the apps. Take a look here for more information. 

### Getting started enabling remote access to on-premises applications
Setting up Application Proxy is accomplished in two steps:

1. [Enable Application Proxy and configure the Connector](active-directory-application-proxy-enable.md)<br>
2. [Publish applications](active-directory-application-proxy-publish.md) - use the quick and easy wizard to get your on-premises apps published and accessible remotely.

## What's next?
There's a lot more you can do with Application Proxy:


- [Publish applications using your own domain name](https://msdn.microsoft.com/library/azure/mt210927.aspx)
- [Enable single-sign on](https://msdn.microsoft.com/library/azure/dn879065.aspx)
- [Working with claims aware applications](https://msdn.microsoft.com/library/azure/mt210926.aspx)
- [Enable conditional access](https://msdn.microsoft.com/library/azure/dn931796.aspx)


### Learn more about Application Proxy
- [Take a look here at our online help](https://msdn.microsoft.com/library/azure/dn768219.aspx)
- [Check out the Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)
- [Watch our videos on Channel 9!](http://channel9.msdn.com/events/Ignite/2015/BRK3864)

## Additional resources
* [Sign up for Azure as an organization](../sign-up-organization.md)
* [Azure Identity](../fundamentals-identity.md)

test

<properties 
	pageTitle="Directory integration tools comparison" 
	description="This is page will provide you with comprehensive tables that compare the various directory integration tools." 
	services="active-directory" 
	documentationCenter="" 
	authors="billmath" 
	manager="stevenpo" 
	editor="curtand"/>

<tags 
	ms.service="active-directory" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/24/2015" 
	ms.author="billmath"/>

# Directory integration tools comparison

Over the years the directory integration tools have grown and evolved.  This document is to help provide a consolidated view of these tools and a comparison of the features that are available in each. 

>[AZURE.NOTE] Azure AD Connect incorporates the components and functionality previously released as Dirsync and AAD Sync. These tools are no longer being released individually, and all future improvements will be included in updates to Azure AD Connect, so that you always know where to get the most current functionality. 
>
>Currently Dirsync is still supported, however at some point in the future it will be deprecated. Once it is deprecated, it will only be supported for a period of time.  After this period of time support for Dirsync will end.


Use the following key for each of the tables.

●  = Available Now</br>
FR = Future Release</br>
PP = Public Preview</br>


## On-Premises to Cloud Synchronization

| Feature  | Azure Active Directory Connect  | Azure Active Directory Synchronization Services (AAD Sync) | Azure Active Directory Synchronization Tool (DirSync)| Forefront Identity Manager 2010 R2 (FIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Connect to single on-premises AD forest | ● | ● | ● | ● |
| Connect to multiple on-premises AD forests |●  | ● |  | ● |
| Connect to multiple on-premises Exchange Orgs | ● |  |  |  |
| Connect to single on-premises LDAP directory | FR |  |  | ● |
| Connect to multiple on-premises LDAP directories |FR  |  |  | ● |
| Connect to on-premises AD and on-premises LDAP directories |FR  |  |  | ● |
| Connect to custom systems (i.e. SQL, Oracle, MySQL, etc.) | FR |  |  | ● |
| Synchronize customer defined attributes (directory extensions) | ● |  |  |  |

## Cloud to On-Premises Synchronization

| Feature  | Azure Active Directory Connect  | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Writeback of devices | ● |  | ● |  |
| Attribute writeback (for Exchange hybrid deployment ) | ● | ● | ● | ● |
| Writeback of users and groups objects |  ●|  | |  |
| Writeback of passwords (from self-service password reset (SSPR) and password change) |  ● | ● |  |  |



## Authentication Feature Support

| Feature  | Azure Active Directory Connect | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Password Sync for single on-premises AD forest | ● | ● | ● |  |
| Password Sync for multiple on-premises AD forests |  ●| ● |  |  |
| Single Sign-on with Federation | ● | ● | ● | ● |
| Writeback of passwords (from SSPR and password change) |●  | ● |  |  |



## Set-up and Installation

| Feature  | Azure Active Directory Connect  | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Supports installation on a Domain Controller | ● | ● | ● |  |
| Supports installation using SQL Express | ● | ● | ● |  |
| Easy upgrade from DirSync |● |  |  |  |
| Localization Windows Server languages | FR | ● | ● |  |
| Support for Windows Server 2008 and Windows Server 2008 R2 | ● for Sync, No for federation| ● | ●  | ● |
| Support for Windows Server 2012 and Windows Server 2012 R2 | ● | ● | ● | Only 2012 |


## Filtering and Configuration

Feature  | Azure Active Directory Connect | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identity Manager 2010 R2 (FIM)  
:-------- |:--------:|:--------:|:--------:|:--------:|
Filter on Domains and Organizational Units | ● | ● | ● | ●  
Filter on objects’ attribute values | ● | ● | ● | ● 
Allow minimal set of attributes to be synchronized (MinSync) | ● | ● |  |   
Allow different service templates to be applied for attribute flows |●  | ● |  |   
Allow removing attributes from flowing from AD to Azure AD | ● | ● |  |  
Allow advanced customization for attribute flows | ● | ● |  | ●  

test

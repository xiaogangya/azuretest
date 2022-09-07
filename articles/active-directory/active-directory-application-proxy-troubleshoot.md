<properties
	pageTitle="Troubleshoot Application Proxy"
	description="Covers how to troubleshoot errors in Azure AD Application Proxy."
	services="active-directory"
	documentationCenter=""
	authors="rkarlin"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/11/2015"
	ms.author="rkarlin"/>



# Troubleshoot Application Proxy


> [AZURE.IMPORTANT] Application Proxy is a feature that is available only if you upgraded to the Premium or Basic edition of Azure Active Directory. For more information, see [Azure Active Directory editions](active-directory-editions.md).

If errors occur in accessing a published application or in publishing applications, check the following options to see if Microsoft Azure AD Application Proxy is working correctly:

- Open the Windows Services console and verify that the “Microsoft AAD Application Proxy Connector” service is enabled and running. You may also want to look at the Application Proxy service properties page, as shown in the following image:

![Microsoft AAD Application Proxy Connector Properties screenshot][1]

- Open Event Viewer and look for events related to the Application Proxy connector located under **Applications and Services Logs** > **Microsoft** > **AadApplicationProxy** > **Connector** > **Admin**.
- If needed, more detailed logs are available by turning on analytics and debugging logs and turning on the Application Proxy connector session log, as shown in the following image:

![Application Proxy Connector session log screenshot][2]


## General errors

Error | Description | Resolution
--- | --- | ---
This corporate app can’t be accessed. You are not authorized to access this application. Authorization failed. Make sure to assign the user with access to this application. | You may not have assigned the user for this application. | Go to the Application tab, and under Users and Groups, assign this user or user group to this application.
This corporate app can’t be accessed. You are not authorized to access this application. Authorization failed. Make sure that the user has a license for Azure Active Directory Premium or Basic. | Your user may get this error when trying to access the app you published if the user who tried to access the application was not explicitly assigned with a Premium/Basic license by the subscriber’s administrator. | Go to the subscriber’s Active Directory **Licenses** tab and make sure that this user or user group is assigned a Premium or Basic license.


## Connector troubleshooting
If registration fails during the Connector wizard installation, you can view the reason for the failure either by looking in the event log under **Windows Logs** > **Application**, or by running the following Windows PowerShell command.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Error | Description | Resolution |
| --- | --- | --- |
| Connector registration failed: Make sure you enabled Application Proxy in the Azure Management Portal and that you entered your Active Directory user name and password correctly. Error: 'One or more errors occurred.' | You closed the registration window without performing login to Azure AD. | Run the Connector wizard again and register the Connector. |
| Connector registration failed: Make sure you enabled Application Proxy in the Azure Management Portal and that you entered your Active Directory user name and password correctly. Error: 'AADSTS50001: Resource `https://proxy.cloudwebappproxy.net/registerapp` is disabled.' | Application Proxy is disabled.  | Make sure you enable Application Proxy in the Azure AD portal before trying to register the Connector. For more information on enabling Application Proxy, see [Enable Application Proxy services](active-directory-application-proxy-enable.md). |
| Connector registration failed: Make sure you enabled Application Proxy in the Azure Management Portal and that you entered your Active Directory user name and password correctly. Error: 'One or more errors occurred.' | If the registration window opens and then immediately closes without allowing you to log in, you will probably get this error. This error occurs when there is some sort of networking error on your system. | Make sure that it is possible to connect from a browser to a public website and that the ports are open as specified in [Application Proxy prerequisites](active-directory-application-proxy-enable.md). |
| Connector registration failed: Make sure your computer is connected to the Internet. Error: 'There was no endpoint listening at `https://connector.msappproxy.net:9090/register/RegisterConnector` that could accept the message. This is often caused by an incorrect address or SOAP action. See InnerException, if present, for more details.' | If you log in using your Azure AD username and password but then you receive this error, it may be that all ports above 8081 are blocked. | Make sure that the necessary ports are open. For more information see [Application Proxy prerequisites](active-directory-application-proxy-enable.md). |
| Clear error is presented in the registration window. Cannot proceed – only to close the window | You entered the wrong username or password. | Try again. |
| Connector registration failed: Make sure you enabled Application Proxy in the Azure Management Portal and that you entered your Active Directory user name and password correctly. Error: 'AADSTS50059: No tenant-identifying information found in either the request or implied by any provided credentials and search by service principle URI has failed. | You are trying to log in using a Microsoft Account and not a domain that is part of the organization ID of the directory you are trying to access. | Make sure that the admin is part of the same domain name as the tenant domain, for example, if the Azure AD domain is contoso.com, the admin should be admin@contoso.com. |
| Failed to retrieve the current execution policy for running PowerShell scripts | If the Connector installation fails, check to make sure that PowerShell executionpolicy is not disabled. | Open the Group Policy Editor. Go to Computer Configuration > Administrative Templates > Windows Components > Windows PowerShell and double click on Turn on Script Execution. This can be set to either Not Configured or Enabled. If set to Enabled, make sure that under Options, the Execution Policy is set to either **Allow local scripts and remote signed scripts** or to **Allow all scripts**. |
| Connector failed to download the configuration | The Connector’s client certificate, which is used for authentication, expired. This may also occur if you have the Connector installed behind a proxy. In this case the Connector cannot access the Internet and will not be able to provide applications to remote users. | Renew trust manually using the `Register-AppProxyConnector` cmdlet in Windows PowerShell. If your Connector is behind a proxy, it is necessary to grant Internet access to the Connector accounts “network services” and “local system”. This can be accomplished either by granting them access to the Proxy or by setting them to bypass the proxy. |
| Connector registration failed: Make sure you are a Global Administrator of your Active Directory to register the Connector. Error: 'The registration request was denied.' | The alias you're trying to log in with isn't an admin on this domain. Your Connector is always installed for the directory that owns the user’s domain. | Make sure that the admin you are trying to log in as has global permissions to the Azure AD tenant.|


## Kerberos errors

| Error | Description | Resolution |
| --- | --- | --- |
| Failed to retrieve the current execution policy for running PowerShell scripts | If the Connector installation fails, check to make sure that PowerShell executionpolicy is not disabled. | Open the Group Policy Editor. Go to Computer Configuration > Administrative Templates > Windows Components > Windows PowerShell and double click on Turn on Script Execution. This can be set to either Not Configured or Enabled. If set to Enabled, make sure that under Options, the Execution Policy is set to either **Allow local scripts and remote signed scripts** or to **Allow all scripts**. |
| 12008 - Azure AD exceeded the maximum number of permitted Kerberos authentication attempts to the backend server. | This event may indicate incorrect configuration between Azure AD and the backend application server, or a problem in time and date configuration on both machines. | The backend server declined the Kerberos ticket created by Azure AD. Verify that the configuration of the Azure AD and the backend application server are configured correctly. Make sure that the time and date configuration on the Azure AD and the backend application server are synchronized. |
| 13016 - Azure AD cannot retrieve a Kerberos ticket on behalf of the user because there is no UPN in the edge token or in the access cookie. | There is a problem with the STS configuration. | Fix the UPN claim configuration in the STS. |
| 13019 - Azure AD cannot retrieve a Kerberos ticket on behalf of the user because of the following general API error. | This event may indicate incorrect configuration between Azure AD and the domain controller server, or a problem in time and date configuration on both machines. | The domain controller declined the Kerberos ticket created by Azure AD. Verify that the configuration of the Azure AD and the backend application server are configured correctly, especially the SPN configuration. Make sure the Azure AD is domain joined to the same domain as the domain controller to ensure that the domain controller establishes trust with Azure AD. Make sure that the time and date configuration on the Azure AD and the domain controller are synchronized. |
| 13020 - Azure AD cannot retrieve a Kerberos ticket on behalf of the user because the backend server SPN is not defined. | This event may indicate incorrect configuration between Azure AD and the domain controller server, or a problem in time and date configuration on both machines. | The domain controller declined the Kerberos ticket created by Azure AD. Verify that the configuration of the Azure AD and the backend application server are configured correctly, especially the SPN configuration. Make sure the Azure AD is domain joined to the same domain as the domain controller to ensure that the domain controller establishes trust with Azure AD. Make sure that the time and date configuration on the Azure AD and the domain controller are synchronized. |
| 13022 - Azure AD cannot authenticate the user because the backend server responds to Kerberos authentication attempts with an HTTP 401 error. | This event may indicate incorrect configuration between Azure AD and the backend application server, or a problem in time and date configuration on both machines. | The backend server declined the Kerberos ticket created by Azure AD. Verify that the configuration of the Azure AD and the backend application server are configured correctly. Make sure that the time and date configuration on the Azure AD and the backend application server are synchronized. |
| The website cannot display the page. | Your user may get this error when trying to access the app you published if the application is an IWA application, the defined SPN for this application may be incorrect. | For IWA apps: Make sure that the SPN configured for this application is correct. |
| The website cannot display the page. | Your user may get this error when trying to access the app you published if the application is an OWA application, this could be caused by one of the following: | The steps to mitigate accordingly: |
| | The defined SPN for this application is incorrect. | Make sure that the SPN configured for this application is correct. |
| | The user who tried to access the application is using a Microsoft account rather than the proper corporate account to sign in, or the user is a guest user. | Make sure the user signs in using their corporate account that matches the domain of the published application. Microsoft Account users and guest cannot access IWA applications. |
| | The user who tried to access the application is not properly defined for this application on the on-prem side. | Make sure that this user has the proper permissions as defined for this backend application on the on-prem machine. |
| This corporate app can’t be accessed. You are not authorized to access this application. Authorization failed. Make sure to assign the user with access to this application. | Your user may get this error when trying to access the app you published if the user who tried to access the application is using a Microsoft Account rather than the proper corporate account to sign in, or the user is a guest user. | Microsoft Account users and guest cannot access IWA applications. Make sure the user signs in using their corporate account that matches the domain of the published application. |
| This corporate app can’t be accessed right now. Please try again later…The connector timed out. | Your user may get this error when trying to access the app you published if the user who tried to access the application is not properly defined for this application on the on-prem side. | Make sure that this user has the proper permissions as defined for this backend application on the on-prem machine. |


## See also

[Using Application Proxy to publish applications for secure remote access](active-directory-application-proxy-configure.md)

[Enable Application Proxy services](active-directory-application-proxy-enable.md)

<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/ConnectorProperties.jpg
[2]: ./media/active-directory-application-proxy-troubleshoot/SessionLog.jpg

test

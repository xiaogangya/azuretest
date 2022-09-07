
<properties 
	pageTitle="Using Azure AD Connect Health with AD FS | Microsoft Azure" 
	description="This is the Azure AD Connect Health page how to monitor your on-premises AD FS infrastructure." 
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
	ms.topic="get-started-article" 
	ms.date="08/14/2015" 
	ms.author="billmath"/>

# Using Azure AD Connect Health with AD FS 
The following documentation is specific to monitoring your AD FS infrastructure with Azure AD Connect Health.

## Alerts for AD FS
The Azure AD Connect Health Alerts section provides you the list of active alerts. Each alert includes relevant information, resolution steps, and links to related documentation. By selecting an active or resolved alert you will see a new blade with additional information, as well as steps you can take to resolve the alert, and links to additional documentation. You can also view historical data on alerts that were resolved in the past.

By selecting an alert you will be provided with additional information as well as steps you can take to resolve the alert and links to additional documentation.

![Azure AD Connect Health Portal](./media/active-directory-aadconnect-health/alert2.png)



## Usage Analytics for AD FS
Azure AD Connect Health Usage Analytics analyzes the authentication traffic of your federation servers. Selecting the usage analytics box will open the usage analytics blade, which will show you the metrics and groupings.

>[AZURE.NOTE] In order to use Usage Analytics with AD FS, you must ensure that AD FS auditing is enabled. For more information, see [Enable Auditing for AD FS](active-directory-aadconnect-health-operations.md#enable-auditing-for-ad-fs). 

![Azure AD Connect Health Portal](./media/active-directory-aadconnect-health/report1.png)

To select additional metrics, specify a time range, or to change the grouping, simply right-click on the usage analytics chart and select Edit Chart. Then you can specify the time range, change or select metrics and change the grouping. You can view the distribution of the authentication traffic based on different "metrics" and group each metric using relevant "group by" parameters described below.

| Metric | Group By | What the grouping means and why it's useful? |
| ------ | -------- | -------------------------------------------- |
| Total Requests: The total number of request processed by the federation service | All | This will show the count of total number of requests without grouping. |
|  | Application | This option will group the total requests based on the targeted relying party. This grouping is useful to understand which application is receiving how much percentage of the total traffic. |
|  | Server | This option will group the total requests based on the server that processed the request. This grouping is useful to understand the load distribution of the total traffic. |
|  | Workplace Join | This option will group the total requests based on if the requests are coming from devices that are workplace joined (known). This grouping is useful to understand if your resources are accessed using devices that are unknown to the identity infrastructure. |
|  | Authentication Method | This option will group the total requests based on the authentication method used for authentication. This grouping is useful to understand the common authentication method that gets used for authentication. Following are the possible authentication methods <ol> <li>Windows Integrated Authentication (Windows)</li> <li>Forms Based Authentication (Forms)</li> <li>SSO (Single Sign On)</li> <li>X509 Certificate Authentication (Certificate)</li> <br>Please note that a request is counted as SSO (Single Sign On) if the federation servers receive the request with an SSO Cookie. In such cases, if the cookie is valid, the user is not asked to provide credentials and gets seamless access to the application. This is common if you have multiple relying parties protected by the federation servers. |
|  | Network Location | This option will group the total requests based on the network location of the user. It can be either intranet or extranet. This grouping is useful to know what percentage of the traffic is coming from the intranet verses extranet. |
| Total Failed Requests: The total number failed requests processed by the federation service. <br> (This metric is only available on AD FS for Windows Server 2012 R2)| Error Type | This will show the number of errors based on predefined error types. This grouping is useful to understand the common types of errors. <ul><li>Incorrect Username or Password: Errors due to incorrect username or password.</li> <li>"Extranet Lockout": Failures due to the requests received from a user that was locked out from extranet </li><li> "Expired Password": Failures due to users logging in with an expired password.</li><li>"Disabled Account": Failures due to users logging with a disabled account.</li><li>"Device Authentication": Failures due to users failing to authenticate using Device Authentication.</li><li>"User Certificate Authentication": Failures due to users failing to authenticate because of an invalid certificate.</li><li>"MFA": Failures due to user failing to authenticate using Multi Factor Authentication.</li><li>"Other Credential": "Issuance Authorization": Failures due to authorization failures.</li><li>"Issuance Delegation": Failures due to issuance delegation errors.</li><li>"Token Acceptance": Failures due to ADFS rejecting the token from a 3rd party Identity Provider.</li><li>"Protocol": Failure due to protocol errors.</li><li>"Unknown": Catch all. Any other failures that do not fit into the defined categories.</li> |
|  | Server | This will group the errors based on the server. This is useful to understand the error distribution across servers. Uneven distribution could be an indicator of a server in a faulty state. |
|  | Network Location | This will group the errors based on the network location of the requests (intranet vs extranet). This is useful to understand the type of requests that are failing. |
|  | Application | This will group the failures based on the targeted application (relying party). This is useful to understand which targeted application is seeing most number of errors. |
| User Count: Average number of unique users active in the system | All | This provides a count of average number of users using the federation service in the selected time slice. The users are not grouped. <br>The average will depend on the time slice selected. |
|  | Application | This will group the average number of users based on the targeted application (relying party). This is useful to understand how many users are using which application. |


## Performance Monitoring for AD FS
Azure AD Connect Health Performance Monitoring provides monitoring information on metrics. By selecting the Monitoring box, a blade will open up that provides detailed information on the metrics.


![Azure AD Connect Health Portal](./media/active-directory-aadconnect-health/perf1.png)


By selecting the Filter option at the top of the blade, you can filter by server to see an individual server’s metrics. To change metrics, simply right-click on the monitoring chart under the monitoring blade and select Edit Chart. Then, from the new blade that opens up, you can select additional metrics from the drop-down and specify a time range for viewing the performance data.




## Related links

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Agent Installation for AD FS](active-directory-aadconnect-health-agent-install-adfs.md)
* [Azure AD Connect Health Operations](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health FAQ](active-directory-aadconnect-health-faq.md)

test

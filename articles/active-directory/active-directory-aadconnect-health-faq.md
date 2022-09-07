<properties 
	pageTitle="Azure AD Connect Health FAQ" 
	description="This FAQ answers questions about Azure AD Connect Health. This FAQ covers questions about using the service, including the billing model, capabilities, limitations, and support." 
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
	ms.date="08/14/2015"
	ms.author="billmath"/>


# Azure AD Connect Health Frequently Asked Questions (FAQ)

This FAQ answers questions about Azure AD Connect Health. This FAQ covers questions about using the service, including the billing model, capabilities, limitations, and support.

## General Questions



**Q: I have several tenants in Azure Active Directory. How do I switch to the one with Azure Active Directory Premium?**

You can switch the Azure AD Tenant by selecting “Home” on the left navigation bar, followed by, selecting the currently logged in User Name on the top right corner and choosing the right tenant account. If the Tenant account is not listed here, select Sign out and then use the global tenant admin credentials of the Azure Active Directory Premium Tenant to log in.


## Installation Questions



**Q: What is the impact of installing the Azure AD Connect Health Agent on individual servers?**

The impact of installing the Microsoft Identity Health Agent on the ADFS servers is minimal with respect to the CPU, Memory consumption network bandwidth and storage.

The numbers below are an approximation.

- CPU consumption: ~1% increase
- Memory consumption: Up to 10 % of the total system memory
- Network Bandwidth Usage: ~1 MB / 1000 ADFS requests
>[AZURE.NOTE]In the event of the agent being unable to communicate to Azure, the agent will store the data locally, up to a maximum limit of 10 % of the total system memory.Once the agent reaches 10% of the total physical memory, if the agent has not been able to upload the data to the service, the new ADFS transactions will overwrite any “cached” transactions on a “least recently serviced” basis.

- Local buffer storage for AD Health Agent: ~20 MB
- Data Storage required for Audit Channel


It is recommended that you provision a disk space of 1024 MB (1 GB) for the AD FS Audit Channel for AD Health Agents to process all the data.

**Q: Will I have to reboot my servers during the installation of the Azure AD Connect Health Agents?**

No. The installation of the agents will not require you to reboot the server. However, installation of some of the prerequisite steps may require a reboot of the server.

For example, on Windows Server 2008 R2 the installation of .Net 4.5 Framework requires a server reboot.


**Q: Does Azure AD Connect Health Services work through a pass-through http proxy?**

Yes, both the registration process and normal operation can function through an explicit proxy set up to forward outbound http requests. “Netsh WinHttp set Proxy” does not work in this case since the agent uses System.Net to make web requests instead of Microsoft Windows HTTP Services. 

Perform any time prior to running Register-AdHealthAgent (The final step of Installation)


- Step 1 – Add entry to the machine.config file


Locate the machine.config file. The file is located in%windir%\Microsoft.NET\Framework64\[version]\config\machine.config</li>

Add the following entry under the <configuration></configuration> element in your machine.config file.
		
	<system.net>  
			<defaultProxy useDefaultCredentials="true">
       		<proxy 
        usesystemdefault="true" 
        proxyaddress="http://YOUR.PROXY.HERE.com"  
        bypassonlocal="true"/>
		</defaultProxy>
	</system.net> 

 

Additional <defaultProxy> information can be found [here](https://msdn.microsoft.com/library/kd3cf2ex(v=vs.110)).

This settings configures .NET applications system-wide to use your explicitly defined proxy when makeing http .NET requests. Modifying each individual app.config is not recommended because it will be undone during auto update. You only have to change one file and it will persist through updates if you only modify machine.config.

- Step 2 - Configure Proxy in Internet Options

Open Internet Explorer -> Settings -> Internet Options -> Connections -> LAN Settings.

Select Use a Proxy Server for you LAN

Select Advanced IF you have different proxy ports for HTTP and HTTPS/Secure




**Q: Does Azure AD Connect Health Services support basic authentication when connecting to Http Proxies?**

No. A mechanism for specifying arbitrary username/password for Basic Authentication is not currently supported.





## Operations Questions



**Q: Do I need to enable auditing on my AD FS Application Proxy Servers or my Web Application Proxy Servers?**

No, auditing does not need to be enabled on AD FS Application Proxy Servers or Web Application Proxy Servers. It only needs to be enabled on the AD FS federated servers.



**Q: How do Azure AD Connect Health Alerts get resolved?**

Azure AD Connect Health Alerts get resolved on a success condition. Azure AD Connect Health Agents detect and report the success conditions to the service on a periodic basis. For a few alerts, the suppression is time based. That is if the same error condition is not observed within 48 hours from alert generation, the alert is automatically resolved. 




**Q: What firewall ports do I need to open for the Azure AD Connect Health Agent to work?**

You will need to have TCP/UDP ports 80 and 443 open for the Azure AD Connect Health Agent to be able to communicate with the Azure AD Health service endpoints.

## Related links

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Agent Installation for AD FS](active-directory-aadconnect-health-agent-install-adfs.md)
* [Using Azure AD Connect Health with AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health Operations](active-directory-aadconnect-health-operations.md)
test

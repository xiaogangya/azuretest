<properties 
 pageTitle="About virtual machine extensions and features | Microsoft Azure" 
 description="Describes many of the virtual machine extensions, grouped by what they provide or improve, such as connectivity and basic management." 
 services="virtual-machines" 
 documentationCenter="" 
 authors="squillace" 
 manager="timlt" 
 editor=""/>
<tags 
 ms.service="virtual-machines" 
 ms.devlang="na" 
 ms.topic="article" 
 ms.tgt_pltfrm="vm-multiple" 
 ms.workload="infrastructure-services"
 ms.date="08/25/2015" 
 ms.author="rasquill"/>
#About virtual machine extensions and features
Microsoft Azure provides VM Extensions built by both Microsoft and by trusted third-party providers to enable security, runtime, debugging, management, and other features you can take advantage of to increase your productivity with Azure Virtual Machines. This topic describes the various features that Azure VM Extensions provide to both Windows and Linux Virtual Machines for your use and points toward documentation for each one.

For details about the VM Agents and how they work to support VM Extensions, see [VM Agent and VM Extensions Overview](https://msdn.microsoft.com/library/dn832621.aspx).

##Azure VM Extensions

VM Extensions implement most of the critical functionality that you want to use with your VMs, including basic functionality like resetting passwords, configuring RDP, and many, many others. Because new extensions are added all the time, the number of possible features your VMs support in Azure continues to increase. By default, several basic VM extensions are installed when you create your VM from the Image Gallery, including **IaaSDiagnostics** (currently Windows VMs only), **VMAccess**, and **BGInfo** (also currently Windows only). However, not all extensions are implemented on both Windows and Linux at any specific time, due to the constant flow of feature updates and new extensions.

##Connectivity and Basic Management

The following extensions are critical to enabling, re-enabling, or disabling basic connectivity with your VMs once they are created and running.

|VM Extension Name|Feature Description|More Information
|---|---|---|
|VMAccessAgent (Windows)|Create, update, and reset user information and RDP and SSH connection configurations.|[Windows](https://msdn.microsoft.com/library/dn606308.aspx)
|VMAccessForLinux (Linux)|Create, update, and reset user information and RDP and SSH connection configurations.|[Linux](http://azure.microsoft.com/blog/2014/08/25/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/)

##Deployment and Configuration Management

The following extensions support different kinds of deployment and configuration management scenarios and features. See also the section on VM Operations and Management, below.

|VM Extension Name|Feature Description|More Information|
|---|---|---|
|**MSEnterpriseApplication**|Implements features for support by Windows System Center.|[System Center 2012 R2 Virtual Machine Roles](http://social.technet.microsoft.com/wiki/contents/articles/18274.system-center-2012-r2-virtual-machine-role-authoring-guide-resource-extension-package.aspx)|
|**Octopus Deploy** (DSC Extension-based)|Supports automated deployment of ASP.NET web applications and Windows Services into development, test, and production environments.|[Getting Started with Octopus Deploy](http://docs.octopusdeploy.com/display/OD/Getting%20started)|
|**Visual Studio Release Manager** (DSC Extension-based)|Supports continuous deployment with Visual Studio.|[Automate deployments with Release Management](https://msdn.microsoft.com/library/dn217874.aspx)|
|**CentosChefClient**|||
|**ChefClient**|Creates a Chef client on Windows. (Can also use the DSC extension, below.)|[Chef and Microsoft Azure](https://www.getchef.com/solutions/azure/)|
|**LinuxChefClient**|||
|**DockerExtension**|Installs the Docker daemon to support remote Docker commands.|[How to Use the Docker Virtual Machine Extension](virtual-machines-docker-vm-extension.md)For more extensive information, see the [Docker VM Extension User Guide](https://github.com/Azure/azure-docker-extension/blob/master/README.md)|
|**DSC**|PowerShell DSC (Desired State Configuration) extension.|[Azure PowerShell DSC (Desired State Configuration) extension](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx)|
|**PuppetEnterpriseAgent**|Implements the features of Puppet Enterprise. |[Puppet on Azure](http://puppetlabs.com/solutions/microsoft)|
|**CustomScriptExtension** (Windows)**CustomScriptForLinux** (Linux)|Invokes custom scripts on the VM at any time: startup or during lifetime.|[Custom Script Extension](https://msdn.microsoft.com/library/dn781373.aspx)[Linux](http://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)|
|**AzureCATExtensionHandler**|Consumes the diagnostic data collected by **IaaSDiagnostics** and few other data sources such as [Azure Storage Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343270.aspx) and transforms it into an aggregated data set appropriate for SAP Host control process to consume|[Azure Enhanced Monitoring for SAP](http://azure.microsoft.com/blog/2014/06/04/azure-enhanced-monitoring-for-sap/)|

##Security and Protection

The extensions in this section provide critical security features for your Azure VMs.

|VM Extension Name|Feature Description|More Information|
|---|---|---|
|**CloudLinkSecureVMWindowsAgent**|Provides Microsoft Azure customers with the capability to encrypt their virtual machine data on a multi-tenant shared infrastructure and fully control of the encryption keys for their encrypted data on Azure storage infrastructure.|[Securing Microsoft Azure Virtual Machines leveraging BitLocker and Native OS encryption](http://www.cloudlinktech.com/azure)|
|**McAfeeEndpointSecurity**|Protects your VM against malicious software.|[McAfee](http://www.mcafeeasap.com/)|
|**TrendMicroDSA**|Enables TrendMicro’s Deep Security platform support to provide intrusion detection and prevention, firewall, anti-malware, web reputation, log inspection, and integrity monitoring.|[How to install and configure Trend Micro Deep Security as a Service on an Azure VM](http://virtual-machines-install-trend.md)|
|**PortalProtectExtension**|Guards against threats to your Microsoft SharePoint environment.|[Securing Your SharePoint Deployment on Azure](http://blog.trendmicro.com/securing-sharepoint-deployment-azure/)|
|**IaaSAntimalware**|Microsoft Antimalware for Azure Cloud Services and Virtual Machines is a real-time protection capability that helps identify and remove viruses, spyware, and other malicious software, with configurable alerts when known malicious or unwanted software attempts to install itself or run on your system.|[Download antimalware documentation](http://go.microsoft.com/fwlink/?linkid=398023&clcid=0x409)|
|**SymantecEndpointProtection**|Symantec Endpoint Protection 12.1.4 enables security and performance across physical and virtual systems.|[How to install and configure Symantec Endpoint Protection on an Azure VM](http://virtual-machines-install-symantec.md)

##VM Operations and Management

Supports common operations management features and behavior. See also the section on Deployment and Configuration Management, above.

|**VM Extension Name**|Feature Description|More Information|
|---|---|---|
|**AzureVmLogCollector**|You can use the **AzureVMLogCollector** extension on-demand to perfom one-time collection of logs from one or more Cloud Service VMs (from both web roles and worker roles) and transfer the collected files to an Azure storage account – all without remotely logging on to any of the VMs. |[AzureLogCollector Extension](https://msdn.microsoft.com/library/dn927183.aspx)|
|**IaaSDiagnostics**|Enables, disables, and configures Azure Diagnostics, and is also used by the **AzureCATExtensionHandler** to support SAP monitoring.|[Microsoft Azure Virtual Machine Monitoring with Azure Diagnostics Extension](http://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/)|
|**OSPatchingForLinux**|Enables the Azure VM administrators to automate the VM OS updates with the customized configurations. You can use the OSPatching extension to configure OS updates for your virtual machines, including: Specify how often and when to install OS patches, Specify what patches to install, and Configure the reboot behavior after updates|[OS Patching Extension Blog Post](http://azure.microsoft.com/blog/2014/10/23/automate-linux-vm-os-updates-using-ospatching-extension/). See also the readme and source on Github at [OS Patching Extension](https://github.com/Azure/azure-linux-extensions).|

##Developing and Debugging

These VM extensions are listed here for completeness, as they provide support for Visual Studio-related features and are not intended to be used directly.

|VM Extension Name|Feature Description|More Information|
|---|---|---|
|**VS14CTPDebugger**|Supports remote debugging from VS using the Azure SDK 2.4|[Remote Debugging in Visual Studio](https://msdn.microsoft.com/library/y7f5zaaa.aspx)|
|**VS2013Debugger**|Supports remote debugging from VS using the Azure SDK 2.4||
|**VS2012Debugger**|Supports remote debugging from VS using the Azure SDK 2.4||
|**RemoteDebugVS14CTP**|Supports remote debugging from VS using the Azure SDK 2.3||
|**RemoteDebugVS2013**|Supports remote debugging from VS using the Azure SDK 2.3||
|**RemoteDebugVS2012**|Supports remote debugging from VS using the Azure SDK 2.3||
|**WebDeployForVSDevTest**|Installs and configures IIS and Web Deploy on Windows Server. Removing or disabling it is not supported.|

##Miscellaneous Features

These extensions provide support for other VM features that might be useful.

|VM Extension Name|Feature Description|More Information|
|---|---|---|
|**BGInfo**|Presents a consolidated picture of useful server information on the desktop when using RDP.|[BGInfo Extension](https://msdn.microsoft.com/library/dn606289.aspx)|
|**HpcVmDrivers**|Installs, configures, and maintains the following network device drivers on a size A8 or A9 virtual machine so that the VM can access the Azure remote direct-memory access (RDMA) network.|[HpcVmDrivers Extension](https://msdn.microsoft.com/library/dn690126.aspx)

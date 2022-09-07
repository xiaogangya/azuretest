<properties 
    pageTitle="What is a Cloud Service Model and Package in Azure" 
    description="Describes the cloud service model (.csdef, .cscfg) and package (.cspkg) in Azure" 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>
<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2015" 
    ms.author="adegeo"/>

# What is the Cloud Service Model and how do I package it?
A cloud service is created from three components, the service definition _(.csdef)_, the service config _(.cscfg)_, and a service package _(.cspkg)_. Both the **ServiceDefinition.csdef** and **ServiceConfig.cscfg** files are XML-based and describe the structure of the cloud service and how it's configured; collectively called the model. The **ServicePackage.cspkg** is a zip file that is generated from the **ServiceDefinition.csdef** and among other things, contains all of the required binary-based dependencies. Azure creates a cloud service from both the **ServicePackage.cspkg** and the **ServiceConfig.cscfg**.

Once the cloud service is running in Azure, you can reconfigure it through the **ServiceConfig.cscfg** file, but you cannot alter the definition.

## What would you like to know more about?

* I want to know more about the [ServiceDefinition.csdef](#csdef) and [ServiceConfig.cscfg](#cscfg) files.
* I already know about that, give me [some examples](#next-steps) on what I can configure.
* I want to create the [ServicePackage.cspkg](#cspkg). 
* I am using Visual Studio and I want to...
    * [Create a new cloud service][vs_create]
    * [Reconfigure an existing cloud service][vs_reconfigure]
    * [Deploy a Cloud Service project][vs_deploy]
    * [Remote desktop into a cloud service instance][remotedesktop]

<a name="csdef"></a>
## ServiceDefinition.csdef
The **ServiceDefinition.csdef** file specifies the settings that are used by Azure to configure a cloud service. The [Azure Service Definition Schema (.csdef File)](https://msdn.microsoft.com/library/azure/ee758711.aspx) provides the allowable format for a service definition file. The following example shows the settings that can be defined for the Web and Worker roles:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

You can refer to the [Service Definition Schema][] for a better understanding of the XML schema used here, however, here is a quick explanation of some of the elements:

>**Sites**  
>Contains the definitions for websites or web applications that are hosted in IIS7.
>
>**InputEndpoints**  
>Contains the definitions for endpoints that are used to contact the cloud service.
>
>**InternalEndpoints**  
>Contains the definitions for endpoints that are used by role instances to communicate with each other.
>
>**ConfigurationSettings**  
>Contains the setting definitions for features of a specific role.
>
>**Certificates**  
>Contains the definitions for certificates that are needed for a role. The previous code example shows a certificate that is used for the configuration of Azure Connect.
>
>**LocalResources**  
>Contains the definitions for local storage resources. A local storage resource is a reserved directory in the file system of the virtual machine in which an instance of a role is running.
>
>**Imports**  
>Contains the definitions for imported modules. The previous code example shows the modules for Remote Desktop Connection and Azure Connect.
>
>**Startup**  
>Contains tasks that are run when the role starts. The tasks are defined in a .cmd or executable file.



<a name="cscfg"></a>
## ServiceConfiguration.cscfg
The configuration of the settings for your cloud service is determined by the values in the **ServiceConfiguration.cscfg** file. You specify the number of instances that you want to deploy for each role in this file. The values for the configuration settings that you defined in the service definition file are added to the service configuration file. The thumbprints for any management certificates that are associated with the cloud service are also added to the file. The [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx) provides the allowable format for a service configuration file.

The service configuration file is not packaged with the application, but is uploaded to Azure as a separate file and is used to configure the cloud service. You can upload a new service configuration file without redeploying your cloud service. The configuration values for the cloud service can be changed while the cloud service is running. The following example shows the configuration settings that can be defined for the Web and Worker roles:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

You can refer to the [Service Configuration Schema](https://msdn.microsoft.com/library/azure/ee758710.aspx) for better understanding the XML schema used here, however, here is a quick explanation of the elements:

>**Instances**  
>Configures the number of running instances for the role. To prevent your cloud service from potentially becoming unavailable during upgrades, it is recommend that you deploy more than one instance of your web-facing roles. By doing this, you are adhering to the guidelines in the [Azure Compute Service Level Agreement (SLA)](http://azure.microsoft.com/support/legal/sla/), which guarantees 99.95% external connectivity for Internet-facing roles when two or more role instances are deployed for a service. 

>**ConfigurationSettings**  
>Configures the settings for the running instances for a role. The name of the `<Setting>` elements must match the setting definitions in the service definition file.

>**Certificates**  
>Configures the certificates that are used by the service. The previous code example shows how to define the certificate for the RemoteAccess module. The value of the *thumbprint* attribute must be set to the thumbprint of the certificate to use.

<p/>

 >[AZURE.NOTE] The thumbprint for the certificate can be added to the configuration file by using a text editor, or the value can be added on the **Certificates** tab of the **Properties** page of the role in Visual Studio.



## Defining ports for role instances
Azure allows only one entry point to a web role. This means that all traffic occurs through one IP address. You can configure your websites to share a port by configuring the host header to direct the request to the correct location. You can also configure your applications to listen to well-known ports on the IP address.

The following sample shows the configuration for a web role with a website and web application. The website is configured as the default entry location on port 80, and the web applications are configured to receive requests from an alternate host header that is called “mail.mysite.cloudapp.net”.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostheader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## Changing the configuration of a role
You can update the configuration of your cloud service while it is running in Azure, without taking the service offline. To change configuration information, you can either upload a new configuration file, or edit the configuration file in place and apply it to your running service. The following changes can be made to the configuration of a service:

- **Changing the values of configuration settings**  
When a configuration setting changes, a role instance can choose to apply the change while the instance is online, or to recycle the instance gracefully and apply the change while the instance is offline.

- **Changing the service topology of role instances**  
Topology changes do not affect running instances, except where an instance is being removed. All remaining instances generally do not need to be recycled; however, you can choose to recycle role instances in response to a topology change.

- **Changing the certificate thumbprint**  
You can only update a certificate when a role instance is offline. If a certificate is added, deleted, or changed while a role instance is online, Azure gracefully takes the instance offline to update the certificate and bring it back online after the change is complete.

### Handling configuration changes with Service Runtime Events
The [Azure Runtime Library](https://msdn.microsoft.com/library/azure/dn511024.aspx) includes the [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) namespace, which provides classes for interacting with the Azure environment from code running in an instance of a role. The [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) class defines the following events that are raised before and after a configuration change:

- **[Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) event**  
This occurs before the configuration change is applied to a specified instance of a role giving you a chance to take down the role instances if required.
- **[Changed](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) event**  
Occurs after the configuration change is applied to a specified instance of a role.

> [AZURE.NOTE] Because certificate changes always take the instances of a role offline, they do not raise the RoleEnvironment.Changing or RoleEnvironment.Changed events.

<a name="cspkg"></a>
## ServicePackage.cspkg
To deploy an application as a cloud service in Azure, you must first package the application in the appropriate format. You can use the **CSPack** command-line tool (installed with the [Azure SDK](http://azure.microsoft.com/downloads/)) to create the package file as an alternative to Visual Studio. 

**CSPack** uses the contents of the service definition file and service configuration file to define the contents of the package. **CSPack** generates an application package file (.cspkg) that you can upload to Azure by using the [Azure Management Portal](cloud-services-how-to-create-deploy/#how-to-deploy-a-cloud-service). By default, the package is named `[ServiceDefinitionFileName].cspkg`, but you can specify a different name by using the `/out` option of **CSPack**.

###### Location of the CSPack tool (on windows)
| SDK Version | Path |
| ----------- | ---- |
| 1.7+        | C:\\Program Files\\Microsoft SDKs\\Azure\\.NET SDK\\\[sdk-version\]\\bin\\ |
| &lt;1.6        | C:\\Program Files\\Azure SDK\\\[sdk-version\]\\bin\\ |

>[AZURE.NOTE]
CSPack.exe (on windows) is available by running the **Microsoft Azure Command Prompt** shortcut that is installed with the SDK.  
>  
>Run the CSPack.exe program by itself to see documentation about all of the possible switches and commands.

<p />

>[AZURE.TIP]
Run your cloud service locally in the **Microsoft Azure Compute Emulator**, use the **/copyonly** option This option copies the binary files for the application to a directory layout from which they can be run in the compute emulator.

### Example command to package a cloud service
The following example creates an application package that contains the information for a web role. The command specifies the service definition file to use, the directory where binary files can be found, and the name of the package file.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

If the application contains both a web role and a worker role, the following command is used:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Where the variables are defined as follows:

| Variable                  | Value |
| ------------------------- | ----- |
| \[DirectoryName\]         | The subdirectory under the root project directory that contains the .csdef file of the Azure project.|
| \[ServiceDefinition\]     | The name of the service definition file. By default, this file is named ServiceDefinition.csdef.  |
| \[OutputFileName\]        | The name for the generated package file. Typically, this is set to the name of the application. If no file name is specified, the application package is created as \[ApplicationName\].cspkg.|
| \[RoleName\]              | The name of the role as defined in the service definition file.|
| \[RoleBinariesDirectory] | The location of the binary files for the role.|
| \[VirtualPath\]           | The physical directories for each virtual path defined in the Sites section of the service definition.|
| \[PhysicalPath\]          | The physical directories of the contents for each virtual path defined in the site node of the service definition.|
| \[RoleAssemblyName\]      | The name of the binary file for the role.| 


## Next steps

I'm creating a cloud service package and I want to...

<!--
* [Configure Sizes for Cloud Services](!!!!!https://msdn.microsoft.com/library/azure/ee814754.aspx)  
* [Configure Local Storage Resources](!!!!!https://msdn.microsoft.com/library/azure/ee758708.aspx)
-->

* [Setup remote desktop for a cloud service instance][remotedesktop]
* [Deploy a Cloud Service project][deploy]

I am using Visual Studio and I want to...

* [Create a new cloud service][vs_create]
* [Reconfigure an existing cloud service][vs_reconfigure]
* [Deploy a Cloud Service project][vs_deploy]
* [Setup remote desktop for a cloud service instance][vs_remote]


[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: https://msdn.microsoft.com/en-us/library/gg443832.aspx
[vs_deploy]: https://msdn.microsoft.com/en-us/library/ee460772.aspx
[vs_reconfigure]: https://msdn.microsoft.com/library/ee405486.aspx
[vs_create]: https://msdn.microsoft.com/en-us/library/ee405487.aspx
test

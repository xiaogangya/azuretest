<properties
 pageTitle="Get started with an HPC Pack cluster to run Excel and SOA workloads | Microsoft Azure"
 description="."
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/18/2015"
 ms.author="danlep"/>

# Get started with an HPC Pack cluster in Azure to run Excel and SOA workloads

This article shows you how to deploy an HPC Pack cluster on Azure infrastructure services (IaaS) using an Azure quickstart template or an Azure PowerShell deployment script. You'll use Azure Marketplace VM images designed to run Microsoft Excel or service-oriented architecture (SOA) workloads with HPC Pack. You can use the cluster to run simple Excel HPC and SOA services from an on-premises client computer. The Excel HPC services include Excel workbook offloading and Excel user-defined functions, or UDFs.

At a high level the following diagram shows the HPC Pack cluster you'll create.

![HPC cluster with nodes running Excel workloads][scenario]

## Prerequisites

* **Client computer** - You'll need a Windows-based client computer to run the Azure PowerShell cluster deployment script (if you choose that deployment method) and to submit sample Excel and SOA jobs to the cluster.

* **Azure subscription** - If you don't have an account, you can create a free trial account in just a couple of minutes. For details, see [Azure Free Trial](http://azure.microsoft.com/pricing/free-trial/).

* **Cores quota** - You might need to increase the quota of cores, especially if you choose to deploy several cluster nodes with multicore VM sizes. If you are using an Azure quickstart template, be aware that the cores quota in Resource Manager is per Azure region, and you might need to increase the quota in a specific region. See [Azure subscription limits, quotas, and constraints](../azure-subscription-service-limits.md). To increase a quota, you can [open an online customer support request](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) at no charge.


## Step 1. Set up an HPC Pack cluster in Azure

We'll show you two ways to set up the cluster: first, using an Azure quickstart template and the Azure Preview Portal; and second, using an Azure PowerShell deployment script.


### Use a quickstart template
Use an Azure quickstart template to quickly and easily deploy an HPC Pack cluster in the Azure Preview portal. When you open the template in the portal, you get a simple UI where you enter the settings for your cluster. Here are the steps.

1. Visit the [Create HPC Cluster template page on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). If you want, review information about the template and the source code.

2. Click **Deploy to Azure** to start a deployment with the template in the Azure Preview portal.

    ![Deploy template to Azure][github]

3. In the portal, follow these steps to enter the parameters for the HPC cluster template.

    a. On the **Edit Template** page, click **Save**.

    ![Save the template][template]

    b. On the **Parameters** page, enter values for the template parameters. (Click the icon next to each setting for help information.) Sample values are shown in the following screen. This example will create a new HPC Pack cluster named *hpc01* in the *hpc.local* domain consisting of a head node and 2 compute nodes. The compute nodes will be created from an HPC Pack VM image including Microsoft Excel.

    ![Enter parameters][parameters]

    >[AZURE.NOTE]The head node VM will be created automatically from the [latest Marketplace  image](http://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) of HPC Pack 2012 R2 on Windows Server 2012 R2. Currently the image is based on HPC Pack 2012 R2 Update 2.
    >
    >Compute node VMs will be created from the latest image of the selected compute node family. Select the **ComputeNode** option for the latest HPC Pack 2012 R2 Update 2 compute image for general purposes. Select **ComputeNodeWithExcel** option for the latest HPC Pack compute node image that includes an evaluation version of Microsoft Excel Professional Plus 2013. If you want to deploy a cluster for general SOA sessions or for Excel UDF offloading, choose the **ComputeNode** option (without Excel installed).
    >
    >When using  **ComputeNodeWithExcel** for production workloads, you'll need to provide a valid Excel license to activate Excel on the compute nodes. Otherwise, the evaluation version of Excel could be expired within 30 days, and running the Excel workbook would constantly fail with the COMExeption (0x800AC472). If this happens, you may log on the head node to clusrun “%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe” on all Excel compute nodes via HPC Cluster Manager console to rearm Excel for another 30 days of evaluation time. The max rearm time for the grace period is 2, after that you may need to provide a valid Excel license.

    c. Choose the subscription.

    d. Create a new resource group for the cluster, such as *hpc01RG*.

    e. Choose a location for the resource group, such as East US.

    f. On the **Legal terms** page, review the terms. If you agree, click **Buy**.

    g. When you are finished setting the values for the template, click **Create** .

    ![Create the cluster][create]

3.	When the deployment completes (it typically takes around 30 minutes), export the cluster certificate file from the cluster head node. In a later step this public certificate will be imported on the client computer to provide the server-side authentication for secure HTTP binding.

    a. Connect to the head node by Remote Desktop from the Azure Preview portal.

     ![Connect to the head node][connect]

    b. Use standard procedures to use Certificate Manager to export the head node certificate (located under Cert:\LocalMachine\My) without the private key. In this example, export *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Export the certificate][cert]

### Use the HPC Pack IaaS Deployment script

The HPC Pack IaaS deployment script provides another versatile way to deploy an HPC Pack cluster. It runs in Azure Service Management (ASM) mode, whereas the template runs in Azure Resource Manager (ARM) mode. Also, the script is compatible with a subscription in either the Azure Global or Azure China service.

**Additional prerequisites**

* **Azure PowerShell** - [Install and configure Azure PowerShell](../powershell-install-configure.md) (version 0.8.10 or later) on your client computer.

* **HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Check the version of the script by running `New-HPCIaaSCluster.ps1 –Version`. This article is based on version 4.4.0 or later of the script.

**Create the configuration file**

 The HPC Pack IaaS deployment script uses an XML configuration file as input which describes the infrastructure of the HPC cluster. To deploy a cluster consisting of a head node and 18 compute nodes created from the compute node image that includes Microsoft Excel, substitute values for your environment into the following sample configuration file. For more information about the configuration file, see the Manual.rtf file in the script folder or the [script documentation](https://msdn.microsoft.com/library/azure/dn864734.aspx).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
<PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName HPCPackInstalled="true">96316178b0644ae08bc4e037635ce104__HPC-Pack-2012R2-Update2-CN-Excel-4.4.4864.0-WS2012R2-ENU</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Notes about the configuration file**

* The **VMName** of the head node must be exactly the same as the **ServiceName**.

* Make sure you specify **EnableWebPortal** so that the head node certificate is generated and exported.

* The post-configuration PowerShell script PostConfig.ps1 configures certain settings on the head node such as setting up the Azure storage connection string, removing the compute node role from the head node, and bringing all nodes online when they are deployed. A sample script follows.

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Run the script**

1. Open the PowerShell console on the client computer as an administrator.

2. Change directory to the script folder (E:\IaaSClusterScript in this example).

    ```
    cd E:\IaaSClusterScript
```

4. Run the command below to deploy the HPC Pack cluster. This example assumes that the configuration file is located in E:\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
```

The HPC Pack deployment script will run for some time. One thing the script wil do is to export and download the cluster certificate and save it in the current user’s Documents folder on the client computer. The script will generate a message similar to the following. In a following step you'll import the certificate in the appropriate certificate store.

```
You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
 C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_2015070716
2011.cer
```

## Step 2. Offload Excel workbooks and run UDFs from an on-premises client

### Offload Excel workbooks

Follow these steps to offload an Excel workbook to run on the HPC Pack cluster in Azure. To do this, you must have Excel 2010 or 2013 already installed on the client computer.

1. Use one of the methods in Step 1 to deploy an HPC Pack cluster with the Excel compute node image. Obtain the cluster certificate file (.cer) and cluster username and password.

2. On the client computer, import the cluster certificate under Cert:\CurrentUser\Root.

3. Make sure Excel is installed. Create an Excel.exe.config file with the following contents in the same folder with Excel.exe on the client computer. This ensures that the HPC Pack 2012 R2 Excel COM add-in will load successfully.

    ```
<?xml version="1.0"?>
<configuration>
  <startup useLegacyV2RuntimeActivationPolicy="true">
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
  </startup>
</configuration>
```
4.	Download the full [HPC Pack 2012 R2 Update 2 installation](http://www.microsoft.com/download/details.aspx?id=47755) and install the HPC Pack client,
or download and install the [HPC Pack 2012 R2 Update 2 client utilities](https://www.microsoft.com/download/details.aspx?id=47754) and the appropriate Visual C++ 2010 redistributable for your computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.	In this example, we use a sample Excel workbook named ConvertiblePricing_Complete.xlsb, available for download [here](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.	Copy the Excel workbook to a working folder such as D:\Excel\Run.

7.	Open the Excel workbook. On the **Develop** ribbon, click **COM Add-Ins** and confirm that the HPC Pack Excel COM add-in is loaded successfully as shown in the following screen.

    ![Excel add-in for HPC Pack][addin]

8.	Edit the VBA macro HPCControlMacros in Excel by changing the commented lines as shown in the following script. Substitute appropriate values for your environment.

    ![Excel macro for HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.	Copy the Excel work book to an upload directory such as D:\Excel\Upload, as specified in the HPC_DependsFiles constant in the VBA macro.

10.	Click the **Cluster** button on the worksheet to run the workbook on the Azure IaaS cluster.

### Run Excel UDFs

To run Excel UDFs, follow the preceding steps 1 – 3 to set up the client computer. For Excel UDFs, you don't need to have the Excel application installed on compute nodes, so you could choose a normal compute node image in Step 1 instead of the compute node image with Excel.

>[AZURE.NOTE] There is a 34 character limit in the Excel 2010 and 2013 cluster connector dialog box. If the full cluster name is longer, e.g. hpcexcelhn01.southeastasia.cloudapp.azure.com, it won't fit in the dialog box. The workaround is to apply the Update 2 QFE KB3085833 (download [here](http://www.microsoft.com/en-us/download/details.aspx?id=48725)) for SOA Session API on the client machine, then set a machine wide variable e.g. *CCP_IAASHN* with the value of the long cluster name and input *%CCP_IAASHN%* in the dialog box as the cluster head node name.

After the cluster is successfully deployed, continue with the following steps to run a sample built-in Excel UDF. For customized Excel UDFs, see these [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) to build the XLLs and deploy them on the IaaS cluster.

1.	Open a new Excel workbook. On the **Develop** ribbon, click **Add-Ins**. Then, in the dialog box, click **Browse**, navigate to the %CCP_HOME%Bin\XLL32 folder, and select the sample ClusterUDF32.xll.

    ![Select the UDF][udf]

2.	Click **File** > **Options** > **Advanced**. Under **Formulas** check **Allow user-defined XLL functions to run a compute cluster**. Then click **Options** and enter the full cluster name in **Cluster head node name**. (As noted previously this input box is limited to 34 characters, so a long cluster name may not fit. You can configure a shorter full name when you deploy a clusters via the IaaS deployment script.)

    ![Configure the UDF][options]

3.	Click the cell with value =XllGetComputerNameC() and press Enter to run the UDF calculation on the IaaS cluster. The function will simply retrieve the name of the compute node on which the UDF runs. For the first run, a credentials dialog box prompts for the username and password to connect to the IaaS cluster.

    ![Run UDF][run]

    When there are a lot of cells to calculate, press Alt-Shift-Ctrl + F9 to run the calculation on all cells.

## Step 3. Run a SOA workload from an on-premises client

To run general SOA applications on the HPC Pack IaaS cluster, first use one of the methods in Step 1 to deploy the IaaS cluster, using a generic compute node image (because you will not need Excel on the compute nodes). Then follow these steps.

1. After retrieving the cluster certificate, import it on the client computer under Cert:\CurrentUser\Root.

2. Install the [HPC Pack 2012 R2 Update 2 SDK](http://www.microsoft.com/download/details.aspx?id=47756) and [HPC Pack 2012 R2 Update 2 client utilities](https://www.microsoft.com/download/details.aspx?id=47754) so you can develop and run SOA client applications.

3. Download the HellowWorldR2 [sample code](https://www.microsoft.com/download/details.aspx?id=41633). Open the HelloWorldR2.sln in Visual Studio 2010 or 2012.

4. Build the EchoService project first and deploy the service to the IaaS cluster in the same way you deploy to an on-premises cluster. For detailed steps, see the Readme.doc in HelloWordR2. Modify and build the HellowWorldR2 and other projects as described below to generate the SOA client applications running on an Azure IaaS cluster from an on-premises client computer.

### Use Http binding with Azure storage queue

To use Http binding with an Azure storage queue, make a few changes to the sample code.

* Update the cluster name.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Optionally, use default TransportScheme in SessionStartInfo or explicitly set it to Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Use default binding for the BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Or set explicitly using the basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Optionally, set the UseAzureQueue flag to true in SessionStartInfo. If not set, it will be set to true by default when the cluster name has Azure domain suffixes and the TransportScheme is Http.

    ```
    info.UseAzureQueue = true;
```

###Use Http binding without Azure storage queue

To do this, explicitly set UseAzureQueue flag to false in the SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### Use NetTcp binding

To use NetTcp binding, the configuration is like connecting to an on-premises cluster. You'll need to open a few endpoints on the head node VM. In the Azure Management Portal do the following.


1. Stop the VM.

2. Add the TCP ports 9090, 9087, 9091, 9094 for the Session, Broker, Broker worker and Data services, respectively

    ![Configure endpoints][endpoint]

3. Start the VM.

The SOA client application requires no changes except altering the head name to the IaaS cluster full name.

## Next steps

* See [these resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) for more information about running Excel workloads with HPC Pack.

* See [Managing SOA Services in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) for more about deploying and managing SOA services with HPC Pack.

<!--Image references-->
[scenario]: ./media/virtual-machines-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-excel-cluster-hpcpack/udf.png

<properties 
	pageTitle="Customize HDInsight Clusters using script actions | Microsoft Azure" 
	description="Learn how to customize HDInsight clusters using Script Action." 
	services="hdinsight" 
	documentationCenter="" 
	authors="nitinme" 
	manager="paulettm" 
	editor="cgronlun"
	tags="azure-portal"/> 

<tags 
	ms.service="hdinsight" 
	ms.workload="big-data" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/07/2015" 
	ms.author="nitinme"/> 

# Customize HDInsight clusters using Script Action

[AZURE.INCLUDE [hdinsight-azure-preview-portal](../../includes/hdinsight-azure-preview-portal.md)]

* [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-v1.md)

HDInsight provides a configuration option called **Script Action** that invokes custom scripts, which define the customization to be performed on the cluster during the provision process. These scripts can be used to install additional software on a cluster, or to change the configuration of applications on a cluster. 


> [AZURE.NOTE] Script Action is only supported on HDInsight cluster version 3.1 or higher with the Windows operating system.  For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).
> 
> Script Action is available as part of the standard Azure HDInsight subscriptions at no extra charge.

HDInsight clusters can be customized in a variety of other ways as well, such as including additional Azure Storage accounts, changing the Hadoop configuration files (core-site.xml, hive-site.xml, etc.), or adding shared libraries (e.g., Hive, Oozie) into common locations in the cluster. These customizations can be done through Azure PowerShell, the Azure HDInsight .NET SDK, or the Azure Preview portal. For more information, see [Provision Hadoop clusters in HDInsight using custom options][hdinsight-provision-cluster].

## Script Action in the cluster provision process

Script Action is only used while a clusters is in the process of being created. The following diagram illustrates when Script Action is executed during the provision process:

![HDInsight cluster customization and stages during cluster provisioning][img-hdi-cluster-states] 

When the script is running, the cluster enters the **ClusterCustomization** stage. At this stage, the script is run under the system admin account, in parallel on all the specified nodes in the cluster, and provides full admin privileges on the nodes. 

> [AZURE.NOTE] Because you have admin privileges on the cluster nodes during the **ClusterCustomization** stage, you can use the script to perform operations like stopping and starting services, including Hadoop-related services. So, as part of the script, you must ensure that the Ambari services and other Hadoop-related services are up and running before the script finishes running. These services are required to successfully ascertain the health and state of the cluster while it is being created. If you change any configuration on the cluster that affects these services, you must use the helper functions that are provided. For more information about helper functions, see [Develop Script Action scripts for HDInsight][hdinsight-write-script].

The output and the error logs for the script are stored in the default Storage account you specified for the cluster. The logs are stored in a table with the name **u<\cluster-name-fragment><\time-stamp>setuplog**. These are aggregate logs from the script run on all the nodes (head node and worker nodes) in the cluster.


Each cluster can accept multiple script actions that are invoked in the order in which they are specified. A script can be ran on the head node, the worker nodes, or both. 

## Call Script Action scripts

Script Action scripts can be used from the Azure Preview Portal, Azure PowerShell, or the HDInsight .NET SDK. This article shows how to use Script Action from the portal. To learn how to use PowerShell and .NET SDK to use Script Action, look at the samples listed in the table below.

HDInsight provides several scripts to install the following components on HDInsight clusters:

Name | Script
----- | -----
**Install Spark** | https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1. See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].
**Install R** | https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1. See [Install and use R on HDInsight clusters][hdinsight-install-r].
**Install Solr** | https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1. See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).
- **Install Giraph** | https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1. See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).



**From the Azure Preview portal**

1. Start provisioning a cluster as described at [Provisioning a cluster using custom options](hdinsight-provision-clusters.md#portal). 
2. Under Optional Configuration, for the **Script Actions** blade, click **add script action** to provide details about the script action, as shown below:

	![Use Script Action to customize a cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Use Script Action to customize a cluster")
	
	<table border='1'>
		<tr><th>Property</th><th>Value</th></tr>
		<tr><td>Name</td>
			<td>Specify a name for the script action.</td></tr>
		<tr><td>Script URI</td>
			<td>Specify the URI to the script that is invoked to customize the cluster. s</td></tr>
		<tr><td>Head/Worker</td>
			<td>Specify the nodes (**Head** or **Worker**) on which the customization script is run.</b>.
		<tr><td>Parameters</td>
			<td>Specify the parameters, if required by the script.</td></tr>
	</table>

	Press ENTER to add more than one script action to install multiple components on the cluster. 

3. Click **Select** to save the script action configuration and continue with cluster provisioning. 
  

## Support for open-source software used on HDInsight clusters
The Microsoft Azure HDInsight service is a flexible platform that enables you to build big-data applications in the cloud by using an ecosystem of open-source technologies formed around Hadoop. Microsoft Azure provides a general level of support for open-source technologies, as discussed in the **Support Scope** section of the <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ website</a>. The HDInsight service provides an additional level of support for some of the components, as described below.

There are two types of open-source components that are available in the HDInsight service:

- **Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of the cluster. For example, YARN ResourceManager, the Hive query language (HiveQL), and the Mahout library belong to this category. A full list of cluster components is available in <a href="http://azure.microsoft.com/documentation/articles/hdinsight-component-versioning/" target="_blank">What's new in the Hadoop cluster versions provided by HDInsight?</a>.
- **Custom components** - You, as a user of the cluster, can install or use in your workload any component available in the community or created by you.

Built-in components are fully supported, and Microsoft Support will help to isolate and resolve issues related to these components.

Custom components receive commercially reasonable support to help you to further troubleshoot the issue. This might result in resolving the issue or asking you to engage available channels for the open-source technologies where deep expertise for that technology is found. For example, there are many community sites that can be used, like: <a href ="https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight" target="_blank">MSDN forum for HDInsight</a> and <a href="http://stackoverflow.com" target="_blank">Stack Overflow</a>. Also, Apache projects have project sites on <a href="http://apache.org" target="_blank">Apache.org</a>; for example, <a href="http://hadoop.apache.org/" target="_blank">Hadoop</a> and <a href="http://spark.apache.org/" target="_blank">Spark</a>.

The HDInsight service provides several ways to use custom components. Regardless of how a component is used or installed on the cluster, the same level of support applies. Below is a list of the most common ways that custom components can be used on HDInsight clusters:

1. Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted to the cluster.
2. Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on the cluster nodes.
3. Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on the HDInsight clusters. These samples are provided without support.

## Develop Script Action scripts

See [Develop Script Action scripts for HDInsight][hdinsight-write-script]. 


## See also

- [Provision Hadoop clusters in HDInsight using custom options][hdinsight-provision-cluster] provides instructions on how to provision an HDInsight cluster by using other custom options.
- [Develop Script Action scripts for HDInsight][hdinsight-write-script]
- [Install and use Spark on HDInsight clusters][hdinsight-install-spark]
- [Install and use R on HDInsight clusters][hdinsight-install-r]
- [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).
- [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: ../install-configure-powershell.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Stages during cluster provisioning"
 
test

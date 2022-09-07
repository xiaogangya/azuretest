<properties
	pageTitle="Install and use Giraph on Hadoop clusters in HDInsight | Microsoft Azure"
	description="Learn how to customize HDInsight cluster with Giraph. You'll use a Script Action configuration option to use a script to install Giraph."
	services="hdinsight"
	documentationCenter=""
	authors="Blackmist"
	manager="paulettm"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/18/2015"
	ms.author="larryfr"/>

# Install Giraph on HDInsight Hadoop clusters, and use Giraph to process large-scale graphs

You can install Giraph on any type of cluster in Hadoop on Azure HDInsight by using **Script Action** cluster customization. Script Action lets you run scripts to customize a cluster, only when the cluster is being created. For more information, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).

In this topic, you will learn how to install Giraph by using Script Action. Once you have installed Giraph, you'll also learn how to use Giraph for most typical applications, which is to process large-scale graphs.

> [AZURE.NOTE] The information in this article is specific to Linux-based HDInsight clusters. For information on working with Windows-based clusters, see [Install Giraph on HDInsight Hadoop clusters (Windows)](hdinsight-hadoop-giraph-install.md)

## <a name="whatis"></a>What is Giraph?

[Apache Giraph](http://giraph.apache.org/) allows you to perform graph processing by using Hadoop, and can be used with Azure HDInsight. Graphs model relationships between objects, such as the connections between routers on a large network like the Internet, or relationships between people on social networks (sometimes referred to as a social graph). Graph processing allows you to reason about the relationships between objects in a graph, such as:

- Identifying potential friends based on your current relationships.
- Identifying the shortest route between two computers in a network.
- Calculating the page rank of webpages.

##What the script does

This script performs the following actions:

* Installs Giraph to `/usr/hdp/current/giraph`
* Copies the `giraph-examples.jar` file to default storage (WASB) for your cluster: `/example/jars/giraph-examples.jar`

## <a name="install"></a>Install Giraph using Script Actions

A sample script to install Giraph on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh). This section provides instructions on how to use the sample script while provisioning the cluster by using the Azure portal.

> [AZURE.NOTE] You can also use Azure PowerShell or the HDInsight .NET SDK to create a cluster using this script. For more information on using these methods, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).

1. Start provisioning a cluster by using the steps in [Provision Linux-based HDInsight clusters](hdinsight-provision-linux-clusters.md#portal), but do not complete provisioning.

2. On the **Optional Configuration** blade, select **Script Actions**, and provide the information below:

	* __NAME__: Enter a friendly name for the script action.
	* __SCRIPT URI__: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
	* __HEAD__: Check this option
	* __WORKER__: Check this option
	* __ZOOKEEPER__: Check this option to install on the Zookeeper node.
	* __PARAMETERS__: Leave this field blank

3. At the bottom of the **Script Actions**, use the **Select** button to save the configuration. Finally, use the **Select** button at the bottom of the **Optional Configuration** blade to save the optional configuration information.

4. Continue provisioning the cluster as described in [Provision Linux-based HDInsight clusters](hdinsight-provision-linux-clusters.md#portal).

## <a name="usegiraph"></a>How do I use Giraph in HDInsight?

Once the cluster has finished provisioning, use the following steps to run the SimpleShortestPathsComputation example included with Giraph. This implements the basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementation for finding the shortest path between objects in a graph.

1. Connect to the HDInsight cluster using SSH:

		ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

	For more information on using SSH with HDInsight, see the following:

	* [Use SSH with Linux-based Hadoop on HDInsight from Linux, Unix, or OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

	* [Use SSH with Linux-based Hadoop on HDInsight from Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

1. Use the following to create a new file named __tiny_graph.txt__:

		nano tiny_graph.txt

	Use the following as the contents of this file:

		[0,0,[[1,1],[3,3]]]
		[1,0,[[0,1],[2,2],[3,1]]]
		[2,0,[[1,2],[4,4]]]
		[3,0,[[0,3],[1,1],[4,4]]]
		[4,0,[[3,4],[2,4]]]

	This data describes a relationship between objects in a directed graph, by using the format [source\_id, source\_value,[[dest\_id], [edge\_value],...]]. Each line represents a relationship between a **source\_id** object and one or more **dest\_id** objects. The **edge\_value** (or weight) can be thought of as the strength or distance of the connection between **source_id** and **dest\_id**.

	Drawn out, and using the value (or weight) as the distance between objects, the above data might look like this:

	![tiny_graph.txt drawn as circles with lines of varying distance between](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

2. To save the file, use __Ctrl+X__, then __Y__, and finally __Enter__ to accept the file name.

3. Use the following to store the data into primary storage for your HDInsight cluster:

		hadoop fs -copyFromLocal tiny_graph.txt /example/data/tiny_graph.txt

4. Run the SimpleShortestPathsComputation example using the following command:

		 hadoop jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnode0:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2

	The parameters used with this command are described in the following table.

	| Parameter | What it does |
	| --------- | ------------ |
	| `jar /usr/hdp/current/giraph/giraph-examples.jar` | The jar file containing the examples. |
	| `org.apache.giraph.GiraphRunner` | The class used to start the examples. |
	| `org.apache.giraph.examples.SimpleShortestPathsCoputation` | The example that will be ran. In this case, it will compute the shortest path between ID 1 and all other IDs in the graph. |
	| `-ca mapred.job.tracker=headnode0:9010` | The headnode for the cluster. |
	| `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` | The input format to use for the input data. |
	| `-vip /example/data/tiny_graph.txt` | The input data file. |
	| `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` | The output format. In this case, ID and value as plain text. |
	| `-op /example/output/shortestpaths` | The output location. |
	| `-w 2` | The number of workers to use. In this case, 2. |

	For more information on these, and other parameters used with Giraph samples, see the [Giraph quickstart](http://giraph.apache.org/quick_start.html).

5. Once the job has finished, the results will be stored in the __wasb:///example/out/shotestpaths__ directory. The files created will begin with __part-m-__ and end with a number indicating the first, second, etc. file. Use the following to view the output:

		hadoop fs -text /example/output/shortestpaths/*

	The output should appear similar to the following:

		0	1.0
		4	5.0
		2	2.0
		1	0.0
		3	1.0

	The SimpleShortestPathComputation example is hard coded to start with object ID 1 and find the shortest path to other objects. So the output should be read as `destination_id distance`, where distance is the value (or weight) of the edges traveled between object ID 1 and the target ID.

	Visualizing this, you can verify the results by traveling the shortest paths between ID 1 and all other objects. Note that the shortest path between ID 1 and ID 4 is 5. This is the total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.

	![Drawing of objects as circles with shortest paths drawn between](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)


## Next steps

- [Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md). Hue is a web UI that makes it easy to create, run and save Pig and Hive jobs, as well as browse the default storage for your HDInsight cluster.

- [Install and use Spark on HDInsight clusters](hdinsight-hadoop-spark-install-linux.md): Instructions on how to use cluster customization to install and use Spark on HDInsight Hadoop clusters. Spark is an open-source parallel processing framework that supports in-memory processing to boost the performance of big-data analytic applications.

- [Install R on HDInsight clusters](hdinsight-hadoop-r-scripts-linux.md): Instructions on how to use cluster customization to install and use R on HDInsight Hadoop clusters. R is an open-source language and environment for statistical computing. It provides hundreds of built-in statistical functions and its own programming language that combines aspects of functional and object-oriented programming. It also provides extensive graphical capabilities.

- [Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md). Use cluster customization to install Solr on HDInsight Hadoop clusters. Solr allows you to perform powerful search operations on data stored.

test

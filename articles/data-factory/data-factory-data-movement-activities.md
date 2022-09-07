<properties 
	pageTitle="Data Movement Activities" 
	description="Learn about Data Factory entities that you can use in an Data Factory pipelines to move data." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/29/2015" 
	ms.author="spelluru"/>

# Data Movement Activities
Data factory has a [globally available service](#global) to support data movement with [Copy activity](#copyactivity) across a variety of data stores listed below. Data factory also has built-in support for [securely moving data between on-premises locations and cloud](#moveonpremtocloud) using the data management gateway.

## Supported data stores for Copy Activity
Copy activity copies data from a **source** data store to a **sink** data store. Data factory supports the following data stores and source, sink combinations. Click on a data store to learn how to copy data from/to that store.

| **Source** | **Sink** |
| ------ | ---- |
| [Azure Blob](data-factory-azure-blob-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS, Azure DocumentDB, On-premises File System |
| [Azure Table](data-factory-azure-table-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS, Azure DocumentDB |
| [Azure SQL Database](data-factory-azure-sql-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS, Azure DocumentDB |
| [Azure DocumentDB](data-factory-azure-documentdb-connector.md) | Azure Blob, Azure Table, Azure SQL Database |
| [SQL Server on IaaS](data-factory-sqlserver-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS |
| [On-premises File System](data-factory-onprem-file-system-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS, On-premises File System |
| [On-premises SQL Server](data-factory-sqlserver-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS |
| [On-premises Oracle Database](data-factory-onprem-oracle-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS |
| [On-premises MySQL Database](data-factory-onprem-mysql-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS |
| [On-premises DB2 Database](data-factory-onprem-db2-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS |
| [On-premises Teradata Database](data-factory-onprem-teradata-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS |
| [On-premises Sybase Database](data-factory-onprem-sybase-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS |
| [On-premises PostgreSQL Database](data-factory-onprem-postgresql-connector.md) | Azure Blob, Azure Table, Azure SQL Database, On-premises SQL Server, SQL Server on IaaS |

## <a name="copyactivity"></a>Copy Activity
Copy activity takes one input dataset (**source**) and copies data per activity configuration to one output dataset (**sink**). Data copy is done in a batch fashion according to the schedule specified on the activity.

> [AZURE.NOTE] To learn about defining activities in general at a high level such as various JSON sections and properties available for all activities, see [Understanding Pipelines & Activities](data-factory-create-pipelines.md) article.

Copy activity provides the following capabilities:

### <a name="global"></a>Globally available data movement
The data movement service powering copy activity is available globally in the following regions and geographies. The globally available topology ensures efficient data movement avoiding cross-region hops in most cases.

| Region | Geography |
| ------ | --------- | 
| Central US | US |
| East US | US |
| East US2 | US |
| North Central US | US |
| South Central US | US |
| West US | US |
| Brazil South | LATAM |
| North Europe | EMEA |
| West Europe | EMEA |
| Southeast Asia | APAC |
| Japan East | APAC |

### <a name="moveonpremtocloud"></a>Securely move data between on-premises location and cloud
One of the challenges for modern data integration is to seamlessly move data to and from on-premises to cloud. Data management gateway is an agent you can install on-premises to enable hybrid data pipelines. 

The data gateway provides the following capabilities: 

1.	Manage access to on-premises data stores securely.
2.	Model on-premises data stores and cloud data stores within the same data factory and move data.
3.	Have a single pane of glass for monitoring and management with visibility into gateway status with data factory cloud based dashboard.


See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) for more details.

### Reliable and cost effective data movement
Copy activity is designed to move large volumes of data in a reliable way, resistant to transient errors across a large variety of data sources. Data can be copied in a cost effective way with the option to enable compression over the wire.

### Type conversions across different type systems
Different data stores have different native type systems. Copy activity performs automatic type conversions from source types to sink types with the following 2 step approach:

1. Convert from native source types to .NET type
2. Convert from .NET type to native sink type

You can find the mapping for a given native type system to .NET for the data store in the respective data store connector articles. You can use these mappings to determine appropriate types while creating your tables so that right conversions are performed during Copy activity.

### Working with different file formats
For file based sources Copy activity supports variety of file format including binary, text and Avro formats.

### Copy Activity Properties
Properties like name, description, input and output tables, various policies etc are available for all types of activities. Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type. 

In case of Copy activity the **typeProperties** section varies depending on the types of sources and sinks. Each of the data store specific page listed above documents these properties specific to the data store type.


## Send Feedback
We would really appreciate your feedback on this article. Please take a few minutes to submit your feedback via [email](mailto:adfdocfeedback@microsoft.com?subject=data-factory-data-movement-activities.md). 
test

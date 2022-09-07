<properties
   pageTitle="How to register data sources"
   description="How-to article highlighting how to register data sources with Azure Data Catalog, including the metadata fields extracted, and the data sources supported during preview."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="08/25/2015"
   ms.author="maroche"/>


# How to register data sources

## Introduction
**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources. In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations to get more value from their existing data. And the first step to making a data source discoverable via **Azure Data Catalog** is to register that data source.
## Registering data sources
Registration is the process of extracting metadata from the data source and copying that data into the **Azure Data Catalog** service. The data remains where it currently resides, and remains under the control of the administrators and policies of the current system.

To register a data source, simply launch the **Azure Data Catalog** data source registration tool from the **Azure Data Catalog** portal. Log in with your Work or school account (the same Azure Active Directory credentials you use to log in to the portal) and then select the data source you want to register.
See the [Get Started with Azure Data Catalog](data-catalog-get-started.md) tutorial for more step-by-step details.

Once the data source has been registered, the Catalog tracks its location and indexes its metadata, so that users can search, browse, and discover the data source, and then use its location to connect to it using the application or tool of their choice.
## Sources supported
In the current preview, **Azure Data Catalog** supports the registration of these data sources and object types:

* SQL Server Database Engine Tables and Views
* Oracle Database Tables and Views
* SQL Server Analysis Services Multidimensional Dimensions, Measures, and KPIs
* SQL Server Analysis Services Tabular Tables
* SQL Server Reporting Services Reports
* Azure Storage Blobs and Directories
* HDFS Files and Directories

> [AZURE.NOTE] SQL Server support also includes Microsoft Azure SQL Database

<br/>

> [AZURE.NOTE] SQL Server Reporting Services support is for native mode servers only – SharePoint mode is not yet supported

<br/>

## Structural metadata
When you’re registering a data source, the registration tool will extract information about the structure of the objects you select – this is referred to as structural metadata.
For all objects this structural metadata will include the object’s location, so that client tools who discover the data. Other structural metadata includes object name and type, and attribute/column name and data type.
## Descriptive metadata
In addition to the core structural metadata extracted from the data source, the data source registration tool will also extract descriptive metadata as well. For SQL Server Analysis Services and SQL Server Reporting Services, this is taken from the Description properties exposed by these services. For SQL Server, values provided using the ms_description extended property will be extracted. For Oracle Database, the data source registration tool will extract the COMMENTS column from the ALL_TAB_COMMENTS view.

In addition to the descriptive metadata extracted from the data source, users can also enter descriptive metadata using the data source registration tool. Users can add tags, and can identify experts for the objects being registered. All of this descriptive metadata is copied to the **Azure Data Catalog** service along with the structural metadata.
## Including previews
By default, only metadata is extracted from data sources and copied to the **Azure Data Catalog** service, but understanding a data source is often made easier by seeing a sample of the data it contains.

The **Azure Data Catalog** data source registration tool allows users to include a snapshot preview of the data in each table and view that is registered. If the user opts-in to including previews during registration, the registration tool will include up to 20 records from each table and view. This snapshot is then copied into the Catalog along with the structural and descriptive metadata.
Note: Wide tables with a large number of columns may have fewer than 20 records included in their preview.
## Updating registrations
Registering a data source will make it discoverable in the **Azure Data Catalog** using the metadata and optional preview extracted during registration. If the data source needs to be updated in the Catalog (for example, if the schema of an object has changed, or tables originally excluded should be included, or a user wants to update the data included in the previews) the data source registration tool can be re-run.

Re-registering an already-registered data source performs a merge “upsert” operation: existing objects will be updated, while new objects will be created. Any metadata provided by users through the **Azure Data Catalog** portal will be maintained.

## Summary
Registering a data source with **Azure Data Catalog** makes that data source easier to discover and understand, by copying structural and descriptive metadata from the data source into the Catalog service. Once a data source has been registered, it can then be annotated, managed, and discovered using the **Azure Data Catalog** portal.

test

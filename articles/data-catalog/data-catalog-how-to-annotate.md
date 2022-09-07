<properties
   pageTitle="How to annotate data sources"
   description="How-to article highlighting how to annotate data assets in Azure Data Catalog, including friendly names, tags, descriptions, and experts."
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
   ms.date="08/17/2015"
   ms.author="maroche"/>


# How to annotate data sources

## Introduction
**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources. In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations to get more value from their existing data. When a data source is been registered with **Azure Data Catalog**, its metadata is copied and indexed by the service, but the story doesn’t end there. **Azure Data Catalog** allows users to provide their own descriptive metadata – such as descriptions and tags – to supplement the metadata extracted from the data source, and to make the data source more understandable to more people.

## Annotation and crowdsourcing
Everyone has an opinion. And this is a good thing.
**Azure Data Catalog** recognizes that different users have different perspectives on enterprise data sources, and that each of these perspectives can be valuable. Consider the following scenario:

* The system administrator knows the service level agreement for the servers or services that host the data source.
* The database administrator knows the backup schedule for each database, and the allowed ETL processing windows.
* The system owner knows the process for users to request access to the data source.
* The data steward knows how the assets and attributes in the data source map to the enterprise data model.
* The analyst knows how the data is used in the context of the business processes he supports.

Each of these perspectives is valuable, and **Azure Data Catalog** uses a crowdsourcing approach to metadata that allows each one to be captured and used to provide a complete picture of registered data sources. Using the **Azure Data Catalog** portal, each user can add and edit his own annotations, while being able to view annotations provided by other users.

## Different types of annotations
During the **Azure Data Catalog** preview, the following types of annotations are supported:

| Annotation     | Notes                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Friendly name  | Friendly names can be supplied at the data asset level, to make the data assets more easily understood. Friendly names are most useful when the underlying object name is cryptic, abbreviated or otherwise not meaningful to users.                                                                                                                            |
| Description    | Descriptions can be supplied at the data asset and attribute / column levels. Descriptions are free-form short text annotations that describe the user’s perspective on the data asset or its use.                                                                                                                                                              |
| Tags           | Tags can be supplied at the data asset and attribute / column levels. Tags are user-defined labels that can be used to categorize data assets or attributes.                                                                                                                                                                                                    |
| Experts        | Experts can be supplied at the data asset level. Experts identify users or groups with expert perspectives on the data and can serve as points of contact for users who discover the registered data sources and have questions that are not answered by the existing annotations.                                                                              |
| Request access | Request access information can be supplied at the data asset level. This information is for users who discover a data source that they do not yet have permissions to access. Users can enter the email address of the user or group who grants access, the URL of the process or tool that users need to gain access, or can enter the process itself as text. |

## Annotating multiple assets
When selecting multiple data assets in the **Azure Data Catalog** portal, users can annotate all selected assets in a single operation. Annotations will apply to all selected assets, making it easy to select and provide a consistent description and sets of tags and experts for related data assets.

> [AZURE.NOTE] Tags and experts can also be provided when registering data assets using the **Azure Data Catalog** data source registration tool.

When selecting multiple tables and views, only columns that all selected data assets have in common will be displayed in the **Azure Data Catalog** portal. This allows users to provide tags and descriptions for all columns with the same name for all selected assets.

## Annotations and discovery
Just as the metadata extracted from the data source during registration is added to the **Azure Data Catalog** search index, user-supplied metadata is also indexed. This means that not only do annotations make it easier for users to understand the data they discover, annotations also make it easier for users to discover the annotated data assets by searching using the terms that make sense to them.

## Summary
Registering a data source with **Azure Data Catalog** makes that data discoverable by copying structural and descriptive metadata from the data source into the Catalog service. Once a data source has been registered, users can provide annotations to make easier to discover and understand from within the **Azure Data Catalog** portal.

test

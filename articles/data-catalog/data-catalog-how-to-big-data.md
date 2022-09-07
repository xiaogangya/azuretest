<properties
   pageTitle="How to work with 'big data' data sources"
   description="How-to article highlighting patters for using Azure Data Catalog  with 'big data' data sources, including Azure Blob Storage and Hadoop HDFS."
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


# How to work with big data sources in Azure Data Catalog

## Introduction
**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources. In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations to get more value from their existing data sources, including big data.

**Azure Data Catalog** supports the registration of Azure Blog Storage blobs and directories as well as Hadoop HDFS files and directories. The semi-structured nature of these data sources provides great flexibility, but it also means that users must consider how the data sources are organized in order to get the most value from registering them with **Azure Data Catalog**.

## Directories as logical data sets

A common pattern for organizing big data sources is to treat directories as logical data sets. Top-level directories are used to define a data set, while subfolders define partitions, and the files they contain store the data itself.

An example of this pattern might be:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

In this example, vehicle_maintenance_events and location_tracking_events represent logical data sets. Each of these folders contains data files which are organized by year and month into subfolders. Each of these folders could potentially contain hundreds or thousands of files.

In this pattern, registering individual files with **Azure Data Catalog** probably does not make sense. Instead, register the directories that represent the data sets that will be meaningful to the users working with the data.

## Reference data files

A complementary pattern is to store reference data sets as individual files. These data sets may be thought of as the 'small' side of big data, and are often similar to dimensions in an analytical data model. Reference data files contain records that are used to provide context for the bulk of the data files stored elsewhere in the big data store.

An example of this pattern might be:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

When an analyst or data scientist is working with the data contained in the larger directory structures, the data in these reference files can be used to provide more detailed information for entities that are referred to only by name or ID in the larger data set.

In this pattern, it will make sense to register the individual reference data files with **Azure Data Catalog**. Each file represents a data set, and each one can be annotated and discovered individually.

## Alternate patterns

The patterns described above are just two possible ways a big data store may be organized, but each implementation will be different. Regardless of how your data sources are structured, when registering big data sources with **Azure Data Catalog**, focus on registering the files and directories that represent the data sets that will be of value to others within your organization. Registering all files and directories can clutter the catalog, making it harder for users to find what they need.

## Summary
Registering data sources with **Azure Data Catalog** makes them easier to discover and understand. By registering and annotating the big data files and directories that represent logical data sets, you can help users find and use the big data sources they need.

test

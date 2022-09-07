<properties 
	pageTitle="Move data to and from File System | Azure Data Factory" 
	description="Learn how to move data to/from on-premises File System using Azure Data Factory." 
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
	ms.date="08/26/2015" 
	ms.author="spelluru"/>

# Move data to and from On-premises File System using Azure Data Factory

This article outlines how you can use data factory copy activity to move data to and from on-premises file system. This article builds on the [data movement activities](data-factory-data-movement-activities.md) article which presents a general overview of data movement with copy activity and supported data store combinations.

Data factory supports connecting to and from on-premises File System via the Data Management Gateway. See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step by step instructions on setting up the gateway. 

**Note:** Apart from the Data Management Gateway no other binaries need to be installed to communicate to and from on-premises File System. 

## Linux File Share 

Perform the following two steps to use a Linux file share with the File Server Linked Service:

- Install [Samba](https://www.samba.org/) on your Linux Server.
- Install and configure Data Management Gateway on a Windows server. Installing gateway on a Linux server is not supported. 
 
## Sample: Copy data from On-premises File System to Azure Blob

The sample below shows:

1.	A linked service of type [OnPremisesFileServer](data-factory-onprem-file-system-connector.md#onpremisesfileserver-linked-service-properties).
2.	A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
3.	An input [dataset](data-factory-create-datasets.md) of type [FileShare](data-factory-onprem-file-system-connector.md#on-premises-file-system-dataset-type-properties).
4.	An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.	The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](data-factory-onprem-file-system-connector.md#file-share-copy-activity-type-properties) and [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

The sample below copies data belonging to a time series from on-premises file system to Azure blob every hour. The JSON properties used in these samples are described in sections following the samples. 

As a first step, do setup the data management gateway as per the instructions in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article. 

**On-premises File Server linked service:**

	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\\Contosogame-Asia",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}

**Azure Blob storage linked service:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**On-premises File System input dataset:**

Data is picked up from a new file every hour with the path & filename reflecting the specific datetime with hour granularity. 

Setting “external”: ”true” and specifying externalData policy informs the Azure Data Factory service that the table is external to the data factory and not produced by an activity in the data factory.

	{
	  "name": "OnpremisesFileSystemInput",
	  "properties": {
	    "type": " FileShare",
	    "linkedServiceName": " OnPremisesFileServerLinkedService ",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
	      "fileName": "{Hour}.csv",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ]
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

**Azure Blob output dataset:**

Data is written to a new blob every hour (frequency: hour, interval: 1). The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed. The folder path uses year, month, day, and hours parts of the start time. 

	{
	  "name": "AzureBlobOutput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%HH"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": "\t",
	        "rowDelimiter": "\n"
	      }
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

**Copy activity:**

The pipeline contains a Copy Activity that is configured to use the above input and output datasets and is scheduled to run every hour. In the pipeline JSON definition, the **source** type is set to **FileSystemSource** and **sink** type is set to **BlobSink**. 
	
	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2015-06-01T18:00:00",
	    "end":"2015-06-01T19:00:00",
	    "description":"Pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "OnpremisesFileSystemtoBlob",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "OnpremisesFileSystemInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureBlobOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "FileSystemSource"
	          },
	          "sink": {
	            "type": "BlobSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	     ]
	   }
	}

##Sample: Copy data from Azure SQL to On-premises File System 

The sample below shows:

1.	A linked service of type AzureSqlDatabase.
2.	A linked service of type OnPremisesFileServer.
3.	An input dataset of type AzureSqlTable. 
3.	An output dataset of type FileShare.
4.	A pipeline with Copy activity that uses SqlSource and FileSystemSink.

The sample copies data belonging to a time series from a table in Azure SQL database to a On-premises File System every hour. The JSON properties used in these samples are described in sections following the samples. 

**Azure SQL linked service:**

	{
	  "name": "AzureSqlLinkedService",
	  "properties": {
	    "type": "AzureSqlDatabase",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

**On-premises File Server linked service:**

	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\\Contosogame-Asia",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}

**Azure SQL input dataset:**

The sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data. 

Setting “external”: ”true” and specifying externalData policy informs the Data Factory service that  the table is external to the data factory and is not produced by an activity in the data factory.

	{
	  "name": "AzureSqlInput",
	  "properties": {
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
	    "typeProperties": {
	      "tableName": "MyTable"
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

**On-premises File System output dataset:**

Data is copied to a new file every hour with the path for the blob reflecting the specific datetime with hour granularity.

	{
	  "name": "OnpremisesFileSystemOutput",
	  "properties": {
	    "type": "FileShare",
	    "linkedServiceName": " OnPremisesFileServerLinkedService ",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
	      "fileName": "{Hour}.csv",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%HH"
	          }
	        }
	      ]
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

**Pipeline with a Copy activity:**
The pipeline contains a Copy Activity that is configured to use the above input and output datasets and is scheduled to run every hour. In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **FileSystemSink**. The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.

	
	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2015-06-01T18:00:00",
	    "end":"2015-06-01T20:00:00",
	    "description":"pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "AzureSQLtoOnPremisesFile",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureSQLInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "OnpremisesFileSystemOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "SqlSource",
	            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
	          },
	          "sink": {
	            "type": "FileSystemSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 3,
	          "timeout": "01:00:00"
	        }
	      }
	     ]
	   }
	}

## OnPremisesFileServer Linked Service properties

You can link an On-premises File System to an Azure Data Factory with On-Premises File Server Linked Service. The following table provides description for JSON elements specific to On-Premises File Server Linked Service. 

Property | Description | Required
-------- | ----------- | --------
type | The type property should be set to **OnPremisesFileServer** | Yes 
host | Host name of the server. Use ‘ \ ’ as the escape character as in the following example: if your share is: \\servername, specify \\\\servername.<p>If the file system is local to the gateway machine, use Local or localhost. If the file system is on a server different from the gateway machine, use \\\\servername.</p> | Yes
userid  | Specify the ID of the user who has access to the server | No (if you choose encryptedCredential)
password | Specify the password for the user (userid) | No (if you choose encryptedCredential 
encryptedCredential | Specify the encrypted credentials that you can get by running the New-AzureDataFactoryEncryptValue cmdlet<p>**Note:** You must use the Azure PowerShell of version 0.8.14 or higher to use cmdlets such as New-AzureDataFactoryEncryptValue with type parameter set to OnPremisesFileSystemLinkedService</p> | No (if you choose to specify userid and password in plain text)
gatewayName | Name of the gateway that the Data Factory service should use to connect to the on-premises file server | Yes

See [Setting Credentials and Security](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security) for details about setting credentials for an on-premises File System data source.

**Example: Using username and password in plain text**
	
	{
	  "Name": "OnPremisesFileServerLinkedService",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "\\\\Contosogame-Asia",
	      "userid": "Admin",
	      "password": "123456",
	      "gatewayName": "mygateway"
	    }
	  }
	}
	
**Example: Using encryptedcredential**

	{
	  "Name": " OnPremisesFileServerLinkedService ",
	  "properties": {
	    "type": "OnPremisesFileServer",
	    "typeProperties": {
	      "host": "localhost",
	      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
	      "gatewayName": "mygateway"
	    }
	  }
	}

## On-premises File System Dataset type properties

For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article. Sections like structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure Blob, Azure Table, On-premises File System, etc...). 

The typeProperties section is different for each type of dataset and provides information about the location, format etc. of the data in the data store. The typeProperties section for dataset of type **FileShare** dataset has the following properties.

Property | Description | Required
-------- | ----------- | --------
folderPath | Path to the folder. Example: myfolder<p>Use escape character ‘ \ ’ for special characters in the string. For example: for folder\subfolder, specify folder\\subfolder and for d:\samplefolder, specify d:\\samplefolder.</p><p>You can combine this with **partitionBy** to have folder paths based on slice start/end date-times.</p> | Yes
fileName | Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder. If you do not specify any value for this property, the table points to all files in the folder.<p>When fileName is not specified for an output dataset, the name of the generated file would be in the following this format: </p><p>Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</p> | No
partitionedBy | partitionedBy can be leveraged to specify a dynamic folderPath, filename for time series data. For example folderPath parameterized for every hour of data. | No
Format | Two formats types are supported: **TextFormat**, **AvroFormat**. You need to set the type property under format to either if this value. When the forAvroFormatmat is TextFormat you can specify additional optional properties for format. See the format section below for more details. | No
fileFilter | Specify a filter to be used to select a subset of files in the folderPath rather than all files. <p>Allowed values are: * (multiple characters) and ? (single character).</p><p>Examples 1: "fileFilter": "*.log"</p>Example 2: "fileFilter": 2014-1-?.txt"</p><p>**Note**: fileFilter is applicable for an input FileShare dataset</p> | No
| compression | Specify the type and level of compression for the data. Supported types are: GZip, Deflate, and BZip2 and supported levels are: Optimal and Fastest. See [Compression support](#compression-support) section for more details.  | No |

> [AZURE.NOTE] filename and fileFilter cannot be used simultaneously.

### Leveraging partionedBy property

As mentioned above, you can specify a dynamic folderPath, filename for time series data with partitionedBy. You can do so with the Data Factory macros and the system variable SliceStart, SliceEnd that indicate the logical time period for a given data slice. 

See [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles to understand more details on time series datasets, scheduling and slices.

#### Sample 1:

	"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
	"partitionedBy": 
	[
	    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
	],

In the above example {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified. The SliceStart refers to start time of the slice. The folderPath is different for each slice. For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.

#### Sample 2:

	"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
	"fileName": "{Hour}.csv",
	"partitionedBy": 
	 [
	    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
	    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
	    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
	    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
	],

In the above example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.

### Specifying TextFormat

If the format is set to **TextFormat**, you can specify the following **optional** properties in the **Format** section within the **typeProperties** section. 

Property | Description | Required
-------- | ----------- | --------
columnDelimiter | The character(s) used as a column separator in a file. The default value is comma (,). | No
rowDelimiter | The character(s) used as a raw separator in file. The default value is any of the following: [“\r\n”, “\r”,” \n”]. | No
escapeChar | The special character used to escape column delimiter shown in content. No default value. You must specify no more than one character for this property.<p>For example, if you have comma (,) as the column delimiter but you want have comma character in the text (example: “Hello, world”), you can define ‘$’ as the escape character and use string “Hello$, world” in the source.</p><p>Note that you cannot specify both escapeChar and quoteChar for a table.</p> | No
quoteChar | The special character is used to quote the string value. The column and row delimiters inside of the quote characters would be treated as part of the string value. No default value. You must specify no more than one character for this property.<p>For example, if you have comma (,) as the column delimiter but you want have comma character in the text (example: <Hello, world>), you can define ‘"’ as the quote character and use string <"Hello, world"> in the source. This property is applicable to both input and output tables.</p><p>Note that you cannot specify both escapeChar and quoteChar for a table.</p> | No
nullValue | The character(s) used to represent null value in blob file content. The default value is “\N”.> | No
encodingName | Specify the encoding name. For the list of valid encoding names, see: Encoding.EncodingName Property. <p>For example: windows-1250 or shift_jis. The default value is: UTF-8.</p> | No

#### Samples:

The following sample shows some of the format properties for **TextFormat**.

	"typeProperties":
	{
	    "folderPath": "MyFolder",
	    "fileName": "MyFileName"
	    "format":
	    {
	        "type": "TextFormat",
	        "columnDelimiter": ",",
	        "rowDelimiter": ";",
	        "quoteChar": "\"",
	        "NullValue": "NaN"
	    }
	},

To use an escapeChar instead of quoteChar, replace the line with quoteChar with the following:

	"escapeChar": "$",

### Specifying AvroFormat

If the format is set to **AvroFormat**, you do not need to specify any properties in the Format section within the typeProperties section. Example:

	"format":
	{
	    "type": "AvroFormat",
	}
	
To use Avro format in a subsequent Hive table, refer to [Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).		

[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## File Share Copy Activity type properties

**FileSystemSource** and **FileSystemSink** do not support any properties at this time.

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]








 

test

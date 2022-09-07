<properties 
	pageTitle="Move data to and from DocumentDB | Azure Data Factory" 
	description="Learn how move data to/from Azure DocumentDB collection using Azure Data Factory" 
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

# Move data to and from DocumentDB using Azure Data Factory

This article outlines how you can use the Copy Activity in an Azure data factory to move data to Azure DocumentDB from another data store and move data from another data store to DocumentDB. This article builds on the [data movement activities](data-factory-data-movement-activities.md) article which presents a general overview of data movement with copy activity and supported data store combinations.

## Sample: Copy data from DocumentDB to Azure Blob

The sample below shows:

1. A linked service of type [DocumentDb](#azure-documentdb-linked-service-properties).
2. A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. An input [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) and [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

The sample copies data in Azure DocumentDB to Azure Blob. The JSON properties used in these samples are described in sections following the samples.

**Azure DocumentDB linked service:**

	{
	  "name": "DocumentDbLinkedService",
	  "properties": {
	    "type": "DocumentDb",
	    "typeProperties": {
	      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
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

**Azure Document DB input dataset:**

The sample assumes you have a collection named **Person** in an Azure DocumentDB database.
 
Setting “external”: ”true” and specifying externalData policy information the Azure Data Factory service that the table is external to the data factory and not produced by an activity in the data factory.

	{
	  "name": "PersonDocumentDbTable",
	  "properties": {
	    "type": "DocumentDbCollection",
	    "linkedServiceName": "DocumentDbLinkedService",
	    "typeProperties": {
	      "collectionName": "Person"
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Day",
	      "interval": 1
	    }
	  }
	}


**Azure Blob output dataset:**

Data is copied to a new blob every hour with the path for the blob reflecting the specific datetime with hour granularity.

	{
	  "name": "PersonBlobTableOut",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "docdb",
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": ",",
	        "nullValue": "NULL"
	      }
	    },
	    "availability": {
	      "frequency": "Day",
	      "interval": 1
	    }
	  }
	}

Sample JSON document in the Person collection in a DocumentDB database: 

	{
	  "PersonId": 2,
	  "Name": {
	    "First": "Jane",
	    "Middle": "",
	    "Last": "Doe"
	  }
	}

DocumentDB supports querying documents using a SQL like syntax over hierarchical JSON documents. 

Example: 
	SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person

The following pipeline copies data from the Person collection in the DocumentDB database to an Azure blob. As part of the copy activity the input and output datasets have been specified.  
	
	{
	  "name": "DocDbToBlobPipeline",
	  "properties": {
	    "activities": [
	      {
	        "type": "Copy",
	        "typeProperties": {
	          "source": {
	            "type": "DocumentDbCollectionSource",
	            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
	            "nestingSeparator": "."
	          },
	          "sink": {
	            "type": "BlobSink",
	            "blobWriterAddHeader": true,
	            "writeBatchSize": 1000,
	            "writeBatchTimeout": "00:00:59"
	          }
	        },
	        "inputs": [
	          {
	            "name": "PersonDocumentDbTable"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "PersonBlobTableOut"
	          }
	        ],
	        "policy": {
	          "concurrency": 1
	        },
	        "name": "CopyFromDocDbToBlob"
	      }
	    ],
	    "start": "2015-04-01T00:00:00Z",
	    "end": "2015-04-02T00:00:00Z"
	  }
	}

## Sample: Copy data from Azure Blob to Azure DocumentDB

The sample below shows:

1. A linked service of type [DocumentDb](#azure-documentdb-linked-service-properties).
2. A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. An output [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) and [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


The sample copies data from Azure blob to Azure DocumentDB. The JSON properties used in these samples are described in sections following the samples.

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

**Azure DocumentDB linked service:**
	
	{
	  "name": "DocumentDbLinkedService",
	  "properties": {
	    "type": "DocumentDb",
	    "typeProperties": {
	      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
	    }
	  }
	}

**Azure Blob input dataset:**

	{
	  "name": "PersonBlobTableIn",
	  "properties": {
	    "structure": [
	      {
	        "name": "Id",
	        "type": "Int"
	      },
	      {
	        "name": "FirstName",
	        "type": "String"
	      },
	      {
	        "name": "MiddleName",
	        "type": "String"
	      },
	      {
	        "name": "LastName",
	        "type": "String"
	      }
	    ],
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "fileName": "input.csv",
	      "folderPath": "docdb",
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": ",",
	        "nullValue": "NULL"
	      }
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Day",
	      "interval": 1
	    }
	  }
	}

**Azure DocumentDB output dataset:**

The sample copies data to a collection named “Person”.

	{
	  "name": "PersonDocumentDbTableOut",
	  "properties": {
	    "structure": [
	      {
	        "name": "Id",
	        "type": "Int"
	      },
	      {
	        "name": "Name.First",
	        "type": "String"
	      },
	      {
	        "name": "Name.Middle",
	        "type": "String"
	      },
	      {
	        "name": "Name.Last",
	        "type": "String"
	      }
	    ],
	    "type": "DocumentDbCollection",
	    "linkedServiceName": "DocumentDbLinkedService",
	    "typeProperties": {
	      "collectionName": "Person"
	    },
	    "availability": {
	      "frequency": "Day",
	      "interval": 1
	    }
	  }
	}

The following pipeline copies data from Azure Blob to the Person collection in the 	DocumentDB. As part of the copy activity the input and output datasets have been specified. 
	
	{
	  "name": "BlobToDocDbPipeline",
	  "properties": {
	    "activities": [
	      {
	        "type": "Copy",
	        "typeProperties": {
	          "source": {
	            "type": "BlobSource"
	          },
	          "sink": {
	            "type": "DocumentDbCollectionSink",
	            "nestingSeparator": ".",
	            "writeBatchSize": 2,
	            "writeBatchTimeout": "00:00:00"
	          }
	          "translator": {
	              "type": "TabularTranslator",
	              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
	          }
	        },
	        "inputs": [
	          {
	            "name": "PersonBlobTableIn"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "PersonDocumentDbTableOut"
	          }
	        ],
	        "policy": {
	          "concurrency": 1
	        },
	        "name": "CopyFromBlobToDocDb"
	      }
	    ],
	    "start": "2015-04-14T00:00:00Z",
	    "end": "2015-04-15T00:00:00Z"
	  }
	}
 
If the sample blob input is as 

	1,John,,Doe

Then the output JSON in DocumentDB will be as:

	{
	  "Id": 1,
	  "Name": {
	    "First": "John",
	    "Middle": null,
	    "Last": "Doe"
	  },
	  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
	}
	
DocumentDB is a NoSQL store for JSON documents, where nested structures are allowed. Azure Data Factory enables user to denote hierarchy via **nestingSeparator**, which is “.” in this example. With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.

## Azure DocumentDB Linked Service properties

The following table provides description for JSON elements specific to Azure DocumentDB linked service. 

| **Property** | **Description** | **Required** |
| -------- | ----------- | --------- |
| type | The type property must be set to: **DocumentDb** | Yes |
| connectionString | Specify information needed to connect to Azure DocumentDB database. | Yes |

## Azure DocumentDB Dataset type properties

For a full list of sections & properties available for defining datasets please refer to the [Creating datasets](data-factory-create-datasets.md) article. Sections like structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc...).
 
The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store. The typeProperties section for the dataset of type **DocumentDbCollection** has the following properties.

| **Property** | **Description** | **Required** |
| -------- | ----------- | -------- |
| collectionName | Name of the DocumentDB document collection. | Yes |


Example:

	{
	  "name": "PersonDocumentDbTable",
	  "properties": {
	    "type": "DocumentDbCollection",
	    "linkedServiceName": "DocumentDbLinkedService",
	    "typeProperties": {
	      "collectionName": "Person"
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Day",
	      "interval": 1
	    }
	  }
	}

## Azure DocumentDB Copy Activity type properties

For a full list of sections & properties available for defining activities please refer to the [Creating Pipelines](data-factory-create-pipelines.md) article. Properties like name, description, input and output tables, various policies etc are available for all types of activities.
 
**Note:** The Copy Activity takes only one input and produces only one output.

Properties available in the typeProperties section of the activity on the other hand vary with each activity type and in case of Copy activity they vary depending on the types of sources and sinks.

In case of Copy activity when source is of type **DocumentDbCollectionSource**
the following properties are available in **typeProperties** section:

| **Property** | **Description** | **Allowed values** | **Required** |
| ------------ | --------------- | ------------------ | ------------ |
| query | Specify the query to read data. | Query string supported by DocumentDB. <p>Example: SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"</p> | No <p>If not specified, the SQL statement that is executed: select <columns defined in structure> from mycollection </p>
| nestingSeparator | Special character to indicate that the document is nested | Any character. <p>DocumentDB is a NoSQL store for JSON documents, where nested structures are allowed. Azure Data Factory enables user to denote hierarchy via nestingSeparator, which is “.” in the above examples. With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</p> | No

**DocumentDbCollectionSink** supports the following properties:

| **Property** | **Description** | **Allowed values** | **Required** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | A special character in the source column name to indicate that nested document is needed. <p>For example above: Name.First in the output table produces the following JSON structure in the DocumentDB document:</p><p>"Name": {<br/>	"First": "John"<br/>},</p> | Character that is used to separate nesting levels.<p>Default value is . (dot).</p> | Character that is used to separate nesting levels. <p>Default value is . (dot).</p> | No | 
| writeBatchSize | Number of parallel requests to DocumentDB service to create documents.<p>You can fine tune the performance when copying data to/from DocumentDB by using this property. You can expect a better performance when you increase writeBatchSize because more parallel requests to DocumentDB are sent. However you’ll need to avoid throttling that can throw the error message: "Request rate is large".</p><p>Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (e.g. S3) to have the most throughput available (2,500 request units/second).</p> | Integer Value | No |
| writeBatchTimeout | Wait time for the operation to complete before it times out. | (Unit = timespan) Example: “00:30:00” (30 minutes). | No |
 
 

test

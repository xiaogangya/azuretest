<properties
	pageTitle="Getting started with Azure table storage and Visual Studio connected services"
	description="How to get started using Azure table storage in a cloud service project in Visual Studio."
	services="storage"
	documentationCenter=""
	authors="patshea123"
	manager="douge"
	editor="tglee"/>

<tags
	ms.service="storage"
	ms.workload="web"
	ms.tgt_pltfrm="vs-getting-started"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/22/2015"
	ms.author="patshea123"/>

# Getting started with Azure table storage and Visual Studio connected services

> [AZURE.SELECTOR]
> - [Getting started](vs-storage-cloud-services-getting-started-tables.md)
> - [What happened](vs-storage-cloud-services-what-happened.md)

> [AZURE.SELECTOR]
> - [Blobs](vs-storage-cloud-services-getting-started-blobs.md)
> - [Queues](vs-storage-cloud-services-getting-started-queues.md)
> - [Tables](vs-storage-cloud-services-getting-started-tables.md)

##Overview

This article describes how to get started using Azure table storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using the Visual Studio **Add Connected Services** dialog. The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.

The Azure Table storage service enables you to store large amounts of structured data. The service is a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud. Azure tables are ideal for storing structured, non-relational data.

To get started, you first need to create a table in your storage account. We'll show you how to create an Azure table in code, and also how to perform basic table and entity operations, such as adding, modifying, reading and reading table entities. The samples are written in C\# code and use the [Azure Storage client library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**NOTE:** Some of the APIs that perform calls out to Azure storage are asynchronous. See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information. The code below assumes async programming methods are being used.

- See [How to use Table storage from .NET](storage-dotnet-how-to-use-tables.md) for more information on programmatically manipulating tables.
- See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.
- See [Cloud Services documentation](http://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.
- See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.

##Access tables in code

To access tables in cloud service projects, you need to include the following items to any C# source files that access Azure table storage.

1. Make sure the namespace declarations at the top of the C# file include these `using` statements.

		using Microsoft.Framework.Configuration;
		using Microsoft.WindowsAzure.Storage;
		using Microsoft.WindowsAzure.Storage.Table;
		using System.Threading.Tasks;
		using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Get a **CloudStorageAccount** object that represents your storage account information. Use the following code to get the storage connection string and storage account information from the Azure service configuration.

		 CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
		   CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
> [AZURE.NOTE]  Use all of the above code in front of the code in the following samples.

3. Get a **CloudTableClient** object to reference the table objects in your storage account.

         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Get a **CloudTable** reference object to reference a specific table and entities.

    	// Get a reference to a table named "peopleTable".
	    CloudTable table = tableClient.GetTableReference("peopleTable");

##Create a table in code

To create the Azure table, just add a call to `CreateIfNotExistsAsync` to the after you get a `CloudTable` object as described in the "Access tables in code" section.

	// Create the CloudTable if it does not exist.
	await table.CreateIfNotExistsAsync();

##Add an entity to a table

To add an entity to a table, create a class that defines the properties of your entity. The following code defines an entity class called **CustomerEntity** that uses the customer's first name as the row key and the last name as the partition key.

	public class CustomerEntity : TableEntity
	{
	    public CustomerEntity(string lastName, string firstName)
	    {
	        this.PartitionKey = lastName;
	        this.RowKey = firstName;
	    }

	    public CustomerEntity() { }

	    public string Email { get; set; }

	    public string PhoneNumber { get; set; }
	}

Table operations involving entities are done using the **CloudTable** object that you created earlier in "Access tables in code." The **TableOperation** object represents the operation to be done. The following code example shows how to create a **CloudTable** object and a **CustomerEntity** object. To prepare the operation, a **TableOperation** is created to insert the customer entity into the table. Finally, the operation is executed by calling **CloudTable.ExecuteAsync**.

	// Get a reference to the **CloudTable** object named 'peopleTable' as described in "Access a table in code".

	// Create a new customer entity.
	CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
	customer1.Email = "Walter@contoso.com";
	customer1.PhoneNumber = "425-555-0101";

	// Create the TableOperation that inserts the customer entity.
	TableOperation insertOperation = TableOperation.Insert(customer1);

	// Execute the insert operation.
	await peopleTable.ExecuteAsync(insertOperation);

##Insert a batch of entities

You can insert multiple entities into a table in a single write operation. The following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them to a **TableBatchOperation** object using the Insert method, and then starts the operation by calling **CloudTable.ExecuteBatchAsync**.

	// Get a reference to a **CloudTable** object named 'peopleTable' as described in "Access a table in code".

	// Create the batch operation.
	TableBatchOperation batchOperation = new TableBatchOperation();

	// Create a customer entity and add it to the table.
	CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
	customer1.Email = "Jeff@contoso.com";
	customer1.PhoneNumber = "425-555-0104";

	// Create another customer entity and add it to the table.
	CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
	customer2.Email = "Ben@contoso.com";
	customer2.PhoneNumber = "425-555-0102";

	// Add both customer entities to the batch insert operation.
	batchOperation.Insert(customer1);
	batchOperation.Insert(customer2);

	// Execute the batch operation.
	await peopleTable.ExecuteBatchAsync(batchOperation);

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

##Add an entity to a table

To add an entity to a table you create a class that defines the properties of your entity. The following code defines an entity class called `CustomerEntity` that uses the customer's first name as the row key and last name as the partition key.

    public class CustomerEntity : TableEntity
    {
         public CustomerEntity(string lastName, string firstName)
         {
             this.PartitionKey = lastName;
             this.RowKey = firstName;
         }

         public CustomerEntity() { }

         public string Email { get; set; }

         public string PhoneNumber { get; set; }
    }

Table operations involving entities are done using the `CloudTable` object you created earlier in "Access tables in code." The `TableOperation` object represents the operation to be done. The following code example shows how to create a `CloudTable` object and a `CustomerEntity` object. To prepare the operation, a `TableOperation` is created to insert the customer entity into the table. Finally, the operation is executed by calling CloudTable.ExecuteAsync.

    // Get a reference to the CloudTable object named 'peopleTable' as described in "Access a table in code".

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

##Insert a batch of entities

You can insert multiple entities into a table in a single write operation. The following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them to a `TableBatchOperation` object using the Insert method, and then starts the operation by calling `CloudTable.ExecuteBatchAsync`.

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code".

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

##Get all of the entities in a partition

To query a table for all of the entities in a partition, use a `TableQuery` object. The following code example specifies a filter for entities where 'Smith' is the partition key. This example prints the fields of each entity in the query results to the console.

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code".

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
    	TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
		token = resultSegment.ContinuationToken;

		foreach (CustomerEntity entity in resultSegment.Results)
    	{
    		Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
    		entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


##Get a single entity

You can write a query to get a single, specific entity. The following code uses a **TableOperation** object to specify a customer named 'Ben Smith'. This method returns just one entity, rather than a collection, and the returned value in **TableResult.Result** is a **CustomerEntity** object. Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the **Table** service.

	// Get a reference to a **CloudTable** object named 'peopleTable' as described in "Access a table in code".

	// Create a retrieve operation that takes a customer entity.
	TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

	// Execute the retrieve operation.
	TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

	// Print the phone number of the result.
	if (retrievedResult.Result != null)
	   Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
	else
	   Console.WriteLine("The phone number could not be retrieved.");

##Delete an entity
You can delete an entity after you find it. The following code looks for a customer entity named "Ben Smith", and if it finds it, it deletes it.

	// Get a reference to a **CloudTable** object named 'peopleTable' as described in "Access a table in code".

	// Create a retrieve operation that expects a customer entity.
	TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

	// Execute the operation.
	TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

	// Assign the result to a CustomerEntity object.
	CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

	// Create the Delete TableOperation and then execute it.
	if (deleteEntity != null)
	{
	   TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

	   // Execute the operation.
	   await peopleTable.ExecuteAsync(deleteOperation);

	   Console.WriteLine("Entity deleted.");
	}

	else
	   Console.WriteLine("Couldn't delete the entity.");

##Next steps

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

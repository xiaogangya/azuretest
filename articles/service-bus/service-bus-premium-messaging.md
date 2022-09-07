<properties
	pageTitle="Service Bus Premium and Standard Messaging tiers overview | Microsoft Azure"
	description="Service Bus Premium and Standard Messaging"
	services="service-bus"
	documentationCenter=".net"
	authors="djrosanova"
	manager="timlt"
	editor=""/>

<tags
	ms.service="service-bus"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="09/03/2015"
	ms.author="sethm"/>

# Service Bus Premium and Standard Messaging Tiers 

Service Bus brokered messaging, which includes messaging entities such as queues and topics, combines enterprise messaging capabilities with rich publish-subscribe semantics at cloud scale. Service Bus brokered messaging is used as the communication backbone for many sophisticated cloud solutions.

With the introduction of the *Premium* tier of Service Bus messaging, we are addressing common customer requests around scale, performance, and availability for mission-critical applications. Although the feature sets are nearly identical, these two tiers of Azure Service Bus messaging are designed to serve different use cases.

Some high-level differences are highlighted in the table below.

| Premium                               | Standard                       |
|---------------------------------------|--------------------------------|
| High throughput                       | Variable throughput            |
| Predictable performance               | Variable latency               |
| Predictable pricing                   | Pay as you go variable pricing |
| Ability to scale up and down workload | N/A                            |

**Azure Service Bus Premium Messaging** provides resource isolation at the CPU and memory layer so that each customer workload runs in isolation. This resource container is called a *messaging unit*. Each premium namespace is allocated at least one messaging unit. You can purchase 1, 2, or 4 messaging units for each Service Bus Premium namespace. A single workload or entity can span multiple messaging units and the number of messaging units can be changed at will, although billing is in 24-hour or daily rate charges. The result is predictable and repeatable performance for your Service Bus-based solution.

Not only is this performance more predictable and available, but it is also faster. Azure Service Bus Premium messaging builds on the storage engine introduced in [Azure Event Hubs](http://azure.microsoft.com/services/event-hubs/). With Premium messaging, peak performance is much faster than the Standard tier.

## Premium Messaging technical differences

The following are a few differences between Premium and Standard messaging tiers.

### Partitioned entities

Partitioned entities are supported in Premium messaging, but they do not function the same way as in the Standard and Basic tiers of Service Bus messaging. Premium messaging does not use SQL as a data store and no longer has the possible resource competition associated with a shared platform. As a result, partitioning is not necessary. Additionally, the partition count has been changed from 16 partitions in Standard messaging to two partitions in Premium. Having 2 partitions ensures availability and is a more appropriate number for the Premium runtime environment. For more information, see [Partitioning Messaging Entities](https://msdn.microsoft.com/library/dn520246.aspx).

### Express entities

Because it runs in a completely isolated runtime environment, there is no longer a need for express entities in Premium messaging. Consequently, express entities are not supported in Premium namespaces. For more information about the express feature, see the [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) property.

## Next steps

To learn more about Service Bus messaging, see the following topics.

- [Introducing Azure Service Bus Premium messaging (blog post)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Introducing Azure Service Bus Premium messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Service Bus messaging overview](service-bus-messaging-overview.md)
- [Azure Service Bus Architectural Overview](fundamentals-service-bus-hybrid-solutions.md)
- [How to use Service Bus queues](service-bus-dotnet-how-to-use-queues.md)

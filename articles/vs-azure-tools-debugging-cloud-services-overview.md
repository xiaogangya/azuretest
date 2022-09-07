<properties 
   pageTitle="Debugging Azure Cloud Services | Microsoft Azure"
   description="Debugging Azure Cloud Services"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="patshea123"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/14/2015"
   ms.author="patshea" />

# Debugging cloud services

You can use different approaches to debug an Azure application by using the Azure Tools for Microsoft Visual Studio and the Azure SDK:

- You can debug an Azure application from Visual Studio when you are developing it, just as you would any Visual C# or Visual Basic application. For more information, see [Debugging a cloud service or virtual machine in Visual Studio](http://go.microsoft.com/fwlink/p/?LinkID=623018).

- You can use Azure Diagnostics to log detailed information from code running within roles, whether the roles are running in the development environment or in Azure. For more information, see [Collecting logging data by using Azure Diagnostics](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- If you are using Visual Studio Enterprise to write roles targeted at the .NET Framework 4 or the .NET Framework 4.5, you can enable IntelliTrace at the time that you deploy an Azure cloud service from Visual Studio. IntelliTrace provides a log that you can use with Visual Studio to debug your application as if it were running in Azure. For more information, see [Debugging a published cloud service with IntelliTrace and Visual Studio]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- You can enable remote debugging on your cloud services at the time when you deploy the cloud service from Visual Studio. If you choose to enable remote debugging for a deployment, remote debugging services are installed on the virtual machines that run each role instance. These services, such as msvsmon.exe, do not affect performance or result in extra costs. For more information, see [Debugging a cloud service or virtual machine in Visual Studio](http://go.microsoft.com/fwlink/p/?LinkID=623018).




test

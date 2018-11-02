---
title: Azure Event Hubs-Beispiele | Microsoft Docs
description: Azure Event Hubs-Beispiele
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2018
ms.author: shvija
ms.openlocfilehash: ed337689360428d66657e5391ee52bb0ae39dc14
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365644"
---
# <a name="event-hubs-samples"></a>Event Hubs-Beispiele 
Sie finden Event Hubs-Beispiele auf [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples). Mit diesen Beispielen werden wichtige Features in [Azure Event Hubs](/azure/event-hubs/) veranschaulicht. Dieser Artikel kategorisiert und beschreibt die verfügbaren Beispiele und stellt Links zu jedem der Beispiele zur Verfügung.

## <a name="net-samples"></a>Beispiele für .NET

| Name des Beispiels | BESCHREIBUNG | 
| ----------- | ----------- | 
| [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) | Dieses Beispiel zeigt, wie Sie eine .NET Core-Konsolenanwendung schreiben können, die eine Reihe von Ereignissen an einen Event Hub sendet. |
| [SampleEHReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) | In diesem Beispiel wird veranschaulicht, wie Sie eine .NET Core-Konsolenanwendung zum Empfangen eines Ereignissatzes von einem Event Hub mithilfe der Bibliothek des Ereignisprozessorhosts schreiben.  | 

## <a name="java-samples"></a>Beispiele für Java

| Name des Beispiels | BESCHREIBUNG | 
| ----------- | ----------- | 
| [SendBatch](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SendBatch)  | In diesem Beispiel wird veranschaulicht, wie Batches mit Ereignissen in Ihrem Event Hub erfasst werden. | 
| [SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) | In diesem Beispiel wird veranschaulicht, wie Ereignisse in Ihrem Event Hub erfasst werden. |
| [AdvanceSendOptions](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/AdvancedSendOptions) | In diesem Beispiel werden die verschiedenen Optionen veranschaulicht, die für Event Hubs zum Erfassen von Ereignissen verfügbar sind. |
| [ReceiveByDateTime](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveByDateTime) | In diesem Beispiel wird veranschaulicht, wie Ereignisse von einer Event Hub-Partition empfangen werden, indem ein spezifischer Datums-/Uhrzeit-Offset verwendet wird. |
| [ReceiveUsingOffset](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingOffset) | In diesem Beispiel wird veranschaulicht, wie Ereignisse von einer Event Hub-Partition empfangen werden, indem ein spezifischer Datenoffset verwendet wird. |  
| [ReceiveUsingSequenceNumber](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingSequenceNumber) | In diesem Beispiel wird veranschaulicht, wie Sie Daten von Event Hub-Partitionen empfangen, indem Sie eine Sequenznummer verwenden. |   
| [EventProcessorSample](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/EventProcessorSample) |In diesem Beispiel wird veranschaulicht, wie Sie Ereignisse von einem Event Hub empfangen, indem Sie den Ereignisprozessorhost verwenden. Auf diese Weise werden die automatische Partitionsauswahl und das Failover über mehrere gleichzeitige Empfänger ermöglicht. | 
| [AutoScaleOnIngress](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/AutoScaleOnIngress) | In diesem Beispiel wird veranschaulicht, wie ein Event Hub bei hohen Lasten automatisch zentral hochskaliert werden kann. Im Beispiel werden Ereignisse mit einer Rate gesendet, die die konfigurierte Rate eines Event Hub gerade überschreitet, sodass der Event Hub zentral hochskaliert wird. | 
| [IngressBenchmark](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/IngressBenchmark) | Dieses Beispiel ermöglicht das Messen der Erfassungsrate. | 

## <a name="python-samples"></a>Python-Beispiele
Sie finden Python-Beispiele für Azure Event Hubs im [azure-event-hubs-python](https://github.com/Azure/azure-event-hubs-python/tree/master/examples)-GitHub-Repository.

## <a name="nodejs-samples"></a>Beispiele für Node.js
Sie finden Node.js-Beispiele für Azure Event Hubs im [azure-event-hubs-node](https://github.com/Azure/azure-event-hubs-node)-GitHub-Repository.

## <a name="go-samples"></a>Go-Beispiele
Sie finden Go-Beispiele für Azure Event Hubs im [azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples)-GitHub-Repository.

## <a name="azure-cli-samples"></a>Azure CLI-Beispiele
Sie finden Azure CLI-Beispiele für Azure Event Hubs im [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/CLI)-GitHub-Repository.

## <a name="azure-powershell-samples"></a>Azure PowerShell-Beispiele
Sie finden Azure PowerShell-Beispiele für Azure Event Hubs im [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/PowerShell)-GitHub-Repository.
 


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie in den folgenden Artikeln:

- [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
- [Event Hubs-Funktionen](event-hubs-features.md)
- [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
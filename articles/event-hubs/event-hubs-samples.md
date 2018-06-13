---
title: Azure Event Hubs-Beispiele | Microsoft Docs
description: Azure Event Hubs-Beispiele
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: sethm
ms.openlocfilehash: a581b7039a3631b7f1dc35816175242f892bd7dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
ms.locfileid: "29802786"
---
# <a name="event-hubs-samples"></a>Event Hubs-Beispiele 

Die Azure Event Hubs-Beispiele veranschaulichen wichtige Features in [Azure Event Hubs](/azure/event-hubs/). Dieser Artikel kategorisiert und beschreibt die verfügbaren Beispiele und stellt Links zu jedem der Beispiele zur Verfügung.

Zum Zeitpunkt der Erstellung dieses Dokuments befinden sich Event Hubs an verschiedenen Orten:

- [MSDN-Codebeispiele für Entwickler](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

Weitere Informationen zu den unterschiedlichen Versionen von .NET Framework finden Sie unter [Frameworks und Ziele](/dotnet/articles/standard/frameworks).

Weitere Beispiele werden im Laufe der Zeit hinzugefügt. Prüfen Sie hier daher regelmäßig, ob Updates verfügbar sind.

## <a name="net-standard"></a>.NET Standard

Die folgenden Beispiele veranschaulichen, wie das Senden und Empfangen von Ereignissen mithilfe des [Event Hubs-Clients](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) für die [.NET Standard-Bibliothek](/dotnet/articles/standard/library) funktioniert.

### <a name="send-events"></a>Senden von Ereignisse 

Das Beispiel [Get started sending](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) (Erste Schritte beim Versenden) zeigt, wie Sie eine .NET Core-Konsolenanwendung schreiben, die Ereignisse an einen Event Hub sendet.

### <a name="receive-events"></a>Empfangen von Ereignissen 

Das Beispiel [Get started receiving with the Event Processor Host](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) (Erste Schritte beim Empfangen mit dem Ereignisprozessorhost) ist eine .NET Core-Konsolenanwendung, die mithilfe des Ereignisprozessorhosts Nachrichten von einem Event Hub empfängt.

## <a name="net-framework"></a>.NET Framework   

Diese Beispiele veranschaulichen weitere Features von Azure Event Hubs, deren Schwerpunkt auf der [.NET Framework-Bibliothek](/dotnet/framework/index) liegt.
 
### <a name="notify-users-of-events-received"></a>Benachrichtigen von Benutzern über empfangene Ereignisse

Das Beispiel [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) benachrichtigt Benutzer über Daten, die von Sensoren oder anderen Systemen empfangen wurden.

### <a name="get-started-with-event-hubs"></a>Erste Schritte mit Event Hubs 

Das Beispiel [Event Hubs Getting Started](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) (Erste Schritte mit Event Hubs) veranschaulicht grundlegende Funktionen von Event Hubs wie das Erstellen eines Event Hubs, Senden von Ereignissen an einen Event Hub und Nutzen von Ereignissen unter Verwendung des [Ereignisprozessorhosts](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).

### <a name="scale-out-event-processing"></a>Horizontales Hochskalieren der Ereignisverarbeitung 

Das Beispiel [Scale out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) (Horizontales Hochskalieren der Ereignisverarbeitung) veranschaulicht die Verwendung des [Ereignisprozessorhosts](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) zum Verteilen der Workloads für die Event Hubs-Datenstromnutzung. Es zeigt zudem, wie die Objekte **EventProcessor** und **EventProcessorFactory** zum Verwalten des Ereignisdatenstroms implementiert werden. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu .NET Framework-Versionen finden Sie unter den folgenden Links:

- [Frameworks und Ziele](/dotnet/articles/standard/frameworks)
- [.NET Framework 4.6 und 4.5](/dotnet/framework/index)

Weitere Informationen zu Event Hubs finden Sie in den folgenden Artikeln:

- [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
- [Event Hubs-Funktionen](event-hubs-features.md)
- [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
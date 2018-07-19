---
title: Übersicht über die Azure Event Hubs-API | Microsoft-Dokumentation
description: Übersicht über verfügbare Azure Event Hubs-APIs
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2018
ms.author: sethm
ms.openlocfilehash: bcd570d5fd2bdcf6cd344f5c81902420163b87ae
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37434540"
---
# <a name="available-event-hubs-apis"></a>Verfügbare Event Hubs-APIs

In diesem Artikel werden die verschiedenen verfügbaren API-Clients beschrieben, die Sie zum Verwalten von Event Hubs-Ressourcen verwenden können.

## <a name="runtime-apis"></a>Laufzeit-APIs

Im folgenden Abschnitt finden Sie eine Beschreibung aller derzeit für Azure Event Hubs verfügbaren Laufzeitclients. Einige dieser Bibliotheken enthalten zwar eingeschränkte Verwaltungsfunktionen, es gibt jedoch auch [bestimmte Bibliotheken](#management-apis) speziell für Verwaltungsvorgänge. Der Schwerpunkt dieser Bibliotheken liegt auf dem Senden und Empfangen von Nachrichten von einem Event Hub.

Weitere Informationen zum aktuellen Status der einzelnen Laufzeitbibliotheken finden Sie in den [zusätzlichen Informationen](#additional-information).

| Sprache/Plattform | Clientpaket | EventProcessorHost-Paket | Repository |
| --- | --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET Framework | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | N/V |
| Java | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Knoten | [NPM](https://www.npmjs.com/package/azure-event-hubs) | N/V | [GitHub](https://github.com/Azure/azure-event-hubs-node) |
| C | N/V | N/V | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>Zusätzliche Informationen

#### <a name="net"></a>.NET

Das .NET-Ökosystem enthält mehrere Laufzeiten, weshalb mehrere .NET-Bibliotheken für Event Hubs zur Verfügung stehen. Die .NET Standard-Bibliothek kann entweder mit .NET Core oder .NET Framework ausgeführt werden. Die .NET Framework-Bibliothek kann hingegen nur in einer .NET Framework-Umgebung ausgeführt werden. Weitere Informationen zu den .NET Framework-Versionen finden Sie unter [Framework-Versionen](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).

#### <a name="node"></a>Knoten

Die [Node.js](https://github.com/Azure/azure-event-hubs-node)-Bibliothek befindet sich derzeit in der Vorschauphase und wird von Microsoft-Mitarbeitern und externen Mitwirkenden nebenbei betrieben. Alle Beiträge, auch Quellcode, sind willkommen und werden überprüfen.

## <a name="management-apis"></a>Verwaltungs-APIs

In der folgenden Tabelle finden Sie eine Liste aller derzeit verfügbaren verwaltungsspezifischen Bibliotheken. Keine dieser Bibliotheken enthält Laufzeitvorgänge. Sie dienen ausschließlich zur Verwaltung von Event Hubs-Entitäten.

| Sprache/Plattform | Verwaltungspaket | Repository |
| --- | --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
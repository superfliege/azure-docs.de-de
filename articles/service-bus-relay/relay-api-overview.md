---
title: "Übersicht über die Azure Relay-API | Microsoft-Dokumentation"
description: "Übersicht über verfügbare Azure Relay-APIs"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 60413bd110fd4021b070f94d6338a50e1b321dea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="available-relay-apis"></a>Verfügbare Relay-APIs

## <a name="runtime-apis"></a>Laufzeit-APIs

In der folgenden Tabelle sind alle derzeit verfügbaren Relay-Laufzeitclients aufgeführt.

Der Abschnitt [Zusätzliche Informationen](#additional-information) enthält weitere Informationen zum Status der einzelnen Laufzeitbibliotheken.

| Sprache/Plattform | Verfügbares Feature | Clientpaket | Repository |
| --- | --- | --- | --- |
| .NET Standard | Hybridverbindungen | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | WCF-Relay | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | N/V |
| Knoten | Hybridverbindungen | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Zusätzliche Informationen

#### <a name="net"></a>.NET
Das .NET-Ökosystem enthält mehrere Laufzeiten, daher stehen mehrere .NET-Bibliotheken für Event Hubs zur Verfügung. Die .NET Standard-Bibliothek kann entweder mit .NET Core oder .NET Framework ausgeführt werden. Die .NET Framework-Bibliothek kann hingegen nur in einer .NET Framework-Umgebung ausgeführt werden. Weitere Informationen zu .NET Frameworks finden Sie unter [Framework-Versionen](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Relay finden Sie unter diesen Links:
* [Was ist Azure Relay?](relay-what-is-it.md)
* [Relay – Häufig gestellte Fragen](relay-faq.md)
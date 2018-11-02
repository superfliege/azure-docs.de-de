---
title: Leistungsüberwachung in Azure Service Fabric | Microsoft-Dokumentation
description: Erfahren Sie mehr zu Leistungsindikatoren zum Überwachen und für Diagnosen von Azure Service Fabric-Clustern.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 1e6ea5d6ae321a0443631ec928912611a68346c6
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408012"
---
# <a name="performance-metrics"></a>Leistungsmetriken

Sie sollten Metriken erfassen, um einen besseren Einblick in die Leistung Ihrer Cluster sowie der darin ausgeführten Anwendungen zu erhalten. Es wird empfohlen, für Service Fabric-Cluster die folgenden Leistungsindikatoren zu erfassen.

## <a name="nodes"></a>Nodes

Für die Computer in Ihrem Cluster sollten Sie das Erfassen der folgenden Leistungsindikatoren in Erwägung ziehen, um Einblick in die Last jedes Computers zu erhalten und entsprechende Entscheidungen zur Skalierung des Clusters treffen zu können.

| Indikatorkategorie | Name des Leistungsindikators |
| --- | --- |
| PhysicalDisk (pro Datenträger) | Durchschn. Warteschlangenlänge der Datenträger-Lesevorgänge |
| PhysicalDisk (pro Datenträger) | Durchschn. Warteschlangenlänge der Datenträger-Schreibvorgänge |
| PhysicalDisk (pro Datenträger) | Durchschn. Datenträger s/gelesen |
| PhysicalDisk (pro Datenträger) | Durchschn. Datenträger s/geschrieben |
| PhysicalDisk (pro Datenträger) | Lesevorgänge/s  |
| PhysicalDisk (pro Datenträger) | Byte gelesen/s  |
| PhysicalDisk (pro Datenträger) | Schreibvorgänge/s |
| PhysicalDisk (pro Datenträger) | Byte geschrieben/s |
| Arbeitsspeicher | Verfügbare MB |
| PagingFile | Prozent genutzt |
| Prozessor (gesamt) | % Prozessorzeit |
| Prozess (pro Dienst) | % Prozessorzeit |
| Prozess (pro Dienst) | Prozess-ID |
| Prozess (pro Dienst) | Private Bytes |
| Prozess (pro Dienst) | Threadanzahl |
| Prozess (pro Dienst) | Virtuelle Bytes |
| Prozess (pro Dienst) | Arbeitssatz |
| Prozess (pro Dienst) | Arbeitsseiten (privat) |
| Netzwerkschnittstelle (alle Instanzen) | Länge der Ausgabewarteschlange |
| Netzwerkschnittstelle (alle Instanzen) | Verworfene ausgehende Pakete |
| Netzwerkschnittstelle (alle Instanzen) | Verworfene empfangene Pakete |
| Netzwerkschnittstelle (alle Instanzen) | Ausgehende Pakete mit Fehlern |
| Netzwerkschnittstelle (alle Instanzen) | Empfangene Pakete mit Fehlern |

## <a name="net-applications-and-services"></a>.NET-Anwendungen und -Dienste

Erfassen Sie die folgenden Leistungsindikatoren, wenn Sie .NET-Dienste an Ihren Cluster bereitstellen. 

| Indikatorkategorie | Name des Leistungsindikators |
| --- | --- |
| .NET CLR Memory (pro Dienst) | Prozess-ID |
| .NET CLR Memory (pro Dienst) | Anzahl von zugesicherten Bytes |
| .NET CLR Memory (pro Dienst) | Anzahl von reservierten Bytes |
| .NET CLR Memory (pro Dienst) | Anzahl von Bytes in allen Heaps |
| .NET CLR Memory (pro Dienst) | Anzahl von Gen 0-Sammlungen |
| .NET CLR Memory (pro Dienst) | Anzahl von Gen 1-Sammlungen |
| .NET CLR Memory (pro Dienst) | Anzahl von Gen 2-Sammlungen |
| .NET CLR Memory (pro Dienst) | GC-Zeitdauer in Prozent |

### <a name="service-fabrics-custom-performance-counters"></a>Benutzerdefinierte Leistungsindikatoren von Service Fabric

Service Fabric generiert eine beträchtliche Menge an benutzerdefinierten Leistungsindikatoren. Wenn Sie das SDK installiert haben, sehen Sie eine umfassende Liste in Ihrer Leistungsmonitoranwendung auf Ihrem Windows-Computer (Start > Leistungsmonitor). 

In der von Ihnen an Ihren Cluster bereitgestellten Anwendung können Sie Indikatoren über die Kategorien `Service Fabric Actor` und `Service Fabric Actor Method` hinzufügen, wenn Sie Reliable Actors verwenden (weitere Informationen finden Sie unter [Diagnose und Leistungsüberwachung für Reliable Actors](service-fabric-reliable-actors-diagnostics.md)).

Wenn Sie Reliable Services verwenden, gibt es dem entsprechend die Indikatorkategorien `Service Fabric Service` und `Service Fabric Service Method`, aus denen Sie Indikatoren erfassen können. 

Wenn Sie Reliable Collections verwenden, wird empfohlen, den `Avg. Transaction ms/Commit` aus dem `Service Fabric Transactional Replicator` hinzuzufügen, um die durchschnittliche Wartezeit pro Transaktion zu erfassen.


## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [die Ereignisgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric.
* Erfassen von Leistungsmetriken über den [Log Analytics-Agent](service-fabric-diagnostics-oms-agent.md)

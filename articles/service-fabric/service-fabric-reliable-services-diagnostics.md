---
title: "Diagnose für zustandsbehaftete zuverlässige Dienste in Azure Service Fabric | Microsoft-Dokumentation"
description: "Diagnosefunktionen für zustandsbehaftete zuverlässige Dienste in Azure Service Fabric"
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: edcaaaf8f1619082b33195aedf1fb1abf32e85b1
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnosefunktionen für zustandsbehaftete Reliable Services
Die StatefulServiceBase-Klasse der zustandsbehafteten zuverlässigen Dienste in Azure Service Fabric gibt [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)-Ereignisse aus. Diese können verwendet werden, um den Dienst zu debuggen, Einblicke in den Laufzeitbetrieb zu erhalten und Fehler zu beheben.

## <a name="eventsource-events"></a>EventSource-Ereignisse
Der Name des EventSource-Ereignisses für die Klasse „StatefulServiceBase“ der zustandsbehafteten zuverlässigen Dienste lautet „Microsoft-ServiceFabric-Services“. Ereignisse aus dieser Ereignisquelle werden beim [Debuggen des Diensts in Visual Studio](service-fabric-debugging-your-application.md) im Fenster für [Diagnoseereignisse](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) angezeigt.

Beispiele für Tools und Technologien, mit deren Hilfe EventSource-Ereignisse erfasst bzw. angezeigt werden können, sind [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) und [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Ereignisse
| Ereignisname | Ereignis-ID | Ebene | Ereignisbeschreibung |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Information |Wird beim Start der RunAsync-Dienstaufgabe ausgegeben |
| StatefulRunAsyncCancellation |2 |Information |Wird beim Abbruch der RunAsync-Dienstaufgabe ausgegeben |
| StatefulRunAsyncCompletion |3 |Information |Wird nach Abschluss der RunAsync-Dienstaufgabe ausgegeben |
| StatefulRunAsyncSlowCancellation |4 |Warnung |Wird ausgegeben, wenn das Abbrechen der RunAsync-Dienstaufgabe zu lange dauert |
| StatefulRunAsyncFailure |5 |Error |Wird bei einer Ausnahme der RunAsync-Dienstaufgabe ausgegeben |

## <a name="interpret-events"></a>Interpretieren von Ereignissen
Die Ereignisse „StatefulRunAsyncInvocation“, „StatefulRunAsyncCompletion“ und „StatefulRunAsyncCancellation“ sind für den Dienstautor nützlich, um den Lebenszyklus eines Diensts sowie die zeitliche Steuerung des Starts, Abbruchs oder Abschlusses eines Diensts zu verstehen. Diese Informationen können hilfreich sein, um Probleme bei Diensten zu debuggen oder den Dienstlebenszyklus zu verstehen.

Dienstautoren sollten unbedingt auf die Ereignisse StatefulRunAsyncSlowCancellation und StatefulRunAsyncFailure achten, da sie auf Probleme mit dem Dienst hinweisen.

StatefulRunAsyncFailure wird ausgegeben, wenn RunAsync() eine Ausnahme ausgibt. In der Regel weist eine Ausnahme auf einen Fehler oder Bug im Dienst hin. Darüber hinaus bewirkt die Ausnahme, dass der Dienst nicht ausgeführt wird, sodass er auf einen anderen Knoten verschoben wird. Dieser Vorgang kann aufwendig sein und eingehende Anforderungen verzögern, während der Dienst verschoben wird. Dienstautoren sollten nach Möglichkeit die Ursache der Ausnahme ermitteln und beheben.

StatefulRunAsyncSlowCancellation wird ausgegeben, wenn eine Abbruchanforderung für RunAsync länger als vier Sekunden dauert. Wenn das Abbrechen eines Diensts zu lange dauert, beeinträchtigt dies seine Fähigkeit, umgehend auf einem anderen Knoten neu gestartet werden zu können. Dieses Szenario kann sich negativ auf die allgemeine Verfügbarkeit des Diensts auswirken.

## <a name="next-steps"></a>Nächste Schritte
[EventSource-Anbieter in PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)

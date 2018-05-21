---
title: Überwachung auf Azure Service Fabric-Anwendungsebene | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über Ereignisse und Protokolle auf Anwendungs- und Dienstebene, die zum Überwachen und Diagnostizieren von Azure Service Fabric-Clustern verwendet werden.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: b8118d83e2be452c6aa5bbc8b7a3c220d26903a1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
---
# <a name="application-and-service-level-logging"></a>Protokollierung auf Anwendungs- und Dienstebene

Das Instrumentieren des Codes ist die Grundlage für die meisten anderen Aspekte bei der Überwachung Ihrer Dienste. Die Instrumentierung ist die einzige Möglichkeit, wie Sie Fehler erkennen und die Diagnose zur Fehlerbehebung stellen können. Es ist zwar technisch möglich, einen Debugger mit einem Produktionsdienst zu verbinden, aber dies ist keine gängige Vorgehensweise. Es ist also wichtig, dass Sie über ausführliche Instrumentierungsdaten verfügen.

Bei einigen Produkten wird Ihr Code automatisch instrumentiert. Obwohl diese Lösungen ggf. gut funktionieren, ist fast immer eine manuelle Instrumentierung erforderlich. Letztlich benötigen Sie ausreichend Informationen für das genaue Debuggen der Anwendung. In diesem Dokument werden die verschiedenen Verfahren für das Instrumentieren des Codes und die jeweiligen Vor- und Nachteile für bestimmte Fälle beschrieben.

Beispiele zur Verwendung dieser Vorschläge finden Sie unter [Hinzufügen von Protokollierung zur Service Fabric-Anwendung](service-fabric-how-to-diagnostics-log.md).

## <a name="eventsource"></a>EventSource

Wenn Sie eine Service Fabric-Lösung aus einer Vorlage in Visual Studio erstellen, wird eine von **EventSource** abgeleitete Klasse (**ServiceEventSource** oder **ActorEventSource**) generiert. Es wird eine Vorlage erstellt, in der Sie Ereignisse für Ihre Anwendung oder Ihren Dienst hinzufügen können. Der **EventSource**-Name **muss** eindeutig sein. Er sollte auf der Standardvorlage „MyCompany-&lt;Projektmappe&gt;-&lt;Projekt&gt;“ basieren und entsprechend umbenannt werden. Mehrere **EventSource**-Definitionen mit dem gleichen Namen führen zur Laufzeit zu Problemen. Jedes definierte Ereignis muss über einen eindeutigen Bezeichner verfügen. Wenn ein Bezeichner nicht eindeutig ist, tritt ein Laufzeitfehler auf. In einigen Organisationen werden Wertebereiche vorab Bezeichnern zugewiesen, um Konflikte zwischen separaten Entwicklungsteams zu vermeiden. Weitere Informationen finden Sie im [Blog von Vance](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) oder in der [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>ASP.NET Core-Protokollierung

Es ist wichtig, dass Sie sorgfältig planen, wie Sie Ihren Code instrumentieren. Mit dem richtigen Instrumentierungsplan können Sie ggf. verhindern, dass Ihre Codebasis destabilisiert wird und eine erneute Instrumentierung des Codes erforderlich wird. Zur Reduzierung des Risikos können Sie eine Instrumentierungsbibliothek wie [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) wählen, die Teil von Microsoft ASP.NET Core ist. ASP.NET Core verfügt über eine [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)-Schnittstelle, die Sie zusammen mit dem Anbieter Ihrer Wahl nutzen können, während gleichzeitig die Auswirkungen auf den vorhandenen Code minimiert werden. Sie können den Code in ASP.NET Core unter Windows und Linux und im vollständigen .NET Framework verwenden, damit Ihr Instrumentierungscode standardisiert ist.

## <a name="application-insights-sdk"></a>Application Insights SDK

Die umfassende Integration von Application Insights in Service Fabric ist vorkonfiguriert. Benutzer können die KI-NuGet-Pakete von Service Fabric hinzufügen und Daten und Protokolle empfangen, die so erstellt und erfasst wurden, dass sie im Azure-Portal angezeigt werden können. Außerdem wird Benutzern das Hinzufügen ihrer eigenen Telemetriedaten empfohlen, um ihre Anwendungen zu diagnostizieren und zu debuggen, und um nachzuverfolgen, welche Dienste und Teile ihrer Anwendung am häufigsten verwendet werden. Die [TelemetryClient](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient?view=azure-dotnet)-Klasse im SDK bietet viele Methoden zum Nachverfolgen von Telemetriedaten in Ihren Anwendungen. Sehen Sie sich in unserem Tutorial [Überwachen und Diagnostizieren einer ASP.NET Core-Anwendung in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md) ein Beispiel zum Instrumentieren und Hinzufügen von Application Insights zu Ihrer Anwendung an.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Protokollanbieter zum Instrumentieren Ihrer Anwendungen und Dienste ausgewählt haben, müssen Ihre Protokolle und Ereignisse aggregiert werden, bevor sie an eine Analyseplattform gesendet werden können. Lesen Sie die Abschnitte zu [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md), [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) und [WAD](service-fabric-diagnostics-event-aggregation-wad.md), um einige der empfohlenen Optionen besser zu verstehen.

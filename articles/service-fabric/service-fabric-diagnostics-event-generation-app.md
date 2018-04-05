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
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: 258aac722aa1c94ecf2cbf0524a3e4b53b8a788c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="application-and-service-level-logging"></a>Protokollierung auf Anwendungs- und Dienstebene

Das Instrumentieren des Codes ist die Grundlage für die meisten anderen Aspekte bei der Überwachung Ihrer Dienste. Die Instrumentierung ist die einzige Möglichkeit, wie Sie Fehler erkennen und die Diagnose zur Fehlerbehebung stellen können. Es ist zwar technisch möglich, einen Debugger mit einem Produktionsdienst zu verbinden, aber dies ist keine gängige Vorgehensweise. Es ist also wichtig, dass Sie über ausführliche Instrumentierungsdaten verfügen.

Bei einigen Produkten wird Ihr Code automatisch instrumentiert. Obwohl diese Lösungen ggf. gut funktionieren, ist fast immer eine manuelle Instrumentierung erforderlich. Letztlich benötigen Sie ausreichend Informationen für das genaue Debuggen der Anwendung. In diesem Dokument werden die verschiedenen Verfahren für das Instrumentieren des Codes und die jeweiligen Vor- und Nachteile für bestimmte Fälle beschrieben.

## <a name="eventsource"></a>EventSource

Wenn Sie eine Service Fabric-Lösung aus einer Vorlage in Visual Studio erstellen, wird eine von **EventSource** abgeleitete Klasse (**ServiceEventSource** oder **ActorEventSource**) generiert. Es wird eine Vorlage erstellt, in der Sie Ereignisse für Ihre Anwendung oder Ihren Dienst hinzufügen können. Der **EventSource**-Name **muss** eindeutig sein. Er sollte auf der Standardvorlage „MyCompany-&lt;Projektmappe&gt;-&lt;Projekt&gt;“ basieren und entsprechend umbenannt werden. Mehrere **EventSource**-Definitionen mit dem gleichen Namen führen zur Laufzeit zu Problemen. Jedes definierte Ereignis muss über einen eindeutigen Bezeichner verfügen. Wenn ein Bezeichner nicht eindeutig ist, tritt ein Laufzeitfehler auf. In einigen Organisationen werden Wertebereiche vorab Bezeichnern zugewiesen, um Konflikte zwischen separaten Entwicklungsteams zu vermeiden. Weitere Informationen finden Sie im [Blog von Vance](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) oder in der [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>ASP.NET Core-Protokollierung

Es ist wichtig, dass Sie sorgfältig planen, wie Sie Ihren Code instrumentieren. Mit dem richtigen Instrumentierungsplan können Sie ggf. verhindern, dass Ihre Codebasis destabilisiert wird und eine erneute Instrumentierung des Codes erforderlich wird. Zur Reduzierung des Risikos können Sie eine Instrumentierungsbibliothek wie [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) wählen, die Teil von Microsoft ASP.NET Core ist. ASP.NET Core verfügt über eine [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)-Schnittstelle, die Sie zusammen mit dem Anbieter Ihrer Wahl nutzen können, während gleichzeitig die Auswirkungen auf den vorhandenen Code minimiert werden. Sie können den Code in ASP.NET Core unter Windows und Linux und im vollständigen .NET Framework verwenden, damit Ihr Instrumentierungscode standardisiert ist.

## <a name="choosing-a-logging-provider"></a>Auswählen eines Protokollierungsanbieters

Wenn Sie für Ihre Anwendung eine hohe Leistung benötigen, ist **EventSource** normalerweise ein geeigneter Ansatz. Für **EventSource** werden *im Allgemeinen* weniger Ressourcen verwendet, und es wird eine bessere Leistung als mit der ASP.NET Core-Protokollierung oder den erhältlichen Drittanbieterlösungen erzielt.  Dies ist für zahlreiche Dienste kein Problem, aber wenn Ihr Dienst leistungsorientiert ist, kann **EventSource** die bessere Wahl sein. Um die gleichen Vorteile wie bei der strukturierten Protokollierung zu erhalten, fällt für Ihre Entwickler bei **EventSource** jedoch ein höherer Aufwand an. Wenn möglich, führen Sie eine schnelle „Prototyperstellung“ für einige Protokollierungsoptionen durch, und wählen Sie dann die Option aus, die für Ihre Anforderungen am besten geeignet ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Protokollanbieter zum Instrumentieren Ihrer Anwendungen und Dienste ausgewählt haben, müssen Ihre Protokolle und Ereignisse aggregiert werden, bevor sie an eine Analyseplattform gesendet werden können. Lesen Sie die Artikel zu [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) und [WAD](service-fabric-diagnostics-event-aggregation-wad.md), um einige der empfohlenen Optionen besser zu verstehen.

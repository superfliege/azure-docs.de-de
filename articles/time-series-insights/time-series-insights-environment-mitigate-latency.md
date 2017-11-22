---
title: "Überwachen der Leistung, Reduzieren der Drosselung und Verhindern von Latenz in Azure Time Series Insights | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Leistungsprobleme überwacht, diagnostiziert und verringert werden können, die zu Latenz und Drosselung in Azure Time Series Insights führen."
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/15/2017
ms.openlocfilehash: 5af5589922ded802703b9ba8f3b0aa8c5524bbad
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="improve-performance-reduce-latency-and-mitigate-throttling-in-azure-time-series-insights"></a>Verbessern der Leistung, Reduzieren der Latenz und Verringern der Drosselung in Azure Time Series Insights
Wenn die Menge der eingehenden Daten die Konfiguration Ihrer Umgebung übersteigt, kann dies zu Latenz oder Drosselung in Azure Time Series Insights führen.

Sie können Latenz und Drosselung vermeiden, indem Sie die Umgebung korrekt für die Menge der zu analysierenden Daten konfigurieren.

Am wahrscheinlichsten treten Latenz und Drosselung in folgenden Fällen auf:

- Sie fügen eine Ereignisquelle hinzu, die alte Daten enthält, die möglicherweise die zugewiesene Eingangsrate überschreiten. (Time Series Insights muss auf den neuesten Stand gebracht werden.)
- Sie fügen einer Umgebung weitere Ereignisquellen hinzu, was zu einer Spitze durch zusätzliche Ereignisse führt (die die Kapazität der Umgebung überschreiten kann).
- Sie übertragen große Mengen an Verlaufsereignissen in eine Ereignisquelle, was zu einer Verzögerung führt. (Time Series Insights muss auf den neuesten Stand gebracht werden.)
- Sie verknüpfen Verweisdaten mit Telemetriedaten, was zu einer umfangreicheren Ereignisgröße führt.  In Bezug auf die Drosselung wird ein eingehendes Datenpaket mit einer Paketgröße von 32 KB als 32 Ereignisse mit jeweils einer Größe von 1 KB behandelt. Die maximal zulässige Ereignisgröße ist 32 KB. Datenpakete mit einer Größe von über 32 KB werden abgeschnitten.


## <a name="monitor-latency-and-throttling-with-alerts"></a>Überwachen von Latenz und Drosselung mit Warnungen

Mithilfe von Warnungen können Sie durch die Umgebung verursachte Latenzprobleme diagnostizieren und verringern. 

1. Klicken Sie im Azure-Portal auf **Metriken**. 

   ![Metriken](media/environment-mitigate-latency/add-metrics.png)

2. Klicken Sie auf **Metrikwarnung hinzufügen**.  

    ![Metrikwarnung hinzufügen](media/environment-mitigate-latency/add-metric-alert.png)

Hier können Sie Warnungen mithilfe der folgenden Metriken konfigurieren:

|Metrik  |Beschreibung  |
|---------|---------|
|**Ingress Received Bytes** (Eingang empfangene Bytes)     | Anzahl der aus Ereignisquellen gelesenen Rohbytes. Die Anzahl der Rohbytes umfasst normalerweise den Eigenschaftennamen und den Eigenschaftswert.  |  
|**Ingress Received Invalid Messages** (Eingang empfangene ungültige Nachrichten)     | Anzahl der aus allen Azure Event Hubs- oder Azure IoT Hub-Ereignisquellen gelesenen ungültigen Nachrichten.      |
|**Ingress Received Messages** (Eingang empfangene Nachrichten)   | Anzahl der aus allen Event Hubs- oder IoT Hub-Ereignisquellen gelesenen Nachrichten.        |
|**Ingress Stored Bytes** (Eingang gespeicherte Bytes)     | Gesamtgröße der gespeicherten und für Abfragen verfügbaren Ereignisse. Die Größe wird lediglich für den Eigenschaftswert berechnet.        |
|**Ingress Stored Events** (Eingang gespeicherte Ereignisse)     |   Anzahl der gespeicherten und für Abfragen verfügbaren vereinfachten Ereignisse.      |

![Latenz](media/environment-mitigate-latency/latency.png)

Eine Möglichkeit besteht darin, eine Warnung vom Typ **Ingress Stored Events** (Eingang gespeicherte Ereignisse) >= einem Schwellenwert etwas unter der Gesamtkapazität der Umgebung für einen Zeitraum von 2 Stunden festzulegen.  Anhand dieser Warnung können Sie feststellen, ob die Kapazität konstant ausgelastet ist. Dies gibt eine hohe Wahrscheinlichkeit von Latenz an.  

Wenn Sie beispielsweise drei S1-Einheiten (oder eine Eingangskapazität von 2.100 Ereignissen pro Minute) bereitgestellt haben, können Sie eine Warnung vom Typ **Ingress Stored Events** (Eingang gespeicherte Ereignisse) >= 1.900 Ereignisse für 2 Stunden festlegen. Wenn dieser Schwellenwert konstant überschritten und dadurch die Warnung ausgelöst wird, ist die Bereitstellung wahrscheinlich nicht ausreichend.  

Auch wenn Sie vermuten, dass eine Drosselung vorliegt, können Sie die Warnung vom Typ **Ingress Received Messages** (Eingang empfangene Nachrichten) mit den ausgehenden Nachrichten der Ereignisquelle vergleichen.  Wenn der Eingang in den Event Hub größer ist als der Wert für **Ingress Received Messages** (Eingang empfangene Nachrichten), wird der Time Series Insights-Dienst wahrscheinlich gedrosselt.

## <a name="improving-performance"></a>Verbessern der Leistung 
Um die Drosselung oder Latenz zu reduzieren, empfiehlt es sich, dies durch Erhöhen der Kapazität der Umgebung zu beheben. 

Sie können Latenz und Drosselung vermeiden, indem Sie die Umgebung korrekt für die Menge der zu analysierenden Daten konfigurieren. Weitere Informationen zum Hinzufügen von Kapazität zu Ihrer Umgebung finden Sie unter [Skalieren Ihrer Umgebung](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Nächste Schritte
- Weitere Schritte zur Problembehandlung finden Sie unter [Diagnostizieren und Beheben von Problemen in der Time Series Insights-Umgebung](time-series-insights-diagnose-and-solve-problems.md).
- Zusätzliche Unterstützung erhalten Sie im [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) oder bei [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). Zudem können Sie sich an den [Azure-Support](https://azure.microsoft.com/support/options/) wenden, um Supportunterstützung zu erhalten.

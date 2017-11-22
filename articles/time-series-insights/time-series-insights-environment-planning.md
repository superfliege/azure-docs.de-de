---
title: Planen der Skalierung Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: "In diesem Artikel werden bewährte Methoden bei der Planung einer Azure Time Series Insights-Umgebung beschrieben, einschließlich Speicherkapazität, Datenaufbewahrung, Eingangskapazität und Überwachung."
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 5fb158ba162dd199f419f9568de08a7a18c833dd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="plan-your-azure-time-series-insights-environment"></a>Planen Ihrer Azure Time Series Insights-Umgebung

In diesem Artikel wird beschrieben, wie Sie Ihre Azure Time Series Insights-Umgebung basierend auf der erwarteten Eingangsrate und den Anforderungen zur Datenaufbewahrung planen.

## <a name="best-practices"></a>Bewährte Methoden

Für den Einstieg in Time Series Insights ist es am besten, wenn Sie wissen, wie viele Daten erwartungsgemäß pro Minute übertragen werden und wie lange die Daten gespeichert werden sollen.  

Weitere Informationen zur Kapazität und Aufbewahrung für beide Time Series Insights-SKUs finden Sie unter [Time Series Insights – Preise](https://azure.microsoft.com/pricing/details/time-series-insights/).

Berücksichtigen Sie im Hinblick auf den langfristigen Erfolg folgende Faktoren zur optimalen Planung der Umgebung: 
- Speicherkapazität
- Aufbewahrungszeitraum
- Eingangskapazität 

## <a name="understand-storage-capacity"></a>Grundlegendes zur Speicherkapazität
Standardmäßig werden Daten in Time Series Insights basierend auf der bereitgestellten Speichermenge (Einheiten mal Speichermenge pro Einheit) und den eingehenden Daten aufbewahrt.

## <a name="understand-data-retention"></a>Grundlegendes zur Datenaufbewahrung
Sie können die Einstellung **Datenaufbewahrungszeit** Ihrer Time Series Insights-Umgebung konfigurieren, sodass eine Aufbewahrungsdauer von bis zu 400 Tagen möglich ist.  Time Series Insights weist zwei Modi auf: In einem Modus wird sichergestellt, dass Ihre Umgebung über die neuesten Daten verfügt (standardmäßig aktiviert). Im anderen Modus wird sichergestellt, dass die Aufbewahrungslimits eingehalten werden, wobei der Dateneingang unterbrochen wird, wenn die Gesamtspeicherkapazität der Umgebung erreicht ist.  Auf der Konfigurationsseite der Umgebung im Azure-Portal können Sie die Aufbewahrungsdauer anpassen und zwischen den beiden Modi wechseln.

Sie können in Ihrer Time Series Insights-Umgebung eine Datenaufbewahrung von maximal 400 Tagen konfigurieren.

## <a name="configure-data-retention"></a>Konfigurieren der Datenaufbewahrung

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihre Time Series Insights-Umgebung aus.

2. Wählen Sie auf der Seite für die **Time Series Insights-Umgebung** unter der Überschrift **Einstellungen** die Option **Konfigurieren** aus. 

3. Geben Sie im Feld **Datenaufbewahrungszeit (in Tagen)** einen Wert zwischen 1 und 400 ein.

   ![Konfigurieren der Aufbewahrung](media/environment-mitigate-latency/configure-retention.png)

## <a name="understand-ingress-capacity"></a>Grundlegendes zur Eingangskapazität

Der andere wichtige Faktor bei der Planung ist die Eingangskapazität, die eine Ableitung der Zuordnung pro Minute darstellt. 

In Bezug auf die Drosselung wird ein eingehendes Datenpaket mit einer Paketgröße von 32 KB als 32 Ereignisse mit jeweils einer Größe von 1 KB behandelt. Die maximal zulässige Ereignisgröße ist 32 KB. Datenpakete mit einer Größe von über 32 KB werden abgeschnitten.

In der folgenden Tabelle ist die Eingangskapazität für jede SKU zusammengefasst:

|SKU  |Ereignisanzahl pro Monat, pro Einheit  |Ereignisgröße pro Monat, pro Einheit  |Ereignisanzahl pro Minute, pro Einheit  | Größe pro Minute, pro Einheit   |
|---------|---------|---------|---------|---------|
|S1     |   30 Millionen     |  30 GB     |  700    |  700 KB   |
|S2     |   300 Millionen    |   300 GB   | 7.000   | 7.000 KB  |

Sie können die Kapazität einer SKU des Typs S1 oder S2 in einer einzelnen Umgebung auf 10 Einheiten erhöhen. Die Migration von einer S1-Umgebung zu einer S2-Umgebung oder von einer S2-Umgebung zu einer S1-Umgebung ist nicht möglich. 

Für die Eingangskapazität sollten Sie zunächst den Gesamteingang bestimmen, den Sie pro Monat benötigen. Ermitteln Sie anschließend die Anforderungen pro Minute, da dies eine Rolle für Drosselung und Latenz spielt.

Wenn Sie eine Spitze im Dateneingang über einen Zeitraum unter 24 Stunden verzeichnen, kann Time Series Insights dies mit einer Eingangsrate „aufholen“, die doppelt so hoch ist wie die oben aufgeführten Raten. 

Wenn Sie z.B. über eine einzelne SKU des Typs S1 verfügen und Daten mit einer Rate von 700 Ereignissen pro Minute und bei einer Spitze in einem Zeitraum unter 1 Stunde mit einer Rate von maximal 1.400 Ereignissen eingehen, weist Ihre Umgebung keine merkliche Latenz auf. Wenn jedoch 1.400 Ereignisse pro Minute in einem Zeitraum von über einer Stunde überschritten werden, treten für Daten, die in Ihrer Umgebung visualisiert werden und zur Abfrage verfügbar sind, wahrscheinlich Latenzprobleme auf. 

Sie wissen möglicherweise nicht im Voraus, wie viele Daten erwartungsgemäß übertragen werden. In diesem Fall finden Sie die Datentelemetrie für [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) und [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) im Azure-Portal. Anhand dieser Telemetrie können Sie die Bereitstellung Ihrer Umgebung bestimmen. Verwenden Sie die Seite **Metriken** im Azure-Portal für die jeweilige Ereignisquelle, um die zugehörige Telemetrie anzuzeigen. Wenn Sie sich mit den Metriken der Ereignisquelle vertraut gemacht haben, können Sie Ihre Time Series Insights-Umgebung effektiver planen und bereitstellen.

## <a name="calculate-ingress-requirements"></a>Berechnen der Eingangsanforderungen

- Überprüfen Sie, ob die Eingangskapazität über der durchschnittlichen Rate pro Minute liegt und ob die Umgebung groß genug ist, um den voraussichtlichen Eingang zu verarbeiten, der doppelt so hoch ist wie die Kapazität über einen Zeitraum von unter 1 Stunde.

- Wenn Eingangsspitzen auftreten, die länger als 1 Stunde dauern, verwenden Sie die Spitzenrate als Durchschnittswert, und stellen Sie eine Umgebung mit der Kapazität zum Verarbeiten der Spitzenrate bereit.
 
## <a name="mitigate-throttling-and-latency"></a>Beschränken der Drosselung und Latenz

Informationen zum Verhindern der Drosselung und Latenz finden Sie unter [Verringern der Latenz und Drosselung](time-series-insights-environment-mitigate-latency.md). 

## <a name="next-steps"></a>Nächste Schritte
- [Hinzufügen einer Event Hub-Ereignisquelle](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [Hinzufügen einer IoT Hub-Ereignisquelle](time-series-insights-how-to-add-an-event-source-iothub.md)

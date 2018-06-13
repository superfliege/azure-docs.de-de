---
title: Planen der Skalierung Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: In diesem Artikel werden bewährte Methoden bei der Planung einer Azure Time Series Insights-Umgebung beschrieben, einschließlich Speicherkapazität, Datenaufbewahrung, Eingangskapazität und Überwachung.
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 45f081c4e1dbd32b46c8a69f32b0b205b948f9b5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652320"
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
- Gestalten Ihrer Ereignisse
- Sicherstellen des Vorhandenseins von Verweisdaten

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

### <a name="calculate-ingress-requirements"></a>Berechnen der Eingangsanforderungen

- Überprüfen Sie, ob die Eingangskapazität über der durchschnittlichen Rate pro Minute liegt und ob die Umgebung groß genug ist, um den voraussichtlichen Eingang zu verarbeiten, der doppelt so hoch ist wie die Kapazität über einen Zeitraum von unter 1 Stunde.

- Wenn Eingangsspitzen auftreten, die länger als 1 Stunde dauern, verwenden Sie die Spitzenrate als Durchschnittswert, und stellen Sie eine Umgebung mit der Kapazität zum Verarbeiten der Spitzenrate bereit.
 
### <a name="mitigate-throttling-and-latency"></a>Beschränken der Drosselung und Latenz

Informationen zum Verhindern der Drosselung und Latenz finden Sie unter [Verringern der Latenz und Drosselung](time-series-insights-environment-mitigate-latency.md). 

## <a name="shaping-your-events"></a>Gestalten Ihrer Ereignisse
Es ist wichtig sicherzustellen, dass für den Vorgang zum Senden von Ereignissen an TSI die Größe der bereitgestellten Umgebung unterstützt wird. (Sie können auch die Größe der Umgebung daran anpassen, wie viele Ereignisse TSI liest, und an die Größe der einzelnen Ereignisse.)  Außerdem ist es wichtig, sich Gedanken über die Attribute zu machen, die Sie beim Abfragen Ihrer Daten für das Slicing und die Filterung verwenden möchten.  In diesem Zusammenhang empfehlen wir Ihnen, den Abschnitt zur JSON-Gestaltung in unserer Dokumentation *Senden von Ereignissen an die Azure Time Series Insights-Umgebung mithilfe von Event Hub* [Dokumentation] (https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events) zu lesen.  Er befindet sich im unteren Teil der Seite.  

## <a name="ensuring-you-have-reference-data-in-place"></a>Sicherstellen des Vorhandenseins von Verweisdaten
Ein Verweisdataset ist eine Sammlung von Elementen, die die Ereignisse aus Ihrer Ereignisquelle ergänzen. Die Time Series Insights-Erfassungs-Engine verknüpft jedes Ereignis aus Ihrer Ereignisquelle mit der entsprechenden Datenzeile in Ihrem Verweisdataset. Dieses ergänzte Ereignis ist dann für Abfragen verfügbar. Diese Verknüpfung basiert auf den in Ihrem Verweisdataset definierten Primärschlüsselspalten.

Hinweis: Verweisdaten werden nicht rückwirkend verknüpft. Das bedeutet, dass nur aktuelle und künftige eingehende Daten nach dem Konfigurieren und Hochladen abgeglichen und dem Verweisdataset hinzugefügt werden.  Wenn Sie planen, eine große Menge von Verlaufsdaten an TSI zu senden, und nicht zuerst Verweisdaten in TSI hochladen oder erstellen, müssen Sie Ihre Arbeitsschritte unter Umständen erneut durchführen (was keinen Spaß macht).  

Weitere Informationen dazu, wie Sie Ihre Verweisdaten in TSI erstellen, hochladen und verwalten, finden Sie in unserer Dokumentation zu den *Verweisdaten* [Dokumentation] (https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).


## <a name="next-steps"></a>Nächste Schritte
- [Hinzufügen einer Event Hub-Ereignisquelle](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [Hinzufügen einer IoT Hub-Ereignisquelle](time-series-insights-how-to-add-an-event-source-iothub.md)

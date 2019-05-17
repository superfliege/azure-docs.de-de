---
title: Planen der Skalierung Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: In diesem Artikel werden bewährte Methoden bei der Planung einer Azure Time Series Insights-Umgebung beschrieben, einschließlich Speicherkapazität, Datenaufbewahrung, Eingangskapazität, Überwachung und Business Disaster Recovery (BCDR).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: bf1f570319370fab99e2f52086bc81df259e3d35
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236491"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Planen Ihrer Azure Time Series Insights-GA-Umgebung

In diesem Artikel wird beschrieben, wie Sie Ihre Azure Time Series Insights-GA-Umgebung (General Availability, allgemeine Verfügbarkeit) auf der Grundlage der erwarteten Eingangsrate und den Anforderungen an die Datenaufbewahrung planen.

## <a name="video"></a>Video

### <a name="learn-more-about-data-retention-in-azuretime-series-insights-and-how-to-plan-for-itbr"></a>Weitere Informationen zur Datenaufbewahrung in AzureTime Series Insights und zur Planung dafür.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>Bewährte Methoden

Für den Einstieg in Time Series Insights ist es am besten, wenn Sie wissen, wie viele Daten erwartungsgemäß pro Minute übertragen werden und wie lange die Daten gespeichert werden sollen.  

Weitere Informationen zur Kapazität und Aufbewahrung für beide Time Series Insights-SKUs finden Sie unter [Time Series Insights – Preise](https://azure.microsoft.com/pricing/details/time-series-insights/).

Berücksichtigen Sie im Hinblick auf den langfristigen Erfolg folgende Faktoren zur optimalen Planung der Umgebung:

- <a href="#understand-storage-capacity">Speicherkapazität</a>
- <a href="#understand-data-retention">Grundlegendes zur Datenaufbewahrung</a>
- <a href="#understand-ingress-capacity">Grundlegendes zur Eingangskapazität</a>
- <a href="#shape-your-events">Gestalten Ihrer Ereignisse</a>
- <a href="#ensure-you-have-reference-data">Sicherstellen des Vorhandenseins von Verweisdaten</a>

## <a name="understand-storage-capacity"></a>Grundlegendes zur Speicherkapazität

Standardmäßig bewahrt Azure Time Series Insights Daten basierend auf der von Ihnen bereitgestellten Speichermenge (Einheiten mal Speichermenge pro Einheit) und dem Eingang auf.

## <a name="understand-data-retention"></a>Grundlegendes zur Datenaufbewahrung

Sie können die Einstellung **Datenaufbewahrungszeit** Ihrer Time Series Insights-Umgebung konfigurieren, sodass eine Aufbewahrungsdauer von bis zu 400 Tagen möglich ist. Time Series Insights weist zwei Modi auf: In einem Modus wird sichergestellt, dass Ihre Umgebung über die neuesten Daten verfügt (standardmäßig aktiviert). Im anderen Modus wird sichergestellt, dass die Aufbewahrungslimits eingehalten werden, wobei der Dateneingang unterbrochen wird, wenn die Gesamtspeicherkapazität der Umgebung erreicht ist.  Auf der Konfigurationsseite der Umgebung im Azure-Portal können Sie die Aufbewahrungsdauer anpassen und zwischen den beiden Modi wechseln.

Sie können in Ihrer Time Series Insights-Umgebung eine Datenaufbewahrung von maximal 400 Tagen konfigurieren.

### <a name="configure-data-retention"></a>Konfigurieren der Datenaufbewahrung

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihre Time Series Insights-Umgebung aus.

1. Wählen Sie auf der Seite für die **Time Series Insights-Umgebung** unter der Überschrift **Einstellungen** die Option **Konfigurieren** aus.

1. Geben Sie im Feld **Datenaufbewahrungszeit (in Tagen)** einen Wert zwischen 1 und 400 ein.

   [![Konfigurieren der Datenaufbewahrung](media/environment-mitigate-latency/configure-retention.png)](media/environment-mitigate-latency/configure-retention.png#lightbox)

> [!TIP]
> Erfahren Sie mehr über die Implementierung einer entsprechenden Datenaufbewahrungsrichtlinie unter [Konfigurieren der Vermerkdauer in Time Series Insights](./time-series-insights-how-to-configure-retention.md).

## <a name="understand-ingress-capacity"></a>Grundlegendes zur Eingangskapazität

Der andere wichtige Faktor bei der Planung ist die Eingangskapazität, die eine Ableitung der Zuordnung pro Minute darstellt.

In Bezug auf die Drosselung wird ein eingehendes Datenpaket mit einer Paketgröße von 32 KB als 32 Ereignisse mit jeweils einer Größe von 1 KB behandelt. Die maximal zulässige Ereignisgröße ist 32 KB. Datenpakete mit einer Größe von über 32 KB werden abgeschnitten.

In der folgenden Tabelle ist die Eingangskapazität für jede SKU zusammengefasst:

|SKU  |Ereignisanzahl / Monat / Einheit  |Ereignisgröße / Monat / Einheit  |Ereignisanzahl / Minute / Einheit  | Größe / Minute / Einheit   |
|---------|---------|---------|---------|---------|
|S1     |   30 Millionen     |  30 GB     |  720    |  720 KB   |
|S2     |   300 Millionen    |   300 GB   | 7.200   | 7.200 KB  |

Sie können die Kapazität einer SKU des Typs S1 oder S2 in einer einzelnen Umgebung auf 10 Einheiten erhöhen. Die Migration von einer S1-Umgebung zu einer S2-Umgebung oder von einer S2-Umgebung zu einer S1-Umgebung ist nicht möglich.

Für die Eingangskapazität sollten Sie zunächst den Gesamteingang bestimmen, den Sie pro Monat benötigen. Ermitteln Sie anschließend die Anforderungen pro Minute, da dies eine Rolle für Drosselung und Latenz spielt.

Wenn Sie eine Spitze im Dateneingang über einen Zeitraum unter 24 Stunden verzeichnen, kann Time Series Insights dies mit einer Eingangsrate „aufholen“, die doppelt so hoch ist wie die oben aufgeführten Raten.

Wenn Sie z.B. über eine einzelne SKU des Typs S1 verfügen und Daten mit einer Rate von 720 Ereignissen pro Minute und bei einer Spitze in einem Zeitraum unter 1 Stunde mit einer Rate von maximal 1440 Ereignissen eingehen, weist Ihre Umgebung keine merkliche Latenz auf. Wenn jedoch 1440 Ereignisse pro Minute in einem Zeitraum von über einer Stunde überschritten werden, treten für Daten, die in Ihrer Umgebung visualisiert werden und zur Abfrage verfügbar sind, wahrscheinlich Latenzprobleme auf.

Sie wissen möglicherweise nicht im Voraus, wie viele Daten erwartungsgemäß übertragen werden. In diesem Fall finden Sie die Datentelemetrie für [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) und [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) im Azure-Portal. Anhand dieser Telemetrie können Sie die Bereitstellung Ihrer Umgebung bestimmen. Verwenden Sie die Seite **Metriken** im Azure-Portal für die jeweilige Ereignisquelle, um die zugehörige Telemetrie anzuzeigen. Wenn Sie sich mit den Metriken der Ereignisquelle vertraut gemacht haben, können Sie Ihre Time Series Insights-Umgebung effektiver planen und bereitstellen.

### <a name="calculate-ingress-requirements"></a>Berechnen der Eingangsanforderungen

- Überprüfen Sie, ob die Eingangskapazität über der durchschnittlichen Rate pro Minute liegt und ob die Umgebung groß genug ist, um den voraussichtlichen Eingang zu verarbeiten, der doppelt so hoch ist wie die Kapazität über einen Zeitraum von unter 1 Stunde.

- Wenn Eingangsspitzen auftreten, die länger als 1 Stunde dauern, verwenden Sie die Spitzenrate als Durchschnittswert, und stellen Sie eine Umgebung mit der Kapazität zum Verarbeiten der Spitzenrate bereit.

### <a name="mitigate-throttling-and-latency"></a>Beschränken der Drosselung und Latenz

Informationen zum Verhindern von Drosselung und Latenz finden Sie unter [Überwachen und Mindern der Drosselung zur Verhinderung von Latenz in Azure Time Series Insights](time-series-insights-environment-mitigate-latency.md).

## <a name="shape-your-events"></a>Gestalten Ihrer Ereignisse

Sie müssen unbedingt sicherstellen, dass die Art, in der Sie Ereignisse an TSI senden, die Größe der Umgebung unterstützt, die Sie bereitstellen (umgekehrt können Sie die Größe der Umgebung danach zuordnen, wie viele Ereignisse TSI liest, und wie groß die einzelnen Ereignisse sind). Außerdem ist es wichtig, sich Gedanken über die Attribute zu machen, die Sie beim Abfragen Ihrer Daten für das Slicing und die Filterung verwenden möchten.

> [!TIP]
> Lesen Sie die Dokumentation zur JSON-Strukturierung in [Senden von Ereignissen an die Azure Time Series Insights-Umgebung mithilfe eines Event Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).

## <a name="ensure-you-have-reference-data"></a>Sicherstellen, dass Sie über Referenzdaten verfügen

Ein **Referenzdataset** ist eine Sammlung von Elementen, die die Ereignisse aus Ihrer Ereignisquelle ergänzen. Die Time Series Insights-Erfassungs-Engine verknüpft jedes Ereignis aus Ihrer Ereignisquelle mit der entsprechenden Datenzeile in Ihrem Verweisdataset. Dieses ergänzte Ereignis ist dann für Abfragen verfügbar. Diese Verknüpfung basiert auf den in Ihrem Verweisdataset definierten Primärschlüsselspalten.

Hinweis: Verweisdaten werden nicht rückwirkend verknüpft. Das bedeutet, dass nur aktuelle und künftige eingehende Daten nach dem Konfigurieren und Hochladen abgeglichen und dem Verweisdataset hinzugefügt werden.  Wenn Sie planen, eine große Menge von Verlaufsdaten an TSI zu senden, und nicht zuerst Verweisdaten in TSI hochladen oder erstellen, müssen Sie Ihre Arbeitsschritte unter Umständen erneut durchführen (was keinen Spaß macht).  

Weitere Informationen dazu, wie Sie Ihre Verweisdaten in TSI erstellen, hochladen und verwalten, finden Sie in unserer [Dokumentation zu Verweis-DataSets](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie zunächst [eine neue Time Series Insights-Umgebung im Azure-Portal](time-series-insights-get-started.md).

- Erfahren Sie, wie Sie Time Series Insights [eine Event Hub-Ereignisquelle hinzufügen](time-series-insights-how-to-add-an-event-source-eventhub.md).

- Erfahren Sie, wie Sie [eine IoT Hub-Ereignisquelle konfigurieren](time-series-insights-how-to-add-an-event-source-iothub.md).

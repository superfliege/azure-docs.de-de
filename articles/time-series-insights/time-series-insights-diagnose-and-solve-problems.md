---
title: Diagnose und Problembehandlung in Azure Time Series Insights | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie häufig auftretende Probleme in Azure Time Series Insights-Umgebung diagnostiziert und behoben werden.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: venkatja
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 04/09/2018
ms.openlocfilehash: dbd32d57206b611a37b5349e5971d2efe272ee1a
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36292873"
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnostizieren und Beheben von Problemen in der Time Series Insights-Umgebung

## <a name="problem-1-no-data-is-shown"></a>Problem 1: Es werden keine Daten angezeigt.
Es gibt verschiedene mögliche Gründe, aus denen Ihre Daten im [Azure Time Series Insights-Explorer](https://insights.timeseries.azure.com) nicht angezeigt werden:

### <a name="possible-cause-a-event-source-data-is-not-in-json-format"></a>Mögliche Ursache A: Die Ereignis-Quelldaten liegen nicht in JSON-Format vor.
Azure Time Series Insights unterstützt nur JSON-Daten. JSON-Beispiele finden Sie unter [Unterstützte JSON-Formen](time-series-insights-send-events.md#supported-json-shapes).

### <a name="possible-cause-b-event-source-key-is-missing-a-required-permission"></a>Mögliche Ursache B: Für den Schlüssel der Ereignisquelle ist eine erforderliche Berechtigung nicht vorhanden.
* Für IoT Hub müssen Sie einen Schlüssel mit der Berechtigung **Dienstverbindung** bereitstellen.

   ![Berechtigung zur Verbindung mit dem IoT Hub-Dienst](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Wie in der vorherigen Abbildung dargestellt, würden die Richtlinien **iothubowner** und **service** funktionieren, da beide die Berechtigung **Dienstverbindung** einschließen.
   
* Für einen Event Hub müssen Sie einen Schlüssel mit der Berechtigung **Lauschen** bereitstellen.

   ![Event Hub-Lauschberechtigung](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Wie in der vorherigen Abbildung dargestellt, würden die Richtlinien **read** oder **write** funktionieren, da beide die Berechtigung **Lauschen** einschließen.

### <a name="possible-cause-c-the-consumer-group-provided-is-not-exclusive-to-time-series-insights"></a>Mögliche Ursache C: Die bereitgestellte Consumergruppe gilt nicht exklusiv für Time Series Insights.
Während der Registrierung eines IoT Hubs oder eines Event Hubs geben Sie die Consumergruppe an, die zum Lesen der Daten verwendet werden soll. Diese Consumergruppe darf **nicht** freigegeben sein. Wenn die Consumergruppe freigegeben wird, trennt der zugrunde liegende Event Hub automatisch einen zufälligen Leser. Stellen Sie eine eindeutige Consumergruppe bereit, aus der Time Series Insights lesen soll.

## <a name="problem-2-some-data-is-shown-but-some-is-missing"></a>Problem 2: Einige Daten werden angezeigt, andere hingegen fehlen.
Wenn Daten nur teilweise angezeigt werden, jedoch ein Datenrückstand besteht, erwägen Sie mehrere mögliche Ursachen:

### <a name="possible-cause-a-your-environment-is-getting-throttled"></a>Mögliche Ursache A: Ihre Umgebung wird gedrosselt.
Dies ist ein häufiges Problem, wenn Umgebungen nach der Erstellung einer Ereignisquelle mit Daten bereitgestellt werden.  Auf Azure IoT Hubs und Events Hubs werden Daten bis zu sieben Tage lang gespeichert.  TSI beginnt immer mit ältesten Ereignis (FIFO) auf der Ereignisquelle.  Wenn Sie auf einer Ereignisquelle über fünf Millionen Ereignisse verfügen und eine Verbindung mit einer TSI-Umgebung vom Typ „S1, eine Einheit“ herstellen, liest TSI also ca. eine Million Ereignisse pro Tag.  Auf den ersten Blick sieht es so aus, als ob dies fünf Tage lang Latenz für TSI bedeutet.  Der tatsächliche Grund ist, dass die Umgebung gedrosselt wird.  Falls Ihre Ereignisquelle ältere Ereignisse enthält, können Sie zwischen zwei Ansätzen wählen:

- Ändern Sie die Aufbewahrungslimits Ihrer Ereignisquelle, damit ältere Ereignisse, die nicht in TSI angezeigt werden sollen, entfernt werden.
- Stellen Sie eine größere Umgebung bereit (in Bezug auf die Anzahl von Einheiten), um den Durchsatz für ältere Ereignisse zu erhöhen.  Wenn Sie für das obige Beispiel die S1-Umgebung für einen Tag auf fünf Einheiten erhöhen, sollte der Rückstand für die Umgebung innerhalb eines Tages abgebaut werden.  Falls bei Ihnen im stabilen Zustand pro Tag maximal 1 Million Ereignisse produziert werden, können Sie die Kapazität für Ereignisse anschließend wieder auf eine Einheit verringern.  

Der Drosselungsgrenzwert wird basierend auf dem Typ und der Kapazität der Umgebungs-SKU erzwungen. Alle Ereignisquellen in der Umgebung nutzen diese Kapazität gemeinsam. Wenn Ihre Event Hub-/IoT Hub-Ereignisquelle Daten per Push über die erzwungenen Grenzwerte hinaus überträgt, treten die Drosselung und eine Verzögerung ein.

Die folgende Abbildung zeigt eine Time Series Insights-Umgebung mit SKU S1 und Kapazität 3. Es können 3 Millionen Ereignisse pro Tag eingehen.

![Aktuelle Kapazität der Umgebungs-SKU](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Angenommen, diese Umgebung erfasst Nachrichten von einem Event Hub. Betrachten Sie die Eingangsrate in der folgenden Abbildung:

![Beispielerfassungsrate für einen Event Hub](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Wie in der Abbildung dargestellt wird, beträgt die tägliche Eingangsrate ca. 67.000 Nachrichten. Diese Rate übersetzt ungefähr 46 Nachrichten pro Minute. Wenn jede Event Hub-Nachricht auf ein einzelnes Time Series Insights-Ereignis reduziert wird, wird keine Drosselung auf diese Umgebung angewendet. Wenn jede Event Hub-Nachricht auf 100 Time Series Insights-Ereignisse reduziert wird, sollten 4.600 Ereignisse pro Minute erfasst werden. Eine SKU S1-Umgebung mit einer Kapazität von 3 kann nur 2.100 Ereignisse pro Minute erfassen (1 Mio. Ereignisse pro Tag = 700 Ereignisse pro Minute von 3 Einheiten = 2.100 Ereignisse pro Minute). Daher tritt aufgrund der Drosselung eine Verzögerung auf. 

Eine Übersicht über die Funktionsweise der Vereinfachungslogik finden Sie unter [Unterstützte JSON-Formen](time-series-insights-send-events.md#supported-json-shapes).

### <a name="recommended-resolution-steps-for-excessive-throttling"></a>Empfohlene Lösungsschritte für übermäßige Drosselung
Um die Verzögerung zu beheben, erhöhen Sie die SKU-Kapazität Ihrer Umgebung. Weitere Informationen finden Sie unter [Skalieren der Time Series Insights-Umgebung](time-series-insights-how-to-scale-your-environment.md).

### <a name="possible-cause-b-initial-ingestion-of-historical-data-is-causing-slow-ingress"></a>Mögliche Ursache B: Die anfängliche Erfassung von Verlaufsdaten bewirkt einen langsamen Eingang.
Wenn Sie eine Verbindung mit einer vorhandenen Ereignisquelle herstellen, besteht die Möglichkeit, dass Ihr Event Hub oder IoT Hub darin bereits Daten aufbewahrt. Die Umgebung beginnt mit dem Abrufen von Daten am Anfang der Aufbewahrungsdauer für Nachrichten aus der Ereignisquelle.

Dieses Verhalten ist das Standardverhalten und kann nicht überschrieben werden. Möglicherweise lösen Sie damit eine Drosselung aus, sodass es eine Weile dauert, bis die Erfassung von Verlaufsdaten abgeschlossen ist.

#### <a name="recommended-resolution-steps-of-large-initial-ingestion"></a>Empfohlene Lösungsschritte für eine umfassende anfängliche Erfassung
Führen Sie zum Beheben dieser Verzögerung die folgenden Schritte aus:
1. Erhöhen Sie die SKU-Kapazität auf den maximal zulässigen Wert (in diesem Fall 10). Sobald die Kapazität erhöht wird, schließt der Erfassungsvorgang wesentlich schneller die alten Daten ab. Sie können die Verarbeitungsgeschwindigkeit der alten Daten über das Verfügbarkeitsdiagramm im [Time Series Insights-Explorer](https://insights.timeseries.azure.com) visualisieren. Die erhöhte Kapazität wird Ihnen in Rechnung gestellt.
2. Sobald die Verzögerung abgefangen wurde, können Sie die SKU-Kapazität wieder auf die normale Erfassungsrate verringern.

## <a name="problem-3-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Problem 3: Meine Einstellung für den *Namen für die Zeitstempeleigenschaft* der Ereignisquelle funktioniert nicht.
Stellen Sie sicher, dass Name und Wert den folgenden Regeln entsprechen:
* Beachten Sie beim Namen der Zeitstempeleigenschaft die _Groß-/Kleinschreibung_.
* Der Wert für die Zeitstempeleigenschaft, der als JSON-Zeichenfolge von Ihrer Ereignisquelle eingeht, sollte das Format _JJJJ-MM-TTTHH:mm:ss.FFFFFFFK_ aufweisen. Ein Beispiel für eine solche Zeichenfolge ist „2008-04-12T12:53Z“.

Sie können am einfachsten sicherstellen, dass Ihr *Name der Zeitstempeleigenschaft* erfasst wird und richtig funktioniert, indem Sie den TSI-Explorer verwenden.  Wählen Sie im TSI-Explorer mithilfe des Diagramms einen Zeitraum aus, nachdem Sie den *Namen der Zeitstempeleigenschaft* angegeben haben.  Klicken Sie mit der rechten Maustaste auf die Auswahl, und wählen Sie die Option *Ereignisse untersuchen*.  Die erste Spaltenüberschrift sollte Ihr *Name der Zeitstempeleigenschaft* sein, und neben dem Wort *timestamp* sollte *($ts)* stehen, und nicht Folgendes:
- *(abc)*: Gibt an, dass TSI die Datenwerte als Zeichenfolgen liest.
- *Kalendersymbol*: Gibt an, dass TSI den Datenwert als *datetime* liest.
- *#*: Gibt an, dass TSI die Datenwerte als ganze Zahl liest.


## <a name="next-steps"></a>Nächste Schritte
- Zusätzliche Unterstützung erhalten Sie im [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) oder bei [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). 
- Zudem können Sie sich an den [Azure-Support](https://azure.microsoft.com/support/options/) wenden, um Unterstützung zu erhalten.

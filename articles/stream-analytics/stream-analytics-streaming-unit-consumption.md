---
title: "Azure Stream Analytics: Überblick über Streamingeinheiten und Informationen zu Anpassungen | Microsoft-Dokumentation"
description: Erfahren Sie, welche Faktoren sich auf die Leistung in Azure Stream Analytics auswirken.
keywords: Premium-Streamingeinheit, Leistung abfragen
services: stream-analytics
documentationcenter: 
author: JSeb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: 14f73322701dffea283432bf2a25b5e6cd3e9de4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="understand-and-adjust-streaming-units"></a>Überblick über Streamingeinheiten und Informationen zu Anpassungen

Azure Stream Analytics aggregiert die Leistungsgewichtung für die Ausführung eines Auftrags in Premium-Streamingeinheiten. Premium-Streamingeinheiten stellen die Computerressourcen dar, die bei der Ausführung eines Auftrags verbraucht werden. SUs bieten anhand eines kombinierten Maßes aus CPU, Arbeitsspeicher und Schreib- und Leseraten eine Möglichkeit zur Beschreibung der relativen Ereignisverarbeitungskapazität. Mit dieser Kapazität können Sie sich auf die Abfragelogik konzentrieren. Sie müssen keine Überlegungen zur Leistung auf Speicherebene anstellen, nicht manuell Speicher für Ihren Auftrag zuweisen, und nicht die ungefähre CPU-Kernzahl kennen, um Ihren Auftrags rechtzeitig auszuführen.

Um eine Streamingverarbeitung mit geringer Latenz zu erreichen, führen Azure Stream Analytics-Aufträge (ASA) die gesamte Verarbeitung im Arbeitsspeicher durch. Wenn nicht genügend Arbeitsspeicher vorhanden ist, tritt beim Streamingauftrag ein Fehler auf. Daher ist es bei einem Produktionsauftrag wichtig, die Ressourcennutzung eines Streamingauftrags zu überwachen und sicherzustellen, dass genügend Ressourcen zugewiesen werden, damit die Aufträge rund um die Uhr ausgeführt werden.

Die Metrik ist eine Prozentzahl zwischen 0 % und 100 %. Bei einem Streamingauftrag mit minimalem Ressourcenbedarf liegt die Metrik „Speichereinheitnutzung in %“ in der Regel zwischen 10 % und 20 %. Die Metrik sollte im Idealfall bei unter 80 % liegen, damit gelegentliche Spitzen verarbeitet werden können.  Sie können eine Warnung für die Metrik festlegen. (Weitere Informationen finden Sie unter [Erstellen von Metrikwarnungen in Azure Monitor für Azure-Dienste – Azure-Portal](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/insights-alerts-portal).)



## <a name="configure-stream-analytics-streaming-units-sus"></a>Konfigurieren von Streamingeinheiten (SUs) für Stream Analytics-Aufträge
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com/) an.
2. Suchen Sie in der Liste der Ressourcen nach dem zu skalierenden Stream Analytics-Auftrag, und öffnen Sie ihn anschließend. 
3. Klicken Sie auf der Seite des Auftrags unter „Konfigurieren“ auf **Skalieren**. 

    ![Konfiguration von Stream Analytics-Aufträgen im Azure-Portal][img.stream.analytics.preview.portal.settings.scale]
    
4. Verwenden Sie den Schieberegler, um die SUs für den Auftrag festzulegen. Beachten Sie, dass Sie auf die jeweiligen SU-Einstellungen beschränkt sind. 


## <a name="monitor-job-performance"></a>Überwachen der Auftragsleistung
Über das Azure-Portal können Sie den Durchsatz eines Auftrags nachverfolgen:

![Überwachen von Aufträgen in Azure Stream Analytics][img.stream.analytics.monitor.job]

Berechnen Sie den erwarteten Durchsatz der Workload. Für den Fall, dass der Durchsatz kleiner als erwartet ist, optimieren Sie die Eingabepartition und die Abfrage, und fügen Sie dem Auftrag zusätzliche SUs hinzu.


## <a name="how-many-sus-are-required-for-a-job"></a>Wie viele Premium-Streamingeinheiten sind für einen Auftrag erforderlich?

Die benötigte Anzahl an Premium-Streamingeinheiten für einen bestimmten Auftrag hängt von der Partitionskonfiguration für die Eingaben und der Abfrage ab, die für den Auftrag definiert ist. Auf der Seite **Skalieren** können Sie die richtige Anzahl von SUs festlegen. Es wird empfohlen, mehr Premium-Streamingeinheiten als erforderlich zuzuweisen. Das Stream Analytics-Verarbeitungsmodul führt zu einer niedrigeren Latenz und einem höheren Durchsatz. Dabei wird zusätzlicher Speicherplatz beansprucht.

Im Allgemeinen wird empfohlen, für Abfragen, die nicht **PARTITIONIEREN NACH** verwenden, mit 6 Premium-Streamingeinheiten zu beginnen. Ermitteln Sie dann die optimale Anzahl mittels Trial-and-Error-Methode. Dabei ändern Sie die Anzahl der SUs, nachdem Sie eine repräsentative Datenmenge übertragen und die Metrik „Speichereinheitnutzung in %“ überprüft haben.

Weitere Informationen über die Auswahl der richtigen Anzahl von SUs finden Sie auf folgender Seite: [Skalieren von Azure Stream Analytics-Aufträgen zur Erhöhung des Durchsatzes bei der Streamingdatenverarbeitung](stream-analytics-scale-jobs.md).

> [!Note]
> Die benötigte SU-Anzahl für einen bestimmten Auftrag hängt von der Partitionskonfiguration für die Eingaben und der für den Auftrag definierten Abfrage ab. Sie können die für Ihr Kontingent maximal festgelegte Anzahl von SUs für einen Auftrag auswählen. Jedes Azure-Abonnement verfügt standardmäßig über ein Kontingent von höchstens 200 SUs für alle Analyseaufträge in einer bestimmten Region. Wenn Sie die SUs für Ihre Abonnements über dieses Kontingent hinaus erhöhen möchten, wenden Sie sich an den [Microsoft-Support](http://support.microsoft.com). Die gültigen Werte für SUs pro Auftrag sind 1, 3, 6 und danach Werte in Schritten von 6.
> Hinweis: Die Verwendung von 1 SU wird für Produktionsaufträge nicht empfohlen. In der Regel sollten 1-SU-Aufträge nur für das Erstellen von Prototypen und das Testen von Aufträgen verwendet werden.



## <a name="factors-increasing-su-utilization"></a>Faktoren zur Erhöhung der Speichereinheitnutzung in % 
### <a name="stateful-query-logic"></a>Zustandsbehaftete Abfragelogik 
Eine einzigartige Funktion eines Azure Stream Analytics-Auftrags besteht darin, eine zustandsbehaftete Verarbeitung wie etwa Aggregate, temporale Verknüpfungen und temporale Analysefunktionen im Fenstermodus auszuführen. Die einzelnen Operatoren behalten einige Zustände bei. 
#### <a name="windowed-aggregates"></a>Aggregate im Fenstermodus
Die Zustandsgröße eines Aggregats im Fenstermodus verhält sich proportional zur Anzahl der Gruppen (Kardinalität) im GROUP BY-Operator. Beispiel: In der folgenden Abfrage ist die mit „clusterid“ verknüpfte Zahl die Kardinalität der Abfrage. 

    SELECT count(*)
    FROM input 
    GROUP BY  clusterid, tumblingwindow (minutes, 5)


Um Probleme zu beheben, die durch eine hohe Kardinalität in der vorherigen Abfrage verursacht wurden, können Sie Ereignisse an den durch „clusterid“ partitionierten Event Hub senden und die Abfrage horizontal hochskalieren, indem Sie dem System die separate Verarbeitung aller Eingangspartitionen durch **PARTITION BY** ermöglichen. Dies wird im folgenden Beispiel gezeigt:


    SELECT count(*) 
    FROM PARTITION BY PartitionId
    GROUP BY PartitionId, clusterid, tumblingwindow (minutes, 5)

Nach dem Partitionieren der Abfrage wird sie auf mehrere Knoten verteilt. Infolgedessen verringert sich die Anzahl eingehender clusterid-Ereignisse auf den einzelnen Knoten, wodurch wiederum die Kardinalität des GROUP BY-Operators reduziert wird. 

Event Hub-Partitionen sollten durch den Gruppierungsschlüssel partitioniert werden, um die Notwendigkeit eines Reduzierungsschritts zu vermeiden. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-overview). 
#### <a name="temporal-join"></a>Temporale Verknüpfung
Die Zustandsgröße einer temporalen Verknüpfung verhält sich proportional zur Anzahl der Ereignisse im zeitlichen Spielraum der Verknüpfung, die die Ereigniseingangsrate multipliziert mit der Größe des Spielraums darstellt. 

Die Anzahl nicht abgeglichener Ereignisse im Verknüpfungsvorgang wirkt sich auf die Arbeitsspeichernutzung für die Abfrage aus. Mit der folgenden Abfrage werden die Anzeigenaufrufe ermittelt, die Klicks generieren:

    SELECT id
    FROM clicks 
    INNER JOIN, impressions ON impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10.

In diesem Beispiel kann es sein, dass viele Anzeigen angezeigt werden, aber nur wenige Benutzer auf diese klicken, und alle Ereignisse müssen im Zeitfenster bleiben. Der belegte Arbeitsspeicher ist proportional zu Fenstergröße und Ereignisrate. 

Um dies zu beheben, senden Sie Ereignisse an den durch die Verknüpfungsschlüssel (in diesem Fall IDs) partitionierten Event Hub, und skalieren Sie die Abfrage horizontal hoch, indem Sie dem System die separate Verarbeitung jeder Eingangspartition mit **PARTITION BY** ermöglichen. Dies wird im folgenden Beispiel gezeigt:

    SELECT id
    FROM clicks PARTITION BY PartitionId
    INNER JOIN impressions PARTITION BY PartitionId 
    ON impression.PartitionId = clocks.PartitionId AND impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10 
</code>

Nach dem Partitionieren der Abfrage wird sie auf mehrere Knoten verteilt. Infolgedessen verringert sich die Anzahl eingehender Ereignisse auf den einzelnen Knoten, wodurch wiederum die Größe des Zustands im Verknüpfungsfenster reduziert wird. 
#### <a name="temporal-analytic-function"></a>Temporale Analysefunktion
Die Zustandsgröße einer temporalen Analysefunktion verhält sich proportional zu der mit der Dauer multiplizierten Ereignisrate. Die Wiederherstellung weist Ähnlichkeiten mit der temporalen Verknüpfung auf. Sie können die Abfrage durch **PARTITION BY** horizontal hochskalieren. 

### <a name="out-of-order-buffer"></a>Puffer für Ereignisse in falscher Reihenfolge 
Benutzer können die Größe eines Puffers für Ereignisse in falscher Reihenfolge im Konfigurationsbereich „Ereignisreihenfolge“ konfigurieren. Der Puffer wird verwendet, um Eingaben für die Dauer des Fensters zu speichern und neu anzuordnen. Die Größe des Puffers verhält sich proportional zu der mit der Größe des Fensters für Ereignisse in falscher Reihenfolge multiplizierten Ereigniseingangsrate. Die Standardfenstergröße beträgt 0. 

Um dies zu beheben, skalieren Sie die Abfrage durch **PARTITION BY** horizontal hoch. Nach dem Partitionieren der Abfrage wird sie auf mehrere Knoten verteilt. Infolgedessen verringert sich die Anzahl eingehender Ereignisse auf den einzelnen Knoten, wodurch wiederum die Größe der Ereignisse in jedem Neuanordnungspuffer reduziert wird. 

### <a name="input-partition-count"></a>Anzahl von Eingabepartitionen 
Jede Eingabepartition einer Auftragseingabe weist einen Puffer auf. Je größer die Anzahl der Eingabepartitionen, desto mehr Ressourcen verbraucht der Auftrag. Azure Stream Analytics (ASA) kann pro SU eine Eingabe von ungefähr 1 MB/s verarbeiten. Daher sollte die Anzahl der SUs von ASA mit der Anzahl der Partitionen Ihres Event Hubs übereinstimmen. In der Regel ist ein 1-SU-Auftrag für einen Event Hub mit 2 Partitionen (der Mindestanzahl für den Event Hub) ausreichend. Wenn der Event Hub eine größere Anzahl von Partitionen aufweist, verbraucht Ihr ASA-Auftrag mehr Ressourcen, nutzt jedoch nicht zwingend den zusätzlichen vom Event Hub bereitgestellten Durchsatz. Für einen Auftrag mit 6 SUs benötigen Sie möglicherweise 4 oder 8 Partitionen vom Event Hub. Die Verwendung eines Event Hubs mit 16 Partitionen oder mehr in einem 1-SU-Auftrag trägt oft zu einem übermäßigen Ressourcenverbrauch bei und sollte daher vermieden werden. 

### <a name="reference-data"></a>Verweisdaten 
Die Verweisdaten in ASA werden zur schnellen Suche in den Speicher geladen. Bei der aktuellen Implementierung wird bei jedem Verknüpfungsvorgang mit Verweisdaten eine Kopie der Verweisdaten im Speicher beibehalten, auch wenn Sie dieselben Verweisdaten mehrmals verknüpfen müssen. Bei Abfragen mit **PARTITION BY** behält jede Partition eine Kopie der Verweisdaten bei, sodass die Partitionen vollständig entkoppelt sind. Durch den Multiplikationseffekt kann die Speicherverwendung schnell in die Höhe schießen, wenn Sie Verweisdaten mehrmals mit mehreren Partitionen verknüpfen.  

#### <a name="use-of-udf-functions"></a>Verwenden von UDF-Funktionen
Wenn Sie eine UDF-Funktion hinzufügen, lädt Azure Stream Analytics die JavaScript-Laufzeit in den Arbeitsspeicher. Dies wirkt sich auf die Speichereinheitnutzung in % aus.


## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen von parallelisierbaren Abfragen in Azure Stream Analytics](stream-analytics-parallelization.md)
* [Skalieren von Azure Stream Analytics-Aufträgen zur Erhöhung des Durchsatzes](stream-analytics-scale-jobs.md)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

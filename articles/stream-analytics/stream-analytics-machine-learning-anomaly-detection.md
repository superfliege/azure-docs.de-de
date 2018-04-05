---
title: Benutzerhandbuch zur Anomalieerkennung in Azure (Vorschauversion) | Microsoft Docs
description: Verwenden Sie Stream Analytics und Machine Learning, um Anomalien zu erkennen.
services: stream-analytics
documentationcenter: ''
author: dubansal
manager: katicad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/12/2018
ms.author: dubansal
ms.openlocfilehash: 9d301f8586038f635ee97a3acdc9c4dc8a2bbcc6
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Anomalieerkennung in Azure Stream Analytics

> [!IMPORTANT]
> Diese Funktionalität befindet sich in der Vorschauphase. Es wird nicht empfohlen, sie mit Produktionsworkloads zu verwenden.

Der Operator **AnomalyDetection** kann zum Erkennen verschiedener Anomalietypen in Ereignisdatenströmen verwendet werden. Beispielsweise kann eine langsame Abnahme des freien Arbeitsspeichers über einen langen Zeitraum Anzeichen eines Speicherlecks sein, oder die Anzahl der Webdienstanforderungen, die in einem Bereich stabil sind, kann sich möglicherweise dramatisch erhöhen oder verringern.  

Der Operator AnomalyDetection erkennt drei Arten von Anomalien: 

* **Bidirektionale Niveauänderung**: Eine nachhaltige Zunahme oder Abnahme des Niveaus der Werte, sowohl nach oben als auch nach unten. Dieser Wert unterscheidet sich von Spitzen und Einbrüchen, bei denen es sich um augenblickliche oder kurzlebige Änderungen handelt.  

* **Langsamer positiver Trend**: Eine langsame Zunahme des Trends im Lauf der Zeit.  

* **Langsamer negativer Trend**: Eine langsame Abnahme des Trends im Lauf der Zeit.  

Wenn Sie den AnomalyDetection-Operator verwenden, müssen Sie die **Limit Duration**-Klausel angeben. Diese Klausel gibt das Zeitintervall an (wie weit zurück im Verlauf vor dem aktuellen Ereignis), das bei der Erkennung von Anomalien berücksichtigt werden soll. Dier Operator kann optional ausschließlich auf Ereignisse eingeschränkt werden, die gemäß der  **When** -Klausel mit bestimmten Eigenschaften oder Bedingungen übereinstimmen. Dieser Operator kann optional auch Gruppen von Ereignissen basierend auf dem in der  **Partition by** -Klausel angegebenen Schlüssel separat verarbeiten. Training und Vorhersage treten für jede Partition unabhängig voneinander auf. 

## <a name="syntax-for-anomalydetection-operator"></a>Syntax für den AnomalyDetection-Operator

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 

**Beispielsyntax**  

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`

### <a name="arguments"></a>Argumente

* **scalar_expression**: Der skalare Ausdruck, über den die Anomalieerkennung ausgeführt wird. Zulässige Werte für diesen Parameter sind Float- oder Bigint-Datentypen, die einen einzelnen (skalaren) Wert zurückgeben. Den Platzhalterausdruck **\*** ist nicht zulässig. Der skalare Ausdruck darf keine anderen analytischen oder externen Funktionen enthalten. 

* **partition_by_clause**: Die `PARTITION BY <partition key>`-Klausel verteilt das Lernen und Training auf separate Partitionen. Das heißt, ein separates Modell würde gemäß dem Wert von `<partition key>` verwendet, und nur Ereignisse mit diesem Wert würden für das Lernen und Trainieren in diesem Modell verwendet. Die folgende Abfrage z.B. trainiert und bewertet einen Messwert nur im Vergleich zu anderen Messwerten desselben Sensors:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

* **limit_duration-Klausel** `DURATION(<unit>, <length>)`: Gibt das Zeitintervall an (wie weit zurück im Verlauf vor dem aktuellen Ereignis), das bei der Erkennung von Anomalien berücksichtigt werden soll. Eine ausführliche Beschreibung der unterstützten Einheiten und deren Abkürzungen finden Sie unter [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics). 

* **when_clause**: Gibt eine boolesche Bedingung für die in der Anomalieerkennungsberechnung berücksichtigten Ereignisse an.

### <a name="return-types"></a>Rückgabetypen

Der AnomalyDetection-Operator gibt einen Datensatz mit allen drei Bewertungen als Ausgabe zurück. Die folgenden Eigenschaften sind den verschiedenen Typen von Anomaliedetektoren zugeordnet:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Um die einzelnen Werte aus dem Datensatz zu extrahieren, verwenden Sie die **GetRecordPropertyValue** Funktion. Beispiel: 

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 

Eine Anomalie eines Typs wird erkannt, wenn eine dieser Anomaliebewertungen einen Schwellenwert überschreitet. Der Schwellenwert kann jede Gleitkommazahl >= 0 sein. Der Schwellenwert ist ein Kompromiss zwischen Empfindlichkeit und Genauigkeit. Beispielsweise wäre die Erkennung bei einem niedrigeren Schwellenwert empfindlicher gegenüber Änderungen und würde mehr Warnungen generieren, während sie bei einem höheren Schwellenwert weniger empfindlich und genauer sein könnte, aber dabei würden einige Anomalien maskiert. Der genaue zu verwendende Schwellenwert hängt vom Szenario ab. Es gibt keine Obergrenze, aber der empfohlene Bereich ist 3,25 bis 5. 

## <a name="anomaly-detection-algorithm"></a>Anomalieerkennungsalgorithmus

* Der AnomalyDetection-Operator verwendet einen **unüberwachten Lernansatz**, bei dem keine Art der Verteilung in den Ereignissen angenommen wird. In der Regel werden zwei Modelle parallel zu einem bestimmten Zeitpunkt verwaltet, wobei eines davon für die Bewertung verwendet und das andere im Hintergrund trainiert wird. Die Modelle zur Erkennung von Anomalien werden mit Daten aus dem aktuellen Datenstrom trainiert und nicht mit einem Out-of-Band-Mechanismus. Die Menge der für das Training verwendeten Daten hängt von der Fenstergröße d ab, die der Benutzer im Parameter Limit Duration angegeben hat. Jedes Modell wird auf der Grundlage von Ereignissen im Wert von d bis 2d trainiert. Es wird empfohlen, mindestens 50 Ereignisse in jedem Fenster zu verwenden, um optimale Ergebnisse zu erzielen. 

* Der AnomalyDetection-Operator verwendet **Semantik des gleitenden Fensters**, um Modelle zu trainieren und Ereignisse zu bewerten. Das bedeutet, dass jedes Ereignis hinsichtlich der Anomalie ausgewertet und eine Bewertung zurückgegeben wird. Die Bewertung ist ein Hinweis auf den Zuverlässigkeitsgrad dieser Anomalie. 

* Der AnomalyDetection-Operator bietet eine **Wiederholbarkeitsgarantie**, wobei die gleiche Eingabe immer die gleiche Bewertung generiert, und zwar unabhängig von der Startzeit der Auftragsausgabe. Die Startzeit der Auftragsausgabe stellt den Zeitpunkt dar, zu dem der Auftrag das erste Ausgabeereignis generiert. Es wird vom Benutzer auf die aktuelle Uhrzeit, einen benutzerdefinierten Wert oder die letzte Ausgabezeit festgelegt (wenn der Auftrag zuvor eine Ausgabe generiert hat). 

### <a name="training-the-models"></a>Trainieren der Modelle 

Im Lauf der Zeit werden Modelle mit verschiedenen Daten trainiert. Um die Bewertungen zu verstehen, ist es hilfreich, den zugrunde liegenden Mechanismus zu verstehen, mit dem die Modelle trainiert werden. Hier ist **t<sub>0</sub>** die **Startzeit der Auftragsausgabe**, und **d** ist die **Fenstergröße** aus dem Limit Duration-Parameter. Angenommen, die Zeit wird in unterteilt in **Hops der Größe d**, beginnend ab `01/01/0001 00:00:00`. Die folgenden Schritte werden verwendet, um das Modell zu trainieren und die Ereignisse zu bewerten:

* Wenn ein Auftrag gestartet wird, liest er Daten beginnend ab der Uhrzeit t<sub>0</sub> – 2d.  
* Wenn die Uhrzeit den nächsten Hop erreicht, wird ein neues Modell M1 erstellt und mit dessen Training begonnen.  
* Wenn die Uhrzeit den nächsten Hop erreicht, wird ein neues Modell M2 erstellt und mit dessen Training begonnen.  
* Wenn t<sub>0</sub> erreicht wird, wird M1 aktiviert und mit der Ausgabe seiner Bewertung begonnen.  
* Beim nächsten Hop finden drei Vorgänge gleichzeitig statt:  

  * M1 wird nicht mehr benötigt und verworfen.  
  * M2 wurde ausreichend trainiert und wird daher für die Bewertung verwendet.  
  * Ein neues Modell M3 wird erstellt, und es wird im Hintergrund mit seinem Training begonnen.  

* Dieser Zyklus wiederholt sich für jeden Hop. Dabei wird das aktive Modell verworfen, auf das parallele Modell umgeschaltet und das Training eines dritten Modells im Hintergrund gestartet. 

Die Schritte sehen schematisch wie folgt aus: 

![Trainieren von Modellen](media/stream-analytics-machine-learning-anomaly-detection/training_model.png)

|**Modell** | **Trainingsstartzeit** | **Zeit, zu der mit der Verwendung seiner Bewertung begonnen wird** |
|---------|---------|---------|
|M1     | 11:20   | 11:33   |
|M2     | 11:30   | 11:40   |
|M3     | 11:40   | 11:50   |

* Modell M1 beginnt das Training um 11:20 Uhr, also mit dem nächsten Hop, nachdem der Auftrag um 11:13 Uhr mit dem Lesen begonnen hat. Die erste Ausgabe wird von M1 um 11:33 Uhr nach dem Training mit 13 Minuten der Daten generiert. 

* Ein neues Modell M2 beginnt ebenfalls um 11:30 Uhr mit dem Training, aber seine Bewertung wird erst ab 11:40 Uhr verwendet, also nachdem es mit 10 Minuten der Daten trainiert wurde. 

* M3 folgt demselben Muster wie M2. 

### <a name="scoring-with-the-models"></a>Bewertung mit den Modellen 

Auf der Ebene von Machine Learning berechnet der Algorithmus zur Erkennung von Anomalien einen Ungewöhnlichkeitswert für jedes eingehende Ereignis, indem er es mit Ereignissen in einem Verlaufsfenster vergleicht. Die Berechnung der Ungewöhnlichkeit unterscheidet sich je nach Art der Anomalie.  

Sehen wir uns die Ungewöhnlichkeitsberechnung im Detail an (es wird angenommen, dass eine Reihe von Verlaufsfenstern mit Ereignissen vorhanden ist): 

1. **Bidirektionale Niveauänderung:** Basierend auf dem Verlaufsfenster wird ein normaler Betriebsbereich als[10. Quantil, 90. Quantil] berechnet, d.h. Wert des 10. Quantils als untere Grenze und Wert des 90. Quantils als obere Grenze. Ein Ungewöhnlichkeitswert für das aktuelle Ereignis wird wie folgt berechnet:  

   - 0, wenn event_value im normalen Betriebsbereich liegt  
   - event_value/90th_percentile, wenn event_value > 90th_percentile  
   - 10th_percentile/event_value, wenn event_value < 10th_percentile  

2. **Langsamer positiver Trend:** Eine Trendlinie über den Ereigniswerten im Verlaufsfenster wird berechnet, und wir suchen nach einem positiven Trend innerhalb der Linie. Der Ungewöhnlichkeitswert wird wie folgt berechnet:  

   - Slope, wenn Slope positiv ist  
   - Andernfalls 0 

1. **Langsamer negativer Trend:** Eine Trendlinie über den Ereigniswerten im Verlaufsfenster wird berechnet, und wir suchen nach einem negativen Trend innerhalb der Linie. Der Ungewöhnlichkeitswert wird wie folgt berechnet: 

   - Slope, wenn Slope negativ ist  
   - Andernfalls 0  

Nachdem der Ungewöhnlichkeitswert für das eingehende Ereignis berechnet wurde, wird ein Martingalwert basierend auf dem Ungewöhnlichkeitswert berechnet (Details zur Berechnung des Martingalwerts finden Sie im [Machine Learning-Blog](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)). Dieser Martingalwert wird als Anomaliebewertung zurückgegeben. Der Martingalwert nimmt als Reaktion auf ungewöhnliche Werte langsam zu, wodurch der Detektor robust gegenüber sporadischen Veränderungen bleibt und Fehlalarme verringert werden. Es verfügt auch über eine nützliche Eigenschaft: 

Wahrscheinlichkeit [t ist so vorhanden, dass M<sub>t</sub> > λ ] < 1/λ, wobei M<sub>t</sub> der Martingalwert zum Zeitpunkt t und λ ein reeller Wert ist. Wenn wir z.B. warnen, wenn M<sub>t</sub>>100, dann ist die Wahrscheinlichkeit von falsch positiven Warnungen kleiner als 1/100.  

## <a name="guidance-for-using-the-bi-directional-level-change-detector"></a>Anleitung für die Verwendung des bidirektionalen Niveauändernungsdetektors 

Der bidirektionale Niveauänderungsdetektor kann in Szenarien wie Stromausfall und Wiederherstellung, Datenverkehr zu Spitzenzeiten usw. verwendet werden. Er arbeitet jedoch so, dass (sobald ein Modell mit bestimmten Daten trainiert wird) ein weiterer Niveauwechsel nur dann anomal ist, wenn der neue Wert höher ist als die bisherige Obergrenze (Änderungsfall höheres Niveau) oder niedriger als die bisherige Untergrenze (Änderungsfall niedrigeres Niveau). Daher sollte ein Modell keine Datenwerte im Bereich des neuen Niveaus (höher oder niedriger) in seinem Trainingsfenster sehen, damit sie als anomal betrachtet werden können. 

Die folgenden Aspekte sollten bei der Verwendung dieses Detektors beachtet werden: 

1. Wenn in der Zeitreihe plötzlich ein Wertanstieg oder -abfall auftritt, erkennt der AnomalyDetection-Operator dies. Aber das Erkennen der Rückkehr zur Normalität erfordert mehr Planung. Wenn sich eine Zeitreihe vor der Anomalie in einem stabilen Zustand befand, was erreicht werden kann, indem das Erkennungsfenster des AnomalyDetection-Operators auf höchstens die halbe Länge der Anomalie festgelegt wird. Dieses Szenario geht davon aus, dass die minimale Dauer der Anomalie vorzeitig abgeschätzt werden kann und dass es genügend Ereignisse in diesem Zeitraum gibt, um das Modell ausreichend zu trainieren (mindestens 50 Ereignisse). 

   Dies wird in den Abbildungen 1 und 2 unten durch eine Änderung der oberen Grenze dargestellt (die gleiche Logik gilt auch für eine Änderung der unteren Grenze). In beiden Abbildungen stellen sind Wellenformen eine anormale Änderung des Niveaus dar. Vertikale orangefarbene Linien kennzeichnen Hopgrenzen, und die Hopgröße entspricht dem Erkennungsfenster, das im AnomalyDetection-Operator angegeben wird. Die grünen Linien geben die Größe des Trainingsfensters an. In Abbildung 1 ist die Hopgröße identisch mit der Zeit, die die Anomalie dauert. In Abbildung 2 ist die Hopgröße halb so groß wie die Zeit, die die Anomalie dauert. In allen Fällen wird eine Änderung nach oben erkannt, weil das für die Bewertung verwendete Modell mit normalen Daten trainiert wurde. Basierend auf der Funktionsweise des bidirektionalen Niveauänderungsdetektors müssen wir jedoch die Normalwerte aus dem Trainingsfenster des Modells ausschließen, das die Rückkehr zur Normalität bewertet. In Abbildung 1 enthält das Training des Bewertungsmodells einige normale Ereignisse, sodass eine Rückkehr zum Normalzustand nicht erkannt werden kann. In Abbildung 2 enthält das Training jedoch nur den anomalen Teil, sodass sicherstellt wird, dass die Rückkehr zur Normalität erkannt wird. Alles, was kleiner als die Hälfte ist, funktioniert aus dem gleichen Grund ebenfalls, wohingegen alles, was größer ist, auch einen Teil der normalen Ereignisse beinhalten wird. 

   ![AD mit Fenstergröße gleich der Anomalielänge](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_anomaly_length.png)

   ![AD mit Fenstergröße gleich der Hälfte der Anomalielänge](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_half_anomaly_length.png)

2. In den Fällen, in denen die Länge der Anomalie nicht vorhergesagt werden kann, arbeitet dieser Detektor nach bestem Ermessen. Die Auswahl eines engeren Zeitfensters schränkt jedoch die Trainingsdaten ein, was die Wahrscheinlichkeit erhöhen würde, die Rückkehr zur Normalität zu erkennen. 

3. Im folgenden Szenario wird die längere Anomalie nicht erkannt, da das Trainingsfenster bereits eine Anomalie mit dem gleichen hohen Wert enthält. 

   ![Anomalien mit derselben Größe](media/stream-analytics-machine-learning-anomaly-detection/anomalies_with_same_length.png)

## <a name="example-query-to-detect-anomalies"></a>Beispielabfrage zum Erkennen von Anomalien 

Mit der folgenden Abfrage kann eine Warnung ausgegeben werden, wenn eine Anomalie erkannt wird.
Wenn der Eingabedatenstrom nicht einheitlich ist, kann der Aggregationsschritt helfen, ihn in eine einheitliche Zeitreihe zu transformieren. Im Beispiel wird AVG verwendet, aber der jeweilige Aggregationstyp ist vom Benutzerszenario abhängig. Wenn darüber hinaus die Lücken einer Zeitreihe größer als das Aggregationsfenster sind, lösen keine Ereignisse in der Zeitreihe die Anomalieerkennung aus (in Übereinstimmung mit der Semantik des gleitenden Fensters). Daher wird die Annahme der Einheitlichkeit unterbrochen, wenn das nächste Ereignis eintrifft. In diesem Fall sollten die Lücken in der Zeitreihe geschlossen werden. Ein möglicher Ansatz besteht darin, wie unten dargestellt das letzte Ereignis in jedem Hopping-Fenster zu nehmen.

```sql
    WITH AggregationStep AS 
    (
         SELECT
               System.Timestamp as tumblingWindowEnd,

               AVG(value) as avgValue

         FROM input
         GROUP BY TumblingWindow(second, 5)
    ),

    FillInMissingValuesStep AS
    (
          SELECT
                System.Timestamp AS hoppingWindowEnd,

                TopOne() OVER (ORDER BY tumblingWindowEnd DESC) AS lastEvent

         FROM AggregationStep
         GROUP BY HOPPINGWINDOW(second, 300, 5)

    ),

    AnomalyDetectionStep AS
    (

          SELECT
                hoppingWindowEnd,
                lastEvent.tumblingWindowEnd as lastTumblingWindowEnd,
                lastEvent.avgValue as lastEventAvgValue,
                system.timestamp as anomalyDetectionStepTimestamp,

                ANOMALYDETECTION(lastEvent.avgValue) OVER (LIMIT DURATION(hour, 1)) as
                scores

          FROM FillInMissingValuesStep
    )

    SELECT
          alert = 1,
          hoppingWindowEnd,
          lastTumblingWindowEnd,
          lastEventAvgValue,
          anomalyDetectionStepTimestamp,
          scores

    INTO output

    FROM AnomalyDetectionStep

    WHERE

        CAST(GetRecordPropertyValue(scores, 'BiLevelChangeScore') as float) >= 3.25

        OR CAST(GetRecordPropertyValue(scores, 'SlowPosTrendScore') as float) >=
        3.25

       OR CAST(GetRecordPropertyValue(scores, 'SlowNegTrendScore') as float) >=
       3.25
```

## <a name="performance-guidance"></a>Leistungsleitfaden

* Verwenden Sie sechs Streamingeinheiten für Aufträge. 
* Senden Sie Ereignisse in einem Abstand von mindestens einer Sekunde.
* Eine nicht partitionierte Abfrage, die den AnomalyDetection-Operator verwendet, kann Ergebnisse mit einer durchschnittlichen Berechnungswartezeit von ungefähr 25 ms generieren.
* Die bei einer partitionierten Abfrage auftretende Wartezeit variiert leicht gemäß der Anzahl der Partitionen, da die Anzahl der Berechnungen höher ist. Allerdings ist die Wartezeit für eine vergleichbare Gesamtanzahl der Ereignisse auf allen Partitionen mit der einer nicht partitionierten Abfrage identisch.
* Während des Lesens von Nicht-Echtzeitdaten wird eine große Datenmenge schnell erfasst. Die Verarbeitung dieser Daten ist zurzeit langsamer. Es stellte sich heraus, dass die Wartezeit in einem solchen Szenario linear mit der Anzahl der Datenpunkte im Fenster statt der Fenstergröße oder des Ereignisintervalls ansteigt. Um die Wartezeit in Nicht-Echtzeitfällen zu reduzieren, erwägen Sie eine geringere Fenstergröße. Alternativ können Sie erwägen, Ihren Auftrag ab der aktuellen Uhrzeit zu starten. Einige Beispiele für Wartezeiten in einer nicht partitionierten Abfrage: 
    - 60 Datenpunkte im Erkennungsfenster können zu einer Wartezeit von 10 Sekunden mit einem Durchsatz von 3 MBit/s führen. 
    - Bei 600 Datenpunkten kann die Wartezeit ungefähr 80 Sekunden mit einem Durchsatz von 0,4 MBit/s erreichen.

## <a name="limitations-of-the-anomalydetection-operator"></a>Einschränkungen des AnomalyDetection-Operators 

* Dieser Operator unterstützt zurzeit keine Erkennung von Spitzen und Einbrüchen. Spitzen und Einbrüche sind spontane oder kurzlebige Veränderungen in der Zeitreihe. 

* Dieser Operator verarbeitet zurzeit keine Saisonalitätsmuster. Saisonalitätsmuster sind wiederholte Muster in den Daten, z.B. ein unterschiedliches Webdatenverkehrsverhalten am Wochenende oder unterschiedliche Einkaufstrends an Black Friday, die keine Anomalien, sondern ein erwartetes Verhaltensmuster darstellen. 

* Dieser Operator erwartet, dass die Eingabezeitreihen einheitlich sind. Ein Ereignisdatenstrom kann durch Aggregieren über ein rollierendes oder Hopping-Fenster vereinheitlicht werden. In Szenarien, in denen die Lücke zwischen Ereignissen immer kleiner als das Aggregationsfenster ist, reicht ein rollierendes Fenster, um die Zeitreihe zu vereinheitlichen. Wenn die Lücke größer sein kann, können die Lücken durch die Wiederholung des letzten Werts mit einem Hopping-Fenster ausgefüllt werden. 

## <a name="references"></a>Referenzen

* [Anomaly Detection – Using Machine Learning to Detect Abnormalities in Time Series Data](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/) (Anomalieerkennung: Verwenden von Machine Learning zum Erkennen von Auffälligkeiten in Zeitreihendaten)
* [Machine Learning Anomaly Detection-API](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Time Series Anomaly Detection](https://msdn.microsoft.com/library/azure/mt775197.aspx) (Anomalieerkennung in Zeitreihen)

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


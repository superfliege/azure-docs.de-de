---
title: Problembehandlung bei Event Hub-Empfängern in Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie mehrere Consumergruppen für Event Hubs-Eingaben in Stream Analytics-Aufträgen verwendet werden.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: aaa8c4e8d273b44f453d3f63f0be1d4baf980649
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2018
---
# <a name="troubleshoot-event-hub-receivers-in-azure-stream-analytics"></a>Problembehandlung bei Event Hub-Empfängern in Azure Stream Analytics

Sie können Azure Event Hubs in Azure Stream Analytics nutzen, um Daten in einem Auftrag zu erfassen oder aus diesem auszugeben. Eine bewährte Methode beim Arbeiten mit Event Hubs ist die Verwendung mehrerer Consumergruppen, um die Skalierbarkeit von Aufträgen sicherzustellen. Ein Grund hierfür ist, dass die Anzahl der Leser im Stream Analytics-Auftrag für eine bestimmte Eingabe sich auf die Anzahl der Leser in einer einzelnen Consumergruppe auswirkt. Die genaue Anzahl der Empfänger basiert auf internen Implementierungsdetails für die Logik der horizontalen Skalierungstopologie. Die Anzahl der Empfänger wird nicht extern verfügbar gemacht. Die Anzahl der Leser kann sich entweder zur Startzeit des Auftrags oder im Verlauf von Auftragsaktualisierungen ändern.

Der folgende Fehler wird angezeigt, wenn die maximale Anzahl der Empfänger überschritten wird: `The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.`

> [!NOTE]
> Wenn sich die Anzahl der Leser während der Aktualisierung des Auftrags ändert, werden Warnungen zu vorübergehenden Problemen in Überwachungsprotokolle geschrieben. Stream Analytics-Aufträge werden automatisch von diesen vorübergehenden Problemen behoben.

## <a name="add-a-consumer-group-in-event-hubs"></a>Hinzufügen einer Consumergruppe in Event Hubs
Führen Sie die folgenden Schritte aus, um Ihrer Event Hubs-Instanz eine neue Consumergruppe hinzuzufügen:

1. Melden Sie sich beim Azure-Portal an.

2. Suchen Sie nach Ihrer Event Hubs-Instanz.

3. Wählen Sie unter der Überschrift **Entitäten** den Eintrag **Event Hubs** aus.

4. Wählen Sie den Event Hub anhand des Namens aus.

5. Wählen Sie auf der Seite **Event Hubs-Instanz** unter der Überschrift **Entitäten** den Eintrag **Consumergruppen** aus. Es wird eine Consumergruppe mit dem Namen **$Default** aufgelistet.

6. Wählen Sie **+ Consumergruppe** aus, um eine neue Consumergruppe hinzuzufügen. 

   ![Hinzufügen einer Consumergruppe in Event Hubs](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Als Sie die Eingabe im Stream Analytics-Auftrag erstellt haben, um auf den Event Hub zu verweisen, haben Sie dort die Consumergruppe angegeben. Wenn keine Angabe erfolgt, wird „$Default“ verwendet. Bearbeiten Sie nach der Erstellung einer neuen Consumergruppe die Event Hub-Eingabe im Stream Analytics-Auftrag, und geben Sie den Namen der neuen Consumergruppe an.


## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Die Anzahl der Leser pro Partition überschreitet den Event Hubs-Grenzwert (5)

Wenn Ihre Streamingabfragesyntax mehrmals auf die gleiche Event Hub-Eingaberessource verweist, kann das Auftragsmodul mehrere Leser pro Abfrage aus derselben Consumergruppe verwenden. Wenn es zu viele Verweise auf dieselbe Consumergruppe gibt, kann der Auftrag den Grenzwert 5 überschreiten und einen Fehler auslösen. In diesen Fällen können Sie mithilfe der im folgenden Abschnitt beschriebenen Lösung eine weitere Unterteilung vornehmen, indem Sie mehrere Eingaben für mehrere Consumergruppen verwenden. 

Es folgen Szenarien, in denen die Anzahl der Leser pro Partition den Event Hubs-Grenzwert 5 überschreitet:

* Mehrere SELECT-Anweisungen: Bei Verwendung mehrerer SELECT-Anweisungen, die auf die **gleiche** Event Hub-Eingabe verweisen, bewirkt jede SELECT-Anweisung, dass ein neuer Empfänger erstellt wird.
* UNION: Wenn Sie UNION verwenden, es ist möglich, dass mehrere Eingaben auf den **gleichen** Event Hub und die gleiche Consumergruppe verweisen.
* SELF JOIN: Wenn Sie einen SELF JOIN-Vorgang verwenden, es ist möglich, dass auf den **gleichen** Event Hub mehrmals verwiesen wird.

## <a name="solution"></a>Lösung

Es folgen bewährte Methoden zum Vermeiden von Szenarien, in denen die Anzahl der Leser pro Partition den Event Hubs-Grenzwert 5 überschreitet.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>Unterteilen der Abfrage mithilfe einer WITH-Klausel in mehrere Schritte

Die WITH-Klausel gibt ein temporäres benanntes Resultset an, auf das in der Abfrage in einer FROM-Klausel verwiesen werden kann. Sie definieren die WITH-Klausel im Ausführungsbereich einer einzelnen SELECT-Anweisung.

Verwenden Sie beispielsweise anstelle dieser Abfrage:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Diese Abfrage:

```
WITH data AS (
   SELECT * FROM inputEventHub
)

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Sicherstellen, dass Eingaben an verschiedene Consumergruppen gebunden werden

Erstellen Sie für Abfragen, bei denen drei oder mehr Eingaben mit der gleichen Event Hubs-Consumergruppe verbunden sind, separate Consumergruppen. Hierfür ist die Erstellung zusätzlicher Stream Analytics-Eingaben erforderlich.


## <a name="next-steps"></a>Nächste Schritte
* [Skalieren von Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Referenz zur Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx)

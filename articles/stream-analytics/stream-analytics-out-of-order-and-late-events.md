---
title: "Behandlung von Ereignissen außerhalb der Reihenfolge oder bei Verzögerung in Azure Stream Analytics | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Stream Analytics mit Ereignissen außerhalb der Reihenfolge oder spät empfangenen Ereignissen in Datenströmen umgeht."
keywords: "außerhalb der Reihenfolge, spät empfangen, Ereignisse"
documentationcenter: 
services: stream-analytics
author: jseb225
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
ms.openlocfilehash: 208dfa14d5d18e106d654539cd80bafdeb90cdf8
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="azure-stream-analytics-event-order-consideration"></a>Berücksichtigung von Ereignissen außerhalb der Reihenfolge in Azure Stream Analytics

## <a name="understand-arrival-time-and-application-time"></a>Grundlegendes zur Eingangszeit und Anwendungszeit

In einem temporalen Datenstrom von Ereignissen wird jedem Ereignis ein Zeitstempel zugewiesen. Azure Stream Analytics weist jedem Ereignis anhand der Eingangszeit oder der Anwendungszeit Zeitstempel zu. Die Spalte „System.Timestamp“ enthält den dem Ereignis zugewiesenen Zeitstempel. Die Eingangszeit wird an der Eingabequelle zugewiesen, wenn das Ereignis die Quelle erreicht. Die Eingangszeit ist EventEnqueuedTime für Event Hub-Eingaben oder die [Zeit der letzten Änderung des Blobs](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) für Blobeingaben. Die Anwendungszeit wird bei Erstellung des Ereignisses zugewiesen und ist Teil der Nutzlast. Verwenden Sie zur Verarbeitung von Ereignissen anhand der Anwendungszeit die „Timestamp by“-Klausel in der SELECT-Abfrage. Ist die „Timestamp by“-Klausel nicht vorhanden, werden Ereignisse basierend auf der Eingangszeit verarbeitet. Auf die Eingangszeit kann bei Event Hubs mithilfe der EventEnqueuedTime-Eigenschaft und bei Blobeingaben mithilfe der BlobLastModified-Eigenschaft zugegriffen werden. Azure Stream Analytics erzeugt die Ausgabe in der Reihenfolge der Zeitstempel und bietet einige Einstellungen zur Behandlung von Daten außerhalb der Reihenfolge.


## <a name="azure-stream-analytics-handling-of-multiple-streams"></a>Behandlung von mehreren Datenströmen in Azure Stream Analytics

Für einen Azure Stream Analytics-Auftrag werden Ereignisse mehrerer Zeitachsen in einigen Fällen kombiniert, z.B.:

* Erzeugen einer Ausgabe aus mehreren Partitionen. Für Abfragen ohne explizite Angabe von „Partition by PartitionId“ müssen Ereignisse aus allen Partitionen kombiniert werden.
* Vereinigung von mindestens zwei unterschiedlichen Eingabequellen
* Verknüpfen von Eingabequellen

Bei Szenarien mit einer Kombination von mehreren Zeitachsen erzeugt Azure Stream Analytics eine Ausgabe für den Zeitstempel *t1* erst, nachdem alle Quellen, die kombiniert wurden, mindestens den Zeitpunkt *t1* erreicht haben.
Beispiel: Wenn die Abfrage aus einer *Event Hub*-Partition mit zwei Partitionen liest und eine Partition (*P1*) übe Ereignisse bis zum Zeitpunkt *t1* und die andere Partition (*P2*) über Ereignisse bis zum Zeitpunkt *t1 + x* verfügt, wird die Ausgabe bis zum Zeitpunkt *t1* erzeugt.
Wenn eine explizite *Partition by PartitionId*-Klausel vorhanden ist, werden beide Partitionen dagegen unabhängig voneinander verarbeitet.
Die Einstellung „Toleranz für Eingangsverzögerung“ wird verwendet, um in einigen Partitionen das Fehlen von Daten verarbeiten zu können.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Konfigurieren der Toleranz für Eingangsverzögerung und der Toleranz für Fehlordnung
Eingabedatenströmen außerhalb der Reihenfolge weisen eine der folgenden Eigenschaften auf:
* Sie wurden sortiert (und sind daher **verzögert**).
* Sie wurden vom System gemäß einer benutzerdefinierten Richtlinie angepasst.

Stream Analytics toleriert bei der Verarbeitung auf Grundlage der **Anwendungszeit** verzögerte Ereignisse und Ereignisse außerhalb der Reihenfolge. Im Azure-Portal sind unter der Option **Ereignisreihenfolge** die folgenden Einstellungen verfügbar: 

![Behandlung von Ereignissen in Stream Analytics](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

**Toleranz für Eingangsverzögerung**
* Diese Einstellung gilt nur für die Verarbeitung auf der Grundlage der Anwendungszeit, andernfalls wird sie ignoriert.
* Hierbei handelt es sich um die maximale Differenz zwischen der Eingangszeit und der Anwendungszeit. Wenn die Anwendungszeit vor (Eingangszeit – Fenster für Eingangsverzögerung) liegt, wird sie auf (Eingangszeit – Fenster für Eingangsverzögerung) festgelegt.
* Wenn mehrere Teile des gleichen Eingabedatenstroms oder mehrere Eingabedatenströme kombiniert werden, ist die Toleranz für die Eingangsverzögerung der maximale Zeitraum, den jeder Teil auf neue Daten wartet. 

Kurz gesagt: Das Fenster für die Eingangsverzögerung ist die maximale Verzögerung zwischen der Ereigniserstellung und dem Eingang des Ereignisses an der Eingabequelle.
Zuerst erfolgt die Anpassung auf der Grundlage der Toleranz für die Eingangsverzögerung, anschließend die Anpassung auf der Grundlage der falschen Reihenfolge. Die Spalte **System.Timestamp** enthält den dem Ereignis zugewiesenen endgültigen Zeitstempel.

**Toleranz für Fehlordnung**
* Ereignisse, die in falscher Reihenfolge eingehen, sich aber im festgelegten Toleranzfenster für Fehlordnung befinden, werden **nach dem Zeitstempel neu angeordnet**. 
* Ereignisse, die außerhalb des Toleranzfensters eintreffen, werden **verworfen oder angepasst**.
    * **Angepasst**: Ereignisse werden so angepasst, als ob sie zum spätmöglichsten zulässigen Zeitpunkt angekommen wären. 
    * **Verworfen**: Ereignisse werden verworfen.

Um Ereignisse, die innerhalb des Toleranzfensters für Fehlordnung eingegangen sind, neu anzuordnen, wird die Ausgabe der Abfrage **um das Toleranzfenster für Fehlordnung verzögert**.

**Beispiel**

* Toleranz für Eingangsverzögerung = 10 Minuten<br/>
* Toleranz für Fehlordnung = 3 Minuten<br/>
* Verarbeitung basierend auf der Anwendungszeit<br/>
* Ereignisse:
   * Ereignis 1: _Anwendungszeit_ = 00:00:00, _Eingangszeit_ = 00:10:01, _System.Timestamp_ = 00:00:01, wird angepasst, da (_Eingangszeit_ - _Anwendungszeit_) größer ist als die Toleranz für Eingangsverzögerung.
   * Ereignis 2: _Anwendungszeit_ = 00:00:01, _Eingangszeit_ = 00:10:01, _System.Timestamp_ = 00:00:01, wird nicht angepasst, da die Anwendungszeit im Fenster für Eingangsverzögerung liegt.
   * Ereignis 3: _Anwendungszeit_ = 00:10:00, _Eingangszeit_ = 00:10:02, _System.Timestamp_ = 00:10:00, wird nicht angepasst, da die Anwendungszeit im Fenster für Eingangsverzögerung liegt.
   * Ereignis 4: _Anwendungszeit_ = 00:09:00, _Eingangszeit_ = 00:10:03, _System.Timestamp_ = 00:09:00, wird mit dem ursprünglichen Zeitstempel akzeptiert, da die Anwendungszeit innerhalb der Toleranz für Fehlordnung liegt.
   * Ereignis 5: _Anwendungszeit_ = 00:06:00, _Eingangszeit_ = 00:10:04, _System.Timestamp_ = 00:07:00, wird angepasst, da die Anwendungszeit vor der Toleranz für Fehlordnung liegt.

## <a name="practical-considerations"></a>Praktische Überlegungen
Wie oben bereits erwähnt, ist die *Toleranz für Eingangsverzögerung* der maximale Unterschied zwischen der Anwendungszeit und der Eingangszeit.
Beim Verarbeiten der Anwendungszeit werden Ereignisse, die nach der konfigurierten *Toleranz für Eingangsverzögerung* eintreten, außerdem angepasst, bevor die Einstellung *Toleranz für Fehlordnung* angewendet wird. „Fehlordnung“ ist also quasi das Minimum der Toleranz für Eingangsverzögerung und der Toleranz für Fehlordnung.

In einem Datenstrom kann es beispielsweise aus folgenden Gründen zur Fehlordnung von Ereignissen kommen:
* Uhrabweichung zwischen den Absendern
* Variable Latenzzeit zwischen Absender und Eingangsereignisquelle

Die Eingangsverzögerung kann beispielsweise folgende Gründe haben:
* Absender fassen die Ereignisse zu einem Batch zusammen und senden sie in einem späteren Intervall
* Latenzzeit zwischen dem Senden des Ereignisses durch den Absender und dem Empfang des Ereignisses in der Eingangsquelle

Beim Konfigurieren von *Toleranz für Eingangsverzögerung* und *Toleranz für Fehlordnung* für einen bestimmten Auftrag sollten die Richtigkeit, die Latenzanforderungen und die obigen Faktoren berücksichtigt werden.

Hier sind einige Beispiele angegeben:

### <a name="example-1"></a>Beispiel 1: 
Die Abfrage verfügt über die Klausel „Partition by PartitionId“, und in einer einzelnen Partition werden Ereignisse mit synchronen Sendemethoden gesendet. Bei synchronen Sendemethoden kommt es zu einer Blockierung, bis das Senden der Ereignisse abgeschlossen ist.
In diesem Fall ist die Fehlordnung null, da die Ereignisse in der richtigen Reihenfolge mit einer expliziten Bestätigung gesendet werden, bevor das nächste Ereignis gesendet wird. Die Eingangsverzögerung ist die maximale Verzögerung zwischen dem Generieren und dem Senden des Ereignisses zuzüglich der maximalen Latenz zwischen Absender und Eingangsquelle.

### <a name="example-2"></a>Beispiel 2:
Die Abfrage verfügt über die Klausel „Partition by PartitionId“ und innerhalb einer einzelnen Partition werden Ereignisse mit der asynchronen Sendemethode gesendet. Bei asynchronen Sendemethoden können mehrere Sendevorgänge gleichzeitig initiiert werden, und dies kann zu einer Fehlordnung von Ereignissen führen.
In diesem Fall gilt sowohl für die Fehlordnung als auch für die Eingangsverzögerung mindestens die maximale Verzögerung zwischen dem Generieren und dem Senden des Ereignisses zuzüglich der maximalen Latenz zwischen Absender und Eingangsquelle.

### <a name="example-3"></a>Beispiel 3:
Die Abfrage verfügt nicht über die Klausel „Partition by PartitionId“, und es sind mindestens zwei Partitionen vorhanden.
Die Konfiguration entspricht Beispiel 2. Das Fehlen von Daten in einer der Partitionen kann aber dazu führen, dass die Ausgabe um ein zusätzliches Fenster „Toleranz für Eingangsverzögerung“ verzögert wird.

## <a name="to-summarize"></a>Zusammenfassung
* Die Toleranz für Eingangsverzögerung und das Fehlordnungsfenster sollten basierend auf der Richtigkeit und den Latenzanforderungen konfiguriert werden, und außerdem sollte berücksichtigt werden, wie die Ereignisse gesendet werden.
* Es ist ratsam, für „Toleranz für Fehlordnung“ einen niedrigeren Wert als für „Toleranz für Eingangsverzögerung“ zu wählen.
* Beim Kombinieren von mehreren Zeitachsen kann das Fehlen von Daten in einer der Quellen oder Partitionen dazu führen, dass die Ausgabe um ein zusätzliches Fenster „Toleranz für Eingangsverzögerung“ verzögert wird.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um weitere Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Referenz zur Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur REST-API für die Stream Analytics-Verwaltung](https://msdn.microsoft.com/library/azure/dn835031.aspx)

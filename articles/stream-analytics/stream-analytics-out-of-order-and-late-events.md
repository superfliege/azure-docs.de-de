---
title: Behandlung von Ereignissen außerhalb der Reihenfolge oder bei Verzögerung in Azure Stream Analytics | Microsoft-Dokumentation
description: Erfahren Sie, wie Stream Analytics mit Ereignissen außerhalb der Reihenfolge oder spät empfangenen Ereignissen in Datenströmen umgeht.
keywords: außerhalb der Reihenfolge, spät empfangen, Ereignisse
documentationcenter: ''
services: stream-analytics
author: jseb225
manager: ryanw
ms.assetid: ''
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: 3c1924ad87715f7a44c3666991e792adc3a20af9
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="azure-stream-analytics-event-order-considerations"></a>Überlegungen zur Ereignisreihenfolge in Azure Stream Analytics

## <a name="arrival-time-and-application-time"></a>Eingangszeit und Anwendungszeit

In einem temporalen Datenstrom von Ereignissen wird jedem Ereignis ein Zeitstempel zugewiesen. Azure Stream Analytics weist jedem Ereignis anhand der Eingangszeit oder der Anwendungszeit einen Zeitstempel zu. Die Spalte **System.Timestamp** enthält den dem Ereignis zugewiesenen Zeitstempel. 

Die Eingangszeit wird an der Eingabequelle zugewiesen, wenn das Ereignis die Quelle erreicht. Auf die Eingangszeit kann bei Event Hub-Eingaben mithilfe der **EventEnqueuedTime**-Eigenschaft und bei Blobeingaben mithilfe der [BlobProperties.LastModified](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3)-Eigenschaft zugegriffen werden. 

Die Anwendungszeit wird bei Erstellung des Ereignisses zugewiesen und ist Teil der Nutzlast. Verwenden Sie zur Verarbeitung von Ereignissen anhand der Anwendungszeit die **Timestamp by**-Klausel in der SELECT-Abfrage. Wenn die **Timestamp by**-Klausel nicht vorhanden ist, werden Ereignisse basierend auf der Eingangszeit verarbeitet. 

Azure Stream Analytics erzeugt die Ausgabe in der Reihenfolge der Zeitstempel und bietet Einstellungen zur Behandlung von Daten außerhalb der Reihenfolge.


## <a name="handling-of-multiple-streams"></a>Umgang mit mehreren Datenströmen

Bei einem Azure Stream Analytics-Auftrag werden Ereignisse mehrerer Zeitachsen in Fällen wie den folgenden kombiniert:

* Erzeugen einer Ausgabe aus mehreren Partitionen. Für Abfragen ohne explizite Angabe einer **Partition by PartitionId**-Klausel müssen Ereignisse aus allen Partitionen kombiniert werden.
* Vereinigung von mindestens zwei unterschiedlichen Eingabequellen
* Verknüpfen von Eingabequellen

Bei Szenarien mit einer Kombination von mehreren Zeitachsen erzeugt Azure Stream Analytics eine Ausgabe für den Zeitstempel *t1* erst, nachdem alle Quellen, die kombiniert wurden, mindestens den Zeitpunkt *t1* erreicht haben. Angenommen, die Abfrage liest aus einer Event Hub-Partition, die zwei Partitionen umfasst. Eine der Partitionen, *P1*, verfügt über Ereignisse bis zum Zeitpunkt *t1*. Die andere Partition *P2* verfügt über Ereignisse bis zum Zeitpunkt *t1 + x*. Die Ausgabe wird dann bis zum Zeitpunkt *t1* erzeugt. Wenn jedoch eine explizite **Partition by PartitionId**-Klausel vorhanden ist, werden beide Partitionen unabhängig voneinander verarbeitet.

Die Einstellungen „Toleranz für Eingangsverzögerung“ wird verwendet, um in einigen Partitionen das Fehlen von Daten verarbeiten zu können.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Konfigurieren der Toleranz für Eingangsverzögerung und der Toleranz für Fehlordnung
Eingabedatenströmen außerhalb der Reihenfolge weisen eine der folgenden Eigenschaften auf:
* Sie wurden sortiert (und sind daher verzögert).
* Sie wurden vom System gemäß einer benutzerdefinierten Richtlinie angepasst.

Stream Analytics toleriert bei der Verarbeitung auf Grundlage der Anwendungszeit verzögerte Ereignisse und Ereignisse außerhalb der Reihenfolge. Im Azure-Portal sind unter der Option **Ereignisreihenfolge** die folgenden Einstellungen verfügbar: 

![Behandlung von Ereignissen in Stream Analytics](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

### <a name="late-arrival-tolerance"></a>Toleranz für Eingangsverzögerung
Die Toleranz für Eingangsverzögerung gilt nur für die Verarbeitung auf der Grundlage der Anwendungszeit. Andernfalls wird die Einstellung ignoriert.

Die Toleranz für Eingangsverzögerung ist die maximale Differenz zwischen der Eingangszeit und der Anwendungszeit. Wenn ein Ereignis nach der Toleranz für Eingangsverzögerung eintritt (z.B. Anwendungszeit *app_t* < Eingangszeit *arr_t* - Toleranz für Eingangsverzögerung *late_t*), wird das Ereignis an den Maximalwert der Toleranz für Eingangsverzögerung (*arr_t* - *late_t*) angepasst. Das Fenster für die Eingangsverzögerung ist die maximale Verzögerung zwischen der Ereigniserstellung und dem Eingang des Ereignisses an der Eingabequelle. 

Wenn mehrere Teile des gleichen Eingabedatenstroms oder mehrere Eingabedatenströme kombiniert werden, ist die Toleranz für die Eingangsverzögerung der maximale Zeitraum, den jeder Teil auf neue Daten wartet. 

Zuerst erfolgt die Anpassung auf der Grundlage der Toleranz für Eingangsverzögerung. Anschließend erfolgt die Anpassung auf der Grundlage der Toleranz für Fehlordnung. Die Spalte **System.Timestamp** enthält den dem Ereignis zugewiesenen endgültigen Zeitstempel.

### <a name="out-of-order-tolerance"></a>Toleranz für Fehlordnung
Ereignisse, die in falscher Reihenfolge eingehen, sich aber im festgelegten Toleranzfenster für Fehlordnung befinden, werden nach dem Zeitstempel neu angeordnet. Ereignisse, die außerhalb des Toleranzfensters eingehen, werden wie folgt verarbeitet:
* **Angepasst**: Ereignisse werden so angepasst, als ob sie zum spätmöglichsten zulässigen Zeitpunkt angekommen wären. 
* **Verworfen**: Ereignisse werden verworfen.

Wenn Ereignisse, die innerhalb des Toleranzfensters für Fehlordnung eingegangen sind, in Stream Analytics neu angeordnet werden, wird die Ausgabe der Abfrage um das Toleranzfenster für Fehlordnung verzögert.

### <a name="early-events"></a>Frühe Ereignisse
Bei der Verarbeitung nach Anwendungszeit werden Ereignisse, deren Anwendungszeit mehr als 5 Minuten vor der Ankunftszeit liegt, entweder gelöscht oder gemäß der ausgewählten Konfigurationsoption angepasst.

### <a name="example"></a>Beispiel

* Toleranz für Eingangsverzögerung = 10 Minuten<br/>
* Toleranz für Fehlordnung = 3 Minuten<br/>
* Verarbeitung basierend auf der Anwendungszeit<br/>
* Ereignisse:
   1. **Anwendungszeit** = 00:00:00, **Eingangszeit** = 00:10:01, **System.Timestamp** = 00:00:01: Wird angepasst, da (**Eingangzeit - Anwendungszeit**) größer ist als die Toleranz für Eingangsverzögerung.
   2. **Anwendungszeit** = 00:00:01, **Eingangszeit** = 00:10:01, **System.Timestamp** = 00:00:01: Wird nicht angepasst, da die Anwendungszeit im Fenster für Eingangsverzögerung liegt.
   3. **Anwendungszeit** = 00:10:00, **Eingangszeit** = 00:10:02, **System.Timestamp** = 00:10:00: Wird nicht angepasst, da die Anwendungszeit im Fenster für Eingangsverzögerung liegt.
   4. **Anwendungszeit** = 00:09:00, **Eingangszeit** =00:10:03, **System.Timestamp** = 00:09:00: Wird mit dem ursprünglichen Zeitstempel akzeptiert, da die Anwendungszeit innerhalb der Toleranz für Fehlordnung liegt.
   5. **Anwendungszeit** = 00:06:00, **Eingangszeit** = 00:10:04, **System.Timestamp** = 00:07:00: Wird angepasst, da die Anwendungszeit vor der Toleranz für Fehlordnung liegt.

### <a name="practical-considerations"></a>Praktische Überlegungen
Wie bereits erwähnt, ist die Toleranz für Eingangsverzögerung der maximale Unterschied zwischen der Anwendungszeit und der Eingangszeit. Beim Verarbeiten der Anwendungszeit werden Ereignisse, die nach der konfigurierten Toleranz für Eingangsverzögerung eintreten, angepasst, bevor die Einstellung „Toleranz für Fehlordnung“ angewendet wird. „Fehlordnung“ ist also quasi das Minimum der Toleranz für Eingangsverzögerung und der Toleranz für Fehlordnung.

Gründe für die Fehlordnung von Ereignissen in einem Datenstrom:
* Uhrabweichung zwischen den Absendern
* Variable Latenzzeit zwischen dem Absender und der Eingangsereignisquelle

Gründe für die Eingangsverzögerung:
* Absender fassen die Ereignisse zu einem Batch zusammen und senden sie in einem späteren Intervall
* Latenzzeit zwischen dem Senden des Ereignisses durch den Absender und dem Empfang des Ereignisses in der Eingangsquelle

Beim Konfigurieren der Toleranz für Eingangsverzögerung und der Toleranz für Fehlordnung für einen bestimmten Auftrag sollten die Richtigkeit, die Latenzanforderungen und die oben genannten Faktoren berücksichtigt werden.

Es folgen einige Beispiele.

#### <a name="example-1"></a>Beispiel 1 
Die Abfrage verfügt über eine **Partition by PartitionId**-Klausel. In einer einzelnen Partition werden Ereignisse mit synchronen Sendemethoden gesendet. Bei synchronen Sendemethoden kommt es zu einer Blockierung, bis das Senden der Ereignisse abgeschlossen ist.

In diesem Fall ist die Fehlordnung null, da die Ereignisse in der richtigen Reihenfolge mit einer expliziten Bestätigung gesendet werden, bevor das nächste Ereignis gesendet wird. Die Eingangsverzögerung ist die maximale Verzögerung zwischen dem Generieren und dem Senden des Ereignisses zuzüglich der maximalen Latenz zwischen dem Absender und der Eingangsquelle.

#### <a name="example-2"></a>Beispiel 2
Die Abfrage verfügt über eine **Partition by PartitionId**-Klausel. In einer einzelnen Partition werden Ereignisse mit asynchronen Sendemethoden gesendet. Bei asynchronen Sendemethoden können mehrere Sendevorgänge gleichzeitig initiiert werden, und dies kann zu einer Fehlordnung von Ereignissen führen.

In diesem Fall gilt sowohl für die Fehlordnung als auch für die Eingangsverzögerung mindestens die maximale Verzögerung zwischen dem Generieren und dem Senden des Ereignisses zuzüglich der maximalen Latenz zwischen dem Absender und der Eingangsquelle.

#### <a name="example-3"></a>Beispiel 3
Die Abfrage verfügt über keine **Partition by PartitionId**-Klausel, und es sind mindestens zwei Partitionen vorhanden.

Die Konfiguration entspricht der in Beispiel 2. Das Fehlen von Daten in einer der Partitionen kann aber dazu führen, dass die Ausgabe um ein zusätzliches Fenster „Toleranz für Eingangsverzögerung“ verzögert wird.

## <a name="handling-event-producers-with-differing-timelines"></a>Behandlung von Ereignisproducern mit unterschiedlichen Zeitachsen
Ein einzelner Eingabeereignisdatenstrom enthält häufig Ereignisse, die von mehreren Ereignisproducern stammen, z.B. von einzelnen Geräten. Diese Ereignisse können aus den zuvor erläuterten Gründen in falscher Reihenfolge eingehen. Obwohl die Fehlordnung in diesen Szenarien innerhalb der Ereignisproducer hoch ist, ist sie innerhalb der Ereignisse eines einzelnen Producers gering (oder auch nicht vorhanden).

Azure Stream Analytics bietet allgemeine Mechanismen zur Behandlung von Ereignissen außerhalb der Reihenfolge. Diese Mechanismen führen zu Verarbeitungsverzögerungen (die durch Warten auf den Eingang der verstreuten Ereignisse in das System entstehen), gelöschten oder angepassten Ereignissen oder zu beidem.

Dennoch verarbeitet die gewünschte Abfrage in vielen Szenarien Ereignisse aus unterschiedlichen Ereignisproducern unabhängig voneinander. Möglicherweise werden Ereignisse z.B. pro Fenster oder pro Gerät aggregiert. In diesen Fällen muss die Ausgabe, die einem Ereignisproducer entspricht, nicht verzögert werden, um auf die anderen Ereignisproducer zu warten. Das heißt, die Zeitabweichung zwischen den Producern muss nicht berücksichtigt werden. Sie kann daher ignoriert werden.

Dies bedeutet natürlich, dass die eigentlichen Ausgabeereignisse in Bezug auf ihre Zeitstempel außerhalb der Reihenfolge liegen. Dieses Verhalten muss vom Downstreamconsumer verarbeitet werden können. Die einzelnen Ereignisse in der Ausgabe sind jedoch richtig.

Azure Stream Analytics implementiert diese Funktion mithilfe der [TIMESTAMP BY OVER](https://msdn.microsoft.com/library/azure/mt573293.aspx)-Klausel.

## <a name="summary"></a>Zusammenfassung
* Konfigurieren Sie die Toleranz für Eingangsverzögerung und das Fenster für Fehlordnung basierend auf der Richtigkeit und den Latenzanforderungen. Berücksichtigen Sie außerdem, wie die Ereignisse gesendet werden.
* Es wird empfohlen, für „Toleranz für Fehlordnung“ einen niedrigeren Wert als für „Toleranz für Eingangsverzögerung“ auszuwählen.
* Beim Kombinieren von mehreren Zeitachsen kann das Fehlen von Daten in einer der Quellen oder Partitionen dazu führen, dass die Ausgabe um ein zusätzliches Fenster „Toleranz für Eingangsverzögerung“ verzögert wird.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um weitere Hilfe zu erhalten, nutzen Sie das [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Referenz zur Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur REST-API für die Stream Analytics-Verwaltung](https://msdn.microsoft.com/library/azure/dn835031.aspx)

---
title: Streamen von Daten als Eingabe in Azure Stream Analytics
description: Hier erfahren Sie, wie eine Datenverbindung mit Stream Analytics eingerichtet wird. Eingaben umfassen einen Datenstrom von Ereignissen und Verweisdaten.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/19/2018
ms.openlocfilehash: 5ebf2d1025c8f9469a83a408cb79e3d944a601bc
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Streamen von Daten als Eingabe in Stream Analytics

Stream Analytics akzeptiert eingehende Daten aus verschiedenen Arten von Ereignisquellen. Die Datenverbindung, die als Eingabe in einen Stream Analytics-Auftrag bereitgestellt wird, wird als *Eingabe* des Auftrags bezeichnet. 

Stream Analytics verfügt über eine erstklassige Integration in Azure-Datenströme als Eingaben aus drei Arten von Ressourcen:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) 

Diese Eingabequellen können aus demselben Azure-Abonnement wie dem Ihres Stream Analytics-Auftrags oder aus einem anderen Abonnement stammen.

## <a name="compare-stream-and-reference-inputs"></a>Vergleichen von Stream- und Verweiseingaben
Werden Daten mithilfe von Push an eine Datenquelle übertragen, werden sie vom Stream Analytics-Auftrag genutzt und in Echtzeit verarbeitet. Eingaben werden in zwei Typen unterteilt: Datenstromeingaben und Verweisdateneingaben.

### <a name="data-stream-input"></a>Datenstromeingabe
Ein Datenstrom ist eine ungebundene Abfolge von Ereignissen im Verlauf der Zeit. Stream Analytics-Aufträge müssen mindestens eine Datenstromeingabe enthalten. Event Hubs, IoT Hubs und Blob Storage werden als Datenstrom-Eingabequellen unterstützt. Event Hubs werden verwendet, um Ereignisdatenströme von verschiedenen Geräten und Diensten zu erfassen. Bei diesen Datenströmen kann es sich beispielsweise um Aktivitätsfeeds sozialer Medien, Börseninformationen oder Daten von Sensoren handeln. IoT Hubs sind zum Sammeln von Daten von verbundenen Geräten in IoT-Szenarien (Internet of Things) optimiert.  Blob Storage kann als Eingabequelle für die Erfassung von Massendaten als Datenstrom (z.B. Protokolldateien) verwendet werden.  

### <a name="reference-data-input"></a>Verweisdateneingabe
Stream Analytics unterstützt auch Eingaben wie *Verweisdaten*. Dies sind Hilfsdaten, die entweder statisch sind oder sich langsam ändern. Verweisdaten werden typischerweise für Korrelation und Suchvorgänge verwendet. Beispielsweise können Sie Daten in der Datenstromeingabe mit den Daten in den Verweisdaten verknüpfen – ähnlich wie bei einer SQL-Verknüpfung zum Suchen statischer Werte. Azure Blob Storage ist derzeit die einzige unterstützte Eingabequelle für Verweisdaten. Blobs für Verweisdatenquellen sind auf eine Größe von 100 MB beschränkt.

Informationen zum Erstellen von Verweisdateneingaben finden Sie unter [Verwenden von Verweisdaten](stream-analytics-use-reference-data.md).  

### <a name="compression"></a>Komprimierung
Stream Analytics unterstützt die Komprimierung für alle Datenstrom-Eingabequellen. Derzeit werden folgende Verweistypen unterstützt: „Keine“, „GZip“ und „Deflate“. Die Unterstützung der Komprimierung steht für Referenzdaten nicht zur Verfügung. Wenn das Eingabeformat komprimierte Avro-Daten sind, werden sie transparent behandelt. Sie müssen bei der Avro-Serialisierung keinen Komprimierungstyp angeben. 

## <a name="create-or-edit-inputs"></a>Erstellen oder Bearbeiten von Eingaben
Um neue Eingaben zu erstellen und bestehende Eingaben in Ihrem Streamingauftrag aufzulisten oder zu bearbeiten, können Sie das Azure-Portal verwenden:
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), um Ihren Stream Analytics-Auftrag zu finden und auszuwählen.
2. Wählen Sie unter der Überschrift **EINSTELLUNGEN** die Option **Eingaben**. 
4. Auf der Seite **Eingaben** sind alle vorhandenen Eingaben aufgeführt. 
5. Wählen Sie auf der Seite **Eingaben** die Option **Datenstromeingabe hinzufügen** oder **Referenzeingabe hinzufügen**, um die Detailseite zu öffnen.
6. Wählen Sie eine vorhandene Eingabe aus, um die Details zu bearbeiten, und wählen Sie **Speichern**, um eine vorhandene Eingabe zu bearbeiten.
7. Wählen Sie auf der Seite mit den Eingabedetails **Test**, um zu überprüfen, ob die Verbindungsoptionen gültig und funktionsfähig sind. 
8. Klicken Sie mit der rechten Maustaste auf den Namen einer vorhandenen Eingabe, und wählen Sie **Beispieldaten aus Eingabe** für weitere Tests.

Sie können auch [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/New-AzureRmStreamAnalyticsInput), [.Net-API](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [REST-API](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-input) und [Visual Studio](stream-analytics-tools-for-visual-studio.md) zum Erstellen, Bearbeiten und Testen von Stream Analytics-Auftragseingaben verwenden.

## <a name="stream-data-from-event-hubs"></a>Streamen von Daten aus Event Hubs

Azure Event Hubs sind hoch skalierbare Ereigniserfasser zum Veröffentlichen/Abonnieren. Ein Event Hub kann Millionen von Ereignissen pro Sekunde erfassen. Auf diese Weise können Sie riesige Datenmengen verarbeiten und analysieren, die von vernetzten Geräten und Anwendungen erzeugt werden. Event Hubs und Stream Analytics bieten Ihnen zusammen eine End-to-End-Lösung für Echtzeitanalysen. Event Hubs ermöglichen es Ihnen, Ereignisse in Echtzeit an Azure zu übergeben, sodass Stream Analytics-Aufträge diese Ereignisse in Echtzeit verarbeiten können. Beispielsweise können Sie Webklicks, Sensormesswerte oder Onlineprotokollereignisse an Event Hubs senden. Anschließend können Sie Stream Analytics-Aufträge erstellen, um Event Hubs als Eingabedatenströme zum Filtern, Aggregieren und Korrelieren in Echtzeit zu verwenden.

Der Standardzeitstempel von Ereignissen, die von Event Hubs in Stream Analytics stammen, ist der Zeitstempel, an dem das Ereignis im Event Hub eingeht, also `EventEnqueuedUtcTime`. Zum Verarbeiten der Daten als Datenstrom mit einem Zeitstempel in der Ereignisnutzlast müssen Sie das Schlüsselwort [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwenden.

### <a name="consumer-groups"></a>Verbrauchergruppen
Sie sollten für jede Event Hub-Eingabe in Stream Analytics eine eigene Consumergruppe konfigurieren. Wenn ein Auftrag eine Selbstverknüpfung oder mehrere Eingaben enthält, können einige Eingaben möglicherweise von mehreren nachgeschalteten Lesern gelesen werden. Dies wirkt sich auf die Anzahl der Leser in einer einzelnen Consumergruppe aus. Zur Vermeidung der Überschreitung des Event Hub-Limits von fünf Lesern pro Consumergruppe pro Partition empfiehlt es sich, eine Consumergruppe für jeden Stream Analytics-Auftrag anzugeben. Darüber hinaus gilt ein Grenzwert von 20 Consumergruppen pro Event Hub. Weitere Informationen finden Sie im [Programmierleitfaden zu Event Hubs](../event-hubs/event-hubs-programming-guide.md).

### <a name="stream-data-from-event-hubs"></a>Streamen von Daten aus Event Hubs
In der folgenden Tabelle wird jede Eigenschaft im Azure-Portal auf der Seite **Neue Eingabe** erläutert, um Dateneingaben aus einem Event Hub zu streamen:

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| **Eingabealias** |Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Eingabe zu verweisen. |
| **Abonnement** | Wählen Sie das Abonnement, in dem die Event Hub-Ressource vorhanden ist. | 
| **Event Hub-Namespace** | Ein Event Hub-Namespace ist ein Container für einen Satz von Nachrichtenentitäten. Sie haben bei der Erstellung einer neuen Event Hub-Instanz auch den Namespace erstellt. |
| **Event Hub-Name** | Der Name des Event Hubs, der als Eingabe verwendet wird. |
| **Event Hub-Richtlinienname** | Die SAS-Richtlinie, die Zugriff auf den Event Hub ermöglicht. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. Diese Option wird automatisch ausgefüllt, es sei denn, Sie wählen die Option zum manuellen Festlegen der Event-Hub-Einstellungen.|
| **Event Hub-Consumergruppe** (empfohlen) | Es wird dringend empfohlen, für jeden Stream Analytics-Auftrag eine eigene Consumergruppe zu verwenden. Diese Zeichenfolge identifiziert die Consumergruppe, die zum Erfassen von Daten aus dem Event Hub verwendet werden soll. Wenn keine Consumergruppe angegeben wird, verwendet der Stream Analytics-Auftrag die $Default-Consumergruppe.  |
| **Ereignisserialisierungsformat** | Das Serialisierungsformat (JSON, CSV oder Avro) des eingehenden Datenstroms. |
| **Codieren** | UTF-8 ist derzeit das einzige unterstützte Codierungsformat. |
| **Typ der Ereigniskomprimierung** | Der Komprimierungstyp, der zum Lesen des eingehenden Datenstroms verwendet wird, z.B. „Keine“, „GZip“ oder „Deflate“. |

Wenn Ihre Daten aus einer Event Hub-Datenstromeingabe stammen, haben Sie Zugriff auf folgende Metadatenfelder in Ihrer Stream Analytics-Abfrage:

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| **EventProcessedUtcTime** |Das Datum und die Uhrzeit der Verarbeitung des Ereignisses durch Stream Analytics. |
| **EventEnqueuedUtcTime** |Das Datum und die Uhrzeit des Ereignisempfangs durch die Event Hubs. |
| **PartitionId** |Die nullbasierte Partitions-ID für den Eingabeadapter. |

Beispielsweise können Sie anhand dieser Felder eine wie im folgenden Beispiel gezeigte Abfrage schreiben:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> Bei Verwendung von Event Hub als Endpunkt für IoT Hub-Routen können Sie mithilfe der [GetMetadataPropertyValue-Funktion](https://msdn.microsoft.com/en-us/library/azure/mt793845.aspx) auf die IoT Hub-Metadaten zugreifen.
> 

## <a name="stream-data-from-iot-hub"></a>Streamen von Daten aus IoT Hub
Azure IoT Hub ist ein hochgradig skalierbares Erfassungsmodul für das Veröffentlichen und Abonnieren von Ereignissen, das für IoT-Szenarien optimiert ist.

Der Standardzeitstempel von Ereignissen, die von IoT Hub in Stream Analytics stammen, ist der Zeitstempel, an dem das Ereignis in IoT Hub eingeht, also `EventEnqueuedUtcTime`. Zum Verarbeiten der Daten als Datenstrom mit einem Zeitstempel in der Ereignisnutzlast müssen Sie das Schlüsselwort [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwenden.

> [!NOTE]
> Nur Nachrichten, die mit der `DeviceClient`-Eigenschaft gesendet wurden, können verarbeitet werden.
> 

### <a name="consumer-groups"></a>Verbrauchergruppen
Sie sollten für jede IoT Hub-Eingabe in Stream Analytics eine eigene Consumergruppe konfigurieren. Wenn ein Auftrag eine Selbstverknüpfung oder mehrere Eingaben enthält, können einige Eingaben möglicherweise von mehreren nachgeschalteten Lesern gelesen werden. Dies wirkt sich auf die Anzahl der Leser in einer einzelnen Consumergruppe aus. Zur Vermeidung der Überschreitung des Azure IoT Hub-Limits von fünf Lesern pro Consumergruppe pro Partition empfiehlt es sich, eine Consumergruppe für jeden Stream Analytics-Auftrag anzugeben.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Konfigurieren von IoT Hub als Datenstromeingabe
In der folgenden Tabelle wird jede Eigenschaft im Azure-Portal auf der Seite **Neue Eingabe** erläutert, wenn Sie IoT Hub als Datenstromeingabe konfigurieren.

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| **Eingabealias** | Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Eingabe zu verweisen.|
| **Abonnement** | Wählen Sie das Abonnement, in dem die IoT Hub-Ressource vorhanden ist. | 
| **IoT Hub** | Der Name von IoT Hub, der als Eingabe verwendet wird. |
| **Endpunkt** | Der Endpunkt für IoT Hub.|
| **Name der SAS-Richtlinie** | Die SAS-Richtlinie, die Zugriff auf IoT Hub ermöglicht. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| **Schlüssel für SAS-Richtlinie** | Der Schlüssel für den gemeinsamen Zugriff, der für die Autorisierung des Zugriffs auf IoT Hub verwendet wird.  Diese Option wird automatisch ausgefüllt, es sei denn, Sie wählen die Option zum manuellen Festlegen der IoT Hub-Einstellungen. |
| **Consumergruppe** | Es wird dringend empfohlen, für jeden Stream Analytics-Auftrag eine andere Consumergruppe zu verwenden. Die Consumergruppe, die zum Erfassen von Daten aus IoT Hub verwendet werden soll. Stream Analytics verwendet die $Default-Consumergruppe, sofern nicht anders angegeben.  |
| **Ereignisserialisierungsformat** | Das Serialisierungsformat (JSON, CSV oder Avro) des eingehenden Datenstroms. |
| **Codieren** | UTF-8 ist derzeit das einzige unterstützte Codierungsformat. |
| **Typ der Ereigniskomprimierung** | Der Komprimierungstyp, der zum Lesen des eingehenden Datenstroms verwendet wird, z.B. „Keine“, „GZip“ oder „Deflate“. |


Wenn Ihre gestreamten Daten aus IoT Hub stammen, haben Sie Zugriff auf folgende Metadatenfelder in Ihrer Stream Analytics-Abfrage:

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| **EventProcessedUtcTime** | Das Datum und die Uhrzeit der Verarbeitung des Ereignisses. |
| **EventEnqueuedUtcTime** | Das Datum und die Uhrzeit des Ereignisempfangs durch den IoT Hub. |
| **PartitionId** | Die nullbasierte Partitions-ID für den Eingabeadapter. |
| **IoTHub.MessageId** | Eine ID, die zum Korrelieren einer bidirektionalen Kommunikation in IoT Hub verwendet wird. |
| **IoTHub.CorrelationId** | Eine ID, die in Nachrichtenantworten und im Feedback in IoT Hub verwendet wird. |
| **IoTHub.ConnectionDeviceId** | Die Authentifizierung-ID, die zum Senden dieser Nachricht verwendet wird. Dieser Wert wird auf servicebound-Nachrichten von IoT Hub gestempelt. |
| **IoTHub.ConnectionDeviceGenerationId** | Die Generierungs-ID des authentifizierten Geräts, das zum Senden dieser Nachricht verwendet wurde. Dieser Wert wird auf servicebound-Nachrichten von IoT Hub gestempelt. |
| **IoTHub.EnqueuedTime** | Der Zeitpunkt, an dem die Nachricht durch IoT Hub empfangen wurde. |
| **IoTHub.StreamId** | Eine benutzerdefinierte Ereigniseigenschaft, die vom Absendergerät hinzugefügt wird. |


## <a name="stream-data-from-blob-storage"></a>Streamen von Daten aus Blob Storage
Für Szenarien mit großen Mengen unstrukturierter Daten, die in der Cloud gespeichert werden sollen, bietet der Azure Blob Storage eine kostengünstige und skalierbare Lösung. Daten in Blob Storage werden in der Regel als ruhende Daten angesehen. Allerdings können Blobdaten als Datenstrom von Stream Analytics verarbeitet werden. 

Ein häufig verwendetes Szenario für Blob Storage-Eingaben bei Stream Analytics ist die Protokollverarbeitung. In diesem Szenario wurden Telemetriedatendateien von einem System erfasst, die zur Extraktion aussagekräftiger Daten analysiert und verarbeitet werden müssen.

Der Standardzeitstempel von Blob Storage-Ereignissen in Stream Analytics ist der Zeitstempel, an dem das Blob zuletzt geändert wurde, also `BlobLastModifiedUtcTime`. Zum Verarbeiten der Daten als Datenstrom mit einem Zeitstempel in der Ereignisnutzlast müssen Sie das Schlüsselwort [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwenden.

Eingaben im CSV-Format *müssen* über eine Überschriftenzeile verfügen, um Felder für das Dataset zu definieren. Alle Felder der Überschriftenzeile müssen eindeutig sein.

Stream Analytics unterstützt derzeit keine Deserialisierung von AVRO-Nachrichten, die durch die Event Hub-Erfassung oder benutzerdefinierte Endpunkte von IoT Hub Azure Storage-Containern generiert wurden.

> [!NOTE]
> Stream Analytics unterstützt das Hinzufügen von Inhalten zu einer vorhandenen Blobdatei nicht. Stream Analytics zeigt jede Datei nur einmal an. Des Weiteren werden alle Änderungen, die in der Datei vorgenommen wurden, nachdem der Auftrag die Daten gelesen hat, nicht verarbeitet. Die Methode, alle Daten für eine Blobdatei auf einmal hochzuladen und dann zusätzliche neuere Ereignisse einer anderen, neuen Blobdatei hinzuzufügen, hat sich bewährt.
> 

### <a name="configure-blob-storage-as-a-stream-input"></a>Konfigurieren von Blob Storage als Datenstromeingabe 

In der folgenden Tabelle wird jede Eigenschaft im Azure-Portal auf der Seite **Neue Eingabe** erläutert, wenn Sie Blob Storage als Datenstromeingabe konfigurieren.

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| **Eingabealias** | Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Eingabe zu verweisen. |
| **Abonnement** | Wählen Sie das Abonnement, in dem die IoT Hub-Ressource vorhanden ist. | 
| **Speicherkonto** | Der Name des Speicherkontos an, in dem sich die Blobdateien befinden. |
| **Speicherkontoschlüssel** | Der geheime Schlüssel, der dem Speicherkonto zugeordnet ist. Diese Option wird automatisch ausgefüllt, es sei denn, Sie wählen die Option zum manuellen Festlegen der Blob Storage-Einstellungen. |
| **Container** | Der Container für die Blobeingabe. Container stellen eine logische Gruppierung für Blobs bereit, die im Microsoft Azure-Blobdienst gespeichert sind. Wenn Sie ein Blob in den Azure Blob Storage-Dienst hochladen, müssen Sie einen Container für dieses Blob angeben. Sie können entweder **Vorhandenes Element verwenden** oder **Neues Element erstellen** wählen, um einen neuen Container zu erstellen.|
| **Pfadmuster** (optional) | Der Dateipfad, der verwendet wird, um die Blobs im angegebenen Container zu suchen. In dem Pfad können Sie mindestens eine Instanz der folgenden drei Variablen angeben: `{date}`, `{time}` oder `{partition}`.<br/><br/>Beispiel 1: `cluster1/logs/{date}/{time}/{partition}`<br/><br/>Beispiel 2: `cluster1/logs/{date}`<br/><br/>Das Zeichen `*` ist kein zulässiger Wert für das Pfadpräfix. Es sind nur gültige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure Blob-Zeichen</a> zulässig. |
| **Datumsformat** (optional) | Wenn Sie die Datumsvariable im Pfad verwenden, wird das Datumsformat, in dem die Dateien organisiert sind, verwendet. Beispiel: `YYYY/MM/DD` |
| **Zeitformat** (optional) |  Wenn Sie die Zeitvariable im Pfad verwenden, wird das Zeitformat, in dem die Dateien organisiert sind, verwendet. Der einzige derzeit unterstützte Wert ist `HH` für Stunden. |
| **Ereignisserialisierungsformat** | Das Serialisierungsformat (JSON, CSV oder Avro) für eingehende Datenströme. |
| **Codieren** | Bei CSV und JSON ist UTF-8 gegenwärtig das einzige unterstützte Codierungsformat. |
| **Komprimierung** | Der Komprimierungstyp, der zum Lesen des eingehenden Datenstroms verwendet wird, z.B. „Keine“, „GZip“ oder „Deflate“. |

Wenn Ihre Daten aus einer Blob Storage-Quelle stammen, haben Sie Zugriff auf folgende Metadatenfelder in Ihrer Stream Analytics-Abfrage:

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| **BlobName** |Der Name des Eingabe-Blobs, aus dem das Ereignis stammt. |
| **EventProcessedUtcTime** |Das Datum und die Uhrzeit der Verarbeitung des Ereignisses durch Stream Analytics. |
| **BlobLastModifiedUtcTime** |Das Datum und die Uhrzeit der letzten Änderung des Blobs. |
| **PartitionId** |Die nullbasierte Partitions-ID für den Eingabeadapter. |

Beispielsweise können Sie anhand dieser Felder eine wie im folgenden Beispiel gezeigte Abfrage schreiben:

```sql
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
```

## <a name="next-steps"></a>Nächste Schritte
Sie haben sich mit Datenverbindungsoptionen in Azure für Ihre Stream Analytics-Aufträge vertraut gemacht. Weitere Informationen zu Stream Analytics finden Sie unter:

* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

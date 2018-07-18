---
title: Verwenden von Referenzdaten für Suchvorgänge in Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie Sie Referenzdaten nutzen, um nach Daten in einem Abfragedesign eines Azure Stream Analytics-Auftrags zu suchen oder diese zu korrelieren.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 25c25a58b4c6eab5419f645e8e916e034e7803dd
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016889"
---
# <a name="using-reference-data-for-lookups-in-stream-analytics"></a>Verwenden von Referenzdaten für Suchvorgänge in Stream Analytics
Verweisdaten werden auch als Nachschlagetabelle bezeichnet und sind ein begrenztes statisches oder sich nur langsam veränderndes Dataset, das für die Suche oder Korrelation mit Ihrem Datenstrom verwendet wird. Azure Stream Analytics lädt Verweisdaten in den Arbeitsspeicher, um eine Streamverarbeitung mit geringer Wartezeit zu erreichen. Für den Einsatz von Verweisdaten in Ihrem Azure Stream Analytics-Auftrag verwenden Sie in der Regel [Verweisdaten für JOIN-Vorgänge](https://msdn.microsoft.com/library/azure/dn949258.aspx) in Ihrer Abfrage. Stream Analytics verwendet Azure Blob Storage als Speicherschicht für Verweisdaten, und mit Azure Data Factory können Verweisdaten aus einer [beliebigen Anzahl von cloudbasierten und lokalen Datenspeichern](../data-factory/copy-activity-overview.md) transformiert und/oder in Azure Blob Storage kopiert werden, um sie als Verweisdaten zu verwenden. Referenzdaten werden als (in der Eingabekonfiguration definierte) Blobsequenz in aufsteigender Reihenfolge nach dem im Blobnamen angegebenen Datums- bzw. Uhrzeitwert modelliert. Hinzufügungen sind jeweils **nur** am Sequenzende möglich. Hierzu muss der verwendete Datums-/Uhrzeitwert den Wert des letzten Blobs in der Sequenz **übersteigen**.

Stream Analytics unterstützt Verweisdaten mit einer **Größe von bis zu 300 MB**. Der Grenzwert von 300 MB für die maximale Größe von Verweisdaten ist nur mit einfachen Abfragen erreichbar. Angesichts der zunehmenden Abfragekomplexität durch Einbeziehung der statusbehafteten Verarbeitung (beispielsweise in Form von Aggregaten im Fenstermodus, temporalen Joins oder temporalen Analysefunktion) ist davon auszugehen, dass die unterstützte Maximalgröße für Verweisdaten abnimmt. Wenn Azure Stream Analytics die Verweisdaten nicht laden und keine komplexen Vorgänge ausführen kann, steht für den Auftrag nicht genügend Arbeitsspeicher zur Verfügung, und der Auftrag ist nicht erfolgreich. In solchen Fällen erreicht die Metrik „Nutzung der Speichereinheit in %“ den Wert „100 %“.    

|**Anzahl von Streamingeinheiten**  |**Ungefähre unterstützte Maximalgröße (in MB)**  |
|---------|---------|
|1   |50   |
|3   |150   |
|6 und mehr   |300   |

Ab einer Anzahl von sechs Streamingeinheiten für einen Auftrag erhöht sich die unterstützte Maximalgröße für Verweisdaten nicht weiter.

Die Unterstützung der Komprimierung steht für Referenzdaten nicht zur Verfügung. 

## <a name="configuring-reference-data"></a>Konfigurieren von Verweisdaten
Um die Verweisdaten zu konfigurieren, müssen Sie zunächst eine Eingabe vom Typ **Verweisdaten**erstellen. Die folgende Tabelle enthält den Namen jeder Eigenschaft, die Sie beim Erstellen der Verweisdateneingabe angeben müssen, sowie die entsprechenden Beschreibungen:

|**Eigenschaftenname**  |**Beschreibung**  |
|---------|---------|
|Eingabealias   | Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Eingabe zu verweisen.   |
|Speicherkonto   | Der Name des Speicherkontos, in dem sich Ihre Blobs befinden. Wenn sich das Konto im gleichen Abonnement befindet wie Ihr Stream Analytics-Auftrag, können Sie es in der Dropdownliste auswählen.   |
|Speicherkontoschlüssel   | Der geheime Schlüssel, der dem Speicherkonto zugeordnet ist. Dies wird automatisch aufgefüllt, wenn sich das Speicherkonto im selben Abonnement befindet wie Ihr Stream Analytics-Auftrag.   |
|Speichercontainer   | Container stellen eine logische Gruppierung für Blobs bereit, die im Microsoft Azure-Blobdienst gespeichert sind. Wenn Sie ein Blob in den Blobdienst hochladen, müssen Sie einen Container für das Blob angeben.   |
|Pfadmuster   | Der Pfad, der verwendet wird, um Ihre Blobs innerhalb des angegebenen Containers zu suchen. In dem Pfad können Sie mindestens eine Instanz der folgenden beiden Variablen angeben:<BR>{date}, {time}<BR>Beispiel 1: products/{date}/{time}/product-list.csv<BR>Beispiel 2: products/{date}/product-list.csv   |
|Datumsformat [optional]   | Wenn Sie innerhalb des von Ihnen angegebenen Pfadmusters „{date}“ verwendet haben, können Sie in der Dropdownliste mit den unterstützten Formaten das Datumsformat auswählen, in dem Ihre Blobs gespeichert werden sollen.<BR>Beispiele: YYYY/MM/DD, MM/DD/YYYY...   |
|Zeitformat [optional]   | Wenn Sie innerhalb des von Ihnen angegebenen Pfadmusters „{time}“ verwendet haben, können Sie in der Dropdownliste mit den unterstützten Formaten das Zeitformat auswählen, in dem Ihre Blobs gespeichert werden sollen.<BR>Beispiele: HH, HH/mm, HH-mm  |
|Ereignisserialisierungsformat   | Um sicherzustellen, dass Ihre Abfragen wie erwartet funktionieren, muss Stream Analytics das Serialisierungsformat kennen, das Sie für eingehende Datenströme verwenden. Die unterstützten Formate für Verweisdaten sind CSV und JSON.  |
|Codieren   | UTF-8 ist derzeit das einzige unterstützte Codierungsformat.  |

## <a name="generating-reference-data-on-a-schedule"></a>Generieren von Verweisdaten nach einem Zeitplan
Wenn es sich bei Ihren Verweisdaten um ein sich langsam änderndes Dataset handelt, wird die Unterstützung für das Aktualisieren von Verweisdaten aktiviert, indem Sie in der Eingabekonfiguration ein Pfadmuster mit den Ersetzungstoken „{date}“ und „{time}“ angeben. Stream Analytics ruft die aktualisierten Definitionen von Verweisdaten auf der Grundlage dieses Pfadmusters ab. Beispiel: Das Muster `sample/{date}/{time}/products.csv` mit dem Datumsformat **YYYY-MM-DD** und dem Zeitformat **HH-mm** weist Stream Analytics an, das aktualisierte Blob `sample/2015-04-16/17-30/products.csv` am 16. April 2015 um 17:30 (UTC-Zeitzone) abzurufen.

> [!NOTE]
> Stream Analytics-Aufträge suchen derzeit nur dann nach der Blobaktualisierung, wenn die Zeit des Computers die im Blobnamen codierte Zeit erreicht. Der Auftrag sucht beispielsweise am 16. April 2015 ab 17:30 Uhr (UTC-Zeitzone) zum frühestmöglichen Zeitpunkt nach `sample/2015-04-16/17-30/products.csv`. Er sucht *nie* nach einem Blob mit einer codierten Zeit vor der letzten Erkennung.
> 
> Beispiel: Nachdem der Auftrag das Blob `sample/2015-04-16/17-30/products.csv` gefunden hat, werden alle Dateien ignoriert, deren codierte Zeit vor 17:30 Uhr am 16. April 2015 liegt. Wenn im gleichen Container also nachträglich das Blob `sample/2015-04-16/17-25/products.csv` erstellt wird, wird es vom Auftrag nicht verwendet.
> 
> Analog dazu gilt: Wenn `sample/2015-04-16/17-30/products.csv` erst am 16. April 2015 um 22:03 Uhr erstellt wird und im Container kein Blob mit einer früheren Zeit enthalten ist, verwendet der Auftrag diese Datei ab dem 16. April 2015, 22:03 Uhr. Bis dahin werden die vorherigen Verweisdaten verwendet.
> 
> Eine Ausnahme besteht darin, wenn der Auftrag Daten rückwirkend verarbeiten muss oder wenn der Auftrag zum ersten Mal gestartet wird. Zum Startzeitpunkt sucht der Auftrag nach dem aktuellsten Blob, das vor der angegebenen Startzeit des Auftrags erstellt wurde. Damit wird sichergestellt, dass beim Starten des Auftrags ein **nicht leeres** Verweisdataset zur Verfügung steht. Sollte keines gefunden werden, erscheint die folgende Diagnose: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

Mit [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) kann das Erstellen der aktualisierten Blobs orchestriert werden, die von Stream Analytics zur Aktualisierung der Verweisdatendefinitionen benötigt werden. Data Factory ist ein cloudbasierter Daten-Integrationsdienst, der das Verschieben und Transformieren von Daten organisiert und automatisiert. Data Factory unterstützt [die Verbindung zu einer großen Anzahl von cloudbasierten und lokalen Datenspeichern](../data-factory/copy-activity-overview.md) und das mühelose Verschieben von Daten in regelmäßigen von Ihnen festgelegten Abständen. Weitere Informationen sowie eine schrittweise Anleitung zum Einrichten einer Data Factory-Pipeline zum Generieren von Verweisdaten für Stream Analytics, die nach einem vordefinierten Zeitplan aktualisiert werden, finden Sie in diesem [GitHub-Beispiel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tipps zum Aktualisieren Ihrer Verweisdaten
1. Das Überschreiben von Verweisdatenblobs hat nicht zur Folge, dass Stream Analytics das Blob erneut lädt, und in einigen Fällen kann es zu einem Auftragsfehler führen. Zum Ändern von Verweisdaten wird empfohlen, ein neues Blob mit demselben in der Auftragseingabe definierten Container und Pfadmuster hinzuzufügen und Datums-/Uhrzeitwerte zu verwenden, die den Wert des letzten Blobs in der Sequenz **übersteigen** .
2. Verweisdatenblobs werden **nicht** nach der Zeit „Zuletzt geändert“ des Blobs sortiert, sondern nur nach den Werten für Uhrzeit und Datum, die im Blobnamen mithilfe der {date}- und {time}-Ersetzungen angegeben werden.
3. Um die Auflistung einer großen Anzahl von Blobs zu vermeiden, sollten Sie darüber nachdenken, sehr alte Blobs zu löschen, für die die Verarbeitung nicht mehr durchgeführt wird. Beachten Sie, dass ASA in einigen Szenarien, wie z.B. einem Neustart, eine kleine Menge nachbearbeiten muss.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

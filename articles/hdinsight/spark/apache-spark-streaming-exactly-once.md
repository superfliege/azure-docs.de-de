---
title: Erstellen von Spark Streaming-Aufträgen mit Ereignisverarbeitung vom Typ „Exactly-Once“ – Azure HDInsight
description: Anleitung zum Einrichten von Spark Streaming zur ausschließlich einmaligen Verarbeitung eines Ereignisses.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/26/2018
ms.openlocfilehash: ae170e90cede26bd6a43fcc10b93fcd7490d838f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618820"
---
# <a name="create-spark-streaming-jobs-with-exactly-once-event-processing"></a>Erstellen von Spark Streaming-Aufträgen mit Ereignisverarbeitung vom Typ „Exactly-Once“

In Anwendungen zur Streamverarbeitung gibt es verschiedene Ansätze zur erneuten Verarbeitung von Nachrichten nach Auftreten eines Fehlers im System:

* Mindestens einmal: Jede Nachricht wird garantiert verarbeitet, aber sie könnte mehrmals verarbeitet werden.
* Höchstens einmal: Jede Nachricht wird verarbeitet oder auch nicht. Wenn eine Nachricht verarbeitet wird, wird sie nur einmal verarbeitet.
* Genau einmal: Jede Nachricht wird garantiert einmal und nur einmal verarbeitet.

Dieser Artikel zeigt das Konfigurieren von Spark Streaming für die genau einmalige Verarbeitung.

## <a name="exactly-once-semantics-with-spark-streaming"></a>Semantik vom Typ „Exactly Once“ mit Spark Streaming

Betrachten Sie zuerst, wie alle Systemfehlerpunkte neu starten, nachdem ein Problem aufgetreten ist, und wie Sie Datenverluste vermeiden können. Eine Spark Streaming-Anwendung hat:

* Eine Eingabequelle
* Mindestens ein Empfängerprozess, der Daten aus der Eingabequelle abruft
* Aufgaben, die die Daten verarbeiten
* Eine Ausgabesenke
* Ein Treiberprozess, der den Auftrag mit langer Ausführungszeit verwaltet

Die „Exactly-Once“-Semantik erfordert, dass keine Daten an irgendeiner Stelle verloren gehen, und dass die Nachrichtenverarbeitung neu gestartet werden kann, unabhängig davon, wo der Fehler auftritt.

### <a name="replayable-sources"></a>Wiedergebbare Quellen

Die Quelle, aus der Ihre Spark Streaming-Anwendung Ihre Ereignisse liest, muss *wiedergebbar* sein. Dies bedeutet, dass in Fällen, in denen die Nachricht abgerufen wurde, jedoch dann ein Fehler im System auftrat, bevor die Nachricht persistent gespeichert oder verarbeitet werden konnte, die Quelle die gleiche Nachricht erneut bereitstellen muss.

In Azure stellen sowohl Azure Event Hubs als auch Kafka auf HDInsight wiedergebbare Quellen bereit. Weitere Beispiele für wiedergebbare Quellen sind ein fehlertolerantes Dateisystem wie HDFS, Azure Storage-Blobs oder Azure Data Lake Store, wo alle Daten unbegrenzt aufbewahrt werden und Sie die Daten zu einem beliebigen Zeitpunkt in ihrer Gesamtheit erneut lesen können.

### <a name="reliable-receivers"></a>Zuverlässige Empfänger

In Spark Streaming haben Datenquellen wie Event Hubs und Kafka *zuverlässige Empfänger*, wobei jeder Empfänger seinen Fortschritt beim Lesen der Quelle verfolgt. Ein zuverlässiger Empfänger speichert seinen Status beständig in einem fehlertoleranten Speicher, entweder in ZooKeeper oder in Spark Streaming-Prüfpunkten, die in HDFS geschrieben werden. Wenn ein solcher Empfänger ausfällt und später neu gestartet wird, kann er den Punkt auswählen, an dem er ausgefallen ist.

### <a name="use-the-write-ahead-log"></a>Verwenden des Write-Ahead-Protokolls

Spark Streaming unterstützt die Verwendung eines Write-Ahead-Protokolls, in dem jedes empfangene Ereignis zunächst in das Prüfpunktverzeichnis von Spark in einem fehlertoleranten Speicher geschrieben und dann in einem Resilient Distributed Dataset (RDD) gespeichert wird. In Azure ist der fehlertolerante Speicher HDFS, gesichert entweder durch Azure Storage oder Azure Data Lake Store. In Ihrer Spark Streaming-Anwendung wird das Write-Ahead-Protokoll durch Festlegen der `spark.streaming.receiver.writeAheadLog.enable`-Konfigurationseinstellung für alle Empfänger auf `true` aktiviert. Das Write-Ahead-Protokoll bietet Fehlertoleranz bei Ausfällen sowohl der Treiber als auch der Executors.

Für Worker, die Aufgaben mit den Ereignisdaten ausführen, wird jede RDD definitionsgemäß auf mehrere Worker sowohl repliziert als auch verteilt. Wenn bei einer Aufgabe ein Fehler auftritt, weil der ausführende Worker abstürzt, wird die Aufgabe auf einem anderen Worker neu gestartet, der über ein Replikat der Ereignisdaten verfügt – daher geht das Ereignis nicht verloren.

### <a name="use-checkpoints-for-drivers"></a>Verwenden von Prüfpunkten für Treiber

Die Auftragstreiber müssen neu gestartet werden können. Wenn der Treiber abstürzt, der Ihre Spark Streaming-Anwendung ausführt, reißt er alle ausgeführten Empfänger, Aufgaben und RDDs, die Ereignisdaten speichern, mit sich. In diesem Fall müssen Sie in der Lage sein, den Fortschritt des Auftrags zu speichern, sodass Sie ihn später fortsetzen können. Hierzu werden in regelmäßigen Abständen Prüfpunkte des gerichteten azyklischen Graphs (Directed Acyclic Graph, DAG) des DStream in einen fehlertoleranten Speicher geschrieben. Die DAG-Metadaten umfassen die Konfiguration, die zum Erstellen der Streaminganwendung verwendet wird, die Vorgänge, die die Anwendung definieren, und alle Batches, die in die Warteschlange gestellt wurden, aber noch nicht abgeschlossen sind. Anhand dieser Metadaten kann ein fehlerhafter Treiber mit den Prüfpunktinformationen neu gestartet werden. Wenn der Treiber neu gestartet wird, startet er neue Empfänger, die selbst die Ereignisdaten aus dem Write-Ahead-Protokoll in RDDs wiederherstellen.

Prüfpunkte werden in Spark Streaming in zwei Schritten aktiviert. 

1. Konfigurieren Sie im StreamingContext-Objekt den Speicherpfad für die Prüfpunkte:

    val ssc = new StreamingContext(spark, Seconds(1))  ssc.checkpoint("/path/to/checkpoints")

    In HDInsight sollten diese Prüfpunkte in Standardspeicher gespeichert werden, der Ihrem Cluster angefügt ist – entweder Azure Storage oder Azure Data Lake Store.

2. Geben Sie als Nächstes ein Prüfpunktintervall (in Sekunden) auf dem DStream an. Bei jedem Intervall werden aus dem Eingabeereignis abgeleitete Zustandsdaten dauerhaft im Speicher abgelegt. Persistente Zustandsdaten können den beim Neuerstellen des Zustands aus dem Quellereignis erforderlichen Rechenaufwand reduzieren.

    val lines = ssc.socketTextStream("hostname", 9999)  lines.checkpoint(30)  ssc.start()  ssc.awaitTermination()

### <a name="use-idempotent-sinks"></a>Verwenden Sie idempotente Senken

Die Zielsenke, in die Ihr Auftrag Ergebnisse schreibt, muss die Situation behandeln können, dass sie das gleiche Ergebnis mehrmals erhält. Die Senke muss solche doppelten Ergebnisse erkennen und ignorieren können. Eine *idempotente* Senke kann mehrere Male mit denselben Daten ohne Änderung des Zustands aufgerufen werden.

Sie können idempotente Senken durch Implementierung von Logik erstellen, die zuerst überprüft, ob das eingehende Ergebnis im Datenspeicher vorhanden ist. Wenn das Ergebnis bereits vorhanden ist, sollte der Schreibvorgang aus der Perspektive des Spark-Auftrags erfolgreich sein, jedoch sollte Ihr Datenspeicher in Wirklichkeit die doppelt vorhandenen Daten ignorieren. Wenn das Ergebnis nicht vorhanden ist, sollte die Senke dieses neue Ergebnis in den Speicher einfügen. 

Sie könnten z.B. eine gespeicherte Prozedur mit Azure SQL-Datenbank verwenden, die Ereignisse in eine Tabelle einfügt. Diese gespeicherte Prozedur sucht zuerst mit den Schlüsselfeldern nach dem Ereignis, und nur dann, wenn kein entsprechendes Ereignis gefunden wird, wird der Datensatz in die Tabelle eingefügt.

Ein weiteres Beispiel ist die Verwendung eines partitionierten Dateisystems wie Azure Storage-Blobs oder Azure Data Lake Store. In diesem Fall muss Ihre Senkenlogik nicht überprüfen, ob die Datei vorhanden ist. Wenn die Datei, die das Ereignis darstellt, vorhanden ist, wird sie einfach mit denselben Daten überschrieben. Andernfalls wird eine neue Datei unter dem berechneten Pfad erstellt.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Spark Streaming](apache-spark-streaming-overview.md)
* [Erstellen von hochverfügbaren Spark Streaming-Aufträgen in YARN](apache-spark-streaming-high-availability.md)

---
title: Verwenden von Apache Pig per Remotedesktop in HDInsight – Azure
description: Hier erfahren Sie, wie Sie den Pig-Befehl verwenden, um Apache Pig Latin-Anweisungen über eine Remotedesktopverbindung mit einem Windows-basierten Apache Hadoop-Cluster in HDInsight auszuführen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 69eeadd8252d20dd25ac6d0abbbf0c6c158383d5
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438624"
---
# <a name="run-apache-pig-jobs-from-a-remote-desktop-connection"></a>Ausführen von Apache Pig-Aufträgen über eine Remotedesktopverbindung
[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält eine exemplarische Vorgehensweise für die Verwendung des Apache Pig-Befehls zur Ausführung von Pig Latin-Anweisungen über eine Remotedesktopverbindung mit einem Windows-basierten HDInsight-Cluster. Mithilfe von Pig Latin können Sie MapReduce-Anwendungen erstellen, indem Sie Datentransformationen beschreiben, anstatt Map- und Reduce-Funktionen zu verwenden.

> [!IMPORTANT]  
> Remotedesktop ist nur in HDInsight-Clustern verfügbar, die als Betriebssystem Windows verwenden. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Für HDInsight 3.4 oder höher finden Sie Informationen zum interaktiven Ausführen von Pig-Aufträgen direkt im Cluster mithilfe einer Befehlszeile unter [Verwenden von Apache Pig mit HDInsight und SSH](apache-hadoop-use-pig-ssh.md).

## <a id="prereq"></a>Voraussetzungen
Damit Sie die in dieser Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen Windows-basierten HDInsight-Cluster (Hadoop in HDInsight)
* Ein Clientcomputer unter Windows 10, Windows 8 oder Windows 7

## <a id="connect"></a>Verbinden mit dem Remotedesktop
Aktivieren Sie Remotedesktop für den HDInsight-Cluster, und stellen Sie anschließend mithilfe der Anweisungen unter [Verbinden mit HDInsight-Clustern über RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)eine Verbindung her.

## <a id="pig"></a>Verwenden des Pig-Befehls
1. Nachdem Sie eine Remotedesktopverbindung hergestellt haben, starten Sie die **Hadoop-Befehlszeile** über das Symbol auf dem Desktop.
2. Starten Sie den Pig-Befehl wie folgt:

        %pig_home%\bin\pig

    Es wird eine `grunt>` -Eingabeaufforderung angezeigt.
3. Geben Sie die folgende Anweisung ein:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Dieser Befehl lädt die Inhalte der Datei "sample.log" in die LOGS-Datei. Sie können den Inhalt der Datei mit dem folgenden Befehl anzeigen.

        DUMP LOGS;
4. Transformieren Sie die Daten durch Anwendung eines regulären Ausdrucks, um nur den Protokolliergrad aus jedem Datensatz zu extrahieren.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Sie können **ABBILDSICHERUNG** zum Anzeigen der Daten nach der Transformation verwenden. In diesem Fall `DUMP LEVELS;`.
5. Fahren Sie mit der Anwendung der Transformationen mithilfe der folgenden Anweisungen fort. Verwenden Sie `DUMP`, um das Ergebnis der Transformation nach jedem Schritt anzuzeigen.

    <table>
    <tr>
    <th>Anweisung</th><th>Funktionsbeschreibung</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</td><td>Entfernt Zeilen mit NULL-Wert für die Protokollebene und speichert die Ergebnisse in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</td><td>Gruppiert die Zeilen nach Protokollebene und speichert die Ergebnisse in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</td><td>Erstellt eine neue Gruppe von Daten, die jeden eindeutigen Protokollebenenwert und die Häufigkeit seines Auftretens enthält. Dies wird in FREQUENCIES gespeichert.</td>
    </tr>
    <tr>
    <td>RESULT = order FREQUENCIES by COUNT desc;</td><td>Ordnet die Protokollebenen nach Anzahl (absteigend) und speichert sie in RESULT.</td>
    </tr>
</table>

6. Sie können die Ergebnisse einer Transformation auch mithilfe der Anweisung `STORE` speichern. Beispiel: Der folgende Befehl speichert `RESULT` im Verzeichnis **/example/data/pigout** des Standardspeichercontainers für Ihren Cluster.

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]  
   > Die Daten werden im angegebenen Verzeichnis in Dateien namens **part-nnnnn**gespeichert. Wenn das Verzeichnis bereits vorhanden ist, erhalten Sie eine Fehlermeldung.
   >
   >
   
7. Geben Sie die folgende Anweisung ein, um die grunt-Eingabeaufforderung zu beenden.

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin-Batchdateien
Den Pig-Befehl können Sie auch zum Ausführen von Pig Latin verwenden, das in einer Datei enthalten ist.

1. Öffnen Sie nach dem Beenden der grunt-Eingabeaufforderung **Editor**, und erstellen Sie eine neue Datei namens **pigbatch.pig** im Verzeichnis **%PIG_HOME%**.
2. Geben oder fügen Sie die folgenden Zeilen in die Datei **pigbatch.pig** ein, und speichern Sie sie dann:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Gehen Sie wie folgt vor, um die Datei **pigbatch.pig** mit dem Pig-Befehl auszuführen.

        pig %PIG_HOME%\pigbatch.pig

    Wenn der Batchauftrag abgeschlossen ist, sollte die folgende Ausgabe angezeigt werden. Diese sollte mit der Ausgabe übereinstimmen, die Sie bei Verwendung von `DUMP RESULT;` in den vorherigen Schritten erhalten haben:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Zusammenfassung
Wie Sie sehen, können Sie mit dem Pig-Befehl interaktiv MapReduce-Vorgänge ausführen oder Pig Latin-Aufträge ausführen, die in einer Batchdatei gespeichert sind.

## <a id="nextsteps"></a>Nächste Schritte
Allgemeine Informationen zu Pig in HDInsight:

* [Verwenden von Apache Pig mit Apache Hadoop in HDInsight](hdinsight-use-pig.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Apache Hive mit Apache Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md)

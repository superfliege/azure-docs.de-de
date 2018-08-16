---
title: Verwenden von Hadoop Pig mit SSH in einem HDInsight-Cluster – Azure
description: Erfahren Sie, wie Sie eine Verbindung zu einem Linux-basierten Hadoop-Cluster mit SSH aufbauen und dann den Pig-Befehl zum Ausführen von Pig Latin-Anweisungen interaktiv oder als Stapelverarbeitungsauftrag verwenden.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.openlocfilehash: c521f5781c1fb8bae1e036649ee31744d0742796
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590295"
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Ausführen von Pig-Jobs auf einem Linux-basierten Cluster mit dem Pig-Befehl (SSH)

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Erfahren Sie, wie Sie Pig-Aufträge interaktiv über eine SSH-Verbindung in Ihrem HDInsight-Cluster ausführen. Mit der Programmiersprache Pig Latin können Sie Transformationen beschreiben, die auf die Eingabedaten angewendet werden, um die gewünschte Ausgabe zu generieren.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen Linux-basierten HDInsight-Cluster. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>Verbinden mit SSH

Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Im folgenden Beispiel stellt das Konto namens **sshuser** eine Verbindung mit einem Cluster namens **myhdinsight** her:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Verwenden des Pig-Befehls

1. Nachdem die Verbindung hergestellt ist, starten Sie die Pig-Befehlszeilenschnittstelle (CLI) mit dem folgenden Befehl:

    ```bash
    pig
    ```

    Nach einem kurzen Moment ändert sich die Eingabeaufforderung in `grunt>`.

2. Geben Sie die folgende Anweisung ein:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    ```

    Dieser Befehl lädt die Inhalte der Datei "sample.log" in LOGS. Sie können den Inhalt der Datei mit der folgenden Anweisung anzeigen:

    ```piglatin
    DUMP LOGS;
    ```

3. Als Nächstes transformieren Sie die Daten durch Anwendung eines regulären Ausdrucks, um nur den Protokolliergrad aus jedem Datensatz unter Verwendung der folgenden Anweisung zu extrahieren:

    ```piglatin
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    ```

    Sie können **ABBILDSICHERUNG** zum Anzeigen der Daten nach der Transformation verwenden. Verwenden Sie in diesem Fall `DUMP LEVELS;`.

4. Fahren Sie mit der Anwendung der Transformationen mithilfe der Anweisungen in der folgenden Tabelle fort:

    | Pig Latin-Anweisung | Wirkung der Anweisung |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Entfernt Zeilen mit NULL-Werten für die Protokollebene und speichert die Ergebnisse in `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Gruppiert die Zeilen nach Protokollebene und speichert die Ergebnisse in `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Erstellt eine Gruppe von Daten, die jeden eindeutigen Protokollebenenwert und die Häufigkeit seines Auftretens enthält. Das Dataset wird unter `FREQUENCIES` gespeichert. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Ordnet die Protokollebenen nach Anzahl (absteigend) und speichert sie in `RESULT`. |

    > [!TIP]
    > Verwenden Sie `DUMP`, um das Ergebnis der Transformation nach jedem Schritt anzuzeigen.

5. Sie können die Ergebnisse einer Transformation auch mithilfe der Anweisung `STORE` speichern. Die folgende Anweisung speichert `RESULT` z.B im Verzeichnis `/example/data/pigout` des Standardspeichers für Ihren Cluster:

    ```piglatin
    STORE RESULT into '/example/data/pigout';
    ```

   > [!NOTE]
   > Die Daten werden im angegebenen Verzeichnis in Dateien namens `part-nnnnn` gespeichert. Wenn das Verzeichnis bereits vorhanden ist, erhalten Sie einen Fehler.

6. Geben Sie die folgende Anweisung ein, um die grunt-Eingabeaufforderung zu beenden:

    ```piglatin
    QUIT;
    ```

### <a name="pig-latin-batch-files"></a>Pig Latin-Batchdateien

Den Pig-Befehl können Sie auch zum Ausführen von Pig Latin verwenden, das in einer Datei enthalten ist.

1. Verwenden Sie nach dem Beenden der Grunt-Eingabeaufforderung den folgenden Befehl, um eine Datei namens `pigbatch.pig` zu erstellen.

    ```bash
    nano ~/pigbatch.pig
    ```

2. Geben oder fügen Sie die folgenden Zeilen ein:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;
    ```

    Drücken Sie dann __STRG__ + __X__, __Y__ und die __EINGABETASTE__, um die Datei zu speichern.

3. Verwenden Sie den folgenden Befehl, um die Datei `pigbatch.pig` mit dem Pig-Befehl auszuführen.

    ```bash
    pig ~/pigbatch.pig
    ```

    Sobald der Batchauftrag abgeschlossen ist, wird die folgende Ausgabe angezeigt:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Pig in HDInsight finden Sie im folgenden Artikel:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Weitere Informationen zu anderen Methoden zur Verwendung von Hadoop in HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

---
title: Verwenden von Apache Hadoop Pig mit SSH in einem HDInsight-Cluster – Azure
description: Hier erfahren Sie, wie Sie eine SSH-Verbindung mit einem Linux-basierten Apache Hadoop-Cluster herstellen und dann mithilfe des Pig-Befehls Pig Latin-Anweisungen ausführen (entweder interaktiv oder als Stapelverarbeitungsauftrag).
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: a674ee52ae5e8f8f800d4584a53c808ceae70156
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435054"
---
# <a name="run-apache-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Ausführen von Apache Pig-Jobs in einem Linux-basierten Cluster mithilfe des Pig-Befehls (SSH)

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Hier erfahren Sie, wie Sie Apache Pig-Aufträge interaktiv über eine SSH-Verbindung in Ihrem HDInsight-Cluster ausführen. Mit der Programmiersprache Pig Latin können Sie Transformationen beschreiben, die auf die Eingabedaten angewendet werden, um die gewünschte Ausgabe zu generieren.

> [!IMPORTANT]  
> Die Schritte in diesem Dokument erfordern einen Linux-basierten HDInsight-Cluster. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>Verbinden mit SSH

Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Im folgenden Beispiel stellt das Konto namens **sshuser** eine Verbindung mit einem Cluster namens **myhdinsight** her:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Verwenden des Apache Pig-Befehls

1. Nachdem die Verbindung hergestellt ist, starten Sie die Pig-Befehlszeilenschnittstelle (CLI) mit dem folgenden Befehl:

    ```bash
    pig
    ```

    Nach einem kurzen Moment ändert sich die Eingabeaufforderung in `grunt>`.

2. Geben Sie die folgende Anweisung ein:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    ```

    Dieser Befehl lädt die Inhalte der Datei „sample.log“ in LOGS. Sie können den Inhalt der Datei mit der folgenden Anweisung anzeigen:

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

* [Verwenden von Apache Pig mit Apache Hadoop in HDInsight](hdinsight-use-pig.md)

Weitere Informationen zu anderen Methoden zur Verwendung von Hadoop in HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von Apache Hive mit Apache Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md)

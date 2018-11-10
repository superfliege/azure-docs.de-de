---
title: Migrieren lokaler Apache Hadoop-Cluster zu Azure HDInsight – Best Practices für die Datenmigration
description: Lernen Sie bewährte Methoden für die Datenmigration zum Migrieren von lokalen Hadoop-Clustern zu Azure HDInsight kennen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 6b06b8eb8d5e18acd3107ec5cccac79fc7be7edc
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418176"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---data-migration-best-practices"></a>Migrieren lokaler Apache Hadoop-Cluster zu Azure HDInsight – Best Practices für die Datenmigration

Dieser Artikel enthält Empfehlungen für die Datenmigration zu Azure HDInsight. Er ist Teil einer Reihe von Artikeln, die bewährte Methoden für die Migration von lokalen Apache Hadoop-Systemen zu Azure HDInsight bieten.

## <a name="migrate-data-from-on-premises-to-azure"></a>Migrieren lokaler Daten zu Azure

Es gibt im Wesentlichen zwei Optionen zum Migrieren von Daten vom lokalen System zur Azure-Umgebung:

1.  Datenübertragung über das Netzwerk mit TLS
    1.  Über das Internet
    2.  ExpressRoute
2.  Datenversand
    1.  Import/Export-Dienst
        - Nur Interne SATA-HDDs oder -SSDs
        - Verschlüsselung im Ruhezustand (AES-128/AES-256)
        - Ein Importauftrag kann bis zu 10 Datenträger umfassen
        - Verfügbar in allen öffentlichen Regionen und allgemeine Verfügbarkeit
    1.  Data Box
        - Bis zu 80 TB an Daten pro Data Box
        - Verschlüsselung im Ruhezustand (AES-256)
        - Verwendet NAS-Protokolle und unterstützt gängige Tools zum Kopieren von Daten
        - Robuste Hardware
        - Nur verfügbar in den USA und in der öffentlichen Vorschau

Die folgende Tabelle zeigt die ungefähre Dauer der Datenübertragung basierend auf der Datenmenge und der Netzwerkbandbreite. Verwenden Sie eine Data Box, wenn die Datenmigration mehr als drei Wochen dauern würde.

|**Datenmenge**|**Netzwerkbandbreite**|||
|---|---|---|---|
|| **45 MBit/s (T3)**|**100 MBit/s**|**1 GBit/s**|**10 GBit/s**
|1 TB|2 Tage|1 Tag| 2 Stunden|14 Minuten|
|10 TB|22 Tage|10 Tage|1 Tag|2 Stunden|
|35 TB|76 Tage|34 Tage|3 Tage|8 Stunden|
|80 TB|173 Tage|78 Tage|8 Tage|19 Stunden|
|100 TB|216 Tage|97 Tage|10 Tage|1 Tag|
|200 TB|1 Jahr|194 Tage|19 Tage|2 Tage|
|500 TB|3 Jahre|1 Jahr|49 Tage|5 Tage|
|1 PB|6 Jahre|3 Jahre|97 Tage|10 Tage|
|2 PB|12 Jahre|5 Jahre|194 Tage|19 Tage|

Native Azure-Tools wie z.B. DistCp, Azure Data Factory und AzureCp können zur Übertragung von Daten über das Netzwerk verwendet werden. Für denselben Zweck kann auch das Drittanbietertool WANDisco verwendet werden. Für die laufende Datenübertragung von einer lokalen Umgebung in Azure-Speichersysteme können Kafka Mirrormaker und Sqoop verwendet werden.

## <a name="performance-considerations-when-using-apache-distcp"></a>Überlegungen zur Leistung bei der Verwendung von Apache DistCp

DistCp ist ein Apache-Projekt, das einen MapReduce-Zuordnungsauftrag verwendet, um Daten zu übertragen, Fehler zu behandeln und nach Auftreten dieser Fehler für eine Wiederherstellung zu sorgen. Jeder Zuordnungsaufgabe wird eine Liste der Quelldateien zugewiesen. Die Zuordnungsaufgabe kopiert anschließend alle ihr zugewiesenen Dateien an das Ziel. Die Leistung von DistCp kann durch verschiedene Techniken verbessert werden.

### <a name="increase-the-number-of-mappers"></a>Erhöhen der Anzahl von Mappern

DistCp versucht, Zuordnungsaufgaben so zu erstellen, dass mit jeder Aufgabe in etwa dieselbe Anzahl von Bytes kopiert wird. Standardmäßig verwenden DistCp-Aufträge 20 Mapper. Durch die Verwendung von mehr Mappern für DistCp (mit dem Parameter „m“ an der Befehlszeile) wird die Parallelität während der Datenübertragung erhöht und die Dauer der Datenübertragung verringert. Beim Erhöhen der Anzahl von Mappern müssen jedoch zwei Aspekte berücksichtigt werden:

1. Die niedrigste Granularität für DistCp ist eine einzelne Datei. Das Angeben einer Anzahl von Mappern, die die Anzahl von Quelldateien übersteigt, hat keinen Nutzen und führt zu einer Vergeudung der verfügbaren Clusterressourcen.
1. Berücksichtigen Sie beim Festlegen der Anzahl von Mappern den verfügbaren Yarn-Arbeitsspeicher für den Cluster. Jede Zuordnungsaufgabe wird als Yarn-Container gestartet. Vorausgesetzt, dass keine anderen ressourcenintensiven Workloads im Cluster ausgeführt werden, kann die Anzahl von Mappern mithilfe der folgenden Formel bestimmt werden: m = (Anzahl von Workerknoten \* YARN-Arbeitsspeicher für jeden Workerknoten)/YARN-Containergröße. Wenn jedoch andere Anwendungen Arbeitsspeicher belegen, verwenden Sie nur einen Teil des YARN-Arbeitsspeichers für DistCp-Aufträge.

### <a name="use-more-than-one-distcp-job"></a>Verwenden mehrerer DistCp-Aufträge

Wenn das zu verschiebende Dataset größer ist als 1 TB, verwenden Sie mehr als einen DistCp-Auftrag. Auf diese Weise verringern Sie die Auswirkung von Fehlern. Bei einem Auftragsfehler müssen Sie nur diesen spezifischen Auftrag neu starten und nicht alle Aufträge.

### <a name="consider-splitting-files"></a>Erwägen einer Dateiaufteilung

Wenn eine kleine Anzahl großer Dateien vorliegt, erwägen Sie eine Aufteilung dieser Dateien in 256-MB-Dateiblöcke, um durch mehr Mapper potenziell die parallele Verarbeitung zu erhöhen.

### <a name="use-the-strategy-command-line-parameter"></a>Verwenden des Befehlszeilenparameters „strategy“

Erwägen Sie die Verwendung des Parameters `strategy = dynamic` in der Befehlszeile. Der Standardwert für den Parameter `strategy` lautet `uniform size`, wodurch mit jeder Zuordnungsaufgabe in etwa die gleiche Anzahl von Bytes kopiert wird. Wenn Sie den Parameterwert in `dynamic` ändern, wird die Datei in mehrere Dateiblöcke aufgeteilt. Die Anzahl von Dateiblöcken ist ein Vielfaches der Anzahl von Zuordnungen. Jede Zuordnungsaufgabe ist einem Dateiblock zugewiesen. Nachdem alle Pfade in einem Block verarbeitet wurden, wird der aktuelle Block gelöscht und ein neuer Block angefordert. Der Prozess wird fortgesetzt, bis keine Blöcke mehr verfügbar sind. Durch diesen „dynamischen“ Ansatz können schnellere Zuordnungsaufgaben mehr Pfade verarbeiten als langsamere Zuordnungen. Dies wiederum führt zu einer schnelleren Verarbeitung des DistCp-Auftrags insgesamt.

### <a name="increase-the-number-of-threads"></a>Erhöhen der Anzahl von Threads

Überprüfen Sie, ob das Erhöhen des Parameters `-numListstatusThreads` die Leistung verbessert. Dieser Parameter steuert die Anzahl von Threads, die zum Erstellen der Dateiauflistung verwendet werden. Der Höchstwert für diesen Parameter lautet 40.

### <a name="use-the-output-committer-algorithm"></a>Verwenden des committer-Algorithmus für die Ausgabe

Überprüfen Sie, ob die Übergabe des Parameters `-Dmapreduce.fileoutputcommitter.algorithm.version=2` die DistCp-Leistung verbessert. Dieser committer-Algorithmus für die Ausgabe umfasst Optimierungen in Bezug auf das Schreiben von Ausgabedateien am Ziel. Der folgende Befehl ist ein Beispiel, das die Nutzung der verschiedenen Parameter veranschaulicht:

```bash
hadoop distcp -Dmapreduce.fileoutputcommitter.algorithm.version=2 -numListstatusThreads 30 -m 100 -strategy dynamic hdfs://nn1:8020/foo/bar wasb://<container_name>@<storage_account_name>.blob.core.windows.net/foo/
```

## <a name="metadata-migration"></a>Migration von Metadaten

### <a name="hive"></a>Hive

Der Hive-Metastore kann entweder mithilfe von Skripts oder unter Verwendung der Datenbankreplikation migriert werden.

#### <a name="hive-metastore-migration-using-scripts"></a>Migration des Hive-Metastores mithilfe von Skripts

1. Generieren Sie die Hive-DDLs aus einem lokalen Hive-Metastore. Dieser Schritt kann mit einem [Wrapper-Bash-Skript] ausgeführt werden. (https://github.com/hdinsight/hdinsight.github.io/blob/master/hive/hive-export-import-metastore.md)
1. Bearbeiten Sie die generierte DDL, um die HDFS-URL durch WASB/ADLS/ABFS-URLs zu ersetzen.
1. Führen Sie die aktualisierte DDL vom HDInsight-Cluster aus für den Metastore aus.
1. Stellen Sie sicher, dass die Versionen des Hive-Metastores in der lokalen Umgebung und der Cloud kompatibel sind.

#### <a name="hive-metastore-migration-using-db-replication"></a>Migration des Hive-Metastores mithilfe der Datenbankreplikation

- Richten Sie die Datenbankreplikation zwischen der lokalen Datenbank für den Hive-Metastore und der HDInsight-Metastore-Datenbank ein.
- Verwenden Sie „Hive MetaTool“, um die HDFS-URL durch WASB/ADLS/ABFS-URLs zu ersetzen. Beispiel:

```bash
./hive --service metatool -updateLocation hdfs://nn1:8020/ wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
```

### <a name="ranger"></a>Ranger

- Exportieren Sie lokale Ranger-Richtlinien in XML-Dateien.
- Transformieren Sie lokale HDFS-basierte Pfade mit einem Tool wie XSLT in WASB/ADLS.
- Importieren Sie die Richtlinien in Ranger unter HDInsight.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den nächsten Artikel in dieser Reihe:

- [Best Practices für Sicherheit und DevOps bei der Migration lokaler Hadoop-Cluster zu Azure HDInsight](apache-hadoop-on-premises-migration-best-practices-security-devops.md)
---
title: Kopieren von Daten in Azure Data Lake Storage Gen2 (Vorschauversion) mithilfe von Distcp | Microsoft-Dokumentation
description: Verwenden des Distcp-Tools zum Kopieren von Daten in und aus Data Lake Storage Gen2 (Vorschauversion)
services: storage
documentationcenter: ''
author: seguler
manager: jahogg
editor: seguler
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: how-to
ms.date: 06/27/2018
ms.author: seguler
ms.openlocfilehash: 2a958ceb0b3a1db9d06d045a8161fa6cd3ef5aba
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059925"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-storage-gen2-preview"></a>Kopieren von Daten zwischen Azure Storage-Blobs und Data Lake Storage Gen2 (Vorschauversion) mithilfe von Distcp

Bei einem HDInsight-Cluster mit Zugriff auf Azure Data Lake Storage Gen2 (Vorschauversion) können Sie Tools aus dem Hadoop-Ökosystem (beispielsweise Distcp) verwenden, um Daten aus einem HDInsight-Clusterspeicher (WASB) in ein Data Lake Storage Gen2-fähiges Konto zu kopieren (**und umgekehrt**). Dieser Artikel enthält Anweisungen zur Verwendung des Distcp-Tools.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Azure-Speicherkonto mit aktivierter Funktion Azure Data Lake Storage (Vorschauversion)**. Anleitungen zum Erstellen eines solchen Kontos finden Sie unter [Erstellen eines Speicherkontos in Azure Data Lake Storage Gen2 (Vorschauversion)](quickstart-create-account.md).
* **Azure HDInsight-Cluster** mit Zugriff auf ein Data Lake Storage-Konto. Siehe [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](use-hdi-cluster.md). Stellen Sie sicher, dass Remotedesktop für den Cluster aktiviert ist.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Verwenden von Distcp eines HDInsight-Linux-Clusters

Ein HDInsight-Cluster enthält das Distcp-Dienstprogramm, das zum Kopieren von Daten aus verschiedenen Quellen in einen HDInsight-Cluster verwendet werden kann. Wenn Sie den HDInsight-Cluster so konfiguriert haben, dass er Azure Blob Storage und Azure Data Lake Storage zusammen verwendet, kann das Distcp-Dienstprogramm ebenfalls direkt zum Kopieren von Daten zwischen diesen genutzt werden. In diesem Abschnitt wird die Verwendung des Distcp-Dienstprogramms erläutert.

1. Verwenden Sie SSH auf Ihrem Desktop, um eine Verbindung mit dem Cluster herzustellen. Informationen hierzu finden Sie unter [Verbinden mit einem Linux-basierten HDInsight-Cluster](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Führen Sie die Befehle von der SSH-Eingabeaufforderung aus.

2. Überprüfen Sie, ob Sie auf die Azure Storage-Blobs (WASB) zugreifen können. Führen Sie den folgenden Befehl aus:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Die Ausgabe sollte eine Liste der Inhalte im Speicherblob enthalten.

3. Überprüfen Sie zudem, ob Sie vom Cluster auf das Data Lake Storage-Konto zugreifen können. Führen Sie den folgenden Befehl aus:

        hdfs dfs -ls abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/

    Die Ausgabe sollte eine Liste der Dateien bzw. Ordner im Data Lake Storage-Konto bereitstellen.

4. Verwenden Sie Distcp zum Kopieren von Daten aus WASB in ein Data Lake Storage-Konto.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder

    Der Befehl kopiert den Inhalt des Ordners **/example/data/gutenberg/** im Blob-Speicher in den Ordner **/myfolder** im Data Lake Storage-Konto.

5. Verwenden Sie Distcp ebenso zum Kopieren von Daten aus einem Data Lake Storage-Konto in den Blob-Speicher (WASB).

        hadoop distcp abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Der Befehl kopiert den Inhalt des Ordners **/myfolder** im Data Lake Store-Konto in den Ordner **/example/data/gutenberg/** in WASB.

## <a name="performance-considerations-while-using-distcp"></a>Überlegungen zur Leistung bei der Verwendung von DistCp

Da die kleinste Granularitätseinheit von DistCp eine einzelne Datei ist, ist die maximale Anzahl gleichzeitiger Kopien der wichtigste Parameter für die Data Lake Storage-Optimierung. Um die Anzahl gleichzeitiger Kopien zu steuern, wird der Parameter für die Anzahl von Zuordnungen (m) über die Befehlszeile festgelegt. Dieser Parameter gibt die maximale Anzahl von Zuordnungen an, die zum Kopieren von Daten verwendet werden. Der Standardwert ist 20.

**Beispiel**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Wie bestimme ich die Anzahl der zu verwendenden Zuordnungen?

Hier sind einige hilfreiche Informationen zur Vorgehensweise angegeben.

* **Schritt 1: Bestimmen des YARN-Gesamtarbeitsspeichers** – Zunächst muss der YARN-Arbeitsspeicher bestimmt werden, der für den Cluster zur Verfügung steht, in dem Sie den DistCp-Auftrag ausführen. Diese Information finden Sie im dem Cluster zugeordneten Ambari-Portal. Navigieren Sie zu YARN, und zeigen Sie die Konfigurationsregisterkarte an, um den YARN-Arbeitsspeicher zu ermitteln. Um den YARN-Gesamtarbeitsspeicher zu erhalten, multiplizieren Sie den YARN-Arbeitsspeicher pro Knoten mit der Anzahl von Knoten in Ihrem Cluster.

* **Schritt 2: Berechnen der Anzahl von Zuordnungen** – Der Wert von **m** entspricht dem Quotienten des YARN-Gesamtarbeitsspeichers dividiert durch die YARN-Containergröße. Die YARN-Containergröße finden Sie ebenfalls im Ambari-Portal. Navigieren Sie zu YARN, zeigen Sie die Konfigurationsregisterkarte an. Die YARN-Containergröße wird in diesem Fenster angezeigt. Die Formel zur Berechnung der Anzahl von Zuordnungen (**m**) sieht wie folgt aus:

        m = (number of nodes * YARN memory for each node) / YARN container size

**Beispiel**

Angenommen, Sie verfügen über einen Cluster mit vier D14v2-Knoten und möchten 10 TB an Daten aus zehn unterschiedlichen Ordnern übertragen. Jeder Ordner enthält unterschiedliche Datenmengen, und auch die Dateigrößen in den einzelnen Ordnern sind unterschiedlich.

* YARN-Gesamtarbeitsspeicher: Über das Ambari-Portal ermitteln Sie, dass der YARN-Arbeitsspeicher für einen D14-Knoten 96 GB beträgt. Für einen Cluster mit vier Knoten beträgt der YARN-Gesamtarbeitsspeicher demnach: 

        YARN memory = 4 * 96GB = 384GB

* Anzahl von Zuordnungen: Über das Ambari-Portal ermitteln Sie, dass die YARN-Containergröße für einen D14-Clusterknoten 3072 beträgt. Die Anzahl von Zuordnungen beträgt somit:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Wenn andere Anwendungen Arbeitsspeicher verwenden, können Sie festlegen, dass für DistCp nur ein Teil des YARN-Arbeitsspeichers Ihres Clusters verwendet werden soll.

### <a name="copying-large-datasets"></a>Kopieren umfangreicher Datasets

Wenn das zu verschiebende Dataset sehr groß ist (beispielsweise > 1 TB) oder Sie über zahlreiche unterschiedliche Ordner verfügen, empfiehlt sich unter Umständen die Verwendung mehrerer DistCp-Aufträge. Dadurch ergibt sich zwar wahrscheinlich kein Leistungsgewinn, die Aufträge werden jedoch verteilt, sodass im Falle eines Fehlers bei einem Auftrag nicht das gesamte Projekt, sondern lediglich der betroffene Auftrag neu gestartet werden muss.

### <a name="limitations"></a>Einschränkungen

* Zur Optimierung der Leistung versucht DistCp, Zuordnungen mit ähnlicher Größe zu erstellen. Eine höhere Anzahl von Zuordnungen führt nicht unbedingt zu mehr Leistung.

* DistCp ist auf eine einzelne Zuordnung pro Datei beschränkt. Aus diesem Grund sollte die Anzahl von Zuordnungen die Anzahl von Dateien nicht überschreiten. Der Umstand, dass DistCp einer Datei nur eine einzelne Zuordnung zuweisen kann, beschränkt den Umfang der Parallelität, die beim Kopieren großer Dateien verwendet werden kann.

* Wenn Sie über eine geringe Anzahl großer Dateien verfügen, sollten Sie diese in Dateiblöcke mit jeweils 256 MB aufteilen, um das Parallelitätspotenzial zu erhöhen. 

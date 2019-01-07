---
title: Datenszenarien mit Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Hier finden Sie grundlegende Informationen zu den verschiedenen Szenarien und Tools, in bzw. mit denen Daten in Data Lake Storage Gen2 (ehemals Azure Data Lake Store) erfasst, verarbeitet, heruntergeladen und visualisiert werden können.
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.openlocfilehash: 978f86141d72cc7be43f24909f9780ab9570605d
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52976314"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Verwenden von Azure Data Lake Storage Gen2 für Big Data-Anforderungen

Es gibt vier wichtige Phasen in der Big Data-Verarbeitung:

* Erfassung von großen Datenmengen in einem Datenspeicher, in Echtzeit oder in Batches
* Verarbeiten der Daten
* Herunterladen der Daten
* Visualisieren der Daten

In diesem Artikel betrachten wir diese Phasen in Bezug auf Azure Data Lake Storage Gen2, um die Optionen und Tools kennenzulernen, die zur Erfüllung Ihrer Big Data-Anforderungen verfügbar sind.

## <a name="ingest-data-into-data-lake-storage-gen2"></a>Erfassen von Daten in Data Lake Storage Gen2
In diesem Abschnitt werden die unterschiedlichen Quellen von Daten sowie die verschiedenen Methoden hervorgehoben, mit denen Daten in einem Data Lake Storage Gen2-Konto erfasst werden können.

![Erfassen von Daten in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/ingest-data.png "Erfassen von Daten in Data Lake Storage Gen2")

### <a name="ad-hoc-data"></a>Ad-hoc-Daten:
Dies steht für kleinere Datasets, die zum Erstellen von Prototypen einer Big Data-Anwendung verwendet werden. Es gibt, abhängig von der Quelle der Daten, verschiedene Möglichkeiten zum Erfassen von Ad-hoc-Daten.

| Data source | Erfassen Sie mit |
| --- | --- |
| Lokalem Computer |<ul> <li>[Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/)</ul> |
| Azure Storage-Blob |<ul> <li>[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)</li> <li>[Das Tool AzCopy](../common/storage-use-azcopy-v10.md)</li><li>[Ausführung von DistCp auf einem HDInsight-Cluster](data-lake-storage-use-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Streamingdaten
Diese Daten können von verschiedenen Quellen wie Anwendungen, Geräten, Sensoren etc. generiert werden. Diese Daten können in Data Lake Storage Gen2 von verschiedenen Tools erfasst werden. Diese Tools erfassen und verarbeiten die Daten in der Regel ereignisbezogen in Echtzeit und schreiben dann die Ereignisse batchweise in Data Lake Storage Gen2, damit sie weiterverarbeitet werden können.

Folgende Tools können Sie verwenden:

* [Azure HDInsight Storm](../../hdinsight/storm/apache-storm-write-data-lake-store.md): Daten aus dem Storm-Cluster können direkt in Data Lake Storage Gen2 geschrieben werden.

### <a name="relational-data"></a>Relationale Daten
Sie können auch Daten aus relationalen Datenbanken erfassen. Relationale Datenbanken sammeln über einen Zeitraum hinweg umfangreiche Datenmengen, die bei Verarbeitung durch eine Big Data-Pipeline wichtige Einblicke bieten können. Mit den folgenden Tools können Sie solche Daten in Data Lake Storage Gen2 verschieben.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Webserverprotokolldaten (mit benutzerdefinierten Anwendungen hochladen)
Auf diesen Typ von Dataset wird besonders hingewiesen, da die Analyse von Webserverprotokolldaten eine gängige Praxis für Big Data-Anwendungen ist und voraussetzt, dass zahlreiche Protokolldateien in Data Lake Storage Gen2 hochgeladen werden. Sie können jedes der folgenden Tools nutzen, um eigene Skripts oder Anwendungen zum Hochladen solcher Daten zu schreiben.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)

Für das Hochladen von Webserverprotokolldaten und auch anderer Arten von Daten (z. B. soziale Stellungnahmen) ist es ein guter Ansatz, wenn Sie Ihre eigenen benutzerdefinierten Skripts/Anwendungen schreiben, denn dies gibt Ihnen die Flexibilität, Ihre Datenhochladekomponente als Teil in Ihre Big Data-Anwendung einzubeziehen. In einigen Fällen kann dieser Code die Form eines Skripts oder eines einfachen Befehlszeilenprogramms annehmen. In anderen Fällen kann der Code verwendet werden, die Big Data-Verarbeitung in eine Geschäftsanwendung oder -lösung zu integrieren.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Mit Azure HDInsight-Clustern verknüpfte Daten 
Die meisten HDInsight-Clustertypen (Hadoop, HBase, Storm) unterstützen Data Lake Storage Gen2 als Datenspeicherrepository. HDInsight-Cluster greifen aus Azure Storage-Blobs (WASB) auf Daten zu. Zur Verbesserung der Leistung können Sie die Daten aus WASB in ein mit dem Cluster verknüpftes Data Lake Storage Gen2-Konto kopieren. Mit den folgenden Tools können Sie die Daten kopieren.

* [Apache DistCp](data-lake-storage-use-distcp.md)
* [Das Tool AzCopy](../common/storage-use-azcopy-v10.md)
* [Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>In lokalen oder IaaS Hadoop-Clustern gespeicherte Daten
Große Datenmengen können in vorhandenen Hadoop-Clustern oder lokal auf Computern gespeichert werden, die HDFS verwenden. Die Hadoop-Cluster können sich in einer lokalen Bereitstellung oder innerhalb eines IaaS-Clusters in Azure befinden. Möglicherweise bestehen Anforderungen für das einmalige oder regelmäßige Kopieren solcher Daten in Azure Data Lake Storage Gen2. Hierfür stehen verschiedene Möglichkeiten zur Verfügung. Im Folgenden finden Sie eine Liste der Alternativen und die jeweiligen Vor- und Nachteile.

| Vorgehensweise | Details | Vorteile | Überlegungen |
| --- | --- | --- | --- |
| Verwenden von Azure Data Factory (ADF), um Daten direkt aus Hadoop-Clustern in Azure Data Lake Storage Gen2 zu kopieren |[ADF unterstützt HDFS als Datenquelle.](../../data-factory/connector-hdfs.md) |ADF bietet sofortige Unterstützung für HDFS und erstklassige End-to-End-Verwaltung und -Überwachung. |Ein Datenverwaltungsgateway muss lokal oder im IaaS-Cluster bereitgestellt werden. |
| Verwenden Sie Distcp, um Daten von Hadoop in Azure Storage zu kopieren. Kopieren Sie die Daten dann mit einer geeigneten Methode aus Azure Storage in Data Lake Storage Gen2. |Sie können Daten mithilfe der folgenden Komponenten aus Azure Storage in Data Lake Storage Gen2 kopieren: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[Das Tool AzCopy](../common/storage-use-azcopy-v10.md)</li><li>[Ausführung von Apache DistCp auf HDInsight-Clustern](data-lake-storage-use-distcp.md)</li></ul> |Sie können Open Source-Tools verwenden. |Der Prozess erfordert mehrere Schritte und verschiedene Technologien. |

### <a name="really-large-datasets"></a>Sehr große Datasets
Das Hochladen von Datasets im Bereich mehrerer Terabyte kann mithilfe der oben beschriebenen Methoden manchmal langsam und kostspielig sein. In solchen Fällen können Sie die folgenden Optionen verwenden:

* **Verwenden von Azure ExpressRoute**. Azure ExpressRoute ermöglicht Ihnen, private Verbindungen zwischen Azure-Rechenzentren und Ihrer lokalen Infrastruktur zu erstellen. Dies ist eine zuverlässige Option zur Übertragung großer Datenmengen. Weitere Informationen finden Sie in der [Dokumentation zu Azure ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="process-data-stored-in-data-lake-storage-gen2"></a>Verarbeiten von in Data Lake Storage Gen2 gespeicherten Daten
Sobald die Daten in Data Lake Storage Gen2 verfügbar sind, können Sie mit den unterstützten Big Data-Anwendungen Analysen für diese Daten ausführen. Derzeit können Sie mit Azure HDInsight und Azure Databricks Datenanalyseaufträge für die in Data Lake Storage Gen2 gespeicherten Daten ausführen.

![Analysieren von Daten in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/analyze-data.png "Analysieren von Daten in Data Lake Storage Gen2")


## <a name="download-data-from-data-lake-storage-gen2"></a>Herunterladen von Daten aus Data Lake Storage Gen2
Ggf. möchten Sie auch für Szenarien wie die folgenden Daten aus Azure Data Lake Storage Gen2 herunterladen oder verschieben:

* Verschieben von Daten aus anderen Repositorys zur Verbindung mit Ihren vorhandenen Datenverarbeitungspipelines. Sie möchten z. B. Daten aus Data Lake Storage Gen2 in die Azure SQL-Datenbank oder auf eine lokale SQL Server-Instanz verschieben.
* Herunterladen von Daten auf Ihren lokalen Computer für die Verarbeitung in IDE-Umgebungen beim Erstellen von Anwendungsprototypen.

![Ausgeben von Daten aus Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/egress-data.png "Ausgeben von Daten aus Data Lake Storage Gen2")

In solchen Fällen können Sie eine der folgenden Optionen verwenden:

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-storage-use-distcp.md)

## <a name="visualize-data-in-data-lake-storage-gen2"></a>Visualisieren von Daten in Data Lake Storage Gen2
Sie können eine Kombination aus Diensten verwenden, um visuelle Darstellungen von in Data Lake Storage Gen2 gespeicherten Daten zu erstellen.

![Visualisieren von Daten in Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/visualize-data.png "Visualisieren von Daten in Data Lake Storage Gen2")

* Sie können beginnen, indem Sie [Daten mithilfe von Azure Data Factory aus Data Lake Storage Gen2 nach Azure SQL Data Warehouse verschieben](../../data-factory/copy-activity-overview.md)
* Danach können Sie [Power BI in Azure SQL Data Warehouse integrieren](../../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) , um eine visuelle Darstellung der Daten zu erstellen.

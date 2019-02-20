---
title: Integrieren von Azure Data Lake Storage Gen2 in andere Azure-Dienste | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Data Lake Storage Gen2 in andere Azure-Dienste integrieren.
documentationcenter: ''
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: nitinme
ms.openlocfilehash: 7bc4889403e1d52cfa3b18792a554f0faf605132
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885518"
---
# <a name="integrate-azure-data-lake-storage-gen2-with-other-azure-services"></a>Integrieren von Azure Data Lake Storage Gen2 in andere Azure-Dienste

Sie können Daten mithilfe der Azure-Dienste in Ihrem Azure Data Lake Storage Gen2-Speicherkonto erfassen, analysieren und visualisieren. Wählen Sie die Dienste aus, die für Ihre auszuführenden Aufgaben am besten geeignet sind.

## <a name="ingest-data-into-your-data-lake"></a>Erfassen von Daten in Ihrem Data Lake

Diese Dienste helfen Ihnen beim Auffüllen Ihres Data Lakes mit Daten.

### <a name="azure-data-factory"></a>Azure Data Factory

Sie können mithilfe von [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) Daten aus folgenden Quellen erfassen:

* Azure-Tabellen
* Azure SQL-Datenbanken
* Azure SQL Data Warehouse
* Azure Storage-Blobs
* Lokale Datenbanken

Siehe [Verschieben von Daten in und aus Data Lake Storage Gen2 mithilfe von Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md).

## <a name="analyze-and-visualize-data-in-your-data-lake"></a>Analysieren und Visualisieren von Daten in Ihrem Data Lake

Diese Dienste können Data Lake Storage Gen2 als Speicherendpunkt verwenden.

### <a name="azure-hdinsight"></a>Azure HDInsight

Sie können einen [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)-Cluster bereitstellen, der Data Lake Storage Gen2 als HDFS-kompatiblen Speicher verwendet. In dieser Version können Sie für Hadoop- und Storm-Cluster unter Windows und Linux Data Lake Storage Gen2 nur als Zusatzspeicher verwenden. Solche Cluster verwenden weiterhin Azure Storage (WASB) als Standardspeicher. Für HBase-Cluster unter Windows und Linux jedoch können Sie Data Lake Storage Gen2 als Standardspeicher und als Zusatzspeicher verwenden.

Siehe [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2).

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

Sie können [Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-overview.md) zum Arbeiten mit Big Data in der Cloud verwenden. Profitieren Sie von einer dynamischen Ressourcenbereitstellung und der Möglichkeit, Daten im Exabytebereich zu analysieren. Data Lake Analytics ist optimiert, damit Data Lake Storage Gen2 als Datenquelle verwendet werden kann. 

Siehe [Erste Schritte mit Data Lake Analytics mithilfe von Data Lake Storage Gen2](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="copy-data-to-other-repositories"></a>Kopieren von Daten in andere Repositorys

Verwenden Sie diese Dienste, um Daten aus Ihrem Data Lake zu kopieren und in anderen Repositorys abzulegen.

### <a name="sql-data-warehouse"></a>SQL Data Warehouse

Sie können PolyBase verwenden, um Daten aus Data Lake Storage Gen2 in SQL Data Warehouse zu laden. Weitere Informationen finden Sie unter [Laden von Daten aus Data Lake Storage Gen2 in SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über Azure Data Lake Storage Gen2](data-lake-storage-introduction.md)
* [Verwenden von Azure Data Lake Storage Gen2 für Big Data-Anforderungen](data-lake-storage-data-scenarios.md)

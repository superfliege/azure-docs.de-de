---
title: "Transformieren von Daten mit der Aktivität „Gespeicherte Prozedur“ in Azure Data Factory | Microsoft-Dokumentation"
description: "Informationen, wie Sie die SQL Server-Aktivität „Gespeicherte Prozedur“ in einer Data Factory-Pipeline zum Aufrufen einer gespeicherten Prozedur in einer Azure SQL-Datenbank-Instanz oder Azure SQL Data Warehouse-Instanz verwenden können."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: shengc
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: f13f8aa0ca8686c0582bed77d047c9e6b39f7aa2
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---


# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory"></a>Transformieren von Daten mit der SQL Server-Aktivität „Gespeicherte Prozedur“ in Azure Data Factory | Microsoft-Dokumentation
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-stored-proc-activity.md)
> * [Version 2: Vorschauversion](transform-data-using-stored-procedure.md)


Sie verwenden Transformationsaktivitäten in einer Data Factory-[Pipeline](concepts-pipelines-activities.md), um Rohdaten in Vorhersagen und Erkenntnisse umzuwandeln und zu verarbeiten. Die Aktivität der „Gespeicherte Prozedur“ ist eine der Transformationsaktivitäten, die Data Factory unterstützt. Dieser Artikel baut auf dem Artikel [Transformieren von Daten](transform-data.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten in Data Factory bietet.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, lesen Sie [Aktivität „Gespeicherte Prozedur“ in V1](v1/data-factory-stored-proc-activity.md).
> 
> Wenn Sie noch nicht mit Azure Data Factory vertraut sind, lesen Sie zunächst den Artikel [Einführung in Azure Data Factory](introduction.md), und durchlaufen Sie anschließend das Tutorial [Transformieren von Daten](tutorial-transform-data-spark-powershell.md), bevor Sie diesen Artikel lesen. 

Sie können die Aktivität „Gespeicherte Prozedur“ verwenden, um eine gespeicherte Prozedur in einem der folgenden Datenspeicher in Ihrem Unternehmen oder auf einem virtuellen Azure-Computer (VM) aufzurufen: 

- Azure SQL-Datenbank
- Azure SQL Data Warehouse
- SQL Server-Datenbank.  Wenn Sie SQL Server verwenden, müssen Sie die selbstgehostete Integration Runtime auf dem Computer installieren, der die Datenbank hostet, oder auf einem separaten Computer, der Zugriff auf die Datenbank hat. Die selbstgehostete Integration Runtime ist eine Komponente, die lokale Datenquellen bzw. Datenquellen auf virtuellen Azure Computern mit Clouddiensten auf sichere und verwaltete Weise verbindet. Im Artikel [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) finden Sie Details.

> [!IMPORTANT]
> Beim Kopieren von Daten nach Azure SQL-Datenbank oder SQL Server können Sie **SqlSink** in der Kopieraktivität für das Aufrufen einer gespeicherten Prozedur mit der **sqlWriterStoredProcedureName**-Eigenschaft konfigurieren. Ausführliche Informationen zur Eigenschaft finden Sie in den folgenden Artikeln zu Connectors: [Azure SQL-Datenbank](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md). Das Aufrufen einer gespeicherten Prozedur während des Kopierens von Daten in ein Azure SQL Data Warehouse mithilfe einer Kopieraktivität wird nicht unterstützt. Sie können jedoch mithilfe der Aktivität „Gespeicherte Prozedur“ eine gespeicherte Prozedur in einem SQL Data Warehouse aufrufen. 
>
> Beim Kopieren von Daten aus Azure SQL-Datenbank, SQL Server oder Azure SQL Data Warehouse können Sie **SqlSource** in der Kopieraktivität für das Aufrufen einer gespeicherten Prozedur zum Lesen von Daten aus der Quelldatenbank mit der **sqlReaderStoredProcedureName**-Eigenschaft konfigurieren. Weitere Informationen finden Sie in den folgenden Artikeln zu Connectors: [Azure SQL-Datenbank](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md), [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md).          

 

## <a name="syntax-details"></a>Syntaxdetails
So sieht das JSON-Format zum Definieren der Aktivität „Gespeicherte Prozedur“ aus:

```json
{
    "name": "Stored Procedure Activity",
    "description":"Description",
    "type": "SqlServerStoredProcedure",
    "linkedServiceName": {
        "referenceName": "AzureSqlLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": {
            "identifier": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" }

        }
    }
}
```

In der folgenden Tabelle werden diese JSON-Eigenschaften beschrieben:

| Eigenschaft                  | Beschreibung                              | Erforderlich |
| ------------------------- | ---------------------------------------- | -------- |
| Name                      | Der Name der Aktivität                     | Ja      |
| Beschreibung               | Ein Text, der beschreibt, wofür die Aktivität verwendet wird. | Nein       |
| Typ                      | Für die Aktivität „Gespeicherte Prozedur“ ist der Aktivitätstyp „SqlServerStoredProcedure“. | Ja      |
| linkedServiceName         | Verweis auf die Azure SQL-Datenbank-, Azure SQL Data Warehouse- oder SQL Server-Instanz, die in Data Factory als ein verknüpfter Dienst registriert ist. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md). | Ja      |
| storedProcedureName       | Geben Sie den Namen der gespeicherten Prozedur in der Azure SQL-Datenbank oder dem Azure SQL Data Warehouse oder der SQL Server-Datenbank an, die bzw. das vom verknüpften Dienst dargestellt wird, den die Ausgabetabelle verwendet. | Ja      |
| storedProcedureParameters | Geben Sie Werte für Parameter der gespeicherten Prozedur an. Verwenden Sie `"param1": { "value": "param1Value","type":"param1Type" }` zum Übergeben von Parameterwerten und deren nativen Typ, der von der Datenquelle unterstützt wird. Wenn Sie für einen Parameter „null“ übergeben müssen, verwenden Sie die folgende Syntax: *"param1": { "Wert": null }* (nur Kleinbuchstaben). | Nein       |

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden: 

* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchausführungsaktivität](transform-data-using-machine-learning.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)


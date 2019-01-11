---
title: Transformieren von Daten mit der Aktivität „Gespeicherte Prozedur“ in Azure Data Factory | Microsoft-Dokumentation
description: Informationen, wie Sie die SQL Server-Aktivität „Gespeicherte Prozedur“ in einer Data Factory-Pipeline zum Aufrufen einer gespeicherten Prozedur in einer Azure SQL-Datenbank-Instanz oder Azure SQL Data Warehouse-Instanz verwenden können.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: douglasl
ms.openlocfilehash: 54d0ce39ea511958824acb753bcf7102d33a6c90
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444027"
---
# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory"></a>Transformieren von Daten mit der SQL Server-Aktivität „Gespeicherte Prozedur“ in Azure Data Factory | Microsoft-Dokumentation
> [!div class="op_single_selector" title1="Wählen Sie die Version des Data Factory-Dienstes aus, den Sie verwenden:"]
> * [Version 1](v1/data-factory-stored-proc-activity.md)
> * [Aktuelle Version](transform-data-using-stored-procedure.md)

Sie verwenden Transformationsaktivitäten in einer Data Factory-[Pipeline](concepts-pipelines-activities.md), um Rohdaten in Vorhersagen und Erkenntnisse umzuwandeln und zu verarbeiten. Die Aktivität der „Gespeicherte Prozedur“ ist eine der Transformationsaktivitäten, die Data Factory unterstützt. Dieser Artikel baut auf dem Artikel [Transformieren von Daten](transform-data.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten in Data Factory bietet.

> [!NOTE]
> Wenn Sie noch nicht mit Azure Data Factory vertraut sind, lesen Sie zunächst den Artikel [Einführung in Azure Data Factory](introduction.md), und durchlaufen Sie anschließend das Tutorial [Transformieren von Daten](tutorial-transform-data-spark-powershell.md), bevor Sie diesen Artikel lesen. 

Sie können die Aktivität „Gespeicherte Prozedur“ verwenden, um eine gespeicherte Prozedur in einem der folgenden Datenspeicher in Ihrem Unternehmen oder auf einem virtuellen Azure-Computer (VM) aufzurufen: 

- Azure SQL-Datenbank
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

| Eigenschaft                  | BESCHREIBUNG                              | Erforderlich |
| ------------------------- | ---------------------------------------- | -------- |
| name                      | Der Name der Aktivität                     | JA      |
| Beschreibung               | Ein Text, der beschreibt, wofür die Aktivität verwendet wird. | Nein        |
| type                      | Für die Aktivität „Gespeicherte Prozedur“ lautet der Aktivitätstyp **SqlServerStoredProcedure**. | JA      |
| linkedServiceName         | Verweis auf die **Azure SQL-Datenbank**-, **Azure SQL Data Warehouse**- oder **SQL Server**-Instanz, die in Data Factory als ein verknüpfter Dienst registriert ist. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md). | JA      |
| storedProcedureName       | Geben Sie den Namen der gespeicherten Prozedur an, die aufgerufen werden soll. | JA      |
| storedProcedureParameters | Geben Sie die Werte für Parameter der gespeicherten Prozedur an. Verwenden Sie `"param1": { "value": "param1Value","type":"param1Type" }` zum Übergeben von Parameterwerten und deren Typ, der von der Datenquelle unterstützt wird. Wenn Sie für einen Parameter „null“ übergeben müssen, verwenden Sie die folgende Syntax: `"param1": { "value": null }` (nur Kleinbuchstaben). | Nein        |

## <a name="error-info"></a>Fehlerinformationen

Wenn bei einer gespeicherten Prozedur ein Fehler auftritt und Fehlerdetails zurückgegeben werden, können Sie die Fehlerinformationen nicht direkt in der Aktivitätsausgabe erfassen. Data Factory überträgt jedoch alle Ereignisse zur Aktivitätsausführung zu Azure Monitor. Außerdem gibt Data Factory Fehlerdetails in Azure Monitor aus. Sie können beispielsweise E-Mail-Benachrichtigungen zu diesen Ereignissen einrichten. Weitere Informationen finden Sie unter [Benachrichtigen und Überwachen von Data Factorys mithilfe von Azure Monitor](monitor-using-azure-monitor.md).

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

---
title: Lookup-Aktivität in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie die Lookupaktivität verwendet wird, um einen Wert in einer externen Quelle zu suchen. Auf die Ausgabe kann durch nachfolgende Aktivitäten verwiesen werden.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: f55e85bb424f4f5973fd6d633b6adf9fbca4d0ef
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="lookup-activity-in-azure-data-factory"></a>Lookup-Aktivität in Azure Data Factory
Mit der Lookupaktivität können Sie einen Datensatz, einen Tabellennamen oder einen Wert in einer externen Quelle lesen oder suchen. Auf die Ausgabe kann durch nachfolgende Aktivitäten verwiesen werden. 

Die Lookupaktivität ist nützlich, wenn Sie eine Liste von Dateien, Datensätzen oder Tabellen dynamisch aus einer Konfigurationsdatei oder einer Datenquelle abrufen möchten. Die Ausgabe der Aktivität kann von anderen Aktivitäten weiter verwendet werden, um eine bestimmte Verarbeitung nur für diese Elemente auszuführen.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Azure Data Factory, die sich derzeit in der Vorschauphase befindet. Wenn Sie die allgemein verfügbare Version 1 (GA) des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Die folgenden Datenquellen werden derzeit für die Lookupaktivität unterstützt:
- JSON-Datei in Azure Blob Storage
- JSON-Datei in Dateisystem
- Azure SQL-Datenbank (aus einer Abfrage konvertierte JSON-Daten)
- Azure SQL Data Warehouse (aus einer Abfrage konvertierte JSON-Daten)
- SQL Server (aus einer Abfrage konvertierte JSON-Daten)
- Azure Table Storage (aus einer Abfrage konvertierte JSON-Daten)

Von der Lookup-Aktivität werden maximal **5.000** Zeilen mit einer Größe von bis zu **10 MB** zurückgegeben.

## <a name="syntax"></a>Syntax

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## <a name="type-properties"></a>Typeigenschaften
NAME | BESCHREIBUNG | Typ | Erforderlich?
---- | ----------- | ---- | --------
dataset | Enthält die Datasetreferenz für die Lookupaktivität. Derzeit werden folgende Datasettypen unterstützt:<ul><li>`AzureBlobDataset` für [Azure Blob Storage](connector-azure-blob-storage.md#dataset-properties) als Quelle</li><li>`FileShareDataset` für [Dateisystem](connector-file-system.md#dataset-properties) als Quelle</li><li>`AzureSqlTableDataset` für [Azure SQL-Datenbank](connector-azure-sql-database.md#dataset-properties) oder [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#dataset-properties) als Quelle</li><li>`SqlServerTable` für [SQL Server](connector-sql-server.md#dataset-properties) als Quelle</li><li>`AzureTableDataset` für [Azure Table Storage](connector-azure-table-storage.md#dataset-properties) als Quelle</li> | Schlüssel-Wert-Paar | Ja
Quelle | Enthält spezifische Quelleneigenschaften für das Dataset, identisch mit der Quelle der Kopieraktivität. Details finden Sie in jedem entsprechenden Connectorartikel im Abschnitt „Eigenschaften der Kopieraktivität“. | Schlüssel-Wert-Paar | Ja
firstRowOnly | Gibt an, ob nur die erste Zeile oder alle Zeilen zurückgegeben werden sollen. | Boolescher Wert | Nein. Der Standardwert ist `true`.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Verwenden des Ergebnisses der Lookupaktivität in einer nachfolgenden Aktivität

Das Lookupergebnis wird im Abschnitt `output` des Ergebnisses der Aktivitätsausführung zurückgegeben.

* **Wenn `firstRowOnly` auf `true` (Standardwert) gesetzt ist**, wird das Ausgabeformat wie im folgenden Code gezeigt. Das Lookupergebnis befindet sich unter dem festen Schlüssel `firstRow`. Um das Ergebnis in nachfolgenden Aktivitäten zu nutzen, verwenden Sie das Muster `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Wenn `firstRowOnly` auf `false` festgelegt ist**, wird das Ausgabeformat wie im folgenden Code gezeigt. Das Feld `count` gibt an, wie viele Datensätze zurückgegeben werden, und es werden ausführliche Werte unter dem festen Array `value` angezeigt. In diesem Fall folgt auf die Lookupaktivität in der Regel eine [ForEach-Aktivität](control-flow-for-each-activity.md). Um das Array `value` an das Feld `items` der ForEach-Aktivität zu übergeben, verwenden Sie das Muster `@activity('MyLookupActivity').output.value`. Für den Zugriff auf Elemente in Array `value` verwenden Sie die folgende Syntax: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Hier finden Sie ein Beispiel: `@{activity('lookupActivity').output.value[0].tablename}`.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "TableName" : "Table1"
            },
            {
                "Id": "2",
                "TableName" : "Table2"
            }
        ]
    } 
    ```

## <a name="example"></a>Beispiel
In diesem Beispiel kopieren Sie mit der Kopieraktivität Daten aus einer SQL-Tabelle in Ihrer Azure SQL-Datenbankinstanz nach Azure Blob Storage. Der Name der SQL-Tabelle wird in einer JSON-Datei in Blob Storage gespeichert. Die Lookup-Aktivität sucht den Tabellennamen zur Laufzeit. Bei diesem Ansatz kann JSON dynamisch geändert werden, ohne dass Sie Pipelines oder Datasets erneut bereitstellen müssen. 

Dieses Beispiel zeigt nur das Lookup für die erste Zeile. Wenn Sie weitere Informationen zum Lookup für alle Zeilen und dem Verketten von Ergebnissen mit der ForEach-Aktivität benötigen, lesen Sie die Beispiele in [Massenkopieren von mehreren Tabellen mithilfe von Azure Data Factory](tutorial-bulk-copy.md).

### <a name="pipeline"></a>Pipeline
Diese Pipeline enthält zwei Aktivitäten: *Lookup* und *Kopieren*. 

- Die Lookupaktivität ist für die Verwendung von „LookupDataset“ konfiguriert, das auf einen Speicherort in Azure Blob Storage verweist. Die Lookup-Aktivität liest den Namen der SQL-Tabelle aus einer JSON-Datei an diesem Speicherort. 
- Die Kopieraktivität verwendet die Ausgabe der Lookupaktivität (Name der SQL-Tabelle). Die tableName-Eigenschaft im Quelldataset (SourceDataset) ist für die Verwendung der Ausgabe der Lookupaktivität konfiguriert. Die Kopieraktivität kopiert Daten aus der SQL-Tabelle an einen Speicherort in Azure Blob Storage, der durch die SinkDataset-Eigenschaft angegeben wird. 


```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ],
                "inputs": [ 
                    { 
                        "referenceName": "SourceDataset", 
                        "type": "DatasetReference" 
                    } 
                ],
                "outputs": [ 
                    { 
                        "referenceName": "SinkDataset", 
                        "type": "DatasetReference" 
                    } 
                ]
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Suchdataset
Das Lookupdataset verweist auf die Datei *sourcetable.json* im Lookupordner von Azure Storage, der durch den AzureStorageLinkedService-Typ angegeben wird. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-the-copy-activity"></a>Quelldataset für die Kopieraktivität
Das Quelldataset verwendet die Ausgabe der Lookup-Aktivität, also den Namen der SQL-Tabelle. Die Kopieraktivität kopiert Daten aus der SQL-Tabelle an einen Speicherort in Azure Blob Storage, der durch das Senkendataset angegeben wird. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-the-copy-activity"></a>Senkendataset für die Kopieraktivität
Die Kopieraktivität kopiert Daten aus der SQL-Tabelle in die Datei *filebylookup.csv* im Ordner *csv* in Azure Storage, der durch die AzureStorageLinkedService-Eigenschaft angegeben wird. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
            "format": {
                "type": "TextFormat"                                                                    
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Mit Azure Storage verknüpfter Dienst
Dieses Speicherkonto enthält die JSON-Datei mit den Namen der SQL-Tabellen. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>",
                "type": "SecureString"
            }
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Mit Azure SQL-Datenbank verknüpfter Dienst
Diese Azure SQL-Datenbankinstanz enthält die Daten, die in Blob Storage kopiert werden sollen. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": {
                "value": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;",
                "type": "SecureString"
            }
        }
    }
}
```

### <a name="sourcetablejson"></a>sourcetable.json

#### <a name="set-of-objects"></a>Satz von Objekten

```json
{
  "Id": "1",
  "tableName": "Table1",
}
{
   "Id": "2",
  "tableName": "Table2",
}
```

#### <a name="array-of-objects"></a>Array von Objekten

```json
[ 
    {
        "Id": "1",
          "tableName": "Table1",
    }
    {
        "Id": "2",
        "tableName": "Table2",
    }
]
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter anderen Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden: 

- [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md)
- [ForEach-Aktivität](control-flow-for-each-activity.md)
- [Aktivität „Metadaten abrufen“](control-flow-get-metadata-activity.md)
- [Webaktivität](control-flow-web-activity.md)

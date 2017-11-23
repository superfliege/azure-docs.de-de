---
title: "Lookup-Aktivität in Azure Data Factory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie die Lookup-Aktivität verwendet wird, um einen Wert in einer externen Quelle zu suchen. Auf die Ausgabe kann durch nachfolgende Aktivitäten verwiesen werden."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: shlo
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: spelluru
ms.openlocfilehash: d498705ef7f714b4f15b8d2722053bf3081b5045
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2017
---
# <a name="lookup-activity-in-azure-data-factory"></a>Lookup-Aktivität in Azure Data Factory
Mit der Lookup-Aktivität können Sie einen Datensatz/Tabellennamen/Wert in einer externen Quelle lesen oder suchen. Auf die Ausgabe kann durch nachfolgende Aktivitäten verwiesen werden. 

Die folgenden Datenquellen werden derzeit für die Lookup-Aktivität unterstützt:
- JSON-Datei in Azure-Blob
- Lokale JSON-Datei
- Azure SQL-Datenbank (aus einer Abfrage konvertierte JSON-Daten)
- Azure Table Storage (aus einer Abfrage konvertierte JSON-Daten)

Die Lookup-Aktivität ist nützlich, wenn Sie dynamisch eine Liste der Dateien/Datensätze/Tabellen aus einer Konfigurationsdatei oder einer Datenquelle abrufen möchten. Die Ausgabe der Aktivität kann von anderen Aktivitäten weiter verwendet werden, um eine bestimmte Verarbeitung nur für diese Elemente auszuführen.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie die [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md).


## <a name="example"></a>Beispiel
In diesem Beispiel kopieren Sie mit der Kopieraktivität Daten aus einer SQL-Tabelle in Azure SQL-Datenbank nach Azure Blob Storage. Der Name der SQL-Tabelle wird in einer JSON-Datei in Blob Storage gespeichert. Die Lookup-Aktivität sucht den Tabellennamen zur Laufzeit. Bei diesem Ansatz kann JSON dynamisch geändert werden, ohne dass Pipelines/Datasets erneut bereitgestellt werden müssen. 

### <a name="pipeline"></a>Pipeline
Diese Pipeline enthält zwei Aktivitäten: **Suchen** und **Kopieren**. 

- Die Lookup-Aktivität ist für die Verwendung von „LookupDataset“ konfiguriert, das auf einen Speicherort in einem Azure-Blobspeicher verweist. Die Lookup-Aktivität liest den Namen der SQL-Tabelle aus einer JSON-Datei an diesem Speicherort. 
- Die Kopieraktivität verwendet die Ausgabe der Lookup-Aktivität (Name der SQL-Tabelle). tableName im Quelldataset (SourceDataset) ist für die Verwendung der Ausgabe der Lookup-Aktivität konfiguriert. Die Kopieraktivität kopiert Daten aus der SQL-Tabelle an einen Speicherort in Azure Blob Storage, der durch SinkDataset angegeben wird. 


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
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.tableName}" 
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
Das Suchdataset verweist auf die Datei „sourcetable.json“ im Ordner „lookup“ im Azure-Speicher, der durch AzureStorageLinkedService angegeben wird. 

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
            "tableName": "@{activity('LookupActivity').output.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-the-copy-activity"></a>Senkendataset für die Kopieraktivität
Die Kopieraktivität kopiert Daten aus der SQL-Tabelle in die Datei „filebylookup.csv“ im Ordner „csv“ im Azure-Speicher, der durch AzureStorageLinkedService angegeben wird. 

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

### <a name="azure-storage-linked-service"></a>Mit Azure-Speicher verknüpfter Dienst
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
Diese Azure SQL-Datenbank enthält die Daten, die in den Blobspeicher kopiert werden sollen. 

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



## <a name="type-properties"></a>Typeigenschaften
Name | Beschreibung | Typ | Erforderlich
---- | ----------- | ---- | --------
Dataset | Das dataset-Attribut dient dazu, den Datasetverweis für die Suche bereitzustellen. Derzeit werden folgende Datasettypen unterstützt:<ul><li>FileShareDataset</li><li>AzureBlobDataset</li><li>AzureSqlTableDataset</li><li>AzureTableDataset</li> | Schlüssel-Wert-Paar | Ja
Quelle | Spezifische Quelleneigenschaften für das Dataset, identisch mit der Quelle der Kopieraktivität | Schlüssel-Wert-Paar | Nein
firstRowOnly | Gibt die erste Zeile oder alle Zeilen zurück. | Boolescher Wert | Nein

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter anderen Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden: 

- [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md)
- [ForEach-Aktivität](control-flow-for-each-activity.md)
- [Aktivität „Metadaten abrufen“](control-flow-get-metadata-activity.md)
- [Webaktivität](control-flow-web-activity.md)

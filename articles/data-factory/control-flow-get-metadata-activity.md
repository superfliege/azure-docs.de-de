---
title: "Aktivität „Metadaten abrufen“ in Azure Data Factory | Microsoft-Dokumentation"
description: "Informationen, wie Sie die SQL Server-Aktivität \"Gespeicherte Prozedur\" in einer Data Factory-Pipeline zum Aufrufen einer gespeicherten Prozedur in einer Azure SQL-Datenbank oder einem Azure SQL Data Warehouse verwenden können."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: shlo
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 99182b2ed91f6d60f499be0078077bf52fe8b366
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Aktivität „Metadaten abrufen“ in Azure Data Factory
Die Aktivität „Metadaten abrufen“ kann zum Abrufen von Metadaten für alle Daten in Azure Data Factory verwendet werden. Diese Aktivität wird nur für Data Factorys der Version 2 unterstützt. Sie kann in folgenden Szenarien verwendet werden:

- Überprüfen der Metadateninformationen von Daten
- Auslösen einer Pipeline, wenn Daten bereit/verfügbar sind

Die folgende Funktionalität ist in der Ablaufsteuerung verfügbar:
- Die Ausgabe der Aktivität „Metadaten abrufen“ kann in bedingten Ausdrücken zur Validierung verwendet werden.
- Eine Pipeline kann über eine „Wiederholen bis“-Schleife ausgelöst werden, wenn die Bedingung erfüllt ist.

Die Aktivität „Metadaten abrufen“ nutzt ein Dataset als erforderliche Eingabe und gibt Metadateninformationen zurück, die als Ausgabe verfügbar sind. Derzeit werden nur Azure-Blobdatasets unterstützt. Die unterstützten Metadatenfelder sind „size“, „structure“ und „lastModified“-Zeit.  

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie die [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md).


## <a name="syntax"></a>Syntax

### <a name="get-metadata-activity-definition"></a>Definition der Aktivität „Metadaten abrufen“:
Im folgenden Beispiel gibt die Aktivität „Metadaten abrufen“ Metadaten zu den Daten zurück, die durch MyDataset dargestellt werden. 

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```
### <a name="dataset-definition"></a>Definition des Datasets:

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "Filename": "file.json",
            "format":{
                "type":"JsonFormat"
                "nestedSeperator": ","
            }
        }
    }
}
```

### <a name="output"></a>Ausgabe
```json
{
    "size": 1024,
    "structure": [
        {
            "name": "id",
            "type": "Int64"
        }, 
    ],
    "lastModified": "2016-07-12T00:00:00Z"
}
```

## <a name="type-properties"></a>Typeigenschaften
Die Aktivität „Metadaten abrufen“ kann zurzeit die folgenden Typen von Metadateninformationen aus einem Azure-Speicherdataset abrufen.

Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
fieldList | Listet die erforderlichen Typen der Metadateninformationen auf.  | <ul><li>size</li><li>structure</li><li>lastModified</li></ul> |    Nein<br/>Falls leer, gibt die Aktivität alle drei unterstützten Metadateninformationen zurück. 
Dataset | Das Referenzdataset, dessen Metadatenaktivität von der Aktivität „Metadaten abrufen“ abgerufen werden soll. <br/><br/>Der derzeit unterstützte Datasettyp ist Azure-Blob. Die beiden untergeordneten Eigenschaften sind: <ul><li><b>referenceName</b>: Verweis auf ein vorhandenes Azure-Blobdataset</li><li><b>type</b>: Da auf das Dataset verwiesen wird, ist der Typ „DatasetReference“</li></ul> |    <ul><li>String</li><li>DatasetReference</li></ul> | Ja

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter anderen Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden: 

- [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md)
- [ForEach-Aktivität](control-flow-for-each-activity.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [Webaktivität](control-flow-web-activity.md)

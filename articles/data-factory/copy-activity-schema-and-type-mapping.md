---
title: "Schemazuordnung in Kopieraktivität | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Kopieraktivität in Azure Data Factory beim Kopieren von Daten Schemas und Datentypen aus Quelldaten Senkendaten zuordnet."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jingwang
ms.openlocfilehash: 459c792028d3eede059814324597811b24e65ac2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="schema-mapping-in-copy-activity"></a>Schemazuordnung in Kopieraktivität
In diesem Artikel wird beschrieben, wie Kopieraktivität in Azure Data Factory Schemazuordnung und Datentypzuordnung von Quelldaten zu Senkendaten beim Kopieren der Daten ausführt.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie die [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md).


## <a name="column-mapping"></a>Spaltenzuordnung

Standardmäßig ordnet Kopieraktivität **Quelldaten nach Spaltennamen Senkendaten zu**, es sei denn, [explizite Spaltenzuordnung](#explicit-column-mapping) ist konfiguriert. Genauer gesagt, macht Kopieraktivität Folgendes:

1. Lesen von Daten aus der Quelle und Bestimmen des Quellschemas

    * Für Datenquellen mit vordefiniertem Schema im Datenspeicher/Datei-Format, z.B. Datenbanken/Dateien mit Metadaten (Avro/ORC/Parquet/Text mit Header), wird das Quellschema aus dem Abfrageergebnis oder Dateimetadaten extrahiert.
    * Für Datenquellen mit flexiblem Schema, z.B. Azure Table/Cosmos DB, wird das Quellschema aus dem Abfrageergebnis abgeleitet. Sie können es überschreiben, indem Sie „structure“ im Dataset angeben.
    * Für Textdateien ohne Header werden Standardspaltennamen mit dem Muster „Prop_0“, „Prop_1“, ... generiert. Sie können sie durch Angabe von „structure“ im Dataset überschreiben.
    * Für Dynamics-Quellen müssen Sie die Schemainformationen im Dataset im Abschnitt „structure“ angeben.

2. Wenden Sie explizite Spaltenzuordnung an, falls angegeben.

3. Schreiben der Daten in die Senke

    * Für Datenspeicher mit vordefiniertem Schema werden die Daten in die Spalten identischen Namens geschrieben.
    * Für Datenspeicher ohne festes Schema und Dateiformate werden die Spaltennamen/Metadaten auf dem Quellschema basierend generiert.

### <a name="explicit-column-mapping"></a>Explizite Spaltenzuordnung

Sie können **columnMappings** im Abschnitt **typeProperties** der Kopieraktivität angeben, um eine explizite Spaltenzuordnung durchzuführen. In diesem Szenario ist der Abschnitt „structure“ für Eingabe- und Ausgabedatasets erforderlich. Die Spaltenzuordnung unterstützt die **Zuordnung aller oder einer Teilmenge der Spalten in „structure“ des Quelldatasets zu allen Spalten in „structure“ des Senkendatasets**. Im Folgenden sind Fehlerbedingungen angegeben, die zu einer Ausnahme führen:

* Das Ergebnis der Abfrage des Quelldatenspeichers enthält keinen Spaltennamen, der im Abschnitt „structure“ des Eingabedatasets angegeben wird.
* Der Senkendatenspeicher (sofern mit vordefiniertem Schema) enthält keinen Spaltennamen, der im Abschnitt „structure“ des Ausgabedatasets angegeben wird.
* Entweder sind weniger Spalten oder mehr Spalten in „structure“ des Senkendatasets, als in der Zuordnung angegeben.
* Doppelte Zuordnung.

#### <a name="explicit-column-mapping-example"></a>Beispiel für explizite Spaltenzuordnung

In diesem Beispiel verfügt die Eingabetabelle über eine Struktur, und diese verweist auf eine Tabelle in einer lokalen SQL-Datenbank.

```json
{
    "name": "SqlServerInput",
    "properties": {
        "structure":
         [
            { "name": "UserId"},
            { "name": "Name"},
            { "name": "Group"}
         ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "SqlServerLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTable"
        }
    }
}
```

In diesem Beispiel verfügt die Ausgabetabelle über eine Struktur, und diese verweist auf eine Tabelle in einer Azure SQL-Datenbank.

```json
{
    "name": "AzureSqlOutput",
    "properties": {
        "structure":
        [
            { "name": "MyUserId"},
            { "name": "MyName" },
            { "name": "MyGroup"}
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SinkTable"
        }
    }
}
```

Der folgende JSON-Code definiert eine Kopieraktivität in einer Pipeline. Die Spalten der Quelle werden mithilfe der **translator**-Eigenschaft den Spalten der Senke (**columnMappings**) zugeordnet.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [
        {
            "referenceName": "SqlServerInput",
            "type": "DatasetReference"
        }
    ],
    "outputs": [
        {
            "referenceName": "AzureSqlOutput",
            "type": "DatasetReference"
        }
    ],
    "typeProperties":    {
        "source": { "type": "SqlSource" },
        "sink": { "type": "SqlSink" },
        "translator":
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    }
}
```

**Ablauf der Spaltenzuordnung:**

![Ablauf der Spaltenzuordnung](./media/copy-activity-schema-and-type-mapping/column-mapping-sample.png)

## <a name="data-type-mapping"></a>Datentypzuordnung

Kopieraktivität führt Zuordnungen von Quelltypen zu Senkentypen mit dem folgenden 2-Schritte-Ansatz durch:

1. Konvertieren von nativen Quelltypen in Azure Data Factory-Zwischendatentypen
2. Konvertieren von Azure Data Factory-Zwischendatentypen in nativen Senkentyp

Sie können die Zuordnung des nativen Typs zum Zwischendatentyp im „Datentypzuordnung“-Abschnitt jedes Connectorthemas finden.

### <a name="supported-data-types"></a>Unterstützte Datentypen

Data Factory unterstützt die folgenden Zwischendatentypen: Sie können bei der Angabe von Typinformationen in der Konfiguration der [Datasetstruktur](concepts-datasets-linked-services.md#dataset-structure) folgende Werte verwenden:

* Byte[]
* Boolean
* Datetime
* Datetimeoffset
* Decimal
* Doppelt
* Guid
* Int16
* Int32
* Int64
* Single
* String
* Timespan

### <a name="explicit-data-type-conversion"></a>Explizite Konvertierung von Datentypen

Beim Kopieren von Daten in Datenspeicher mit festem Schema, z.B. SQL Server/Oracle, sollte die explizite Typkonvertierung quellseitig deklariert werden, wenn die gleiche Spalte in Quelle und Senke unterschiedlichen Typs ist:

* Für die Dateiquelle, z.B. CSV/Avro, wird die Typkonvertierung mit vollständiger Spaltenliste (quellseitiger Spaltenname und senkenseitiger Quelltyp) über die Quellstruktur deklariert.
* Für die relationale Quelle (z.B. SQL/Oracle) sollte die Typkonvertierung durch explizite Typumwandlung in der Abfrageanweisung erreicht werden.

## <a name="when-to-specify-dataset-structure"></a>Wann ist „structure“ im Dataset erforderlich?

In folgenden Szenarien ist „structure“ in Dataset erforderlich:

* Anwenden [expliziter Datentypkonvertierung](#explicit-data-type-conversion) für Dateiquellen beim Kopieren (Eingabedataset)
* Anwenden [expliziter Spaltenzuordnung](#explicit-column-mapping) beim Kopieren (sowohl für Eingabe- als auch Ausgabedataset)
* Kopieren aus Dynamics 365/CRM-Quelle (Eingabedataset)
* Kopieren nach Cosmos DB als geschachteltes Objekt, wenn die Quelle keine JSON-Datei ist (Ausgabedataset)

In folgenden Szenarien wird „structure“ im Dataset vorgeschlagen:

* Kopieren aus einer Textdatei ohne Header (Eingabedataset). Sie können die Spaltennamen für die Textdatei den entsprechenden Senkenspalten angleichen, um keine explizite Spaltenzuordnung anzugeben.
* Kopieren aus Datenspeichern mit flexiblem Schema, z.B. Azure Table/Cosmos DB (Eingabedataset), um zu gewährleisten, dass die erwarteten Daten (Spalten) kopiert werden, anstatt dass die Kopieraktivität das Schema bei jeder Aktivitätsausführung anhand der obersten Zeile(n) ableitet.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Fault tolerance of copy activity in Azure Data Factory](copy-activity-fault-tolerance.md) (Fehlertoleranz der Kopieraktivität in Azure Data Factory)
- [Copy Activity performance and tuning guide](copy-activity-performance.md) (Handbuch zur Leistung und Optimierung der Kopieraktivität)

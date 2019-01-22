---
title: Schemazuordnung in Kopieraktivität | Microsoft-Dokumentation
description: Erfahren Sie, wie Kopieraktivität in Azure Data Factory beim Kopieren von Daten Schemas und Datentypen aus Quelldaten Senkendaten zuordnet.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: c2f58a3510699cdf74e3150d3ad5882929f4f05b
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358710"
---
# <a name="schema-mapping-in-copy-activity"></a>Schemazuordnung in Kopieraktivität
In diesem Artikel wird beschrieben, wie Kopieraktivität in Azure Data Factory Schemazuordnung und Datentypzuordnung von Quelldaten zu Daten der Empfangsquelle (Senkendaten) beim Kopieren der Daten ausführt.

## <a name="column-mapping"></a>Spaltenzuordnung

Die Spaltenzuordnung wird angewendet, wenn Daten zwischen tabellarischen Datencontainern kopiert werden. Standardmäßig ordnet Kopieraktivität **Quelldaten nach Spaltennamen Senkendaten zu**, es sei denn, [explizite Spaltenzuordnung](#explicit-column-mapping) ist konfiguriert. Genauer gesagt, macht Kopieraktivität Folgendes:

1. Lesen von Daten aus der Quelle und Bestimmen des Quellschemas

    * Für Datenquellen mit vordefiniertem Schema im Datenspeicher/Datei-Format, z.B. Datenbanken/Dateien mit Metadaten (Avro/ORC/Parquet/Text mit Header), wird das Quellschema aus dem Abfrageergebnis oder Dateimetadaten extrahiert.
    * Für Datenquellen mit flexiblem Schema, z.B. Azure Table/Cosmos DB, wird das Quellschema aus dem Abfrageergebnis abgeleitet. Sie können es überschreiben, indem Sie „structure“ im Dataset konfigurieren.
    * Für Textdateien ohne Header werden Standardspaltennamen mit dem Muster „Prop_0“, „Prop_1“, ... generiert. Sie können sie überschreiben, indem Sie „structure“ im Dataset konfigurieren.
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
            "columnMappings": 
            {
                "UserId": "MyUserId",
                "Group": "MyGroup",
                "Name": "MyName"
            }
        }
    }
}
```

Wenn Sie zum Angeben der Spaltenzuordnung die Syntax `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` verwenden, wird sie weiterhin unverändert unterstützt.

**Ablauf der Spaltenzuordnung:**

![Ablauf der Spaltenzuordnung](./media/copy-activity-schema-and-type-mapping/column-mapping-sample.png)

## <a name="schema-mapping"></a>Schemazuordnung

Die Schemazuordnung wird angewendet, wenn Daten zwischen hierarchisch und tabellarisch strukturierten Daten kopiert werden, z. B. Kopieren aus MongoDB/REST in eine Textdatei oder aus SQL in die Azure Cosmos DB-API für MongoDB. Folgende Eigenschaften werden im Abschnitt `translator` der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Kopieraktivität „translator“ muss auf Folgendes festgelegt werden: **TabularTranslator** | JA |
| schemaMapping | Eine Sammlung von Schlüssel-Wert-Paaren, die die Zuordnungsbeziehung von der tabellarischen zur hierarchischen Seite darstellt.<br/>- **Key:** Der Spaltenname der tabellarischen Daten wie in der Datasetstruktur definiert.<br/>- **Value:** Der Ausdruck des JSON-Pfads für jedes Feld, das extrahiert und zugeordnet werden soll. Bei Feldern unter dem Stammobjekt beginnen Sie mit Stamm „$“. Bei Feldern innerhalb des Arrays, die anhand der `collectionReference`-Eigenschaft ausgewählt werden, beginnen Sie mit dem Arrayelement.  | JA |
| collectionReference | Wenn Sie Daten durchlaufen und Objekte **innerhalb eines Arrayfelds** mit demselben Muster extrahieren, und wenn Sie möchten, dass jedes Objekt in einer neuen Zeile steht, geben Sie den JSON-Pfad dieses Arrays für die übergreifende Anwendung an. Diese Eigenschaft wird nur unterstützt, wenn hierarchische Daten die Quelle sind. | Nein  |

**Beispiel: Kopieren aus MongoDB nach SQL:**

Sie besitzen zum Beispiel ein MongoDB-Dokument mit folgendem Inhalt: 

```json
{
    "id": {
        "$oid": "592e07800000000000000000"
    },
    "number": "01",
    "date": "20170122",
    "orders": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "name": "Seattle" } ]
}
```

Wenn Sie es in eine Azure SQL-Tabelle im folgenden Format kopieren möchten, indem Sie die Daten im Array *(order_pd und order_price)* vereinfachen und mit den allgemeinen Stamminformationen *(Anzahl, Datum und Stadt)* überkreuzt verknüpfen, dann gilt Folgendes:

| orderNumber | orderDate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | Seattle |
| 01 | 20170122 | P2 | 13 | Seattle |
| 01 | 20170122 | P3 | 231 | Seattle |

Konfigurieren Sie die Regel für die Schemazuordnung wie im folgenden JSON-Beispiel für die Kopieraktivität:

```json
{
    "name": "CopyFromMongoDBToSqlAzure",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "MongoDbV2Source"
        },
        "sink": {
            "type": "SqlSink"
        },
        "translator": {
            "type": "TabularTranslator",
            "schemaMapping": {
                "orderNumber": "$.number", 
                "orderDate": "$.date", 
                "order_pd": "prod", 
                "order_price": "price",
                "city": " $.city[0].name"
            },
            "collectionReference":  "$.orders"
        }
    }
}
```

## <a name="data-type-mapping"></a>Datentypzuordnung

Kopieraktivität führt Zuordnungen von Quelltypen zu Senkentypen mit dem folgenden 2-Schritte-Ansatz durch:

1. Konvertieren von nativen Quelltypen in Azure Data Factory-Zwischendatentypen
2. Konvertieren von Azure Data Factory-Zwischendatentypen in nativen Senkentyp

Sie können die Zuordnung des nativen Typs zum Zwischendatentyp im „Datentypzuordnung“-Abschnitt jedes Connectorthemas finden.

### <a name="supported-data-types"></a>Unterstützte Datentypen

Data Factory unterstützt die folgenden Zwischendatentypen: Sie können beim Konfigurieren von Typinformationen in der Konfiguration der [Datasetstruktur](concepts-datasets-linked-services.md#dataset-structure) folgende Werte angeben:

* Byte[]
* Boolescher Wert
* DateTime
* Datetimeoffset
* DECIMAL
* Double
* Guid
* Int16
* Int32
* Int64
* Single
* Zeichenfolge
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

* Kopieren aus einer Textdatei ohne Header (Eingabedataset). Sie können die Spaltennamen für die Textdatei den entsprechenden Senkenspalten angleichen, um keine explizite Spaltenzuordnung zu konfigurieren.
* Kopieren aus Datenspeichern mit flexiblem Schema, z.B. Azure Table/Cosmos DB (Eingabedataset), um zu gewährleisten, dass die erwarteten Daten (Spalten) kopiert werden, anstatt dass die Kopieraktivität das Schema bei jeder Aktivitätsausführung anhand der obersten Zeile(n) ableitet.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Fault tolerance of copy activity in Azure Data Factory](copy-activity-fault-tolerance.md) (Fehlertoleranz der Kopieraktivität in Azure Data Factory)
- [Copy Activity performance and tuning guide](copy-activity-performance.md) (Handbuch zur Leistung und Optimierung der Kopieraktivität)

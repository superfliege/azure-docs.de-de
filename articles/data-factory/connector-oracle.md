---
title: Kopieren von Daten nach bzw. aus Oracle mit Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie mithilfe der Data Factory Daten von unterstützten Quellspeichern in die Oracle-Datenbank oder aus Oracle in unterstützte Senkenspeicher kopiert werden."
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
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: ad9807fc72e3a6e0a6d3916d2d9e042af1fa83ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-and-to-oracle-using-azure-data-factory"></a>Kopieren von Daten aus und nach Oracle mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-onprem-oracle-connector.md)
> * [Version 2 – Vorschau](connector-oracle.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer Oracle-Datenbank und in eine Oracle-Datenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (General Availability, GA) ist, lesen Sie [Oracle-Connector in V1](v1/data-factory-onprem-oracle-connector.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Sie können Daten aus einer Oracle-Datenbank in einen unterstützten Senkendatenspeicher oder Daten aus einem unterstützten Senkendatenspeicher in eine Oracle-Datenbank kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Oracle-Connector unterstützt insbesondere die folgenden Versionen der Oracle-Datenbank:

    - Oracle 12c R1 (12.1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10.1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

## <a name="prerequisites"></a>Voraussetzungen

Zum Kopieren von Daten aus einer Oracle-Datenbank bzw. in eine Oracle-Datenbank, die nicht öffentlich zugänglich ist, müssen Sie eine selbstgehostete Integrationslaufzeit einrichten. Im Artikel [Selbstgehostete Integrationslaufzeit](create-self-hosted-integration-runtime.md) finden Sie Details zur Integrationslaufzeit. Die Integrationslaufzeit bietet einen integrierten Oracle-Treiber. Daher müssen beim Kopieren von Daten aus bzw. nach Oracle keine Treiber manuell installiert werden.

## <a name="getting-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Oracle-Connector verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Oracle verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **Oracle** festgelegt werden. | Ja |
| connectionString | Geben Sie Informationen an, die zur Verbindung mit der Oracle-Datenbankinstanz erforderlich sind. Legen Sie für dieses Feld „SecureString“ fest. | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die selbstgehostete Integration Runtime oder Azure Integration Runtime verwenden (sofern Ihr Datenspeicher öffentlich zugänglich ist). Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. |Nein |

**Beispiel:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Oracle-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus bzw. nach Oracle die type-Eigenschaft des Datasets auf **OracleTable** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft des Datasets muss auf **OracleTable** festgelegt werden. | Ja |
| tableName |Name der Tabelle in der Oracle Database, auf die der verknüpfte Dienst verweist. | Ja |

**Beispiel:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Oracle-Quelle und -Senke unterstützt werden.

### <a name="oracle-as-source"></a>Oracle als Quelle

Legen Sie zum Kopieren von Daten aus Oracle den Quelltyp in der Kopieraktivität auf **OracleSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **OracleSource** festgelegt werden. | Ja |
| oracleReaderQuery | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"SELECT * FROM MyTable"`. | Nein |

Ohne Angabe von „oracleReaderQuery“ werden die im Abschnitt „structure“ des Datasets definierten Spalten zum Erstellen einer Abfrage (`select column1, column2 from mytable`) verwendet, die auf die Oracle-Datenbank angewendet wird. Falls die Datasetdefinition nicht den Abschnitt „structure“ enthält, werden alle Spalten der Tabelle ausgewählt.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-sink"></a>Oracle als Senke

Legen Sie zum Kopieren von Daten in Oracle den Senkentyp in der Kopieraktivität auf **OracleSink** fest. Folgende Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **OracleSink** festgelegt werden. | Ja |
| writeBatchSize | Fügt Daten in die SQL-Tabelle ein, wenn die Puffergröße "writeBatchSize" erreicht.<br/>Zulässige Werte sind ganze Zahlen (Anzahl der Zeilen). |Nein (Standard = 10.000) |
| writeBatchTimeout | Die Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird.<br/>Zulässige Werte sind Zeiträume. Beispiel: 00:30:00 (30 Minuten) | Nein |
| preCopyScript | Geben Sie eine auszuführende SQL-Abfrage für die Kopieraktivität an, ehe Sie bei der jeder Ausführung Daten in Oracle schreiben. Sie können diese Eigenschaft nutzen, um die vorab geladenen Daten zu bereinigen. | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Datentypzuordnung für Oracle

Beim Kopieren von Daten aus bzw. nach Oracle werden die folgenden Zuordnungen von Oracle-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

| Datentyp "Oracle" | Data Factory-Zwischendatentyp |
|:--- |:--- |
| BFILE |Byte[] |
| BLOB |Byte[] |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |Dezimal, Zeichenfolge (wenn Genauigkeit > 28) |
| INTEGER |Dezimal, Zeichenfolge (wenn Genauigkeit > 28) |
| LONG |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| NUMBER |Dezimal, Zeichenfolge (wenn Genauigkeit > 28) |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIMESTAMP WITH LOCAL TIME ZONE |String |
| TIMESTAMP WITH TIME ZONE |String |
| UNSIGNED INTEGER |NUMBER |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> Die Datentypen „INTERVAL YEAR TO MONTH“ und „INTERVAL DAY TO SECOND“ werden nicht unterstützt.


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
---
title: Kopieren von Daten aus Teradata mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie mehr über den Teradata-Connector von Data Factory, mit dem Sie Daten aus einer Teradata-Datenbank als Senken in von Data Factory unterstützte Datenspeicher kopieren können."
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
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 905a2bf1b42819a531bc4b16dd1e6f5539e80068
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Kopieren von Daten aus Teradata mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: allgemein verfügbar](v1/data-factory-onprem-teradata-connector.md)
> * [Version 2 – Vorschauversion](connector-teradata.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer Teradata-Datenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, lesen Sie [Teradata-Connector in V1](v1/data-factory-onprem-teradata-connector.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer Teradata-Datenbank in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der Teradata-Connector unterstützt insbesondere Folgendes:

- Teradata, **Version 12 und höher**
- Kopieren von Dateien unter Verwendung der **Standard**- oder **Windows**-Authentifizierung

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie zum Verwenden dieses Teradata-Connectors diese Schritte aus:

- Richten Sie eine selbstgehostete Integration Runtime ein. Im Artikel [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) finden Sie Details.
- Installieren Sie den [.NET-Datenanbieter für Teradata](http://go.microsoft.com/fwlink/?LinkId=278886), Version 14 oder höher, auf dem Computer mit der Integration Runtime.

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Teradata-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Teradata verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **Teradata** festgelegt sein. | Ja |
| server | Name des Teradata-Servers. | Ja |
| authenticationType | Typ der Authentifizierung für die Verbindung mit der Teradata-Datenbank.<br/>Zulässige Werte: **Basic** und **Windows**. | Ja |
| username | Geben Sie einen Benutzernamen für das Herstellen der Verbindung mit der Teradata-Datenbank an. | Ja |
| password | Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. Legen Sie für dieses Feld „SecureString“ fest. | Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Eine selbstgehostete Integrationslaufzeit ist erforderlich, wie unter [Voraussetzungen](#prerequisites) erwähnt wird. |Ja |

**Beispiel:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Dataset „Teradata“ unterstützt werden.

Legen Sie zum Kopieren von Daten aus Teradata die „type“-Eigenschaft des Datasets auf **RelationalTable** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **RelationalTable** festgelegt werden. | Ja |
| tableName | Name der Tabelle in der Teradata-Datenbank. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

**Beispiel:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Quelle „Teradata“ unterstützt werden.

### <a name="teradata-as-source"></a>Teradata als Quelle

Legen Sie zum Kopieren von Daten aus Teradata den Quellentyp in der Kopieraktivität auf **RelationalSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **RelationalSource** festgelegt werden. | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"SELECT * FROM MyTable"`. | Nein (wenn „tableName“ im Dataset angegeben ist) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-teradata"></a>Datentypzuordnung für Teradata

Beim Kopieren von Daten aus Teradata werden die folgenden Zuordnungen von Teradata-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

| Teradata-Datentyp | Data Factory-Zwischendatentyp |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| Char |Zeichenfolge |
| Clob |Zeichenfolge |
| Datum |Datetime |
| DECIMAL |DECIMAL |
| Double |Double |
| Graphic |Zeichenfolge |
| Ganze Zahl  |Int32 |
| Interval Day |Zeitraum |
| Interval Day To Hour |Zeitraum |
| Interval Day To Minute |Zeitraum |
| Interval Day To Second |Zeitraum |
| Interval Hour |Zeitraum |
| Interval Hour To Minute |Zeitraum |
| Interval Hour To Second |Zeitraum |
| Interval Minute |Zeitraum |
| Interval Minute To Second |Zeitraum |
| Interval Month |Zeichenfolge |
| Interval Second |Zeitraum |
| Interval Year |Zeichenfolge |
| Interval Year To Month |Zeichenfolge |
| Number |Double |
| Period(Date) |Zeichenfolge |
| Period(Time) |Zeichenfolge |
| Period(Time With Time Zone) |Zeichenfolge |
| Period(Timestamp) |Zeichenfolge |
| Period(Timestamp With Time Zone) |Zeichenfolge |
| SmallInt |Int16 |
| Zeit |Zeitraum |
| Time With Time Zone |Zeichenfolge |
| Zeitstempel |Datetime |
| Timestamp With Time Zone |DateTimeOffset |
| VarByte |Byte[] |
| VarChar |Zeichenfolge |
| VarGraphic |Zeichenfolge |
| xml |Zeichenfolge |


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
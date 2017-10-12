---
title: Kopieren von Daten aus Sybase mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten aus Sybase mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
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
ms.openlocfilehash: 1170ee7232e1046e194f5223f7b7bf582ef18dfe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-sybase-using-azure-data-factory"></a>Kopieren von Daten aus Sybase mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-onprem-sybase-connector.md)
> * [Version 2: Vorschauversion](connector-sybase.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer Sybase-Datenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, lesen Sie [Sybase-Connector in V1](v1/data-factory-onprem-sybase-connector.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Sie können Daten aus einer Sybase-Datenbank in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Sybase-Connector unterstützt insbesondere Folgendes:

- Sybase, **Version 16 und höher**
- Kopieren von Dateien unter Verwendung der Authentifizierung des Typs **Basic** oder **Windows**

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie zum Verwenden dieses Sybase-Connectors diese Schritte aus:

- Richten Sie eine selbstgehostete Integration Runtime ein. Im Artikel [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) finden Sie Details.
- Installieren Sie den [Datenanbieter für Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 oder höher auf dem Computer mit der Integration Runtime.

## <a name="getting-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Sybase-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Sybase verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die „type“-Eigenschaft muss auf **Sybase** festgelegt sein. | Ja |
| server | Name des Sybase-Servers. |Ja |
| database | Name der Sybase-Datenbank. |Ja |
| schema | Name des Schemas in der Datenbank. |Nein |
| authenticationType | Typ der Authentifizierung für die Verbindung mit der Sybase-Datenbank.<br/>Zulässige Werte: **Basic** und **Windows**. |Ja |
| username | Geben Sie einen Benutzernamen für das Herstellen der Verbindung mit der Sybase-Datenbank an. |Ja |
| password | Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. Legen Sie für dieses Feld „SecureString“ fest. |Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Eine selbstgehostete Integrationslaufzeit ist erforderlich, wie unter [Voraussetzungen](#prerequisites) erwähnt wird. |Ja |

**Beispiel:**

```json
{
    "name": "SybaseLinkedService",
    "properties": {
        "type": "Sybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Dataset „Sybase“ unterstützt werden.

Legen Sie zum Kopieren von Daten aus Sybase die „type“-Eigenschaft des Datasets auf **RelationalTable** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die „type“-Eigenschaft des Datasets muss auf **RelationalTable** festgelegt werden. | Ja |
| tableName | Name der Tabelle in der Sybase-Datenbank. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

**Beispiel**

```json
{
    "name": "SybaseDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Sybase linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Quelle „Sybase“ unterstützt werden.

### <a name="sybase-as-source"></a>Sybase als Quelle

Legen Sie zum Kopieren von Daten aus Sybase den Quellentyp in der Kopieraktivität auf **RelationalSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **RelationalSource** festgelegt werden. | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"SELECT * FROM MyTable"`. | Nein (wenn „tableName“ in „dataset“ angegeben ist) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromSybase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Sybase input dataset name>",
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

## <a name="data-type-mapping-for-sybase"></a>Datentypzuordnung für Sybase

Beim Kopieren von Daten aus Sybase werden die folgenden Zuordnungen von Sybase-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

Sybase unterstützt T-SQL-Typen. Informationen zu einer Tabellen mit Zuordnungen von SQL-Typen zu Azure Data Factory-Zwischendatentypen finden Sie im Abschnitt [Azure SQL-Datenbank-Connector: Datentypzuordnung](connector-azure-sql-database.md#data-type-mapping-for-azure-sql-database).


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
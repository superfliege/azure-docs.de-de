---
title: Kopieren von Daten aus Netezza mithilfe von Azure Data Factory (Beta) | Microsoft-Dokumentation
description: Erfahren Sie, wie Daten aus Netezza mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: jingwang
ms.openlocfilehash: 469e72a70d23b3d23eeeb68b3aa2a9e3527d038e
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33940136"
---
# <a name="copy-data-from-netezza-using-azure-data-factory-beta"></a>Kopieren von Daten aus Netezza mithilfe von Azure Data Factory (Beta)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Netezza zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Kopieraktivität in V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Dieser Connector ist aktuell in der Betaphase. Sie können ihn ausprobieren und uns Feedback geben. Verwenden Sie ihn nicht in Produktionsumgebungen.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Netezza in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory enthält einen integrierten Treiber zum Sicherstellen der Konnektivität. Daher müssen Sie mit diesem Connector keinen Treiber manuell installieren.

## <a name="getting-started"></a>Erste Schritte

Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Netezza-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Netezza verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **Netezza** festgelegt werden. | Ja |
| connectionString | Eine ODBC-Verbindungszeichenfolge zum Herstellen einer Verbindung mit Netezza. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die selbstgehostete Integration Runtime oder Azure Integration Runtime verwenden (sofern Ihr Datenspeicher öffentlich zugänglich ist). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

Eine typische Verbindungszeichenfolge ist `Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>`. Weitere Eigenschaften, die Sie für Ihren Fall festlegen können:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |:--- |
| SecurityLevel | Der Sicherheitsgrad (SSL/TLS), den der Treiber für die Verbindung mit dem Datenspeicher verwendet. Beispiel: `SecurityLevel=preferredSecured`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Folgende Werte werden unterstützt:<br/>- Nur ungesichert (**OnlyUnSecured**): Der Treiber verwendet kein SSL.<br/>- Bevorzugt ungesichert (**PreferredUnSecured**) (Standardeinstellung): Wenn der Server eine Wahl bietet, verwendet der Treiber kein SSL. <br/>- Bevorzugt gesichert (**PreferredSecured**): Wenn der Server eine Wahl bietet, verwendet der Treiber SSL. <br/>- Nur gesichert (**OnlySecured**): Der Treiber stellt nur eine Verbindung her, wenn eine SSL-Verbindung verfügbar ist. | Nein  |
| CaCertFile | Der vollständige Pfad zum vom Server verwendeten SSL-Zertifikat. Beispiel: `UseSystemTrustStore=<cert path>;`| Ja, wenn SSL aktiviert ist |

**Beispiel:**

```json
{
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Netezza-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus Netezza die type-Eigenschaft des Datasets auf **NetezzaTable** fest. Bei diesem Dataset-Typ gibt es keine zusätzliche typspezifische Eigenschaft.

**Beispiel**

```json
{
    "name": "NetezzaDataset",
    "properties": {
        "type": "NetezzaTable",
        "linkedServiceName": {
            "referenceName": "<Netezza linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Netezza-Quelle unterstützt werden.

### <a name="netezza-as-source"></a>Netezza als Quelle

Legen Sie zum Kopieren von Daten aus Netezza den Quelltyp in der Kopieraktivität auf **NetezzaSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **NetezzaSource** festgelegt werden. | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"SELECT * FROM MyTable"`. | Ja |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromNetezza",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Netezza input dataset name>",
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
                "type": "NetezzaSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

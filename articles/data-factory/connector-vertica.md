---
title: Kopieren von Daten aus Vertica mithilfe von Azure Data Factory (Beta) | Microsoft-Dokumentation
description: Erfahren Sie, wie Daten aus Vertica mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: jingwang
ms.openlocfilehash: 56be714224ccdaf9af1afcb18e24a85bed7522b4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34617850"
---
# <a name="copy-data-from-vertica-using-azure-data-factory-beta"></a>Kopieren von Daten aus Vertica mithilfe von Azure Data Factory (Beta)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Vertica zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Kopieraktivität in V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Dieser Connector ist aktuell in der Betaphase. Sie können ihn ausprobieren und uns Feedback geben. Verwenden Sie ihn nicht in Produktionsumgebungen.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Vertica in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory enthält einen integrierten Treiber zum Sicherstellen der Konnektivität. Daher müssen Sie mit diesem Connector keinen Treiber manuell installieren.

## <a name="getting-started"></a>Erste Schritte

Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Vertica-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Vertica verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **Vertica** festgelegt werden. | Ja |
| connectionString | Eine ODBC-Verbindungszeichenfolge zum Herstellen einer Verbindung mit Vertica. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die selbstgehostete Integration Runtime oder Azure Integration Runtime verwenden (sofern Ihr Datenspeicher öffentlich zugänglich ist). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

**Beispiel:**

```json
{
    "name": "VerticaLinkedService",
    "properties": {
        "type": "Vertica",
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die vom Vertica-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus Vertica die type-Eigenschaft des Datasets auf **VerticaTable** fest. Bei diesem Dataset-Typ gibt es keine zusätzliche typspezifische Eigenschaft.

**Beispiel**

```json
{
    "name": "VerticaDataset",
    "properties": {
        "type": "VerticaTable",
        "linkedServiceName": {
            "referenceName": "<Vertica linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die von der Vertica-Quelle unterstützt werden.

### <a name="vertica-as-source"></a>Vertica als Quelle

Legen Sie zum Kopieren von Daten aus Vertica den Quelltyp in der Kopieraktivität auf **VerticaSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **VerticaSource** festgelegt werden. | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"SELECT * FROM MyTable"`. | Ja |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromVertica",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Vertica input dataset name>",
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
                "type": "VerticaSource",
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

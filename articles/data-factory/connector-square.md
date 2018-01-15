---
title: Kopieren von Daten aus Square mithilfe von Azure Data Factory (Beta) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten aus Square mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
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
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: 71c73f034a3c32c9bdf1532106a3aba9521afb17
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="copy-data-from-square-using-azure-data-factory-beta"></a>Kopieren von Daten aus Square mithilfe von Azure Data Factory (Beta)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Square zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Kopieraktivität in V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Dieser Connector ist aktuell in der Betaphase. Sie können ihn ausprobieren und uns Feedback geben. Verwenden Sie ihn nicht in Produktionsumgebungen.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Square in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory enthält einen integrierten Treiber zum Sicherstellen der Konnektivität. Daher müssen Sie mit diesem Connector keinen Treiber manuell installieren.

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Square-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Square verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **Square** festgelegt werden. | Ja |
| host | Die URL der Square-Instanz. (mystore.mysquare.com)  | Ja |
| clientId | Die Client-ID, die Ihrer Square-Anwendung zugeordnet ist.  | Ja |
| clientSecret | Der geheime Clientschlüssel, der Ihrer Square-Anwendung zugeordnet ist. Sie können dieses Feld optional als SecureString markieren, um es sicher in ADF zu speichern, oder dieses Kennwort in Azure Key Vault speichern und von dort von der Kopieraktivität abrufen lassen, wenn Datenkopiervorgänge durchgeführt werden. Weitere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). | Ja |
| redirectUri | Die Umleitungs-URL, die im Dashboard der Square-Anwendung zugewiesen ist. (http://localhost:2500)  | Ja |
| useEncryptedEndpoints | Gibt an, ob die Endpunkte der Datenquelle mit HTTPS verschlüsselt sind. Der Standardwert lautet „true“.  | Nein  |
| useHostVerification | Gibt an, ob der Hostname im Zertifikat des Servers mit dem Hostnamen des Servers übereinstimmen muss, wenn eine Verbindung über SSL hergestellt wird. Der Standardwert lautet „true“.  | Nein  |
| usePeerVerification | Gibt an, ob die Identität des Servers bei Verbindung über SSL überprüft werden soll. Der Standardwert lautet „true“.  | Nein  |

**Beispiel:**

```json
{
    "name": "SquareLinkedService",
    "properties": {
        "type": "Square",
        "typeProperties": {
            "host" : "mystore.mysquare.com",
            "clientId" : "<clientId>",
            "clientSecret": {
                 "type": "SecureString",
                 "value": "<clientSecret>"
            },
            "redirectUri" : "http://localhost:2500"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Square-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus Square die „type“-Eigenschaft des Datasets auf **SquareObject** fest. Bei diesem Dataset-Typ gibt es keine zusätzliche typspezifische Eigenschaft.

**Beispiel**

```json
{
    "name": "SquareDataset",
    "properties": {
        "type": "SquareObject",
        "linkedServiceName": {
            "referenceName": "<Square linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Square-Quelle unterstützt werden.

### <a name="squaresource-as-source"></a>SquareSource als Quelle

Legen Sie zum Kopieren von Daten aus Square den Quellentyp in der Kopieraktivität auf **SquareSourceSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **SquareSource** festgelegt werden. | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"SELECT * FROM Business"`. | Ja |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromSquare",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Square input dataset name>",
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
                "type": "SquareSource",
                "query": "SELECT * FROM Business"
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

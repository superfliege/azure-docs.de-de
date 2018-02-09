---
title: Kopieren von Daten aus Google BigQuery mithilfe von Azure Data Factory (Betaversion) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten aus Google BigQuery mithilfe einer Kopieraktivität in eine Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
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
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: 3b559e64f38727b1e390160515b7614ad1dfaa97
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory-beta"></a>Kopieren von Daten aus Google BigQuery mithilfe von Azure Data Factory (Betaversion)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Google BigQuery zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die Data Factory-Version 1 verwenden, die allgemein verfügbar ist, lesen Sie [Verschieben von Daten mit der Kopieraktivität](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Dieser Connector befindet sich aktuell in der Betaversion. Sie können ihn ausprobieren und uns Feedback geben. Verwenden Sie ihn nicht in Produktionsumgebungen.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Google BigQuery in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

 Data Factory bietet zum Herstellen von Konnektivität einen integrierten Treiber. Aus diesem Grund müssen Sie Treiber für die Verwendung dieses Connectors nicht manuell installieren.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten verwendet werden, die für den Google BigQuery-Connector spezifisch sind.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Google BigQuery verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **GoogleBigQuery** festgelegt werden. | Ja |
| project | Das Projekt-ID des BigQuery-Standardprojekts, das abgefragt werden soll.  | Ja |
| additionalProjects | Eine durch Trennzeichen getrennte Liste mit Projekt-IDs öffentlicher BigQuery-Projekte, auf die zugegriffen werden soll.  | Nein  |
| requestGoogleDriveScope | Ob Zugriff auf Google Drive angefordert werden soll. Das Zulassen des Zugriffs auf Google Drive ermöglicht die Unterstützung von Verbundtabellen, die BigQuery-Daten mit Daten auf Google Drive kombinieren. Der Standardwert ist **false**.  | Nein  |
| authenticationType | Der OAuth 2.0-Authentifizierungsmechanismus, der für die Authentifizierung verwendet wird. „ServiceAuthentication“ kann nur für eine selbstgehostete Integration Runtime verwendet werden. <br/>Zulässige Werte: **ServiceAuthentication** und **UserAuthentication**. | Ja |
| refreshToken | Das Aktualisierungstoken, das von Google für die Autorisierung des Zugriffs auf BigQuery für „UserAuthentication“ abgerufen wird. Sie können dieses Feld als SecureString markieren, um es sicher in Data Factory zu speichern. Bei der Durchführung des Datenkopiervorgangs können Sie das Kennwort auch in Azure Key Vault speichern und die Kopieraktivität darüber erfolgen lassen. Weitere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). | Nein  |
| E-Mail | Die E-Mail-ID des Dienstkontos, die für „ServiceAuthentication“ verwendet wird. Diese kann nur für die selbstgehostete Integration Runtime verwendet werden.  | Nein  |
| keyFilePath | Der vollständige Pfad zur P12-Schlüsseldatei, die zur Authentifizierung der E-Mail-Adresse des Dienstkontos verwendet wird. Diese kann nur für die selbstgehostete Integration Runtime verwendet werden.  | Nein  |
| trustedCertPath | Der vollständige Pfad der PEM-Datei mit vertrauenswürdigen Zertifizierungsstellenzertifikaten zur Überprüfung des Servers beim Verbindungsaufbau über SSL. Diese Eigenschaft kann nur festgelegt werden, wenn Sie SSL für die selbstgehostete Integration Runtime verwenden. Der Standardwert ist die Datei „cacerts.pem“, die mit der Integration Runtime installiert wird.  | Nein  |
| useSystemTrustStore | Gibt an, ob ein Zertifizierungsstellenzertifikat aus dem Vertrauensspeicher des Systems oder aus einer angegebenen PEM-Datei verwendet werden soll. Der Standardwert ist **false**.  | Nein  |

**Beispiel:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project ID>",
            "additionalProjects" : "<additional project IDs>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "refreshToken": {
                 "type": "SecureString",
                 "value": "<refresh token>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Google BigQuery-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus Google BigQuery die „type“-Eigenschaft des Datasets auf **GoogleBigQueryObject** fest. Bei diesem Dataset-Typ gibt es keine zusätzliche typspezifische Eigenschaft.

**Beispiel**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Google BigQuery-Quelltyp unterstützt werden.

### <a name="googlebigquerysource-as-a-source-type"></a>GoogleBigQuerySource als Quelltyp

Legen Sie zum Kopieren von Daten aus Google BigQuery den Quelltyp in der Kopieraktivität auf **GoogleBigQuerySource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **GoogleBigQuerySource** festgelegt werden. | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `"SELECT * FROM MyTable"`. | Ja |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
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
                "type": "GoogleBigQuerySource",
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
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

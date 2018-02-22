---
title: Kopieren von Daten aus Impala mithilfe von Azure Data Factory (Betaversion) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten aus Impala mithilfe einer Kopieraktivität in eine Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: e1f745fc70395f06d2eb3d98644d54c314a0ef26
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-from-impala-by-using-azure-data-factory-beta"></a>Kopieren von Daten aus Impala mithilfe von Azure Data Factory (Betaversion)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Impala zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die Data Factory-Version 1 verwenden, die allgemein verfügbar ist, lesen Sie [Verschieben von Daten mit der Kopieraktivität](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Dieser Connector befindet sich aktuell in der Betaversion. Sie können ihn ausprobieren und Feedback geben. Verwenden Sie ihn nicht in Produktionsumgebungen.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Impala in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

 Data Factory bietet zum Herstellen von Konnektivität einen integrierten Treiber. Aus diesem Grund müssen Sie Treiber für die Verwendung dieses Connectors nicht manuell installieren.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Impala-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Impala verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **Impala** festgelegt werden. | Ja |
| host | Die IP-Adresse oder der Hostname des Impala-Servers (d.h. 192.168.222.160)  | Ja |
| port | Der TCP-Port, den der Impala-Server verwendet, um auf Clientverbindungen zu lauschen. Der Standardwert ist 21050.  | Nein  |
| authenticationType | Der zu verwendende Authentifizierungstyp. <br/>Zulässige Werte: **Anonymous**, **SASLUsername** und **UsernameAndPassword**. | Ja |
| username | Der Benutzername für den Zugriff auf den Impala-Server. Der Standardwert ist „Anonymous“, wenn „SASLUsername“ verwendet wird.  | Nein  |
| password | Das Kennwort, das dem Benutzernamen entspricht, wenn Sie „UsernameAndPassword“ verwenden. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Nein  |
| enableSsl | Gibt an, ob die Verbindungen mit dem Server mit dem Server mittels SSL verschlüsselt werden soll. Der Standardwert ist **false**.  | Nein  |
| trustedCertPath | Der vollständige Pfad der PEM-Datei mit vertrauenswürdigen Zertifizierungsstellenzertifikaten zur Überprüfung des Servers beim Verbindungsaufbau über SSL. Diese Eigenschaft kann nur festgelegt werden, wenn Sie SSL für die selbstgehostete Integration Runtime verwenden. Der Standardwert ist die Datei „cacerts.pem“, die mit der Integration Runtime installiert wird.  | Nein  |
| useSystemTrustStore | Gibt an, ob ein Zertifizierungsstellenzertifikat aus dem Vertrauensspeicher des Systems oder aus einer angegebenen PEM-Datei verwendet werden soll. Der Standardwert ist **false**.  | Nein  |
| allowHostNameCNMismatch | Gibt an, ob ein Name für das von der Zertifizierungsstelle ausgestellte SSL-Zertifikat erforderlich ist, der mit dem Hostnamen des Servers übereinstimmt, wenn eine Verbindung über SSL hergestellt wird. Der Standardwert ist **false**.  | Nein  |
| allowSelfSignedServerCert | Gibt an, ob vom Server selbstsignierte Zertifikate zugelassen werden. Der Standardwert ist **false**.  | Nein  |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die selbstgehostete Integration Runtime oder Azure Integration Runtime verwenden (sofern Ihr Datenspeicher öffentlich zugänglich ist). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

**Beispiel:**

```json
{
    "name": "ImpalaLinkedService",
    "properties": {
        "type": "Impala",
        "typeProperties": {
            "host" : "<host>",
            "port" : "<port>",
            "authenticationType" : "UsernameAndPassword",
            "username" : "<username>",
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Impala-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus Impala die „type“-Eigenschaft des Datasets auf **ImpalaObject** fest. Bei diesem Dataset-Typ gibt es keine zusätzliche typspezifische Eigenschaft.

**Beispiel**

```json
{
    "name": "ImpalaDataset",
    "properties": {
        "type": "ImpalaObject",
        "linkedServiceName": {
            "referenceName": "<Impala linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Impala-Quelltyp unterstützt werden.

### <a name="impala-as-a-source-type"></a>Impala als Quelltyp

Legen Sie zum Kopieren von Daten aus der Impala den Quelltyp in der Kopieraktivität auf **ImpalaSource** fest. Die folgenden Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **ImpalaSource** festgelegt werden. | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `"SELECT * FROM MyTable"`. | Ja |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromImpala",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Impala input dataset name>",
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
                "type": "ImpalaSource",
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

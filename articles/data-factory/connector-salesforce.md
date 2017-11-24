---
title: Kopieren von Daten aus/nach Salesforce mit Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe einer Kopieraktivität in einer Azure Data Factory-Pipeline Daten aus Salesforce in unterstützte Senkendatenspeicher (ODER) aus unterstützten Quelldatenspeichern nach Salesforce kopieren."
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
ms.date: 11/09/2017
ms.author: jingwang
ms.openlocfilehash: 017d03b76bd19a0b3a1e19c22233c61be9067d0d
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="copy-data-fromto-salesforce-using-azure-data-factory"></a>Kopieren von Daten aus/nach Salesforce mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-salesforce-connector.md)
> * [Version 2 – Vorschauversion](connector-salesforce.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus und nach Salesforce kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (General Availability, GA) ist, lesen Sie [Salesforce-Connector in V1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Salesforce in jeden unterstützten Senkendatenspeicher bzw. Daten aus jedem unterstützten Quelldatenspeicher nach Salesforce kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Salesforce-Connector unterstützt insbesondere Folgendes:

- Die folgenden Editionen von Salesforce: **Developer Edition, Professional Edition, Enterprise Edition oder Unlimited Edition**.
- Datenkopiervorgänge aus der bzw. in die **Produktionsumgebung, den Sandkasten und die benutzerdefinierte Domäne** von Salesforce.

## <a name="prerequisites"></a>Voraussetzungen

* API-Berechtigungen müssen in Salesforce aktiviert sein. Informationen hierzu finden Sie unter [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Anforderungslimits in Salesforce

Salesforce weist Grenzwerte sowohl für die Gesamtanzahl von API-Anforderungen als auch für die Anzahl gleichzeitiger API-Anforderungen auf. Beachten Sie folgende Punkte:

- Wenn die Anzahl von gleichzeitigen Anforderungen das Limit überschreitet, setzt eine Drosselung ein, und es werden zufällig generierte Fehler angezeigt.
- Wenn die Gesamtanzahl von Anforderungen das Limit überschreitet, wird das Salesforce-Konto 24 Stunden lang gesperrt.

In beiden Szenarien erhalten Sie möglicherweise auch den Fehler „REQUEST_LIMIT_EXCEEDED“. Weitere Informationen finden Sie im Abschnitt „API Request Limits“ (API-Anforderungslimits) im Artikel [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) (Salesforce-Entwicklerlimits).

## <a name="getting-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Salesforce-Connector verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Salesforce verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ |Die type-Eigenschaft muss auf **Salesforce**festgelegt sein. |Ja |
| environmentUrl | Geben Sie die URL der Salesforce-Instanz an. <br> – Der Standardwert ist `"https://login.salesforce.com"`. <br> – Um Daten aus einem Sandkasten zu kopieren, geben Sie `"https://test.salesforce.com"` an. <br> – Geben Sie zum Kopieren von Daten aus einer benutzerdefinierten Domäne z.B. `"https://[domain].my.salesforce.com"` an. |Nein |
| username |Geben Sie einen Benutzernamen für das Benutzerkonto an. |Ja |
| password |Geben Sie ein Kennwort für das Benutzerkonto an.<br/><br/>Sie können dieses Feld optional als SecureString markieren, um es sicher in ADF zu speichern, oder dieses Kennwort in Azure Key Vault speichern und von dort von der ADF-Kopieraktivität abrufen lassen, wenn Datenkopiervorgänge durchgeführt werden. Weitere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). |Ja |
| securityToken |Geben Sie ein Sicherheitstoken für das Benutzerkonto an. Anweisungen zum Abrufen oder Zurücksetzen eines Sicherheitstokens finden Sie unter [Zurücksetzen Ihres Sicherheitstokens](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) . Allgemeine Informationen zu Sicherheitstoken finden Sie unter [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm)(Sicherheit und die API).<br/><br/>Sie können dieses Feld optional als SecureString markieren, um es sicher in ADF zu speichern, oder dieses Sicherheitstoken in Azure Key Vault speichern und von dort von der ADF-Kopieraktivität abrufen lassen, wenn Datenkopiervorgänge durchgeführt werden. Weitere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. | Quelle: Nein, Senke: Ja |

>[!IMPORTANT]
>Um Daten nach Salesforce zu kopieren, erstellen Sie explizit [eine Azure IR](create-azure-integration-runtime.md#create-azure-ir) mit einem Standort in der Nähe Ihres Salesforce-Standorts, und nehmen Sie eine Zuordnung im verknüpften Dienst wie im folgenden Beispiel vor.

**Beispiel: Speichern einer Anmeldeinformation in ADF**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel: Speichern einer Anmeldeinformation in Azure Key Vault**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Salesforce-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus/nach Salesforce die type-Eigenschaft des Datasets auf **SalesforceObject** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **SalesforceObject**festgelegt sein.  | Ja |
| objectApiName | Der Name des Salesforce-Objekts, aus dem Daten abgerufen werden sollen. | Quelle: Nein, Senke: Ja |

> [!IMPORTANT]
> Der Abschnitt „__c“ von „API Name“ wird für benutzerdefinierte Objekte benötigt.

![Data Factory – Salesforce-Verbindung – API-Name](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Beispiel:**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "objectApiName": "MyTable__c"
        }
    }
}
```

>[!NOTE]
>Zur Abwärtskompatibilität funktioniert beim Kopieren von Daten aus Salesforce weiterhin das vorherige Dataset vom Typ „RelationalTable“. Ihnen wird jedoch empfohlen, auf den neuen Typ „SalesforceObject“ umzustellen.

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die „type“-Eigenschaft des Datasets muss auf **RelationalTable** festgelegt werden. | Ja |
| tableName | Name der Tabelle in Salesforce. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Salesforce-Quelle und -Senke unterstützt werden.

### <a name="salesforce-as-source"></a>Salesforce als Quelle

Legen Sie zum Kopieren von Daten aus Salesforce den Quelltyp in der Kopieraktivität auf **SalesforceSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **SalesforceSource** festgelegt werden. | Ja |
| query |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. Sie können eine SQL-92-Abfrage oder eine Abfrage vom Typ [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) verwenden. Beispiel: `select * from MyTable__c`. | Nein (wenn „tableName“ im Dataset angegeben ist) |

> [!IMPORTANT]
> Der Abschnitt „__c“ von „API Name“ wird für benutzerdefinierte Objekte benötigt.

![Data Factory – Salesforce-Verbindung – API-Name](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Zur Abwärtskompatibilität funktioniert beim Kopieren von Daten aus Salesforce weiterhin die vorherige Kopierquelle vom Typ „RelationalSource“. Ihnen wird jedoch empfohlen, auf den neuen Typ „SalesforceSource“ umzustellen.

### <a name="salesforce-as-sink"></a>Salesforce als Senke

Legen Sie zum Kopieren von Daten nach Salesforce den Senkentyp in der Kopieraktivität auf **SalesforceSink** fest. Folgende Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **SalesforceSink** festgelegt werden. | Ja |
| writeBehavior | Das Schreibverhalten für den Vorgang.<br/>Zulässige Werte: **Insert** und **Upsert**. | Nein (Standardwert ist „Insert“) |
| externalIdFieldName | Der Name des externen ID-Felds für den upsert-Vorgang. Das angegebene Feld muss im Salesforce-Objekt als „External Id Field“ definiert werden und darf in den entsprechenden Eingabedaten keine NULL-Werte aufweisen. | Ja für „Upsert“ |
| writeBatchSize | Die Zeilenanzahl der Daten, die in jedem Batch in Salesforce geschrieben werden. | Nein (Standardwert ist 5000) |
| ignoreNullValues | Gibt an, ob Nullwerte aus Eingabedaten während des Schreibvorgangs ignoriert werden sollen.<br/>Zulässige Werte: **true** und **false**.<br>- **true**: Lassen Sie die Daten im Zielobjekt unverändert, wenn Sie einen upsert-/update-Vorgang durchführen, und fügen Sie einen definierten Standardwert ein, wenn Sie einen insert-Vorgang durchführen.<br/>- **false**: Aktualisieren Sie die Daten im Zielobjekt zu NULL, wenn Sie einen upsert-/update-Vorgang durchführen, und fügen Sie einen NULL-Wert ein, wenn Sie einen insert-Vorgang durchführen. | Nein (Standardwert ist „false“) |

### <a name="example-salesforce-sink-in-copy-activity"></a>Beispiel: Salesforce-Senke in Kopieraktivität

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Tipps zu Abfragen

### <a name="retrieving-data-from-salesforce-report"></a>Abrufen von Daten aus Salesforce-Bericht

Sie können Daten aus Salesforce-Berichten abrufen, indem Sie die Abfrage `{call "<report name>"}` angeben. Beispiel: `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Abrufen von gelöschten Datensätzen aus dem Salesforce-Papierkorb

Zum Abfragen der vorläufig gelöschten Datensätze aus dem Salesforce-Papierkorb können Sie in der Abfrage **„IsDeleted = 1“** angeben. Beispiel:

* Zum Abfragen lediglich der gelöschten Datensätze geben Sie „select * from MyTable__c **where IsDeleted= 1**“ an.
* Zum Abfragen aller Datensätze, d.h. der vorhandenen und der gelöschten Datensätze, geben Sie „select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**“ an.

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Abrufen von Daten mithilfe der WHERE-Klausel für die DateTime-Spalte

Achten Sie beim Angeben der SOQL- oder SQL-Abfrage auf den Unterschied beim DateTime-Format. Beispiel:

* **SOQL-Beispiel**: `SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL-Beispiel**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}"`

## <a name="data-type-mapping-for-salesforce"></a>Datentypzuordnung für Salesforce

Beim Kopieren von Daten aus Salesforce werden die folgenden Zuordnungen von Salesforce-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

| Salesforce-Datentyp | Data Factory-Zwischendatentyp |
|:--- |:--- |
| Auto Number |String |
| Kontrollkästchen |Boolean |
| Currency |Doppelt |
| Datum |DateTime |
| Datum/Uhrzeit |DateTime |
| E-Mail |String |
| ID |String |
| Lookup Relationship |String |
| Multi-Select Picklist |String |
| Number |Doppelt |
| Prozent |Doppelt |
| Phone |String |
| Picklist |String |
| Text |String |
| Text Area |String |
| Text Area (Long) |String |
| Text Area (Rich) |String |
| Text (Encrypted) |String |
| URL |String |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
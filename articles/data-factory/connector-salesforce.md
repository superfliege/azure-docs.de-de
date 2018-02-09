---
title: Kopieren von Daten aus und nach Salesforce mit Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe einer Kopieraktivität in einer Data Factory-Pipeline Daten aus Salesforce in unterstützte Senkendatenspeicher oder aus unterstützten Quelldatenspeichern nach Salesforce kopieren."
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
ms.openlocfilehash: 4a6138f0927f9761677d6da1ae05546286ad3898
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="copy-data-from-and-to-salesforce-by-using-azure-data-factory"></a>Kopieren von Daten aus und nach Salesforce mit Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [ Version 1 – allgemein verfügbar](v1/data-factory-salesforce-connector.md)
> * [Version 2 – Vorschauversion](connector-salesforce.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus und nach Salesforce zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die Data Factory-Version 1 verwenden, die allgemein verfügbar ist, lesen Sie [Salesforce-Connector in Version 1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Salesforce in beliebige unterstützte Senkendatenspeicher kopieren. Zudem können Sie Daten aus einem beliebigen unterstützten Quelldatenspeicher nach Salesforce kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Salesforce-Connector unterstützt insbesondere Folgendes:

- Salesforce Developer, Professional, Enterprise oder Unlimited Edition.
- Datenkopiervorgänge aus der und in die Produktionsumgebung, den Sandkasten und die benutzerdefinierte Domäne von Salesforce.

## <a name="prerequisites"></a>Voraussetzungen

API-Berechtigungen müssen in Salesforce aktiviert sein. Weitere Informationen finden Sie unter [Enable API access in Salesforce by permission set](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/) (Aktivieren des API-Zugriffs in Salesforce mithilfe eines Berechtigungssatzes).

## <a name="salesforce-request-limits"></a>Anforderungslimits in Salesforce

Salesforce weist Grenzwerte sowohl für die Gesamtanzahl von API-Anforderungen als auch für die Anzahl gleichzeitiger API-Anforderungen auf. Beachten Sie folgende Punkte:

- Wenn die Anzahl von gleichzeitigen Anforderungen das Limit überschreitet, setzt eine Drosselung ein, und es werden zufällig generierte Fehler angezeigt.
- Wenn die Gesamtanzahl von Anforderungen das Limit überschreitet, wird das Salesforce-Konto 24 Stunden lang gesperrt.

In beiden Szenarien erhalten Sie möglicherweise auch die Fehlermeldung „REQUEST_LIMIT_EXCEEDED“. Weitere Informationen finden Sie im Abschnitt „API Request Limits“ (API-Anforderungslimits) im Dokument [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) (Salesforce-Entwicklerlimits).

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Salesforce-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Salesforce verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type |Die type-Eigenschaft muss auf **Salesforce**festgelegt sein. |Ja |
| environmentUrl | Geben Sie die URL der Salesforce-Instanz an. <br> – Der Standardwert ist `"https://login.salesforce.com"`. <br> – Um Daten aus einem Sandkasten zu kopieren, geben Sie `"https://test.salesforce.com"` an. <br> – Geben Sie zum Kopieren von Daten aus einer benutzerdefinierten Domäne z.B. `"https://[domain].my.salesforce.com"` an. |Nein  |
| username |Geben Sie einen Benutzernamen für das Benutzerkonto an. |Ja |
| password |Geben Sie ein Kennwort für das Benutzerkonto an.<br/><br/>Sie können dieses Feld als SecureString markieren, um es sicher in Data Factory zu speichern. Bei der Ausführung des Datenkopiervorgangs können Sie das Kennwort auch in Azure Key Vault speichern und die Kopieraktivität darüber erfolgen lassen. Weitere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). |Ja |
| securityToken |Geben Sie ein Sicherheitstoken für das Benutzerkonto an. Anweisungen zum Abrufen oder Zurücksetzen eines Sicherheitstokens finden Sie unter [Get a security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) (Abrufen eines Sicherheitstokens). Allgemeine Informationen zu Sicherheitstoken finden Sie unter [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm)(Sicherheit und die API).<br/><br/>Sie können dieses Feld als SecureString markieren, um es sicher in Data Factory zu speichern. Bei der Ausführung des Datenkopiervorgangs können Sie das Sicherheitstoken auch in Key Vault speichern und die Kopieraktivität darüber erfolgen lassen. Weitere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. | Nein für die Quelle. Ja für die Senke, wenn der mit der Quelle verknüpfte Dienst keine Integration Runtime aufweist. |

>[!IMPORTANT]
>Beim Kopieren von Daten in Salesforce kann die standardmäßige Azure Integration Runtime nicht zum Ausführen des Kopiervorgangs verwendet werden. Mit anderen Worten, wenn für den mit der Quelle verknüpften Dienst keine Integration Runtime festgelegt wurde, [erstellen Sie explizit eine Azure Integration Runtime](create-azure-integration-runtime.md#create-azure-ir) in der Nähe Ihrer Salesforce-Instanz. Ordnen Sie den mit Salesforce verknüpften Dienst wie im folgenden Beispiel zu.

**Beispiel: Speichern von Anmeldeinformationen in Data Factory**

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

**Beispiel: Speichern von Anmeldeinformationen im Key Vault**

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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Salesforce-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus und nach Salesforce die type-Eigenschaft des Datasets auf **SalesforceObject** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **SalesforceObject** festgelegt sein.  | Ja |
| objectApiName | Der Name des Salesforce-Objekts, aus dem Daten abgerufen werden sollen. | Quelle: Nein, Senke: Ja |

> [!IMPORTANT]
> Der Teil „__c“ von **API Name** wird für benutzerdefinierte Objekte benötigt.

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
>Abwärtskompatibilität: Beim Kopieren von Daten aus Salesforce funktioniert weiterhin das vorherige Dataset vom Typ „RelationalTable“. Ihnen wird jedoch empfohlen, auf den neuen Typ „SalesforceObject“ umzustellen.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **RelationalTable** festgelegt werden. | Ja |
| tableName | Name der Tabelle in Salesforce. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Salesforce-Quelle und -Senke unterstützt werden.

### <a name="salesforce-as-a-source-type"></a>Salesforce als Quelltyp

Legen Sie zum Kopieren von Daten aus Salesforce den Quelltyp in der Kopieraktivität auf **SalesforceSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **SalesforceSource** festgelegt werden. | Ja |
| query |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. Sie können eine SQL-92-Abfrage oder eine Abfrage vom Typ [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) verwenden. Ein Beispiel ist `select * from MyTable__c`. | Nein (wenn „tableName“ im Dataset angegeben ist) |

> [!IMPORTANT]
> Der Teil „__c“ von **API Name** wird für benutzerdefinierte Objekte benötigt.

![Data Factory – Salesforce-Verbindung – Liste der API-Namen](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

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
>Abwärtskompatibilität: Beim Kopieren von Daten aus Salesforce funktioniert die Quelle weiterhin bei Verwendung des Typs „RelationalSource“. Ihnen wird jedoch empfohlen, auf den neuen Typ „SalesforceSource“ umzustellen.

### <a name="salesforce-as-a-sink-type"></a>Salesforce als Senkentyp

Legen Sie zum Kopieren von Daten nach Salesforce den Senkentyp in der Kopieraktivität auf **SalesforceSink** fest. Folgende Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **SalesforceSink** festgelegt werden. | Ja |
| writeBehavior | Das Schreibverhalten für den Vorgang.<br/>Zulässige Werte: **Insert** und **Upsert**. | Nein (Standardwert ist „Insert“) |
| externalIdFieldName | Der Name des externen ID-Felds für den upsert-Vorgang. Das angegebene Feld muss als „Externes ID-Feld“ im Salesforce-Objekt definiert werden. Es kann keine NULL-Werte in den entsprechenden Eingabedaten haben. | Ja für „Upsert“ |
| writeBatchSize | Die Zeilenanzahl der Daten, die in jedem Batch in Salesforce geschrieben werden. | Nein (Standardwert ist 5000) |
| ignoreNullValues | Gibt an, ob NULL-Werte aus Eingabedaten während eines Schreibvorgangs ignoriert werden sollen.<br/>Zulässige Werte: **true** und **false**.<br>- **True**: Lassen Sie die Daten im Zielobjekt unverändert, wenn Sie einen upsert- oder update-Vorgang ausführen. Fügen Sie beim Ausführen eines insert-Vorgangs einen definierten Standardwert ein.<br/>- **False**: Aktualisieren Sie die Daten im Zielobjekt auf NULL, wenn Sie einen upsert- oder update-Vorgang ausführen. Fügen Sie beim Ausführen eines insert-Vorgangs einen NULL-Wert ein. | Nein (Standardwert ist „false“) |

**Beispiel: Salesforce-Senke in einer Kopieraktivität**

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
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

### <a name="retrieve-data-from-a-salesforce-report"></a>Abrufen von Daten aus einem Salesforce-Bericht

Sie können Daten aus Salesforce-Berichten abrufen, indem Sie eine Abfrage als `{call "<report name>"}` angeben. Ein Beispiel ist `"query": "{call \"TestReport\"}"`.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Abrufen von gelöschten Datensätzen aus dem Salesforce-Papierkorb

Zum Abfragen der vorläufig gelöschten Datensätze aus dem Salesforce-Papierkorb können Sie in der Abfrage **„IsDeleted = 1“** angeben. Beispiel: 

* Zum Abfragen lediglich der gelöschten Datensätze geben Sie „select * from MyTable__c **where IsDeleted= 1***“ an.
* Zum Abfragen aller Datensätze, d.h. der vorhandenen und der gelöschten, geben Sie „select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**“ an.

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>Abrufen von Daten mithilfe einer Where-Klausel für die Spalte „DateTime“

Achten Sie beim Angeben der SOQL- oder SQL-Abfrage auf den Unterschied beim DateTime-Format. Beispiel: 

* **SOQL-Beispiel**: `SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL-Beispiel**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}"`

## <a name="data-type-mapping-for-salesforce"></a>Datentypzuordnung für Salesforce

Beim Kopieren von Daten aus Salesforce werden die folgenden Zuordnungen von Salesforce-Datentypen zu Data Factory-Zwischendatentypen verwendet. Weitere Informationen dazu, wie die Kopieraktivität das Quellschema und den Datentyp zur Senke zuordnet, finden Sie unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md).

| Salesforce-Datentyp | Data Factory-Zwischendatentyp |
|:--- |:--- |
| Auto Number |Zeichenfolge |
| Kontrollkästchen |Boolescher Wert |
| Currency |Double |
| Datum |Datetime |
| Datum/Uhrzeit |Datetime |
| E-Mail |Zeichenfolge |
| id |Zeichenfolge |
| Lookup Relationship |Zeichenfolge |
| Multi-Select Picklist |Zeichenfolge |
| Number |Double |
| Prozent |Double |
| Phone |Zeichenfolge |
| Picklist |Zeichenfolge |
| Text |Zeichenfolge |
| Text Area |Zeichenfolge |
| Text Area (Long) |Zeichenfolge |
| Text Area (Rich) |Zeichenfolge |
| Text (Encrypted) |Zeichenfolge |
| URL |Zeichenfolge |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
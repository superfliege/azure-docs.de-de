---
title: Kopieren von Daten in oder aus Azure Cosmos DB (SQL-API) mit Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Data Factory Daten aus unterstützten Quelldatenspeichern in oder aus Azure Cosmos DB in unterstützte Senkenspeicher kopieren können.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: jingwang
ms.openlocfilehash: 16c02f1f47f556f550519feec78e7dd26b302e18
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53103794"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-sql-api-by-using-azure-data-factory"></a>Kopieren von Daten in oder aus Azure Cosmos DB (SQL-API) mithilfe von Azure Data Factory

> [!div class="op_single_selector" title1="Wählen Sie die Version des Data Factory-Dienstes aus, den Sie verwenden:"]
> * [Version 1](v1/data-factory-azure-documentdb-connector.md)
> * [Aktuelle Version](connector-azure-cosmos-db.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus und in Azure Cosmos DB (SQL-API) zu kopieren. Dieser Artikel baut auf dem Artikel zur [Kopieraktivität in Azure Data Factory](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Azure Cosmos DB in einen beliebigen unterstützten Senkendatenspeicher oder Daten aus einem beliebigen unterstützten Quelldatenspeicher nach Azure Cosmos DB kopieren. Eine Liste der Datenspeicher, die die Kopieraktivität als Quellen und Senken unterstützt, finden Sie unter [Unterstützte Datenspeicher und Formate](copy-activity-overview.md#supported-data-stores-and-formats).

Sie können den Azure Cosmos DB-Connector für die folgenden Aufgaben verwenden:

- Kopieren von Daten in die oder aus der [SQL-API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction) von Azure Cosmos DB.
- Schreiben in Azure Cosmos DB als **insert** oder **upsert**.
- Importieren und Exportieren von JSON-Dokumenten in ihrem jeweiligen Zustand oder Kopieren von Daten aus einem tabellarischen Dataset oder in ein tabellarisches Dataset. Die Beispiele zeigen eine SQL-Datenbank und eine CSV-Datei. Informationen zum Kopieren von Daten in ihrem jeweiligen Zustand in bzw. aus JSON-Dateien oder in eine andere bzw. aus einer anderen Azure Cosmos DB-Sammlung finden Sie unter [Importieren oder Exportieren von JSON-Dokumenten](#importexport-json-documents).

Data Factory wird in die [Azure Cosmos DB-BuklExecutor-Bibliothek](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) integriert, um beim Schreiben in Azure Cosmos DB die beste Leistung zu erzielen.

>[!NOTE]
>Dieser Konnektor unterstützt lediglich das Kopieren von Daten in bzw. aus der SQL-API von Cosmos DB.

> [!TIP]
> Das Video [Datenmigration](https://youtu.be/5-SRNiC_qOU) führt Sie schrittweise durch das Kopieren von Daten aus Azure Blob Storage in Azure Cosmos DB. Das Video beschreibt auch Überlegungen zur Leistungsoptimierung beim Erfassen von Daten in Azure Cosmos DB im Allgemeinen.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die Sie zum Definieren von Data Factory-Entitäten verwenden können, die spezifisch für Azure Cosmos DB sind.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den verknüpften Azure Cosmos DB-Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft muss auf **CosmosDb** festgelegt werden. | JA |
| connectionString |Geben Sie die zum Herstellen einer Verbinden mit der Azure Cosmos DB-Datenbank erforderlichen Informationen an.<br /><br />**Hinweis**: Sie müssen Datenbankinformationen in der Verbindungszeichenfolge angeben, wie in den folgenden Beispielen gezeigt. Markieren Sie dieses Feld als Typ **SecureString**, um es sicher in Data Factory zu speichern. Sie können auch [auf ein Geheimnis verweisen, das in Azure Key Vault](store-credentials-in-key-vault.md) gespeichert ist. |JA |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder eine selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn diese Eigenschaft nicht angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

**Beispiel**

```json
{
    "name": "CosmosDbLinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
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

Dieser Abschnitt enthält eine Liste der Eigenschaften, die das Azure Cosmos DB-Dataset unterstützt. 

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie unter [Datasets und verknüpfte Dienste](concepts-datasets-linked-services.md). 

Legen Sie zum Kopieren von Daten aus bzw. in Azure Cosmos DB die **type**-Eigenschaft des Datasets auf **DocumentDbCollection** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft des Datasets muss auf **DocumentDbCollection** festgelegt werden. |JA |
| collectionName |Der Name der Azure Cosmos DB-Dokumentsammlung. |JA |

**Beispiel**

```json
{
    "name": "CosmosDbDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schema per Data Factory

Für schemafreie Datenspeicher wie Azure Cosmos DB leitet die Kopieraktivität das Schema auf eine der in der folgenden Liste beschriebenen Arten ab. Die bewährte Methode besteht darin, die Struktur der Daten im Abschnitt **structure** anzugeben, es sei denn, Sie möchten [JSON-Dokumente in ihrem jeweiligen Zustand importieren oder exportieren](#import-or-export-json-documents).

* Wenn Sie die Struktur der Daten mithilfe der **structure**-Eigenschaft in der Datasetdefinition angeben, betrachtet Data Factory diese Struktur als das Schema. 

    Wenn eine Zeile keinen Wert für eine Spalte enthält, wird ein NULL-Wert als Spaltenwert angegeben.
* Wenn Sie die Struktur der Daten nicht mithilfe der **structure**-Eigenschaft in der Datasetdefinition angeben, leitet der Data Factory-Dienst das Schema unter Verwendung der ersten Zeile in den Daten ab. 

    Wenn die erste Zeile nicht das vollständige Schema enthält, fehlen im Ergebnis des Kopiervorgangs einige Spalten.

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Azure Cosmos DB-Quelle und -Senke unterstützt werden.

Eine vollständige Liste mit den verfügbaren Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie unter [Pipelines](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-as-source"></a>Azure Cosmos DB als Quelle

Legen Sie zum Kopieren von Daten aus Azure Cosmos DB den **Quelltyp** in der Kopieraktivität auf **DocumentDbCollectionSource** fest. 

Die folgenden Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft der Quelle der Kopieraktivität muss auf **DocumentDbCollectionSource** festgelegt werden. |JA |
| query |Geben Sie die Azure Cosmos DB-Abfrage an, um Daten zu lesen.<br/><br/>Beispiel:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nein  <br/><br/>Falls nicht angegeben, wird die folgende SQL-Anweisung ausgeführt: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Ein Sonderzeichen, das angibt, dass das Dokument geschachtelt ist und das Resultset vereinfacht werden soll.<br/><br/>Wenn eine Azure Cosmos DB-Abfrage beispielsweise das geschachtelte Ergebnis `"Name": {"First": "John"}` zurückgibt, identifiziert die Kopieraktivität den Spaltennamen als `Name.First` mit dem Wert „John“, wenn der Wert von **nestedSeparator**  **ist.** (Punkt). |Nein <br />Der Standardwert ist **.** (Punkt)) |

**Beispiel**

```json
"activities":[
    {
        "name": "CopyFromCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Document DB input dataset name>",
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
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-as-sink"></a>Azure Cosmos DB als Senke

Legen Sie zum Kopieren von Daten in Azure Cosmos DB den **Senkentyp** in der Kopieraktivität auf **DocumentDbCollectionSink** fest. 

Die folgenden Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft der Senke der Kopieraktivität muss auf **DocumentDbCollectionSink** festgelegt werden. |JA |
| writeBehavior |Beschreibt, wie Daten in Azure Cosmos DB geschrieben werden. Zulässige Werte: **insert** und **upsert**.<br/><br/>Das Verhalten von **upsert** besteht darin, das Dokument zu ersetzen, wenn ein Dokument mit der gleichen ID bereits vorhanden ist. Andernfalls wird das Dokument eingefügt.<br /><br />**Hinweis**: Data Factory generiert automatisch eine ID für ein Dokument, wenn eine ID weder im Originaldokument noch durch Spaltenzuordnung angegeben wird. Dies bedeutet, dass Sie sicherstellen müssen, dass Ihr Dokument eine ID besitzt, damit **upsert** wie erwartet funktioniert. |Nein <br />(der Standardwert ist **insert**) |
| writeBatchSize | Data Factory verwendet die [Azure Cosmos DB-BulkExecutor-Bibliothek](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) zum Schreiben von Daten in Azure Cosmos DB. Die **writeBatchSize**-Eigenschaft steuert die Größe der in der Bibliothek bereitgestellten Dokumente. Sie können versuchen, den Wert für **writeBatchSize** zu erhöhen, um die Leistung zu verbessern, oder den Wert verringern, falls Ihre Dokumente groß sind. Weitere Tipps finden Sie weiter unten. |Nein <br />(der Standardwert ist **10.000**) |
| nestingSeparator |Ein Sonderzeichen im **Quell**spaltennamen, um anzugeben, dass ein geschachteltes Dokument erforderlich ist. <br/><br/>Beispielsweise generiert `Name.First` in der Struktur des Ausgabedatasets folgende JSON-Struktur im Azure Cosmos DB-Dokument, sofern es sich bei **nestedSeparator** um einen  **handelt.** (Punkt): `"Name": {"First": "[value maps to this column from source]"}`  |Nein <br />Der Standardwert ist **.** (Punkt)) |

>[!TIP]
>Cosmos DB begrenzt die Größe der einzelnen Anforderung auf 2 MB. Die Formel lautet: Anforderungsgröße = Einzeldokumentgröße * Schreibbatchgröße. Sollte ein Fehler mit dem Hinweis auftreten, dass die **Anforderung zu groß ist**, **verringern Sie den `writeBatchSize`-Wert** in der Kopiersenkenkonfiguration.

**Beispiel**

```json
"activities":[
    {
        "name": "CopyToCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="import-or-export-json-documents"></a>Importieren oder Exportieren von JSON-Dokumenten

Sie können diesen Azure Cosmos DB-Connector verwenden, um die folgenden Aufgaben auf einfache Weise zu erledigen:

* Importieren von JSON-Dokumenten aus verschiedenen Quellen in Azure Cosmos DB, z.B. aus Azure Blob Storage, Azure Data Lake Storage und anderen dateibasierten Speichern, die von Azure Data Factory unterstützt werden.
* Exportieren von JSON-Dokumenten aus einer Azure Cosmos DB-Sammlung in verschiedene dateibasierte Speicher.
* Kopieren von Dokumenten in ihrem jeweiligen Zustand zwischen zwei Azure Cosmos DB-Sammlungen.

So erhalten Sie eine vom Schema unabhängige Kopie:

* Aktivieren Sie bei Verwendung des Tools zum Kopieren von Daten die Option **Export as-is to JSON files or Cosmos DB collection** (Wie vorhanden in JSON-Dateien oder Cosmos DB-Sammlung exportieren).
* Wenn Sie Aktivitätserstellung verwenden, geben Sie den Abschnitt **structure** (auch *schema* genannt) im Azure Cosmos DB-Dataset nicht an. Geben Sie die **nestingSeparator**-Eigenschaft in der Azure Cosmos DB-Quelle oder -Senke in der Kopieraktivität ebenfalls nicht an. Wenn Sie aus JSON-Dateien importieren oder in JSON-Dateien exportieren, geben Sie im entsprechenden Dataset des Dateispeichers den Typ **format** als **JsonFormat** an, und konfigurieren Sie das **filePattern** wie im Abschnitt [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format) beschrieben. Geben Sie dann den Abschnitt **structure** nicht an, und überspringen Sie die restlichen Formateinstellungen.

## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Datenspeicher, die von der Kopieraktivität als Quellen und Senken in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).

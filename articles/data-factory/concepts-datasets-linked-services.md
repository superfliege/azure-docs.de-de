---
title: Datasets und verknüpfte Dienste in Azure Data Factory | Microsoft-Dokumentation
description: Informationen über Datasets und verknüpfte Dienste in Data Factory. Verknüpfte Dienste verknüpfen Compute/-Datenspeicher mit einer Data Factory. Datasets stellen Eingabe/Ausgabe-Daten dar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: shlo
ms.openlocfilehash: 17043ef3450554dd4ea272a4b7732367bae8e369
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="datasets-and-linked-services-in-azure-data-factory"></a>Datasets und verknüpfte Dienste in Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: allgemein verfügbar](v1/data-factory-create-datasets.md)
> * [Version 2 – Vorschauversion](concepts-datasets-linked-services.md)

In diesem Artikel wird beschrieben, was Datasets sind, wie sie im JSON-Format definiert werden, und wie sie in Azure Data Factory V2-Pipelines verwendet werden. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Datasets in Azure Data Factory](v1/data-factory-create-datasets.md).

Wenn Sie noch nicht mit Data Factory vertraut sind, sollten Sie [Einführung in Azure Data Factory](introduction.md) lesen, um eine Übersicht zu erhalten. 

## <a name="overview"></a>Übersicht
Eine Data Factory kann eine oder mehrere Pipelines haben. Bei einer **Pipeline** handelt es sich um eine logische Gruppierung von **Aktivitäten**, die zusammen eine Aufgabe bilden. Die Aktivitäten in einer Pipeline definieren Aktionen, die Sie auf Ihre Daten anwenden. Sie könnten z.B. mit einer Kopieraktivität Daten aus einer lokalen SQL Server-Instanz in eine Instanz von Azure Blob Storage kopieren. Anschließend könnten Sie eine Hive-Aktivität verwenden, die ein Hive-Skript für einen Azure HDInsight-Cluster ausführt, um Daten aus dem Blob Storage zu verarbeiten, um Ausgabedaten zu produzieren. Schließlich könnten Sie die Ausgabedaten mit einer zweiten Kopieraktivität in ein Azure SQL Data Warehouse kopieren, auf Basis dessen Business Intelligence-Berichtslösungen (BI) erstellt werden. Weitere Informationen zu Pipelines und Aktivitäten finden Sie unter [Pipelines und Aktivitäten in Azure Data Factory](concepts-pipelines-activities.md).

Ein **Datensatz** ist eine benannte Ansicht von Daten, die einfach auf die Daten verweist, die Sie in Ihren **Aktivitäten** als Ein- und Ausgabe verwenden möchten. Datasets bestimmen Daten in verschiedenen Datenspeichern, z.B. Tabellen, Dateien, Ordnern und Dokumenten. Ein Azure-Blobdataset kann beispielsweise den Blobcontainer und -ordner in Blob Storage angeben, aus dem die Aktivität die Daten lesen soll.

Bevor Sie ein Dataset erstellen, müssen Sie einen **verknüpften Dienst** erstellen, um Ihren Datenspeicher mit der Data Factory zu verknüpfen. Verknüpfte Dienste ähneln Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die für Data Factory zum Herstellen einer Verbindung mit externen Ressourcen erforderlich sind. Sie können sich dies wie folgt vorstellen: Das Dataset stellt die Struktur der Daten innerhalb des verknüpften Datenspeichers dar, und der verknüpfte Dienst definiert die Verbindung mit der Datenquelle. Ein mit Azure Storage verknüpfter Dienst verbindet z.B. ein Speicherkonto mit der Data Factory. Ein Azure-Blob-Dataset stellt den Blobcontainer und den Ordner innerhalb des Azure-Speicherkontos dar, das die zu verarbeitenden Eingabeblobs enthält.

Hier ist ein Beispielszenario. Um Daten aus Blob Storage in eine SQL-Datenbank zu kopieren, erstellen Sie zwei verknüpfte Dienste: Azure Storage und Azure SQL-Datenbank. Erstellen Sie dann zwei Datasets: Azure-Blobdataset (das sich auf den mit Azure Storage verknüpften Dienst bezieht) und Azure SQL-Tabellendataset (das sich auf den mit Azure SQL-Datenbank verknüpften Dienst bezieht). Die mit Azure Storage und Azure SQL-Datenbank verknüpften Dienste enthalten Verbindungszeichenfolgen, die Data Factory zur Laufzeit verwendet, um die Verbindung mit Ihrem Azure Storage bzw. mit Ihrer Instanz von Azure SQL-Datenbank herzustellen. Das Azure-Blobdataset gibt den Blobcontainer und Blobordner an, der die Eingabeblobs in Ihrer Blob Storage-Instanz enthält. Das Azure SQL-Tabellendataset gibt die SQL-Tabelle in Ihrer SQL-Datenbank an, in die die Daten kopiert werden sollen.

Das folgende Diagramm zeigt die Beziehung zwischen Pipeline, Aktivität, Dataset und verknüpftem Dienst in der Data Factory an:

![Beziehung zwischen Pipeline, Aktivität, Dataset und verknüpften Diensten](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>JSON-Text für verknüpfte Dienste
Ein verknüpfter Dienst in Data Factory wird wie folgt im JSON-Format definiert:

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

In der folgenden Tabelle werden die Eigenschaften im obigen JSON-Code beschrieben:

Eigenschaft | BESCHREIBUNG | Erforderlich |
-------- | ----------- | -------- |
name | Name des verknüpften Diensts. Siehe [Azure Data Factory – Benennungsregeln](naming-rules.md). |  Ja |
type | Typ des verknüpften Diensts. Beispiel: AzureStorage (Datenspeicher) oder AzureBatch (Compute). Siehe die Beschreibung von „typeProperties“. | Ja |
typeProperties | Die Typeigenschaften unterscheiden sich für jeden Datenspeicher- oder Computetyp. <br/><br/> Informationen zu den unterstützten Datenspeichertypen und ihren Typeigenschaften finden Sie in der Tabelle [Datensatztyp](#dataset-type) in diesem Artikel. Navigieren Sie zum Artikel über den Datenspeicherconnector, um mehr über die für einen Datenspeicher spezifischen Typeigenschaften zu erfahren. <br/><br/> Informationen zu den unterstützten Computetypen und ihren Typeigenschaften finden Sie unter [Verknüpfte Computedienste](compute-linked-services.md). | Ja |
connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure Integration Runtime oder selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. | Nein 

## <a name="linked-service-example"></a>Beispiel für einen verknüpften Dienst
Der folgende verknüpfte Dienst ist ein mit Azure Storage verknüpfter Dienst. Beachten Sie, dass „type“ auf „AzureStorage“ festgelegt ist. Die Typeigenschaften für den verknüpften Azure Storage-Dienst umfassen eine Verbindungszeichenfolge. Der Data Factory-Dienst verwendet diese Verbindungszeichenfolge für die Verbindung mit dem Datenspeicher zur Laufzeit. 

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-json"></a>JSON-DataSet
Ein Dataset in Data Factory wird wie folgt im JSON-Format definiert:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                 "type": "LinkedServiceReference",
        },
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}

```
In der folgenden Tabelle werden die Eigenschaften im obigen JSON-Code beschrieben:

Eigenschaft | BESCHREIBUNG | Erforderlich |
-------- | ----------- | -------- |
name | Name des Datasets. Siehe [Azure Data Factory – Benennungsregeln](naming-rules.md). |  Ja |
type | Typ des Datasets. Geben Sie einen der von Data Factory unterstützten Typen an (z. B.: „AzureBlob“, „AzureSqlTable“). <br/><br/>Weitere Informationen finden Sie unter [Datasettyp](#dataset-type). | Ja |
structure | Schema des Datasets. Unter [Dataset: structure](#dataset-structure) finden Sie weitere Details. | Nein  |
typeProperties | Die Typeigenschaften der einzelnen Typen (z.B. Azure-Blob, Azure SQL-Tabelle) sind unterschiedlich. Ausführliche Informationen über die unterstützten Typen und deren Eigenschaften finden Sie unter [Dataset: type](#dataset-type). | Ja |

## <a name="dataset-example"></a>Datasetbeispiel
Im folgenden Beispiel stellt das Dataset eine Tabelle namens MyTable in einer SQL-Datenbank dar.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": {
                "referenceName": "MyAzureSqlLinkedService",
                 "type": "LinkedServiceReference",
        },
        "typeProperties":
        {
            "tableName": "MyTable"
        },
    }
}

```
Beachten Sie folgende Punkte:

- „type“ ist auf „AzuresqlTable“ festgelegt.
- Als tableName-Typeigenschaft (spezifisch für AzureSplTable-Typ) ist „MyTable“ festgelegt.
- linkedServiceName verweist auf einen verknüpften Dienst vom Typ AzureSqlDatabase, der im nächsten JSON-Codeausschnitt definiert ist.

## <a name="dataset-type"></a>Datasettyp
Es gibt viele verschiedene Typen von Datasets, je nach eingesetztem Datenspeicher. In der folgenden Tabelle finden Sie eine Liste der von Data Factory unterstützten Datenspeicher. Klicken Sie auf einen Datenspeicher, um Informationen zum Erstellen eines verknüpften Diensts und eines Datasets für diesen Datenspeicher zu erhalten.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

Im vorherigen Abschnitt ist im Beispiel der Typ des Datasets auf **AzureSqlTable** festgelegt. Ebenso ist für ein Azure-Blobdataset der Typ des Datasets auf **AzureBlob** festgelegt, wie im folgenden JSON-Code dargestellt:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
                "referenceName": "MyAzureStorageLinkedService",
                 "type": "LinkedServiceReference",
        }, 
 
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        }
    }
}
```
## <a name="dataset-structure"></a>Datasetstruktur
Der Abschnitt **structure** ist optional. In ihm ist das Schema des Datasets in einer Sammlung der Namen und Datentypen der Spalten definiert. Im Abschnitt „structure“ legen Sie Typinformationen fest, die zum Konvertieren von Typen und Zuordnen von Spalten von der Quelle zum Ziel bereitgestellt werden.

Jede Spalte im Abschnitt „structure“ enthält die folgenden Eigenschaften:

Eigenschaft | BESCHREIBUNG | Erforderlich
-------- | ----------- | --------
name | Name der Spalte. | Ja
type | Datentyp der Spalte. Data Factory unterstützt die folgenden Zwischendatentypen als zulässige Werte: **Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset und Timespan**. | Nein 
culture | Zu verwendendes .NET-basiertes Gebietsschema, wenn der Typ ein .NET-Typ ist: `Datetime` oder `Datetimeoffset`. Der Standardwert lautet `en-us`. | Nein 
format | Zu verwendende Formatzeichenfolge, wenn der Typ ein .NET-Typ ist: `Datetime` oder `Datetimeoffset`. Informationen zum Formatieren von Datum und Uhrzeit finden Sie unter [Benutzerdefinierte Formatzeichenfolgen für Datum und Uhrzeit](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). | Nein 

### <a name="example"></a>Beispiel
Nehmen Sie für das folgende Beispiel an, dass die Quellblobdaten im CSV-Format vorliegen und drei Spalten aufweisen: „userid“, „name“ und „lastlogindate“. Sie haben den Typ „Int64“, „String“ bzw. „Datetime“ mit einem benutzerdefinierten datetime-Format mit abgekürzten französischen Namen für die Wochentage.

Definieren Sie die Blobdatasetstruktur wie folgt zusammen mit Typdefinitionen für die Spalten:

```json
"structure":
[
    { "name": "userid", "type": "Int64"},
    { "name": "name", "type": "String"},
    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
]
```

### <a name="guidance"></a>Anleitungen

Anhand der folgenden Anleitungen können Sie erkennen, wann der Abschnitt **structure** mit welchen Informationen verwendet werden sollte. Hier finden Sie weitere Informationen darüber, wie Data Factory Quelldaten zu Senken zuordnet und wann Strukturinformationen der [Schema- und Typzuordnung](copy-activity-schema-and-type-mapping.md) angegeben werden müssen.

- **Für Datenquellen mit festem Schema** geben Sie den Abschnitt „structure“ nur an, wenn Sie Quellspalten zu Senkenspalten zuordnen möchten und die Spaltennamen nicht identisch sind. Diese Art von strukturierter Datenquelle speichert Informationen zu Datenschema und Datentyp zusammen mit den Daten selbst. Zu Beispielen für strukturierte Datenquellen gehören SQL Server, Oracle und Azure SQL-Datenbank.<br/><br/>Da Typinformationen für strukturierte Datenquellen bereits verfügbar sind, sollten Sie keine Typinformationen einschließen, wenn Sie sich für die Verwendung des Abschnitts „structure“ entscheiden.
- **Geben Sie für Datenquellen ohne Schema oder mit schwachem Schema, z.B. eine Textdatei im Blobspeicher**, „structure“ auch an, wenn das Dataset eine Eingabe für eine Kopieraktivität ist und die Datentypen des Quelldatasets für die Senke in native Typen konvertiert werden müssen. Geben Sie „structure“ darüber hinaus an, wenn Sie Quellspalten zu Senkenspalten zuordnen möchten.

## <a name="create-datasets"></a>Erstellen von Datasets
Datasets können Sie mit einem dieser Tools oder SDKs erstellen: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST-API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager-Vorlage und Azure-Portal

## <a name="v1-vs-v2-datasets"></a>V1- im Vergleich zu V2-Datasets

Hier sind einige Unterschiede zwischen Data Factory v1- und Data Factory v2-Datasets: 

- Die externe Eigenschaft wird in V2 nicht unterstützt. Sie wird durch einen [Trigger](concepts-pipeline-execution-triggers.md) ersetzt.
- Die Richtlinien- und Verfügbarkeitseigenschaften werden in V2 nicht unterstützt. Die Startzeit für eine Pipeline hängt von [Triggern](concepts-pipeline-execution-triggers.md) ab.
- Bereichsbezogene (in einer Pipeline definierte) Datasets werden in V2 nicht unterstützt. 

## <a name="next-steps"></a>Nächste Schritte
Im folgenden Tutorial finden Sie schrittweise Anleitungen zum Erstellen von Pipelines und Datasets mit einem dieser Tools oder SDKs. 

- [Schnellstart: Erstellen einer Data Factory mithilfe von .NET](quickstart-create-data-factory-dot-net.md)
- [Schnellstart: Erstellen einer Data Factory mithilfe von PowerShell](quickstart-create-data-factory-powershell.md)
- [Schnellstart: Erstellen einer Data Factory mithilfe der REST-API](quickstart-create-data-factory-rest-api.md)
- Schnellstart: Erstellen einer Data Factory mithilfe des Azure-Portals

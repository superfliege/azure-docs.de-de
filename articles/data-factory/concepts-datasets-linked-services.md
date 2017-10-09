---
title: "Datasets und verknüpfte Dienste in Azure Data Factory | Microsoft-Dokumentation"
description: "Informationen über Datasets und verknüpfte Dienste in Data Factory. Verknüpfte Dienste verknüpfen Compute/-Datenspeicher mit einer Data Factory. Datasets stellen Eingabe/Ausgabe-Daten dar."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: 
ms.date: 09/05/2017
ms.author: shlo
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: f03c91b7b27a4fb39b996599efd11242a785b2b2
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="datasets-and-linked-services-in-azure-data-factory"></a>Datasets und verknüpfte Dienste in Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-create-datasets.md)
> * [Version 2 – Vorschauversion](concepts-datasets-linked-services.md)

In diesem Artikel wird beschrieben, was Datasets sind, wie sie im JSON-Format definiert werden, und wie sie in Azure Data Factory V2-Pipelines verwendet werden. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Datasets in Azure Data Factory](v1/data-factory-create-datasets.md).

Wenn Sie noch nicht mit Data Factory vertraut sind, sollten Sie [Einführung in Azure Data Factory](introduction.md) lesen, um eine Übersicht zu erhalten. 

## <a name="overview"></a>Übersicht
Eine Data Factory kann eine oder mehrere Pipelines haben. Bei einer **Pipeline** handelt es sich um eine logische Gruppierung von **Aktivitäten**, die zusammen eine Aufgabe bilden. Die Aktivitäten in einer Pipeline definieren Aktionen, die Sie auf Ihre Daten anwenden. Sie könnten z.B. mit einer Kopieraktivität Daten aus einer lokalen SQL Server-Instanz in eine Instanz von Azure Blob Storage kopieren. Anschließend könnten Sie eine Hive-Aktivität verwenden, die ein Hive-Skript für einen Azure HDInsight-Cluster ausführt, um Daten aus dem Blob Storage zu verarbeiten, um Ausgabedaten zu produzieren. Schließlich könnten Sie die Ausgabedaten mit einer zweiten Kopieraktivität in ein Azure SQL Data Warehouse kopieren, auf Basis dessen Business Intelligence-Berichtslösungen (BI) erstellt werden. Weitere Informationen zu Pipelines und Aktivitäten finden Sie unter [Pipelines und Aktivitäten in Azure Data Factory](concepts-pipelines-activities.md).

Ein **Datensatz** ist eine benannte Ansicht von Daten, die einfach auf die Daten verweist, die Sie in Ihren **Aktivitäten** als Ein- und Ausgabe verwenden möchten. Datasets bestimmen Daten in verschiedenen Datenspeichern, z.B. Tabellen, Dateien, Ordnern und Dokumenten. Ein Azure-Blobdataset kann beispielsweise den Blobcontainer und -ordner in Blob Storage angeben, aus dem die Aktivität die Daten lesen soll.

Bevor Sie ein Dataset erstellen, müssen Sie einen **verknüpften Dienst** erstellen, um Ihren Datenspeicher mit der Data Factory zu verknüpfen. Verknüpfte Dienste ähneln Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die für Data Factory zum Herstellen einer Verbindung mit externen Ressourcen erforderlich sind. Sie können sich dies wie folgt vorstellen: Das Dataset stellt die Struktur der Daten innerhalb des verknüpften Datenspeichers dar, und der verknüpfte Dienst definiert die Verbindung mit der Datenquelle. Ein mit Azure Storage verknüpfter Dienst verbindet z.B. ein Speicherkonto mit der Data Factory. Ein Azure-Blob-Dataset stellt den Blobcontainer und den Ordner innerhalb des Azure-Speicherkontos dar, das die zu verarbeitenden Eingabeblobs enthält.

Hier ist ein Beispielszenario. Um Daten aus Blob Storage in eine SQL-Datenbank zu kopieren, erstellen Sie zwei verknüpfte Dienste: Azure Storage und Azure SQL-Datenbank. Erstellen Sie dann zwei Datasets: Azure-Blobdataset (das sich auf den mit Azure Storage verknüpften Dienst bezieht) und Azure SQL-Tabellendataset (das sich auf den mit Azure SQL-Datenbank verknüpften Dienst bezieht). Die mit Azure Storage und Azure SQL-Datenbank verknüpften Dienste enthalten Verbindungszeichenfolgen, die Data Factory zur Laufzeit verwendet, um die Verbindung mit Ihrem Azure Storage bzw. mit Ihrer Instanz von Azure SQL-Datenbank herzustellen. Das Azure-Blobdataset gibt den Blobcontainer und Blobordner an, der die Eingabeblobs in Ihrer Blob Storage-Instanz enthält. Das Azure SQL-Tabellendataset gibt die SQL-Tabelle in Ihrer SQL-Datenbank an, in die die Daten kopiert werden sollen.

Das folgende Diagramm zeigt die Beziehung zwischen Pipeline, Aktivität, Dataset und verknüpftem Dienst in der Data Factory an:

![Beziehung zwischen Pipeline, Aktivität, Dataset und verknüpften Diensten](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

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

Eigenschaft | Beschreibung | Erforderlich | Standard
-------- | ----------- | -------- | -------
Name | Name des Datasets. | Siehe [Azure Data Factory – Benennungsregeln](naming-rules.md). | Ja | NA
type | Typ des Datasets. | Geben Sie einen der von Data Factory unterstützten Typen an (z. B.: „AzureBlob“, „AzureSqlTable“). <br/><br/>Weitere Informationen finden Sie unter [Datasettyp](#dataset-types). | Ja | NA
structure | Schema des Datasets. | Unter [Dataset: structure](#dataset-structure) finden Sie weitere Details. | Nein | NA
typeProperties | Die Typeigenschaften der einzelnen Typen (z.B. Azure-Blob, Azure SQL-Tabelle) sind unterschiedlich. Ausführliche Informationen über die unterstützten Typen und deren Eigenschaften finden Sie unter [Dataset: type](#dataset-type). | Ja | NA

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

## <a name="linked-service-example"></a>Beispiel für einen verknüpften Dienst
AzureSqlLinkedService wird wie folgt definiert:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```
Im vorherigen JSON-Codeausschnitt:

- **type** wird auf „AzureSqlDatabase“ festgelegt.
- Die **connectionString**-Typeigenschaft gibt Informationen zur Verbindung mit einer SQL-Datenbank an.

Wie Sie sehen, definiert der verknüpfte Dienst das Herstellen einer Verbindung mit einer SQL-Datenbank. Das Dataset definiert, welche Tabelle als Eingabe und Ausgabe für die Aktivität in einer Pipeline verwendet wird.

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
Der Abschnitt **structure** ist optional. In ihm ist das Schema des Datasets in einer Sammlung der Namen und Datentypen der Spalten definiert. Im Abschnitt „structure“ legen Sie Typinformationen fest, die zum Konvertieren von Typen und Zuordnen von Spalten von der Quelle zum Ziel bereitgestellt werden. Im folgenden Beispiel hat das Dataset drei Spalten: „timestamp“, „projectname“ und „pageviews“. Die Spalten haben jeweils den folgenden Typ: „String“, „String“, „Decimal“.

```json
[
    { "name": "timestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Jede Spalte im Abschnitt „structure“ enthält die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
name | Name der Spalte. | Ja
type | Datentyp der Spalte. | Nein
culture | Zu verwendendes .NET-basiertes Gebietsschema, wenn der Typ ein .NET-Typ ist: `Datetime` oder `Datetimeoffset`. Der Standardwert lautet `en-us`. | Nein
format | Zu verwendende Formatzeichenfolge, wenn der Typ ein .NET-Typ ist: `Datetime` oder `Datetimeoffset`. | Nein

Anhand der folgenden Anleitungen können Sie entscheiden, wann der Abschnitt **structure** mit welchen Informationen verwendet werden sollte.

- **Für strukturierte Datenquellen** geben Sie den Abschnitt „structure“ nur an, wenn Sie Quellspalten zu Zielspalten zuordnen möchten und die Spaltennamen nicht identisch sind. Diese Art von strukturierter Datenquelle speichert Informationen zu Datenschema und Datentyp zusammen mit den Daten selbst. Zu Beispielen für strukturierte Datenquellen gehören SQL Server, Oracle und Azure SQL-Datenbank.<br/><br/>Da Typinformationen für strukturierte Datenquellen bereits verfügbar sind, sollten Sie keine Typinformationen einschließen, wenn Sie sich für die Verwendung des Abschnitts „structure“ entscheiden.
- **Für das Schema von Lesedatenquellen (insbesondere Blob Storage)** können Sie Daten speichern, ohne Schema- oder Typinformationen mit den Daten zu speichern. Für diese Datenquellentypen geben Sie „structure“ an, wenn Sie Quellspalten zu Zielspalten zuordnen möchten. Geben Sie „structure“ auch an, wenn das Dataset eine Eingabe für eine Kopieraktivität ist und die Datentypen des Quelldatasets in systemeigene Typen für das Ziel (Senke) konvertiert werden müssen.<br/><br/> Data Factory unterstützt die folgenden Werte für das Bereitstellen von Typinformationen in der Struktur: `Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset, and Timespan`. 

Hier finden Sie weitere Informationen darüber, wie Data Factory Quelldaten aufgrund der [Schema- und Typzuordnung]( copy-activity-schema-and-type-mapping.md) Senken zuordnet, und wann Strukturinformationen angegeben werden müssen.

## <a name="create-datasets"></a>Erstellen von Datasets
Sie können mit einem dieser Tools oder SDKs Datasets erstellen: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell]((quickstart-create-data-factory-powershell.md), [REST-API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager-Vorlage und Azure-Portal

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


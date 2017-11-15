---
title: "Kopieraktivität in Azure Data Factory | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen über die Kopieraktivität in Azure Data Factory, die Sie zum Kopieren von Daten aus einem unterstützten Quelldatenspeicher in einen unterstützten Senkendatenspeicher verwenden können."
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
ms.date: 11/02/2017
ms.author: jingwang
ms.openlocfilehash: 9629a86e72e0d5943f0e50e04268720fbedf6c35
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="copy-activity-in-azure-data-factory"></a>Kopieraktivität in Azure Data Factory

## <a name="overview"></a>Übersicht

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-data-movement-activities.md)
> * [Version 2 – Vorschau](copy-activity-overview.md)

In Azure Data Factory können Sie die Kopieraktivität verwenden, um Daten zwischen lokalen Datenspeichern und Clouddatenspeichern zu kopieren. Nach dem Kopieren können die Daten weiter transformiert und analysiert werden. Sie können die Kopieraktivität auch zum Veröffentlichen von Transformations- und Analyseergebnissen für die Verwendung für Business Intelligence (BI) und in Anwendungen verwenden.

![Rolle der Kopieraktivität](media/copy-activity-overview/copy-activity.png)

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Kopieraktivität in V1](v1/data-factory-data-movement-activities.md).

Die Kopieraktivität wird in [Integration Runtime](concepts-integration-runtime.md) ausgeführt. Für ein anderes Datenkopierszenario kann eine andere Integration Runtime-Konfiguration genutzt werden:

* Beim Kopieren von Daten zwischen Datenspeichern, die beide öffentlich zugänglich sind, kann die Kopieraktivität von **Azure Integration Runtime** unterstützt werden. Azure Integration Runtime ist sicher, zuverlässig, skalierbar und [global verfügbar](concepts-integration-runtime.md#integration-runtime-location).
* Beim Kopieren von Daten aus bzw. in lokale Datenspeicher oder Datenspeicher in einem Netzwerk mit Zugriffssteuerung (z.B. Azure Virtual Network) müssen Sie eine **selbstgehostete integrierte Runtime** einrichten, um das Kopieren der Daten zu ermöglichen.

Integration Runtime muss mit jedem Quell- und Senkendatenspeicher verknüpft werden. Erfahren Sie, wie die Kopieraktivität [bestimmt, welche IR verwendet werden muss](concepts-integration-runtime.md#determining-which-ir-to-use).

Die Kopieraktivität durchläuft die folgenden Phasen, um Daten aus einer Quelle in eine Senke zu kopieren. Folgendes gilt für den Dienst, auf dem die Kopieraktivität basiert:

1. Er liest Daten aus einem Quelldatenspeicher.
2. Er führt die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung usw. durch. Diese Vorgänge erfolgen basierend auf den Konfigurationen von Eingabedataset, Ausgabedataset und Kopieraktivität.
3. Er schreibt Daten in den Senken-/Zieldatenspeicher.

![Kopieraktivität – Übersicht](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Unterstützte Datenspeicher und Formate

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Unterstützte Dateiformate

Sie können die Kopieraktivität verwenden, um **Dateien unverändert** zwischen zwei dateibasierten Datenspeichern zu kopieren. In diesem Fall werden die Daten effizient ohne Serialisierung/Deserialisierung kopiert.

Die Kopieraktivität unterstützt auch das Lesen und Schreiben von Dateien in den folgenden Formaten: **Text, JSON, Avro, ORC und Parquet**. Außerdem unterstützt sie folgende Komprimierungscodecs: **„GZip“, „Deflate“, „BZip2“ und „ZipDeflate“**. Weitere Informationen finden Sie unter [Unterstützte Datei- und Komprimierungsformate](supported-file-formats-and-compression-codecs.md).

Sie können z.B. folgende Kopieraktivitäten ausführen:

* Kopieren von Daten in einer lokalen SQL Server-Instanz und Schreiben in Azure Data Lake Store im ORC-Format
* Kopieren von Dateien im Textformat (CSV) aus dem lokalen Dateisystem und Schreiben in einen Azure-Blob im Avro-Format
* Kopieren von ZIP-Dateien aus dem lokalen Dateisystem und Dekomprimieren in Azure Data Lake Store
* Kopieren von Daten im GZip-komprimierten Textformat (CSV) aus einem Azure-Blob und Schreiben in Azure SQL-Datenbank

## <a name="configuration"></a>Konfiguration

Um die Kopieraktivität in Azure Data Factory zu verwenden, ist Folgendes erforderlich:

1. **Erstellen Sie verknüpfte Dienste für Quell- und Senkendatenspeicher.** Im Abschnitt „Eigenschaften des verknüpften Diensts“ im Connectorartikel finden Sie Informationen zur Konfiguration und zu unterstützten Eigenschaften. Sie finden die Liste der unterstützten Connectors im Abschnitt [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats).
2. **Erstellen Sie Datasets für die Quelle und die Senke.** Im Abschnitt mit Dataseteigenschaften in den Artikeln zu Quell- und Senkenconnectors finden Sie Informationen zur Konfiguration und zu unterstützten Eigenschaften.
3. **Erstellen Sie eine Pipeline mit einer Kopieraktivität.** Der nächste Abschnitt enthält ein Beispiel.  

### <a name="syntax"></a>Syntax

Die folgende Vorlage einer Kopieraktivität enthält eine vollständige Liste unterstützter Eigenschaften. Geben Sie diejenigen an, die zu Ihrem Szenario passen.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "cloudDataMovementUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

### <a name="syntax-details"></a>Syntaxdetails

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die Eigenschaft „type“ einer Kopieraktivität muss auf **Copy** festgelegt werden | Ja |
| inputs | Geben Sie das Dataset an, das Sie erstellt haben und das auf die Quelldaten verweist. Die Kopieraktivität unterstützt nur eine einzelne Eingabe. | Ja |
| outputs | Geben Sie das Dataset an, das Sie erstellt haben und das auf die Senkendaten verweist. Die Kopieraktivität unterstützt nur eine einzelne Ausgabe. | Ja |
| typeProperties | Eine Gruppe von Eigenschaften zum Konfigurieren der Kopieraktivität. | Ja |
| Quelle | Geben Sie den Quelltyp für den Kopiervorgang und die zugehörigen Eigenschaften zum Abrufen von Daten an.<br/><br/>Informationen finden Sie im Abschnitt mit den Eigenschaften der Kopieraktivität im Connectorartikel, der unter [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats) aufgeführt ist. | Ja |
| sink | Geben Sie den Senkentyp für den Kopiervorgang und die zugehörigen Eigenschaften zum Schreiben von Daten an.<br/><br/>Informationen finden Sie im Abschnitt mit den Eigenschaften der Kopieraktivität im Connectorartikel, der unter [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats) aufgeführt ist. | Ja |
| translator | Geben Sie explizite Spaltenzuordnungen von der Quelle zur Senke an. Gilt, wenn das Standardverhalten für den Kopiervorgang Ihre Anforderungen nicht erfüllen kann.<br/><br/>Informationen finden Sie unter [Schema- und Datentypzuordnung](copy-activity-schema-and-type-mapping.md). | Nein |
| cloudDataMovementUnits | Geben Sie die Leistungsfähigkeit von [Azure Integration Runtime](concepts-integration-runtime.md) an, um das Kopieren der Daten zu unterstützen.<br/><br/>Informationen finden Sie unter [Einheiten für Clouddatenverschiebungen](copy-activity-performance.md). | Nein |
| parallelCopies | Geben Sie die Parallelität an, die die Kopieraktivität beim Lesen von Daten aus der Quelle und beim Schreiben von Daten in die Senke verwenden soll.<br/><br/>Informationen finden Sie unter [Parallele Kopie](copy-activity-performance.md#parallel-copy). | Nein |
| enableStaging<br/>stagingSettings | Wählen Sie aus, dass die vorläufigen Daten in einem Blobspeicher bereitgestellt werden, statt Daten direkt aus der Quelle in die Senke zu kopieren.<br/><br/>Nützliche Szenarien und Konfigurationsdetails finden Sie unter [Gestaffeltes Kopieren](copy-activity-performance.md#staged-copy). | Nein |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Wählen Sie aus, wie nicht kompatible Zeilen beim Kopieren von Daten aus der Quelle in die Senke behandelt werden sollen.<br/><br/>Informationen finden Sie unter [Fehlertoleranz](copy-activity-fault-tolerance.md). | Nein |

## <a name="monitoring"></a>Überwachung

Ausführungsdetails der Kopieraktivität und Leistungsmerkmale werden im Ausführungsergebnis der Kopieraktivität im Ausgabeabschnitt zurückgegeben. Im Folgenden finden Sie eine vollständige Liste. Informationen zum Überwachen der Aktivitätsausführung finden Sie im [Abschnitt mit Schnellstartanleitungen für die Überwachung ](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run). Sie können die Leistung und Konfiguration Ihres Szenarios mit der [Leistungsreferenz](copy-activity-performance.md#performance-reference) für die Kopieraktivität aus unseren internen Tests vergleichen.

| Eigenschaftenname  | Beschreibung | Einheit |
|:--- |:--- |:--- |
| dataRead | Größe der Daten, die aus der Quelle gelesen werden | Int64-Wert in Bytes |
| dataWritten | Größe der Daten, die in die Senke geschrieben werden | Int64-Wert in Bytes |
| rowsCopied | Anzahl der Zeilen, die kopiert werden (gilt nicht für eine Binärkopie). | Int64-Wert (ohne Einheit) |
| rowsSkipped | Anzahl der übersprungenen, nicht kompatiblen Zeilen. Sie können das Feature aktivieren, indem Sie „enableSkipIncompatibleRow“ auf „true“ festlegen. | Int64-Wert (ohne Einheit) |
| throughput | Verhältnis, in dem Daten übertragen werden | Gleitkommazahl in KB/s |
| copyDuration | Die Dauer des Kopiervorgangs | Int32-Wert in Sekunden |
| sqlDwPolyBase | Wenn PolyBase beim Kopieren von Daten in SQL Data Warehouse verwendet wird. | Boolean |
| redshiftUnload | Wenn UNLOAD beim Kopieren von Daten aus Redshift verwendet wird. | Boolean |
| hdfsDistcp | Wenn DistCp beim Kopieren von Daten aus HDFS verwendet wird. | Boolean |
| effectiveIntegrationRuntime | Zeigt an, welche Integration Runtimes verwendet werden, um die Aktivitätsausführung zu unterstützen. Dabei wird folgendes Format verwendet: `<IR name> (<region if it's Azure IR>)`. | Text (Zeichenfolge) |
| usedCloudDataMovementUnits | Die effektiven Einheiten für Clouddatenverschiebungen beim Kopieren. | Int32-Wert |
| redirectRowPath | Pfad zum Protokoll der übersprungenen, nicht kompatible Zeilen im Blobspeicher, den Sie unter „redirectIncompatibleRowSettings“ konfigurieren. Siehe das folgende Beispiel. | Text (Zeichenfolge) |
| billedDuration | Die Dauer, die für Datenverschiebungen in Rechnung gestellt wird. | Int32-Wert in Sekunden |

```json
"output": {
    "dataRead": 1024,
    "dataWritten": 2048,
    "rowsCopies": 100,
    "rowsSkipped": 2,
    "throughput": 1024.0,
    "copyDuration": 3600,
    "redirectRowPath": "https://<account>.blob.core.windows.net/<path>/<pipelineRunID>/",
    "redshiftUnload": true,
    "sqlDwPolyBase": true,
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedCloudDataMovementUnits": 8,
    "billedDuration": 28800
}
```

## <a name="schema-and-data-type-mapping"></a>Schema- und Datentypzuordnung

Unter [Schema- und Datentypzuordnung](copy-activity-schema-and-type-mapping.md) wird beschrieben, wie die Kopieraktivität die Quelldaten der Senke zugeordnet.

## <a name="fault-tolerance"></a>Fehlertoleranz

Standardmäßig beendet die Kopieraktivität das Kopieren von Daten und gibt einen Fehler zurück, wenn nicht kompatible Daten zwischen Quelle und Senke festgestellt werden. Sie können explizit konfigurieren, dass nicht kompatible Zeilen übersprungen und protokolliert werden, und nur die kompatiblen Daten kopieren, damit der Kopiervorgang erfolgreich ist. Weitere Details finden Sie unter [Copy Activity fault tolerance - skip incompatible rows](copy-activity-fault-tolerance.md) (Fehlertoleranz der Kopieraktivität – Überspringen inkompatibler Zeilen).

## <a name="performance-and-tuning"></a>Leistung und Optimierung

Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](copy-activity-performance.md)beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken. Außerdem wird die beobachtete Leistung bei internen Tests aufgeführt und es werden verschiedene Methoden zur Optimierung der Leistung der Kopieraktivität erläutert.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Schnellstartanleitungen, Tutorials und Beispielen:

- [Kopieren von Daten von einem Speicherort in einen anderen Speicherort im gleichen Azure-Blobspeicher](quickstart-create-data-factory-dot-net.md)
- [Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank](tutorial-copy-data-dot-net.md)
- [Kopieren von Daten aus einer lokalen SQL Server-Instanz in Azure](tutorial-hybrid-copy-powershell.md)

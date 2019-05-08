---
title: Kopieren von Daten aus/in Azure File Storage mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure Data Factory Daten aus Azure File Storage in unterstützte Senkendatenspeicher oder aus unterstützten Quelldatenspeichern in Azure File Storage kopieren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: jingwang
ms.openlocfilehash: b43532ade35e4e01573abbd3f47d009beadd9c60
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267717"
---
# <a name="copy-data-from-or-to-azure-file-storage-by-using-azure-data-factory"></a>Kopieren von Daten aus bzw. in Azure File Storage mithilfe von Azure Data Factory

In diesem Artikel erfahren Sie, wie Sie mithilfe der Kopieraktivität in Azure Data Factory Daten aus bzw. in Azure File Storage (Azure Files) kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Azure File Storage in einen beliebigen unterstützten Senkendatenspeicher bzw. Daten aus einem beliebigen unterstützten Quelldatenspeicher in Azure File Storage kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Azure File Storage-Connector unterstützt insbesondere das Kopieren von Dateien im unveränderten Zustand sowie das Analysieren/Generieren von Dateien mit den [unterstützten Dateiformaten und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md).

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Azure File Storage-spezifischen Data Factory-Entitäten verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Azure File Storage verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **FileServer**. | Ja |
| host | Gibt den Azure File Storage-Endpunkt an als: <br/>– Unter Verwendung der Benutzeroberfläche: Geben Sie `\\<storage name>.file.core.windows.net\<file service name>`<br/>– Unter Verwendung von JSON: `"host": "\\\\<storage name>.file.core.windows.net\\<file service name>"`. | Ja |
| userid | Geben Sie den Benutzer für den Zugriff auf Azure File Storage an als: <br/>– Unter Verwendung der Benutzeroberfläche: Geben Sie `AZURE\<storage name>`<br/>– Unter Verwendung von JSON: `"userid": "AZURE\\<storage name>"`. | Ja |
| password | Geben Sie den Speicherzugriffsschlüssel an. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Quelle: Nein, Senke: Ja |

>[!IMPORTANT]
> - Wenn Sie Daten unter Verwendung von Azure Integration Runtime in Azure File Storage kopieren möchten, [erstellen Sie explizit eine Azure-IR](create-azure-integration-runtime.md#create-azure-ir) mit dem Standort Ihres File Storage-Diensts, und konfigurieren Sie die Zuordnung wie im folgenden Beispiel.
> - Wenn Sie Daten unter Verwendung einer selbstgehosteten Integration Runtime außerhalb von Azure aus/in Azure File Storage kopieren möchten, vergessen Sie nicht, den ausgehenden TCP-Port 445 in Ihrem lokalen Netzwerk zu öffnen.

>[!TIP]
>Wenn Sie die Benutzeroberfläche der Anwendungsdefinitionsdatei für die Dokumenterstellung verwenden, finden Sie den spezifischen Eintrag für „Azure File Storage“ zum Erstellen eines verknüpften Diensts, der darunter ein Objekt vom Typ `FileServer` generiert.

**Beispiel:**

```json
{
    "name": "AzureFileStorageLinkedService",
    "properties": {
        "type": "FileServer",
        "typeProperties": {
            "host": "\\\\<storage name>.file.core.windows.net\\<file service name>",
            "userid": "AZURE\\<storage name>",
            "password": {
                "type": "SecureString",
                "value": "<storage access key>"
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste mit Eigenschaften, die vom Azure File Storage-Dataset unterstützt werden.

Wenn Sie Daten aus/in Azure File Storage kopieren möchten, legen Sie die type-Eigenschaft des Datasets auf **FileShare** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf folgenden Wert festgelegt werden: **FileShare** |Ja |
| folderPath | Pfad zum Ordner. <br/><br/>Platzhalterfilter werden unterstützt. Zulässige Platzhalter sind: `*` (entspricht null oder mehr Zeichen) und `?` (entspricht null oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn Ihr tatsächlicher Ordnername einen Platzhalter oder dieses Escapezeichen enthält. <br/><br/>Beispiele: Stammordner/Unterordner/. Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). |Ja |
| fileName | **Name oder Platzhalterfilter** für die Dateien unter dem angegebenen Wert für „folderPath“. Wenn Sie für diese Eigenschaft keinen Wert angeben, verweist das Dataset auf alle Dateien im Ordner. <br/><br/>Für Filter sind folgende Platzhalter zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen).<br/>– Beispiel 1: `"fileName": "*.csv"`<br/>– Beispiel 2: `"fileName": "???20180427.txt"`<br/>Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Dateiname einen Platzhalter oder dieses Escapezeichen enthält.<br/><br/>Wenn „fileName“ nicht für ein Ausgabedataset und **preserveHierarchy** nicht in der Aktivitätssenke angegeben sind, generiert die Kopieraktivität den Dateinamen automatisch mit dem folgenden Muster: „*Data.[GUID der Aktivitätsausführungs-ID].[GUID bei Verwendung von „FlattenHierarchy“].[Format, sofern konfiguriert].[Komprimierung, sofern konfiguriert]*“, z.B. „Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz“. Wenn Sie Daten aus einer Quelle im Tabellenformat kopieren und dabei anstelle einer Abfrage den Tabellennamen verwenden, lautet das Namensmuster „*[Tabellenname].[ Format].[Komprimierung, sofern konfiguriert]*“, z.B. „MyTable.csv“. |Nein  |
| modifiedDatetimeStart | Dateifilterung basierend auf dem Attribut: Letzte Änderung. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br/><br/> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird.  Wenn `modifiedDatetimeStart` den datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist.  Wenn `modifiedDatetimeEnd` den datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.| Nein  |
| modifiedDatetimeEnd | Dateifilterung basierend auf dem Attribut: Letzte Änderung. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br/><br/> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird.  Wenn `modifiedDatetimeStart` den datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist.  Wenn `modifiedDatetimeEnd` den datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.| Nein  |
| format | Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen.<br/><br/>Für das Analysieren oder Generieren von Dateien mit einem bestimmten Format werden die folgenden Dateiformattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** und **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| compression | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Folgende Typen werden unterstützt: **GZip**, **Deflate**, **BZip2** und **ZipDeflate**.<br/>Folgende Ebenen werden unterstützt: **Optimal** und **Fastest**. |Nein  |

>[!TIP]
>Wenn Sie alle Dateien eines Ordners kopieren möchten, geben Sie nur **folderPath** an.<br>Wenn Sie eine einzelne Datei mit einem bestimmten Namen kopieren möchten, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Dateinamen an.<br>Wenn Sie eine Teilmenge der Dateien eines Ordners kopieren möchten, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Platzhalterfilter an.

>[!NOTE]
>Wenn Sie die Eigenschaft „fileFilter“ für den Dateifilter verwendet haben, wird sie weiterhin unverändert unterstützt. Es wird aber empfohlen, ab sofort die Filterfunktion zu verwenden, die „fileName“ neu hinzugefügt wurde.

**Beispiel:**

```json
{
    "name": "AzureFileStorageDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<Azure File Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit Eigenschaften, die von der Azure File Storage-Quelle und -Senke unterstützt werden.

### <a name="azure-file-storage-as-source"></a>Azure File Storage als Quelle

Wenn Sie Daten aus Azure File Storage kopieren möchten, legen Sie den Quelltyp in der Kopieraktivität auf **FileSystemSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf Folgendes festgelegt werden: **FileSystemSource** |Ja |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Beachten Sie Folgendes: Wenn „recursive“ auf TRUE festgelegt und die Senke ein dateibasierter Speicher ist, wird ein leerer Ordner/Unterordner nicht in die Senke kopiert bzw. nicht in ihr erstellt.<br/>Zulässige Werte sind **true** (Standard) oder **false**. | Nein  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure File Storage input dataset name>",
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
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-file-storage-as-sink"></a>Azure File Storage als Senke

Wenn Sie Daten in Azure File Storage kopieren möchten, legen Sie den Senkentyp in der Kopieraktivität auf **FileSystemSink** fest. Folgende Eigenschaften werden im Abschnitt **sink** unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf Folgendes festgelegt sein: **FileSystemSink** |Ja |
| copyBehavior | Definiert das Kopierverhalten, wenn es sich bei der Quelle um Dateien aus einem dateibasierten Datenspeicher handelt.<br/><br/>Zulässige Werte sind:<br/><b>- PreserveHierarchy (Standard)</b>: Die Dateihierarchie wird im Zielordner beibehalten. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><b>- FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich in der ersten Ebene des Zielordners. Für die Zieldateien wird ein automatisch ein Name erzeugt. <br/><b>- MergeFiles</b>: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Datei-/Blob-Name angegeben wurde, entspricht der Name dem angegebenen Namen, andernfalls dem automatisch generierten Dateinamen. | Nein  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure File Storage output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "FileSystemSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Beispiele für Ordner- und Dateifilter

Dieser Abschnitt beschreibt das resultierende Verhalten für den Ordnerpfad und den Dateinamen mit Platzhalterfiltern.

| folderPath | fileName | recursive | Quellordnerstruktur und Filterergebnis (Dateien mit **Fettformatierung** werden abgerufen.)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (leer, Standardwert verwenden) | false | OrdnerA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.csv<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*` | (leer, Standardwert verwenden) | true | OrdnerA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*` | `*.csv` | false | OrdnerA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.csv<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*` | `*.csv` | true | OrdnerA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |

### <a name="recursive-and-copybehavior-examples"></a>Beispiele für "recursive" und "copyBehavior"

Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen von rekursiven und CopyBehavior-Werten.

| recursive | copyBehavior | Struktur des Quellordners | Resultierendes Ziel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der gleichen Struktur erstellt wie die Quelle:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 |
| true |flattenHierarchy | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei5 |
| true |mergeFiles | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp; Inhalte von Datei1 + Datei2 + Datei3 + Datei4 + Datei5 werden in einer Datei mit einem automatisch generierten Namen zusammengeführt. |
| false |preserveHierarchy | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |flattenHierarchy | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |mergeFiles | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Inhalte von Datei1 + Datei2 werden zu einer Datei mit einem automatisch generierten Namen zusammengeführt. Automatisch generierter Name für Datei1<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).

---
title: Kopieren von Daten in oder aus Azure Data Lake Storage Gen2 (Vorschau) mithilfe von Data Factory (Vorschau) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Azure Data Factory Daten in oder aus Azure Data Lake Storage Gen2 (Vorschau) kopieren.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: ded02fc78d276cd37f7b8db3b3d9de1c4e5f1b2f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53105188"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen2-preview-using-azure-data-factory-preview"></a>Kopieren von Daten in oder aus Azure Data Lake Storage Gen2 (Vorschau) mithilfe von Azure Data Factory (Vorschau)

Azure Data Lake Storage Gen2 (Vorschau) setzt auf [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md) auf und bietet eine Reihe von Funktionen für die Big Data-Analyse. Er ermöglicht Ihnen die Arbeit mit Ihren Daten über das Dateisystem und den Objektspeicher.

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten in und aus Data Lake Storage Gen2 zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus jedem unterstützten Quelldatenspeicher nach Data Lake Storage Gen2 kopieren. Außerdem können Sie auch Daten aus Data Lake Storage Gen2 in jeden unterstützten Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md).

Dieser Connector unterstützt insbesondere Folgendes:

- Kopieren von Dateien durch Verwendung eines Kontoschlüssels, eines Dienstprinzipals oder verwalteter Identitäten für die Authentifizierung von Azure-Ressourcen.
- Kopieren von Dateien im jeweiligen Zustand oder Analysieren bzw. Generieren von Dateien mit den [unterstützten Dateiformaten und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md)

>[!TIP]
>Wenn Sie den hierarchischen Namespace aktivieren, beachten Sie, dass derzeit keine Interoperabilität bei Vorgängen zwischen Blob- und ADLS Gen2-APIs besteht. Falls der Fehler „ErrorCode=FilesystemNotFound“ mit dem Meldungstext „Das angegebene Dateisystem ist nicht vorhanden.“ auftritt, ist dies auf das angegebene Senkendateisystem zurückzuführen, das an anderer Stelle anstelle der ADLS Gen2-API über die Blob-API erstellt wurde. Geben Sie zum Beheben des Problems ein neues Dateisystem mit einem Namen an, der nicht als Name eines Blobcontainers vorhanden ist. Von ADF wird dieses Dateisystem automatisch während des Datenkopiervorgangs erstellt.

>[!NOTE]
>Wenn Sie die Option _„Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben“_ in den Firewalleinstellungen von Azure Storage aktivieren, tritt beim Herstellen der Verbindung mit Data Lake Storage Gen2 mithilfe von Azure Integration Runtime der Fehler „Verboten“ auf, da ADF nicht als vertrauenswürdiger Microsoft-Dienst gelten. Verwenden Sie stattdessen die selbstgehostete Integration Runtime zum Herstellen der Verbindung.

## <a name="get-started"></a>Erste Schritte

>[!TIP]
>Eine exemplarische Vorgehensweise zur Verwendung des Data Lake Storage Gen2-Connectors finden Sie unter [Laden von Daten in Azure Data Lake Storage Gen2](load-azure-data-lake-storage-gen2.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Data Lake Storage Gen2 verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Der Azure Data Lake Storage Gen2-Connector unterstützt die folgenden Authentifizierungstypen (Details finden Sie im entsprechenden Abschnitt):

- [Kontoschlüsselauthentifizierung](#account-key-authentication)
- [Dienstprinzipalauthentifizierung](#service-principal-authentication)
- [Verwaltete Identitäten für Azure-Ressourcenauthentifizierung](#managed-identity)

### <a name="account-key-authentication"></a>Kontoschlüsselauthentifizierung

Für die Verwendung der Authentifizierung mit dem Speicherkontoschlüssel werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **AzureBlobFS** festgelegt sein. |Ja |
| URL | Endpunkt für Data Lake Storage Gen2 im Format `https://<accountname>.dfs.core.windows.net` | Ja | 
| accountKey | Kontoschlüssel für den Data Lake Storage Gen2-Dienst. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

**Beispiel:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung

Zum Verwenden der Dienstprinzipalauthentifizierung führen Sie die folgenden Schritte aus:

1. Registrieren Sie eine Anwendungsentität in Azure Active Directory (Azure AD), indem Sie die Anleitungen unter [Registrieren Ihrer Anwendung bei einem Azure AD-Mandanten](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant) befolgen. Notieren Sie sich die folgenden Werte, die Sie zum Definieren des verknüpften Diensts verwenden:

    - Anwendungs-ID
    - Anwendungsschlüssel
    - Mandanten-ID

2. Erteilen Sie dem Dienstprinzipal geeignete Berechtigungen in Azure Storage.

    - Weisen Sie **Als Quelle** in der Zugriffssteuerung (IAM) mindestens die Rolle **Storage-Blobdatenleser** zu.
    - Weisen Sie **Als Senke** in der Zugriffssteuerung (IAM) mindestens die Rolle **Mitwirkender an Storage-Blobdaten** zu.

Diese Eigenschaften werden im verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **AzureBlobFS** festgelegt sein. |Ja |
| URL | Endpunkt für Data Lake Storage Gen2 im Format `https://<accountname>.dfs.core.windows.net` | Ja | 
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | Ja |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als **SecureString**, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja |
| Mandant | Geben Sie die Mandanteninformationen (Domänenname oder Mandanten-ID) für Ihre Anwendung an. Diese können Sie abrufen, indem Sie den Mauszeiger über den rechten oberen Bereich im Azure-Portal bewegen. | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

**Beispiel:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a>Verwaltete Identitäten für Azure-Ressourcenauthentifizierung

Eine Data Factory kann einer [verwalteten Identität für Azure-Ressourcen](data-factory-service-identity.md) zugeordnet werden, die diese spezielle Data Factory darstellt. Sie können diese Dienstidentität direkt für die Blob Storage-Authentifizierung verwenden, ähnlich wie bei der Verwendung Ihres eigenen Dienstprinzipals. Sie erlaubt dieser bestimmten Factory den Zugriff auf und das Kopieren von Daten aus Ihrem bzw. in Ihren Blob Storage.

Um verwaltete Identitäten für die Azure-Ressourcenauthentifizierung zu verwenden, gehen Sie folgendermaßen vor:

1. [Rufen Sie die Data Factory-Dienstidentität](data-factory-service-identity.md#retrieve-service-identity) ab, indem Sie den Wert von „DIENSTIDENTITÄTSANWENDUNGS-ID“ kopieren, der zusammen mit der Factory generiert wurde.

2. Erteilen Sie der verwalteten Entität geeignete Berechtigungen in Azure Storage. 

    - Weisen Sie **Als Quelle** in der Zugriffssteuerung (IAM) mindestens die Rolle **Storage-Blobdatenleser** zu.
    - Weisen Sie **Als Senke** in der Zugriffssteuerung (IAM) mindestens die Rolle **Mitwirkender an Storage-Blobdaten** zu.

Diese Eigenschaften werden im verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **AzureBlobFS** festgelegt sein. |Ja |
| URL | Endpunkt für Data Lake Storage Gen2 im Format `https://<accountname>.dfs.core.windows.net` | Ja | 
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

**Beispiel:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Folgende Eigenschaften werden für das Azure Data Lake Storage-Dataset unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft des Datasets muss auf **AzureBlobFSFile** festgelegt sein. |Ja |
| folderPath | Pfad zum Ordner in Data Lake Storage Gen2. Der Platzhalterfilter wird nicht unterstützt. Wenn keine Angabe vorhanden ist, wird auf das Stammverzeichnis verwiesen. Beispiel: „Stammordner/Unterordner/“ |Nein  |
| fileName | **Name oder Platzhalterfilter** für die Dateien unter dem angegebenen Wert für „folderPath“. Wenn Sie für diese Eigenschaft keinen Wert angeben, verweist das Dataset auf alle Dateien im Ordner. <br/><br/>Für Filter sind folgende Platzhalter zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen).<br/>- Beispiel 1: `"fileName": "*.csv"`<br/>- Beispiel 2: `"fileName": "???20180427.txt"`<br/>Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Dateiname einen Platzhalter oder dieses Escapezeichen enthält.<br/><br/>Wenn „fileName“ nicht für ein Ausgabedataset und **preserveHierarchy** nicht in der Aktivitätssenke angegeben sind, generiert die Kopieraktivität den Dateinamen automatisch mit dem folgenden Muster: „*Data.[GUID der Aktivitätsausführungs-ID].[GUID bei Verwendung von „FlattenHierarchy“].[Format, sofern konfiguriert].[Komprimierung, sofern konfiguriert]*“, z.B. „Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz“. Wenn Sie Daten aus einer Quelle im Tabellenformat kopieren und dabei anstelle einer Abfrage den Tabellennamen verwenden, lautet das Namensmuster „*[Tabellenname].[ Format].[Komprimierung, sofern konfiguriert]*“, z.B. „MyTable.csv“. |Nein  |
| format | Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), überspringen Sie den Formatabschnitt in den Definitionen der Eingabe- und Ausgabedatasets.<br/><br/>Für das Analysieren oder Generieren von Dateien mit einem bestimmten Format werden die folgenden Dateiformattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** und **ParquetFormat**. Sie müssen die **type**-Eigenschaft unter **format** auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Unterstützte Typen sind **GZip**, **Deflate**, **BZIP2** und **ZipDeflate**.<br/>Unterstützte Grade sind **Optimal** und **Schnellste**. |Nein  |

>[!TIP]
>Wenn Sie alle Dateien eines Ordners kopieren möchten, geben Sie nur **folderPath** an.<br>Wenn Sie eine einzelne Datei mit einem bestimmten Namen kopieren möchten, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Dateinamen an.<br>Wenn Sie eine Teilmenge der Dateien eines Ordners kopieren möchten, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Platzhalterfilter an. 

**Beispiel:**

```json
{
    "name": "AzureDataLakeStorageDataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName": "myfile.csv.gz",
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie in den Artikeln [Konfigurationen für die Kopieraktivität](copy-activity-overview.md#configuration) und [Pipelines und Aktivitäten](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Data Lake Storage Gen2-Quelle und -Senke unterstützt werden.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake Storage Gen2 als Quelltyp

Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **AzureBlobFSSource** festgelegt sein. |Ja |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Beachten Sie Folgendes: Wenn „recursive“ auf „true“ festgelegt ist und es sich bei der Senke um einen dateibasierten Speicher handelt, wird ein leerer Ordner oder Unterordner nicht in die Senke kopiert und dort auch nicht erstellt.<br/>Zulässige Werte sind **true** (Standard) und **false**. | Nein  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromAzureDataLakeStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Data Lake Storage input dataset name>",
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
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Azure Data Lake Storage Gen2 als Senkentyp

Folgende Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Senke der Kopieraktivität muss auf **AzureBlobFSSink** festgelegt sein. |Ja |
| copyBehavior | Definiert das Kopierverhalten, wenn es sich bei der Quelle um Dateien aus einem dateibasierten Datenspeicher handelt.<br/><br/>Zulässige Werte sind:<br/><b>- PreserveHierarchy (Standard)</b>: Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><b>- FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich auf der ersten Ebene des Zielordners. Die Namen für die Zieldateien werden automatisch generiert. <br/><b>- MergeFiles</b>: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Dateiname angegeben wurde, entspricht der zusammengeführte Dateiname dem angegebenen Namen. Andernfalls wird der Dateiname automatisch generiert. | Nein  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToAzureDataLakeStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="some-recursive-and-copybehavior-examples"></a>Beispiele für „recursive“ und „copyBehavior“

Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen von rekursiven und CopyBehavior-Werten.

| recursive | copyBehavior | Struktur des Quellordners | Resultierendes Ziel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der gleichen Struktur erstellt wie die Quelle:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 |
| true |flattenHierarchy | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei5 |
| true |mergeFiles | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Die Inhalte von Datei1 + Datei2 + Datei3 + Datei4 + Datei5 werden in einer Datei mit einem automatisch generierten Dateinamen zusammengeführt. |
| false |preserveHierarchy | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |flattenHierarchy | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |mergeFiles | Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Die Inhalte von Datei1 + Datei2 werden in einer Datei mit einem automatisch generierten Dateinamen zusammengeführt. Automatisch generierter Name für Datei1<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |

## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).

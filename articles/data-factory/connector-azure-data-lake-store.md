---
title: Kopieren von Daten nach bzw. aus Azure Data Lake Storage Gen1 mit Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie mithilfe der Data Factory Daten von unterstützten Quelldatenspeichern nach Azure Data Lake Store oder aus Data Lake Store in unterstützte Senkenspeicher kopiert werden.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: jingwang
ms.openlocfilehash: 3b1abe60fc81ae0316e2d0552a1750129171ff5f
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345452"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-by-using-azure-data-factory"></a>Kopieren von Daten nach und aus Azure Data Lake Storage Gen1 mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1](v1/data-factory-azure-datalake-connector.md)
> * [Aktuelle Version](connector-azure-data-lake-store.md)

In diesem Artikel wird beschrieben, wie Sie die Copy-Aktivität (Kopieraktivität) in Azure Data Factory verwenden, um Daten nach und aus Azure Data Lake Storage Gen1 zu kopieren (bisher als Azure Data Lake Store bezeichnet). Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einem beliebigen unterstützten Quelldatenspeicher nach Azure Data Lake Store bzw. Daten aus Azure Data Lake Store in einen beliebigen unterstützten Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der Azure Data Lake Store-Connector unterstützt insbesondere Folgendes:

- Kopieren von Dateien mit Authentifizierung über **Dienstprinzipal** oder **verwaltete Identitäten für Azure-Ressourcen**.
- Kopieren von Dateien im jeweiligen Zustand oder Analysieren bzw. Generieren von Dateien mit den [unterstützten Dateiformaten und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md)

> [!IMPORTANT]
> Wenn Sie Daten mithilfe der selbstgehosteten Integration Runtime kopieren, konfigurieren Sie die Unternehmensfirewall so, dass sie an Port 443 ausgehenden Datenverkehr an `<ADLS account name>.azuredatalakestore.net` und `login.microsoftonline.com/<tenant>/oauth2/token` zulässt. Letzterer ist der Azure-Sicherheitstokendienst (STS), mit dem IR kommunizieren muss, um das Zugriffstoken zu erhalten.

## <a name="get-started"></a>Erste Schritte

> [!TIP]
> Eine exemplarische Vorgehensweise zur Verwendung des Azure Data Lake Store-Connectors finden Sie unter [Laden von Daten in Azure Data Lake Store](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Azure Data Lake Store verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Azure Data Lake Store verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureDataLakeStore** festgelegt werden. | JA |
| dataLakeStoreUri | Informationen zum Azure Data Lake Store-Konto. Diese Informationen haben eines der folgenden Formate: `https://[accountname].azuredatalakestore.net/webhdfs/v1` oder `adl://[accountname].azuredatalakestore.net/`. | JA |
| subscriptionId | ID des Azure-Abonnements, zu dem das Data Lake Store-Konto gehört. | Erforderlich für Senke |
| resourceGroupName | Name der Azure-Ressourcengruppe, zu der das Data Lake Store-Konto gehört. | Erforderlich für Senke |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

Weitere Eigenschaften und JSON-Beispiele für die verschiedenen Authentifizierungstypen finden Sie in den folgenden Abschnitten:

- [Verwenden der Dienstprinzipalauthentifizierung](#using-service-principal-authentication)
- [Verwenden verwalteter Identitäten für Azure-Ressourcenauthentifizierung](#managed-identity)

### <a name="using-service-principal-authentication"></a>Verwenden der Dienstprinzipalauthentifizierung

Wenn Sie die Dienstprinzipalauthentifizierung verwenden möchten, registrieren Sie in Azure Active Directory (Azure AD) eine Anwendungsentität, und gewähren Sie ihr Zugriff auf Data Lake Store. Eine ausführliche Anleitung finden Sie unter [Dienst-zu-Dienst-Authentifizierung](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Notieren Sie sich die folgenden Werte, die Sie zum Definieren des verknüpften Diensts verwenden:

- Anwendungs-ID
- Anwendungsschlüssel
- Mandanten-ID

>[!IMPORTANT]
> Erteilen Sie dem Dienstprinzipal die korrekte Berechtigung in Azure Data Lake Store:
>- Erteilen Sie **als Quelle** im Daten-Explorer unter „Zugriff“ mindestens die Berechtigung **Lesen und Ausführen**, um die Dateien in Ordnern/Unterordnern aufzulisten und zu kopieren, oder die Berechtigung **Lesen**, um eine einzelne Datei zu kopieren, und wählen Sie das Hinzufügen zu **diesem Ordner und allen Unterordnern** für „rekursiv“ und das Hinzufügen als **ein Zugriffsberechtigungseintrag und ein Standardberechtigungseintrag** aus. Es gelten keine Anforderungen für die Zugriffssteuerung (IAM) auf Kontoebene.
>- Erteilen Sie **als Senke** im Daten-Explorer unter „Zugriff“ mindestens die Berechtigung **Schreiben und Ausführen**, um untergeordnete Elemente im Ordner zu erstellen, und wählen Sie das Hinzufügen zu **diesem Ordner und allen Unterordnern** für „rekursiv“ und das Hinzufügen als **ein Zugriffsberechtigungseintrag und ein Standardberechtigungseintrag** aus. Wenn Sie Azure IR für die Erstellung von Kopien verwenden (sowohl Quelle als auch Senke befinden sich in der Cloud), erteilen Sie in der Kontozugriffssteuerung (IAM) mindestens die Rolle **Leser**, um Data Factory die Erkennung der Data Lake Store-Region zu ermöglichen. Wenn Sie diese IAM-Rolle vermeiden möchten, [erstellen Sie explizit eine Azure IR-Instanz](create-azure-integration-runtime.md#create-azure-ir) mit dem Standort von Data Lake Store, und ordnen Sie sie wie im folgenden Beispiel im verknüpften Data Lake Store-Dienst zu.

>[!NOTE]
>Wenn Sie das **Tool zum Kopieren von Daten** zum Erstellen einer Kopierpipeline bzw. die **ADF-UI** zum Testen von Verbindungs-/Navigationsordnern während der Erstellung verwenden, muss die Berechtigung des Dienstprinzipals **auf Stammebene mit der Berechtigung „Execute“** gewährt werden, um Ordner aufzuführen, die im Stamm beginnen. Die Ausführung von Kopiervorgängen kann jedoch funktionieren, solange die Berechtigung für die zu kopierenden Daten erteilt wird. Sie können die Erstellungsvorgänge überspringen, wenn Sie die Berechtigung einschränken möchten.

Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | JA |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | JA |
| Mandant | Geben Sie die Mandanteninformationen (Domänenname oder Mandanten-ID) für Ihre Anwendung an. Diese können Sie abrufen, indem Sie im Azure-Portal mit der Maus auf den Bereich oben rechts zeigen. | JA |

**Beispiel:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Verwenden verwalteter Identitäten für Azure-Ressourcenauthentifizierung

Eine Data Factory kann einer [verwalteten Identität für Azure-Ressourcen](data-factory-service-identity.md) zugeordnet werden, die diese spezielle Data Factory darstellt. Sie können diese Dienstidentität direkt für die Data Lake Store-Authentifizierung verwenden, ähnlich wie bei der Verwendung Ihres eigenen Dienstprinzipals. Sie erlaubt dieser bestimmten Factory den Zugriff auf und das Kopieren von Daten von/zu Ihrem Data Lake Store.

So verwenden Sie verwaltete Identitäten für die Azure-Ressourcenauthentifizierung:

1. [Rufen Sie die Data Factory-Dienstidentität](data-factory-service-identity.md#retrieve-service-identity) ab, indem Sie den Wert von „DIENSTIDENTITÄTSANWENDUNGS-ID“ kopieren, der zusammen mit der Factory generiert wurde.
2. Gewähren Sie der Dienstidentität auf dieselbe Weise Zugriff auf Data Lake Store wie beim Dienstprinzipal, und beachten Sie dabei die folgenden Hinweise.

>[!IMPORTANT]
> Stellen Sie sicher, dass Sie der Data Factory-Dienstidentität die richtige Berechtigung in Azure Data Lake Store gewähren:
>- Erteilen Sie **als Quelle** im Daten-Explorer unter „Zugriff“ mindestens die Berechtigung **Lesen und Ausführen**, um die Dateien in Ordnern/Unterordnern aufzulisten und zu kopieren, oder die Berechtigung **Lesen**, um eine einzelne Datei zu kopieren, und wählen Sie das Hinzufügen zu **diesem Ordner und allen Unterordnern** für „rekursiv“ und das Hinzufügen als **ein Zugriffsberechtigungseintrag und ein Standardberechtigungseintrag** aus. Es gelten keine Anforderungen für die Zugriffssteuerung (IAM) auf Kontoebene.
>- Erteilen Sie **als Senke** im Daten-Explorer unter „Zugriff“ mindestens die Berechtigung **Schreiben und Ausführen**, um untergeordnete Elemente im Ordner zu erstellen, und wählen Sie das Hinzufügen zu **diesem Ordner und allen Unterordnern** für „rekursiv“ und das Hinzufügen als **ein Zugriffsberechtigungseintrag und ein Standardberechtigungseintrag** aus. Wenn Sie Azure IR für die Erstellung von Kopien verwenden (sowohl Quelle als auch Senke befinden sich in der Cloud), erteilen Sie in der Kontozugriffssteuerung (IAM) mindestens die Rolle **Leser**, um Data Factory die Erkennung der Data Lake Store-Region zu ermöglichen. Wenn Sie diese IAM-Rolle vermeiden möchten, [erstellen Sie explizit eine Azure IR-Instanz](create-azure-integration-runtime.md#create-azure-ir) mit dem Standort von Data Lake Store, und ordnen Sie sie wie im folgenden Beispiel im verknüpften Data Lake Store-Dienst zu.

>[!NOTE]
>Wenn Sie das **Tool zum Kopieren von Daten** zum Erstellen einer Kopierpipeline bzw. die **ADF-UI** zum Testen von Verbindungs-/Navigationsordnern während der Erstellung verwenden, muss die Berechtigung **auf Stammebene mit der Berechtigung „Execute“** gewährt werden, um Ordner aufzuführen, die im Stamm beginnen. Die Ausführung von Kopiervorgängen kann jedoch funktionieren, solange die Berechtigung für die zu kopierenden Daten erteilt wird. Sie können die Erstellungsvorgänge überspringen, wenn Sie die Berechtigung einschränken möchten.

In Azure Data Factory müssen Sie außer den allgemeinen Data Lake Store-Informationen im verknüpften Dienst keine weiteren Eigenschaften angeben.

**Beispiel:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Azure Data Lake Store-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten nach bzw. aus Azure Data Lake Store die type-Eigenschaft des Datasets auf **AzureDataLakeStoreFile** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **AzureDataLakeStoreFile** festgelegt werden. |JA |
| folderPath | Pfad zum Ordner in Data Lake Store. Der Platzhalterfilter wird nicht unterstützt. Wenn keine Angabe vorhanden ist, wird auf das Stammverzeichnis verwiesen. Beispiel: „<Stammordner>/<Unterordner>/“ |Nein  |
| fileName | **Name oder Platzhalterfilter** für die Dateien unter dem angegebenen Wert für „folderPath“. Wenn Sie für diese Eigenschaft keinen Wert angeben, verweist das Dataset auf alle Dateien im Ordner. <br/><br/>Für Filter sind folgende Platzhalter zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen).<br/>- Beispiel 1: `"fileName": "*.csv"`<br/>- Beispiel 2: `"fileName": "???20180427.txt"`<br/>Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Dateiname einen Platzhalter oder dieses Escapezeichen enthält.<br/><br/>Wenn „fileName“ nicht für ein Ausgabedataset und **preserveHierarchy** nicht in der Aktivitätssenke angegeben ist, generiert die Kopieraktivität den Dateinamen automatisch mit dem folgenden Format: *Data.[Aktivitätsausführungs-GUID].[GUID bei FlattenHierarchy].[Format, sofern konfiguriert].[Komprimierung, sofern konfiguriert]*. Ein Beispiel hierfür ist „Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz“. |Nein  |
| format | Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen.<br/><br/>Wenn Dateien mit einem bestimmten Format analysiert oder generiert werden sollen, werden die folgenden Formattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Unterstützte Typen sind: **Gzip**, **Deflate**, **bzip2** und **ZipDeflate**.<br/>Unterstützte Grade sind: **Optimal** und **Schnellste**. |Nein  |

>[!TIP]
>Wenn Sie alle Dateien eines Ordners kopieren möchten, geben Sie nur **folderPath** an.<br>Wenn Sie eine einzelne Datei mit einem bestimmten Namen kopieren möchten, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Dateinamen an.<br>Wenn Sie eine Teilmenge der Dateien eines Ordners kopieren möchten, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Platzhalterfilter an. 

**Beispiel:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Azure Data Lake Store-Quelle und -Senke unterstützt werden.

### <a name="azure-data-lake-store-as-source"></a>Azure Data Lake Store als Quelle

Legen Sie zum Kopieren von Daten aus Azure Data Lake Store den Quelltyp in der Kopieraktivität auf **AzureDataLakeStoreSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **AzureDataLakeStoreSource** festgelegt werden. |JA |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Beachten Sie Folgendes: Wenn „recursive“ auf TRUE festgelegt und die Senke ein dateibasierter Speicher ist, wird ein leerer Ordner/Unterordner nicht in die Senke kopiert bzw. nicht in ihr erstellt.<br/>Zulässige Werte sind **true** (Standard) oder **false**. | Nein  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS input dataset name>",
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
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Azure Data Lake Store als Senke

Legen Sie zum Kopieren von Daten in Azure Data Lake Store den Senkentyp in der Kopieraktivität auf **AzureDataLakeStoreSink** fest. Folgende Eigenschaften werden im Abschnitt **sink** unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **AzureDataLakeStoreSink** festgelegt werden. |JA |
| copyBehavior | Definiert das Kopierverhalten, wenn es sich bei der Quelle um Dateien aus einem dateibasierten Datenspeicher handelt.<br/><br/>Zulässige Werte sind:<br/><b>- PreserveHierarchy (Standard)</b>: Die Dateihierarchie wird im Zielordner beibehalten. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><b>- FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich in der ersten Ebene des Zielordners. Für die Zieldateien wird ein automatisch ein Name erzeugt. <br/><b>- MergeFiles</b>: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Datei-/Blob-Name angegeben wurde, entspricht der Name dem angegebenen Namen, andernfalls dem automatisch generierten Dateinamen. | Nein  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

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

---
title: Kopieren von Daten nach oder aus Azure Blob Storage mit Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie mithilfe von Data Factory Daten aus unterstützten Quelldatenspeichern nach Azure Blob Storage oder aus Blob Storage in unterstützte Senkendatenspeicher kopiert werden.
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: jingwang
ms.openlocfilehash: 83be53edf240220726639b51381b487c5b742cee
ms.sourcegitcommit: 3dcb1a3993e51963954194ba2a5e42260d0be258
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50754085"
---
# <a name="copy-data-to-or-from-azure-blob-storage-by-using-azure-data-factory"></a>Kopieren von Daten nach oder aus Azure Blob Storage mit Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1](v1/data-factory-azure-blob-connector.md)
> * [Aktuelle Version](connector-azure-blob-storage.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten nach und aus Azure Blob Storage zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

Informationen zu Azure Data Factory finden Sie im [Einführungsartikel](introduction.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Zudem können Sie Daten aus jedem unterstützten Quelldatenspeicher nach Blob Storage kopieren. Sie können auch Daten aus Blob Storage in jeden unterstützten Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md).

Dieser Blob Storage-Connector unterstützt insbesondere Folgendes:

- Kopieren von Daten in bzw. aus Azure Storage-Konten für allgemeine Zwecke und heißen/kalten Blob Storage. 
- Kopieren von Blobs mithilfe des Kontoschlüssels, der Dienst-SAS, des Dienstprinzipals oder verwalteter Identitäten für Azure-Ressourcenauthentifizierungen.
- Kopieren von Blobs aus Block-, Anfüge- oder Seitenblobs und Kopieren von Daten ausschließlich in Blockblobs. Azure Storage Premium wird als Senke nicht unterstützt, da dieser Dienst über Seitenblobs funktioniert.
- Kopieren von Blobs im jeweiligen Zustand oder Analysieren bzw. Generieren von Blobs mit den [unterstützten Dateiformaten und Komprimierungscodecs](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Blob Storage verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Der Azure Blob-Connector unterstützt die folgenden Authentifizierungstypen (Details finden Sie im entsprechenden Abschnitt):

- [Kontoschlüsselauthentifizierung](#account-key-authentication)
- [SAS-Authentifizierung (Shared Access Signature)](#shared-access-signature-authentication)
- [Dienstprinzipalauthentifizierung](#service-principal-authentication)
- [Verwaltete Identitäten für Azure-Ressourcenauthentifizierung](#managed-identity)

>[!NOTE]
>HDInsight, Azure Machine Learning und die Azure SQL Data Warehouse PolyBase-Last unterstützen nur Authentifizierung mit dem Azure Blob Storage-Kontoschlüssel.

### <a name="account-key-authentication"></a>Kontoschlüsselauthentifizierung

Für die Verwendung der Authentifizierung mit dem Speicherkontoschlüssel werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureBlobStorage** (empfohlen) oder **AzureStorage** (siehe Hinweise unten) festgelegt werden. |JA |
| connectionString | Geben Sie für die connectionString-Eigenschaft die Informationen ein, die zum Herstellen einer Verbindung mit Azure Storage erforderlich sind. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |JA |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

>[!NOTE]
>Wenn Sie den verknüpften Dienst vom Typ „AzureStorage“ verwendet haben, wird dies weiterhin unterstützt. Es wird jedoch empfohlen, in Zukunft diesen neuen verknüpften Dienst vom Typ „AzureBlobStorage“ zu verwenden.

**Beispiel:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
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

### <a name="shared-access-signature-authentication"></a>SAS-Authentifizierung (Shared Access Signature)

Shared Access Signatures bieten delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Sie können eine SAS verwenden, um einem Client für einen bestimmten Zeitraum eingeschränkte Berechtigungen für Objekte in Ihrem Speicherkonto zu gewähren. Sie müssen die Zugriffsschlüssel für Ihr Konto nicht freigeben. Die SAS ist ein URI, dessen Abfrageparameter alle erforderlichen Informationen für den authentifizierten Zugriff auf eine Speicherressource enthalten. Um mit der SAS auf Speicherressourcen zuzugreifen, muss der Client diese nur an den entsprechenden Konstruktor bzw. die entsprechende Methode übergeben. Weitere Informationen zu Shared Access Signatures finden Sie unter [Verwenden von Shared Access Signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
>- Data Factory unterstützt jetzt **SAS (Shared Access Signatures) für Dienste** sowie für **Konten**. Weitere Informationen zu diesen beiden SAS-Arten und ihrer Erstellung finden Sie unter [Arten von Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures).
>- In der späteren Datasetkonfiguration ist der Ordnerpfad der absolute Pfad ab der Containerebene. Sie müssen einen Pfad konfigurieren, der sich am Pfad in Ihrem SAS-URI orientiert.

> [!TIP]
> Um eine Dienst-SAS für Ihr Speicherkonto zu generieren, können Sie die folgenden PowerShell-Befehle ausführen. Ersetzen Sie die Platzhalter, und gewähren Sie die erforderliche Berechtigung.
> `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Für die Verwendung der SAS-Authentifizierung werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureBlobStorage** (empfohlen) oder **AzureStorage** (siehe Hinweise unten) festgelegt werden. |JA |
| sasUri | Geben Sie den SAS-URI für die Storage-Ressourcen wie Blobs, Container oder Tabellen an. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |JA |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

>[!NOTE]
>Wenn Sie den verknüpften Dienst vom Typ „AzureStorage“ verwendet haben, wird dies weiterhin unterstützt. Es wird jedoch empfohlen, in Zukunft diesen neuen verknüpften Dienst vom Typ „AzureBlobStorage“ zu verwenden.

**Beispiel:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Berücksichtigen Sie beim Erstellen eines SAS-URIs die folgenden Aspekte:

- Legen Sie geeignete Lese-/Schreib-Berechtigungen für Objekte fest, basierend auf der Verwendung des verknüpften Diensts in Ihrer Data Factory (Lesen, Schreiben, Lesen/Schreiben).
- Legen Sie für **Ablaufzeit** einen geeigneten Wert fest. Stellen Sie sicher, dass der Zugriff auf Storage-Objekte nicht während des aktiven Zeitraums der Pipeline abläuft.
- Der URI sollte je nach Bedarf auf der richtigen Container-/Blob- oder Tabellenebene erstellt werden. Ein SAS-URI zu einem Blob ermöglicht Data Factory den Zugriff auf dieses spezielle Blob. Ein SAS-URI zu einem Blobspeichercontainer ermöglicht Data Factory das Durchlaufen von Blobs in diesem Container. Wenn Sie den Zugriff später auf mehr Objekte ausweiten oder auf weniger Objekte beschränken oder den SAS-URI aktualisieren möchten, denken Sie daran, den verknüpften Dienst mit dem neuen URI zu aktualisieren.

### <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung

Informationen zur Dienstprinzipalauthentifizierung für Azure Storage im Allgemeinen finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mithilfe von Azure Active Directory](../storage/common/storage-auth-aad.md).

Zum Verwenden der Dienstprinzipalauthentifizierung führen Sie die folgenden Schritte aus:

1. Registrieren Sie eine Anwendungsentität in Azure Active Directory (Azure AD), indem Sie die Anleitungen unter [Registrieren Ihrer Anwendung bei einem Azure AD-Mandanten](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant) befolgen. Notieren Sie sich die folgenden Werte, die Sie zum Definieren des verknüpften Diensts verwenden:

    - Anwendungs-ID
    - Anwendungsschlüssel
    - Mandanten-ID

2. Erteilen Sie dem Dienstprinzipal die entsprechenden Berechtigung in Azure Blob Storage. Weitere Details zu den Rollen finden Sie unter [Verwalten von Zugriffsrechten für Azure Storage-Daten mit RBAC](../storage/common/storage-auth-aad-rbac.md).

    - Weisen Sie **Als Quelle** in der Zugriffssteuerung (IAM) mindestens die Rolle **Storage-Blobdatenleser** zu.
    - Weisen Sie **Als Senke** in der Zugriffssteuerung (IAM) mindestens die Rolle **Mitwirkender an Storage-Blobdaten** zu.

Diese Eigenschaften werden für den mit Azure Blob Storage verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureBlobStorage** festgelegt werden. |JA |
| serviceEndpoint | Geben Sie den Azure Blob Storage-Dienstendpunkt mit dem Muster `https://<accountName>.blob.core.windows.net/` an. |JA |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | JA |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als **SecureString**, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | JA |
| Mandant | Geben Sie die Mandanteninformationen (Domänenname oder Mandanten-ID) für Ihre Anwendung an. Diese können Sie abrufen, indem Sie den Mauszeiger über den rechten oberen Bereich im Azure-Portal bewegen. | JA |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

>[!NOTE]
>Dienstprinzipalauthentifizierung wird nur durch den verknüpften Dienst vom Typ „AzureBlobStorage“unterstützt, nicht jedoch vom vorherigen verknüpften Dienst vom Typ „AzureStorage“.

**Beispiel:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/",
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

Informationen zur MSI-Authentifizierung für Azure Storage im Allgemeinen finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mithilfe von Azure Active Directory](../storage/common/storage-auth-aad.md).

Um verwaltete Identitäten für die Azure-Ressourcenauthentifizierung zu verwenden, gehen Sie folgendermaßen vor:

1. [Rufen Sie die Data Factory-Dienstidentität](data-factory-service-identity.md#retrieve-service-identity) ab, indem Sie den Wert von „DIENSTIDENTITÄTSANWENDUNGS-ID“ kopieren, der zusammen mit der Factory generiert wurde.

2. Erteilen Sie dem Dienstprinzipal die entsprechenden Berechtigung in Azure Blob Storage. Weitere Details zu den Rollen finden Sie unter [Verwalten von Zugriffsrechten für Azure Storage-Daten mit RBAC](../storage/common/storage-auth-aad-rbac.md).

    - Weisen Sie **Als Quelle** in der Zugriffssteuerung (IAM) mindestens die Rolle **Storage-Blobdatenleser** zu.
    - Weisen Sie **Als Senke** in der Zugriffssteuerung (IAM) mindestens die Rolle **Mitwirkender an Storage-Blobdaten** zu.

Diese Eigenschaften werden für den mit Azure Blob Storage verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureBlobStorage** festgelegt werden. |JA |
| serviceEndpoint | Geben Sie den Azure Blob Storage-Dienstendpunkt mit dem Muster `https://<accountName>.blob.core.windows.net/` an. |JA |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

> [!NOTE]
> Verwaltete Identitäten für die Azure-Ressourcenauthentifizierung werden nur vom verknüpften Dienst des Typs „AzureBlobStorage“ unterstützt, nicht jedoch vom vorherigen verknüpften Dienst des Typs „AzureStorage“. 

**Beispiel:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste mit Eigenschaften, die vom Blob Storage-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus und nach Blob Storage die type-Eigenschaft des Datasets auf **AzureBlob** fest. Die folgenden Eigenschaften werden unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **AzureBlob** festgelegt werden. |JA |
| folderPath | Der Pfad zum Container und Ordner im Blobspeicher. Der Platzhalterfilter wird nicht unterstützt. Beispiel: myblobcontainer/myblobfolder/. |JA |
| fileName | **Name oder Platzhalterfilter** für die Blobs unter dem angegebenen Wert für „folderPath“. Wenn Sie für diese Eigenschaft keinen Wert angeben, zeigt das Dataset auf alle Blobs im Ordner. <br/><br/>Für Filter sind folgende Platzhalter zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen).<br/>- Beispiel 1: `"fileName": "*.csv"`<br/>- Beispiel 2: `"fileName": "???20180427.txt"`<br/>Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Dateiname einen Platzhalter oder dieses Escapezeichen enthält.<br/><br/>Wenn „fileName“ nicht für ein Ausgabedataset und **preserveHierarchy** nicht in der Aktivitätssenke angegeben ist, generiert die Kopieraktivität den Blobnamen automatisch mit dem folgenden Muster: „*Data.[activity run id GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]*“. Ein Beispiel hierfür ist „Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz“. |Nein  |
| format | Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), überspringen Sie den Formatabschnitt in den Definitionen der Eingabe- und Ausgabedatasets.<br/><br/>Die folgenden Formate werden unterstützt, wenn Sie Dateien mit einem bestimmten Format analysieren oder generieren möchten,: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** und **ParquetFormat**. Sie müssen die **type**-Eigenschaft unter **format** auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Unterstützte Typen sind **GZip**, **Deflate**, **BZIP2** und **ZipDeflate**.<br/>Unterstützte Grade sind **Optimal** und **Schnellste**. |Nein  |

>[!TIP]
>Um alle Blobs in einem Ordner zu kopieren, geben Sie nur **folderPath** an.<br>Um einen einzelnen Blob mit einem angegebenen Namen zu kopieren, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Dateinamen an.<br>Um eine Teilmenge der Blobs in einem Ordner zu kopieren, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Platzhalterfilter an. 

**Beispiel:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Blob storage linked service name>",
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit Eigenschaften, die von der Blob Storage-Quelle und -Senke unterstützt werden.

### <a name="blob-storage-as-a-source-type"></a>Blob Storage als Quelltyp

Legen Sie zum Kopieren von Daten aus Blob Storage den Quelltyp in der Kopieraktivität auf **BlobSource** fest. Die folgenden Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **BlobSource** festgelegt werden. |JA |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Beachten Sie Folgendes: Wenn „recursive“ auf „true“ festgelegt ist und es sich bei der Senke um einen dateibasierten Speicher handelt, wird ein leerer Ordner oder Unterordner nicht in die Senke kopiert und dort auch nicht erstellt.<br/>Zulässige Werte sind **true** (Standard) und **false**. | Nein  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
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
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="blob-storage-as-a-sink-type"></a>Blob Storage als Senkentyp

Legen Sie zum Kopieren von Daten nach Blob Storage den Senkentyp in der Kopieraktivität auf **BlobSink** fest. Die folgenden Eigenschaften werden im Abschnitt **sink** unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **BlobSink** festgelegt werden. |JA |
| copyBehavior | Definiert das Kopierverhalten, wenn es sich bei der Quelle um Dateien aus einem dateibasierten Datenspeicher handelt.<br/><br/>Zulässige Werte sind:<br/><b>- PreserveHierarchy (Standard)</b>: Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><b>- FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich auf der ersten Ebene des Zielordners. Die Namen für die Zieldateien werden automatisch generiert. <br/><b>- MergeFiles</b>: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Datei- oder Blobname angegeben wurde, entspricht der Name der Zusammenführungsdatei dem angegebenen Namen. Andernfalls wird der Dateiname automatisch generiert. | Nein  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
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

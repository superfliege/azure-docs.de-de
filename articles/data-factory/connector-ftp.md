---
title: Kopieren von Daten von einem FTP-Server mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten von einem FTP-Server mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
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
ms.date: 09/18/2017
ms.author: jingwang
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 52c90de32e3545cdb1f0210dfa695c7bcb67bedc
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="copy-data-from-ftp-server-by-using-azure-data-factory"></a>Kopieren von Daten von einem FTP-Server mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-ftp-connector.md)
> * [Version 2 – Vorschau](connector-ftp.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten von einem FTP-Server zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (General Availability, GA) ist, lesen Sie [FTP-Connector in V1](v1/data-factory-ftp-connector.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Sie können Daten von einem FTP-Server in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der FTP-Connector unterstützt insbesondere Folgendes:

- Kopieren von Dateien unter Verwendung der **Standard**- oder **anonymen** Authentifizierung
- Kopieren von Dateien im jeweiligen Zustand oder Analysieren von Dateien mit den [unterstützten Dateiformaten und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md)

## <a name="get-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python-SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](create-self-hosted-integration-runtime.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für FTP verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit FTP verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **FtpServer** festgelegt werden. | Ja |
| host | Gibt den Namen oder die IP-Adresse des FTP-Servers an. | Ja |
| port | Gibt den Port, den der FTP-Server abhört, an.<br/>Zulässige Werte sind ganze Zahlen (Standardwert **21**). | Nein |
| enableSsl | Gibt an, ob FTP über einen SSL/TLS-Kanal verwendet werden soll.<br/>Zulässige Werte sind **true** (Standard) oder **false** | Nein |
| enableServerCertificateValidation | Gibt an, ob die Überprüfung des SSL-Zertifikats aktiviert werden soll, wenn Sie FTP über SSL/TLS-Kanal verwenden.<br/>Zulässige Werte sind **true** (Standard) oder **false** | Nein |
| authenticationType | Gibt den Authentifizierungstyp an.<br/>Zulässige Werte sind **Basic** oder **Anonymous** | Ja |
| userName | Gibt den Benutzer an, der Zugriff auf den FTP-Server hat. | Nein |
| password | Gibt das Kennwort für den Benutzer (userName) an. Legen Sie für dieses Feld „SecureString“ fest. | Nein |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. |Nein |

**Beispiel 1: Verwenden der anonymen Authentifizierung**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel 2: Verwenden der Standardauthentifizierung**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom FTP-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten von einem FTP-Server die type-Eigenschaft des Datasets auf **FileShare** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft des Datasets muss auf **FileShare** festgelegt werden. |Ja |
| folderPath | Pfad zum Ordner. Beispiel: „<Ordner>/<Unterordner>/“ |Ja |
| fileName | Geben Sie den Namen der Datei in der Eigenschaft **folderPath** an, wenn Sie einen Kopiervorgang aus einer bestimmten Datei durchführen möchten. Wenn Sie keine Werte für diese Eigenschaft angeben, verweist das Dataset auf alle Dateien im Ordner als Quelle. |Nein |
| fileFilter | Geben Sie einen Filter zur Auswahl einer Teilmenge der Dateien in "folderPath" statt alle Dateien an. Ist nur anwendbar, wenn „fileName“ nicht angegeben ist. <br/><br/>Zulässige Werte sind `*` (mehrere Zeichen) oder `?` (einzelnes Zeichen).<br/>- Beispiel 1: `"fileFilter": "*.log"`<br/>- Beispiel 2: `"fileFilter": 2017-09-??.txt"` |Nein |
| format | Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen.<br/><br/>Wenn Dateien mit einem bestimmten Format analysiert werden sollen, werden die folgenden Formattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Unterstützte Typen sind: **Gzip**, **Deflate**, **bzip2** und **ZipDeflate**.<br/>Unterstützte Grade sind: **Optimal** und **Schnellste**. |Nein |
| useBinaryTransfer | Gibt an, ob der binäre Übertragungsmodus verwendet werden soll. Für den Binärmodus (Standardwert) lautet der Wert „true“, und für ASCII lautet er „false“. |Nein |

**Beispiel:**

```json
{
    "name": "FTPDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<FTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der FTP-Quelle unterstützt werden.

### <a name="ftp-as-source"></a>FTP als Quelle

Legen Sie zum Kopieren von Daten von FTP den Quelltyp in der Kopieraktivität auf **FileSystemSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **FileSystemSource** festgelegt werden. |Ja |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden.<br/>Zulässige Werte sind **true** (Standard) oder **false**. | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<FTP input dataset name>",
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


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).

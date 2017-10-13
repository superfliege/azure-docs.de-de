---
title: Kopieren von Daten aus HTTP-Quellen mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten aus einer Cloud- oder lokalen HTTP-Quelle mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
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
ms.openlocfilehash: b27e852b70651cec34b200bde362ff0efae30226
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-http-endpoint-using-azure-data-factory"></a>Kopieren von Daten von einem HTTP-Endpunkt mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-http-connector.md)
> * [Version 2 – Vorschau](connector-http.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten von einem HTTP-Endpunkt zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (General Availability, GA) ist, lesen Sie [HTTP-Connector in V1](v1/data-factory-http-connector.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Sie können Daten aus einer HTTP-Quelle in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der HTTP-Connector unterstützt insbesondere Folgendes:

- Abrufen von Daten von einem HTTP/HTTPS-Endpunkt mithilfe der HTTP-Methode **GET** oder **POST**
- Abrufen von Daten mithilfe der Authentifizierungstypen: **anonym**, **Standard**, **Digest**, **Windows** und **ClientCertificate**
- Kopieren der HTTP-Antwort im jeweiligen Zustand oder Analysieren der HTTP-Antwort mit den [unterstützten Dateiformaten und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md)

Der Unterschied zwischen diesem Connector und dem [Webtabellenconnector](connector-web-table.md) besteht darin, dass Letzterer zur Extraktion von Tabelleninhalten aus HTML-Seiten verwendet wird.

## <a name="getting-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie Schritt-für-Schritt-Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den HTTP-Connector verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit HTTP verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **HttpServer** festgelegt werden. | Ja |
| url | Basis-URL zum Webserver | Ja |
| enableServerCertificateValidation | Geben Sie an, ob die Überprüfung des SSL-Zertifikats des Servers aktiviert werden soll, wenn eine Verbindung mit dem HTTP-Endpunkt hergestellt wird. | Nein. Der Standardwert ist TRUE. |
| authenticationType | Gibt den Authentifizierungstyp an. Zulässige Werte: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**. <br><br> Weitere Eigenschaften und JSON-Beispiele für diese Authentifizierungstypen finden Sie in den Abschnitten nach dieser Tabelle. | Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. |Nein |

### <a name="using-basic-digest-or-windows-authentication"></a>Verwenden der Authentifizierung des Typs „Basic“, „Digest“ oder „Windows“

Legen Sie die Eigenschaft „authenticationType“ auf **Basic**, **Digest** oder **Windows** fest, und geben Sie zusammen mit den im vorherigen Abschnitt beschriebenen generischen Eigenschaften folgende Eigenschaften an:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| userName | Benutzername zum Zugreifen auf den HTTP-Endpunkt | Ja |
| password | Kennwort für den Benutzer (userName). Legen Sie für dieses Feld „SecureString“ fest. | Ja |

**Beispiel**

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
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

### <a name="using-clientcertificate-authentication"></a>Verwenden der ClientCertificate-Authentifizierung

Legen Sie zur Verwendung der ClientCertificate-Authentifizierung die Eigenschaft „authenticationType“ auf **ClientCertificate** fest, und geben Sie zusammen mit den im vorherigen Abschnitt beschriebenen generischen Eigenschaften folgende Eigenschaften an:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| embeddedCertData | Base64-codierte Zertifikatdaten | Geben Sie entweder `embeddedCertData` oder `certThumbprint` an. |
| certThumbprint | Der Fingerabdruck des Zertifikats, der im Zertifikatspeicher Ihres Computers mit der selbstgehosteten Integrationslaufzeit installiert wurde. Dieser ist nur anwendbar, wenn für „connectVia“ eine Integrationslaufzeit vom Typ „selbstgehostet“ angegeben wird. | Geben Sie entweder `embeddedCertData` oder `certThumbprint` an. |
| password | Das Kennwort des Zertifikats Legen Sie für dieses Feld „SecureString“ fest. | Nein |

Wenn Sie „certThumbprint“ für die Authentifizierung verwenden und das Zertifikat im persönlichen Speicher des lokalen Computers installiert wird, müssen Sie der selbstgehosteten Integrationslaufzeit Leseberechtigungen gewähren:

1. Starten Sie die Microsoft Management Console (MMC). Fügen Sie das für **Lokaler Computer** vorgesehene Snap-In **Zertifikate** hinzu.
2. Erweitern Sie **Zertifikate** > **Personal** (Persönlich), und klicken Sie auf **Zertifikate**.
3. Klicken Sie im persönlichen Speicher mit der rechten Maustaste auf das Zertifikat, und wählen Sie **Alle Aufgaben** -> **Private Schlüssel verwalten...**.
3. Fügen Sie auf der Registerkarte **Sicherheit** das Benutzerkonto hinzu, unter dem der Hostdienst der Integrationslaufzeit (DIAHostService) mit dem Lesezugriff auf das Zertifikat ausgeführt wird.

**Beispiel 1: Verwenden von „certThumbprint“**

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "HttpServer",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel 2: Verwenden von „embeddedCertData“**

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "HttpServer",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<base64 encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom HTTP-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus HTTP die type-Eigenschaft des Datasets auf **HttpFile** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft des Datasets muss auf **HttpFile** festgelegt werden. | Ja |
| relativeUrl | Eine relative URL zu der Ressource, die die Daten enthält. Wenn diese Eigenschaft nicht angegeben ist, wird nur die URL verwendet, die in der Definition des verknüpften Diensts angegeben ist. | Nein |
| requestMethod | HTTP-Methode.<br/>Zulässige Werte sind **Get** (Standard) oder **Post**. | Nein |
| additionalHeaders | Zusätzliche HTTP-Anforderungsheader | Nein |
| requestBody | Text für die HTTP-Anforderung | Nein |
| format | Wenn Sie **Daten vom HTTP-Endpunkt im vorliegenden Zustand** abrufen möchten, ohne diese analysieren und in einen dateibasierten Speicher kopieren zu müssen, überspringen Sie den Abschnitt „format“ in den Definitionen des Eingabe- und Ausgabedatasets.<br/><br/>Wenn der HTTP-Antwortinhalt während des Kopierens analysiert werden soll, werden die folgenden Dateiformattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Unterstützte Typen sind: **Gzip**, **Deflate**, **bzip2** und **ZipDeflate**.<br/>Unterstützte Grade sind: **Optimal** und **Schnellste**. |Nein |

**Beispiel 1: Verwenden der Get-Methode (Standard)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Beispiel 2: Verwenden der Post-Methode**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der HTTP-Quelle unterstützt werden.

### <a name="http-as-source"></a>HTTP als Quelle

Legen Sie zum Kopieren von Daten von HTTP den Quelltyp in der Kopieraktivität auf **HttpSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **HttpSource** festgelegt werden. | Ja |
| httpRequestTimeout | Das Timeout (TimeSpan) für die HTTP-Anforderung, um eine Antwort zu empfangen. Dabei handelt es sich um das Timeout zum Empfangen einer Antwort, nicht das Timeout zum Lesen von Antwortdaten.<br/> Der Standardwert ist „00:01:40“.  | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
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
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
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
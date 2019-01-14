---
title: Kopieren von Daten aus einer HTTP-Quelle mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Daten aus einer Cloud- oder lokalen HTTP-Quelle mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/24/2018
ms.author: jingwang
ms.openlocfilehash: 1f3e9be3a0048c4bf2e87ac23cbdc76b1aaa649f
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166405"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Kopieren von Daten von einem HTTP-Endpunkt mithilfe von Azure Data Factory

> [!div class="op_single_selector" title1="Wählen Sie die Version des Data Factory-Dienstes aus, den Sie verwenden:"]
> * [Version 1](v1/data-factory-http-connector.md)
> * [Aktuelle Version](connector-http.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten von einem HTTP-Endpunkt zu kopieren. Dieser Artikel baut auf dem Artikel zur [Kopieraktivität in Azure Data Factory](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer HTTP-Quelle in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die die Kopieraktivität als Quellen und Senken unterstützt, finden Sie unter [Unterstützte Datenspeicher und Formate](copy-activity-overview.md#supported-data-stores-and-formats).

Sie können diesen HTTP-Connector für die folgenden Aufgaben verwenden:

- Abrufen von Daten von einem HTTP/HTTPS-Endpunkt mithilfe der HTTP-Methoden **GET** oder **POST**.
- Abrufen von Daten mithilfe der folgenden Authentifizierungstypen: **Anonymous**, **Basic**, **Digest**, **Windows** und **ClientCertificate**.
- Kopieren der HTTP-Antwort im jeweiligen Zustand oder Analysieren der HTTP-Antwort mit den [unterstützten Dateiformaten und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md).

Der Unterschied zwischen diesem Connector und dem [Webtabellenconnector](connector-web-table.md) besteht darin, dass der Webtabellenconnector zur Extraktion von Tabelleninhalten aus einer HTML-Webseite verwendet wird.

> [!TIP]
> Um eine HTTP-Anforderung für den Datenabruf zu testen, bevor Sie den HTTP-Connector in Data Factory konfigurieren, informieren Sie sich über die API-Spezifikation für Header- und Textanforderungen. Sie können Tools wie Postman oder einen Webbrowser für die Überprüfung verwenden.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

In den folgenden Abschnitten finden Sie Details zu Eigenschaften, mit denen Sie Data Factory-Entitäten definieren können, die spezifisch für den HTTP-Connector sind.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit HTTP verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft muss auf **HttpServer** festgelegt werden. | JA |
| URL | Die Basis-URL zum Webserver. | JA |
| enableServerCertificateValidation | Geben Sie an, ob die Überprüfung des SSL-Zertifikats des Servers aktiviert werden soll, wenn eine Verbindung mit einem HTTP-Endpunkt hergestellt wird. Wenn der HTTPS-Server ein selbstsigniertes Zertifikat verwendet, legen Sie diese Eigenschaft auf **FALSE** fest. | Nein <br /> (der Standardwert ist **TRUE**) |
| authenticationType | Gibt den Authentifizierungstyp an. Zulässige Werte: **Anonymous**, **Basic**, **Digest**, **Windows** und **ClientCertificate**. <br><br> Weitere Eigenschaften und JSON-Beispiele für diese Authentifizierungstypen, finden Sie in den Abschnitten, die auf diese Tabelle folgen. | JA |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder eine selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, verwendet diese Eigenschaft die standardmäßige Azure Integration Runtime. |Nein  |

### <a name="using-basic-digest-or-windows-authentication"></a>Verwenden der Authentifizierung des Typs „Basic“, „Digest“ oder „Windows“

Legen Sie die **authenticationType**-Eigenschaft auf **Basic**, **Digest** oder **Windows** fest. Geben Sie zusätzlich zu den im vorherigen Abschnitt beschriebenen generischen Eigenschaften die folgenden Eigenschaften an:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| userName | Der Benutzername, der für den Zugriff auf den HTTP-Endpunkt verwendet werden soll. | JA |
| password | Das Kennwort für den Benutzer (der Wert **userName**). Markieren Sie dieses Feld als Typ **SecureString**, um es sicher in Data Factory zu speichern. Sie können auch [auf ein Geheimnis verweisen, das in Azure Key Vault](store-credentials-in-key-vault.md) gespeichert ist. | JA |

**Beispiel**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<user name>",
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

Um ClientCertificate-Authentifizierung zu verwenden, legen Sie die Eigenschaft **authenticationType** auf **ClientCertificate** fest. Geben Sie zusätzlich zu den im vorherigen Abschnitt beschriebenen generischen Eigenschaften die folgenden Eigenschaften an:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| embeddedCertData | Base64-codierte Zertifikatdaten. | Geben Sie **embeddedCertData** oder **certThumbprint** an. |
| certThumbprint | Der Fingerabdruck des Zertifikats, das im Zertifikatspeicher Ihres Computers für die selbstgehostete Integration Runtime installiert wurde. Ist nur anwendbar, wenn für die **connectVia**-Eigenschaft eine Integration Runtime vom Typ „selbstgehostet“ angegeben wird. | Geben Sie **embeddedCertData** oder **certThumbprint** an. |
| password | Das Kennwort, das dem Zertifikat zugeordnet ist. Markieren Sie dieses Feld als Typ **SecureString**, um es sicher in Data Factory zu speichern. Sie können auch [auf ein Geheimnis verweisen, das in Azure Key Vault](store-credentials-in-key-vault.md) gespeichert ist. | Nein  |

Wenn Sie **certThumbprint** für die Authentifizierung verwenden und das Zertifikat im persönlichen Speicher des lokalen Computers installiert wird, erteilen Sie der selbstgehosteten Integration Runtime Leseberechtigungen:

1. Öffnen Sie die Microsoft Management Console (MMC). Fügen Sie das für **Lokaler Computer** vorgesehene Snap-In **Zertifikate** hinzu.
2. Erweitern Sie **Zertifikate** > **Persönlich**, und wählen Sie dann **Zertifikate** aus.
3. Klicken Sie im persönlichen Speicher mit der rechten Maustaste auf das Zertifikat, und wählen Sie dann **Alle Aufgaben** > **Private Schlüssel verwalten** aus.
3. Fügen Sie auf der Registerkarte **Sicherheit** das Benutzerkonto hinzu, unter dem der Hostdienst der Integration Runtime (DIAHostService) mit Lesezugriff auf das Zertifikat ausgeführt wird.

**Beispiel 1: Verwenden von „certThumbprint“**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
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
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<Base64-encoded cert data>",
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

Dieser Abschnitt enthält eine Liste der Eigenschaften, die das HTTP-Dataset unterstützt. 

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie unter [Datasets und verknüpfte Dienste](concepts-datasets-linked-services.md). 

Legen Sie zum Kopieren von Daten aus HTTP die **type**-Eigenschaft des Datasets auf **HttpFile** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft des Datasets muss auf **HttpFile** festgelegt werden. | JA |
| relativeUrl | Eine relative URL zu der Ressource, die die Daten enthält. Wenn die Eigenschaft nicht angegeben ist, wird nur die URL verwendet, die in der Definition des verknüpften Diensts angegeben ist. | Nein  |
| requestMethod | Die HTTP-Methode. Zulässige Werte sind **Get** (Standardwert) und **Post**. | Nein  |
| additionalHeaders | Zusätzliche HTTP-Anforderungsheader | Nein  |
| requestBody | Der Text der HTTP-Anforderung. | Nein  |
| format | Wenn Sie Daten vom HTTP-Endpunkt im vorliegenden Zustand abrufen möchten, ohne diese analysieren und in einen dateibasierten Speicher kopieren zu müssen, überspringen Sie den Abschnitt **format** in den Definitionen des Eingabe- und Ausgabedatasets.<br/><br/>Wenn der HTTP-Antwortinhalt während des Kopierens analysiert werden soll, werden die folgenden Dateiformattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** und **ParquetFormat**. Legen Sie die **type**-Eigenschaft unter **format** auf einen dieser Werte fest. Weitere Informationen finden Sie unter [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein  |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Unterstützte Typen: **Gzip**, **Deflate**, **bzip2** und **ZipDeflate**.<br/>Unterstützte Grade: **Optimal** und **Fastest**. |Nein  |

> [!NOTE]
> Die unterstützte Nutzlastgröße für HTTP-Anforderungen beträgt etwa 500 KB. Wenn die Nutzlast, die Sie an Ihre Webendpunkt übergeben möchten, größer als 500 KB ist, sollten Sie eine Aufteilung der Nutzlast in kleinere Blöcke erwägen.

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

Dieser Abschnitt enthält eine Liste der Eigenschaften, die die HTTP-Quelle unterstützt.

Eine vollständige Liste mit den verfügbaren Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie unter [Pipelines](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>HTTP als Quelle

Legen Sie zum Kopieren von Daten aus HTTP den **Quelltyp** in der Kopieraktivität auf **HttpSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft der Quelle der Kopieraktivität muss auf **HttpSource** festgelegt werden. | JA |
| httpRequestTimeout | Das Timeout (der Wert **TimeSpan**) für die HTTP-Anforderung, um eine Antwort zu empfangen. Bei diesem Wert handelt es sich um das Timeout zum Empfangen einer Antwort, nicht um das Timeout zum Lesen von Antwortdaten. Der Standardwert ist **00:01:40**.  | Nein  |

**Beispiel**

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

Eine Liste der Datenspeicher, die von der Kopieraktivität als Quellen und Senken in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher und Formate](copy-activity-overview.md#supported-data-stores-and-formats).

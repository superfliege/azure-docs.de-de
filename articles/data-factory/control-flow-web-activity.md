---
title: Webaktivität in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Webaktivität, eine der Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden, verwenden können, um einen REST-Endpunkt aus einer Pipeline aufzurufen.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: e6846661370fcad139730fc0443d9df54fa12a70
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="web-activity-in-azure-data-factory"></a>Webaktivität in Azure Data Factory
Die Webaktivität kann verwendet werden, um einen benutzerdefinierten REST-Endpunkt aus einer Data Factory-Pipeline aufzurufen. Sie können Datasets und verknüpfte Dienste zur Verwendung und für den Zugriff durch die Aktivität übergeben. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-introduction.md) weiter.

## <a name="syntax"></a>Syntax

```json
{  
   "name":"MyWebActivity",
   "type":"WebActivity",
   "typeProperties":{  
      "method":"Post",
      "url":"<URLEndpoint>",
      "headers":{  
         "Content-Type":"application/json"
      },
      "authentication":{  
         "type":"ClientCertificate",  
         "pfx":"****",
         "password":"****"
      },
      "datasets":[  
         {  
            "referenceName":"<ConsumedDatasetName>",
            "type":"DatasetReference",
            "parameters":{  
               ...
            }
         }
      ],
      "linkedServices":[  
         {  
            "referenceName":"<ConsumedLinkedServiceName>",
            "type":"LinkedServiceReference"
         }
      ]
   }
}

```

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
name | Name der Webaktivität | Zeichenfolge | Ja
type | Muss auf **WebActivity** festgelegt sein. | Zeichenfolge | Ja
method | REST-API-Methode für den Zielendpunkt. | Eine Zeichenfolge. <br/><br/>Unterstützte Typen: GET, POST, PUT | Ja
URL | Zielendpunkt und Pfad | Zeichenfolge (oder Ausdruck mit resultType der Zeichenfolge). Nach einer Minute tritt für die Aktivität ein Timeout mit einem Fehler auf, falls sie keine Antwort vom Endpunkt erhält. | Ja
headers | Header, die in der Anforderung gesendet werden. Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`. | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Ja, der Content-Type-Header ist erforderlich. `"headers":{ "Content-Type":"application/json"}`
body | Stellt die an den Endpunkt gesendete Nutzlast dar. Erforderlich für POST/PUT-Methoden.  | Zeichenfolge (oder Ausdruck mit resultType der Zeichenfolge). <br/><br/>Weitere Informationen finden Sie in den Details zum Schema der Anforderungsnutzlast im Abschnitt [Schema der Anforderungsnutzlast](#request-payload-schema). | Nein 
authentication | Die zum Aufrufen des Endpunkts verwendete Authentifizierungsmethode. Unterstützte Typen sind „Basic“ oder „ClientCertificate“. Weitere Informationen finden Sie im Abschnitt [Authentifizierung](#authentication). Wenn keine Authentifizierung erforderlich ist, schließen Sie diese Eigenschaft aus. | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Nein 
Datasets | Liste der Datasets, die an den Endpunkt übergeben werden. | Array von Datasetverweisen. Kann ein leeres Array sein. | Ja
linkedServices | Liste der verknüpften Dienste, die an den Endpunkt übergeben werden. | Array von Verweisen auf verknüpfte Dienste. Kann ein leeres Array sein. | Ja

> [!NOTE]
> REST-Endpunkte, die die Webaktivität aufruft, müssen eine Antwort vom Typ JSON zurückgeben. Nach einer Minute tritt für die Aktivität ein Timeout mit einem Fehler auf, falls sie keine Antwort vom Endpunkt erhält.

## <a name="authentication"></a>Authentifizierung

### <a name="none"></a>Keine
Wenn keine Authentifizierung erforderlich ist, schließen Sie die Eigenschaft „authentication“ nicht ein.

### <a name="basic"></a>Basic
Geben Sie Benutzername und Kennwort für die Standardauthentifizierung an. 

```json
"authentication":{  
   "type":"Basic,
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>Clientzertifikat
Geben Sie die Base64-codierten Inhalte einer PFX-Datei und das Kennwort an. 

```json
"authentication":{  
   "type":"ClientCertificate",
   "pfx":"****",   
   "password":"****"
}
```
## <a name="request-payload-schema"></a>Schema der Anforderungsnutzlast
Wenn Sie die POST/PUT-Methode verwenden, stellt die Eigenschaft „body“ die Nutzlast dar, die an den Endpunkt gesendet wird. Sie können verknüpfte Dienste und Datasets als Teil der Nutzlast übergeben. Dies ist das Schema für die Nutzlast: 

```json
{
    "body": {
        "myMessage": "Sample",
        "datasets": [{
            "name": "MyDataset1",
            "properties": {
                ...
            }
        }],
        "linkedServices": [{
            "name": "MyStorageLinkedService1",
            "properties": {
                ...
            }
        }]
    }
} 
```

## <a name="example"></a>Beispiel
In diesem Beispiel ruft die Webaktivität in der Pipeline einen REST-Endpunkt auf. Sie übergibt einen verknüpften Azure SQL-Dienst und ein Azure SQL-Dataset an den Endpunkt. Der REST-Endpunkt verwendet die Azure SQL-Verbindungszeichenfolge für die Verbindung mit dem Azure SQL-Server und gibt den Namen der Instanz des SQL-Servers zurück. 

### <a name="pipeline-definition"></a>Definition der Pipeline

```json
{
    "name": "<MyWebActivityPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyWebActivity>",
                "type": "WebActivity",
                "typeProperties": {
                    "method": "Post",
                    "url": "@pipeline().parameters.url",
                    "headers": {
                        "Content-Type": "application/json"
                    },
                    "authentication": {
                        "type": "ClientCertificate",
                        "pfx": "*****",
                        "password": "*****"
                    },
                    "datasets": [
                        {
                            "referenceName": "MySQLDataset",
                            "type": "DatasetReference",
                            "parameters": {
                                "SqlTableName": "@pipeline().parameters.sqlTableName"
                            }
                        }
                    ],
                    "linkedServices": [
                        {
                            "referenceName": "SqlLinkedService",
                            "type": "LinkedServiceReference"
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "sqlTableName": {
                "type": "String"
            },
            "url": {
                "type": "String"
            }
        }
    }
}

```

### <a name="pipeline-parameter-values"></a>Parameterwerte der Pipeline

```json
{
    "sqlTableName": "department",
    "url": "https://adftes.azurewebsites.net/api/execute/running"
}

```

### <a name="web-service-endpoint-code"></a>Webdienst-Endpunktcode

```csharp

[HttpPost]
public HttpResponseMessage Execute(JObject payload)
{
    Trace.TraceInformation("Start Execute");

    JObject result = new JObject();
    result.Add("status", "complete");

    JArray datasets = payload.GetValue("datasets") as JArray;
    result.Add("sinktable", datasets[0]["properties"]["typeProperties"]["tableName"].ToString());

    JArray linkedServices = payload.GetValue("linkedServices") as JArray;
    string connString = linkedServices[0]["properties"]["typeProperties"]["connectionString"].ToString();

    System.Data.SqlClient.SqlConnection sqlConn = new System.Data.SqlClient.SqlConnection(connString);

    result.Add("sinkServer", sqlConn.DataSource);

    Trace.TraceInformation("Stop Execute");

    return this.Request.CreateResponse(HttpStatusCode.OK, result);
}

```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter anderen Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden: 

- [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md)
- [ForEach-Aktivität](control-flow-for-each-activity.md)
- [Aktivität „Metadaten abrufen“](control-flow-get-metadata-activity.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)

---
title: "Transformieren von Daten mit dem U-SQL-Skript – Azure | Microsoft-Dokumentation"
description: "Erläutert die Datenverarbeitung oder -transformation durch Ausführen von U-SQL-Skripts für einen Azure Data Lake Analytics-Computedienst."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: shengc
ms.openlocfilehash: 5e54464ceabfe1fea2af80d63e538bea6a0a50a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Transformieren von Daten durch Ausführen von U-SQL-Skripts für Azure Data Lake Analytics 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-usql-activity.md)
> * [Version 2 – Vorschauversion](transform-data-using-data-lake-analytics.md)

Eine Pipeline in einer Azure Data Factory verarbeitet Daten in verknüpften Speicherdiensten mithilfe verknüpfter Compute Services. Sie enthält eine Abfolge von Aktivitäten, wobei jede Aktivität einen bestimmten Verarbeitungsvorgang ausführt. In diesem Kapitel wird die **U-SQL-Aktivität von Data Lake Analytics** beschrieben, die ein **U-SQL**-Skript auf einem mit **Azure Data Lake Analytics** verknüpften Computedienst ausführt. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [USQL-Aktivität in V1](v1/data-factory-usql-activity.md).

Erstellen Sie ein Azure Data Lake Analytics-Konto, bevor Sie mit einer U-SQL-Aktivität von Data Lake Analytics eine Pipeline erstellen. Weitere Informationen zu Azure Data Lake Analytics finden Sie unter [Erste Schritte mit Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).


## <a name="azure-data-lake-analytics-linked-service"></a>Mit Azure Data Lake Analytics verknüpfter Dienst
Sie erstellen einen mit **Azure Data Lake Analytics** verknüpften Dienst, um einen Azure Data Lake Analytics-Computedienst mit einer Azure Data Factory zu verknüpfen. Die Data Lake Analytics-U-SQL-Aktivität in der Pipeline verweist auf diesen verknüpften Dienst. 

Die folgende Tabelle enthält Beschreibungen der allgemeinen Eigenschaften, die in der JSON-Definition verwendet werden. 

| Eigenschaft                 | Beschreibung                              | Erforderlich                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **type**                 | Legen Sie die Typeigenschaft auf **AzureDataLakeAnalytics**fest. | Ja                                      |
| **accountName**          | Name des Azure Data Lake Analytics-Kontos.  | Ja                                      |
| **dataLakeAnalyticsUri** | URI des Azure Data Lake Analytics-Kontos.           | Nein                                       |
| **subscriptionId**       | Azure-Abonnement-ID                    | Nein (falls nicht angegeben, wird das Abonnement der Data Factory verwendet). |
| **resourceGroupName**    | Azure-Ressourcengruppenname                | Nein (falls nicht angegeben, wird die Ressourcengruppe der Data Factory verwendet). |

### <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung
Der mit Azure Data Lake Analytics verknüpfte Dienst erfordert für die Verbindung mit dem Azure Data Lake Analytics-Dienst eine Dienstprinzipalauthentifizierung. Wenn Sie die Dienstprinzipalauthentifizierung verwenden möchten, registrieren Sie in Azure Active Directory (Azure AD) eine Anwendungsentität und gewähren ihr Zugriff auf Data Lake Analytics und den verwendeten Data Lake Store. Eine ausführliche Anleitung finden Sie unter [Dienst-zu-Dienst-Authentifizierung](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Notieren Sie sich die folgenden Werte, die Sie zum Definieren des verknüpften Diensts verwenden:
* Anwendungs-ID
* Anwendungsschlüssel 
* Mandanten-ID

Verwenden Sie die Dienstprinzipalauthentifizierung, indem Sie die folgenden Eigenschaften angeben:

| Eigenschaft                | Beschreibung                              | Erforderlich |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Geben Sie die Client-ID der Anwendung an.     | Ja      |
| **servicePrincipalKey** | Geben Sie den Schlüssel der Anwendung an.           | Ja      |
| **tenant**              | Geben Sie die Mandanteninformationen (Domänenname oder Mandanten-ID) für Ihre Anwendung an. Diese können Sie abrufen, indem Sie im Azure-Portal mit der Maus auf den Bereich oben rechts zeigen. | Ja      |

**Beispiel: Dienstprinzipalauthentifizierung**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics URI",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }       
    }
}
```

Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md).

## <a name="data-lake-analytics-u-sql-activity"></a>U-SQL-Aktivität für Data Lake Analytics
Der folgende JSON-Ausschnitt definiert eine Pipeline mit einer U-SQL-Aktivität für Data Lake Analytics. Die Aktivitätsdefinition verwendet einen Verweis auf den zuvor erstellten mit Azure Data Lake Analytics verknüpften Dienst. Um ein Data Lake Analytics-U-SQL-Skript auszuführen, übermittelt Data Factory das von Ihnen angegebene Skript an Data Lake Analytics. Die erforderlichen Ein- und Ausgaben werden im Skript definiert, damit Data Lake Analytics sie abruft und ausgibt. 

```json
{
    "name": "ADLA U-SQL Activity",
    "description": "description",
    "type": "DataLakeAnalyticsU-SQL",
    "linkedServiceName": {
        "referenceName": "AzureDataLakeAnalyticsLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "LinkedServiceofAzureBlobStorageforscriptPath",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
        "degreeOfParallelism": 3,
        "priority": 100,
        "parameters": {
            "in": "/datalake/input/SearchLog.tsv",
            "out": "/datalake/output/Result.tsv"
        }
    }   
}
```

Die folgende Tabelle beschreibt die Namen und Eigenschaften, die für diese Aktivität spezifisch sind. 

| Eigenschaft            | Beschreibung                              | Erforderlich |
| :------------------ | :--------------------------------------- | :------- |
| Name                | Name der Aktivität in der Pipeline     | Ja      |
| Beschreibung         | Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird.  | Nein       |
| Typ                | Für die Data Lake Analytics-U-SQL-Aktivität ist der Aktivitätstyp **DataLakeAnalyticsU-SQL**. | Ja      |
| linkedServiceName   | Mit Azure Data Lake Analytics verknüpfter Dienst. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md).  |Ja       |
| scriptPath          | Der Pfad zum Ordner, der das U-SQL-Skript enthält. Beim Dateinamen wird Groß-/Kleinschreibung unterschieden. | Ja      |
| scriptLinkedService | Verknüpfter Dienst, der den Speicher, der das Skript enthält, mit der Data Factory verknüpft. | Ja      |
| degreeOfParallelism | Die maximale Anzahl von Knoten, die zum Ausführen des Auftrags gleichzeitig verwendet werden. | Nein       |
| priority            | Bestimmt, welche der in der Warteschlange befindlichen Aufträge als erstes ausgeführt werden. Je niedriger die Zahl, desto höher die Priorität. | Nein       |
| parameters          | Parameter für das U-SQL-Skript          | Nein       |
| runtimeVersion      | Die Runtime-Version des zu verwendenden U-SQL-Moduls | Nein       |
| compilationMode     | <p>Der Kompilierungsmodus von U-SQL. Muss einen der folgenden Werte aufweisen: **Semantic:** Es werden nur Semantiküberprüfungen und erforderliche Integritätsprüfungen ausgeführt. **Full:** Es wird die vollständige Kompilierung ausgeführt, einschließlich Syntaxprüfung, Optimierung, Codegenerierung usw. **SingleBox:** Es wird die vollständige Kompilierung ausgeführt, wobei die TargetType-Einstellung auf „SingleBox“ festgelegt ist. Wenn Sie für diese Eigenschaft keinen Wert angeben, bestimmt der Server den optimalen Kompilierungsmodus. | Nein |

Die von Data Factory übermittelte Skriptdefinition finden Sie unter [Skriptdefinition „SearchLogProcessing.txt“](#sample-u-sql-script). 

## <a name="sample-u-sql-script"></a>U-SQL-Beispielskript

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

In obigem Skriptbeispiel wird die Ein- und Ausgabe des Skripts in den Parametern **@in** und **@out** definiert. Die Werte für die Parameter **@in** und **@out** im U-SQL-Skript werden von Data Factory dynamisch mithilfe des Abschnitts „parameters“ übergeben. 

Sie können in Ihrer Pipelinedefinition auch andere Eigenschaften wie etwa degreeOfParallelism oder „priority“ für die Aufträge angeben, die im Azure Data Lake Analytics-Dienst ausgeführt werden.

## <a name="dynamic-parameters"></a>Dynamische Parameter
In der beispielhaften Pipelinedefinition werden die Eingabe- und Ausgabeparameter mit hartcodierten Werten zugewiesen. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Anstelle von hartcodierten Werten können dynamische Parameter verwendet werden. Beispiel: 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

In diesem Fall werden die Eingabedateien weiterhin aus dem Ordner „/datalake/input“ abgerufen und die Ausgabedateien im Ordner „datalake/output“ generiert. Die Dateinamen sind dynamisch und basieren auf der Startzeit des Slices.  

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden: 

* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchausführungsaktivität](transform-data-using-machine-learning.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)

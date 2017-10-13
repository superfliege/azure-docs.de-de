---
title: "Erstellen von Datenpipelines für die Vorhersage mithilfe von Azure Data Factory | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine Vorhersagepipeline mithilfe der Azure Machine Learning-Batchausführungsaktivität in Azure Data Factory erstellen."
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
ms.openlocfilehash: 413f12d301a0e2c47048d23b2d4fb7de6423256d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Erstellen von Vorhersagepipelines mithilfe von Azure Machine Learning und Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-azure-ml-batch-execution-activity.md)
> * [Version 2 – Vorschauversion](transform-data-using-machine-learning.md)

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) ermöglicht es Ihnen, Predictive Analytics-Lösungen zu erstellen, zu testen und bereitzustellen. Allgemein betrachtet, geschieht dies in drei Schritten:

1. **Erstellen eines Trainingsexperiments**. Sie führen diesen Schritt mit Azure ML Studio aus. ML Studio ist eine visuelle Entwicklungsumgebung für die Zusammenarbeit, mit der Sie ein Predictive Analytics-Modell mithilfe von Trainingsdaten trainieren und testen können.
2. **Konvertierten in ein Vorhersageexperiment**. Sobald Ihr Modell mit vorhandenen Daten trainiert wurde, können Sie es verwenden, um neue Daten zu bewerten. Sie bereiten das Experiment auf die Bewertung vor und optimieren es.
3. **Bereitstellen des Experiments als Webdienst**. Sie können das Bewertungsexperiment als Azure-Webdienst veröffentlichen. Sie können Daten über diesen Webdienstendpunkt an Ihr Modell senden und Ergebnisvorhersagen vom Modell empfangen.  

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, lesen Sie [Machine Learning-Batchausführungsaktivität in V1](v1/data-factory-azure-ml-batch-execution-activity.md).


### <a name="data-factory-and-machine-learning-together"></a>Data Factory und Machine Learning zusammen
Azure Data Factory ermöglicht die einfache Erstellung von Pipelines, die einen veröffentlichten [Azure Machine Learning][azure-machine-learning]-Webdienst für Predictive Analytics nutzen. Mithilfe der **Batchausführungsaktivität** in einer Azure Data Factory-Pipeline können Sie einen Azure ML-Webdienst aufrufen, um Vorhersagen zu den Daten im Batch zu machen. 

Im Laufe der Zeit müssen die Vorhersagemodelle in den Azure ML-Bewertungsexperimenten mit neuen Eingabedatasets neu trainiert werden. Sie können ein Azure ML-Modell über eine Data Factory-Pipeline neu trainieren, indem Sie die folgenden Schritte ausführen:

1. Veröffentlichen Sie das Trainingsexperiment (nicht das Vorhersageexperiment) als Webdienst. Für diesen Schritt können Sie Azure ML Studio verwenden, wie Sie es beim Veröffentlichen des Vorhersageexperiments als Webdienst im vorherigen Szenario getan haben.
2. Verwenden Sie die Azure ML-Batchausführungsaktivität, um den Webdienst für das Trainingsexperiment aufzurufen. Grundsätzlich können Sie die Azure ML-Batchausführungsaktivität verwenden, um den Trainingswebdienst und den Bewertungswebdienst aufzurufen.

Nachdem Sie das erneute Training abgeschlossen haben, aktualisieren Sie den Bewertungswebdienst (das Vorhersageexperiment, das als Webdienst verfügbar gemacht wurde) mithilfe der **Azure ML-Aktivität zur Ressourcenaktualisierung** mit dem neu trainierten Modell. Einzelheiten finden Sie im Artikel [Aktualisieren von Modellen mithilfe der Ressourcenaktualisierungsaktivität](update-machine-learning-models.md).

## <a name="azure-machine-learning-linked-service"></a>Mit Azure Machine Learning verknüpfter Dienst

Sie können einen mit verknüpften **Azure Machine Learning**-Dienst erstellen, um einen Azure Machine Learning-Webdienst mit einer Azure Data Factory zu verknüpfen. Der verknüpfte Dienst wird von der Batchausführungsaktivität und [Ressourcenaktualisierungsaktivität](update-machine-learning-models.md) von Azure Machine Learning verwendet. 


```JSON
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "URL to Azure ML Predictive Web Service",
            "apiKey": {
                "type": "SecureString",
                "value": "api key"
            }
        }
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Im Artikel zu [von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md) finden Sie Beschreibungen der Eigenschaften in der JSON-Definition. 

Azure Machine Learning unterstützt für Ihr Vorhersageexperiment sowohl klassische als auch neue Webdienste. Sie können denjenigen wählen, den Sie in Data Factory verwenden möchten. Um die Informationen zur Erstellung des verknüpften Azure Machine Learning-Diensts zu erhalten, besuchen Sie „https://services.azureml.net“. Hier sind alle Ihre neuen und klassischen Webdienste aufgelistet. Klicken Sie auf den Webdienst, auf den Sie zugreifen möchten, und klicken Sie auf die Seite **Consume**. Kopieren Sie **Primary Key** für die **apiKey**-Eigenschaft und **Batch Requests** für die **mlEndpoint**-Eigenschaft. 

![Azure Machine Learning-Webdienste](./media/transform-data-using-machine-learning/web-services.png)

##<a name="azure-machine-learning-batch-execution-activity"></a>Azure Machine Learning-Batchausführungsaktivität

Der folgende JSON-Codeausschnitt definiert eine Azure Machine Learning-Batchausführungsaktivität. Die Aktivitätsdefinition verwendet einen Verweis auf den zuvor erstellten mit Azure Machine Learning verknüpften Dienst. 

```JSON
{
    "name": "AzureMLExecutionActivityTemplate",
    "description": "description",
    "type": "AzureMLBatchExecution",
    "linkedServiceName": {
        "referenceName": "AzureMLLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "webServiceInputs": {
            "<web service input name 1>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                }, 
                "FilePath":"path1"
            }, 
            "<web service input name 2>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference" 
                }, 
                "FilePath":"path2"
            }        
        },
        "webServiceOutputs": {
            "<web service output name 1>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"   
                }, 
                "FilePath":"path3"
            }, 
            "<web service output name 2>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"   
                }, 
                "FilePath":"path4"
            }         
        },
        "globalParameters": {
            "<Parameter 1 Name>": "<parameter value>",
            "<parameter 2 name>": "<parameter 2 value>"
        }
    }
}
```



| Eigenschaft          | Beschreibung                              | Erforderlich |
| :---------------- | :--------------------------------------- | :------- |
| Name              | Name der Aktivität in der Pipeline     | Ja      |
| Beschreibung       | Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird.  | Nein       |
| Typ              | Für die Data Lake Analytics-U-SQL-Aktivität ist der Aktivitätstyp **AzureMLBatchExecution**. | Ja      |
| linkedServiceName | Mit dem verknüpften Azure Machine Learning-Dienst verknüpfte Dienste. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md). | Ja      |
| webServiceInputs  | Schlüssel, Wertepaare, Zuordnung der Namen von Eingaben in den Azure Machine Learning-Webdienst. Der Schlüssel muss mit den Eingabeparametern übereinstimmen, die im veröffentlichten Azure Machine Learning-Webdienst definiert sind. Der Wert ist ein Paar aus verknüpften Azure Storage-Diensten und „FilePath“-Eigenschaften zum Angeben der Speicherorte von Eingabeblobs. | Nein       |
| webServiceOutputs | Schlüssel, Wertepaare, Zuordnung der Namen von Ausgaben aus dem Azure Machine Learning-Webdienst. Der Schlüssel muss mit den Ausgabeparametern übereinstimmen, die im veröffentlichten Azure Machine Learning-Webdienst definiert sind. Der Wert ist ein Paar aus verknüpften Azure Storage-Diensten und „FilePath“-Eigenschaften zum Angeben der Speicherorte von Ausgabeblobs. | Nein       |
| globalParameters  | Schlüssel, Wertepaare, die an den Endpunkt des Azure ML-Batchausführungsdienst übergeben werden. Schlüssel müssen mit den Webdienstparametern übereinstimmen, die im veröffentlichten Azure ML-Webdienst definiert sind. Werte werden in der „GlobalParameters“-Eigenschaft der Azure ML-Batchausführungsanforderung übergeben. | Nein       |

### <a name="scenario-1-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Szenario 1: Experimente mit Eingaben/Ausgaben für den Webdienst, die auf Daten in Azure Blob Storage verweisen

In diesem Szenario werden mit dem Azure Machine Learning-Webdienst anhand der Daten aus einer Datei eines Azure-Blob-Speichers Vorhersagen erstellt und die Vorhersageergebnisse im Blob-Speicher gespeichert. Das folgende JSON-Skript definiert eine Data Factory-Pipeline mit einer AzureMLBatchExecution-Aktivität. Auf die Eingabe- und Ausgabedaten in Azure Blob Storage wird über ein Paar aus „LinkedName“ und „FilePath“ verwiesen. In dem Beispiel des verknüpften Diensts sind die Ein- und Ausgaben unterschiedlich. Sie können verschiedene verknüpfte Dienste für alle Ihre Ein- und Ausgaben verwenden. Data Factory ist in der Lage, die richtigen Dateien auszuwählen und an den Azure ML-Webdienst zu senden. 

> [!IMPORTANT]
> In Ihrem Azure ML-Experiment haben Eingabe- und Ausgabeports von Webdiensten und globale Parameter Standardnamen („input1“, „input2“), die Sie anpassen können. Die Namen, die Sie für die Einstellungen webServiceInputs, webServiceOutputs und globalParameters verwenden, müssen den Namen in den Experimenten genau entsprechen. Sie können die Beispiel-Anforderungsnutzlast auf der Hilfeseite für die Batchausführung für Ihren Azure ML-Endpunkt anzeigen, um die erwartete Zuordnung zu überprüfen.
>
> 

```JSON
{
    "name": "AzureMLExecutionActivityTemplate",
    "description": "description",
    "type": "AzureMLBatchExecution",
    "linkedServiceName": {
        "referenceName": "AzureMLLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "webServiceInputs": {
            "input1": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                }, 
                "FilePath":"amltest/input/in1.csv"
            }, 
            "input2": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference" 
                }, 
                "FilePath":"amltest/input/in2.csv"
            }        
        },
        "webServiceOutputs": {
            "outputName1": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"   
                }, 
                "FilePath":"amltest2/output/out1.csv"
            }, 
            "outputName2": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"   
                }, 
                "FilePath":"amltest2/output/out2.csv"
            }         
        }
    }
}
```
### <a name="scenario-2-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Szenario 2: Experimente mit den Modulen „Import Data“ und „Output Data“ zum Verweisen auf Daten in verschiedenen Speichern
Ein weiteres gängiges Szenario beim Erstellen von Azure ML-Experimenten ist die Verwendung der Module „Import Data“ und „Output Data“. Das Modul „Import Data“ wird verwendet, um Daten in ein Experiment zu laden, während mit dem Modul „Output Data“ Daten aus Ihren Experimenten gespeichert werden. Einzelheiten zu den Modulen „Import Data“ und „Output Data“ finden Sie in der MSDN Library in den Themen [Import Data](https://msdn.microsoft.com/library/azure/dn905997.aspx) und [Output Data](https://msdn.microsoft.com/library/azure/dn905984.aspx).     

Bei Verwendung der Module „Import Data“ und „Output Data“ empfiehlt es sich, einen Webdienstparameter für jede Eigenschaft dieser Module zu verwenden. Durch diese Webparameter können Sie die Werte zur Laufzeit konfigurieren. Sie können z.B. ein Experiment mit dem Modul „Import Data“erstellen, das eine Azure SQL-Datenbank-Instanz mit dem Namen „XXX.database.windows.net“ verwendet. Nach Bereitstellung des Webdiensts sollen die Nutzer des Webdiensts eine weitere Azure SQL Server-Instanz mit dem Namen `YYY.database.windows.net` angeben können. Durch Verwendung eines Webdienstparameters wird ermöglicht, dass dieser Wert konfiguriert werden kann.

> [!NOTE]
> Eingaben und Ausgaben für den Webdienst unterscheiden sich von Webdienstparametern. Im ersten Szenario haben Sie gesehen, wie Eingaben und Ausgaben für einen Azure ML-Webdienst angegeben werden können. In diesem Szenario übergeben Sie Parameter für einen Webdienst, die Eigenschaften der Module „Import Data“ und „Output Data“ entsprechen.
>
> 

Betrachten wir nun ein Szenario für die Verwendung von Webdienstparametern. Sie haben einen Azure Machine Learning-Webdienst bereitgestellt, bei dem Daten mithilfe eines Reader-Moduls aus einer von Azure Machine Learning unterstützten Datenquelle (z.B. Azure SQL-Datenbank) gelesen werden. Nach der Batchausführung werden die Ergebnisse mit einem Writer-Modul (Azure SQL-Datenbank) geschrieben.  In den Experimenten sind keine Eingaben und Ausgaben für den Webdienst definiert. In diesem Fall wird empfohlen, die relevanten Webdienstparameter für das Reader- und das Writer-Modul zu konfigurieren. Bei dieser Konfiguration können das Reader- und Writer-Modul bei Verwendung der AzureMLBatchExecution-Aktivität konfiguriert werden. Sie geben die Webdienstparameter wie folgt im Abschnitt **globalParameters** im JSON-Code der Aktivität an.

```JSON
"typeProperties": {
    "globalParameters": {
        "Database server name": "<myserver>.database.windows.net",
        "Database name": "<database>",
        "Server user account name": "<user name>",
        "Server user account password": "<password>"
    }
}
```


> [!NOTE]
> Bei Webdienstparametern wird Groß-/Kleinschreibung unterschieden, weshalb Sie sicherstellen müssen, dass die Namen, die Sie im JSON-Code der Aktivität angeben, denjenigen entsprechen, die vom Webdienst verfügbar gemacht werden.
>

Nachdem Sie das erneute Training abgeschlossen haben, aktualisieren Sie den Bewertungswebdienst (das Vorhersageexperiment, das als Webdienst verfügbar gemacht wurde) mithilfe der **Azure ML-Aktivität zur Ressourcenaktualisierung** mit dem neu trainierten Modell. Einzelheiten finden Sie im Artikel [Aktualisieren von Modellen mithilfe der Ressourcenaktualisierungsaktivität](update-machine-learning-models.md).



## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden: 

* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)

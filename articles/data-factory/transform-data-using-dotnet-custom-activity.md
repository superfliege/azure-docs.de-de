---
title: "Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline"
description: "Erfahren Sie, wie Sie benutzerdefinierte Aktivitäten erstellen und in einer Azure Data Factory-Pipeline verwenden."
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
ms.date: 08/10/2017
ms.author: shengc
ms.openlocfilehash: e470071ca0ff45fce0a410b18ea9a91e1925af4b
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-use-custom-activities.md)
> * [Version 2 – Vorschauversion](transform-data-using-dotnet-custom-activity.md)

Es existieren zwei Aktivitätstypen, die Sie in einer Azure Data Factory-Pipeline verwenden können.

- [Datenverschiebungsaktivitäten](copy-activity-overview.md) zum Verschieben von Daten zwischen [unterstützten Quell- und Senkendatenspeichern](copy-activity-overview.md#supported-data-stores-and-formats).
- [Datentransformationsaktivitäten](transform-data.md) zum Transformieren von Daten mithilfe von Computediensten wie Azure HDInsight, Azure Batch und Azure Machine Learning. 

Wenn Sie Daten in einen bzw. aus einem von Data Factory nicht unterstützten Datenspeicher verschieben oder auf eine Weise transformieren/verarbeiten müssen, die von Data Factory nicht unterstützt wird, können Sie auch eine **benutzerdefinierte Aktivität** mit Ihrer eigenen Datenverschiebungs- bzw. -transformationslogik erstellen und in einer Pipeline verwenden. Die benutzerdefinierte Aktivität führt Ihre angepasste Codelogik in einem **Azure Batch**-Pool mit virtuellen Computern aus.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](v1/data-factory-use-custom-activities.md).
 

Lesen Sie die folgenden Themen, wenn Sie noch nicht mit dem Azure Batch-Dienst vertraut sind:

* [Azure Batch – Grundlagen](../batch/batch-technical-overview.md) finden Sie eine Übersicht über den Azure Batch-Dienst.
* Erstellen Sie ein Azure Batch-Konto mit dem Cmdlet [New-AzureRmBatchAccount](/powershell/module/azurerm.batch/New-AzureRmBatchAccount?view=azurermps-4.3.1), oder erstellen Sie das Azure Batch-Konto über das [Azure-Portal](../batch/batch-account-create-portal.md). Im Blog [Using Azure PowerShell to Manage Azure Batch Account](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) (Verwenden von Azure PowerShell zum Verwalten eines Azure Batch-Kontos) ist die Verwendung dieses Cmdlets im Detail beschrieben.
* [New-AzureBatchPool](/powershell/module/azurerm.batch/New-AzureBatchPool?view=azurermps-4.3.1) , um einen Azure Batch-Pool zu erstellen.

## <a name="azure-batch-linked-service"></a>Verknüpfter Azure Batch-Dienst 
Der folgende JSON-Code definiert einen verknüpften Azure Batch-Beispieldienst. Einzelheiten finden Sie unter [Von Azure Data Factory unterstützte Compute-Umgebungen](compute-linked-services.md).

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "batchaccount",
            "accessKey": {
                "type": "SecureString",
                "value": "access key"
            },
            "batchUri": "https://batchaccount.region.batch.azure.com",
            "poolName": "poolname",
            "linkedServiceName": {
                "referenceName": "StorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

 Weitere Informationen zu diesem verknüpften Azure Batch-Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md). 

## <a name="custom-activity"></a>Benutzerdefinierte Aktivität

Der folgende JSON-Ausschnitt definiert eine Pipeline mit einer einfachen benutzerdefinierten Aktivität. Die Aktivitätsdefinition enthält einen Verweis auf den verknüpften Azure Batch-Dienst. 

```json
{
    "name": "MyCustomActivityPipeline",
    "properties": {
      "description": "Custom activity sample",
      "activities": [{
        "type": "Custom",
        "name": "MyCustomActivity",
        "linkedServiceName": {
          "referenceName": "AzureBatchLinkedService",
          "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "command": "helloworld.exe",
          "folderPath": "customactv2/helloworld",
          "resourceLinkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
          }
        }
      }]
    }
  }
```

In diesem Beispiel ist „helloworld. exe“ eine benutzerdefinierte Anwendung, die im Ordner „customactv2/helloworld“ des Azure Storage-Kontos gespeichert ist, das im „resourceLinkedService“ verwendet wird. Die benutzerdefinierte Aktivität übermittelt diese benutzerdefinierte Anwendung an Azure Batch. Sie können den Befehl in jede beliebige Anwendung ändern, die im Zielbetriebssystem auf den Knoten im Azure Batch-Pool ausgeführt werden kann. 

Die folgende Tabelle beschreibt die Namen und Eigenschaften, die für diese Aktivität spezifisch sind. 

| Eigenschaft              | Beschreibung                              | Erforderlich |
| :-------------------- | :--------------------------------------- | :------- |
| Name                  | Name der Aktivität in der Pipeline     | Ja      |
| Beschreibung           | Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird.  | Nein       |
| Typ                  | Für die benutzerdefinierte Aktivität ist der Aktivitätstyp **Custom**. | Ja      |
| linkedServiceName     | Mit Azure Batch verknüpfter Dienst. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md).  | Ja      |
| command               | Befehl der benutzerdefinierten Anwendung, der ausgeführt werden soll. Wenn die Anwendung bereits auf dem Knoten des Azure Batch-Pools verfügbar ist, können „resourceLinkedService“ und „folderPath“ übersprungen werden. Sie können beispielsweise den Befehl `cmd /c dir` angeben, was vom Knoten des Azure Batch-Pools nativ unterstützt wird. | Ja      |
| resourceLinkedService | Mit dem Speicherkonto verknüpfter Azure Storage-Dienst, in dem die benutzerdefinierte Anwendung gespeichert wird. | Nein       |
| folderPath            | Pfad zum Ordner der benutzerdefinierten Anwendung und allen ihren abhängigen Elementen | Nein       |
| referenceObjects      | Array vorhandener verknüpfter Dienste und Datasets. Die referenzierten verknüpften Dienste und Datasets werden im JSON-Format an die benutzerdefinierte Anwendung übergeben, sodass Ihr benutzerdefinierter Code auf Data Factory-Ressourcen verweisen kann. | Nein       |
| extendedProperties    | Benutzerdefinierte Eigenschaften, die im JSON-Format an die benutzerdefinierte Anwendung übergeben werden können, sodass Ihr benutzerdefinierter Code auf zusätzliche Eigenschaften verweisen kann. | Nein       |

## <a name="executing-commands"></a>Ausführen von Befehlen

Mithilfe einer benutzerdefinierten Aktivität können Sie direkt einen Befehl ausführen. Im folgenden Beispiel wird ein „Echo Hello World“-Befehl auf den Azure Batch-Poolzielknoten ausgeführt und in „stdout“ ausgegeben. 

  ```json
  {
    "name": "MyCustomActivity",
    "properties": {
      "description": "Custom activity sample",
      "activities": [{
        "type": "Custom",
        "name": "MyCustomActivity",
        "linkedServiceName": {
          "referenceName": "AzureBatchLinkedService",
          "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "command": "cmd /c echo hello world"
        }
      }]
    }
  } 
  ```

## <a name="passing-objects-and-properties"></a>Übergeben von Objekten und Eigenschaften

Dieses Beispiel zeigt, wie Sie „referenceObjects“ und „extendedProperties“ verwenden können, um Data Factory-Objekte und benutzerdefinierte Eigenschaften an Ihre benutzerdefinierte Anwendung zu übergeben. 


```json
{
  "name": "MyCustomActivityPipeline",
  "properties": {
    "description": "Custom activity sample",
    "activities": [{
      "type": "Custom",
      "name": "MyCustomActivity",
      "linkedServiceName": {
        "referenceName": "AzureBatchLinkedService",
        "type": "LinkedServiceReference"
      },
      "typeProperties": {
        "command": "SampleApp.exe",
        "folderPath": "customactv2/SampleApp",
        "resourceLinkedService": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        },
        "referenceObjects": {
          "linkedServices": [{
            "referenceName": "AzureBatchLinkedService",
            "type": "LinkedServiceReference"
          }]
        },
        "extendedProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "aSampleSecureString"
            },
            "PropertyBagPropertyName1": "PropertyBagValue1",
            "propertyBagPropertyName2": "PropertyBagValue2",
            "dateTime1": "2015-04-12T12:13:14Z"              
        }
      }
    }]
  }
}
```

Wenn die Aktivität ausgeführt wird, werden „referenceObjects“ und „extendedProperties“ in folgenden Dateien gespeichert, die im selben Ausführungsordner von „SampleApp.exe“ bereitgestellt werden: 

- activity.json

  Speichert „extendedProperties“ und Eigenschaften der benutzerdefinierten Aktivität. 

- linkedServices.json

  Speichert ein Array verknüpfter Dienste, die in der „referenceObjects“-Eigenschaft definiert sind. 

- datasets.json

  Speichert ein Array von Datasets, die in der „referenceObjects“-Eigenschaft definiert sind. 

Der folgende Beispielcode veranschaulicht, wie „SampleApp.exe“ auf die erforderlichen Informationen in JSON-Dateien zugreifen kann: 

```C#
using Newtonsoft.Json;
using System;
using System.IO;

namespace SampleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            //From Extend Properties
            dynamic activity = JsonConvert.DeserializeObject(File.ReadAllText("activity.json"));
            Console.WriteLine(activity.typeProperties.extendedProperties.connectionString.value);

            // From LinkedServices
            dynamic linkedServices = JsonConvert.DeserializeObject(File.ReadAllText("linkedServices.json"));
            Console.WriteLine(linkedServices[0].properties.typeProperties.connectionString.value);
        }
    }
}
```

## <a name="retrieve-execution-outputs"></a>Abrufen von Ausführungsausgaben

  Sie können mit dem folgenden PowerShell-Befehl eine Pipelineausführung starten: 

  ```.powershell
  $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName
  ```
  Wenn die Pipeline ausgeführt wird, können Sie mit den folgenden Befehlen die Ausgabe der Ausführung überprüfen: 

  ```.powershell
  while ($True) {
      $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

      if(!$result) {
          Write-Host "Waiting for pipeline to start..." -foregroundcolor "Yellow"
      }
      elseif (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
          Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
      }
      else {
          Write-Host "Pipeline '"$pipelineName"' run finished. Result:" -foregroundcolor "Yellow"
          $result
          break
      }
      ($result | Format-List | Out-String)
      Start-Sleep -Seconds 15
  }

  Write-Host "Activity `Output` section:" -foregroundcolor "Yellow"
  $result.Output -join "`r`n"

  Write-Host "Activity `Error` section:" -foregroundcolor "Yellow"
  $result.Error -join "`r`n"
  ```

  Die Angaben für **stdout** und **stderr** Ihrer benutzerdefinierten Anwendung werden im Container **adfjobs** im verknüpften Azure Storage-Dienst gespeichert, den Sie beim Erstellen des verknüpften Azure Batch-Diensts mit einer GUID der Aufgabe definiert haben. Den detaillierten Pfad finden Sie in der Ausgabe der Aktivitätsausführung, wie im folgenden Ausschnitt gezeigt: 

  ```shell
  Pipeline ' MyCustomActivity' run finished. Result:

  ResourceGroupName : resourcegroupname
  DataFactoryName   : datafactoryname
  ActivityName      : MyCustomActivity
  PipelineRunId     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
  PipelineName      : MyCustomActivity
  Input             : {command}
  Output            : {exitcode, outputs, effectiveIntegrationRuntime}
  LinkedServiceName : 
  ActivityRunStart  : 10/5/2017 3:33:06 PM
  ActivityRunEnd    : 10/5/2017 3:33:28 PM
  DurationInMs      : 21203
  Status            : Succeeded
  Error             : {errorCode, message, failureType, target}

  Activity Output section:
  "exitcode": 0
  "outputs": [
    "https://shengcstorbatch.blob.core.windows.net/adfjobs/<GUID>/output/stdout.txt",
    "https://shengcstorbatch.blob.core.windows.net/adfjobs/<GUID>/output/stderr.txt"
  ]
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
  Activity Error section:
  "errorCode": ""
  "message": ""
  "failureType": ""
  "target": "MyCustomActivity"
  ```
Wenn Sie den Inhalt von „stdout.txt“ in nachgelagerten Aktivitäten nutzen möchten, können Sie den Pfad zur Datei „stdout.txt“ im Ausdruck „@activity('MyCustomActivity').output.outputs[0]“ abrufen. 

  > [!IMPORTANT]
  > - Die Dateien „activity.json“, „linkedServices.json“ und „datasets.json“ werden im Ordner „runtime“ der Aufgabe „Bath“ gespeichert. In diesem Beispiel werden die Dateien „activity.json“, „linkedServices.json“ und „datasets.json“ im Pfad „https://adfv2storage.blob.core.windows.net/adfjobs/<GUID>/runtime/“ gespeichert. Bei Bedarf müssen diese separat bereinigt werden. 
  > - Da verknüpfte Dienste die selbstgehostete Integration Runtime verwenden, werden die sensiblen Informationen wie Schlüssel oder Kennwörter von der selbstgehosteten Integration Runtime verschlüsselt. Dies soll gewährleisten, dass die Anmeldeinformationen in der vom Kunden definierten privaten Netzwerkumgebung verbleiben. Einige sensible Felder können fehlen, wenn auf sie von Ihrem eigenen Anwendungscode auf diese Weise verwiesen wird. Verwenden Sie bei Bedarf „SecureString“ in „extendedProperties“ anstelle des Verweises auf den verknüpften Dienst. 

## <a name="difference-between-custom-activity-in-azure-data-factory-v2-and-custom-dotnet-activity-in-azure-data-factory-v1"></a>Unterschied zwischen einer benutzerdefinierten Aktivität in Azure Data Factory V2 und einer (benutzerdefinierten) DotNet-Aktivität in Azure Data Factory V1 

  In Azure Data Factory V1 implementieren Sie den Code einer (benutzerdefinierten) DotNet-Aktivität, indem Sie ein .NET-Klassenbibliotheksprojekt mit einer Klasse erstellen, die die Execute-Methode der IDotNetActivity-Schnittstelle implementiert. Die verknüpften Dienste, Datasets und erweiterten Eigenschaften in der JSON-Nutzlast der (benutzerdefinierten) DotNet-Aktivität werden als stark typisierte Objekte an die Execution-Methode übergeben. Weitere Informationen finden Sie unter [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](v1/data-factory-use-custom-activities.md). Aus diesem Grund muss der benutzerdefinierte Code in .NET Framework 4.5.2 geschrieben und auf Windows-basierten Azure Batch-Poolknoten ausgeführt werden. 

  In einer benutzerdefinierten Aktivität von Azure Data Factory V2 muss keine .NET-Schnittstelle implementiert werden. Sie können jetzt direkt Befehle, Skripts und Ihren eigenen als ausführbare Datei erfüllten benutzerdefinierten Code ausführen. Hierfür müssen Sie lediglich die Command-Eigenschaft zusammen mit der Eigenschaft „folderPath“ angeben. Die benutzerdefinierte Aktivität lädt die ausführbare Datei und die Abhängigkeiten in den Ordnerpfad hoch und führt den Befehl für Sie aus. 

  Ihre ausführbare Datei kann auf die verknüpften Dienste, Datasets (definiert in referenceObjects) und erweiterten Eigenschaften in der JSON-Nutzlast der benutzerdefinierten Aktivität als JSON-Dateien zugreifen. Mit dem JSON-Serialisierungsmodul können Sie auf die gewünschten Eigenschaften zugreifen, wie im vorhergehenden Codebeispiel von „SampleApp.exe“ gezeigt wird. 

  Mit den Änderungen, die in der benutzerdefinierten Aktivität von Azure Data Factory V2 eingeführt wurden, können Sie nun mühelos eine eigene benutzerdefinierte Codelogik in Ihrer bevorzugten Programmiersprache schreiben und diese unter den von Azure Batch unterstützten Betriebssystemen Windows und Linux ausführen. 

  Wenn Sie bereits über einen .NET-Code verfügen, der für die (benutzerdefinierte) DotNet-Aktivität von V1 geschrieben wurde, müssen Sie den Code gemäß den folgenden allgemeinen Richtlinien für diese ändern, damit sie mit einer benutzerdefinierten Aktivität von V2 funktionieren:  

  > - Ändern Sie das Projekt von einer .NET-Klassenbibliothek in eine Konsolen-App. 
  > - Starten Sie Ihre Anwendung mit der Main-Methode. Die Execute-Methode der IDotNetActivity-Schnittstelle ist nicht mehr erforderlich. 
  > - Lesen und analysieren Sie die verknüpften Dienste, Datasets und Aktivitäten mit dem JSON-Serialisierungsmodul statt als stark typisierte Objekten, und übergeben Sie die Werte der erforderlichen Eigenschaften an Ihre eigene Hauptcodelogik. Ein Beispiel finden Sie im vorangehenden Code von „SampleApp.exe“. 
  > - Das Protokollierungsobjekt wird nicht mehr unterstützt. Ausführbare Ausgaben können über die Konsole ausgegeben und werden in „stdout.txt“ gespeichert. 
  > - Das NuGet-Paket „Microsoft.Azure.Management.DataFactories“ ist nicht mehr erforderlich. 
  > - Kompilieren Sie Ihren Code, laden Sie ausführbare Dateien und Abhängigkeiten in Azure Storage hoch, und definieren Sie den Pfad in der folderPath-Eigenschaft. 

## <a name="auto-scaling-of-azure-batch"></a>Automatische Skalierung von Azure Batch
Sie können einen Azure Batch-Pool auch mit dem Feature **Automatisch skalieren** erstellen. Sie können z.B. einen Azure Batch-Pool ohne dedizierte VM erstellen und dabei eine Formel für die automatische Skalierung angeben, die von der Anzahl der ausstehenden Aufgaben abhängig ist. 

Mit der hier angeführten Beispielformel wird folgendes Verhalten erzielt: Anfänglich umfasst der Pool nach seiner Erstellung einen virtuellen Computer. Die Metrik „$PendingTasks“ legt die Anzahl der Aufgaben im ausgeführten und im aktiven (in der Warteschlange) Zustand fest.  Die Formel sucht nach der durchschnittlichen Anzahl ausstehender Aufgaben in den letzten 180 Sekunden und legt TargetDedicated auf den entsprechenden Wert fest. Dadurch wird sichergestellt, dass TargetDedicated nie die Anzahl von 25 virtuellen Computern überschreitet. Wenn also neue Aufgaben gesendet werden, wächst der Pool automatisch an. Beim Abschluss von Aufgaben werden virtuelle Computer nacheinander frei, und durch die automatische Skalierung werden diese virtuellen Computer reduziert. startingNumberOfVMs und maxNumberofVMs können entsprechend den jeweiligen Anforderungen angepasst werden.

Formel für die automatische Skalierung:

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Weitere Informationen hierzu finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](../batch/batch-automatic-scaling.md) .

Wenn der Pool die Standardeinstellung für [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx)verwendet, kann es 15 bis 30 Minuten dauern, bis der Batch-Dienst den virtuellen Computer vorbereitet hat und die benutzerdefinierte Aktivität ausgeführt wird.  Wenn der Pool eine andere Einstellung für „autoScaleEvaluationInterval“ nutzt, könnte der Batch-Dienst „autoScaleEvaluationInterval“ + 10 Minuten verwenden.


## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden: 

* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Machine Learning-Batchausführungsaktivität](transform-data-using-machine-learning.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)

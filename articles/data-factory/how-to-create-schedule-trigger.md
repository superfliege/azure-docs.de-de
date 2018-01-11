---
title: Erstellen von Triggern in Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie in Azure Data Factory ein Trigger erstellt wird, der eine Pipeline gemäß einem Zeitplan ausführt."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: shlo
ms.openlocfilehash: eed286c01604ab0e9ac2113d56cbce268503668d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Erstellen eines Triggers in Azure Data Factory, der eine Pipeline gemäß einem Zeitplan ausführt
Dieser Artikel enthält die Schritte zum Erstellen, Starten und Überwachen eines Triggers. Informationen zum Konzept von Triggern finden Sie unter [Pipelineausführung und Trigger](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell
In diesem Abschnitt erfahren Sie, wie Sie mit Azure PowerShell einen Trigger erstellen, starten und überwachen. Wenn Sie sehen möchten, wie dieses Beispiel funktioniert, durchlaufen zuerst den [Schnellstart: Erstellen einer Data Factory mit Azure PowerShell](quickstart-create-data-factory-powershell.md). Fügen Sie anschließend den folgenden Code zur „main“-Methode hinzu, der einen Zeitplantrigger erstellt und startet, der alle 15 Minuten ausgeführt wird. Der Trigger ist einer Pipeline zugeordnet (**Adfv2QuickStartPipeline**), die Sie als Teil des Schnellstarts erstellen.

1. Erstellen Sie im Ordner „C:\ADFv2QuickStartPSH“ eine JSON-Datei mit dem Namen „MyTrigger.json“ und dem folgenden Inhalt: 

    > [!IMPORTANT]
    > Legen Sie **startTime** auf die aktuelle UTC-Zeit und **endTime** auf eine Stunde nach der aktuellen UTC-Zeit fest, bevor Sie die JSON-Datei speichern.

    ```json   
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00",
                    "endTime": "2017-12-08T01:00:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```
    
    Im JSON-Codeausschnitt: 
    - **type** des Triggers wird auf **ScheduleTrigger** festgelegt. 
    - **frequency** wird auf **Minute** und **interval** auf **15** festgelegt. Deshalb führt der Trigger die Pipeline alle 15 Minuten zwischen dem Start- und Endzeitpunkt aus. 
    - **endTime** ist eine Stunde nach **startTime**, weshalb der Trigger die Pipeline 15 Minuten, 30 Minuten und 45 Minuten nach „startTime“ ausführt. Vergessen Sie nicht, „startTime“ in die aktuelle UTC-Zeit und „endTime“ in eine Stunde nach „startTime“ zu ändern.  
    - Der Trigger ist der Pipeline **Adfv2QuickStartPipeline** zugeordnet. Um einen Trigger mehreren Pipelines zuzuordnen, fügen Sie weitere **pipelineReference**-Abschnitte hinzu. 
    - Die Pipeline im Schnellstart akzeptiert zwei **Parameter**. Aus diesem Grund übergeben Sie Werte für diese Parameter aus dem Trigger. 
2. Erstellen Sie mit dem Cmdlet **Start-AzureRmDataFactoryV2Trigger** einen Trigger.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
3. Bestätigen Sie, dass der Status des Triggers **Stopped** ist, indem Sie das Cmdlet **Get-AzureRmDataFactoryV2Trigger** verwenden. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
4. Starten Sie den Trigger mit dem Cmdlet **Start-AzureRmDataFactoryV2Trigger**: 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
5. Bestätigen Sie, dass der Status des Triggers **Started** ist, indem Sie das Cmdlet **Get-AzureRmDataFactoryV2Trigger** verwenden.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
6.  Rufen Sie Triggerausführungen mithilfe von PowerShell mit dem Cmdlet **Get-AzureRmDataFactoryV2TriggerRun** ab. Um die Informationen über Triggerausführungen zu erhalten, führen Sie den folgenden Befehl regelmäßig aus: Aktualisieren Sie die Werte **TriggerRunStartedAfter** und **TriggerRunStartedBefore**, damit sie mit den Werten in der Triggerdefinition übereinstimmen. 

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```

    Informationen zum Überwachen von Trigger- bzw. Pipelineausführungen im Azure-Portal finden Sie unter [Überwachen der Pipelineausführungen](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="use-net-sdk"></a>Verwenden von .NET SDK
In diesem Abschnitt erfahren Sie, wie Sie mit dem .NET SDK einen Trigger erstellen, starten und überwachen. Wenn Sie sehen möchten, wie dieser Code funktioniert, durchlaufen zuerst den [Schnellstart: Erstellen einer Data Factory mit .NET SDK](quickstart-create-data-factory-dot-net.md). Fügen Sie anschließend den folgenden Code zur „main“-Methode hinzu, der einen Zeitplantrigger erstellt und startet, der alle 15 Minuten ausgeführt wird. Der Trigger ist einer Pipeline zugeordnet (**Adfv2QuickStartPipeline**), die Sie als Teil des Schnellstarts erstellen. 

```csharp
            //create the trigger
            Console.WriteLine("Creating the trigger");

            // set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // set the start time to current UTC time and the end time to be one hour after.
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // now, create the trigger by invoking CreateOrUpdate method. 
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            //start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);

```

Fügen Sie zum Überwachen einer Triggerausführung den folgenden Code vor der letzten `Console.WriteLine`-Anweisung hinzu: 

```csharp
            // Check the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Informationen zum Überwachen von Trigger- bzw. Pipelineausführungen im Azure-Portal finden Sie unter [Überwachen der Pipelineausführungen](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="use-python-sdk"></a>Verwenden des Python SDK
In diesem Abschnitt erfahren Sie, wie Sie mit dem Python SDK einen Trigger erstellen, starten und überwachen. Wenn Sie sehen möchten, wie dieser Code funktioniert, durchlaufen zuerst den [Schnellstart: Erstellen einer Data Factory mit dem Python SDK](quickstart-create-data-factory-python.md). Fügen Sie dann im Python-Skript den folgenden Codeblock nach dem Codeblock „monitor the pipeline run“ hinzu. Dieser Code erzeugt einen Zeitplantrigger, der alle 15 Minuten zwischen den angegebenen Start- und Endzeiten ausgeführt wird. Ändern Sie „start_time“ in die aktuelle UTC-Zeit und „end_time“ in eine Stunde nach der aktuellen UTC-Zeit. 

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00', end_time='2017-12-12T05:00:00', time_zone='UTC') 
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```
Informationen zum Überwachen von Trigger- bzw. Pipelineausführungen im Azure-Portal finden Sie unter [Überwachen der Pipelineausführungen](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="use-resource-manager-template"></a>Verwenden von Resource Manager-Vorlagen
Sie können mit einer Azure Resource Manager-Vorlage einen Trigger erstellen. Detaillierte Anweisungen finden Sie unter [Erstellen einer Azure Data Factory mit einer Resource Manager-Vorlage](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Übergeben der Startzeit des Triggers an eine Pipeline
In Version 1 unterstützte Azure Data Factory das Lesen oder Schreiben von partitionierten Daten mithilfe der Systemvariablen SliceStart/SliceEnd/WindowStart/WindowEnd. In Version 2 können Sie dieses Verhalten mithilfe eines Pipelineparameters und der Anfangszeit/geplanten Zeit eines Triggers als Wert des Parameters erreichen. Im folgenden Beispiel wird die geplante Zeit des Triggers als Wert an den Pipelineparameter „scheduledRunTime“ übergeben. 

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    
Weitere Informationen finden Sie unter [Lesen oder Schreiben partitionierter Daten](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Nächste Schritte
Detaillierte Informationen zu Triggern finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md#triggers).
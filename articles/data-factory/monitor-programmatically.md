---
title: "Programmgesteuertes Überwachen einer Azure Data Factory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe verschiedener Software Development Kits (SDKs) eine Pipeline in einer Data Factory überwachen können."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2017
ms.author: spelluru
ms.openlocfilehash: 376bc64bee85fbc073b6ea4a39ecd013c23e791f
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Programmgesteuertes Überwachen einer Azure Data Factory
In diesem Artikel wird beschrieben, wie Sie mithilfe verschiedener Software Development Kits (SDKs) eine Pipeline in einer Data Factory überwachen können. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts in der allgemein verfügbaren Version (GA) verwenden, lesen Sie den Artikel [Überwachen und Verwalten von Azure Data Factory-Pipelines mit dem Azure-Portal und PowerShell](v1/data-factory-monitor-manage-pipelines.md).

## <a name="net"></a>.NET
Eine vollständige exemplarische Vorgehensweise zum Erstellen und Überwachen einer Pipeline mit dem .NET SDK finden Sie unter [Erstellen einer Data Factory und Pipeline mit dem .NET SDK](quickstart-create-data-factory-dot-net.md).

1. Fügen Sie den folgenden Code hinzu, um kontinuierlich den Status der Pipelineausführung zu überwachen, bis die Pipeline das Kopieren der Daten beendet hat.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Fügen Sie den folgenden Code hinzu, der Ausführungsdetails zur Kopieraktivität abruft, z.B. die Größe der gelesenen/geschriebenen Daten.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

Eine vollständige Dokumentation zum .NET SDK finden Sie in der [.NET SDK-Referenz für Data Factory](/dotnet/api/microsoft.azure.management.datafactory?view=azure-dotnet).

## <a name="python"></a>Python
Eine vollständige exemplarische Vorgehensweise zum Erstellen und Überwachen einer Pipeline mit dem Python-SDK finden Sie unter [Erstellen einer Data Factory und Pipeline mithilfe von Python](quickstart-create-data-factory-python.md).

Fügen Sie den folgenden Code hinzu, um die Pipelineausführung zu überwachen:

```python
#Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Eine vollständige Dokumentation zum Python-SDK finden Sie in der [Python-SDK-Referenz für Data Factory](/python/api/overview/azure/datafactory?view=azure-python).

## <a name="rest-api"></a>REST-API
Eine vollständige exemplarische Vorgehensweise zum Erstellen und Überwachen einer Pipeline mit der REST-API finden Sie unter [Erstellen einer Azure Data Factory und einer Pipeline mithilfe der REST-API](quickstart-create-data-factory-rest-api.md).
 
1. Führen Sie das folgende Skript aus, um den Status der Pipelineausführung kontinuierlich zu überwachen, bis das Kopieren der Daten beendet ist.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ($response.Status -eq "InProgress") {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```
2. Führen Sie das folgende Skript aus, um Ausführungsdetails zur Kopieraktivität abzurufen, z.B. die Größe der gelesenen/geschriebenen Daten.

    ```PowerShell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

In der [REST-API-Referenz für Data Factory](/rest/api/datafactory/) finden Sie eine vollständige Dokumentation zur REST-API.

## <a name="powershell"></a>PowerShell
Eine vollständige exemplarische Vorgehensweise zum Erstellen und Überwachen einer Pipeline mit PowerShell finden Sie unter [Erstellen einer Data Factory und Pipeline mithilfe von PowerShell](quickstart-create-data-factory-powershell.md).

1. Führen Sie das folgende Skript aus, um den Status der Pipelineausführung kontinuierlich zu überwachen, bis das Kopieren der Daten beendet ist.

    ```powershell
    while ($True) {
        $run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
        }

        Start-Sleep -Seconds 30
    }
    ```
2. Führen Sie das folgende Skript aus, um Ausführungsdetails zur Kopieraktivität abzurufen, z.B. die Größe der gelesenen/geschriebenen Daten.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

Die vollständige Dokumentation zu PowerShell-Cmdlets finden Sie in der [PowerShell-Cmdlet-Referenz für Data Factory](/powershell/module/azurerm.datafactoryv2/?view=azurermps-4.4.1).

## <a name="next-steps"></a>Nächste Schritte
Im Artikel [Überwachen von Pipelines mit Azure Monitor](monitor-using-azure-monitor.md) finden Sie Informationen zum Überwachen von Data Factory-Pipelines mit Azure Monitor. 


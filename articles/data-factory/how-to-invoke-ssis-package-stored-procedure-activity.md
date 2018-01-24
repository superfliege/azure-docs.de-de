---
title: "Aufrufen des SSIS-Pakets mithilfe von Azure Data Factory – Aktivität der gespeicherten Prozedur | Microsoft-Dokumentation"
description: "In diesem Artikel wird das Aufrufen eines SSIS-Pakets (SQL Server Integration Services) aus einer Azure Data Factory-Pipeline mithilfe einer Aktivität einer gespeicherten Prozedur beschrieben."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: 713e9ad7a76c15cbde912954e00991a80b995683
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Aufrufen eines SSIS-Pakets mithilfe einer Aktivität einer gespeicherten Prozedur in Azure Data Factory
In diesem Artikel wird das Aufrufen eines SSIS-Pakets aus einer Azure Data Factory-Pipeline mithilfe einer Aktivität einer gespeicherten Prozedur beschrieben. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, lesen Sie [Invoke SSIS packages using stored procedure activity in version 1](v1/how-to-invoke-ssis-package-stored-procedure-activity.md) (Aufrufen von SSIS-Paketen mithilfe einer Aktivität einer gespeicherten Prozedur in Version 1).

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-sql-database"></a>Azure SQL-Datenbank 
Die exemplarische Vorgehensweise in diesem Artikel verwendet eine Azure SQL-Datenbank, in der der SSIS-Katalog gehostet ist. Alternativ können Sie eine verwaltete Instanz von Azure SQL (private Vorschau) verwenden.

## <a name="create-an-azure-ssis-integration-runtime"></a>Erstellen einer Azure SSIS Integration Runtime
Erstellen Sie eine Azure-SSIS Integration Runtime, wenn Sie noch nicht über eine verfügen, indem Sie die Schritt-für-Schritt-Anweisung im [Tutorial: Bereitstellen von SSIS-Paketen](tutorial-deploy-ssis-packages-azure.md) befolgen.

### <a name="azure-powershell"></a>Azure PowerShell
Installieren Sie die aktuellen Azure PowerShell-Module, indem Sie die Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps) befolgen. 

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
Sie können wahlweise die gleiche Data Factory verwenden, die die Azure-SSIS IR aufweist, oder eine separate Data Factory erstellen. Das folgende Verfahren beschreibt die Schritte zum Erstellen einer Data Factory. In dieser Data Factory erstellen Sie eine Pipeline mit einer Aktivität einer gespeicherten Prozedur. Die Aktivität der gespeicherten Prozedur führt eine gespeicherte Prozedur in der SSISDB-Datenbank aus, um Ihr SSIS-Paket auszuführen. 

1. Definieren Sie eine Variable für den Ressourcengruppennamen zur späteren Verwendung in PowerShell-Befehlen. Kopieren Sie den folgenden Befehlstext nach PowerShell, geben Sie einen Namen für die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in doppelten Anführungszeichen an, und führen Sie dann den Befehl aus. Beispiel: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$ResourceGroupName` einen anderen Wert zu, und führen Sie den Befehl erneut aus.
2. Führen Sie den folgenden Befehl aus, um die Azure-Ressourcengruppe zu erstellen: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$ResourceGroupName` einen anderen Wert zu, und führen Sie den Befehl erneut aus. 
3. Definieren Sie eine Variable für den Namen der Data Factory. 

    > [!IMPORTANT]
    >  Aktualisieren Sie den Data Factory-Namen, damit er global eindeutig ist. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Führen Sie zum Erstellen der Data Factory das folgende Cmdlet vom Typ **Set-AzureRmDataFactoryV2** aus. Verwenden Sie dabei den Standort und die Eigenschaft „ResourceGroupName“ aus der Variablen „$ResGrp“: 
    
    ```powershell       
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName -Location $ResGrp.Location -Name $dataFactoryName 
    ```

Beachten Sie folgende Punkte:

* Der Name der Azure Data Factory muss global eindeutig sein. Wenn die folgende Fehlermeldung angezeigt wird, ändern Sie den Namen, und wiederholen Sie den Vorgang.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Damit Sie Data Factory-Instanzen erstellen können, muss das Benutzerkonto, mit dem Sie sich bei Azure anmelden, ein Mitglied der Rolle **Mitwirkender** oder **Besitzer** oder ein **Administrator** des Azure-Abonnements sein.
* Derzeit können Sie mit Data Factory Version 2 nur in den Regionen „USA, Osten“, „USA, Osten 2“ und „Europa, Westen“ Data Factorys erstellen. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.

### <a name="create-an-azure-sql-database-linked-service"></a>Erstellen eines verknüpften Azure SQL-Datenbankdiensts
In diesem Schritt erstellen Sie einen verknüpften Dienst, um Ihre Azure SQL-Datenbank, in der der SSIS-Katalog gehostet ist, mit der Data Factory zu verbinden. Die Data Factory verwendet Informationen in diesem verknüpften Dienst zum Herstellen der Verbindung mit der SSISDB-Datenbank und führt eine gespeicherte Prozedur zum Ausführen eines SSIS-Pakets aus. 

1. Erstellen Sie im Ordner **C:\ADF\RUNSSISPackage** eine JSON-Datei mit dem Namen **AzureSQLDatabaseLinkedService.json** und folgendem Inhalt: 

    > [!IMPORTANT]
    > Ersetzen Sie &lt;servername&gt;, &lt;username&gt; und &lt;password&gt; durch Werte Ihrer Azure SQL-Datenbank. Speichern Sie anschließend die Datei.

    ```json
    {
        "name": "AzureSqlDbLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            }
        }
    }
    ```

2. Wechseln Sie in **Azure PowerShell** zum Ordner **C:\ADF\RunSSISPackage**.

3. Führen Sie nun das Cmdlet **Set-AzureRmDataFactoryV2LinkedService** zum Erstellen des verknüpften Diensts **AzureSqlDatabaseLinkedService** aus. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureSqlDatabaseLinkedService" -File ".\AzureSqlDatabaseLinkedService.json"
    ```

## <a name="create-a-pipeline-with-stored-procedure-activity"></a>Erstellen einer Pipeline mit einer Aktivität einer gespeicherten Prozedur 
In diesem Schritt erstellen Sie eine Pipeline mit einer Aktivität einer gespeicherten Prozedur. Die Aktivität ruft die gespeicherte Prozedur „sp_executesql“ auf, um Ihr SSIS-Paket auszuführen. 

1. Erstellen Sie im Ordner **C:\ADF\RunSSISPackage** eine JSON-Datei mit dem Namen **RunSSISPackagePipeline.json** und folgendem Inhalt:

    > [!IMPORTANT]
    > Ersetzen Sie &lt;FOLDER NAME&gt;, &lt;PROJECT NAME&gt;, &lt;PACKAGE NAME&gt; durch die Namen von Ordner, Projekt und Paket im SSIS-Katalog, und speichern Sie dann die Datei. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [
                {
                    "name": "My SProc Activity",
                    "description":"Runs an SSIS package",
                    "type": "SqlServerStoredProcedure",
                    "linkedServiceName": {
                        "referenceName": "AzureSqlDbLinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "typeProperties": {
                        "storedProcedureName": "sp_executesql",
                        "storedProcedureParameters": {
                            "stmt": {
                                "value": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER NAME>', @project_name=N'<PROJECT NAME>', @package_name=N'<PACKAGE NAME>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                            }
                        }
                    }
                }
            ]
        }
    }
    ```

2. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Pipeline** aus, um die Pipeline **RunSSISPackagePipeline** zu erstellen.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Hier ist die Beispielausgabe:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

## <a name="create-a-pipeline-run"></a>Erstellen einer Pipelineausführung
Verwenden Sie das Cmdlet **Invoke-AzureRmDataFactoryV2Pipeline**, um die Pipeline auszuführen. Das Cmdlet gibt die ID der Pipelineausführung für die zukünftige Überwachung zurück.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name
```

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

Führen Sie das folgende PowerShell-Skript aus, um den Status der Pipelineausführung kontinuierlich zu überwachen, bis das Kopieren der Daten beendet ist. Kopieren Sie das folgende Skript, fügen Sie es in das PowerShell-Fenster ein, und drücken Sie die EINGABETASTE. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

## <a name="create-a-trigger"></a>Erstellen eines Triggers
Im vorherigen Schritt wurde die Pipeline auf Anforderung aufgerufen. Alternativ können Sie einen Zeitplantrigger erstellen, um die Pipeline nach Zeitplan (stündlich, täglich usw.) auszuführen.

1. Erstellen Sie im Ordner **C:\ADF\RunSSISPackage** eine JSON-Datei mit dem Namen **MyTrigger.json** und dem folgenden Inhalt: 

    ```json
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Hour",
                    "interval": 1,
                    "startTime": "2017-12-07T00:00:00-08:00",
                    "endTime": "2017-12-08T00:00:00-08:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "RunSSISPackagePipeline"
                    },
                    "parameters": {}
                }
            ]
        }
    }    
    ```
2. Wechseln Sie in **Azure PowerShell** zum Ordner **C:\ADF\RunSSISPackage**.
3. Führen Sie das **Set-AzureRmDataFactoryV2Trigger**-Cmdlet aus, um den Trigger zu erstellen. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Standardmäßig befindet sich der Trigger im beendeten Zustand. Starten Sie den Trigger, indem Sie das Cmdlet **Start-AzureRmDataFactoryV2Trigger** ausführen. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" 
    ```
5. Bestätigen Sie, dass der Trigger sich im gestarteten Zustand befindet, indem Sie das Cmdlet **Get-AzureRmDataFactoryV2Trigger** ausführen. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"     
    ```    
6. Führen Sie nach der nächsten vollen Stunde den folgenden Befehl aus. Wenn die aktuelle Uhrzeit z.B. 15:25 UTC ist, führen Sie den Befehl um 16:00 UTC aus. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-06" -TriggerRunStartedBefore "2017-12-09"
    ```

    Sie können in Ihrem Azure SQL Server die folgende Abfrage für die SSISDB-Datenbank ausführen, um zu überprüfen, ob das Paket ausgeführt wurde. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Nächste Schritte
Sie können die Pipeline auch mithilfe des Azure-Portals überwachen. Schritt-für-Schritt-Anweisungen finden Sie unter [Überwachen der Pipeline](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

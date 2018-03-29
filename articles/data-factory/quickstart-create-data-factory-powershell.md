---
title: Kopieren von Daten in Blob Storage mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory-Instanz, um Daten von einem Speicherort in einem Azure Blob-Speicher an einen anderen Speicherort zu kopieren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: hero-article
ms.date: 01/22/2018
ms.author: jingwang
ms.openlocfilehash: a060ac1051f56b123f76606c771f0957096d2457
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="create-an-azure-data-factory-using-powershell"></a>Erstellen einer Azure Data Factory mithilfe von PowerShell 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: allgemein verfügbar](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 – Vorschauversion](quickstart-create-data-factory-powershell.md)

Diese Schnellstartanleitung beschreibt, wie Sie PowerShell verwenden, um eine Azure Data Factory zu erstellen. Die in dieser Data Factory erstellte Pipeline **kopiert** Daten aus einem Ordner in einen anderen Ordner in Azure Blob Storage. Ein Tutorial zum **Transformieren** von Daten mithilfe von Azure Data Factory finden Sie im Tutorial [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](transform-data-using-spark.md). 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
>
> Dieser Artikel enthält keine ausführliche Einführung in den Data Factory-Dienst. Eine Einführung in den Azure Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](introduction.md).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="azure-powershell"></a>Azure PowerShell
Installieren Sie die aktuellen Azure PowerShell-Module, indem Sie die Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps) befolgen.

#### <a name="log-in-to-powershell"></a>Anmelden an PowerShell

1. Starten Sie **PowerShell** auf Ihrem Computer. Lassen Sie PowerShell bis zum Ende dieser Schnellstartanleitung geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.
2. Führen Sie den folgenden Befehl aus, und geben Sie den gleichen Azure-Benutzernamen und das gleiche Kennwort ein, die Sie bei der Anmeldung beim Azure-Portal verwendet haben:
       
    ```powershell
    Login-AzureRmAccount
    ```        
3. Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen:

    ```powershell
    Get-AzureRmSubscription
    ```
4. Sollten Ihrem Konto mehrere Azure-Abonnements zugeordnet sein, führen Sie den folgenden Befehl aus, um das zu verwendende Abonnement auszuwählen. Ersetzen Sie **SubscriptionId** durch die ID Ihres Azure-Abonnements:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
1. Definieren Sie eine Variable für den Ressourcengruppennamen zur späteren Verwendung in PowerShell-Befehlen. Kopieren Sie den folgenden Befehlstext nach PowerShell, geben Sie einen Namen für die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in doppelten Anführungszeichen an, und führen Sie dann den Befehl aus. Beispiel: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFQuickStartRG";
    ```

    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$ResourceGroupName` einen anderen Wert zu, und führen Sie den Befehl erneut aus.
2. Führen Sie den folgenden Befehl aus, um die Azure-Ressourcengruppe zu erstellen: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'East US'
    ``` 
    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$ResourceGroupName` einen anderen Wert zu, und führen Sie den Befehl erneut aus. 
3. Definieren Sie eine Variable für den Namen der Data Factory. 

    > [!IMPORTANT]
    >  Aktualisieren Sie den Data Factory-Namen, damit er global eindeutig ist. Beispiel: ADFTutorialFactorySP1127. 

    ```powershell
    $dataFactoryName = "ADFQuickStartFactory";
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

## <a name="create-a-linked-service"></a>Erstellen eines verknüpften Diensts

Erstellen Sie verknüpfte Dienste in einer Data Factory, um Ihre Datenspeicher und Computedienste mit der Data Factory zu verknüpfen. In dieser Schnellstartanleitung erstellen Sie einen mit Azure Storage verknüpften Dienst, der sowohl als Quellspeicher als auch als Senkenspeicher verwendet wird. Der verknüpfte Dienste enthält die Verbindungsinformationen, die der Data Factory-Dienst zur Laufzeit zur Verbindungsherstellung verwendet.

1. Erstellen Sie eine JSON-Datei mit dem Namen **AzureStorageLinkedService.json** im Ordner **C:\ADFv2QuickStartPSH** und dem folgenden Inhalt (erstellen Sie den Ordner „ADFv2QuickStartPSH“, wenn dieser noch nicht vorhanden ist): 

    > [!IMPORTANT]
    > Ersetzen Sie &lt;accountname&gt; und &lt;accountkey&gt; durch den Namen bzw. Schlüssel Ihres Azure-Speicherkontos, bevor Sie die Datei speichern.

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>;EndpointSuffix=core.windows.net",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
    Wählen Sie bei Verwendung des Editors im Dialogfeld **Speichern unter** für **Dateityp** die Option **Alle Dateien**. Andernfalls wird der Datei ggf. die Erweiterung `.txt` hinzugefügt. Beispiel: `AzureStorageLinkedService.json.txt`. Wenn Sie die Datei vor dem Öffnen im Editor im Datei-Explorer erstellen, wird die Erweiterung `.txt` ggf. nicht angezeigt, da die Option **Erweiterungen bei bekannten Dateitypen ausblenden** standardmäßig aktiviert ist. Entfernen Sie die Erweiterung `.txt`, bevor Sie mit dem nächsten Schritt fortfahren.
2. Wechseln Sie in **PowerShell** zum Ordner **ADFv2QuickStartPSH**.

    ```powershell
    Set-Location 'C:\ADFv2QuickStartPSH'
    ```
3. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2LinkedService** aus, um den verknüpften Dienst **AzureStorageLinkedService** zu erstellen. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Hier ist die Beispielausgabe:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Erstellen eines Datasets
In diesem Schritt definieren Sie ein Dataset, das die Daten repräsentiert, die aus einer Quelle in eine Senke kopiert werden sollen. Das Dataset ist vom Typ **AzureBlob**. Es verweist auf den **mit Azure Storage verknüpften Dienst**, den Sie im vorherigen Schritt erstellt haben. Es verwendet einen Parameter zum Erstellen der **folderPath**-Eigenschaft. Für ein Eingabedataset übergibt die Kopieraktivität in der Pipeline den Eingabepfad als Wert für diesen Parameter. Analog dazu übergibt die Kopieraktivität für ein Ausgabedataset den Ausgabepfad als Wert für diesen Parameter. 

1. Erstellen Sie im Ordner **C:\ADFv2QuickStartPSH** eine JSON-Datei mit dem Namen **BlobDataset.json** und dem folgenden Inhalt:

    ```json
    {
        "name": "BlobDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "@{dataset().path}"
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            },
            "parameters": {
                "path": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Dataset** aus, um das Dataset **BlobDataset** zu erstellen.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "BlobDataset" -DefinitionFile ".\BlobDataset.json"
    ```

    Hier ist die Beispielausgabe:

    ```
    DatasetName       : BlobDataset
    ResourceGroupName : <resourceGroupname>
    DataFactoryName   : <dataFactoryName>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline
  
In dieser Schnellstartanleitung erstellen Sie eine Pipeline mit einer Aktivität, die zwei Parameter akzeptiert: Eingabeblobpfad und Ausgabeblobpfad. Die Werte für diese Parameter werden festgelegt, wenn die Pipeline ausgelöst bzw. ausgeführt wird. Für die Kopieraktivität wird für die Ein- und Ausgabe das gleiche Blobdataset verwendet, das im vorherigen Schritt erstellt wurde. Wenn das Dataset als Eingabedataset verwendet wird, wird der Eingabepfad angegeben. Wenn das Dataset als Ausgabedataset verwendet wird, wird der Ausgabepfad angegeben. 

1. Erstellen Sie im Ordner **C:\ADFv2QuickStartPSH** eine JSON-Datei mit dem Namen **Adfv2QuickStartPipeline.json** und dem folgenden Inhalt:

    ```json
    {
        "name": "Adfv2QuickStartPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToBlob",
                    "type": "Copy",
                    "inputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.inputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.outputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    }
                }
            ],
            "parameters": {
                "inputPath": {
                    "type": "String"
                },
                "outputPath": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Pipeline** aus, um die Pipeline **Adfv2QuickStartPipeline** zu erstellen.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile ".\Adfv2QuickStartPipeline.json"
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

In diesem Schritt legen Sie Werte für die Pipelineparameter **inputPath** und **outputPath** mit tatsächlichen Werten für die Pfade des Quell- und des Senkenblobs fest. Dann erstellen Sie eine Pipelineausführung mithilfe dieser Argumente. 

1. Erstellen Sie im Ordner **C:\ADFv2QuickStartPSH** eine JSON-Datei mit dem Namen **PipelineParameters.json** und dem folgenden Inhalt:

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```
2. Führen Sie das Cmdlet **Invoke-AzureRmDataFactoryV2Pipeline** aus, um eine Pipelineausführung zu erstellen und die Parameterwerte zu übergeben. Das Cmdlet gibt die ID der Pipelineausführung für die zukünftige Überwachung zurück.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

1. Führen Sie das folgende PowerShell-Skript aus, um den Status der Pipelineausführung kontinuierlich zu überwachen, bis das Kopieren der Daten beendet ist. Kopieren Sie das folgende Skript, fügen Sie es in das PowerShell-Fenster ein, und drücken Sie die EINGABETASTE. 

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

    Hier ist die Beispielausgabe der Pipelineausführung:

    ```
    Pipeline is running...status: InProgress
    Pipeline run finished. The status is:  Succeeded
    
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    RunId             : 0000000000-0000-0000-0000-0000000000000
    PipelineName      : Adfv2QuickStartPipeline
    LastUpdated       : 9/28/2017 8:28:38 PM
    Parameters        : {[inputPath, adftutorial/input], [outputPath, adftutorial/output]}
    RunStart          : 9/28/2017 8:28:14 PM
    RunEnd            : 9/28/2017 8:28:38 PM
    DurationInMs      : 24151
    Status            : Succeeded
    Message           :
    ```

    Bei folgendem Fehler:
    ```
    Activity CopyFromBlobToBlob failed: Failed to detect region of linked service 'AzureStorage' : 'AzureStorageLinkedService' with error '[Region Resolver] Azure Storage failed to get address for DNS. Warning: System.Net.Sockets.SocketException (0x80004005): No such host is known
    ```
    führen Sie folgende Schritte aus: 
    1. Überprüfen Sie in „AzureStorageLinkedService.json“, ob der Name und der Schlüssel Ihres Azure Storage-Kontos korrekt sind. 
    2. Vergewissern Sie sich, dass das Format der Verbindungszeichenfolge richtig ist. Die Eigenschaften, etwa „AccountName“ und „AccountKey“, müssen durch Strichpunkt (`;`) getrennt sein. 
    3. Wenn der Kontoname und der Kontoschlüssel in spitzen Klammern stehen, entfernen Sie die Klammern. 
    4. Hier sehen Sie ein Beispiel für eine Verbindungszeichenfolge: 

        ```json
        "connectionString": {
            "value": "DefaultEndpointsProtocol=https;AccountName=mystorageaccountname;AccountKey=mystorageacountkey;EndpointSuffix=core.windows.net",
            "type": "SecureString"
        }
        ```
    5. Erstellen Sie den verknüpften Dienst anhand der folgenden Schritte im Abschnitt [Erstellen eines verknüpften Diensts](#create-a-linked-service) neu. 
    6. Führen Sie die Pipeline anhand der Schritte im Abschnitt [Erstellen einer Pipelineausführung](#create-a-pipeline-run) erneut aus. 
    7. Führen Sie den aktuellen Überwachungsbefehl erneut aus, um die neue Pipelineausführung zu überwachen. 
1. Führen Sie das folgende Skript aus, um Ausführungsdetails zur Kopieraktivität abzurufen, z.B. die Größe der gelesenen/geschriebenen Daten.

    ```powershell
    Write-Output "Activity run details:"
    $Result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineRunId $RunId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $Result

    Write-Output "Activity 'Output' section:"
    $Result.Output -join "`r`n"

    Write-Output "Activity 'Error' section:"
    $Result.Error -join "`r`n"
    ```
3. Vergewissern Sie sich, dass die Ausgabe ähnlich aussieht wie die folgende Beispielausgabe des Ergebnisses einer Aktivitätsausführung:

    ```json
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    ActivityName      : CopyFromBlobToBlob
    PipelineRunId     : 00000000000-0000-0000-0000-000000000000
    PipelineName      : Adfv2QuickStartPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, copyDuration, throughput...}
    LinkedServiceName :
    ActivityRunStart  : 9/28/2017 8:28:18 PM
    ActivityRunEnd    : 9/28/2017 8:28:36 PM
    DurationInMs      : 18095
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity 'Output' section:
    "dataRead": 38
    "dataWritten": 38
    "copyDuration": 7
    "throughput": 0.01
    "errors": []
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)"
    "usedCloudDataMovementUnits": 2
    "billedDuration": 14
    ```

[!INCLUDE [data-factory-quickstart-verify-output-cleanup.md](../../includes/data-factory-quickstart-verify-output-cleanup.md)] 

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-dot-net.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 

---
title: Kopieren von Daten in Blob Storage mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory, um Daten von einem Ordner in einen anderen Ordner in Azure Blob Storage oder an einen anderen Speicherort in derselben Blob Storage-Instanz zu kopieren.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: 8ee2f48db009da4660a03f91194c4e99f6ecac4a
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="create-an-azure-data-factory-using-powershell"></a>Erstellen einer Azure Data Factory mithilfe von PowerShell 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 – Vorschauversion](quickstart-create-data-factory-powershell.md)

Diese Schnellstartanleitung beschreibt, wie Sie PowerShell verwenden, um eine Azure Data Factory zu erstellen. Die in dieser Data Factory erstellte Pipeline kopiert Daten aus einem Ordner in einen anderen Ordner in einem Azure-Blobspeicher. Ein Tutorial zum Transformieren von Daten mithilfe von Azure Data Factory finden Sie im Tutorial [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](transform-data-using-spark.md). 

Dieser Artikel enthält keine ausführliche Einführung in den Data Factory-Dienst. Eine Einführung in den Azure Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](introduction.md).

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-subscription"></a>Azure-Abonnement
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

### <a name="azure-storage-account"></a>Azure Storage-Konto
Sie verwenden in diesem Schnellstart ein Azure Storage-Allzweckkonto (Blob Storage) als Datenspeicher vom Typ **Quelle** und vom Typ **Senke/Ziel**. Falls Sie noch nicht über ein Azure Storage-Allzweckkonto verfügen, helfen Ihnen die Informationen unter [Erstellen Sie ein Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) weiter. 

#### <a name="get-storage-account-name-and-account-key"></a>Abrufen des Speicherkontonamens und des Kontoschlüssels
In diesem Schnellstart verwenden Sie Name und Schlüssel Ihres Azure Storage-Kontos. Das folgende Verfahren enthält die Schritte zum Abrufen des Namens und Schlüssels für Ihr Speicherkonto. 

1. Starten Sie einen Webbrowser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com). Melden Sie sich mit Ihrem Azure-Benutzernamen und -Kennwort an. 
2. Klicken Sie im Menü auf der linken Seite auf **Weitere Dienste >**, filtern Sie nach dem Schlüsselwort **Speicher**, und wählen Sie **Speicherkonten**.

    ![Suchen nach einem Speicherkonto](media/quickstart-create-data-factory-powershell/search-storage-account.png)
3. Filtern Sie in der Liste mit den Speicherkonten nach Ihrem Speicherkonto (falls erforderlich), und wählen Sie **Ihr Speicherkonto** aus. 
4. Wählen Sie auf der Seite **Speicherkonto** im Menü die Option **Zugriffsschlüssel**.

    ![Abrufen des Speicherkontonamens und -schlüssels](media/quickstart-create-data-factory-powershell/storage-account-name-key.png)
5. Kopieren Sie die Werte für die Felder **Speicherkontoname** und **key1** in die Zwischenablage. Fügen Sie sie in einen Editor ein, und speichern Sie sie.  

#### <a name="create-input-folder-and-files"></a>Erstellen des Eingabeordners und der Dateien
In diesem Abschnitt erstellen Sie einen Blobcontainer mit dem Namen „adftutorial“ in Ihrem Azure-Blobspeicher. Anschließend erstellen Sie den Ordner „input“ im Container und laden eine Beispieldatei in den Eingabeordner hoch. 

1. Installieren Sie den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/), falls er auf Ihrem Computer noch nicht vorhanden ist. 
2. Starten Sie den **Microsoft Azure Storage-Explorer** auf Ihrem Computer.   
3. Wählen Sie im Fenster **Connect to Azure Storage** (Verbindung mit Azure Storage herstellen) die Option **Use a storage account name and key** (Einen Speicherkontonamen und -schlüssel verwenden), und klicken Sie auf **Weiter**. Falls das Fenster **Connect to Azure Storage** (Verbindung mit Azure Storage herstellen) nicht angezeigt wird, können Sie in der Strukturansicht mit der rechten Maustaste auf **Speicherkonten** und anschließend auf **Connect to Azure storage** (Verbindung mit Azure Storage herstellen) klicken. 

    ![Herstellen einer Verbindung mit Azure Storage](media/quickstart-create-data-factory-powershell/storage-explorer-connect-azure-storage.png)
4. Fügen Sie im Fenster **Attach using Name and Key** (Mit Name und Schlüssel anfügen) den **Kontonamen** und **Kontoschlüssel** aus dem vorherigen Schritt ein. Klicken Sie auf **Weiter**. 
5. Klicken Sie im Fenster **Verbindungszusammenfassung** auf **Verbinden**.
6. Vergewissern Sie sich, dass Ihr Speicherkonto in der Strukturansicht unter **(Local and Attached) ((Lokal und angefügt))** -> **Speicherkonten** angezeigt wird. 
7. Erweitern Sie **BLOB-Container**, und stellen Sie sicher, dass der Blobcontainer **adftutorial** nicht vorhanden ist. Sie können die nächsten Schritte zum Erstellen des Containers überspringen, falls er bereits vorhanden sein sollte. 
8. Klicken Sie mit der rechten Maustaste auf **BLOB-Container**, und wählen Sie **BLOB-Container erstellen**.

    ![Erstellen eines Blobcontainers](media/quickstart-create-data-factory-powershell/stroage-explorer-create-blob-container-menu.png)
9. Geben Sie als Name **adftutorial** ein, und drücken Sie die **EINGABETASTE**. 
10. Vergewissern Sie sich, dass der Container **adftutorial** in der Strukturansicht ausgewählt ist. 
11. Klicken Sie in der Symbolleiste auf **Neuer Ordner**. 

    ![Schaltfläche „Ordner erstellen“](media/quickstart-create-data-factory-powershell/stroage-explorer-new-folder-button.png)
12. Geben Sie im Fenster **Neues virtuelles Verzeichnis erstellen** **input** als **Name** ein, und klicken Sie auf **OK**. 

    ![Dialogfeld „Verzeichnis erstellen“](media/quickstart-create-data-factory-powershell/storage-explorer-create-new-directory-dialog.png)
13. Starten Sie den **Editor**, und erstellen Sie eine Datei mit dem Namen **emp.txt** und folgendem Inhalt: 
    
    ```
    John, Doe
    Jane, Doe
    ```    
    Speichern Sie sie im Ordner **c:\ADFv2QuickStartPSH**. Erstellen Sie den Ordner **ADFv2QuickStartPSH**, falls er nicht vorhanden ist. 
14. Klicken Sie in der Symbolleiste auf die Schaltfläche **Hochladen**, und wählen Sie die Option **Dateien hochladen**. 

    ![Schaltfläche zum Hochladen](media/quickstart-create-data-factory-powershell/storage-explorer-upload-button.png)
15. Wählen Sie im Fenster **Dateien hochladen** für **Dateien** die Option `...`. 
16. Navigieren Sie im Fenster **Select folder to upload** (Ordner für Upload auswählen) zum Ordner mit der Datei **emp.txt**, und wählen Sie die Datei aus. 

    ![Dialogfeld „Dateien hochladen“](media/quickstart-create-data-factory-powershell/storage-explorer-upload-files-dialog.png)
17. Klicken Sie im Fenster **Dateien hochladen** auf **Hochladen**. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Installieren von Azure Powershell
Installieren Sie die neueste Version von Azure PowerShell, falls sie auf Ihrem Computer noch nicht vorhanden ist. 

1. Navigieren Sie in Ihrem Webbrowser zur Seite mit den [Azure SDK-Downloads und SDKs](https://azure.microsoft.com/downloads/). 
2. Klicken Sie unter **Befehlszeilentools** -> **PowerShell** auf **Windows-Installation**. 
3. Führen Sie die **MSI**-Datei aus, um Azure PowerShell zu installieren. 

Ausführliche Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Anmelden an Azure PowerShell
Starten Sie **PowerShell** auf Ihrem Computer. Lassen Sie Azure PowerShell bis zum Ende dieser Schnellstartanleitung geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.

1. Führen Sie den folgenden Befehl aus, und geben Sie den Azure-Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen, wenn Sie über mehrere Azure-Abonnements verfügen:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Führen Sie den folgenden Befehl aus, um das gewünschte Abonnement auszuwählen: Ersetzen Sie **SubscriptionId** durch die ID Ihres Azure-Abonnements:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
1. Definieren Sie eine Variable für den Ressourcengruppennamen zur späteren Verwendung in PowerShell-Befehlen. Kopieren Sie den folgenden Befehlstext nach PowerShell, geben Sie einen Namen für die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in doppelten Anführungszeichen an, und führen Sie dann den Befehl aus. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Definieren Sie eine Variable für den Data Factory-Namen zur späteren Verwendung in PowerShell-Befehlen. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Definieren Sie eine Variable für den Speicherort der Data Factory: 

    ```powershell
    $location = "East US"
    ```
4. Führen Sie den folgenden Befehl aus, um die Azure-Ressourcengruppe zu erstellen: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 
    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$resourceGroupName` einen anderen Wert zu, und versuchen Sie es noch einmal. Fahren Sie mit dem nächsten Schritt fort, wenn Sie die Ressourcengruppe für andere Benutzer freigeben möchten. 
5. Führen Sie zum Erstellen der Data Factory das folgende **Set-AzureRmDataFactoryV2**-Cmdlet aus: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Beachten Sie folgende Punkte:

* Der Name der Azure Data Factory muss global eindeutig sein. Wenn die folgende Fehlermeldung angezeigt wird, ändern Sie den Namen, und wiederholen Sie den Vorgang.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Um Data Factory-Instanzen zu erstellen, müssen Sie **Mitwirkender** oder **Administrator** des Azure-Abonnements sein.
* Derzeit können Sie mit Data Factory Version 2 nur in den Regionen „USA, Osten“, „USA, Osten 2“ und „Europa, Westen“ Data Factorys erstellen. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.

## <a name="create-a-linked-service"></a>Erstellen eines verknüpften Diensts

Erstellen Sie verknüpfte Dienste in einer Data Factory, um Ihre Datenspeicher und Computedienste mit der Data Factory zu verknüpfen. In dieser Schnellstartanleitung müssen Sie nur einen verknüpften Azure Storage-Dienst erstellen, der sowohl als Quell- als auch als Senkendatenspeicher verwendet wird – in diesem Beispiel als „AzureStorageLinkedService“ bezeichnet.

1. Erstellen Sie eine JSON-Datei mit dem Namen **AzureStorageLinkedService.json** im Ordner **C:\ADFv2QuickStartPSH** und dem folgenden Inhalt (erstellen Sie den Ordner „ADFv2QuickStartPSH“, wenn dieser noch nicht vorhanden ist): 

    > [!IMPORTANT]
    > Ersetzen Sie &lt;accountname&gt; und &lt;accountkey&gt; durch den Namen bzw. Schlüssel Ihres Azure Storage-Kontos, bevor Sie die Datei speichern.

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>",
                    "type": "SecureString"
                }
            }
        }
    }
    ```

2. Wechseln Sie in **Azure PowerShell** zum Ordner **ADFv2QuickStartPSH**.

3. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2LinkedService** aus, um den verknüpften Dienst **AzureStorageLinkedService** zu erstellen. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Hier ist die Beispielausgabe:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Erstellen eines Datasets

Sie definieren ein Dataset, das die Daten repräsentiert, die aus einer Quelle in eine Senke kopiert werden sollen. In diesem Beispiel verweist dieses Blobdataset auf den verknüpften Azure Storage-Dienst, den Sie im vorherigen Schritt erstellt haben. Das Dataset akzeptiert einen Parameter, dessen Wert in einer Aktivität festgelegt ist, die das Dataset verwendet. Der Parameter wird verwendet, um den **folderPath** zu erstellen, der auf den Speicherort der Dateien zeigt.

1. Erstellen Sie im Ordner **C:\ADFv2QuickStartPSH** eine JSON-Datei mit dem Namen **BlobDataset.json** und dem folgenden Inhalt:

    ```json
    {
        "name": "BlobDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": {
                    "value": "@{dataset().path}",
                    "type": "Expression"
                }
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
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "BlobDataset" -DefinitionFile ".\BlobDataset.json"
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
  
In diesem Beispiel enthält die Pipeline eine Aktivität und akzeptiert zwei Parameter: Eingabeblobpfad und Ausgabeblobpfad. Die Werte für diese Parameter werden festgelegt, wenn die Pipeline ausgelöst bzw. ausgeführt wird. Für die Kopieraktivität wird für die Ein- und Ausgabe das gleiche Blobdataset verwendet, das im vorherigen Schritt erstellt wurde. Wenn das Dataset als Eingabedataset verwendet wird, wird der Eingabepfad angegeben. Wenn das Dataset als Ausgabedataset verwendet wird, wird der Ausgabepfad angegeben. 

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
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile ".\Adfv2QuickStartPipeline.json"
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

    Ersetzen Sie den Wert von **inputPath** und **outputPath** durch den Pfad des Quell- bzw. Senkenblobs, wenn Sie andere Container und Ordner nutzen.

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```

2. Führen Sie das Cmdlet **Invoke-AzureRmDataFactoryV2Pipeline** aus, um eine Pipelineausführung zu erstellen und die Parameterwerte zu übergeben. Das Cmdlet erfasst auch die ID der Pipelineausführung für die zukünftige Überwachung.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-a-pipeline-run"></a>Überwachen einer Pipelineausführung

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

## <a name="verify-the-output"></a>Überprüfen der Ausgabe
Die Pipeline erstellt den Ausgabeordner automatisch im Blobcontainer „adftutorial“. Anschließend wird die Datei „emp.txt“ aus dem Eingabe- in den Ausgabeordner kopiert. Verwenden Sie den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/), um zu überprüfen, ob die Blobs von „inputBlobPath“ nach „outputBlobPath“ kopiert werden. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Die im Rahmen dieser Schnellstartanleitung erstellten Ressourcen können auf zwei Arten bereinigt werden. Sie können die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) einschließlich aller darin enthaltenen Ressourcen löschen. Falls die anderen Ressourcen erhalten bleiben sollen, löschen Sie nur die Data Factory, die Sie in diesem Tutorial erstellt haben.

Führen Sie den folgenden Befehl aus, um die gesamte Ressourcengruppe zu löschen: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Führen Sie den folgenden Befehl aus, um nur die Data Factory zu löschen: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-dot-net.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 

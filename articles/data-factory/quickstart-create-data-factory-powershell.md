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
ms.date: 11/16/2017
ms.author: jingwang
ms.openlocfilehash: 254dcb6642afc19f434df837c9073d2dd7314313
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2017
---
# <a name="create-an-azure-data-factory-using-powershell"></a>Erstellen einer Azure Data Factory mithilfe von PowerShell 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: Allgemein verfügbare Version](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 – Vorschauversion](quickstart-create-data-factory-powershell.md)

Diese Schnellstartanleitung beschreibt, wie Sie PowerShell verwenden, um eine Azure Data Factory zu erstellen. Die in dieser Data Factory erstellte Pipeline **kopiert** Daten aus einem Ordner in einen anderen Ordner in einem Azure-Blobspeicher. Ein Tutorial zum **Transformieren** von Daten mithilfe von Azure Data Factory finden Sie im Tutorial [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](transform-data-using-spark.md). 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
>
> Dieser Artikel enthält keine ausführliche Einführung in den Data Factory-Dienst. Eine Einführung in den Azure Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](introduction.md).

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-subscription"></a>Azure-Abonnement
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

### <a name="azure-roles"></a>Azure-Rollen
Damit Sie Data Factory-Instanzen erstellen können, muss das Benutzerkonto, mit dem Sie sich bei Azure anmelden, ein Mitglied der Rolle **Mitwirkender** oder **Besitzer** oder ein **Administrator** des Azure-Abonnements sein. Klicken Sie im Azure-Portal in der oberen rechten Ecke auf Ihren **Benutzernamen** und auf **Berechtigungen**, um Ihre Berechtigungen im Abonnement anzuzeigen. Wenn Sie Zugriff auf mehrere Abonnements besitzen, wählen Sie das entsprechende Abonnement aus. Beispielanweisungen zum Hinzufügen eines Benutzers zu einer Rolle finden Sie im Artikel [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="azure-storage-account"></a>Azure Storage-Konto
Sie verwenden in diesem Schnellstart ein Azure Storage-Allzweckkonto (Blob Storage) als Datenspeicher vom Typ **Quelle** und vom Typ **Ziel**. Falls Sie noch nicht über ein Azure Storage-Allzweckkonto verfügen, helfen Ihnen die Informationen unter [Erstellen Sie ein Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) weiter. 

#### <a name="get-storage-account-name-and-account-key"></a>Abrufen des Speicherkontonamens und des Kontoschlüssels
In diesem Schnellstart verwenden Sie Name und Schlüssel Ihres Azure-Speicherkontos. Das folgende Verfahren enthält die Schritte zum Abrufen des Namens und Schlüssels für Ihr Speicherkonto. 

1. Starten Sie einen Webbrowser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com). Melden Sie sich mit Ihrem Azure-Benutzernamen und -Kennwort an. 
2. Klicken Sie im Menü auf der linken Seite auf **Weitere Dienste >**, filtern Sie nach dem Schlüsselwort **Speicher**, und wählen Sie **Speicherkonten**.

    ![Suchen nach einem Speicherkonto](media/quickstart-create-data-factory-powershell/search-storage-account.png)
3. Filtern Sie in der Liste mit den Speicherkonten nach Ihrem Speicherkonto (falls erforderlich), und wählen Sie **Ihr Speicherkonto** aus. 
4. Wählen Sie auf der Seite **Speicherkonto** im Menü die Option **Zugriffsschlüssel**.

    ![Abrufen des Speicherkontonamens und -schlüssels](media/quickstart-create-data-factory-powershell/storage-account-name-key.png)
5. Kopieren Sie die Werte für die Felder **Speicherkontoname** und **key1** in die Zwischenablage. Fügen Sie sie in einen Editor ein, und speichern Sie sie.  

#### <a name="create-input-folder-and-files"></a>Erstellen des Eingabeordners und der Dateien
In diesem Abschnitt erstellen Sie einen Blobcontainer mit dem Namen **adftutorial** in Ihrem Azure-Blobspeicher. Anschließend erstellen Sie den Ordner **Eingabe** im Container und laden eine Beispieldatei in den Eingabeordner hoch. 

1. Wechseln Sie auf der Seite **Speicherkonto** zu **Übersicht**, und klicken Sie dann auf **Blobs**. 

    ![Option zum Auswählen von Blobs](media/quickstart-create-data-factory-powershell/select-blobs.png)
2. Klicken Sie auf der Seite **Blob-Dienst** auf der Symbolleiste auf **+ Container**. 

    ![Schaltfläche „Container hinzufügen“](media/quickstart-create-data-factory-powershell/add-container-button.png)    
3. Geben Sie im Dialogfeld **Neuer Container** als Namen **adftutorial** ein, und klicken Sie auf **OK**. 

    ![Eingeben des Containernamens](media/quickstart-create-data-factory-powershell/new-container-dialog.png)
4. Klicken Sie in der Liste der Container auf **adftutorial**. 

    ![Auswählen des Containers](media/quickstart-create-data-factory-powershell/seelct-adftutorial-container.png)
1. Klicken Sie auf der Seite **Container** auf der Symbolleiste auf **Hochladen**.  

    ![Schaltfläche zum Hochladen](media/quickstart-create-data-factory-powershell/upload-toolbar-button.png)
6. Klicken Sie auf der Seite **Blob hochladen** auf **Erweitert**.

    ![Klicken auf den Link „Erweitert“](media/quickstart-create-data-factory-powershell/upload-blob-advanced.png)
7. Starten Sie **Editor**, und erstellen Sie eine Datei mit dem Namen **emp.txt** und folgendem Inhalt: Speichern Sie sie im Ordner **c:\ADFv2QuickStartPSH**: Erstellen Sie den Ordner **ADFv2QuickStartPSH**, sofern er noch nicht vorhanden ist.
    
    ```
    John, Doe
    Jane, Doe
    ```    
8. Wählen Sie im Azure-Portal auf der Seite **Blob hochladen** im Feld **Dateien** die Datei **emp.txt** aus. 
9. Geben Sie für das Feld **In Ordner hochladen** den Wert **Eingabe** ein. 

    ![Einstellungen für das Hochladen von Blobs](media/quickstart-create-data-factory-powershell/upload-blob-settings.png)    
10. Vergewissern Sie sich, dass als Ordner **Eingabe** und als Datei **emp.txt** ausgewählt ist, und klicken Sie auf **Hochladen**.
11. Daraufhin sollten in der Liste die Datei **emp.txt** und der Status des Uploads angezeigt werden. 
12. Schließen Sie die Seite **Blob hochladen**, indem Sie in der Ecke auf **X** klicken. 

    ![Schließen der Seite „Blob hochladen“](media/quickstart-create-data-factory-powershell/close-upload-blob.png)
1. Lassen Sie die Seite **Container** geöffnet. Sie überprüfen darauf am Ende dieser Schnellstartanleitung die Ausgabe. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Installieren von Azure Powershell
Installieren Sie die neueste Version von Azure PowerShell, falls sie auf Ihrem Computer noch nicht vorhanden ist. 

1. Navigieren Sie in Ihrem Webbrowser zur Seite mit den [Azure SDK-Downloads und SDKs](https://azure.microsoft.com/downloads/). 
2. Klicken Sie unter **Befehlszeilentools** -> **PowerShell** auf **Windows-Installation**. 
3. Führen Sie die **MSI**-Datei aus, um Azure PowerShell zu installieren. 

Ausführliche Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Anmelden an Azure PowerShell

1. Starten Sie **PowerShell** auf Ihrem Computer. Lassen Sie Azure PowerShell bis zum Ende dieser Schnellstartanleitung geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.

    ![Starten von PowerShell](media/quickstart-create-data-factory-powershell/search-powershell.png)
1. Führen Sie den folgenden Befehl aus, und geben Sie den gleichen Azure-Benutzernamen und das gleiche Kennwort ein, die Sie bei der Anmeldung beim Azure-Portal verwendet haben:
       
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
1. Definieren Sie eine Variable für den Ressourcengruppennamen zur späteren Verwendung in PowerShell-Befehlen. Kopieren Sie den folgenden Befehlstext nach PowerShell, geben Sie einen Namen für die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in doppelten Anführungszeichen an, und führen Sie dann den Befehl aus. Beispiel: `"adfrg"`.
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Definieren Sie eine Variable für den Namen der Data Factory. 

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
    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$resourceGroupName` einen anderen Wert zu, und führen Sie den Befehl erneut aus. 
5. Führen Sie zum Erstellen der Data Factory das folgende **Set-AzureRmDataFactoryV2**-Cmdlet aus: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
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
In diesem Schritt definieren Sie ein Dataset, das die Daten repräsentiert, die aus einer Quelle in eine Senke kopiert werden sollen. Das Dataset ist vom Typ **AzureBlob**. Es verweist auf den **mit Azure Storage verknüpften Dienst**, den Sie im vorherigen Schritt erstellt haben. Es verwendet einen Parameter zum Erstellen der **folderPath**-Eigenschaft. Für ein Eingabedataset übergibt die Kopieraktivität in der Pipeline den Eingabepfad als Wert für diesen Parameter. Analog dazu übergibt die Kopieraktivität für ein Ausgabedataset den Ausgabepfad als Wert für diesen Parameter. 

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

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```
2. Führen Sie das Cmdlet **Invoke-AzureRmDataFactoryV2Pipeline** aus, um eine Pipelineausführung zu erstellen und die Parameterwerte zu übergeben. Das Cmdlet gibt die ID der Pipelineausführung für die zukünftige Überwachung zurück.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

1. Führen Sie das folgende PowerShell-Skript aus, um den Status der Pipelineausführung kontinuierlich zu überwachen, bis das Kopieren der Daten beendet ist. Kopieren Sie das folgende Skript, fügen Sie es in das PowerShell-Fenster ein, und drücken Sie die EINGABETASTE. 

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
Die Pipeline erstellt den Ausgabeordner automatisch im Blobcontainer „adftutorial“. Anschließend wird die Datei „emp.txt“ aus dem Eingabe- in den Ausgabeordner kopiert. 

1. Klicken Sie im Azure-Portal auf der Seite des Containers **adftutorial** auf **Aktualisieren**, um den Ausgabeordner anzuzeigen. 
    
    ![Aktualisieren](media/quickstart-create-data-factory-powershell/output-refresh.png)
2. Klicken Sie in der Ordnerliste auf **Ausgabe**. 
2. Überprüfen Sie, ob die Datei **emp.txt** in den Ausgabeordner kopiert wurde. 

    ![Aktualisieren](media/quickstart-create-data-factory-powershell/output-file.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Die im Rahmen dieser Schnellstartanleitung erstellten Ressourcen können auf zwei Arten bereinigt werden. Sie können die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) einschließlich aller darin enthaltenen Ressourcen löschen. Falls die anderen Ressourcen erhalten bleiben sollen, löschen Sie nur die Data Factory, die Sie in diesem Tutorial erstellt haben.

Wenn Sie eine Ressourcengruppe löschen, werden alle Ressourcen einschließlich enthaltener Data Factorys gelöscht. Führen Sie den folgenden Befehl aus, um die gesamte Ressourcengruppe zu löschen: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Wenn Sie nur die Data Factory und nicht die gesamte Ressourcengruppe löschen möchten, führen Sie den folgenden Befehl aus: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-dot-net.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 

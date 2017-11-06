---
title: Kopieren von lokalen Daten in die Cloud mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe der selbstgehosteten Integration Runtime in Azure Data Factory Daten aus einem lokalen Datenspeicher in die Azure-Cloud kopieren.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/06/2017
ms.author: jingwang
ms.openlocfilehash: 95d1dce536f8f8f0fc8d93f201520fd84f0f7629
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="copy-data-between-on-premises-and-cloud"></a>Kopieren von Daten zwischen lokalen Quellen und der Cloud

[!INCLUDE [data-factory-what-is-include-md](../../includes/data-factory-what-is-include.md)]

#### <a name="this-tutorial"></a>Dieses Tutorial umfasst folgende Punkte

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) weiter.

In diesem Tutorial verwenden Sie Azure PowerShell, um eine Data Factory-Pipeline zu erstellen, die Daten aus einer lokalen SQL Server-Datenbank in einen Azure Blob Storage verschiebt. Sie erstellen und verwenden eine selbstgehostete Integration Runtime (IR) von Azure Data Factory, die eine Integration von lokalen Datenspeichern und Clouddatenspeichern ermöglicht.  Informationen zur Verwendung von anderen Tools/SDKs zum Erstellen einer Data Factory finden Sie unter [Schnellstarts](quickstart-create-data-factory-dot-net.md).

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen eine Data Factory.
> * Erstellen einer selbstgehosteten Integration Runtime.
> * Erstellen und Verschlüsseln eines lokalen verknüpften SQL Server-Diensts in der selbstgehosteten Integration Runtime.
> * Erstellen des verknüpften Azure Storage-Diensts.
> * Erstellen von SQL Server- und Azure Storage-Datasets.
> * Erstellen einer Pipeline mit Kopieraktivität zum Verschieben der Daten
> * Starten einer Pipelineausführung
> * Überwachen der Pipeline- und Aktivitätsausführungen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* **SQL Server**. In diesem Tutorial verwenden Sie eine lokale SQL Server-Datenbank als **Quelldatenspeicher**.
* **Azure Storage-Konto**. In diesem Tutorial verwenden Sie Azure Blob Storage als **Ziel-/Senkendatenspeicher**. Wenn Sie kein Azure Storage-Konto haben, finden Sie im Artikel [Erstellen eines Speicherkontos](../storage/common/storage-create-storage-account.md#create-a-storage-account) Schritte zum Erstellen eines Azure Storage-Kontos.
* **Azure PowerShell**. Befolgen Sie die Anweisungen unter [Get started with Azure PowerShell cmdlets](/powershell/azure/install-azurerm-ps) (Erste Schritte mit Azure PowerShell-Cmdlets).

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Starten Sie **PowerShell**. Lassen Sie Azure PowerShell bis zum Ende dieses Tutorials geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.

    Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben:
    ```powershell
    Login-AzureRmAccount
    ```
    Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen:

    ```powershell
    Get-AzureRmSubscription
    ```
    Führen Sie den folgenden Befehl aus, um das gewünschte Abonnement auszuwählen: Ersetzen Sie **SubscriptionId** durch die ID Ihres Azure-Abonnements:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"
    ```
2. Führen Sie zum Erstellen einer Data Factory das Cmdlet **Set-AzureRmDataFactoryV2** aus. Ersetzen Sie vor dem Ausführen des Befehls die Platzhalter durch Ihre eigenen Werte.

    ```powershell
    $resourceGroupName = "<your resource group to create the factory>"
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>"
    $df = Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName
    ```

    Beachten Sie folgende Punkte:

    * Der Name der Azure Data Factory muss global eindeutig sein. Wenn die folgende Fehlermeldung angezeigt wird, ändern Sie den Namen, und wiederholen Sie den Vorgang.

        ```
        Data factory name "<data factory name>" is not available.
        ```

    * Um Data Factory-Instanzen zu erstellen, müssen Sie Mitwirkender oder Administrator des Azure-Abonnements sein.
    * Derzeit ermöglicht der Data Factory-Dienst das Erstellen einer Data Factory nur in den Regionen „USA, Osten“ und „USA, Osten 2“. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.

## <a name="create-a-self-hosted-ir"></a>Erstellen einer selbstgehosteten IR

In diesem Abschnitt erstellen Sie eine selbstgehostete Integration Runtime und ordnen sie einem lokalen Knoten (Computer) zu.

1. Erstellen Sie eine selbstgehostete Integration Runtime. Verwenden Sie einen eindeutigen Namen. Es darf keine andere Integration Runtime mit dem gleichen Namen vorhanden sein.

   ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Hier ist die Beispielausgabe:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Führen Sie den folgenden Befehl aus, um den Status der erstellten Integration Runtime abzurufen.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Hier ist die Beispielausgabe:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Führen Sie den folgenden Befehl aus, um Authentifizierungsschlüssel abzurufen, mit denen Sie die selbstgehostete Integration Runtime beim Data Factory-Dienst in der Cloud registrieren können. Kopieren Sie einen der Schlüssel zum Registrieren der selbstgehosteten Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Hier ist die Beispielausgabe:

   ```json
   {
       "AuthKey1":  "IR@8437c862-d6a9-4fb3-87dd-7d4865a9e845@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@8437c862-d6a9-4fb3-85dd-7d4865a9e845@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

4. Jetzt können Sie die selbstgehostete Integration Runtime auf einen lokalen Windows-Computer [herunterladen](https://www.microsoft.com/download/details.aspx?id=39717) und den Authentifizierungsschlüssel aus dem vorherigen Schritt verwenden, um die selbstgehostete Integration Runtime manuell zu registrieren.

   ![Registrieren der Integration Runtime](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)

   Folgende Meldung wird angezeigt, wenn die selbstgehostete Integration Runtime erfolgreich registriert wurde:

   ![Erfolgreich registriert](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

   Folgende Seite wird angezeigt, wenn der Knoten mit dem Clouddienst verbunden ist:

   ![Knoten ist verbunden](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Erstellen eines verknüpften Azure Storage-Diensts (Ziel/Senke)

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **AzureStorageLinkedService.json** und dem folgenden Inhalt. Erstellen Sie den Ordner „ADFv2Tutorial“, falls dieser noch nicht vorhanden ist.  Ersetzen Sie &lt;accountname&gt; und &lt;accountkey&gt; durch den Namen bzw. Schlüssel Ihres Azure Storage-Kontos.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```

2. Wechseln Sie in **Azure PowerShell** zum Ordner **ADFv2Tutorial**.

   Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2LinkedService** aus, um den verknüpften Dienst **AzureStorageLinkedService** zu erstellen. Die Cmdlets in diesem Tutorial akzeptieren Werte für die Parameter **ResourceGroupName** und **DataFactoryName**. Alternativ dazu können Sie auch das vom Cmdlet „Set-AzureRmDataFactoryV2“ zurückgegebene **DataFactory**-Objekt übergeben, ohne „ResourceGroupName“ und „DataFactoryName“ bei jeder Ausführung eines Cmdlets einzugeben.

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Hier ist eine Beispielausgabe:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Erstellen und Verschlüsseln einen verknüpftes SQL Server-Diensts (Quelle)

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **SqlServerLinkedService.json** und dem folgenden Inhalt: Ersetzen Sie **&lt;servername>**, **&lt;databasename>**, **&lt;username>**, **&lt;servername>** und **&lt;password>** durch die Werte Ihrer SQL Server-Instanz, bevor Sie die Datei speichern. Ersetzen Sie **&lt;integration** **runtime** **name>** durch den Namen Ihrer Integration Runtime.

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```
2. Um vertrauliche Daten aus der JSON-Nutzlast in der lokalen selbstgehosteten Integration Runtime zu verschlüsseln, können Sie **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** ausführen und die obige JSON-Nutzlast übergeben. So wird sichergestellt, dass die Anmeldeinformationen per DPAPI (Data Protection Application Programming Interface) verschlüsselt und lokal auf dem Knoten der selbstgehosteten Integration Runtime gespeichert werden. Die Ausgabenutzlast kann an eine weitere JSON-Datei umgeleitet werden (in diesem Fall „encryptedLinkedService.json“), die verschlüsselte Anmeldeinformationen enthält.

    Ersetzen Sie **&lt;integration runtime name&gt;** durch den Namen Ihrer Integration Runtime, bevor Sie den Befehl ausführen.

   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName <integration runtime name> -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Führen Sie den folgenden Befehl mithilfe der im vorherigen Schritt erstellten JSON-Datei aus, um den **SqlServerLinkedService** zu erstellen:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Erstellen von Datasets

### <a name="prepare-an-on-premises-sql-server-for-the-tutorial"></a>Vorbereitung einer lokalen SQL Server-Instanz für das Tutorial

In diesem Schritt erstellen Sie die Eingabe- und Ausgabedatasets, die ein- und ausgehende Daten für den Kopiervorgang darstellen (lokale SQL Server-Datenbank = > Azure-Blobspeicher). Führen Sie vor dem Erstellen von Datasets die folgenden Schritte aus (ausführliche Schritte finden Sie im Anschluss an die Liste):

- Erstellen Sie eine Tabelle mit dem Namen **emp** in der SQL Server-Datenbank, die Sie der Data Factory als verknüpften Dienst hinzugefügt haben, und fügen Sie verschiedene Beispieleinträge in die Tabelle ein.
- Erstellen Sie einen Blob-Container mit dem Namen **adftutorial** im Azure-Blob-Speicherkonto, das Sie der Data Factory als verknüpften Dienst hinzugefügt haben.


1. Verwenden Sie in der Datenbank, die Sie für den lokalen verknüpften SQL Server-Dienst (**SqlServerLinkedService**) angegeben haben, das folgende SQL-Skript zum Erstellen der Tabelle **emp** in der Datenbank.

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Fügen Sie einige Beispiele in die Tabelle ein:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```



### <a name="create-a-dataset-for-source-sql-database"></a>Erstellen eines Datasets für die SQL-Quelldatenbank

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **SqlServerDataset.json** und dem folgenden Inhalt:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Dataset** aus, um das Dataset **SqlServerDataset** zu erstellen.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Erstellen eines Datasets für eine Azure Blob Storage-Senke

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **AzureBlobDataset.json** und dem folgenden Inhalt:

    > [!IMPORTANT]
    > Bei diesem Beispielcode wird davon ausgegangen, dass Sie im Azure Blob Storage über einen Container mit dem Namen **adftutorial** verfügen.

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. Um das Dataset **AzureBlobDataset** zu erstellen, führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Dataset** aus.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-pipelines"></a>Erstellen von Pipelines

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>Erstellen der Pipeline „SqlServerToBlobPipeline“

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **SqlServerToBlobPipeline.json** und dem folgenden Inhalt:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Pipeline** aus, um die Pipeline **SQLServerToBlobPipeline** zu erstellen.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```



## <a name="start-and-monitor-a-pipeline-run"></a>Starten und Überwachen einer Pipelineausführung

1. Starten Sie eine Pipelineausführung für die Pipeline „SQLServerToBlobPipeline“, und erfassen Sie die ID der Pipelineausführung für die zukünftige Überwachung.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. Führen Sie das folgende Skript aus, um den Ausführungsstatus der Pipeline **SQLServerToBlobPipeline** kontinuierlich zu überprüfen und das Endergebnis auszugeben.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Hier ist die Ausgabe der Beispielausführung:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. Sie können die Ausführungs-ID der Pipeline **SQLServerToBlobPipeline** abrufen und das detaillierte Ergebnis der Aktivitätsausführung wie folgt überprüfen.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Hier ist die Ausgabe der Beispielausführung:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 4,
      "throughput": 0.01,
      "errors": []
    }
    ```
4. Stellen Sie eine Verbindung mit Ihrer Azure Blob Storage-Senke her, und vergewissern Sie sich, dass die Daten ordnungsgemäß aus der Azure SQL-Datenbank kopiert wurden.

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer selbstgehosteten Integration Runtime.
> * Erstellen und Verschlüsseln eines lokalen verknüpften SQL Server-Diensts in der selbstgehosteten Integration Runtime.
> * Erstellen des verknüpften Azure Storage-Diensts.
> * Erstellen von SQL Server- und Azure Storage-Datasets.
> * Erstellen einer Pipeline mit Kopieraktivität zum Verschieben der Daten
> * Starten einer Pipelineausführung
> * Überwachen der Pipeline- und Aktivitätsausführungen.

Im Artikel [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats) finden Sie eine Liste der Datenspeicher, die als Quellen und Senken von Azure Data Factory unterstützt werden.

Fahren Sie mit dem folgenden Tutorial fort, um mehr über das Kopieren von Daten per Massenvorgang aus einer Quelle in ein Ziel zu erfahren:

> [!div class="nextstepaction"]
>[Massenkopieren von Daten](tutorial-bulk-copy.md)

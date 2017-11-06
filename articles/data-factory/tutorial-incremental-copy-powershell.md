---
title: Inkrementelles Kopieren von Daten mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline, die Daten aus einer Azure SQL-Datenbank in einen Azure Blob Storage kopiert.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/06/2017
ms.author: shlo
ms.openlocfilehash: f6f90f35fc4d26c0cdb021e85a159d4984d48399
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="incrementally-load-data-from-azure-sql-database-to-azure-blob-storage"></a>Inkrementelles Laden von Daten aus Azure SQL-Datenbank in Azure Blob Storage

[!INCLUDE [data-factory-what-is-include-md](../../includes/data-factory-what-is-include.md)]

#### <a name="this-tutorial"></a>Dieses Tutorial umfasst folgende Punkte

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) weiter.

Im Rahmen der Datenintegrationserfahrung ist eines der weit verbreiteten Szenarios das inkrementelle Laden von Daten in regelmäßigen Abständen, um aktualisierte Analyseergebnisse, nach dem anfänglichen Laden von Daten und Analysen, zu aktualisieren. In diesem Tutorial konzentrieren Sie sich auf das Laden ausschließlich neuer oder aktualisierter Datensätze aus den Datenquellen in Datensenken. Die Ausführung ist im Vergleich zu vollständigem Laden, insbesondere für große Datasets effizienter.    

Sie können Data Factory zum Erstellen von Lösungen mit hohem Grenzwert verwenden, um das inkrementelle Laden von Daten zu erzielen, indem Sie Lookup-, Copy- und Stored Procedure-Aktivitäten in einer Pipeline verwenden.  

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Vorbereiten des Datenspeichers zum Speichern des Grenzwerts.   
> * Erstellen einer Data Factory.
> * Erstellen Sie verknüpfte Dienste. 
> * Erstellen des Quell-, Senken-, Grenzwert-Datasets.
> * Erstellen einer Pipeline.
> * Ausführen der Pipeline.
> * Überwachen der Pipelineausführung. 

## <a name="overview"></a>Übersicht
Das allgemeine Lösungsdiagramm: 

![Lädt Daten inkrementell](media\tutorial-Incrementally-load-data-from-azure-sql-to-blob\incrementally-load.png)

Hier sind die wesentlichen Schritte beim Erstellen dieser Lösung: 

1. **Select the watermark column (Wählen Sie die Grenzwert-Spalte aus)**.
    Wählen Sie eine Spalte im Quelldatenspeicher aus, die verwendet werden kann, um die neuen oder aktualisierten Datensätze für jede Ausführung in Segmente aufzuteilen. Normalerweise steigen die Daten in dieser ausgewählten Spalte (z.B. Last_modify_time oder ID), wenn Zeilen erstellt oder aktualisiert werden. Der maximale Wert in dieser Spalte wird als Grenzwert verwendet.
2. **Prepare a data store to store the watermark value (Vorbereiten eines Datenspeichers zum Speichern des Grenzwerts)**.   
    In diesem Tutorial speichern Sie den Grenzwert in einer Azure SQL-Datenbank.
3. **Create a pipeline with the following workflow (Erstellen Sie eine Pipeline mit dem folgenden Workflow):** 
    
    Die Pipeline in dieser Lösung enthält die folgenden Aktivitäten:
  
    1. Erstellen Sie zwei **Lookup**-Aktivitäten. Verwenden Sie die erste Lookup-Aktivität, um den letzten Grenzwert abzurufen. Verwenden Sie die zweite Lookup-Aktivität, um den neuen Grenzwert abzurufen. Diese Grenzwerte werden an die Kopieraktivität übergeben. 
    2. Erstellen Sie eine **Kopieraktivität**, die Zeilen aus dem Quelldatenspeicher kopiert, wobei der Wert der Grenzwert-Spalte größer als der alte Grenzwert und kleiner als der neue Grenzwert ist. Anschließend werden die Delta-Daten aus dem Quelldatenspeicher in einen Blob Storage als neue Datei kopiert. 
    3. Erstellen Sie eine **Gespeicherte Prozedur-Aktivität**, die den Grenzwert für die nächste Pipeline-Ausführung aktualisiert. 


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure SQL-Datenbank**. Sie verwenden die Datenbank als den **Quell**-Datenspeicher. Wenn Sie noch nicht über eine Azure SQL-Datenbank verfügen, finden Sie im Artikel [Erstellen einer Azure SQL-Datenbank](../sql-database/sql-database-get-started-portal.md) die Schritte zum Erstellen einer solchen Datenbank.
* **Azure Storage-Konto**. Sie verwenden den Blob Storage als den **Senken**-Datenspeicher. Wenn Sie kein Azure Storage-Konto haben, finden Sie im Artikel [Create a storage account (Erstellen eines Speicherkontos)](../storage/common/storage-create-storage-account.md#create-a-storage-account) eine Anleitung zum Erstellen eines Azure Storage-Kontos. Erstellen Sie einen Container mit dem Namen **Adftutorial**. 
* **Azure PowerShell**. Befolgen Sie die Anweisungen unter [Get started with Azure PowerShell cmdlets](/powershell/azure/install-azurerm-ps) (Erste Schritte mit Azure PowerShell-Cmdlets).

### <a name="create-a-data-source-table-in-your-azure-sql-database"></a>Erstellen Sie eine Datenquelletabelle in Ihrer Azure SQL-Datenbank
1. Öffnen Sie **SQL Server Management Studio** in **Server Explorer**, klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie die **Neue Abfrage** aus.
2. Führen Sie den folgenden SQL-Befehl für Ihre Azure SQL-Datenbank aus, um eine Tabelle mit dem Namen `data_source_table` als Quelldatenspeicher zu erstellen.  
    
    ```sql
    create table data_source_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );

    INSERT INTO data_source_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'aaaa','9/1/2017 12:56:00 AM'),
    (2, 'bbbb','9/2/2017 5:23:00 AM'),
    (3, 'cccc','9/3/2017 2:36:00 AM'),
    (4, 'dddd','9/4/2017 3:21:00 AM'),
    (5, 'eeee','9/5/2017 8:06:00 AM');
    ```
    In diesem Tutorial verwenden Sie **LastModifytime** als die **Grenzwert**-Spalte.  Die Daten im Quelldatenspeicher sind in der folgenden Tabelle dargestellt:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    ```

### <a name="create-another-table-in-sql-database-to-store-the-high-watermark-value"></a>Erstellen Sie eine andere Tabelle in SQL-Datenbank zum Speichern des hohen Grenzwerts.
1. Führen Sie den folgenden SQL-Befehl für Ihre Azure SQL-Datenbank aus, um eine Tabelle mit dem Namen `watermarktable`, zum Speichern des Grenzwerts zu erstellen.  
    
    ```sql
    create table watermarktable
    (
    
    TableName varchar(255),
    WatermarkValue datetime,
    );
    ```
3. Legen Sie den Standard-**Wert** für den hohen Grenzwert beim Tabellennamen des Quelldatenspeichers fest.  (In diesem Tutorial ist der Tabellenname: **Data_source_table**)

    ```sql
    INSERT INTO watermarktable
    VALUES ('data_source_table','1/1/2010 12:00:00 AM')    
    ```
4. Überprüfen Sie die Daten in Tabelle: `watermarktable`.
    
    ```sql
    Select * from watermarktable
    ```
    Ausgabe: 

    ```
    TableName  | WatermarkValue
    ----------  | --------------
    data_source_table | 2010-01-01 00:00:00.000
    ```

### <a name="create-a-stored-procedure-in-azure-sql-database"></a>Erstellen Sie eine gespeicherte Prozedur in Azure SQL-Datenbank 

Führen Sie den folgenden Befehl zum Erstellen einer gespeicherten Prozedur in Ihrer Azure SQL-Datenbank aus.

```sql
CREATE PROCEDURE sp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN
    
    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName
    
END
```

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
    Set-AzureRmDataFactoryV2 -ResourceGroupName "<your resource group to create the factory>" -Location "East US" -Name "<specify the name of data factory to create. It must be globally unique.>" 
    ```

    Beachten Sie folgende Punkte:

    * Der Name der Azure Data Factory muss global eindeutig sein. Wenn die folgende Fehlermeldung angezeigt wird, ändern Sie den Namen, und wiederholen Sie den Vorgang.

        ```
        The specified Data Factory name '<data factory name>' is already in use. Data Factory names must be globally unique.
        ```

    * Data Factory-Instanzen können nur von Mitwirkenden oder Administratoren des Azure-Abonnements erstellt werden.
    * Derzeit ermöglicht Data Factory V2 das Erstellen einer Data Factory nur in der Region „USA, Osten“. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.


## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Um Ihre Datenspeicher und Compute Services mit der Data Factory zu verknüpfen, können Sie verknüpfte Dienste in einer Data Factory erstellen. In diesem Abschnitt erstellen Sie verknüpfte Dienste mit Ihrem Azure Storage-Konto und mit Ihrer Azure SQL-Datenbank. 

### <a name="create-azure-storage-linked-service"></a>Erstellen Sie einen mit Azure Storage verknüpften Dienst.
1. Erstellen Sie eine JSON-Datei mit dem Namen **AzureStorageLinkedService.json** im Ordner **C:\ADF** und dem folgenden Inhalt: (Erstellen Sie den Ordner „ADF“, wenn dieser noch nicht vorhanden ist.). Ersetzen Sie `<accountName>` und `<accountKey>` durch den Namen bzw. Schlüssel Ihres Azure Storage-Kontos, bevor Sie die Datei speichern.

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
2. Wechseln Sie in **Azure PowerShell** zum Ordner **ADF**.
3. Führen Sie nun das Cmdlet **Set-AzureRmDataFactoryV2LinkedService** zum Erstellen des verknüpften Diensts: **AzureStorageLinkedService** aus. Im folgenden Beispiel, übergeben Sie Werte für die **ResourceGroupName**- und **DataFactoryName**-Parameter. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-azure-sql-database-linked-service"></a>Erstellen Sie einen Azure SQL-Datenbank -verknüpften Dienst.
1. Erstellen Sie eine JSON-Datei mit dem Namen **AzureSQLDatabaseLinkedService.json** im Ordner **C:\ADF** und dem folgenden Inhalt: (Erstellen Sie den Ordner „ADF“, wenn dieser noch nicht vorhanden ist.). Ersetzen Sie **&lt;Server&gt;- und &lt;userid&gt;-, und &lt;Kennwort&gt;**-Namen Ihres Azure SQL-Servers, Ihrer Benutzer-ID und Ihres Kennworts, bevor Sie die Datei speichern. 

    ```json
    {
        "name": "AzureSQLDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "value": "Server = tcp:<server>.database.windows.net,1433;Initial Catalog=<database name>; Persist Security Info=False; User ID=<user name> ; Password=<password>; MultipleActiveResultSets = False; Encrypt = True; TrustServerCertificate = False; Connection Timeout = 30;",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
2. Wechseln Sie in **Azure PowerShell** zum Ordner **ADF**.
3. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2LinkedService**, zum Erstellen des verknüpften Diensts: **AzureSQLDatabaseLinkedService** aus. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSQLDatabaseLinkedService" -File ".\AzureSQLDatabaseLinkedService.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    LinkedServiceName : AzureSQLDatabaseLinkedService
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDatabaseLinkedService
    ProvisioningState :
    ```

## <a name="create-datasets"></a>Erstellen von Datasets
In diesem Schritt erstellen Sie Datasets zur Darstellung von Quell- und Senkendaten. 

### <a name="create-a-source-dataset"></a>Erstellen eines Quelldatasets

1. Erstellen Sie eine JSON-Datei mit dem Namen „SourceDataset.json“ im selben Ordner und dem folgenden Inhalt: 

    ```json
    {
        "name": "SourceDataset",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "data_source_table"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }
   
    ```
    In diesem Tutorial wird der Tabellenname: **Data_source_table** verwendet. Ersetzen Sie ihn, wenn Sie eine Tabelle mit einem anderen Namen verwenden. 
2.  Führen Sie nun „Set-AzureRmDataFactoryV2Dataset cmdlet“ zum Erstellen des Datasets „SourceDataset“ aus.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SourceDataset" -File ".\SourceDataset.json"
    ```

    Hier ist die Beispielausgabe des Cmdlets:
    
    ```json
    DatasetName       : SourceDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset
    ```

### <a name="create-a-sink-dataset"></a>Erstellen Sie ein Senkendataset

1. Erstellen Sie eine JSON-Datei mit dem Namen „SinkDataset.json“ im selben Ordner und dem folgenden Inhalt: 

    ```json
    {
        "name": "SinkDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/incrementalcopy",
                "fileName": "@CONCAT('Incremental-', pipeline().RunId, '.txt')", 
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }   
    ```

    > [!IMPORTANT]
    > In diesem Ausschnitt wird davon ausgegangen, dass Sie einen BLOB-Container mit dem Namen **Adftutorial** in Ihrem Azure Blob Storage besitzen. Erstellen Sie den Container, wenn er noch nicht vorhanden ist (oder) geben Sie den Namen eines bereits vorhandenen ein. Der Ausgabeordner `incrementalcopy` wird automatisch erstellt, wenn er nicht bereits im Container vorhanden ist. In diesem Tutorial wird der Dateiname dynamisch mit dem Ausdruck: `@CONCAT('Incremental-', pipeline().RunId, '.txt')` generiert.
2.  Führen Sie nun „Set-AzureRmDataFactoryV2Datasetcmdlet“ zum Erstellen des Datasets „SinkDataset“ aus.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SinkDataset" -File ".\SinkDataset.json"
    ```

    Hier ist die Beispielausgabe des Cmdlets:
    
    ```json
    DatasetName       : SinkDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset    
    ```

## <a name="create-a-dataset-for-watermark"></a>Erstellen Sie ein Dataset für Grenzwerte
In diesem Schritt erstellen Sie ein Dataset zum Speichern eines hohen Grenzwerts. 

1. Erstellen Sie eine JSON-Datei mit dem Namen „WatermarkDataset.json“ im selben Ordner und dem folgenden Inhalt: 

    ```json
    {
        "name": " WatermarkDataset ",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "watermarktable"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }    
    ```
2.  Führen Sie nun das „Set-AzureRmDataFactoryV2Datasetcmdlet“ zum Erstellen des Datasets „WatermarkDataset“ aus.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "WatermarkDataset" -File ".\WatermarkDataset.json"
    ```

    Hier ist die Beispielausgabe des Cmdlets:
    
    ```json
    DatasetName       : WatermarkDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset    
    ```

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline
In diesem Tutorial erstellen Sie eine Pipeline mit zwei Lookup-Aktivitäten, einer Kopieraktivität und einer gespeicherte Prozedur-Akvivität, die in einer Pipeline verkettet sind. 


1. Erstellen Sie eine JSON-Datei: „IncrementalCopyPipeline.json“ im selben Ordner und dem folgenden Inhalt: 

    ```json
    {
        "name": "IncrementalCopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "LookupOldWaterMarkActivity",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from watermarktable"
                        },
    
                        "dataset": {
                        "referenceName": "WatermarkDataset",
                        "type": "DatasetReference"
                        }
                    }
                },
                {
                    "name": "LookupNewWaterMarkActivity",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "select MAX(LastModifytime) as NewWatermarkvalue from data_source_table"
                        },
    
                        "dataset": {
                        "referenceName": "SourceDataset",
                        "type": "DatasetReference"
                        }
                    }
                },
                
                {
                    "name": "IncrementalCopyActivity",
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "select * from data_source_table where LastModifytime > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and LastModifytime <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "dependsOn": [
                        {
                            "activity": "LookupNewWaterMarkActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        },
                        {
                            "activity": "LookupOldWaterMarkActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ],
    
                    "inputs": [
                        {
                            "referenceName": "SourceDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "SinkDataset",
                            "type": "DatasetReference"
                        }
                    ]
                },
    
                {
                    "name": "StoredProceduretoWriteWatermarkActivity",
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
    
                        "storedProcedureName": "sp_write_watermark",
                        "storedProcedureParameters": {
                            "LastModifiedtime": {"value": "@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}", "type": "datetime" },
                            "TableName":  { "value":"@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}", "type":"String"}
                        }
                    },
    
                    "linkedServiceName": {
                        "referenceName": "AzureSQLDatabaseLinkedService",
                        "type": "LinkedServiceReference"
                    },
    
                    "dependsOn": [
                        {
                            "activity": "IncrementalCopyActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ]
                }
            ]
            
        }
    }
    ```
    

2. Führen Sie das Cmdlet „Set-AzureRmDataFactoryV2Pipeline“ aus, um folgende Pipeline zu erstellen: IncrementalCopyPipeline.
    
   ```powershell
   Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "IncrementalCopyPipeline" -File ".\IncrementalCopyPipeline.json"
   ``` 

   Hier ist die Beispielausgabe: 

   ```json
    PipelineName      : IncrementalCopyPipeline
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Activities        : {LookupOldWaterMarkActivity, LookupNewWaterMarkActivity, IncrementalCopyActivity, StoredProceduretoWriteWatermarkActivity}
    Parameters        :
   ```
 
## <a name="run-the-pipeline"></a>Führen Sie die Pipeline aus.

1. Führen Sie die Pipeline: **IncrementalCopyPipeline** mit **Invoke-AzureRmDataFactoryV2Pipeline**-Cmdlet aus. Ersetzen Sie Platzhalter mit Ihrem eigenen Ressourcengruppen- und Data Factory-Namen.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup "<your resource group>" -dataFactoryName "<your data factory name>"
    ``` 
2. Überprüfen Sie den Status der Pipeline durch Ausführen des „Get-AzureRmDataFactoryV2ActivityRun“-Cmdlets, bis Sie sehen, dass alle Aktivitäten erfolgreich ausgeführt wurden. Ersetzen Sie Platzhalter durch Ihre eigene geeignete time für den Parameter „RunStartedAfter“, und „RunStartedBefore“.  In diesem Tutorial verwenden wir - RunStartedAfter "2017/09/14" - RunStartedBefore "2017/09/15"

    ```powershell
    Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $RunId -RunStartedAfter "<start time>" -RunStartedBefore "<end time>"
    ```

    Hier ist die Beispielausgabe:
 
    ```json
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupNewWaterMarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {NewWatermarkvalue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:42:42 AM
    ActivityRunEnd    : 9/14/2017 7:42:50 AM
    DurationInMs      : 7777
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupOldWaterMarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {TableName, WatermarkValue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:42:42 AM
    ActivityRunEnd    : 9/14/2017 7:43:07 AM
    DurationInMs      : 25437
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : IncrementalCopyActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, rowsCopied, copyDuration...}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:43:10 AM
    ActivityRunEnd    : 9/14/2017 7:43:29 AM
    DurationInMs      : 19769
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : StoredProceduretoWriteWatermarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {storedProcedureName, storedProcedureParameters}
    Output            : {}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:43:32 AM
    ActivityRunEnd    : 9/14/2017 7:43:47 AM
    DurationInMs      : 14467
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ```

## <a name="review-the-results"></a>Überprüfen der Ergebnisse

1. Im Azure Blob Storage (Senkenpeicher) sollten Sie sehen, dass die Daten in die im „SinkDataset“ definierte Datei kopiert wurden.  Im aktuellen Tutorial ist der Dateiname `Incremental- d4bf3ce2-5d60-43f3-9318-923155f61037.txt`.  Öffnen Sie die Datei, und Sie können Datensätze in der Datei sehen, die mit den Daten in Azure SQL-Datenbank identisch sind.

    ```
    1,aaaa,2017-09-01 00:56:00.0000000
    2,bbbb,2017-09-02 05:23:00.0000000
    3,cccc,2017-09-03 02:36:00.0000000
    4,dddd,2017-09-04 03:21:00.0000000
    5,eeee,2017-09-05 08:06:00.0000000
    ``` 
2. Überprüfen Sie den aktuellen Wert von `watermarktable`, und Sie sehen, dass der Grenzwert aktualisiert wurde.

    ```sql
    Select * from watermarktable
    ```
    
    Hier ist die Beispielausgabe:
 
    TableName | Grenzwert
    --------- | --------------
    Data_source_table   2017-09-05  8:06:00.000

### <a name="insert-data-into-data-source-store-to-verify-delta-data-loading"></a>Fügen Sie Daten in den Datenspeicher ein, um das Laden von Delta-Daten zu überprüfen

1. Fügen Sie neue Daten in Azure SQL-Datenbank (Datenquellenspeicher) ein:

    ```sql
    INSERT INTO data_source_table
    VALUES (6, 'newdata','9/6/2017 2:23:00 AM')
    
    INSERT INTO data_source_table
    VALUES (7, 'newdata','9/7/2017 9:01:00 AM')
    ``` 

    Die aktualisierten Daten in der Azure SQL-Datenbank sind wie folgt:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    6 | newdata | 2017-09-06 02:23:00.000
    7 | newdata | 2017-09-07 09:01:00.000
    ```
2. Führen Sie die Pipeline: **IncrementalCopyPipeline** mit **Invoke-AzureRmDataFactoryV2Pipeline**-Cmdlet aus. Ersetzen Sie Platzhalter mit Ihrem eigenen Ressourcengruppen- und Data Factory-Namen.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup "<your resource group>" -dataFactoryName "<your data factory name>"
    ```
3. Überprüfen Sie den Status der Pipeline durch Ausführen des **Get-AzureRmDataFactoryV2ActivityRun**-Cmdlets, bis Sie sehen, dass alle Aktivitäten erfolgreich ausgeführt wurden. Ersetzen Sie Platzhalter durch Ihre eigene geeignete time für den Parameter „RunStartedAfter“, und „RunStartedBefore“.  In diesem Tutorial verwenden wir - RunStartedAfter "2017/09/14" - RunStartedBefore "2017/09/15"

    ```powershell
    Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $RunId -RunStartedAfter "<start time>" -RunStartedBefore "<end time>"
    ```

    Hier ist die Beispielausgabe:
 
    ```json
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupNewWaterMarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {NewWatermarkvalue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:52:26 AM
    ActivityRunEnd    : 9/14/2017 8:52:58 AM
    DurationInMs      : 31758
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupOldWaterMarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {TableName, WatermarkValue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:52:26 AM
    ActivityRunEnd    : 9/14/2017 8:52:52 AM
    DurationInMs      : 25497
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : IncrementalCopyActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, rowsCopied, copyDuration...}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:53:00 AM
    ActivityRunEnd    : 9/14/2017 8:53:20 AM
    DurationInMs      : 20194
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : StoredProceduretoWriteWatermarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {storedProcedureName, storedProcedureParameters}
    Output            : {}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:53:23 AM
    ActivityRunEnd    : 9/14/2017 8:53:41 AM
    DurationInMs      : 18502
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ```
4.  Im Azure Blob Storage sollten Sie sehen, dass eine andere Datei im Azure Blob Storage erstellt wurde. In diesem Tutorial ist der neue Dateiname Name `Incremental-2fc90ab8-d42c-4583-aa64-755dba9925d7.txt`.  Öffnen Sie diese Datei, und Sie sehen, dass 2 Zeilen Datensätze enthalten sind:
5.  Überprüfen Sie den aktuellen Wert von `watermarktable`, und Sie können sehen, dass der Grenzwert erneut aktualisiert wurde.

    ```sql
    Select * from watermarktable
    ```
    Beispielausgabe: 
    
    TableName | Grenzwert
    --------- | ---------------
    „Data_source_table“ | 2017-09-07 09:01:00.000

     
## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie die folgenden Schritte ausgeführt: 

> [!div class="checklist"]
> * Definieren einer **Grenzwert**-Spalte und Speichern dieser in Azure SQL-Datenbank.  
> * Erstellen einer Data Factory.
> * Erstellen von verknüpften Diensten für SQL-Datenbank und Blob Storage. 
> * Erstellen von Quell- und Senkendatasets.
> * Erstellen einer Pipeline.
> * Ausführen der Pipeline.
> * Überwachen der Ausführung der Pipeline. 

Gehen Sie zum nächsten Tutorial, um mehr über das Transformieren von Daten mit einem Spark-Cluster in Azure zu erfahren:

> [!div class="nextstepaction"]
>[Transform data using Spark cluster in cloud (Transformieren von Daten mit Spark-Cluster in der Cloud)](tutorial-transform-data-spark-powershell.md).




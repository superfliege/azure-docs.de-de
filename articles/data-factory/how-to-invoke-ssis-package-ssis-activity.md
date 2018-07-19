---
title: Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ – Azure | Microsoft-Dokumentation
description: In diesem Artikel wird das Ausführen eines SSIS-Pakets (SQL Server Integration Services) in einer Azure Data Factory-Pipeline mithilfe der Aktivität „SSIS-Paket ausführen“ beschrieben.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/09/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: db5941528eedd10cf252607dbe2160bd498a70de
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951966"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory
In diesem Artikel wird das Ausführen eines SSIS-Pakets in einer Azure Data Factory-Pipeline mithilfe der Aktivität „SSIS-Paket ausführen“ beschrieben. 

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-sql-database"></a>Azure SQL-Datenbank 
Die exemplarische Vorgehensweise in diesem Artikel verwendet eine Azure SQL-Datenbank, in der der SSIS-Katalog gehostet ist. Alternativ können Sie eine verwaltete Instanz von Azure SQL (Vorschau) verwenden.

## <a name="create-an-azure-ssis-integration-runtime"></a>Erstellen einer Azure SSIS Integration Runtime
Erstellen Sie eine Azure-SSIS Integration Runtime, wenn Sie noch nicht über eine verfügen, indem Sie die Schritt-für-Schritt-Anweisung im [Tutorial: Bereitstellen von SSIS-Paketen](tutorial-create-azure-ssis-runtime-portal.md) befolgen.

## <a name="data-factory-ui-azure-portal"></a>Data Factory-Benutzeroberfläche (Azure-Portal)
In diesem Abschnitt erstellen Sie mithilfe der Data Factory-Benutzeroberfläche eine Data Factory-Pipeline mit einer Aktivität „SSIS-Paket ausführen“, die ein SSIS-Paket ausführt.

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory
Der erste Schritt besteht darin, eine Data Factory mit dem Azure-Portal zu erstellen. 

1. Starten Sie den Webbrowser **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.
2. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). 
3. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte für das Feld „Name“ der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
     ![Name nicht verfügbar – Fehler](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
4. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
      - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
      - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
    Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
4. Wählen Sie **V2** als **Version** aus.
5. Wählen Sie den **Standort** für die Data Factory aus. In der Dropdownliste werden nur Standorte angezeigt, die von Data Factory unterstützt werden. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich an anderen Standorten befinden.
6. Wählen Sie die Option **An Dashboard anheften** aus.     
7. Klicken Sie auf **Create**.
8. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Die Data Factory wird bereitgestellt**. 

    ![Kachel „Die Data Factory wird bereitgestellt“](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
    ![Data Factory-Startseite](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Anwendung für die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten. 

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Erstellen einer Pipeline mit einer Aktivität „SSIS-Paket ausführen“
In diesem Schritt erstellen Sie über die Data Factory-Benutzeroberfläche eine Pipeline. Sie fügen eine Aktivität „SSIS-Paket ausführen“ zur Pipeline hinzu und konfigurieren sie für die Ausführung des SSIS-Pakets. 

1. Klicken Sie auf der Seite „Erste Schritte“ auf **Pipeline erstellen**. 

    ![Seite für die ersten Schritte](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Allgemein**, und ziehen Sie die Aktivität **SSIS-Paket ausführen** auf die Oberfläche des Pipeline-Designers. 

   ![Ziehen und Ablegen der SSIS-Aktivität auf der Oberfläche des Designers](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. Geben Sie auf der Registerkarte **Allgemein** der Eigenschaften für die Aktivität „SSIS-Paket ausführen“ einen Namen und eine Beschreibung für die Aktivität an. Legen Sie ein optionales Timeout und Wiederholungswerte fest.

    ![Festlegen von Eigenschaften auf der Registerkarte „Allgemein“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. Wählen Sie auf der Registerkarte **Einstellungen** der Eigenschaften für die Aktivität „SSIS-Paket ausführen“ die Azure SSIS Integration Runtime aus, die der Datenbank `SSISDB`, in der das Paket bereitgestellt wird, zugeordnet ist. Geben Sie den Paketpfad in der Datenbank `SSISDB` im Format `<folder name>/<project name>/<package name>.dtsx` an. Geben Sie optional den 32-Bit-Ausführungsmodus und einen vordefinierten oder benutzerdefinierten Protokolliergrad sowie einen Umgebungspfad im Format `<folder name>/<environment name>` an.

    ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

5. Klicken Sie zum Überprüfen der Pipelinekonfiguration in der Symbolleiste auf **Überprüfen**. Klicken Sie zum Schließen des **Pipeline Validation Report** (Pipelineüberprüfungsbericht) auf **>>**.

6. Veröffentlichen Sie die Pipeline in der Data Factory, indem Sie auf die Schaltfläche **Alle veröffentlichen** klicken. 

### <a name="optionally-parameterize-the-activity"></a>Optionales Parametrisieren der Aktivität

Weisen Sie optional Werte, Ausdrücke oder Funktionen, die auf Data Factory-Systemvariablen verweisen können, Ihren Projekt- oder Paketparametern im JSON-Format zu. Dazu verwenden Sie die Schaltfläche **Quellcode anzeigen** am unteren Rand des Felds für die Aktivität „SSIS-Paket ausführen“ oder die Schaltfläche **Code** in der oberen rechten Ecke des Pipelinebereichs. Sie können beispielsweise Ihrem SSIS-Projekt Data Factory-Pipelineparameter zuweisen oder Paketparameter zuweisen, wie es in den folgenden Screenshots zu sehen ist:

![Bearbeiten eines JSON-Skripts für die Aktivität „SSIS-Paket ausführen“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-parameters.png)

![Hinzufügen von Parametern zur Aktivität „SSIS-Paket ausführen“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-parameters2.png)

### <a name="run-and-monitor-the-pipeline"></a>Ausführen und Überwachen der Pipeline
In diesem Abschnitt lösen Sie eine Pipelineausführung aus, und überwachen diese. 

1. Klicken Sie auf der Symbolleiste auf **Trigger** und anschließend auf **Jetzt auslösen**, um eine Pipelineausführung auszulösen. 

    ![Manuelles Auslösen](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. Wählen Sie im Fenster **Pipelineausführung** die Option **Fertig stellen** aus. 

3. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Es werden die Pipelineausführung, der zugehörige Status sowie weitere Informationen (z.B. Startzeit der Ausführung) angezeigt. Klicken Sie zum Aktualisieren der Ansicht auf **Aktualisieren**.

    ![Pipelineausführungen](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

4. Klicken Sie in der Spalte **Aktionen** auf den Link **Aktivitätsausführungen anzeigen**. Sie sehen, dass nur eine Aktivität ausgeführt wird, da die Pipeline nur eine Aktivität (die Aktivität „SSIS-Paket ausführen“) enthält.

    ![Aktivitätsausführungen](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

5. Sie können auf Ihrem Azure SQL-Server die folgende **Abfrage** für die SSISDB-Datenbank ausführen, um zu überprüfen, ob das Paket ausgeführt wurde. 

    ```sql
    select * from catalog.executions
    ```

    ![Überprüfen von Paketausführungen](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

6. Sie können auch die SSISDB-Ausführungs-ID aus der Ausgabe der Pipeline-Aktivitätsausführung abrufen und anhand der ID umfangreichere Ausführungsprotokolle und Fehlermeldungen in SSMS überprüfen.

    ![Abrufen der Ausführungs-ID](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

> [!NOTE]
> Sie können auch einen geplanten Trigger für die Pipeline erstellen, damit die Pipeline basierend auf einem Zeitplan (stündlich, täglich usw.) ausgeführt wird. Ein Beispiel finden Sie unter [Erstellen einer Data Factory über die Azure Data Factory-Benutzeroberfläche](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="azure-powershell"></a>Azure PowerShell
In diesem Abschnitt erstellen Sie mithilfe von Azure PowerShell eine Data Factory-Pipeline mit einer SSIS-Aktivität, die ein SSIS-Paket ausführt. 

Installieren Sie die aktuellen Azure PowerShell-Module, indem Sie die Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps) befolgen. 

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory
Sie können wahlweise die gleiche Data Factory verwenden, die die Azure-SSIS IR aufweist, oder eine separate Data Factory erstellen. Das folgende Verfahren beschreibt die Schritte zum Erstellen einer Data Factory. Sie erstellen in dieser Data Factory eine Pipeline mit einer SSIS-Aktivität. Die SSIS-Aktivität führt Ihr SSIS-Paket aus. 

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
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName `
                                            -Location $ResGrp.Location `
                                            -Name $dataFactoryName 
    ```

Beachten Sie folgende Punkte:

* Der Name der Azure Data Factory muss global eindeutig sein. Wenn die folgende Fehlermeldung angezeigt wird, ändern Sie den Namen, und wiederholen Sie den Vorgang.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Damit Sie Data Factory-Instanzen erstellen können, muss das Benutzerkonto, mit dem Sie sich bei Azure anmelden, ein Mitglied der Rolle **Mitwirkender** oder **Besitzer** oder ein **Administrator** des Azure-Abonnements sein.
* Eine Liste der Azure-Regionen, in denen Data Factory derzeit verfügbar ist, finden Sie, indem Sie die für Sie interessanten Regionen auf der folgenden Seite auswählen und dann **Analysen** erweitern, um **Data Factory** zu finden: [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/). Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.

### <a name="create-a-pipeline-with-an-ssis-activity"></a>Erstellen einer Pipeline mit einer SSIS-Aktivität 
In diesem Schritt erstellen Sie eine Pipeline mit einer SSIS-Aktivität. Die Aktivität führt Ihr SSIS-Paket aus. 

1. Erstellen Sie im Ordner **C:\ADF\RunSSISPackage** eine JSON-Datei mit dem Namen **RunSSISPackagePipeline.json** und einem Inhalt wie im folgenden Beispiel:

    > [!IMPORTANT]
    > Bevor Sie die Datei speichern, ersetzen Sie Objektnamen, Beschreibungen, Pfade, Eigenschafts- und Parameterwerte, Kennwörter und andere Variablenwerte. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [{
                "name": "mySSISActivity",
                "description": "My SSIS package/activity description",
                "type": "ExecuteSSISPackage",
                "typeProperties": {
                    "connectVia": {
                        "referenceName": "myAzureSSISIR",
                        "type": "IntegrationRuntimeReference"
                    },
                    "runtime": "x64",
                    "loggingLevel": "Basic",
                    "packageLocation": {
                        "packagePath": "FolderName/ProjectName/PackageName.dtsx"            
                    },
                    "environmentPath":   "FolderName/EnvironmentName",
                    "projectParameters": {
                        "project_param_1": {
                            "value": "123"
                        }
                    },
                    "packageParameters": {
                        "package_param_1": {
                            "value": "345"
                        }
                    },
                    "projectConnectionManagers": {
                        "MyAdonetCM": {
                            "userName": {
                                "value": "sa"
                            },
                            "passWord": {
                                "value": {
                                    "type": "SecureString",
                                    "value": "abc"
                                }
                            }
                        }
                    },
                    "packageConnectionManagers": {
                        "MyOledbCM": {
                            "userName": {
                                "value": "sa"
                            },
                            "passWord": {
                                "value": {
                                    "type": "SecureString",
                                    "value": "def"
                                }
                            }
                        }
                    },
                    "propertyOverrides": {
                        "\\PackageName.dtsx\\MaxConcurrentExecutables ": {
                            "value": 8,
                            "isSensitive": false
                        }
                    }
                },
                "policy": {
                    "timeout": "0.01:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30
                }
            }]
        }
    }
    ```

2.  Navigieren Sie in Azure PowerShell zum Ordner `C:\ADF\RunSSISPackage`.

3. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Pipeline** aus, um die Pipeline **RunSSISPackagePipeline** zu erstellen.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                   -ResourceGroupName $ResGrp.ResourceGroupName `
                                                   -Name "RunSSISPackagePipeline"
                                                   -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Hier ist die Beispielausgabe:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="create-a-pipeline-run"></a>Erstellen einer Pipelineausführung
Verwenden Sie das Cmdlet **Invoke-AzureRmDataFactoryV2Pipeline**, um die Pipeline auszuführen. Das Cmdlet gibt die ID der Pipelineausführung für die zukünftige Überwachung zurück.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

Führen Sie das folgende PowerShell-Skript aus, um den Status der Pipelineausführung kontinuierlich zu überwachen, bis das Kopieren der Daten beendet ist. Kopieren Sie das folgende Skript, fügen Sie es in das PowerShell-Fenster ein, und drücken Sie die EINGABETASTE. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

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

Sie können die Pipeline auch mithilfe des Azure-Portals überwachen. Schritt-für-Schritt-Anweisungen finden Sie unter [Überwachen der Pipeline](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="create-a-trigger"></a>Erstellen eines Triggers
Im vorherigen Schritt wurde die Pipeline auf Anforderung ausgeführt. Alternativ können Sie einen Zeitplantrigger erstellen, um die Pipeline nach Zeitplan (stündlich, täglich usw.) auszuführen.

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
3. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Trigger** aus, um den Trigger zu erstellen. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                    -DataFactoryName $DataFactory.DataFactoryName `
                                    -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Standardmäßig befindet sich der Trigger im beendeten Zustand. Starten Sie den Trigger, indem Sie das Cmdlet **Start-AzureRmDataFactoryV2Trigger** ausführen. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                      -DataFactoryName $DataFactory.DataFactoryName `
                                      -Name "MyTrigger" 
    ```
5. Bestätigen Sie, dass der Trigger sich im gestarteten Zustand befindet, indem Sie das Cmdlet **Get-AzureRmDataFactoryV2Trigger** ausführen. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                    -DataFactoryName $DataFactoryName `
                                    -Name "MyTrigger"     
    ```    
6. Führen Sie nach der nächsten vollen Stunde den folgenden Befehl aus. Wenn die aktuelle Uhrzeit z.B. 15:25 UTC ist, führen Sie den Befehl um 16:00 UTC aus. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                       -DataFactoryName $DataFactoryName `
                                       -TriggerName "MyTrigger" `
                                       -TriggerRunStartedAfter "2017-12-06" `
                                       -TriggerRunStartedBefore "2017-12-09"
    ```

    Sie können in Ihrem Azure SQL Server die folgende Abfrage für die SSISDB-Datenbank ausführen, um zu überprüfen, ob das Paket ausgeführt wurde. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>Nächste Schritte
Informationen finden Sie im folgenden Blogbeitrag:
-   [Modernisieren und Erweitern von ETL/ELT-Workflows mit SSIS-Aktivitäten in ADF-Pipelines](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)

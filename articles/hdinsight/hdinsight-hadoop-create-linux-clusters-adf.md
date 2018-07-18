---
title: 'Tutorial: Erstellen bedarfsgesteuerter Hadoop-Cluster in Azure HDInsight mit Data Factory | Microsoft Dokumentation'
description: Sie erfahren, wie Sie bedarfsgesteuerte Hadoop-Cluster in HDInsight mit Azure Data Factory erstellen.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: nitinme
ms.openlocfilehash: 9d54d3481176b36a0d13a9b8af2fad03349b81be
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229252"
---
# <a name="tutorial-create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Tutorial: Erstellen bedarfsgesteuerter Hadoop-Cluster in HDInsight mit Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

In diesem Artikel erfahren Sie, wie Sie bei Bedarf mit Azure Data Factory einen Hadoop-Cluster in Azure HDInsight erstellen. Sie können dann Datenpipelines in Azure Data Factory verwenden, um Hive-Aufträge ausführen und den Cluster zu löschen. Am Ende dieses Tutorials erfahren Sie, wie Sie die Ausführung eines Big Data-Auftrags operationalisieren, bei dem die Clustererstellung, Auftragsausführung und Clusterlöschung nach einem Zeitplan ausgeführt werden.

Dieses Tutorial enthält die folgenden Aufgaben: 

> [!div class="checklist"]
> * Erstellen eines Azure-Speicherkontos
> * Verstehen von Azure Data Factory-Aktivitäten
> * Erstellen einer Data Factory über das Azure-Portal
> * Erstellen von verknüpften Diensten
> * Erstellen einer Pipeline
> * Auslösen einer Pipeline
> * Überwachen einer Pipeline
> * Überprüfen der Ausgabe

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Azure PowerShell. Anweisungen hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

* Ein Azure Active Directory-Dienstprinzipal. Nachdem Sie den Dienstprinzipal erstellt haben, müssen Sie noch die **Anwendungs-ID** und den **Authentifizierungsschlüssel** mithilfe der Anweisungen im verknüpften Artikel abrufen. Sie benötigen sie später in diesem Tutorial. Dieser Dienstprinzipal muss außerdem Mitglied der Rolle *Mitwirkender* in dem Abonnement oder der Ressourcengruppe sein, in dem bzw. der der Cluster erstellt wird. Anweisungen zum Abrufen der erforderlichen Werte und zum Zuweisen der richtigen Rollen finden Sie unter [Erstellen eines Azure Active Directory-Dienstprinzipals](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

In diesem Abschnitt erstellen Sie ein Speicherkonto, das als Standardspeicher für den bei Bedarf erstellten HDInsight-Cluster verwendet wird. Dieses Speicherkonto enthält auch das HiveQL-Beispielskript (**hivescript.hql**), mit dem Sie einen Hive-Beispielauftrag simulieren, der im Cluster ausgeführt wird.

In diesem Abschnitt wird ein Azure PowerShell-Skript verwendet, um das Speicherkonto zu erstellen und die erforderlichen Dateien im Speicherkonto zu kopieren. Das Azure PowerShell-Beispielskript in diesem Abschnitt führt die folgenden Aufgaben aus:

1. Führt die Anmeldung bei Azure durch
2. Erstellt eine Azure-Ressourcengruppe.
3. Erstellt ein Azure Storage-Konto.
4. Erstellt einen Blobcontainer im Speicherkonto
5. Kopiert das HiveQL-Beispielskript (**hivescript.hql**) in den Blobcontainer. Sie finden das Skript unter [https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). Das Beispielskript ist bereits in einem anderen öffentlichen Blobcontainer verfügbar. Das folgende PowerShell-Skript erstellt eine Kopie dieser Dateien in dem selbst erstellten Azure Storage-Konto.


**So erstellen Sie mithilfe von Azure PowerShell ein Speicherkonto und kopieren die Dateien**
> [!IMPORTANT]
> Geben Sie Namen für die Azure-Ressourcengruppe und das Azure-Speicherkonto an, die anhand des Skripts erstellt werden sollen.
> Notieren Sie den **Namen der Ressourcengruppe**, den **Namen des Speicherkontos** und den **Speicherkontoschlüssel**, die vom Skript ausgegeben werden. Sie benötigen diese Angaben im nächsten Abschnitt.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
Login-AzureRmAccount
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

**So überprüfen Sie die Erstellung des Speicherkontos**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im linken Bereich **Ressourcengruppen** aus.
3. Doppelklicken Sie auf den Namen der Ressourcengruppe, die Sie im PowerShell-Skript erstellt haben. Verwenden Sie den Filter, wenn zu viele Ressourcengruppen aufgeführt werden.
4. Auf der Kachel **Ressourcen** sollte eine Ressource aufgeführt sein, sofern Sie die Ressourcengruppe nicht für andere Projekte freigegeben haben. Diese Ressource ist das Speicherkonto mit dem Namen, den Sie zuvor angegeben haben. Wählen Sie den Speicherkontonamen aus.
5. Wählen Sie die Kachel **Blobs** aus.
6. Wählen Sie den Container **adfgetstarted** aus. Der Ordner **hivescripts** wird angezeigt.
7. Öffnen Sie den Ordner, und überprüfen Sie, ob er die Beispielskriptdatei **hivescript.hql** enthält.

## <a name="understand-the-azure-data-factory-activity"></a>Verstehen der Azure Data Factory-Aktivität

[Azure Data Factory](../data-factory/introduction.md) orchestriert und automatisiert das Verschieben und Transformieren von Daten. Azure Data Factory kann einen HDInsight Hadoop-Cluster Just-In-Time erstellen, um einen eingehenden Datenslice zu verarbeiten, und den Cluster löschen, wenn die Verarbeitung abgeschlossen ist. 

In Azure Data Factory kann eine Data Factory über mindestens eine Datenpipeline verfügen. Eine Datenpipeline verfügt über mindestens eine Aktivität. Es gibt zwei Arten von Aktivitäten:

* [Datenverschiebungsaktivitäten:](../data-factory/copy-activity-overview.md) Sie verwenden Datenverschiebungen zum Verschieben von Daten aus einem Quelldatenspeicher in einen Zieldatenspeicher.
* [Datentransformationsaktivitäten](../data-factory/transform-data.md). Sie können Datentransformationsaktivitäten verwenden, um Daten zu übertragen und zu verarbeiten. Die HDInsight-Hive-Aktivität ist eine der Transformationsaktivitäten, die von Data Factory unterstützt werden. In diesem Tutorial verwenden Sie die Hive-Transformation.

In diesem Artikel konfigurieren Sie die Hive-Aktivität, um einen HDInsight Hadoop-Cluster bedarfsgesteuert zu erstellen. Folgendes geschieht während der Ausführung der Aktivität zur Verarbeitung von Daten:

1. Ein HDInsight Hadoop-Cluster wird automatisch Just-In-Time zur Verarbeitung des Slice für Sie erstellt. 

2. Die Eingabedaten werden durch Ausführen des folgenden HiveQL-Skripts im Cluster verarbeitet. In diesem Tutorial führt das HiveQL-Skript, das der Hive-Aktivität zugeordnet ist, die folgenden Aktionen aus:

    * Es verwendet die vorhandene Tabelle (*hivesampletable*) zum Erstellen einer weiteren Tabelle (**HiveSampleOut**).
    * Es füllt die Tabelle **HiveSampleOut** mit bestimmten Spalten aus der ursprünglichen Tabelle *hivesampletable* auf.

3. Der HDInsight Hadoop-Cluster wird gelöscht, sobald die Verarbeitung abgeschlossen ist, und der Cluster befindet sich während der konfigurierten Zeitspanne im Leerlauf (TimeToLive-Einstellung). Wenn der nächste Datenslice zur Verarbeitung innerhalb dieser TimeToLive-Leerlaufzeit verfügbar ist, wird der gleiche Cluster wie für die Verarbeitung des Slice verwendet.  

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

2. Wählen Sie im Azure-Portal **Ressource erstellen** > **Daten + Analysen** > **Data Factory** aus.

    ![Azure Data Factory im Portal](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Azure Data Factory im Portal")

2. Geben Sie die Werte wie im folgenden Screenshot gezeigt ein, oder wählen Sie sie aus:

    ![Erstellen einer Azure Data Factory mit dem Azure-Portal](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "Erstellen einer Azure Data Factory mit dem Azure-Portal")

    Geben Sie folgende Werte ein bzw. wählen diese aus:
    
    |Eigenschaft  |BESCHREIBUNG  |
    |---------|---------|
    |**Name** |  Geben Sie einen Namen für die Data Factory ein. Dieser Name muss global eindeutig sein.|
    |**Abonnement**     |  Wählen Sie Ihr Azure-Abonnement. |
    |**Ressourcengruppe**     | Wählen Sie **Vorhandene verwenden** und dann die Ressourcengruppe aus, die Sie mit dem PowerShell-Skript erstellt haben. |
    |**Version**     | Wählen Sie **V2 (Vorschau)** aus. |
    |**Location**     | Der Standort wird automatisch auf die Region festgelegt, die Sie beim Erstellen der Ressourcengruppe zuvor angegeben haben. Für dieses Tutorial wird der Standort auf **USA, Osten 2** festgelegt. |
    

3. Wählen Sie **An Dashboard anheften** und dann **Erstellen** aus. Im Portal-Dashboard wird die neue Kachel **Bereitstellung wird übermittelt** angezeigt. Das Erstellen einer Data Factory kann zwischen 2 und 4 Minuten dauern.

    ![Vorlagenbereitstellungsstatus](./media/hdinsight-hadoop-create-linux-clusters-adf/deployment-progress-tile.png "Vorlagenbereitstellungsstatus") 
 
4. Nachdem die Data Factory erstellt wurde, wird im Portal die Übersicht für die Data Factory angezeigt.

    ![Azure Data Factory – Übersicht](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Azure Data Factory – Übersicht")

5. Wählen Sie **Erstellen und überwachen** aus, um das Azure Data Factory-Portal für das Erstellen und Überwachen zu starten.

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten

In diesem Abschnitt erstellen Sie zwei verknüpfte Dienste in Ihrer Data Factory.

- Einen **verknüpften Azure Storage-Dienst**, der ein Azure-Speicherkonto mit der Data Factory verknüpft. Dieser Speicher wird vom bedarfsgesteuerten HDInsight-Cluster verwendet. Außerdem enthält er das Hive-Skript, das im Cluster ausgeführt wird.
- Einen **bedarfsgesteuerten verknüpften HDInsight-Dienst**. Azure Data Factory erstellt automatisch einen HDInsight-Cluster und führt das Hive-Skript aus. Anschließend wird der HDInsight-Cluster gelöscht, nachdem er sich für einen vorkonfigurierten Zeitraum im Leerlauf befunden hat.

###  <a name="create-an-azure-storage-linked-service"></a>Erstellen eines verknüpften Azure Storage-Diensts

1. Wählen Sie im linken Bereich auf der Seite **Erste Schritte** das Symbol **Bearbeiten** aus.

    ![Erstellen eines verknüpften Azure Data Factory-Diensts](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "Erstellen eines verknüpften Azure Data Factory-Diensts")

2. Wählen Sie links unten im Fenster die Option **Verbindungen** und dann **+ Neu** aus.

    ![Erstellen von Verbindungen in Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "Erstellen von Verbindungen in Azure Data Factory")

3. Wählen Sie im Dialogfeld **Neuer verknüpfter Dienst** die Option **Azure Blob Storage** und dann **Weiter** aus.

    ![Erstellen eines mit Azure Storage verknüpften Diensts für Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "Erstellen eines mit Azure Storage verknüpften Diensts für Data Factory")

4. Geben Sie einen Namen für den mit Storage verknüpften Dienst an, und wählen Sie das Azure Storage-Konto aus, das Sie mit dem PowerShell-Skript erstellt haben. Wählen Sie anschließend **Fertig stellen** aus.

    ![Angeben eines Namens für den mit Azure Storage verknüpften Dienst](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "Angeben eines Namens für den mit Azure Storage verknüpften Dienst")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Erstellen eines bedarfsgesteuerten verknüpften HDInsight-Diensts

1. Wählen Sie erneut die Schaltfläche **+ Neu**, um einen weiteren verknüpften Dienst zu erstellen.

2. Wählen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die Option **Compute** > **Azure HDInsight** und dann **Weiter**.

    ![Erstellen eines mit HDInsight verknüpften Diensts für Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "Erstellen eines mit HDInsight verknüpften Diensts für Azure Data Factory")

3. Geben Sie im Fenster **Neuer verknüpfter Dienst** die erforderlichen Werte an.

    ![Angeben von Werten für einen mit HDInsight verknüpften Dienst](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "Angeben von Werten für einen mit HDInsight verknüpften Dienst")

    Geben Sie die folgenden Werte ein, und behalten Sie davon abgesehen die Standardwerte bei.

    | Eigenschaft | BESCHREIBUNG |
    | --- | --- |
    | NAME | Geben Sie einen Namen für den mit HDInsight verknüpften Dienst ein. |
    | Typ | Wählen Sie **HDInsight bedarfsgesteuert** aus. |
    | Mit Azure-Speicher verknüpfter Dienst | Wählen Sie den zuvor erstellten mit Storage verknüpften Dienst aus. |
    | Clustertyp | Wählen Sie **hadoop** aus. |
    | Gültigkeitsdauer | Geben Sie an, wie lange der HDInsight-Cluster verfügbar sein soll, bevor er automatisch gelöscht wird.|
    | Dienstprinzipal-ID | Geben Sie die Anwendungs-ID des Azure Active Directory-Dienstprinzipals an, den Sie zur Vorbereitung erstellt haben. |
    | Dienstprinzipalschlüssel | Geben Sie den Authentifizierungsschlüssel für den Azure Active Directory-Dienstprinzipal an. |
    | Clusternamenspräfix | Geben Sie einen Wert an, der allen von der Data Factory erstellten Clustertypen vorangestellt wird. |
    | Ressourcengruppe | Wählen Sie die Ressourcengruppe, die Sie zuvor mit dem PowerShell-Skript erstellt haben, aus.| 
    | SSH-Benutzername für den Cluster | Geben Sie einen SSH-Benutzernamen ein. |
    | SSH-Kennwort für den Cluster | Geben Sie ein Kennwort für den SSH-Benutzer an. |

    Wählen Sie **Fertig stellen** aus.

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

1. Klicken Sie auf die Schaltfläche **+** (Plus) und dann auf **Pipeline**.

    ![Erstellen einer Pipeline in Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "Erstellen einer Pipeline in Azure Data Factory")

2. Erweitern Sie in der Toolbox **Aktivitäten** die Option **HDInsight**, und ziehen Sie die Aktivität **Hive** auf die Oberfläche des Pipeline-Designers. Geben Sie auf der Registerkarte **Allgemein** einen Namen für die Aktivität an.

    ![Hinzufügen von Aktivitäten zur Data Factory-Pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "Hinzufügen von Aktivitäten zur Data Factory-Pipeline")

3. Vergewissern Sie sich, dass die Hive-Aktivität ausgewählt ist, und wählen Sie die Registerkarte **HDI-Cluster** und dann in der Dropdownliste **Verknüpfter HDInsight-Dienst** den verknüpften Dienst, den Sie zuvor für HDInsight erstellt haben, aus.

    ![Angeben von Details zum HDInsight-Cluster für die Pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "Angeben von Details zum HDInsight-Cluster für die Pipeline")

4. Wählen Sie die Registerkarte **Skript** aus, und führen Sie die folgenden Schritte aus:

    a. Wählen Sie unter **Mit dem Skript verknüpfter Dienst** die Option **HDIStorageLinkedService** aus. Dieser Wert ist der zuvor erstellte mit Storage verknüpfte Dienst.

    b. Wählen Sie für **Dateipfad** die Option **Storage durchsuchen** aus, und navigieren Sie zum Speicherort des Hive-Beispielskripts. Wenn Sie zuvor das PowerShell-Skript ausgeführt haben, sollte dieser Speicherort `adfgetstarted/hivescripts/hivescript.hql` lauten.

    ![Angeben von Details zum Hive-Skript für die Pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "Angeben von Details zum Hive-Skript für die Pipeline")

    c. Wählen Sie unter **Erweitert** > **Parameter** die Option **AutoAusfüllen aus Skript** aus. Diese Option sucht alle Parameter im Hive-Skript, die zur Laufzeit Werte erfordern. Das von Ihnen verwendete Skript (**hivescript.hql**) verfügt über den Parameter **Output**. Geben Sie den Wert im Format `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` an, sodass er auf einen vorhandenen Ordner in Ihrem Azure Storage zeigt. Beim Pfad wird die Groß-/Kleinschreibung beachtet. Dies ist der Pfad, in dem die Ausgabe des Skripts gespeichert wird.

    ![Angeben von Parametern für das Hive-Skript](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "Angeben von Parametern für das Hive-Skript")

5. Wählen Sie **Überprüfen** aus, um die Pipeline zu überprüfen. Wählen Sie die Schaltfläche **>>** (Pfeil nach rechts), um das Überprüfungsfenster zu schließen.

    ![Überprüfen der Azure Data Factory-Pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "Überprüfen der Azure Data Factory-Pipeline")

5. Wählen Sie abschließend **Alle veröffentlichen** aus, um die Artefakte in Azure Data Factory zu veröffentlichen.

    ![Veröffentlichen der Azure Data Factory-Pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "Veröffentlichen der Azure Data Factory-Pipeline")

## <a name="trigger-a-pipeline"></a>Auslösen einer Pipeline

1. Wählen Sie auf der Designeroberfläche auf der Symbolleiste **Auslöser** > **Jetzt auslösen** aus.

    ![Auslösen der Azure Data Factory-Pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "Auslösen der Azure Data Factory-Pipeline")

2. Wählen Sie auf der eingeblendeten Seitenleiste **Fertig stellen** aus.

## <a name="monitor-a-pipeline"></a>Überwachen einer Pipeline

1. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. In der Liste mit den **Pipelineausführungen** wird eine Pipelineausführung angezeigt. Beachten Sie den Status der Ausführung in der Spalte **Status**.

    ![Überwachen der Azure Data Factory-Pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "Überwachen der Azure Data Factory-Pipeline")

2. Wählen Sie zum Aktualisieren des Status die Option **Aktualisieren**.

3. Sie können auch das Symbol **View Activity Runs** (Aktivitätsausführungen anzeigen) auswählen, um die mit der Pipelineausführung verknüpfte Aktivitätsausführung anzuzeigen. Da die von Ihnen erstellte Pipeline nur eine einzelne Aktivität enthält, wird im Screenshot nur eine Aktivitätsausführung angezeigt. Um wieder in die vorherige Ansicht zu wechseln, wählen Sie oben auf der Seite **Pipelines** aus.

    ![Überwachen der Azure Data Factory-Pipelineaktivität](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "Überwachen der Azure Data Factory-Pipelineaktivität")


## <a name="verify-the-output"></a>Überprüfen der Ausgabe

1. Um die Ausgabe zu überprüfen, navigieren Sie im Azure-Portal zu dem Speicherkonto, das Sie für dieses Tutorial verwendet haben. Die folgenden Ordner und Container sollten angezeigt werden:

    - Sie sehen, dass **adfgerstarted/outputfolder** die Ausgabe des Hive-Skripts enthält, das als Teil der Pipeline ausgeführt wurde.

    - Sie sehen den Container **adfhdidatafactory-\<Name-des-verknüpften-Diensts>-\<Zeitstempel>**. Dieser Container ist der Standardspeicherort des HDInsight-Clusters, der als Teil der Pipelineausführung erstellt wurde.

    - Der Container **adfjobs** enthält die Protokolle des Azure Data Factory-Auftrags.  

        ![Überprüfen der Azure Data Factory-Pipelineausgabe](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "Überprüfen der Azure Data Factory-Pipelineausgabe")


## <a name="clean-up-the-tutorial"></a>Bereinigen des Tutorials

Bei einer bedarfsgesteuerten HDInsight-Clustererstellung müssen Sie den HDInsight-Cluster nicht explizit löschen. Der Cluster wird gemäß der Konfiguration, die Sie beim Erstellen der Pipeline bereitgestellt haben, gelöscht. Auch nachdem der Cluster gelöscht wurde, bleiben die mit dem Cluster verbundenen Speicherkonten jedoch vorhanden. Dieses Verhalten ist beabsichtigt, damit Ihre Daten intakt bleiben. Wenn Sie die Daten nicht beibehalten möchten, können Sie das erstellte Speicherkonto aber löschen.

Alternativ können Sie die gesamte Ressourcengruppe löschen, die Sie für dieses Tutorial erstellt haben. Dadurch werden das Speicherkonto und die Azure Data Factory, die Sie erstellt haben, gelöscht.

### <a name="delete-the-resource-group"></a>Löschen der Ressourcengruppe

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im linken Bereich **Ressourcengruppen** aus.
3. Wählen Sie den Namen der Ressourcengruppe, die Sie im PowerShell-Skript erstellt haben, aus. Verwenden Sie den Filter, wenn zu viele Ressourcengruppen aufgeführt werden. Daraufhin wird die Ressourcengruppe geöffnet.
4. Auf der Kachel **Ressourcen** sollten das Standardspeicherkonto und die Data Factory aufgeführt sein, wenn Sie die Ressourcengruppe nicht für andere Projekte freigegeben haben.
5. Wählen Sie die Option **Ressourcengruppe löschen**. Auf diese Weise werden das Speicherkonto und die Daten im Speicherkonto gelöscht.

    ![Löschen der Ressourcengruppe](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "Löschen der Ressourcengruppe")

6. Geben Sie den Namen der Ressourcengruppe ein, um den Löschvorgang zu bestätigen, und wählen Sie dann **Löschen** aus.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie mit Azure Data Factory einen bedarfsgesteuerten HDInsight-Cluster erstellen und Hive-Aufträge ausführen. Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie HDInsight-Cluster mit einer benutzerdefinierten Konfiguration erstellen.

> [!div class="nextstepaction"]
>[Erstellen von Azure HDInsight-Clustern mit einer benutzerdefinierten Konfiguration](hdinsight-hadoop-provision-linux-clusters.md)



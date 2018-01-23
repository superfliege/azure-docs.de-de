---
title: "Erstellen einer Azure Data Factory über die Azure Data Factory-Benutzeroberfläche | Microsoft-Dokumentation"
description: In diesem Tutorial wird gezeigt, wie Sie eine Data Factory mit einer Pipeline erstellen, die Daten aus einem Ordner in einen anderen Ordner in Azure Blob Storage kopiert.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: ebce4e0d137d2e56cc914efad357593af7abb255
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-data-factory-using-the-azure-data-factory-ui"></a>Erstellen einer Data Factory über die Azure Data Factory-Benutzeroberfläche
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: allgemein verfügbar](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 – Vorschauversion](quickstart-create-data-factory-portal.md)

Diese Schnellstartanleitung beschreibt, wie Sie mithilfe der Azure Data Factory-Benutzeroberfläche eine Data Factory erstellen und überwachen. Die in dieser Data Factory erstellte Pipeline **kopiert** Daten aus einem Ordner in einen anderen Ordner in einem Azure-Blobspeicher. Ein Tutorial zum **Transformieren** von Daten mithilfe von Azure Data Factory finden Sie im Tutorial [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](tutorial-transform-data-spark-portal.md). 


> [!NOTE]
> Wenn Sie mit Azure Data Factory nicht vertraut sind, lesen Sie vor der Durchführung dieses Schnellstarts die Informationen unter [Einführung in Azure Data Factory](data-factory-introduction.md). 
>
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 (GA) von Data Factory verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) weiter.

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Video 
Dieses Video enthält Informationen zur Data Factory-Benutzeroberfläche: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). 
2. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **ADFTutorialDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte der folgende Fehler für das Feld „Name“ auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
     ![Name nicht verfügbar – Fehler](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
4. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
      - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
      - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
    Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
4. Wählen Sie **V2 (Vorschau)** als **Version** aus.
5. Wählen Sie den **Standort** für die Data Factory aus. In der Dropdownliste werden nur Standorte angezeigt, die von Data Factory unterstützt werden. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich an anderen Standorten befinden.
6. Wählen Sie die Option **An Dashboard anheften** aus.     
7. Klicken Sie auf **Create**.
8. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Die Data Factory wird bereitgestellt**. 

    ![Kachel „Die Data Factory wird bereitgestellt“](media//quickstart-create-data-factory-portal/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in dieser Abbildung angezeigt:
   
    ![Data Factory-Startseite](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
10. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Anwendung für die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten. 
11. Wechseln Sie auf der Seite für die ersten Schritte am linken Rand zur Registerkarte **Bearbeiten** wie in der folgenden Abbildung gezeigt: 

    ![Seite für die ersten Schritte](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-azure-storage-linked-service"></a>Erstellen des mit Azure Storage verknüpften Diensts
In diesem Schritt erstellen Sie einen verknüpften Dienst, der Ihr Azure Storage-Konto mit der Data Factory verbindet. Der verknüpfte Dienste enthält die Verbindungsinformationen, die der Data Factory-Dienst zur Laufzeit zur Verbindungsherstellung verwendet.

2. Klicken Sie auf **Verbindungen** und dann auf der Symbolleiste auf die Schaltfläche **Neu**. 

    ![Neue Verbindung](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
3. Wählen Sie auf der Seite **Neuer verknüpfter Dienst** die Option **Azure Blob Storage**, und klicken Sie dann auf **Weiter**. 

    ![Auswählen von Azure Storage](./media/quickstart-create-data-factory-portal/select-azure-storage.png)
4. Führen Sie auf der Seite **Neuer verknüpfter Dienst** die folgenden Schritte aus: 

    1. Geben Sie unter **Name**  den Namen **AzureStorageLinkedService** ein.
    2. Geben Sie unter **Speicherkontoname** den Namen des Azure Storage-Kontos ein.
    3. Klicken Sie auf **Verbindung testen**, um zu überprüfen, ob der Data Factory-Dienst eine Verbindung mit dem Speicherkonto herstellen kann. 
    4. Klicken Sie auf **Speichern**, um den verknüpften Dienst zu speichern. 

        ![Einstellungen für den mit Azure Storage verknüpften Dienst](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 
5. Vergewissern Sie sich, dass **AzureStorageLinkedService** in der Liste der verknüpften Dienste angezeigt wird. 

    ![Mit Azure Storage verknüpfter Dienst in der Liste](./media/quickstart-create-data-factory-portal/azure-storage-linked-service-in-list.png)

## <a name="create-datasets"></a>Erstellen von Datasets
In diesem Schritt erstellen Sie zwei Datasets: **InputDataset** und **OutputDataset**. Diese Datasets sind vom Typ **AzureBlob**. Sie verweisen auf den **mit Azure Storage verknüpften Dienst**, den Sie im vorherigen Schritt erstellt haben. 

Das Eingabedataset stellt die Quelldaten im Eingabeordner dar. In der Definition des Eingabedatasets geben Sie den Blobcontainer (**adftutorial**), den Ordner (**input**) und die Datei (**emp.txt**) an, die die Quelldaten enthält. 

Das Ausgabedataset stellt die Daten dar, die zum Ziel kopiert werden. In der Definition des Ausgabedatasets geben Sie den Blobcontainer (**adftutorial**), den Ordner (**output**) und die Datei an, in die die Daten kopiert werden. Jeder Ausführung einer Pipeline ist eine eindeutige ID zugeordnet, auf die mit der Systemvariablen **RunId** zugegriffen werden kann. Der Name der Ausgabedatei wird basierend auf der Ausführungs-ID der Pipeline dynamisch ausgewertet.   

In den Einstellungen des verknüpften Diensts haben Sie das Azure-Speicherkonto angegeben, das die Quelldaten enthält. In den Einstellungen des Quelldatasets geben Sie an, wo genau sich die Quelldaten befinden (Blobcontainer, Order und Datei). In den Einstellungen des Senkendatasets geben Sie an, wohin die Daten kopiert werden (Blobcontainer, Order und Datei). 
 
1. Klicken Sie auf die Schaltfläche **+ (Plus)** und dann auf **Dataset**.

    ![Menü „Neues Dataset“](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
2. Wählen Sie auf der Seite **Neues Dataset** die Option **Azure Blob Storage**, und klicken Sie dann auf **Fertig stellen**. 

    ![Auswählen von Azure Blob Storage](./media/quickstart-create-data-factory-portal/select-azure-blob-storage.png)
3. Geben Sie im Fenster **Eigenschaften** für das Dataset als **Name** den Namen **InputDataset** ein. 

    ![Allgemeine Einstellungen des Datasets](./media/quickstart-create-data-factory-portal/dataset-general-page.png)
4. Wechseln Sie zur Registerkarte **Verbindung**, und führen Sie die folgenden Schritte aus: 

    1. Wählen Sie als verknüpften Dienst **AzureStorageLinkedService** aus. 
    2. Klicken Sie für **Dateipfad** auf die Schaltfläche **Durchsuchen**. 
        ![Suchen der Eingabedatei](./media/quickstart-create-data-factory-portal/file-path-browse-button.png)
    3. Navigieren Sie im Fenster **Choose a file or folder** (Datei oder Ordner auswählen) zum Ordner **input** im Container **adftutorial**, wählen Sie die Datei **emp.txt** aus, und klicken Sie auf **Fertig stellen**.

        ![Navigieren zur Eingabedatei](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    4. (optional) Klicken Sie auf **Datenvorschau**, um eine Vorschau der Daten in der Datei „emp.txt“ anzuzeigen.     
5. Wiederholen Sie die Schritte zum Erstellen des Ausgabedatasets.  

    1. Klicken Sie links auf die Schaltfläche **+ (Plus)** und dann auf **Dataset**.
    2. Wählen Sie auf der Seite **Neues Dataset** die Option **Azure Blob Storage**, und klicken Sie dann auf **Fertig stellen**.
    3. Geben Sie als Name **OutputDataset** an.
    4. Geben Sie für den Ordner **adftutorial/output** an. Die Copy-Aktivität erstellt den Ausgabeordner, falls er noch nicht vorhanden ist. 
    5. Geben Sie als Dateiname `@CONCAT(pipeline().RunId, '.txt')` ein. Bei jeder Ausführung einer Pipeline wird der Pipelineausführung eine eindeutige ID zugewiesen. Der Ausdruck verkettet die Ausführungs-ID der Pipeline mit **.txt**, um den Namen der Ausgabedatei zu bewerten. Eine Liste der unterstützten Systemvariablen und Ausdrücke finden Sie unter [Von Azure Data Factory unterstützte Systemvariablen](control-flow-system-variables.md) und [Ausdrücke und Funktionen in Azure Data Factory](control-flow-expression-language-functions.md).

        ![Einstellungen des Ausgabedatasets](./media/quickstart-create-data-factory-portal/output-dataset-settings.png)

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline 
In diesem Schritt erstellen und überprüfen Sie eine Pipeline mit einer **Copy**-Aktivität, die das Eingabe- und Ausgabedataset verwendet. Die Copy-Aktivität kopiert Daten aus der in den Einstellungen des Eingabedatasets angegebenen Datei in die Datei, die in den Einstellungen des Ausgabedatasets angegeben ist. Wenn das Eingabedataset nur einen Ordner (nicht den Dateinamen) angibt, kopiert die Copy-Aktivität alle Dateien im Quellordner ans Ziel. 

1. Klicken Sie auf die Schaltfläche **+ (Plus)** und dann auf **Pipeline**. 

    ![Menü „Neue Pipeline“](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
2. Geben Sie im Fenster **Eigenschaften** für **Name** den Namen **CopyPipeline** ein. 

    ![Allgemeine Einstellungen der Pipeline](./media/quickstart-create-data-factory-portal/pipeline-general-settings.png)
3. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Datenfluss**, und verschieben Sie die **Copy**-Aktivität per Drag&Drop aus der Toolbox **Aktivitäten** auf die Oberfläche des Pipeline-Designers. Sie können in der Toolbox **Aktivitäten** auch nach Aktivitäten suchen. Geben Sie unter **Name** den Namen **CopyFromBlobToBlob** ein.

    ![Allgemeine Einstellungen der Copy-Aktivität](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
4. Wechseln Sie in den Einstellungen der Copy-Aktivität zur Registerkarte **Quelle**, und wählen Sie für **Quelldataset** die Option **InputDataset** aus.

    ![Quelleinstellungen der Copy-Aktivität](./media/quickstart-create-data-factory-portal/copy-activity-source-settings.png)    
5. Wechseln Sie in den Einstellungen der Copy-Aktivität zur Registerkarte **Senke**, und wählen Sie für **Senkendataset** die Option **OutputDataset** aus.

    ![Senkeneinstellungen der Copy-Aktivität](./media/quickstart-create-data-factory-portal/copy-activity-sink-settings.png)    
7. Klicken Sie auf **Überprüfen**, um die Einstellungen der Pipeline zu überprüfen. Vergewissern Sie sich, dass die Pipeline überprüft wurde. Klicken Sie auf die Schaltfläche **>>** (Pfeil nach rechts), um die Ausgabe der Überprüfung zu schließen. 

    ![Überprüfen der Pipeline](./media/quickstart-create-data-factory-portal/pipeline-validate-button.png)

## <a name="test-run-the-pipeline"></a>Ausführen eines Testlaufs für die Pipeline
In diesem Schritt führen Sie einen Testlauf für die Pipeline aus, bevor Sie sie in der Data Factory bereitstellen. 

1. Klicken Sie auf der Symbolleiste für die Pipeline auf **Testlauf**. 
    
    ![Pipelinetestläufe](./media/quickstart-create-data-factory-portal/pipeline-test-run.png)
2. Überprüfen Sie, ob der Status der Pipelineausführung auf der Registerkarte **Ausgabe** der Pipelineeinstellungen angezeigt wird. 

    ![Ausgabe des Testlaufs](./media/quickstart-create-data-factory-portal/test-run-output.png)    
3. Vergewissern Sie sich, dass im Ordner **output** des Containers **adftutorial** eine Ausgabedatei angezeigt wird. Ist der Ausgabeordner nicht vorhanden, wird er vom Data Factory-Dienst automatisch erstellt. 
    
    ![Überprüfen der Ausgabe](./media/quickstart-create-data-factory-portal/verify-output.png)


## <a name="trigger-the-pipeline-manually"></a>Manuelles Auslösen der Pipeline
In diesem Schritt stellen Sie Entitäten (verknüpfte Dienste, Datasets, Pipelines) in Azure Data Factory bereit. Anschließend lösen Sie manuell eine Pipelineausführung aus. Sie können Entitäten auch in Ihrem eigenen VSTS-Git-Repository veröffentlichen. Die erforderlichen Schritte werden in einem [anderen Tutorial](tutorial-copy-data-portal.md?#configure-code-repository) behandelt.

1. Vor dem Auslösen einer Pipeline müssen Sie Entitäten in Data Factory veröffentlichen. Klicken Sie zum Veröffentlichen im linken Bereich auf **Veröffentlichen**. 

    ![Schaltfläche "Veröffentlichen"](./media/quickstart-create-data-factory-portal/publish-button.png)
2. Klicken Sie zum manuellen Auslösen der Pipeline auf der Symbolleiste auf **Trigger** und dann auf **Trigger Now** (Jetzt auslösen). 
    
    ![Jetzt auslösen](./media/quickstart-create-data-factory-portal/pipeline-trigger-now.png)

## <a name="monitor-the-pipeline"></a>Überwachen der Pipeline

1. Wechseln Sie zur Registerkarte **Überwachen** auf der linken Seite. Aktualisieren Sie die Liste über die Schaltfläche **Aktualisieren**.

    ![Überwachen der Pipeline](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
2. Klicken Sie unter **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen). Auf dieser Seite wird der Status der Ausführung der Copy-Aktivität angezeigt. 

    ![Aktivitätsausführungen der Pipeline](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
3. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf den Link **Details** (Brillensymbol). Einzelheiten zu den Eigenschaften finden Sie unter [Kopieraktivität in Azure Data Factory](copy-activity-overview.md). 

    ![Einzelheiten zum Kopiervorgang](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
4. Vergewissern Sie sich, dass im Ordner **output** eine neue Datei enthalten ist. 
5. Wenn Sie von der Ansicht **Aktivitätsausführungen** zurück zur Ansicht **Pipelineausführungen** wechseln möchten, klicken Sie auf den Link **Pipelines**. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Auslösen der Pipeline nach einem Zeitplan
Dieser Schritt ist in diesem Tutorial optional. Sie können einen **Planer-Trigger** erstellen, um eine regelmäßige Ausführung der Pipeline (stündlich, täglich usw.) festzulegen. In diesem Schritt erstellen Sie einen Trigger, der bis zur angegebenen Endzeit (Datum und Uhrzeit) minütlich ausgeführt wird. 

1. Wechseln Sie zur Registerkarte **Bearbeiten**. 

    ![Wechseln zur Registerkarte „Bearbeiten“](./media/quickstart-create-data-factory-portal/switch-edit-tab.png)
1. Klicken Sie im Menü auf **Trigger** und dann auf **Neu/Bearbeiten**. 

    ![Menü „Neuer Trigger“](./media/quickstart-create-data-factory-portal/new-trigger-menu.png)
2. Klicken Sie auf der Seite **Add Triggers** (Trigger hinzufügen) auf **Choose trigger...** (Trigger auswählen...) und dann auf **Neu**. 

    ![Hinzufügen von Triggern – neuer Trigger](./media/quickstart-create-data-factory-portal/add-trigger-new-button.png)
3. Aktivieren Sie auf der Seite **Neuer Trigger** unter **Ende** die Option **On Date** (Am), geben Sie eine Endzeit an, die einige Minuten nach der aktuellen Zeit liegt, und klicken Sie auf **Übernehmen**. Da für jede Pipelineausführung Gebühren anfallen, sollten zwischen Endzeit und Startzeit nur wenige Minuten liegen. Vergewissern Sie sich, dass der gleiche Tag festgelegt ist. Stellen Sie jedoch sicher, dass zwischen Veröffentlichungszeit und Endzeit ausreichend Zeit für die Pipelineausführung bleibt. Der Trigger wird erst wirksam, nachdem Sie die Lösung in Data Factory veröffentlicht haben, nicht beim Speichern des Triggers auf der Benutzeroberfläche. 

    ![Triggereinstellungen](./media/quickstart-create-data-factory-portal/trigger-settings.png)
4. Aktivieren Sie auf der Seite **Neuer Trigger** die Option **Aktiviert**, und klicken Sie dann auf **Weiter**. 

    ![Triggereinstellungen – Schaltfläche „Weiter“](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
5. Überprüfen Sie auf der Seite **Neuer Trigger** die Warnmeldung, und klicken Sie auf **Fertig stellen**.

    ![Triggereinstellungen – Schaltfläche „Fertig stellen“](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
6. Klicken Sie auf **Veröffentlichen**, um die Änderungen in Data Factory zu veröffentlichen. 

    ![Schaltfläche "Veröffentlichen"](./media/quickstart-create-data-factory-portal/publish-2.png)
8. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. Sie sehen, dass die Pipeline zwischen Veröffentlichungszeit und Endzeit minütlich ausgeführt wird. Beachten Sie die Werte in der Spalte **Ausgelöst durch**. Die manuelle Triggerausführung stammt aus dem zuvor ausgeführten Schritt (**Trigger Now** (Jetzt auslösen)). 

    ![Überwachen ausgelöster Ausführungen](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
9. Klicken Sie neben **Pipelineausführungen** auf den Pfeil nach unten, um zur Ansicht **Triggerausführungen** zu wechseln. 

    ![Überwachen von Triggerausführungen](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
10. Vergewissern Sie sich, dass für jede Pipelineausführung bis zur angegebenen Endzeit eine **Ausgabedatei** im Ordner **output** erstellt wird. 

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-portal.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 
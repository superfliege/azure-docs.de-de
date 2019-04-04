---
title: Verwenden von Azure Data Factory zum inkrementellen Kopieren neuer und geänderter Dateien nur auf Basis von LastModifiedDate | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory, und verwenden Sie dann das Tool zum Kopieren von Daten, um neue Dateien nur auf Basis von LastModifiedDate inkrementell zu laden.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 1/24/2019
ms.openlocfilehash: e5e184e6dadfa993e0b758ad32b54ea6ccfcb272
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57546533"
---
# <a name="incrementally-copy-new-and-changed-files-based-on-lastmodifieddate-by-using-the-copy-data-tool"></a>Inkrementelles Kopieren neuer und geänderter Dateien auf Basis von LastModifiedDate und mithilfe des Tools zum Kopieren von Daten

In diesem Tutorial verwenden Sie das Azure-Portal, um eine Data Factory zu erstellen. Anschließend verwenden Sie das Tool zum Kopieren von Daten, um eine Pipeline zu erstellen, die neue und geänderte Dateien nur auf Basis von LastModifiedDate inkrementell von einem in den anderen Azure-Blobspeicher kopiert. 

> [!NOTE]
> Falls Sie noch nicht mit Azure Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](introduction.md) zu lesen.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten
> * Überwachen der Pipeline- und Aktivitätsausführungen.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure-Speicherkonto**: Verwenden Sie den Blobspeicher als _Quelldatenspeicher_ und als _Senkendatenspeicher_. Falls Sie noch nicht über ein Azure-Speicherkonto verfügen, finden Sie Anweisungen dazu unter [Erstellen eines Speicherkontos](../storage/common/storage-quickstart-create-account.md).

### <a name="create-two-containers-in-blob-storage"></a>Erstellen von zwei Containern im Blobspeicher

Bereiten Sie Ihren Blobspeicher folgendermaßen auf das Tutorial vor:

1. Erstellen Sie einen Container mit dem Namen **source** (Quelle). Sie können für diese Aufgaben verschiedene Tools verwenden, etwa [Azure Storage-Explorer](https://storageexplorer.com/).

2. Erstellen Sie einen Container mit dem Namen **destination** (Ziel). Sie können für diese Aufgaben verschiedene Tools verwenden, etwa [Azure Storage-Explorer](https://storageexplorer.com/).

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im linken Menü auf **+ Neu** > **Daten + Analysen** > **Data Factory**: 
   
   ![Erstellen einer neuen Data Factory](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 
      
     ![Neue Data Factory](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   Der Name der Data Factory muss _global eindeutig_ sein. Sie erhalten unter Umständen die folgende Fehlermeldung:
   
   ![Fehlermeldung zu neuer Data Factory](./media/tutorial-copy-data-tool/name-not-available-error.png)

   Wenn eine Fehlermeldung zum Namenswert angezeigt wird, geben Sie einen anderen Namen für die Data Factory ein. Verwenden Sie beispielsweise den Namen _**IhrName**_**ADFTutorialDataFactory**. Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Data Factory – Benennungsregeln](naming-rules.md).
3. Wählen Sie das **Azure-Abonnement** aus, in dem die neue Data Factory erstellt werden soll. 
4. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
     
    a. Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe.

    b. Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein. 
         
    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).

5. Wählen Sie unter **Version** die Option **V2**.
6. Wählen Sie unter **Standort** den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die Datenspeicher (etwa Azure Storage und SQL-Datenbank) und Computeeinheiten (etwa Azure HDInsight), die von der Data Factory genutzt werden, können sich an anderen Standorten und in anderen Regionen befinden.
7. Wählen Sie die Option **An Dashboard anheften** aus. 
8. Klicken Sie auf **Erstellen**.
9. Auf dem Dashboard wird auf der Kachel **Deploying Data Factory** (Data Factory wird bereitgestellt...) der Prozessstatus angezeigt.

    ![Kachel „Deploying Data Factory“ (Data Factory wird bereitgestellt...)](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Nach Abschluss der Erstellung wird die Startseite von **Data Factory** angezeigt.
   
    ![Data Factory-Startseite](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten

1. Klicken Sie auf der Seite **Let's get started** (Erste Schritte) auf **Copy Data** (Daten kopieren), um das Tool zum Kopieren von Daten zu starten. 

   ![Kachel für das Tool zum Kopieren von Daten](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
   
2. Gehen Sie auf der Seite **Properties** (Eigenschaften) wie folgt vor:

    a. Geben Sie unter **Task name** (Aufgabenname) **DeltaCopyFromBlobPipeline** ein.

    b. Wählen Sie unter **Task cadence or Task schedule** (Aufgabenhäufigkeit oder Aufgabenzeitplan) die Option **Run regularly on schedule** (Nach Zeitplan regelmäßig ausführen) aus.

    c. Wählen Sie unter **Trigger type** (Triggertyp) die Option **Tumbling Window** (Rollierendes Fenster) aus.
    
    d. Geben Sie unter **Recurrence** (Wiederholung) **15 Minute(s)** (15 Minuten) ein. 
    
    e. Klicken Sie auf **Weiter**. 
    
    Über die Data Factory-Benutzeroberfläche wird eine Pipeline mit dem angegebenen Tasknamen erstellt. 

    ![Eigenschaftenseite](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/copy-data-tool-properties-page.png)
    
3. Führen Sie auf der Seite **Quelldatenspeicher** die folgenden Schritte aus:

    a. Klicken Sie auf **+ Create new connection** (+ Neue Verbindung erstellen), um eine Verbindung hinzuzufügen.
    
    ![Seite „Quelldatenspeicher“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page.png)

    b. Wählen Sie im Katalog **Azure Blob Storage** aus, und klicken Sie dann auf **Continue** (Weiter).
    
    ![Seite „Quelldatenspeicher“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-blob.png)

    c. Wählen Sie auf der Seite **New Linked Service** (Neuer verknüpfter Dienst) in der Liste **Storage account name** (Speicherkontoname) Ihr Speicherkonto aus, und klicken Sie dann auf **Finish** (Fertig stellen).
    
    ![Seite „Quelldatenspeicher“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-linkedservice.png)
    
    d. Wählen Sie den neu erstellten verknüpften Dienst aus, und klicken Sie dann auf **Next** (Weiter). 
    
   ![Seite „Quelldatenspeicher“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-linkedservice.png)

4. Führen Sie auf der Seite **Choose the input file or folder** (Eingabedatei oder -ordner auswählen) die folgenden Schritte aus:
    
    a. Suchen Sie nach dem Ordner **source** (Quelle), wählen Sie diesen aus, und klicken Sie anschließend auf **Choose** (Auswählen).
    
    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-input-file-folder.png)
    
    b. Wählen Sie unter **File loading behavior** (Dateiladeverhalten) die Option **Incremental load: LastModifiedDate** (Inkrementelles Laden: LastModifiedDate) aus.
    
    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-loading-behavior.png)
    
    c. Aktivieren Sie das Kontrollkästchen bei **Binary copy** (Binärkopie), und klicken Sie auf **Next** (Weiter).
    
     ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/check-binary-copy.png)
     
5. Wählen Sie auf der Seite **Destination data store** (Zieldatenspeicher) **AzureBlobStorage** aus, also das gleiche Speicherkonto wie beim Datenquellenspeicher, und klicken Sie anschließend auf **Next** (Weiter).

    ![Seite „Zieldatenspeicher“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/destination-data-store-page-select-linkedservice.png)
    
6. Führen Sie auf der Seite **Choose the output file or folder** (Ausgabedatei oder -ordner auswählen) die folgenden Schritte aus:
    
    a. Suchen Sie nach dem Ordner **destination** (Ziel), wählen Sie diesen aus, und klicken Sie anschließend auf **Choose** (Auswählen).
    
    ![Auswählen der Ausgabedatei oder des Ausgabeordners](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-output-file-folder.png)
    
    b. Klicken Sie auf **Weiter**.
    
     ![Auswählen der Ausgabedatei oder des Ausgabeordners](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/click-next-after-output-folder.png)
    
7. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**. 

    ![Seite "Einstellungen"](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/settings-page.png)
    
8. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie anschließend auf **Weiter**.

    ![Seite „Zusammenfassung“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/summary-page.png)
    
9. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline (Task) zu überwachen.

    ![Bereitstellungsseite](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/deployment-page.png)
    
10. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. In der Spalte **Aktionen** werden Links zum Anzeigen von Aktivitätsausführungsdetails und zum erneuten Ausführen der Pipeline angezeigt. Wählen Sie **Aktualisieren** aus, um die Liste zu aktualisieren, und klicken Sie auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen) in der Spalte **Actions** (Aktionen). 

    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs1.png)

11. Da die Pipeline nur eine Aktivität (Copy-Aktivität) enthält, wird nur ein Eintrag angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie auf den Link **Details** (Brillensymbol) in der Spalte **Aktionen**. 

    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs2.png)
    
    Da sich im Quellcontainer (**source**) in Ihrem Blob Storage-Konto keine Datei befindet, wird in Ihrem Blob Storage-Konto keine Datei angezeigt, die in den Zielcontainer (**destination**) kopiert wurde.
    
    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3.png)
    
12. Erstellen Sie eine leere Textdatei, und benennen Sie diese „file1.txt“. Laden Sie die Datei „file1.txt“ in den Quellcontainer (**source**) in Ihrem Speicherkonto hoch. Sie können für diese Aufgaben verschiedene Tools verwenden, etwa [Azure Storage-Explorer](https://storageexplorer.com/).   

    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3-1.png)
    
13. Wählen Sie **All Pipelines Runs** (Alle Pipelineausführungen) aus, und warten Sie, bis die gleiche Pipeline erneut automatisch ausgelöst wird, um zur Anzeige **Pipeline Runs** (Pipelineausführungen) zurückzugelangen.  

    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs4.png)

14. Wählen Sie für die zweite Pipelineausführung **View Activity Run** (Aktivitätsausführung anzeigen) aus, wenn Sie diese kommen sehen, und verfahren Sie ebenso, um Details anzuzeigen.  

    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs5.png)

    Sie werden bemerken, dass eine Datei („file1.txt“) aus dem Quellcontainer (**source**) in den Zielcontainer (**destination**) Ihres Speicherkontos kopiert wurde.
    
    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs6.png)
    
15. Erstellen Sie eine weitere leere Textdatei, und benennen Sie diese „file2.txt“. Laden Sie die Datei „file2.txt“ in den Quellcontainer (**source**) in Ihrem Speicherkonto hoch. Sie können für diese Aufgaben verschiedene Tools verwenden, etwa [Azure Storage-Explorer](https://storageexplorer.com/).  
    
16. Gehen Sie wie in Schritt 13 und 14 vor. Sie werden bemerken, dass nur die neue Datei („file2.txt“) bei der nächsten Pipelineausführung vom Quellcontainer (**source**) in den Zielcontainer (**destination**) Ihres Speicherkontos kopiert wurde.  
    
    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs7.png)

    Sie können dies auch bestätigen, indem Sie den Azure Storage-Explorer (https://storageexplorer.com/) verwenden, um die Dateien zu überprüfen.
    
    ![Überwachen der Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs8.png)

    
## <a name="next-steps"></a>Nächste Schritte
Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie mithilfe eines Spark-Clusters in Azure Daten transformieren:

> [!div class="nextstepaction"]
>[Transform data using Spark cluster in cloud (Transformieren von Daten mit Spark-Cluster in der Cloud)](tutorial-transform-data-spark-portal.md).
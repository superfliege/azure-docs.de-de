---
title: Kopieren von Daten mithilfe des Azure-Tools zum Kopieren von Daten | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory-Instanz, und verwenden Sie dann das Tool zum Kopieren von Daten, um Daten aus einem Ordner in einem Azure-Blobspeicher in einen anderen Ordner zu kopieren.
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
ms.openlocfilehash: c17e738e3db18503f7cdda24a5f01ceb78e4e6a3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="use-copy-data-tool-to-copy-data"></a>Kopieren von Daten mithilfe des Tools zum Kopieren von Daten 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: allgemein verfügbar](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 – Vorschauversion](quickstart-create-data-factory-copy-data-tool.md)

In dieser Schnellstartanleitung verwenden Sie das Azure-Portal, um eine Data Factory zu erstellen. Anschließend erstellen Sie mithilfe des Tools zum Kopieren von Daten eine Pipeline, die Daten aus einem Ordner in einem Azure-Blobspeicher in einen anderen Ordner kopiert. 

> [!NOTE]
> Wenn Sie mit Azure Data Factory nicht vertraut sind, lesen Sie vor der Durchführung dieses Schnellstarts die Informationen unter [Einführung in Azure Data Factory](data-factory-introduction.md). 
>
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **ADFTutorialDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte für das Feld „Name“ der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“), und wiederholen Sie den Erstellungsversuch. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
     ![Name nicht verfügbar – Fehler](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
4. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
      - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
      - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
      Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
4. Wählen Sie **V2 (Vorschau)** als **Version** aus.
5. Wählen Sie den **Standort** für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich an anderen Standorten bzw. in anderen Regionen befinden.
6. Wählen Sie die Option **An Dashboard anheften** aus.     
7. Klicken Sie auf **Create**.
8. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Die Data Factory wird bereitgestellt**. 

    ![Kachel „Die Data Factory wird bereitgestellt“](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
   ![Data Factory-Startseite](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)
10. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten. 

## <a name="launch-copy-data-tool"></a>Starten des Tools zum Kopieren von Daten

1. Klicken Sie auf der Seite mit den ersten Schritten auf die Kachel **Daten kopieren**, um das Tool zum Kopieren von Daten zu starten. 

   ![Kachel für das Tool zum Kopieren von Daten](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)
2. Klicken Sie auf der Seite **Eigenschaften** des Tools zum Kopieren von Daten auf **Weiter**. Auf dieser Seite können Sie einen Namen für die Pipeline und ihre Beschreibung angeben. 

    ![Tool zum Kopieren von Daten – Seite „Eigenschaften“](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. Wählen Sie auf der Seite **Quelldatenspeicher** die Option **Azure Blob Storage** aus, und klicken Sie auf **Weiter**.

    ![Seite „Quelldatenspeicher“](./media/quickstart-create-data-factory-copy-data-tool/source-data-store-page.png)
4. Wählen Sie auf der Seite **Azure Blob Storage-Konto angeben** in der Dropdownliste **Speicherkontoname** einen Namen aus. 

    ![Angeben des Blob Storage-Kontos](./media/quickstart-create-data-factory-copy-data-tool/specify-blob-storage-account.png)
5. Führen Sie auf der Seite **Choose the input file or folder** (Eingabedatei oder -ordner auswählen) die folgenden Schritte aus:

    1. Navigieren Sie zum Ordner **adftutorial/input**. 
    2. Wählen Sie die Datei **emp.txt** aus.
    3. Klicken Sie auf **Auswählen**. Doppelklicken Sie auf **emp.txt**, um diesen Schritt zu überspringen. 
    4. Klicken Sie auf **Weiter**.
 

    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/quickstart-create-data-factory-copy-data-tool/choose-input-file-folder.png)
6. Auf der Seite **Dateiformateinstellungen** können Sie sehen, dass das Tool die Spalten- und Zeilentrennzeichen automatisch erkennt. Klicken Sie auf **Weiter**. Auf dieser Seite können Sie außerdem eine Vorschau der Daten und das Schema der Eingabedaten anzeigen. 

    ![Seite mit den Dateiformateinstellungen](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-page.png)
7. Wählen Sie auf der Seite **Zieldatenspeicher** die Option **Azure Blob Storage** aus, und klicken Sie auf **Weiter**. 

    ![Seite „Zieldatenspeicher“](./media/quickstart-create-data-factory-copy-data-tool/destination-data-store-page.png)    
8. Wählen Sie auf der Seite **Azure Blob Storage-Konto angeben** Ihr Azure Blob Storage-Konto aus, und klicken Sie auf **Weiter**. 

    ![Seite zum Angeben des Senkendatenspeichers](./media/quickstart-create-data-factory-copy-data-tool/specify-sink-blob-storage-account.png)
9. Führen Sie auf der Seite **Choose the output file or folder** (Ausgabedatei oder -ordner auswählen) die folgenden Schritte aus: 

    1. Geben Sie als **folder path** den Pfad **adftutorial/output** an.
    2. Geben Sie ins Feld **Dateiname** den Namen **emp.txt** ein. 
    3. Klicken Sie auf **Weiter**.
 

    ![Auswählen der Ausgabedatei oder des Ausgabeordners](./media/quickstart-create-data-factory-copy-data-tool/choose-output-file-folder.png) 
10. Klicken Sie auf der Seite **File format settings** (Dateiformateinstellungen) auf **Weiter**. 

    ![Seite mit den Dateiformateinstellungen](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-output-page.png)
11. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**. 

    ![Seite „Erweiterte Einstellungen“](./media/quickstart-create-data-factory-copy-data-tool/advanced-settings-page.png)
12. Überprüfen Sie auf der Seite **Zusammenfassung** alle Einstellungen, und klicken Sie auf „Weiter“. 

    ![Seite „Zusammenfassung“](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)
13. Klicken Sie auf der Seite **Deployment complete** (Bereitstellung abgeschlossen) auf **Überwachen**, um die erstellte Pipeline zu überwachen. 

    ![Bereitstellungsseite](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)
14. Die Anwendung wechselt zur Registerkarte **Überwachen**. Der Status der Pipeline wird auf dieser Seite angezeigt. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. 
    
    ![Seite zum Überwachen der Pipelineausführungen](./media/quickstart-create-data-factory-copy-data-tool/monitor-pipeline-runs-page.png)
15. Klicken Sie in der Spalte „Aktionen“ auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen). Die Pipeline verfügt nur über eine Aktivität vom Typ **Copy**. 

    ![Seite mit den Aktivitätsausführungen](./media/quickstart-create-data-factory-copy-data-tool/activity-runs.png)
16. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf den Link **Details** (Brillensymbol). Einzelheiten zu den Eigenschaften finden Sie unter [Kopieraktivität in Azure Data Factory](copy-activity-overview.md). 

    ![Einzelheiten zum Kopiervorgang](./media/quickstart-create-data-factory-copy-data-tool/copy-operation-details.png)
17. Vergewissern Sie sich, dass im Ordner **output** des Containers **adftutorial** die Datei **emp.txt** erstellt wird. Ist der Ausgabeordner nicht vorhanden, wird er vom Data Factory-Dienst automatisch erstellt. 
18. Wechseln Sie zur Registerkarte **Bearbeiten**, um verknüpfte Dienste, Datasets und Pipelines zu bearbeiten. Informationen zum Bearbeiten dieser Elemente über die Data Factory-Benutzeroberfläche finden Sie unter [Erstellen einer Data Factory über das Azure-Portal](quickstart-create-data-factory-portal.md).

    ![Registerkarte „Bearbeiten“](./media/quickstart-create-data-factory-copy-data-tool/edit-tab.png)

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-portal.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 
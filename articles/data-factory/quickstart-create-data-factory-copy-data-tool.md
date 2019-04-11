---
title: Kopieren von Daten mithilfe des Azure-Tools zum Kopieren von Daten | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory-Instanz, und verwenden Sie dann das Tool zum Kopieren von Daten, um Daten von einem Speicherort in Azure Blob Storage an einen anderen Speicherort zu kopieren.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: yexu
ms.openlocfilehash: a77041b063f043c065df9a2955e9d07b5692f592
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59256259"
---
# <a name="quickstart-use-the-copy-data-tool-to-copy-data"></a>Schnellstart: Kopieren von Daten mithilfe des Tools zum Kopieren von Daten

> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Version 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Aktuelle Version](quickstart-create-data-factory-copy-data-tool.md)

In dieser Schnellstartanleitung verwenden Sie das Azure-Portal, um eine Data Factory zu erstellen. Anschließend erstellen Sie mithilfe des Tools zum Kopieren von Daten eine Pipeline, die Daten aus einem Ordner in Azure Blob Storage in einen anderen Ordner kopiert. 

> [!NOTE]
> Wenn Sie mit Azure Data Factory nicht vertraut sind, lesen Sie vor der Durchführung dieses Schnellstarts die Informationen unter [Einführung in Azure Data Factory](data-factory-introduction.md). 

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im linken Menü auf **Neu** und anschließend auf **Daten + Analysen** und **Data Factory**. 
   
   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
1. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 
      
   ![Seite „Neue Data Factory“](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss *global eindeutig*sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in **&lt;IhrName&gt;ADFTutorialDataFactory**), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
   ![Fehler, wenn ein Name nicht verfügbar ist](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
1. Wählen Sie unter **Abonnement** Ihr Azure-Abonnement aus, in dem die Data Factory erstellt werden soll. 
1. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
     
   - Wählen Sie die Option **Use existing** (Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
   - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
   Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
1. Wählen Sie **V2** als **Version** aus.
1. Wählen Sie unter **Standort** den Standort für die Data Factory aus. 

   In der Liste werden nur Standorte angezeigt, die von Data Factory unterstützt werden und an denen Ihre Azure Data Factory-Metadaten gespeichert werden. Beachten Sie hierbei, dass die von Data Factory verwendeten zugeordneten Datenspeicher (z.B. Azure Storage und Azure SQL-Datenbank) und Computedienste (z.B. Azure HDInsight) in anderen Regionen ausgeführt werden können.

1. Klicken Sie auf **Erstellen**.
1. Nach Abschluss der Erstellung wird die Seite **Data Factory** angezeigt. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Anwendung für die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten.
   
   ![Startseite der Data Factory mit der Kachel „Erstellen und überwachen“](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)

## <a name="start-the-copy-data-tool"></a>Starten des Tools zum Kopieren von Daten

1. Klicken Sie auf der Seite **Erste Schritte** auf die Kachel **Daten kopieren**, um das Tool zum Kopieren von Daten zu starten. 

   ![Kachel „Daten kopieren“](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)

1. Auf der Seite **Eigenschaften** des Tools zum Kopieren von Daten können Sie einen Namen für die Pipeline und ihre Beschreibung eingeben. Klicken Sie anschließend auf **Weiter**. 

   ![Eigenschaftenseite](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
1. Führen Sie auf der Seite **Quelldatenspeicher** die folgenden Schritte aus:

    a. Klicken Sie auf **+ Neue Verbindung erstellen**, um eine Verbindung hinzuzufügen.

    ![Seite „Quelldatenspeicher“](./media/quickstart-create-data-factory-copy-data-tool/new-source-linked-service.png)

    b. Wählen Sie im Katalog **Azure Blob Storage** aus, und klicken Sie dann auf **Weiter**.

    ![Auswählen von Blob Storage aus dem Katalog](./media/quickstart-create-data-factory-copy-data-tool/select-blob-source.png)

    c. Wählen Sie auf der Seite **Azure Blob Storage-Konto angeben** in der Dropdownliste **Speicherkontoname** Ihr Speicherkonto aus, und klicken Sie auf **Fertig stellen**. 

   ![Konfigurieren des Azure Blob Storage-Kontos](./media/quickstart-create-data-factory-copy-data-tool/configure-blob-storage.png)

   d. Wählen Sie den neu erstellten verknüpften Dienst als Quelle aus, und klicken Sie auf **Weiter**.

   ![Auswählen des verknüpften Quelldiensts](./media/quickstart-create-data-factory-copy-data-tool/select-source-linked-service.png)


1. Führen Sie auf der Seite **Choose the input file or folder** (Eingabedatei oder -ordner auswählen) die folgenden Schritte aus:

   a. Klicken Sie auf **Durchsuchen**, um zum Ordner **adftutorial/input** zu navigieren, wählen Sie die Datei **emp.txt** aus, und klicken Sie dann auf **Auswählen**. 

   ![Seite „Eingabedatei oder -ordner auswählen“](./media/quickstart-create-data-factory-copy-data-tool/configure-source-path.png)

   d. Aktivieren Sie die Option **Binary copy** (Binärkopie), um die Datei ohne Änderungen zu übernehmen, und klicken Sie dann auf **Weiter**. 

   ![Seite „Eingabedatei oder -ordner auswählen“](./media/quickstart-create-data-factory-copy-data-tool/select-binary-copy.png)


1. Wählen Sie auf der Seite **Zieldatenspeicher** den soeben erstellten verknüpften Dienst **Azure Blob Storage** aus, und klicken Sie dann auf **Weiter**. 

   ![Seite „Zieldatenspeicher“](./media/quickstart-create-data-factory-copy-data-tool/select-sink-linked-service.png)

1. Geben Sie auf der Seite **Choose the output file or folder** (Ausgabedatei oder -ordner auswählen) als Ordnerpfad den Pfad **adftutorial/output** ein, und klicken Sie auf **Weiter**. 

   ![Seite „Choose the output file or folder“ (Ausgabedatei oder -ordner auswählen)](./media/quickstart-create-data-factory-copy-data-tool/configure-sink-path.png) 

1. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**, um die Standardkonfigurationen zu verwenden. 

1. Überprüfen Sie auf der Seite **Zusammenfassung** alle Einstellungen, und klicken Sie auf **Weiter**. 

    ![Seite „Zusammenfassung“](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)

1. Klicken Sie auf der Seite **Deployment complete** (Bereitstellung abgeschlossen) auf **Überwachen**, um die erstellte Pipeline zu überwachen. 

    ![Seite „Deployment complete“ (Bereitstellung abgeschlossen)](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)

1. Die Anwendung wechselt zur Registerkarte **Überwachen**. Der Status der Pipeline wird auf dieser Registerkarte angezeigt. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. 
    
    ![Überwachen der Pipelineausführung](./media/quickstart-create-data-factory-copy-data-tool/pipeline-monitoring.png)

1. Klicken Sie in der Spalte **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen). Die Pipeline verfügt nur über eine Aktivität vom Typ **Copy**. 

    ![Überwachung der Aktivitätsausführung](./media/quickstart-create-data-factory-copy-data-tool/activity-monitoring.png)
    
1. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf den Link **Details** (Brillensymbol). Einzelheiten zu den Eigenschaften finden Sie unter [Kopieraktivität in Azure Data Factory](copy-activity-overview.md).

    ![Einzelheiten zum Kopiervorgang](./media/quickstart-create-data-factory-copy-data-tool/activity-execution-details.png)

1. Vergewissern Sie sich, dass im Ordner **output** des Containers **adftutorial** die Datei **emp.txt** erstellt wird. Ist der Ausgabeordner nicht vorhanden, wird er vom Data Factory-Dienst automatisch erstellt. 

1. Wechseln Sie im linken Bereich zur Registerkarte **Ersteller** über der Registerkarte **Überwachen**, um verknüpfte Dienste, Datasets und Pipelines zu bearbeiten. Informationen zum Bearbeiten dieser Elemente über die Data Factory-Benutzeroberfläche finden Sie unter [Erstellen einer Data Factory über die Azure Data Factory-Benutzeroberfläche](quickstart-create-data-factory-portal.md).

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-portal.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 
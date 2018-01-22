---
title: Kopieren von Daten aus SQL Server in Blob Storage mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe einer selbstgehosteten Integration Runtime in Azure Data Factory Daten aus einem lokalen Datenspeicher in die Azure-Cloud kopieren.
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
ms.date: 01/11/2018
ms.author: jingwang
ms.openlocfilehash: 7b734a76545dbcbddac3c7ad7beae60d662a9129
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="tutorial-copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Tutorial: Kopieren von Daten aus einer lokalen SQL Server-Datenbank nach Azure Blob Storage
In diesem Tutorial verwenden Sie die Benutzeroberfläche (User Interface, UI) von Azure Data Factory, um eine Data Factory-Pipeline zu erstellen, mit der Daten aus einer lokalen SQL Server-Datenbank nach Azure Blob Storage kopiert werden. Sie erstellen und verwenden eine selbstgehostete Integration Runtime, die Daten zwischen lokalen Speichern und Clouddatenspeichern verschiebt. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Azure Data Factory, die sich derzeit in der Vorschauphase befindet. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) weiter.
> 
> Dieser Artikel enthält keine ausführliche Einführung in den Data Factory-Dienst. Weitere Informationen finden Sie unter [Einführung in Azure Data Factory](introduction.md). 

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen Sie eine selbstgehostete Integration Runtime.
> * Erstellen von verknüpften SQL Server- und Azure Storage-Diensten 
> * Erstellen von SQL Server- und Azure Blob-Datasets
> * Erstellen einer Pipeline mit Kopieraktivität zum Verschieben der Daten
> * Starten einer Pipelineausführung
> * Überwachen der Pipelineausführung.

## <a name="prerequisites"></a>Voraussetzungen
### <a name="azure-subscription"></a>Azure-Abonnement
Wenn Sie nicht bereits ein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

### <a name="azure-roles"></a>Azure-Rollen
Damit Sie Data Factory-Instanzen erstellen können, muss dem Benutzerkonto, mit dem Sie sich bei Azure anmelden, die Rolle *Mitwirkender* oder *Besitzer* zugewiesen sein, oder es muss ein *Administrator* des Azure-Abonnements sein. 

Wählen Sie im Azure-Portal in der oberen rechten Ecke Ihren Benutzernamen und dann **Berechtigungen**, um Ihre Berechtigungen im Abonnement anzuzeigen. Wenn Sie Zugriff auf mehrere Abonnements besitzen, wählen Sie das entsprechende Abonnement aus. Beispielanweisungen zum Hinzufügen eines Benutzers zu einer Rolle finden Sie im Artikel [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 und 2017
In diesem Tutorial verwenden Sie eine lokale SQL Server-Datenbank als *Quelldatenspeicher*. Die Pipeline in der in diesem Tutorial erstellten Data Factory kopiert Daten aus dieser lokalen SQL Server-Datenbank (Quelle) in Azure Blob Storage (Senke). Anschließend erstellen Sie eine Tabelle mit dem Namen **emp** in Ihrer SQL Server-Datenbank und fügen einige Einträge in die Tabelle ein. 

1. Starten Sie SQL Server Management Studio. Falls die Installation auf Ihrem Computer noch nicht vorhanden ist, helfen Ihnen die Informationen unter [Herunterladen von SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) weiter. 

2. Stellen Sie eine Verbindung mit Ihrer SQL Server-Instanz her, indem Sie Ihre Anmeldeinformationen verwenden. 

3. Erstellen Sie eine Beispieldatenbank. Klicken Sie in der Strukturansicht mit der rechten Maustaste auf **Datenbanken**, und wählen Sie anschließend **Neue Datenbank**. 
 
4. Geben Sie im Fenster **Neue Datenbank** einen Namen für die Datenbank ein, und wählen Sie dann **OK**. 

5. Führen Sie das folgende Abfrageskript für die Datenbank aus, um die Tabelle **emp** zu erstellen und einige Beispieldaten einzufügen:

   ```
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO
    
    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
   ```

6. Klicken Sie in der Strukturansicht mit der rechten Maustaste auf die von Ihnen erstellte Datenbank, und wählen Sie anschließend **Neue Abfrage**.

### <a name="azure-storage-account"></a>Azure-Speicherkonto
Sie verwenden in diesem Tutorial ein allgemeines Azure-Speicherkonto (Azure Blob Storage) als Ziel/Senke-Datenspeicher. Falls Sie noch nicht über ein allgemeines Azure-Speicherkonto verfügen, helfen Ihnen die Informationen unter [Erstellen Sie ein Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) weiter. Die Pipeline in der Data Factory, die Sie in diesem Tutorial erstellen, kopiert Daten aus der lokalen SQL Server-Datenbank (Quelle) in diese Azure Blob Storage-Instanz (Senke). 

#### <a name="get-storage-account-name-and-account-key"></a>Abrufen des Speicherkontonamens und des Kontoschlüssels
In diesem Tutorial verwenden Sie Name und Schlüssel Ihres Azure-Speicherkontos. Beschaffen Sie den Namen und Schlüssel Ihres Speicherkontos, indem Sie wie folgt vorgehen: 

1. Melden Sie sich mit Ihrem Azure-Benutzernamen und dem dazugehörigen Kennwort am [Azure-Portal](https://portal.azure.com) an. 

2. Wählen Sie im linken Bereich die Option **Weitere Dienste**, filtern Sie nach dem Schlüsselwort **Speicher**, und wählen Sie dann **Speicherkonten** aus.

    ![Suchen nach einem Speicherkonto](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Filtern Sie in der Liste mit den Speicherkonten nach Ihrem Speicherkonto (falls erforderlich), und wählen Sie Ihr Speicherkonto aus. 

4. Wählen Sie im Fenster **Speicherkonto** die Option **Zugriffsschlüssel**.

    ![Abrufen des Speicherkontonamens und -schlüssels](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Kopieren Sie in den Feldern **Speicherkontoname** und **key1** die Werte, und fügen Sie sie zur späteren Verwendung im Tutorial in einen Editor ein. 

#### <a name="create-the-adftutorial-container"></a>Erstellen des Containers „adftutorial“ 
In diesem Abschnitt erstellen Sie einen Blobcontainer mit dem Namen **adftutorial** in Ihrer Azure Blob Storage-Instanz. 

1. Wechseln Sie im Fenster **Speicherkonto** zu **Übersicht**, und wählen Sie die Option **Blobs**. 

    ![Option zum Auswählen von Blobs](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. Wählen Sie im Fenster **Blob-Dienst** die Option **Container**. 

    ![Schaltfläche „Container hinzufügen“](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. Geben Sie im Fenster **Neuer Container** im Feld **Name** den Namen **adftutorial** ein, und wählen Sie **OK**. 

    ![Eingeben des Containernamens](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Wählen Sie in der Liste mit den Containern die Option **adftutorial**.  

    ![Auswählen des Containers](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Lassen Sie das Fenster **Container** für **adftutorial** geöffnet. Sie überprüfen darauf am Ende des Tutorials die Ausgabe. Data Factory erstellt den Ausgabeordner automatisch in diesem Container, damit Sie ihn nicht selbst erstellen müssen.

    ![Fenster „Container“](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
In diesem Schritt erstellen Sie eine Data Factory und starten die Azure Data Factory-Benutzeroberfläche, um eine Pipeline in der Data Factory zu erstellen. 

1. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/tutorial-hybrid-copy-portal/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **ADFTutorialDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/tutorial-hybrid-copy-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte für das Feld „Name“ der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
     ![Seite „Neue Data Factory“](./media/tutorial-hybrid-copy-portal/name-not-available-error.png)
3. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
4. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
      - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
      - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
        Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
4. Wählen Sie **V2 (Vorschau)** als **Version** aus.
5. Wählen Sie den **Standort** für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.
6. Wählen Sie die Option **An Dashboard anheften** aus.     
7. Klicken Sie auf **Create**.      
8. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Die Data Factory wird bereitgestellt**. 

    ![Kachel „Die Data Factory wird bereitgestellt“](media/tutorial-hybrid-copy-portal/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
   ![Data Factory-Startseite](./media/tutorial-hybrid-copy-portal/data-factory-home-page.png)
10. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Azure Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten. 


## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

1. Klicken Sie auf der Seite **Erste Schritte** auf **Pipeline erstellen**. Eine Pipeline wird automatisch für Sie erstellt. Die Pipeline wird in der Strukturansicht angezeigt, und der dazugehörige Editor ist geöffnet. 

   ![Seite für die ersten Schritte](./media/tutorial-hybrid-copy-portal/get-started-page.png)
2. Geben Sie im unteren Bereich des Fensters **Eigenschaften** auf der Registerkarte **Allgemein** unter **Name** die Zeichenfolge **SQLServerToBlobPipeline** ein.

   ![Pipelinename](./media/tutorial-hybrid-copy-portal/pipeline-name.png)
2. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Datenfluss**, und ziehen Sie die Aktivität **Kopieren** auf die Oberfläche zum Entwerfen von Pipelines. Legen Sie den Namen der Aktivität auf **CopySqlServerToAzureBlobActivity** fest.

   ![Aktivitätsname](./media/tutorial-hybrid-copy-portal/copy-activity-name.png)
3. Wechseln Sie im Eigenschaftenfenster zur Registerkarte **Quelle**, und klicken Sie auf **+ Neu**.

   ![Schaltfläche für neues Quelldataset](./media/tutorial-hybrid-copy-portal/source-dataset-new-button.png)
4. Suchen Sie im Fenster **Neues Dataset** nach **SQL Server**, wählen Sie **SQL Server** aus, und klicken Sie auf **Fertig stellen**. Daraufhin wird eine neue Registerkarte mit dem Titel **SqlServerTable1** angezeigt. Außerdem wird das Dataset **SqlServerTable1** links in der Strukturansicht angezeigt. 

   ![Auswählen von „SQL Server“](./media/tutorial-hybrid-copy-portal/select-sql-server.png)
5. Geben Sie im Eigenschaftenfenster auf der Registerkarte **Allgemein** unter **Name** die Zeichenfolge **SqlServerDataset** ein.
    
   ![Quelldataset – Name](./media/tutorial-hybrid-copy-portal/source-dataset-name.png)
6. Wechseln Sie zur Registerkarte **Verbindungen**, und klicken Sie auf **+ Neu**. In diesem Schritt erstellen Sie eine Verbindung mit dem Quelldatenspeicher (SQL Server-Datenbank). 

   ![Quelldataset – Schaltfläche für neue Verbindung](./media/tutorial-hybrid-copy-portal/source-connection-new-button.png)
7. Klicken Sie im Fenster **Neuer verknüpfter Dienst** auf **Neue Integrationslaufzeit**. In diesem Abschnitt erstellen Sie eine selbstgehostete Integration Runtime und ordnen sie einem lokalen Computer mit der SQL Server-Datenbank zu. Die selbstgehostete Integration Runtime ist die Komponente, die Daten aus der SQL Server-Datenbank auf Ihrem Computer in Azure Blob Storage kopiert. 

   ![Neue Integration Runtime](./media/tutorial-hybrid-copy-portal/new-integration-runtime-button.png)
8. Wählen Sie im Fenster **Integration Runtime Setup** (Integration Runtime-Setup) die Option **Privates Netzwerk** aus, und klicken Sie anschließend auf **Weiter**. 

   ![Auswählen von „Privates Netzwerk“](./media/tutorial-hybrid-copy-portal/select-private-network.png)
9. Geben Sie einen Namen für die Integration Runtime ein, und klicken Sie auf **Weiter**.  
    
   ![Integration Runtime – Name](./media/tutorial-hybrid-copy-portal/integration-runtime-name.png)
10. Klicken Sie im Abschnitt **Option 1: Express setup** (Option 1: Express-Setup) auf **Click here to launch the express setup for this computer** (Klicken Sie hier, um das Express-Setup für diesen Computer zu starten). 

   ![Klicken auf den Link für das Express-Setup](./media/tutorial-hybrid-copy-portal/click-exress-setup.png)
11. Klicken Sie im Fenster **Express-Setup von Integration Runtime (selbstgehostet)** auf **Schließen**. 

   ![Integration Runtime-Setup erfolgreich](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)
12. Klicken Sie im Webbrowser im Fenster **Integration Runtime Setup** (Integration Runtime-Setup) auf **Fertig stellen**. Daraufhin sollte wieder das Fenster **Neuer verknüpfter Dienst** angezeigt werden.

   ![Integration Runtime-Setup – Fertig stellen](./media/tutorial-hybrid-copy-portal/click-finish-integration-runtime-setup.png)
13. Führen Sie im Fenster **Neuer verknüpfter Dienst** die folgenden Schritte aus:

    1. Geben Sie unter **Name** die Zeichenfolge **SqlServerLinkedService** ein.
    2. Vergewissern Sie sich, dass unter **Connect via integration runtime** (Verbindung per Integration Runtime herstellen) die zuvor erstellte selbstgehostete Integration Runtime angezeigt wird.
    3. Geben Sie unter **Servername** den Namen Ihrer SQL Server-Instanz an. 
    4. Geben Sie im Feld **Datenbankname** den Namen der Datenbank mit der Tabelle **emp** an. 
    5. Wählen Sie einen geeigneten **Authentifizierungstyp** aus, den Data Factory beim Herstellen der Verbindung mit Ihrer SQL Server-Datenbank verwenden soll. 
    6. Geben Sie **Benutzername** und **Kennwort** ein. Wenn Ihr Benutzerkonto- oder Servername einen umgekehrten Schrägstrich (\\) enthält, ist es erforderlich, ein Escapezeichen (\\) voranzustellen. Verwenden Sie beispielsweise *mydomain\\\\myuser*. 
    7. Klicken Sie auf **Verbindung testen**. Mit diesem Schritt vergewissern Sie sich, dass der Data Factory-Dienst über die selbstgehostete Integration Runtime, die Sie erstellt haben, eine Verbindung mit Ihrer SQL Server-Datenbank herstellen kann. 
    8. Klicken Sie auf **Speichern**, um den verknüpften Dienst zu speichern.

       ![Mit SQL Server verknüpfter Dienst – Einstellungen](./media/tutorial-hybrid-copy-portal/sql-server-linked-service-settings.png)
14. Sie sollten sich wieder im Fenster mit dem geöffneten Quelldataset befinden. Führen Sie auf der Registerkarte **Verbindung** des Fensters **Eigenschaften** folgende Schritte aus: 

    1. Vergewissern Sie sich, dass unter **Verknüpfter Dienst** die Zeichenfolge **SqlServerLinkedService** angezeigt wird. 
    2. Wählen Sie unter **Tabelle** die Option **[dbo].[emp]** aus.

        ![Quelldataset – Verbindungsinformationen](./media/tutorial-hybrid-copy-portal/source-dataset-connection.png)
15. Wechseln Sie zur Registerkarte mit der Pipeline „SQLServerToBlobPipeline“, oder klicken Sie in der Strukturansicht auf **SQLServerToBlobPipeline**. 

    ![Pipelineregisterkarte](./media/tutorial-hybrid-copy-portal/pipeliene-tab.png)
16. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **Senke**, und klicken Sie auf **+ Neu**. 

    ![Senkendataset – Schaltfläche „Neu“](./media/tutorial-hybrid-copy-portal/sink-dataset-new-button.png)
17. Wählen Sie im Fenster **Neues Dataset** die Option **Azure Blob Storage** aus, und klicken Sie dann auf **Fertig stellen**. Daraufhin wird eine neue Registerkarte für das Dataset geöffnet. Außerdem wird das Dataset in der Strukturansicht angezeigt. 

    ![Auswählen von „Azure Blob Storage“](./media/tutorial-hybrid-copy-portal/select-azure-blob-storage.png)
18. Geben Sie unter **Name** die Zeichenfolge **AzureBlobDataset** ein.

    ![Senkendataset – Name](./media/tutorial-hybrid-copy-portal/sink-dataset-name.png)
19. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **Verbindung**, und klicken Sie für **Verknüpfter Dienst** auf **+ Neu**. 

    ![Schaltfläche für neuen verknüpften Dienst](./media/tutorial-hybrid-copy-portal/new-storage-linked-service-button.png)
20. Führen Sie im Fenster **Neuer verknüpfter Dienst** die folgenden Schritte aus:

    1. Geben Sie unter **Name** die Zeichenfolge **AzureStorageLinkedService** ein.
    2. Wählen Sie unter **Speicherkontoname** Ihr Azure-Speicherkonto aus. 
    3. Klicken Sie auf **Verbindung testen**, um die Verbindung mit Ihrem Azure-Speicherkonto zu testen.
    4. Klicken Sie auf **Speichern**.

        ![Mit Azure Storage verknüpfter Dienst – Einstellungen](./media/tutorial-hybrid-copy-portal/azure-storage-linked-service-settings.png) 
21.  Sie sollten sich wieder im Fenster mit dem geöffneten Senkendataset befinden. Führen Sie auf der Registerkarte **Verbindung** folgende Schritte aus: 

        1. Vergewissern Sie sich, dass unter **Verknüpfter Dienst** die Option **AzureStorageLinkedService** ausgewählt ist.
        2. Geben Sie unter **Dateipfad** für die **Ordnerkomponente** die Zeichenfolge **adftutorial/fromonprem** ein. Sollte der Ausgabeordner im Container „adftutorial“ nicht vorhanden sein, wird der Ausgabeordner automatisch erstellt.
        3. Geben Sie unter **Dateipfad** für die **Dateinamenkomponente** die Zeichenfolge `@CONCAT(pipeline().RunId, '.txt')` ein.

            ![Senkendataset – Verbindung](./media/tutorial-hybrid-copy-portal/sink-dataset-connection.png)
22. Wechseln Sie zur Registerkarte mit der geöffneten Pipeline, oder klicken Sie in der **Strukturansicht** auf die **Pipeline**. Vergewissern Sie sich, dass unter **Sink Dataset** (Senkendataset) die Option **AzureBlobDataset** ausgewählt ist. 

    ![Senkendataset ausgewählt ](./media/tutorial-hybrid-copy-portal/sink-dataset-selected.png)
23. Klicken Sie zum Überprüfen der Pipelineeinstellungen auf der Symbolleiste für die Pipeline auf „Überprüfen“. Schließen Sie die Ausgabe der **Pipelineüberprüfung**durch Klicken auf das **X** in der rechten Ecke. 

    ![Überprüfen der Pipeline](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
1. Klicken Sie auf **Veröffentlichen**, um erstellte Entitäten für den Azure Data Factory-Dienst zu veröffentlichen.

    ![Schaltfläche "Veröffentlichen"](./media/tutorial-hybrid-copy-portal/publish-button.png)
24. Warten Sie, bis die Popupmeldung **Veröffentlichung erfolgreich** angezeigt wird. Den Veröffentlichungsstatus können Sie auch überprüfen, indem Sie im linken Bereich auf den Link **Benachrichtigungen anzeigen** klicken. Schließen Sie das Benachrichtigungsfenster, indem Sie auf das **X** klicken. 

    ![Veröffentlichung erfolgreich](./media/tutorial-hybrid-copy-portal/publishing-succeeded.png)
    

## <a name="trigger-a-pipeline-run"></a>Auslösen einer Pipelineausführung
Klicken Sie auf der Symbolleiste der Pipeline auf **Trigger** und dann auf **Trigger Now** (Jetzt auslösen).

![Jetzt auslösen](./media/tutorial-hybrid-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

1. Wechseln Sie zur Registerkarte **Überwachen**. Dort sehen Sie die Pipeline, die Sie im vorherigen Schritt manuell ausgelöst haben. 

    ![Pipelineausführungen](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
2. Wenn Sie mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf **View Activity Runs** (Aktivitätsausführungen anzeigen). Da die Pipeline nur eine einzelne Aktivität enthält, werden nur Aktivitätsausführungen angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf den Link **Details** (Brillensymbol). Durch Klicken auf **Pipelines** (im oberen Bereich) können Sie zur Ansicht mit den Pipelineausführungen zurückkehren.

    ![Aktivitätsausführungen](./media/tutorial-hybrid-copy-portal/activity-runs.png)

## <a name="verify-the-output"></a>Überprüfen der Ausgabe
Die Pipeline erstellt den Ausgabeordner *fromonprem* automatisch im Blobcontainer `adftutorial`. Vergewissern Sie sich, dass die Datei *dbo.emp.txt* im Ausgabeordner enthalten ist. 

1. Wählen Sie im Azure-Portal im Fenster des Containers **adftutorial** die Option **Aktualisieren**, um den Ausgabeordner anzuzeigen.

    ![Erstellter Ausgabeordner](media/tutorial-hybrid-copy-portal/fromonprem-folder.png)
2. Wählen Sie in der Liste mit den Ordnern die Option `fromonprem`. 
3. Vergewissern Sie sich, dass die Datei `dbo.emp.txt` angezeigt wird.

    ![Ausgabedatei](media/tutorial-hybrid-copy-portal/fromonprem-file.png)


## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure-Blobspeicher von einem Speicherort an einen anderen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen Sie eine selbstgehostete Integration Runtime.
> * Erstellen von verknüpften SQL Server- und Azure Storage-Diensten 
> * Erstellen von SQL Server- und Azure Blob-Datasets
> * Erstellen einer Pipeline mit Kopieraktivität zum Verschieben der Daten
> * Starten einer Pipelineausführung
> * Überwachen der Pipelineausführung.

Eine Liste mit den von der Data Factory unterstützten Datenspeichern finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Fahren Sie mit dem folgenden Tutorial fort, um mehr über das Kopieren von Daten per Massenvorgang aus einer Quelle in ein Ziel zu erfahren:

> [!div class="nextstepaction"]
>[Massenkopieren von Daten](tutorial-bulk-copy-portal.md)

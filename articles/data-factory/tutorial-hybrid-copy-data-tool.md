---
title: Kopieren lokaler Daten mithilfe des Azure-Tools zum Kopieren von Daten | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory-Instanz, und verwenden Sie dann das Tool zum Kopieren von Daten, um Daten aus einer lokalen SQL Server-Datenbank in Azure Blob Storage zu kopieren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: 85b721df1e666903c4966ca240c433ded01c06b7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-the-copy-data-tool"></a>Kopieren von Daten aus einer lokalen SQL Server-Datenbank in Azure Blob Storage mithilfe des Tools zum Kopieren von Daten
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – allgemein verfügbar](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 – Vorschauversion](tutorial-hybrid-copy-data-tool.md)

In diesem Tutorial verwenden Sie das Azure-Portal, um eine Data Factory zu erstellen. Anschließend erstellen Sie mithilfe des Tools zum Kopieren von Daten eine Pipeline, die Daten aus einer lokalen SQL Server-Datenbank in Azure Blob Storage kopiert.

> [!NOTE]
> - Falls Sie noch nicht mit Azure Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](introduction.md) zu lesen.
>
> - Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 von Data Factory verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten
> * Überwachen der Pipeline- und Aktivitätsausführungen.

## <a name="prerequisites"></a>Voraussetzungen
### <a name="azure-subscription"></a>Azure-Abonnement
Wenn Sie nicht bereits ein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

### <a name="azure-roles"></a>Azure-Rollen
Damit Sie Data Factory-Instanzen erstellen können, muss dem Benutzerkonto, mit dem Sie sich bei Azure anmelden, die Rolle *Mitwirkender* oder *Besitzer* zugewiesen sein, oder es muss ein *Administrator* des Azure-Abonnements sein. 

Die Berechtigungen, über die Sie im Abonnement verfügen, können Sie im Azure-Portal einsehen. Wählen Sie oben rechts Ihren Benutzernamen und dann **Berechtigungen** aus. Wenn Sie Zugriff auf mehrere Abonnements besitzen, wählen Sie das entsprechende Abonnement aus. Eine Beispielanleitung zum Hinzufügen eines Benutzers zu einer Rolle finden Sie im Artikel [Hinzufügen oder Ändern von Azure-Abonnementadministratoren](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 und 2017
In diesem Tutorial verwenden Sie eine lokale SQL Server-Datenbank als *Quelldatenspeicher*. Die Pipeline in der in diesem Tutorial erstellten Data Factory kopiert Daten aus dieser lokalen SQL Server-Datenbank (Quelle) in Blob Storage (Senke). Anschließend erstellen Sie eine Tabelle mit dem Namen **emp** in Ihrer SQL Server-Datenbank und fügen einige Beispieleinträge in die Tabelle ein. 

1. Starten Sie SQL Server Management Studio. Falls die Suite auf Ihrem Computer noch nicht installiert wurde, rufen Sie [Herunterladen von SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) auf. 

2. Stellen Sie eine Verbindung mit Ihrer SQL Server-Instanz her, indem Sie Ihre Anmeldeinformationen verwenden. 

3. Erstellen Sie eine Beispieldatenbank. Klicken Sie in der Strukturansicht mit der rechten Maustaste auf **Datenbanken**, und wählen Sie anschließend **Neue Datenbank**. 
 
4. Geben Sie im Fenster **Neue Datenbank** einen Namen für die Datenbank ein, und wählen Sie dann **OK**. 

5. Führen Sie das folgende Abfrageskript für die Datenbank aus, um die Tabelle **emp** zu erstellen und einige Beispieldaten einzufügen. Klicken Sie in der Strukturansicht mit der rechten Maustaste auf die von Ihnen erstellte Datenbank, und wählen Sie anschließend **Neue Abfrage**.

    ```sql
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

### <a name="azure-storage-account"></a>Azure-Speicherkonto
Sie verwenden in diesem Tutorial ein Azure Storage-Allzweckkonto (Blob Storage) als Ziel-/Senkendatenspeicher. Falls Sie noch nicht über ein allgemeines Speicherkonto verfügen, erfahren Sie unter [Erstellen Sie ein Speicherkonto.](../storage/common/storage-create-storage-account.md#create-a-storage-account), wie Sie eins erstellen. Die Pipeline in der Data Factory, die Sie in diesem Tutorial erstellen, kopiert Daten aus der lokalen SQL Server-Datenbank (Quelle) in diese Blob Storage-Instanz (Senke). 

#### <a name="get-the-storage-account-name-and-account-key"></a>Abrufen des Speicherkontonamens und des Kontoschlüssels
In diesem Tutorial verwenden Sie Name und Schlüssel Ihres Speicherkontos. Den Namen und den Schlüssel Ihres Speicherkontos ermitteln Sie anhand der folgenden Schritte: 

1. Melden Sie sich mit Ihrem Azure-Benutzernamen und dem dazugehörigen Kennwort beim [Azure-Portal](https://portal.azure.com) an. 

2. Wählen Sie im linken Bereich **Weitere Dienste** aus. Filtern Sie nach dem Schlüsselwort **Speicher**, und wählen Sie dann **Speicherkonten** aus.

    ![Speicherkontosuche](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Filtern Sie in der Liste mit den Speicherkonten ggf. nach Ihrem Speicherkonto. Wählen Sie dann Ihr Speicherkonto aus. 

4. Wählen Sie im Fenster **Speicherkonto** die Option **Zugriffsschlüssel**.

    ![Zugriffsschlüssel](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Kopieren Sie in den Feldern **Speicherkontoname** und **key1** die Werte, und fügen Sie sie zur späteren Verwendung im Tutorial in einen Editor ein. 

#### <a name="create-the-adftutorial-container"></a>Erstellen des Containers „adftutorial“ 
In diesem Abschnitt erstellen Sie einen Blobcontainer mit dem Namen **adftutorial** in Ihrer Blob Storage-Instanz. 

1. Wechseln Sie im Fenster **Speicherkonto** zu **Übersicht**, und wählen Sie die Option **Blobs**. 

    ![Option zum Auswählen von Blobs](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. Wählen Sie im Fenster **Blob-Dienst** die Option **Container**. 

    ![Schaltfläche „Container“](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. Geben Sie im Fenster **Neuer Container** im Feld **Name** den Namen **adftutorial** ein, und wählen Sie **OK**. 

    ![Neuer Container](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Wählen Sie in der Liste mit den Containern die Option **adftutorial**.

    ![Containerauswahl](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Lassen Sie das Fenster **Container** für **adftutorial** geöffnet. Sie überprüfen darauf am Ende des Tutorials die Ausgabe. Data Factory erstellt den Ausgabeordner automatisch in diesem Container, damit Sie ihn nicht selbst erstellen müssen.

    ![Fenster „Container“](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im Menü auf der linken Seite auf **Neu** > **Daten + Analysen** > **Data Factory**. 
   
   ![Erstellen einer neuen Data Factory](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 
      
     ![Neue Data Factory](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   Der Name der Data Factory muss *global eindeutig* sein. Sollte für das Feld „Name“ die folgende Fehlermeldung angezeigt werden, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
   ![Name der neuen Data Factory](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Wählen Sie das **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
4. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
     
      - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe.

      - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein. 
         
      Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).
5. Wählen Sie **V2 (Vorschau)** als **Version** aus.
6. Wählen Sie unter **Standort** den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die Datenspeicher (etwa Azure Storage und SQL-Datenbank) und Computeeinheiten (etwa Azure HDInsight), die von Data Factory genutzt werden, können sich an anderen Standorten und in anderen Regionen befinden.
7. Wählen Sie die Option **An Dashboard anheften** aus. 
8. Klicken Sie auf **Erstellen**.
9. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status **Deploying Data Factory** (Data Factory wird bereitgestellt...):

    ![Kachel „Deploying Data Factory“ (Data Factory wird bereitgestellt...)](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
10. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt:
   
    ![Data Factory-Startseite](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
11. Klicken Sie auf **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten

1. Klicken Sie auf der Seite **Erste Schritte** auf **Daten kopieren**, um das Tool zum Kopieren von Daten zu starten. 

   ![Kachel für das Tool zum Kopieren von Daten](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. Geben Sie auf der Seite **Eigenschaften** des Tools zum Kopieren von Daten unter **Aufgabenname** den Namen **CopyFromOnPremSqlToAzureBlobPipeline** ein. Klicken Sie anschließend auf **Weiter**. Das Tool zum Kopieren von Daten erstellt eine Pipeline mit dem Namen, den Sie in diesem Feld angeben. 
    
   ![Taskname](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. Wählen Sie auf der Seite **Quelldatenspeicher** die Option **SQL Server** aus, und klicken Sie dann auf **Weiter**. Scrollen Sie ggf. in der Liste nach unten, um zu **SQL Server** zu gelangen. 

   ![Auswahl von SQL Server](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. Geben Sie unter **Verbindungsname** die Zeichenfolge **SqlServerLinkedService** ein. Klicken Sie auf den Link **Create Integration Runtime** (Integration Runtime erstellen). Sie müssen eine selbstgehostete Integration Runtime erstellen, auf Ihren Computer herunterladen und bei Data Factory registrieren. Die selbstgehostete Integration Runtime kopiert Daten zwischen Ihrer lokalen Umgebung und der Cloud.

   ![Erstellen der selbstgehosteten Integration Runtime](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. Geben Sie im Dialogfeld **Create Integration Runtime** (Integration Runtime erstellen) unter **Name** die Zeichenfolge **TutorialIntegration Runtime** ein. Klicken Sie anschließend auf **Erstellen**. 

   ![Name der Integration Runtime](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. Klicken Sie auf **Launch express setup on this computer** (Express-Setup auf diesem Computer starten). Dadurch wird die Integration Runtime auf Ihrem Computer installiert und bei Data Factory registriert. Alternativ können Sie die Installationsdatei über die manuelle Setupoption herunterladen, die Datei ausführen und die Integration Runtime mithilfe des Schlüssels registrieren. 

    ![Link zum Starten des Express-Setups auf diesem Computer](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. Führen Sie die heruntergeladene Anwendung aus. Der Status des Express-Setups wird im Fenster angezeigt. 

    ![Status des Express-Setups](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. Vergewissern Sie sich, dass im Feld **Integration Runtime** die Option **TutorialIntegrationRuntime** ausgewählt ist.

    ![Integration Runtime ausgewählt](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. Gehen Sie unter **Specify the on-premises SQL Server database** (Angeben der lokalen SQL Server-Datenbank) wie folgt vor: 

    a. Geben Sie unter **Verbindungsname** die Zeichenfolge **OnPremSqlLinkedService** ein.

    b. Geben Sie unter **Servername** den Namen Ihrer lokalen SQL Server-Instanz ein.

    c. Geben Sie unter **Datenbankname** den Namen Ihrer lokalen Datenbank ein.

    d. Wählen Sie unter **Authentifizierungstyp** eine geeignete Authentifizierung aus.

    e. Geben Sie unter **Benutzername** den Namen eines Benutzers mit Zugriff auf die lokale SQL Server-Instanz ein.

    f. Geben Sie das **Kennwort** für den Benutzer ein. 
10. Wählen Sie in der Liste auf der Seite **Select tables from which to copy the data or use a custom query** (Tabellen auswählen, aus denen die Daten kopiert werden sollen, oder benutzerdefinierte Abfrage verwenden) die Tabelle **[dbo].[emp]** aus, und klicken Sie anschließend auf **Weiter**. 

    ![Auswählen der Tabelle „emp“](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. Wählen Sie auf der Seite **Zieldatenspeicher** die Option **Azure Blob Storage** aus, und klicken Sie dann auf **Weiter**.

    ![Auswählen von Blob Storage](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. Führen Sie auf der Seite **Azure Blob Storage-Konto angeben** die folgenden Schritte aus: 

    a. Geben Sie im Feld **Verbindungsname** den Namen **AzureStorageLinkedService** ein.

    b. Wählen Sie in der Dropdownliste unter **Speicherkontoname** Ihr Speicherkonto aus. 

    c. Klicken Sie auf **Weiter**.

    ![Angeben des Speicherkontos](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. Geben Sie auf der Seite **Choose the output file or folder** (Ausgabedatei oder -ordner auswählen) unter **Ordnerpfad** den Pfad **adftutorial/fromonprem** ein. Den Container **adftutorial** haben Sie im Rahmen der Vorbereitung erstellt. Ist der Ausgabeordner nicht vorhanden, wird er von Data Factory automatisch erstellt. Sie können auch auf die Schaltfläche **Durchsuchen** klicken und zum Blobspeicher und den dazugehörigen Containern/Ordnern navigieren. Beachten Sie, dass der Name der Ausgabedatei standardmäßig auf **dbo.emp** festgelegt ist.
        
    ![Auswählen der Ausgabedatei oder des Ausgabeordners](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. Klicken Sie auf der Seite **Dateiformateinstellungen** auf **Weiter**. 

    ![Seite mit den Dateiformateinstellungen](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**. 

    ![Seite "Einstellungen"](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. Überprüfen Sie auf der Seite **Zusammenfassung** die Werte sämtlicher Einstellungen, und klicken Sie anschließend auf **Weiter**. 

    ![Seite „Zusammenfassung“](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die erstellte Pipeline oder den erstellten Task zu überwachen.

    ![Bereitstellungsseite](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. Auf der Registerkarte **Überwachen** wird der Status der erstellten Pipeline angezeigt. Mit den Links in der Spalte **Aktionen** können Sie Aktivitätsausführungen anzeigen, die der Pipelineausführung zugeordnet sind, und die Pipeline erneut ausführen. 

    ![Überwachen der Pipelineausführungen](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. Klicken Sie in der Spalte **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen), um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie auf den Link **Details** (Brillensymbol) in der Spalte **Aktionen**. Klicken Sie oben auf **Pipelines**, um zurück zur Ansicht **Pipelineausführungen** zu wechseln.

    ![Überwachung der Aktivitätsausführungen](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Vergewissern Sie sich, dass die Ausgabedatei im Ordner **fromonprem** des Containers **adftutorial** enthalten ist. 
 
    ![Ausgabeblob](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Klicken Sie im linken Bereich auf die Registerkarte **Bearbeiten**, um in den Editor-Modus zu wechseln. Sie können die vom Tool erstellten verknüpften Dienste, Datasets und Pipelines mit dem Editor aktualisieren. Klicken Sie auf **Code**, um den JSON-Code für die im Editor geöffnete Entität anzuzeigen. Ausführliche Informationen zum Bearbeiten dieser Entitäten über die Data Factory-Benutzeroberfläche finden Sie in der [Azure-Portal-Version dieses Tutorials](tutorial-copy-data-portal.md).

    ![Registerkarte „Bearbeiten“](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten aus einer lokalen SQL Server-Datenbank in Blob Storage. Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten
> * Überwachen der Pipeline- und Aktivitätsausführungen.

Eine Liste mit den von Data Factory unterstützten Datenspeichern finden Sie unter [Unterstützte Datenspeicher und Formate](copy-activity-overview.md#supported-data-stores-and-formats).

Fahren Sie mit dem folgenden Tutorial fort, um mehr über das Kopieren von Daten per Massenvorgang aus einer Quelle in ein Ziel zu erfahren:

> [!div class="nextstepaction"]
>[Massenkopieren von Daten](tutorial-bulk-copy-portal.md)

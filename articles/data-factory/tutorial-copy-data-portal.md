---
title: Verwenden des Azure-Portals zum Erstellen einer Data Factory-Pipeline | Microsoft Docs
description: "Dieses Tutorial enthält detaillierte Anweisungen zur Erstellung einer Data Factory mit Pipeline mithilfe des Azure-Portals. Die Pipeline kopiert mit der Copy-Aktivität Daten aus einem Azure-Blobspeicher in eine Azure SQL-Datenbank. "
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
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 424a5ec49018e969edbf90c374a9da7e1d22395d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>Kopieren von Daten aus Azure Blob Storage nach Azure SQL-Datenbank mithilfe von Azure Data Factory
In diesem Tutorial erstellen Sie eine Data Factory über die Azure Data Factory-Benutzeroberfläche (User Interface, UI). Die Pipeline in dieser Data Factory kopiert Daten aus Azure Blob Storage in Azure SQL-Datenbank. Das Konfigurationsmuster in diesem Tutorial gilt für Kopiervorgänge aus einem dateibasierten Datenspeicher in einen relationalen Datenspeicher. Eine Liste der Datenspeicher, die als Quellen und Senken unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> - Falls Sie noch nicht mit Azure Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](introduction.md) zu lesen.
>
> - Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen Sie eine Pipeline mit einer Copy-Aktivität.
> * Ausführen eines Testlaufs für die Pipeline
> * Manuelles Auslösen der Pipeline
> * Auslösen der Pipeline nach einem Zeitplan
> * Überwachen der Pipeline- und Aktivitätsausführungen.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure Storage-Konto**. Sie verwenden den Blob Storage als **Quelldatenspeicher**. Wenn Sie kein Azure Storage-Konto besitzen, finden Sie im Artikel [Erstellen eines Speicherkontos](../storage/common/storage-create-storage-account.md#create-a-storage-account) Schritte zum Erstellen eines solchen Kontos.
* **Azure SQL-Datenbank**. Sie verwenden die Datenbank als **Senkendatenspeicher**. Wenn Sie noch nicht über eine Azure SQL-Datenbank verfügen, finden Sie im Artikel [Erstellen einer Azure SQL-Datenbank](../sql-database/sql-database-get-started-portal.md) die Schritte zum Erstellen einer solchen Datenbank.

### <a name="create-a-blob-and-a-sql-table"></a>Erstellen eines Blobs und einer SQL-Tabelle

Bereiten Sie jetzt Ihr Azure-Blob und Ihre Azure SQL-Datenbank für das Tutorial vor, indem Sie die folgenden Schritte ausführen:

#### <a name="create-a-source-blob"></a>Erstellen eines Quellblobs

1. Starten Sie den Editor. Kopieren Sie den folgenden Text, und speichern Sie ihn als **emp.txt**-Datei auf einem Datenträger.

    ```
    John,Doe
    Jane,Doe
    ```

2. Erstellen Sie im Azure-Blobspeicher einen Container mit dem Namen **adftutorial**- Erstellen Sie einen Ordner namens **input** in diesem Container. Laden Sie anschließend die Datei **emp.txt** in den Ordner **input** hoch. Verwenden Sie für diese Aufgaben das Azure-Portal oder Tools wie [Azure Storage-Explorer](http://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Erstellen einer SQL-Senkentabelle

1. Verwenden Sie das folgende SQL-Skript, um die Tabelle **dbo.emp** in Ihrer Azure SQL-Datenbank zu erstellen.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Gewähren Sie Azure-Diensten den Zugriff auf SQL Server. Stellen Sie sicher, dass die Einstellung **Zugriff auf Azure-Dienste erlauben** für Ihre Azure SQL Server-Instanz **aktiviert** ist, damit der Data Factory-Dienst auf diese Instanz zugreifen kann. Führen Sie folgende Schritte aus, um diese Einstellung zu überprüfen und zu aktivieren:

    1. Klicken Sie links auf den Hub **Weitere Dienste** und dann auf **SQL Server**.
    2. Wählen Sie Ihren Server aus, und klicken Sie unter **EINSTELLUNGEN** auf **Firewall**.
    3. Klicken Sie auf der Seite **Firewalleinstellungen** für **Zugriff auf Azure-Dienste erlauben** auf **EIN**.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
In diesem Schritt erstellen Sie eine Data Factory und starten die Azure Data Factory-Benutzeroberfläche, um eine Pipeline in der Data Factory zu erstellen. 

1. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte für das Feld „Name“ der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
     ![Seite „Neue Data Factory“](./media/tutorial-copy-data-portal/name-not-available-error.png)
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

    ![Kachel „Die Data Factory wird bereitgestellt“](media/tutorial-copy-data-portal/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
   ![Data Factory-Startseite](./media/tutorial-copy-data-portal/data-factory-home-page.png)
10. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten.

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline
In diesem Schritt erstellen Sie eine Pipeline mit einer Copy-Aktivität in der Data Factory. Die Copy-Aktivität kopiert Daten aus Azure Blob Storage in Azure SQL-Datenbank. Im [Schnellstarttutorial](quickstart-create-data-factory-portal.md) haben Sie anhand der folgenden Schritte eine Pipeline erstellt:

1. 1. Erstellen des verknüpften Diensts 
2. Erstellen von Eingabe- und Ausgabedatasets
3. Erstellen einer Pipeline

In diesem Tutorial beginnen Sie mit dem Erstellen der Pipeline und erstellen verknüpfte Dienste und Datasets, wenn Sie sie zum Konfigurieren der Pipeline benötigen. 

1. Klicken Sie auf der Seite „Erste Schritte“ auf die Kachel **Pipeline erstellen**. 

   ![Kachel zum Erstellen der Pipeline](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
3. Geben Sie im Fenster **Eigenschaften** der Pipeline im Feld **Name** den Namen **CopyPipeline** für die Pipeline ein.

    ![Pipelinename](./media/tutorial-copy-data-portal/pipeline-name.png)
4. Erweitern Sie in der Toolbox **Aktivitäten** die Option **DataFlow**, und verschieben Sie die **Copy**-Aktivität aus der Toolbox auf die Oberfläche des Pipeline-Designers. 

    ![Verschieben der Copy-Aktivität per Drag&Drop](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
5. Geben Sie im Fenster **Eigenschaften** auf der Registerkarte **Allgemein** als Name der Aktivität **CopyFromBlobToSql** ein.

    ![Name der Aktivität](./media/tutorial-copy-data-portal/activity-name.png)
6. Wechseln Sie zur Registerkarte **Quelle**. Klicken Sie auf **+ Neu**, um ein Quelldataset zu erstellen. 

    ![Menü für neues Quelldataset](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
7. Wählen Sie im Fenster **Neues Dataset** die Option **Azure Blob Storage** aus, und klicken Sie dann auf **Fertig stellen**. Da sich die Quelldaten in einem Azure-Blobspeicher befinden, wählen Sie „Azure Blob Storage“ als Quelldataset aus. 

    ![Auswählen von „Azure Blob Storage“](./media/tutorial-copy-data-portal/select-azure-storage.png)
8. In der Anwendung wird eine neue **Registerkarte** mit dem Titel **AzureBlob1** geöffnet.

    ![Registerkarte „AzureBlob1“ ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
9. Geben Sie unten im Fenster **Eigenschaften** auf der Registerkarte **Allgemein** im Feld **Name** den Namen **SourceBlobDataset** ein.

    ![Datasetname](./media/tutorial-copy-data-portal/dataset-name.png)
10. Wechseln Sie im Eigenschaftenfenster zur Registerkarte **Verbindung**.   

    ![Registerkarte „Verbindung“](./media/tutorial-copy-data-portal/source-dataset-connection-tab.png)
11. Klicken Sie neben dem Textfeld **Verknüpfter Dienst** auf **+ Neu**. Ein verknüpfter Dienst verbindet einen Datenspeicher oder einen Computedienst mit der Data Factory. In diesem Fall erstellen Sie einen mit Azure Storage verknüpften Dienst, der Ihr Azure Storage-Konto mit dem Datenspeicher verbindet. Der verknüpfte Dienst enthält die Verbindungsinformationen, die der Data Factory-Dienst zum Herstellen einer Verbindung mit dem Blobspeicher zur Laufzeit verwendet. Das Dataset gibt den Container, den Ordner und (optional) die Datei an, die die Quelldaten enthält. 

    ![Schaltfläche für neuen verknüpften Dienst](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
12. Führen Sie im Fenster **Neuer verknüpfter Dienst** die folgenden Schritte aus: 

    1. Geben Sie im Feld **Name**  den Namen **AzureStorageLinkedService** ein. 
    2. Wählen Sie im Feld **Speicherkontoname** Ihr Azure-Speicherkonto aus.
    3. Klicken Sie auf **Verbindung testen**, um die Verbindung mit dem Azure-Speicherkonto zu testen.  
    4. Klicken Sie auf **Speichern**, um den verknüpften Dienst zu speichern.

        ![Neuer mit Azure Storage verknüpfter Dienst](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
13. Klicken Sie für das Feld **Dateipfad** auf **Durchsuchen**.  

    ![Schaltfläche „Durchsuchen“ für die Datei](./media/tutorial-copy-data-portal/file-browse-button.png)
14. Navigieren Sie zum Ordner **adftutorial/input**, wählen Sie die Datei **emp.txt** aus, und klicken Sie auf **Fertig stellen**. Alternativ können Sie auf „emp.txt“ doppelklicken. 

    ![Auswählen der Eingabedatei](./media/tutorial-copy-data-portal/select-input-file.png)
15. Vergewissern Sie sich, dass für **Dateiformat** die Option **Textformat** und für **Spaltentrennzeichen** die Option **Komma (`,`)** festgelegt ist. Wenn in der Quelldatei verschiedene Zeilen- und Spaltentrennzeichen verwendet werden, können Sie für das Feld **Dateiformat** auf **Detect Text Format** (Textformat ermitteln) klicken. Das Tool zum Kopieren von Daten ermittelt automatisch Dateiformat und Trennzeichen für Sie. Sie können diese Werte immer noch überschreiben. Auf dieser Seite können Sie eine Vorschau der Daten anzeigen, indem Sie auf **Datenvorschau** klicken.

    ![Ermitteln des Textformats](./media/tutorial-copy-data-portal/detect-text-format.png)
17. Wechseln Sie im Fenster „Eigenschaften“ zur Registerkarte **Schema**, und klicken Sie auf **Schema importieren**. Beachten Sie, dass die Anwendung zwei Spalten in der Quelldatei erkannt hat. Sie importieren das Schema hier, sodass Sie Spalten aus dem Quelldatenspeicher dem Senkendatenspeicher zuordnen können. Falls Sie keine Spalten zuordnen müssen, können Sie diesen Schritt überspringen. Importieren Sie das Schema für dieses Tutorial.

    ![Ermitteln des Quellschemas](./media/tutorial-copy-data-portal/detect-source-schema.png)  
19. Wechseln Sie nun zur **Registerkarte mit der Pipeline**, oder klicken Sie in der **Strukturansicht** auf der linken Seite auf die Pipeline.  

    ![Pipelineregisterkarte](./media/tutorial-copy-data-portal/pipeline-tab.png)
20. Vergewissern Sie sich, dass im Eigenschaftenfenster im Feld „Source Dataset“ (Quelldataset) die Option **SourceBlobDataset** ausgewählt ist. Auf dieser Seite können Sie eine Vorschau der Daten anzeigen, indem Sie auf **Datenvorschau** klicken. 
    
    ![Quelldataset](./media/tutorial-copy-data-portal/source-dataset-selected.png)
21. Wechseln Sie zur Registerkarte **Senke**, und klicken Sie auf **+ Neu**, um ein Senkendataset zu erstellen. 

    ![Menü für neues Senkendataset](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
22. Wählen Sie im Fenster **Neues Dataset** die Option **Azure SQL-Datenbank**, und klicken Sie auf **Fertig stellen**. In diesem Tutorial kopieren Sie Daten in eine Azure SQL-Datenbank. 

    ![Auswählen einer Azure SQL-Datenbank](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
23. Legen Sie im Eigenschaftenfenster auf der Registerkarte **Allgemein** als Name **OutputSqlDataset** fest. 
    
    ![Name des Ausgabedatasets](./media/tutorial-copy-data-portal/output-dataset-name.png)
24. Wechseln Sie zur Registerkarte **Verbindung**, und klicken Sie für **Verknüpfter Dienst** auf **Neu**. Einem verknüpften Dienst muss ein Dataset zugewiesen werden. Der verknüpfte Dienst enthält die Verbindungszeichenfolge, die der Data Factory-Dienst zum Herstellen einer Verbindung mit der Azure SQL-Datenbank zur Laufzeit verwendet. Das Dataset gibt den Container, den Ordner und (optional) die Datei an, in die die Quelldaten kopiert werden. 
    
    ![Schaltfläche für neuen verknüpften Dienst](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
25. Führen Sie im Fenster **Neuer verknüpfter Dienst** die folgenden Schritte aus: 

    1. Geben Sie in das Feld **Name** den Namen **AzureSqlDatabaseLinkedService** ein. 
    2. Wählen Sie im Feld **Servername** Ihre Azure SQL Server-Instanz aus.
    4. Wählen Sie im Feld **Datenbankname** Ihre Azure SQL-Datenbank aus. 
    5. Geben Sie im Feld **Benutzername** den Namen des Benutzers ein. 
    6. Geben Sie im Feld **Kennwort** das Kennwort für den Benutzer ein. 
    7. Klicken Sie auf **Verbindung testen**, um die Verbindung zu testen.
    8. Klicken Sie auf **Speichern**, um den verknüpften Dienst zu speichern. 
    
        ![Neuer mit Azure SQL-Datenbank verknüpfter Dienst](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

26. Wählen Sie unter **Tabelle** die Option **[dbo].[emp]** aus. 

    ![Auswählen der Tabelle „emp“](./media/tutorial-copy-data-portal/select-emp-table.png)
27. Wechseln Sie zur Registerkarte **Schema**, und klicken Sie auf „Schema importieren“. 

    ![Importieren des Zielschemas](./media/tutorial-copy-data-portal/import-destination-schema.png)
28. Klicken Sie auf die Spalte **ID** und anschließend auf **Löschen**. Da es sich bei der Spalte „ID“ um eine Identitätsspalte in SQL-Datenbank handelt, muss die Copy-Aktivität keine Daten in diese Spalte einfügen.

    ![Löschen der ID-Spalte](./media/tutorial-copy-data-portal/delete-id-column.png)
30. Wechseln Sie zur Registerkarte mit der **Pipeline**, und überprüfen Sie, ob für **Senkendataset** die Option **OutputSqlDataset** ausgewählt ist.

    ![Pipelineregisterkarte](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
31. Wechseln Sie unten im Fenster „Eigenschaften“ zur Registerkarte **Mapping**, und klicken Sie auf **Import Schemas** (Schemas importieren). Beachten Sie, dass die erste und die zweite Spalte in der Quelldatei den Feldern **FirstName** und **LastName** in der SQL-Datenbank zugeordnet werden.

    ![Zuordnen von Schemas](./media/tutorial-copy-data-portal/map-schemas.png)
33. Klicken Sie zum Überprüfen der Pipeline auf die Schaltfläche **Überprüfen**. Klicken Sie auf **>>** (Pfeil nach rechts), um das Überprüfungsfenster zu schließen.

    ![Pipelineüberprüfungsausgabe](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
34. Klicken Sie in der rechten Ecke auf die Schaltfläche **Code**. Sie sehen den JSON-Code, der der Pipeline zugeordnet ist. 

    ![Schaltfläche „Code“](./media/tutorial-copy-data-portal/code-button.png)
35. Der JSON-Code ähnelt folgendem Codeausschnitt:  

    ```json
    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToSql",
                    "type": "Copy",
                    "dependsOn": [],
                    "policy": {
                        "timeout": "7.00:00:00",
                        "retry": 0,
                        "retryIntervalInSeconds": 20
                    },
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "SqlSink",
                            "writeBatchSize": 10000
                        },
                        "enableStaging": false,
                        "parallelCopies": 0,
                        "cloudDataMovementUnits": 0,
                        "translator": {
                            "type": "TabularTranslator",
                            "columnMappings": "Prop_0: FirstName, Prop_1: LastName"
                        }
                    },
                    "inputs": [
                        {
                            "referenceName": "SourceBlobDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "OutputSqlDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ]
                }
            ]
        }
    }
    ```

## <a name="test-run-the-pipeline"></a>Ausführen eines Testlaufs für die Pipeline
Sie können vor dem Veröffentlichen von Artefakten (verknüpfte Dienste, Datasets und Pipeline) in Data Factory oder Ihrem eigenen VSTS-Git-Repository einen Testlauf für eine Pipeline ausführen. 

1. Klicken Sie zum Ausführen eines Testlaufs für die Pipeline auf der Symbolleiste auf **Testlauf**. Der Status der Pipelineausführung wird unten im Fenster auf der Registerkarte **Ausgabe** angezeigt. 

    ![Schaltfläche „Testlauf“](./media/tutorial-copy-data-portal/test-run-output.png)
2. Stellen Sie sicher, dass die Daten aus der Quelldatei in die Ziel-SQL-Datenbank eingefügt werden. 

    ![Überprüfen der SQL-Ausgabe](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. Klicken Sie im linken Bereich auf **Veröffentlichen**. Mit dieser Aktion werden erstellte Entitäten (verknüpfte Dienste, Datasets und Pipeline) in Azure Data Factory veröffentlicht.

    ![Schaltfläche "Veröffentlichen"](./media/tutorial-copy-data-portal/publish-button.png)
4. Warten Sie, bis die Meldung **Erfolgreich veröffentlicht** angezeigt wird. Damit Benachrichtigungsmeldungen angezeigt werden, klicken Sie auf der linken Seitenleiste auf die Registerkarte **Benachrichtigungen anzeigen**. Schließen Sie das Benachrichtigungsfenster, indem Sie auf das **X** klicken.

    ![Anzeigen von Benachrichtigungen](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Konfigurieren des Coderepositorys
Sie können den mit Ihren Data Factory-Artefakten verknüpften Code in einem VSTS-Coderepository (Visual Studio Team System) veröffentlichen. In diesem Schritt erstellen Sie das Coderepository. 

Falls Sie das VSTS-Coderepository nicht verwenden möchten, können Sie diesen Schritt überspringen und die Veröffentlichung in Data Factory wie im vorherigen Schritt fortsetzen. 

1. Klicken Sie in der linken Ecke auf **Data Factory** oder auf den Pfeil nach unten neben dieser Option und dann auf **Configure Code Repository** (Coderepository konfigurieren). 

    ![Schaltfläche „Code“](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. Führen Sie auf der Seite **Repositoryeinstellungen** die folgenden Schritte aus: 
    1. Wählen Sie im Feld **Repositorytyp** die Option **Visual Studio Team Services Git** aus.
    2. Wählen Sie im Feld **Visual Studio Team Services-Konto** Ihr VSTS-Konto aus.
    3. Wählen Sie im Feld **Projektname** ein Projekt in Ihrem VSTS-Konto aus.
    4. Geben Sie als **Name des Git-Repositorys**, das Ihrer Data Factory zugeordnet werden soll, **Tutorial2** ein. 
    5. Vergewissern Sie sich, dass die Option **Import existing Data Factory resources to repository** (Vorhandene Data Factory-Ressourcen in Repository importieren) aktiviert ist. 
    6. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern. 

        ![Repositoryeinstellungen](./media/tutorial-copy-data-portal/repository-settings.png)
3. Vergewissern Sie sich, dass als Repository **VSTS Git** ausgewählt ist.

    ![VSTS Git ausgewählt](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. Navigieren Sie auf einer separaten Registerkarte im Browser zum Repository **Tutorial2**. Sie sehen zwei Branches: **master** und **adf_publish**.

    ![Branches „master“ und „adf_publish“](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Überprüfen Sie, ob sich die **JSON-Dateien** für die Data Factory-Entitäten im Branch **master** befinden.

    ![Dateien im Masterbranch](./media/tutorial-copy-data-portal/master-branch-files.png)
6. Stellen Sie sicher, dass die **JSON-Dateien** noch nicht im Branch **adf_publish** sind. 

    ![Dateien im Branch „adf_publish“](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. Fügen Sie eine **Beschreibung** für die **Pipeline** hinzu, und klicken Sie auf der Symbolleiste auf die Schaltfläche **Speichern**. 

    ![Hinzufügen einer Beschreibung für die Pipeline](./media/tutorial-copy-data-portal/pipeline-description.png)
8. Nun sollte ein **Branch** mit Ihrem Benutzernamen im Repository **Tutorial2** angezeigt werden. Ihre Änderung wurde in Ihrem eigenen Branch, nicht im Masterbranch vorgenommen. Sie können nur Entitäten aus dem Masterbranch veröffentlichen.

    ![Ihr Branch](./media/tutorial-copy-data-portal/your-branch.png)
9. Zeigen Sie auf die Schaltfläche **Synchronisieren** (klicken Sie noch nicht darauf), aktivieren Sie die Option **Commit Changes** (Änderungen committen), und klicken Sie dann auf die Schaltfläche **Synchronisieren**, um Ihre Änderungen mit dem **master**-Branch zu synchronisieren. 

    ![Committen und Synchronisieren Ihrer Änderungen](./media/tutorial-copy-data-portal/commit-and-sync.png)
9. Führen Sie im Fenster „Sync your changes“ (Änderungen synchronisieren) die folgenden Aktionen aus: 

    1. Überprüfen Sie, ob **CopyPipeline** in der aktualisierten Pipelineliste angezeigt wird.
    2. Vergewissern Sie sich, dass **Publish changes after sync** (Änderungen nach der Synchronisierung veröffentlichen) aktiviert ist. Wenn Sie diese Option deaktivieren, synchronisieren Sie nur die Änderungen in Ihrem Branch mit dem Masterbranch, veröffentlichen sie aber nicht im Data Factory-Dienst. Sie können sie später mithilfe der Schaltfläche **Veröffentlichen** veröffentlichen. Wenn Sie diese Option aktivieren, werden die Änderungen zuerst mit dem Masterbranch synchronisiert und anschließend im Data Factory-Dienst veröffentlicht.
    3. Klicken Sie auf **Synchronisieren**. 

    ![Fenster „Sync your changes“ (Änderungen synchronisieren)](./media/tutorial-copy-data-portal/sync-your-changes.png)
10. Nun werden Dateien im Branch **adf_publish** des Repositorys **Tutorial2** angezeigt. In diesem Branch befindet sich auch die Azure Resource Manager-Vorlage für Ihre Data Factory-Lösung.  

    ![Dateien im Branch „adf_publish“](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>Manuelles Auslösen der Pipeline
In diesem Schritt lösen Sie die im vorherigen Schritt veröffentlichte Pipeline manuell aus. 

1. Klicken Sie auf der Symbolleiste auf **Trigger** und dann auf **Trigger Now** (Jetzt auslösen). 

    ![Menü „Trigger Now“ (Jetzt auslösen)](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Sie sehen eine Pipelineausführung, die von einem manuellen Trigger ausgelöst wird. Sie können über Links in der Spalte „Aktionen“ Aktivitätsdetails anzeigen und die Pipeline erneut ausführen.

    ![Überwachen der Pipelineausführung](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. Wenn Sie mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf **View Activity Runs** (Aktivitätsausführungen anzeigen). Da in diesem Beispiel nur eine Aktivität vorhanden ist, wird in der Liste nur ein Eintrag angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie auf den Link **Details** (Brillensymbol) in der Spalte **Aktionen**. Klicken Sie oben auf **Pipelines**, um zurück zur Ansicht **Pipelineausführungen** zu wechseln. Klicken Sie zum Aktualisieren der Ansicht auf **Aktualisieren**.

    ![Anzeigen der Aktivitätsausführungen](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. Stellen Sie sicher, dass mindestens zwei weitere Zeilen zur Tabelle **emp** in der Azure SQL-Datenbank hinzugefügt werden. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Auslösen der Pipeline nach einem Zeitplan
In diesem Zeitplan erstellen Sie einen Planer-Trigger für die Pipeline. Der Trigger führt die Pipeline nach dem angegebenen Zeitplan (stündlich, täglich usw.) aus. In diesem Beispiel legen Sie fest, dass der Trigger bis zur angegebenen Endzeit (Datum und Uhrzeit) minütlich ausgeführt wird. 

1. Wechseln Sie im linken Bereich zur Registerkarte **Bearbeiten**. 

    ![Registerkarte „Bearbeiten“](./media/tutorial-copy-data-portal/edit-tab.png)
2. Klicken Sie auf **Trigger**, und wählen Sie **Neu/Bearbeiten**. Ist die Pipeline nicht aktiv, wechseln Sie zur Pipeline. 

    ![Triggermenü „Neu/Bearbeiten“](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. Klicken Sie im Fenster **Add Triggers** (Trigger hinzufügen) auf **Choose trigger...** (Trigger auswählen...) und dann auf **+ Neu**. 

    ![Hinzufügen von Triggern – neuer Trigger](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. Führen Sie im Fenster **Neuer Trigger** die folgenden Schritte aus: 

    1. Legen Sie für **Name** den Namen **RunEveryMinute** fest.
    2. Wählen Sie für **Ende** die Option **On Date** (Am) aus. 
    3. Klicken Sie für **End On** (Ende am) auf die Dropdownliste.
    4. Wählen Sie das **aktuelle Datum** aus. Standardmäßig wird als Enddatum der nächste Tag festgelegt. 
    5. Legen Sie den Teil für **Minuten** auf einen Wert fest, der einige Minuten in der Zukunft liegt. Der Trigger wird erst nach dem Veröffentlichen der Änderungen aktiviert. Wenn nur einige Minuten dazwischen liegen und Sie die Änderungen bis zur angegebenen Zeit nicht veröffentlicht haben, wird daher kein ausgeführter Trigger angezeigt.  
    6. Klicken Sie auf **Anwenden**. 

        ![Festlegen von Triggereigenschaften](./media/tutorial-copy-data-portal/set-trigger-properties.png)
    7. Aktivieren Sie die Option **Aktiviert**. Sie können die Option deaktivieren und später über das Kontrollkästchen aktivieren.
    8. Klicken Sie auf **Weiter**.


        ![Trigger aktiviert – Weiter](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Für jede Pipelineausführung fallen Gebühren an. Legen Sie daher ein geeignetes Enddatum fest. 
6. Überprüfen Sie auf der Seite **Trigger Run Parameters** (Parameter für die Triggerausführung) die Warnung, und klicken Sie auf **Fertig stellen**. Die Pipeline in diesem Beispiel akzeptiert keine Parameter. 

    ![Pipelineparameter](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
7. Klicken Sie auf **Veröffentlichen**, um die Änderungen im Repository zu veröffentlichen. Der Trigger wird erst aktiviert, wenn die Veröffentlichung erfolgreich war. 

    ![Veröffentlichen des Triggers](./media/tutorial-copy-data-portal/publish-trigger.png) 
8. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**, um die ausgelösten Pipelineausführungen anzuzeigen. 

    ![Ausgelöste Pipelineausführungen](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
9. Klicken Sie auf „Pipelineausführungen“ und dann auf „Triggerausführungen“, um von der Ansicht mit den Pipelineausführungen zur Ansicht mit den Triggerausführungen zu wechseln.
    
    ![Menü „Triggerausführungen“](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
10. Die Triggerausführungen werden in einer Liste angezeigt. 

    ![Liste der Triggerausführungen](./media/tutorial-copy-data-portal/trigger-runs-list.png)
11. Stellen Sie sicher, dass bis zur angegebenen Endzeit zwei Zeilen pro Minute (für jede Pipelineausführung) in die Tabelle **emp** eingefügt werden. 

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen Sie eine Pipeline mit einer Copy-Aktivität.
> * Ausführen eines Testlaufs für die Pipeline
> * Manuelles Auslösen der Pipeline
> * Auslösen der Pipeline nach einem Zeitplan
> * Überwachen der Pipeline- und Aktivitätsausführungen.


Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Daten von einem lokalen Speicherort in die Cloud kopieren: 

> [!div class="nextstepaction"]
>[Kopieren von Daten aus lokalen Quellen in die Cloud](tutorial-hybrid-copy-portal.md)

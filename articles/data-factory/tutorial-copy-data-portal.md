---
title: Verwenden des Azure-Portals zum Erstellen einer Data Factory-Pipeline | Microsoft-Dokumentation
description: Dieses Tutorial enthält detaillierte Anweisungen zur Erstellung einer Data Factory mit Pipeline mithilfe des Azure-Portals. Die Pipeline kopiert mit der Copy-Aktivität Daten aus Azure Blob Storage in eine SQL-Datenbank.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: jingwang
ms.openlocfilehash: a4041d7d2cc9d3fd42a541c316d8d739b3ab733d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57539869"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-azure-data-factory"></a>Kopieren von Daten aus Azure Blob Storage in eine SQL-Datenbank mithilfe von Azure Data Factory
In diesem Tutorial erstellen Sie eine Data Factory über die Azure Data Factory-Benutzeroberfläche (User Interface, UI). Die Pipeline in dieser Data Factory kopiert Daten aus Azure Blob Storage in eine SQL-Datenbank. Das Konfigurationsmuster in diesem Tutorial gilt für Kopiervorgänge aus einem dateibasierten Datenspeicher in einen relationalen Datenspeicher. Eine Liste der Datenspeicher, die als Quellen und Senken unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> - Falls Sie noch nicht mit Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](introduction.md) zu lesen.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mit einer Copy-Aktivität
> * Ausführen eines Testlaufs für die Pipeline
> * Manuelles Auslösen der Pipeline
> * Auslösen der Pipeline nach einem Zeitplan
> * Überwachen der Pipeline- und Aktivitätsausführungen.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Abonnement**. Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure-Speicherkonto**. Sie verwenden Blob Storage als *Quelldatenspeicher*. Wenn Sie kein Speicherkonto besitzen, finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-quickstart-create-account.md) Schritte zum Erstellen eines solchen Kontos.
* **Azure SQL-Datenbank**. Sie verwenden die Datenbank als *Senkendatenspeicher*. Wenn Sie keine SQL-Datenbank besitzen, finden Sie Schritte zum Erstellen einer solchen Datenbank unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Erstellen eines Blobs und einer SQL-Tabelle

Nun bereiten Sie Ihre Blob Storage-Instanz und SQL-Datenbank durch Ausführen der folgenden Schritte auf das Tutorial vor:

#### <a name="create-a-source-blob"></a>Erstellen eines Quellblobs

1. Starten Sie den Editor. Kopieren Sie den folgenden Text, und speichern Sie ihn als **emp.txt**-Datei auf einem Datenträger:

    ```
    John,Doe
    Jane,Doe
    ```

1. Erstellen Sie in Blob Storage einen Container mit dem Namen **adftutorial**. Erstellen Sie einen Ordner namens **input** in diesem Container. Laden Sie anschließend die Datei **emp.txt** in den Ordner **input** hoch. Verwenden Sie für diese Aufgaben das Azure-Portal oder Tools wie [Azure Storage-Explorer](https://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Erstellen einer SQL-Senkentabelle

1. Verwenden Sie das folgende SQL-Skript, um die Tabelle **dbo.emp** in Ihrer SQL-Datenbank zu erstellen:

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

1. Gewähren Sie Azure-Diensten den Zugriff auf SQL Server. Stellen Sie sicher, dass die Einstellung **Zugriff auf Azure-Dienste erlauben** für Ihre SQL Server-Instanz **aktiviert** ist, damit Data Factory Daten in diese SQL Server-Instanz schreiben kann. Führen Sie folgende Schritte aus, um diese Einstellung zu überprüfen und zu aktivieren:

    a. Klicken Sie auf der linken Seite auf **Weitere Dienste** > **SQL Server**.

    b. Wählen Sie Ihren Server aus, und klicken Sie unter **EINSTELLUNGEN** auf **Firewall**.

    c. Klicken Sie auf der Seite **Firewalleinstellungen** für **Zugriff auf Azure-Dienste erlauben** auf **EIN**.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
In diesem Schritt erstellen Sie eine Data Factory und starten die Data Factory-Benutzeroberfläche, um eine Pipeline in der Data Factory zu erstellen. 

1. Öffnen Sie **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.
2. Klicken Sie im Menü auf der linken Seite auf **Ressource erstellen** > **Analytics** > **Data Factory**. 
  
   ![Erstellen einer neuen Data Factory](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
3. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 
      
     ![Neue Data Factory](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss *global eindeutig*sein. Sollte für das Feld „Name“ die folgende Fehlermeldung angezeigt werden, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
   ![Fehlermeldung](./media/tutorial-copy-data-portal/name-not-available-error.png)
4. Wählen Sie das **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
5. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
     
    a. Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe.

    b. Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein. 
         
    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md). 
6. Wählen Sie unter **Version** die Option **V2**.
7. Wählen Sie unter **Standort** einen Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die Datenspeicher (etwa Azure Storage und SQL-Datenbank) und Computeeinheiten (etwa Azure HDInsight), die von der Data Factory genutzt werden, können sich in anderen Regionen befinden.
8. Wählen Sie die Option **An Dashboard anheften** aus. 
9. Klicken Sie auf **Erstellen**. 
10. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status **Deploying Data Factory** (Data Factory wird bereitgestellt...): 

    ![Kachel „Deploying Data Factory“ (Data Factory wird bereitgestellt...)](media/tutorial-copy-data-portal/deploying-data-factory.png)
1. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
    ![Data Factory-Startseite](./media/tutorial-copy-data-portal/data-factory-home-page.png)
1. Klicken Sie auf **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten.

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline
In diesem Schritt erstellen Sie eine Pipeline mit einer Copy-Aktivität in der Data Factory. Die Copy-Aktivität kopiert Daten aus Blob Storage in SQL-Datenbank. Im [Schnellstarttutorial](quickstart-create-data-factory-portal.md) haben Sie anhand der folgenden Schritte eine Pipeline erstellt:

1. 1. Erstellen des verknüpften Diensts 
1. Erstellen von Eingabe- und Ausgabedatasets
1. Erstellen einer Pipeline.

In diesem Tutorial beginnen Sie mit dem Erstellen der Pipeline. Verknüpfte Dienste und Datasets erstellen Sie, wenn Sie sie zum Konfigurieren der Pipeline benötigen. 

1. Wählen Sie auf der Seite **Erste Schritte** die Option **Pipeline erstellen** aus. 

   ![Erstellen der Pipeline](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
1. Geben Sie auf der Registerkarte **Allgemein** der Pipeline als **Name** der Pipeline **CopyPipeline** ein.

1. Erweitern Sie in der Toolbox **Aktivitäten** die Kategorie **Move and Transform** (Verschieben und transformieren), und verschieben Sie die Aktivität **Daten kopieren** aus der Toolbox auf die Oberfläche des Pipeline-Designers. Geben Sie unter **Name** den Namen **CopyFromBlobToSql** ein.

    ![Copy-Aktivität](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)

### <a name="configure-source"></a>Konfigurieren der Quelle

1. Wechseln Sie zur Registerkarte **Quelle**. Klicken Sie auf **+ Neu**, um ein Quelldataset zu erstellen. 

1. Wählen Sie im Fenster **Neues Dataset** die Option **Azure Blob Storage**, und klicken Sie dann auf **Fertig stellen**. Da sich die Quelldaten in Blob Storage befinden, wählen Sie **Azure Blob Storage** als Quelldataset aus. 

    ![Speicherauswahl](./media/tutorial-copy-data-portal/select-azure-blob-dataset.png)

1. Eine neue Registerkarte für das Blobdataset wird geöffnet. Geben Sie unten im Fenster **Eigenschaften** auf der Registerkarte **Allgemein** unter **Name** den Namen **SourceBlobDataset** ein.

    ![Datasetname](./media/tutorial-copy-data-portal/dataset-name.png)

1. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **Verbindung**. Klicken Sie neben dem Textfeld **Verknüpfter Dienst** auf **+ Neu**. 

    ![Schaltfläche für neuen verknüpften Dienst](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)

1. Geben Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) als Name **AzureStorageLinkedService** ein, und wählen Sie in der Liste **Speicherkontoname** Ihr Speicherkonto aus. Klicken Sie dann auf **Speichern**, um den verknüpften Dienst bereitzustellen.

    ![Neuer verknüpfter Dienst](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)

1. Nach der Erstellung des verknüpften Diensts werden wieder die Dataseteinstellungen angezeigt. Klicken Sie neben **Dateipfad** auf **Durchsuchen**.

    ![Schaltfläche „Durchsuchen“ für den Dateipfad](./media/tutorial-copy-data-portal/file-browse-button.png)

1. Navigieren Sie zum Ordner **adftutorial/input**, wählen Sie die Datei **emp.txt** aus, und klicken Sie auf **Fertig stellen**. 

    ![Auswählen der Eingabedatei](./media/tutorial-copy-data-portal/select-input-file.png)

1. Vergewissern Sie sich, dass für **Dateiformat** die Option **Textformat** und für **Spaltentrennzeichen** die Option **Komma (`,`)** festgelegt ist. Wenn in der Quelldatei verschiedene Zeilen- und Spaltentrennzeichen verwendet werden, können Sie für das Feld **Dateiformat** auf **Detect Text Format** (Textformat ermitteln) klicken. Das Tool zum Kopieren von Daten ermittelt automatisch Dateiformat und Trennzeichen für Sie. Sie können diese Werte immer noch überschreiben. Wenn Sie auf dieser Seite eine Vorschau der Daten anzeigen möchten, klicken Sie auf **Datenvorschau**.

    ![Ermitteln des Textformats](./media/tutorial-copy-data-portal/detect-text-format.png)

1. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **Schema**, und klicken Sie auf **Schema importieren**. Beachten Sie, dass die Anwendung zwei Spalten in der Quelldatei erkannt hat. Sie importieren das Schema hier, sodass Sie Spalten aus dem Quelldatenspeicher dem Senkendatenspeicher zuordnen können. Falls Sie keine Spalten zuordnen müssen, können Sie diesen Schritt überspringen. Importieren Sie das Schema für dieses Tutorial.

    ![Ermitteln des Quellschemas](./media/tutorial-copy-data-portal/detect-source-schema.png)  

1. Gehen Sie nun zurück zur Pipeline und zur Registerkarte **Quelle**, und überprüfen Sie, ob **SourceBlobDataset** ausgewählt ist. Wenn Sie auf dieser Seite eine Vorschau der Daten anzeigen möchten, klicken Sie auf **Datenvorschau**. 
    
    ![Quelldataset](./media/tutorial-copy-data-portal/source-dataset-selected.png)

### <a name="configure-sink"></a>Konfigurieren der Senke

1. Wechseln Sie zur Registerkarte **Senke**, und klicken Sie auf **+ Neu**, um ein Senkendataset zu erstellen. 

    ![Senkendataset](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
1. Geben Sie im Fenster **Neues Dataset** im Suchfeld „SQL“ ein, um die Connectors zu filtern. Wählen Sie anschließend **Azure SQL-Datenbank** und dann **Fertig stellen** aus. In diesem Tutorial kopieren Sie Daten in eine SQL-Datenbank. 

    ![Auswahl der SQL-Datenbank](./media/tutorial-copy-data-portal/select-azure-sql-dataset.png)
1. Geben Sie im Fenster **Eigenschaften** auf der Registerkarte **Allgemein** unter **Name** den Namen **OutputSqlDataset** ein. 
    
    ![Name des Ausgabedatasets](./media/tutorial-copy-data-portal/output-dataset-name.png)
1. Wechseln Sie zur Registerkarte **Verbindung**, und klicken Sie neben **Verknüpfter Dienst** auf **+ Neu**. Einem verknüpften Dienst muss ein Dataset zugewiesen werden. Der verknüpfte Dienst enthält die Verbindungszeichenfolge, die Data Factory zum Herstellen einer Verbindung mit der SQL-Datenbank zur Laufzeit verwendet. Das Dataset gibt den Container, den Ordner und (optional) die Datei an, in die die Quelldaten kopiert werden. 
    
    ![Verknüpfter Dienst](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
1. Führen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die folgenden Schritte aus: 

    a. Geben Sie unter **Name** den Namen **AzureSqlDatabaseLinkedService** ein.

    b. Wählen Sie unter **Servername** Ihre SQL Server-Instanz aus.

    c. Wählen Sie unter **Datenbankname** Ihre SQL-Datenbank aus.

    d. Geben Sie unter **Benutzername** den Namen des Benutzers ein.

    e. Geben Sie unter **Kennwort** das Kennwort für den Benutzer ein.

    f. Klicken Sie auf **Verbindung testen**, um die Verbindung zu testen.

    g. Klicken Sie auf **Speichern**, um den verknüpften Dienst zu speichern. 
    
    ![Speichern des neuen verknüpften Diensts](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

1. Wählen Sie unter **Tabelle** die Option **[dbo].[emp]** aus. 

    ![Table](./media/tutorial-copy-data-portal/select-emp-table.png)
1. Wechseln Sie zur Registerkarte **Schema**, und klicken Sie auf **Schema importieren**. 

    ![Auswählen von „Schema importieren“](./media/tutorial-copy-data-portal/import-destination-schema.png)
1. Wählen Sie die Spalte **ID** aus, und klicken Sie dann auf **Löschen**. Da es sich bei der Spalte **ID** um eine Identitätsspalte in der SQL-Datenbank handelt, muss die Copy-Aktivität keine Daten in diese Spalte einfügen.

    ![Löschen der ID-Spalte](./media/tutorial-copy-data-portal/delete-id-column.png)
1. Wechseln Sie zur Registerkarte mit der Pipeline, und überprüfen Sie, ob für **Senkendataset** die Option **OutputSqlDataset** ausgewählt ist.

    ![Pipelineregisterkarte](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        

### <a name="configure-mapping"></a>Konfigurieren der Zuordnung

Wechseln Sie unten im Fenster **Eigenschaften** zur Registerkarte **Mapping**, und klicken Sie auf **Import Schemas** (Schemas importieren). Beachten Sie, dass die erste und die zweite Spalte in der Quelldatei den Feldern **FirstName** und **LastName** in der SQL-Datenbank zugeordnet werden.

![Zuordnen von Schemas](./media/tutorial-copy-data-portal/map-schemas.png)

## <a name="validate-the-pipeline"></a>Überprüfen der Pipeline
Klicken Sie auf der Symbolleiste auf **Überprüfen**, um die Pipeline zu überprüfen.
 
Sie können den JSON-Code der Pipeline anzeigen, indem Sie oben rechts auf **Code** klicken.

## <a name="debug-and-publish-the-pipeline"></a>Debuggen und Veröffentlichen der Pipeline
Sie können vor dem Veröffentlichen von Artefakten (verknüpfte Dienste, Datasets und Pipeline) in Data Factory oder Ihrem eigenen Azure DevOps-Git-Repository eine Pipeline debuggen. 

1. Klicken Sie auf der Symbolleiste auf **Debuggen**, um die Pipeline zu debuggen. Der Status der Pipelineausführung wird unten im Fenster auf der Registerkarte **Ausgabe** angezeigt. 

1. Wenn die Pipeline ausgeführt wird, klicken Sie auf der oberen Symbolleiste auf **Alle veröffentlichen**. Mit dieser Aktion werden erstellte Entitäten (Datasets und Pipelines) in Data Factory veröffentlicht.

    ![Veröffentlichen](./media/tutorial-copy-data-portal/publish-button.png)

1. Warten Sie, bis die Meldung **Erfolgreich veröffentlicht** angezeigt wird. Damit Benachrichtigungsmeldungen angezeigt werden, klicken Sie oben rechts auf **Benachrichtigungen anzeigen** (Schaltfläche mit Glocke). 

## <a name="trigger-the-pipeline-manually"></a>Manuelles Auslösen der Pipeline
In diesem Schritt lösen Sie die im vorherigen Schritt veröffentlichte Pipeline manuell aus. 

1. Wählen Sie in der Symbolleiste die Option **Trigger** und dann **Trigger Now** (Jetzt auslösen). Klicken Sie auf der Seite **Pipeline Run** (Pipelineausführung) auf **Fertig stellen**.  

1. Wechseln Sie links zur Registerkarte **Überwachen**. Sie sehen eine Pipelineausführung, die von einem manuellen Trigger ausgelöst wird. Sie können über Links in der Spalte **Aktionen** Aktivitätsdetails anzeigen und die Pipeline erneut ausführen.

    ![Überwachen der Pipelineausführungen](./media/tutorial-copy-data-portal/monitor-pipeline.png)
1. Klicken Sie in der Spalte **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen), um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Da in diesem Beispiel nur eine Aktivität vorhanden ist, wird in der Liste nur ein Eintrag angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie auf den Link **Details** (Brillensymbol) in der Spalte **Aktionen**. Klicken Sie oben auf **Pipelines**, um zurück zur Ansicht **Pipelineausführungen** zu wechseln. Klicken Sie zum Aktualisieren der Ansicht auf **Aktualisieren**.

    ![Überwachung der Aktivitätsausführungen](./media/tutorial-copy-data-portal/view-activity-runs.png)
1. Stellen Sie sicher, dass zwei weitere Zeilen zur Tabelle **emp** in der SQL-Datenbank hinzugefügt werden. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Auslösen der Pipeline nach einem Zeitplan
In diesem Zeitplan erstellen Sie einen Zeitplantrigger für die Pipeline. Der Trigger führt die Pipeline nach dem angegebenen Zeitplan aus, etwa stündlich oder täglich. In diesem Beispiel legen Sie fest, dass der Trigger bis zur angegebenen Endzeit (Datum und Uhrzeit) minütlich ausgeführt wird. 

1. Navigieren Sie auf der linken Seite zur Registerkarte **Autor** (oberhalb der Registerkarte „Überwachen“). 

1. Navigieren Sie zu Ihrer Pipeline, klicken Sie auf der Symbolleiste auf **Trigger**, und wählen Sie **Neu/Bearbeiten**. 

1. Klicken Sie im Fenster **Add Triggers** (Trigger hinzufügen) auf **Choose trigger** (Trigger auswählen) und dann auf **Neu**. 

    ![Schaltfläche "Neu"](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
1. Führen Sie im Fenster **Neuer Trigger** die folgenden Schritte aus: 

    a. Geben Sie unter **Name** den Namen **RunEveryMinute** ein.

    b. Wählen Sie unter **Ende** die Option **On Date** (Am) aus.

    c. Klicken Sie unter **End On** (Ende am) auf die Dropdownliste.

    d. Wählen Sie die Option für das **aktuelle Datum**. Standardmäßig wird als Enddatum der nächste Tag festgelegt.

    e. Legen Sie den Teil für **Minuten** auf einen Wert fest, der einige Minuten in der Zukunft liegt. Der Trigger wird erst nach dem Veröffentlichen der Änderungen aktiviert. Wenn nur einige Minuten dazwischen liegen und Sie die Änderungen bis zur angegebenen Zeit nicht veröffentlicht haben, wird kein ausgeführter Trigger angezeigt.

    f. Wählen Sie **Übernehmen**. 

    ![Triggereigenschaften](./media/tutorial-copy-data-portal/set-trigger-properties.png)

    g. Aktivieren Sie die Option **Aktiviert**. Sie können die Option deaktivieren und später über das Kontrollkästchen aktivieren.

    h. Klicken Sie auf **Weiter**.

    ![Schaltfläche „Aktiviert“](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Da für jede Pipelineausführung Gebühren anfallen, legen Sie ein geeignetes Enddatum fest. 
1. Überprüfen Sie auf der Seite **Trigger Run Parameters** (Ausführungsparameter für Trigger) die Warnung, und wählen Sie **Fertig stellen** aus. Die Pipeline in diesem Beispiel akzeptiert keine Parameter. 

    ![Triggerausführungsparameter](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)

1. Klicken Sie auf **Alle veröffentlichen**, um die Änderung zu veröffentlichen. 

1. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**, um die ausgelösten Pipelineausführungen anzuzeigen. 

    ![Ausgelöste Pipelineausführungen](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
1. Klicken Sie auf **Pipelineausführungen** und dann auf **Triggerausführungen**, um von der Ansicht **Pipelineausführungen** zur Ansicht **Triggerausführungen** zu wechseln.
    
    ![Triggerausführungen](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
1. Die Triggerausführungen werden in einer Liste angezeigt. 

    ![Liste der Triggerausführungen](./media/tutorial-copy-data-portal/trigger-runs-list.png)
1. Stellen Sie sicher, dass bis zur angegebenen Endzeit zwei Zeilen pro Minute (für jede Pipelineausführung) in die Tabelle **emp** eingefügt werden. 

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in Blob Storage von einem Speicherort in einen anderen. Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mit einer Copy-Aktivität
> * Ausführen eines Testlaufs für die Pipeline
> * Manuelles Auslösen der Pipeline
> * Auslösen der Pipeline nach einem Zeitplan
> * Überwachen der Pipeline- und Aktivitätsausführungen.


Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Daten von einem lokalen Speicherort in die Cloud kopieren: 

> [!div class="nextstepaction"]
>[Tutorial: Kopieren von Daten aus einer lokalen SQL Server-Datenbank nach Azure Blob Storage](tutorial-hybrid-copy-portal.md)

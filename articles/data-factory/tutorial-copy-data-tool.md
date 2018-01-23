---
title: Kopieren von Daten mithilfe des Azure-Tools zum Kopieren von Daten | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory-Instanz, und verwenden Sie dann das Tool zum Kopieren von Daten, um Daten aus Azure Blob Storage in Azure SQL-Datenbank zu kopieren.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: a8c7035ebf462bb168147e9d8eb60742333ce8b8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-copy-data-tool"></a>Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank mithilfe des Tools zum Kopieren von Daten
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1: allgemein verfügbar](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Version 2 – Vorschauversion](tutorial-copy-data-tool.md)

In diesem Tutorial verwenden Sie das Azure-Portal, um eine Data Factory zu erstellen. Anschließend erstellen Sie mithilfe des Tools zum Kopieren von Daten eine Pipeline, die Daten aus einem Azure-Blobspeicher in eine Azure SQL-Datenbank kopiert. 

> [!NOTE]
> - Wenn Sie noch nicht mit Azure Data Factory vertraut sind, sollten Sie die [Einführung in Azure Data Factory](introduction.md) lesen.
>
> - Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten
> * Überwachen der Pipeline- und Aktivitätsausführungen.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure Storage-Konto**. Sie verwenden den Blob Storage als **Quelldatenspeicher**. Wenn Sie kein Azure Storage-Konto besitzen, finden Sie im Artikel [Erstellen eines Speicherkontos](../storage/common/storage-create-storage-account.md#create-a-storage-account) Schritte zum Erstellen eines solchen Kontos.
* **Azure SQL-Datenbank**. Sie verwenden die Datenbank als **Senkendatenspeicher**. Wenn Sie noch nicht über eine Azure SQL-Datenbank verfügen, finden Sie im Artikel [Erstellen einer Azure SQL-Datenbank](../sql-database/sql-database-get-started-portal.md) die Schritte zum Erstellen einer solchen Datenbank.

### <a name="create-a-blob-and-a-sql-table"></a>Erstellen eines Blobs und einer SQL-Tabelle

Bereiten Sie jetzt Ihr Azure-Blob und Ihre Azure SQL-Datenbank für das Tutorial vor, indem Sie die folgenden Schritte ausführen:

#### <a name="create-a-source-blob"></a>Erstellen eines Quellblobs

1. Starten Sie den Editor. Kopieren Sie den folgenden Text, und speichern Sie ihn als **inputEmp.txt**-Datei auf einem Datenträger.

    ```
    John|Doe
    Jane|Doe
    ```

2. Verwenden Sie Tools wie [Azure Storage-Explorer](http://storageexplorer.com/), um den Container **adfv2tutorial** zu erstellen und die Datei **inputEmp.txt** in den Container hochzuladen.

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

2. Gewähren Sie Azure-Diensten den Zugriff auf SQL Server. Stellen Sie sicher, dass die Einstellung **Zugriff auf Azure-Dienste erlauben** für Ihre Azure SQL Server-Instanz aktiviert ist, damit der Data Factory-Dienst Daten in diese Instanz schreiben kann. Führen Sie folgende Schritte aus, um diese Einstellung zu überprüfen und zu aktivieren:

    1. Klicken Sie links auf den Hub **Weitere Dienste** und dann auf **SQL Server**.
    2. Wählen Sie Ihren Server aus, und klicken Sie unter **EINSTELLUNGEN** auf **Firewall**.
    3. Klicken Sie auf der Seite **Firewalleinstellungen** für **Zugriff auf Azure-Dienste erlauben** auf **EIN**.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **ADFTutorialDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte der folgende Fehler für das Feld „Name“ auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
     ![Seite „Neue Data Factory“](./media/tutorial-copy-data-tool/name-not-available-error.png)
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

    ![Kachel „Die Data Factory wird bereitgestellt“](media/tutorial-copy-data-tool/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in dieser Abbildung angezeigt:
   
   ![Data Factory-Startseite](./media/tutorial-copy-data-tool/data-factory-home-page.png)
10. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten

1. Klicken Sie auf der Seite mit den ersten Schritten auf die Kachel **Daten kopieren**, um das Tool zum Kopieren von Daten zu starten. 

   ![Kachel für das Tool zum Kopieren von Daten](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Geben Sie auf der Seite **Eigenschaften** des Tools zum Kopieren von Daten als **Aufgabenname** den Namen **CopyFromBlobToSqlPipeline** ein, und klicken Sie auf **Weiter**. Über die Data Factory-Benutzeroberfläche wird eine Pipeline mit dem Namen erstellt, den Sie als Aufgabennamen angeben. 

    ![Tool zum Kopieren von Daten – Seite „Eigenschaften“](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. Wählen Sie auf der Seite **Quelldatenspeicher** die Option **Azure Blob Storage** aus, und klicken Sie auf **Weiter**. Die Quelldaten befinden sich in einem Azure-Blobspeicher. 

    ![Seite „Quelldatenspeicher“](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. Führen Sie auf der Seite **Azure Blob Storage-Konto angeben** die folgenden Schritte aus: 
    1. Geben Sie im Feld **Verbindungsname** den Namen **AzureStorageLinkedService** ein.
    2. Wählen Sie in der Dropdownliste **Speicherkontoname** Ihr Speicherkonto aus.
    3. Klicken Sie auf **Weiter**.
 

        ![Blob Storage-Konto angeben](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

        Ein verknüpfter Dienst verbindet einen Datenspeicher oder einen Computedienst mit der Data Factory. In diesem Fall erstellen Sie einen mit Azure Storage verknüpften Dienst, der Ihr Azure Storage-Konto mit dem Datenspeicher verbindet. Der verknüpfte Dienst enthält die Verbindungsinformationen, die der Data Factory-Dienst zum Herstellen einer Verbindung mit dem Blobspeicher zur Laufzeit verwendet. Das Dataset gibt den Container, den Ordner und (optional) die Datei an, die die Quelldaten enthält. 
5. Führen Sie auf der Seite **Choose the input file or folder** (Eingabedatei oder -ordner auswählen) die folgenden Schritte aus:
    
    1. Navigieren Sie zum Ordner **adfv2tutorial/input**.
    2. Wählen Sie **inputEmp.txt** aus.
    3. Klicken Sie auf **Auswählen**. Alternativ können Sie auf **inputEmp.txt** doppelklicken. 
    4. Klicken Sie auf **Weiter**.
 

    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/tutorial-copy-data-tool/choose-input-file-folder.png)
6. Auf der Seite **Dateiformateinstellungen** können Sie sehen, dass das Tool die Spalten- und Zeilentrennzeichen automatisch erkennt. Klicken Sie auf **Weiter**. Auf dieser Seite können Sie außerdem eine Vorschau der Daten und das Schema der Eingabedaten anzeigen. 

    ![Seite „Dateiformateinstellungen“](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. Wählen Sie auf der Seite **Zieldatenspeicher** die Option **Azure SQL-Datenbank**, und klicken Sie auf **Weiter**. 

    ![Seite „Zieldatenspeicher“](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. Führen Sie auf der Seite **Azure SQL-Datenbank angeben** die folgenden Schritte aus: 

    1. Geben Sie im Feld **Verbindungsname** den Namen **AzureSqlDatabaseLinkedService** ein. 
    2. Wählen Sie unter **Servername** Ihre Azure SQL Server-Instanz aus.
    3. Wählen Sie unter **Datenbankname** Ihre Azure SQL-Datenbank aus.
    4. Geben Sie im Feld **Benutzername** den Namen des Benutzers ein.
    5. Geben Sie im Feld **Kennwort** das Kennwort des Benutzers ein.
    6. Klicken Sie auf **Weiter**. 

        ![Angeben der Azure SQL-Datenbank](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

        Einem verknüpften Dienst muss ein Dataset zugewiesen werden. Der verknüpfte Dienst enthält die Verbindungszeichenfolge, die der Data Factory-Dienst zum Herstellen einer Verbindung mit der Azure SQL-Datenbank zur Laufzeit verwendet. Das Dataset gibt den Container, den Ordner und (optional) die Datei an, in die die Quelldaten kopiert werden.
9.  Wählen Sie auf der Seite **Tabellenmapping** die Option **[dbo].[emp]** aus, und klicken Sie auf **Weiter**. 

    ![Seite „Tabellenmapping“](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. Auf der Seite **Schemazuordnung** sehen Sie, dass die erste und zweite Spalte in der Eingabedatei den Spalten **FirstName** und **LastName** der Tabelle **emp** zugeordnet werden. 

    ![Seite „Schemazuordnung“](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**. 

    ![Seite "Einstellungen"](./media/tutorial-copy-data-tool/settings-page.png)
12. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie auf **Weiter**.

    ![Seite „Zusammenfassung“](./media/tutorial-copy-data-tool/summary-page.png)
13. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline (Aufgabe) zu überwachen.

    ![Seite „Bereitstellung“](./media/tutorial-copy-data-tool/deployment-page.png)
14. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. In der Spalte **Aktionen** werden Links zum Anzeigen von Aktivitätsausführungsdetails und zum erneuten Ausführen der Pipeline angezeigt. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. 

    ![Überwachen der Pipelineausführungen](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. Wenn Sie mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf **View Activity Runs** (Aktivitätsausführungen anzeigen). Da die Pipeline nur eine Aktivität (Copy-Aktivität) enthält, wird nur ein Eintrag angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie auf den Link **Details** (Brillensymbol) in der Spalte **Aktionen**. Klicken Sie auf **Pipelines**, um zurück zur Ansicht der Pipelineausführungen zu wechseln. Klicken Sie zum Aktualisieren der Ansicht auf **Aktualisieren**. 

    ![Überwachung der Aktivitätsausführungen](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Klicken Sie im linken Bereich auf die Registerkarte **Bearbeiten**, um in den Editor-Modus zu wechseln. Sie können die vom Tool erstellten verknüpften Dienste, Datasets und Pipelines mit dem Editor aktualisieren. Klicken Sie auf **Code**, um den JSON-Code für die im Editor geöffnete Entität anzuzeigen. Ausführliche Informationen zum Bearbeiten dieser Entitäten über die Data Factory-Benutzeroberfläche finden Sie in der [Azure-Portal-Version dieses Tutorials](tutorial-copy-data-portal.md).

    ![Registerkarte „Editor“](./media/tutorial-copy-data-tool/edit-tab.png)
17. Stellen Sie sicher, dass die Daten in die Tabelle **emp** in Ihrer Azure SQL-Datenbank eingefügt werden. 

    ![Überprüfen der SQL-Ausgabe](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten aus einem Azure-Blobspeicher in eine Azure SQL-Datenbank. Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten
> * Überwachen der Pipeline- und Aktivitätsausführungen.

Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Daten von einem lokalen Speicherort in die Cloud kopieren: 

> [!div class="nextstepaction"]
>[Kopieren von Daten aus lokalen Quellen in die Cloud](tutorial-hybrid-copy-data-tool.md)
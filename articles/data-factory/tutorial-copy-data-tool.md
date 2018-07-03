---
title: Kopieren von Daten mithilfe des Azure-Tools zum Kopieren von Daten | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory-Instanz, und verwenden Sie dann das Tool zum Kopieren von Daten, um Daten aus Azure Blob Storage in eine SQL-Datenbank zu kopieren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 06/21/2018
ms.author: jingwang
ms.openlocfilehash: 504f025edef79b310249c29665198438a326881a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052000"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-the-copy-data-tool"></a>Kopieren von Daten aus Azure Blob Storage in eine SQL-Datenbank mithilfe des Tools zum Kopieren von Daten
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Version 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Aktuelle Version](tutorial-copy-data-tool.md)

In diesem Tutorial verwenden Sie das Azure-Portal, um eine Data Factory zu erstellen. Anschließend erstellen Sie mithilfe des Tools zum Kopieren von Daten eine Pipeline, die Daten aus Azure Blob Storage in eine SQL-Datenbank kopiert. 

> [!NOTE]
> Falls Sie noch nicht mit Azure Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](introduction.md) zu lesen.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten
> * Überwachen der Pipeline- und Aktivitätsausführungen.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement:** Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure-Speicherkonto:** Verwenden Sie Blob Storage als _Quelldatenspeicher_. Falls Sie noch nicht über ein Azure-Speicherkonto verfügen, finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md#create-a-storage-account) Anweisungen zur Erstellung.
* **Azure SQL-Datenbank**: Verwenden Sie eine SQL-Datenbank als _Senkendatenspeicher_. Wenn Sie keine SQL-Datenbank besitzen, lesen Sie die Anweisungen unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Erstellen eines Blobs und einer SQL-Tabelle

Bereiten Sie Ihre Blob Storage-Instanz und SQL-Datenbank durch Ausführen der folgenden Schritte auf das Tutorial vor:

#### <a name="create-a-source-blob"></a>Erstellen eines Quellblobs

1. Starten Sie **Editor**. Kopieren Sie den folgenden Text, und speichern Sie ihn in einer Datei namens **inputEmp.txt** auf einem Datenträger:

    ```
    John|Doe
    Jane|Doe
    ```

2. Erstellen Sie einen Container namens **adfv2tutorial**, und laden Sie die Datei „inputEmp.txt“ in den Container hoch. Sie können für diese Aufgaben verschiedene Tools verwenden, etwa [Azure Storage-Explorer](http://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Erstellen einer SQL-Senkentabelle

1. Verwenden Sie das folgende SQL-Skript, um eine Tabelle namens **dbo.emp** in Ihrer SQL-Datenbank zu erstellen:

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

2. Gewähren Sie Azure-Diensten den Zugriff auf SQL Server. Vergewissern Sie sich, dass für den Server mit SQL Server die Einstellung **Zugriff auf Azure-Dienste erlauben** aktiviert ist. Diese Einstellung ermöglicht Data Factory das Schreiben von Daten in Ihre SQL Server-Instanz. Führen Sie folgende Schritte aus, um diese Einstellung zu überprüfen und zu aktivieren:

    a. Klicken Sie links auf **Weitere Dienste** und dann auf **SQL Server**.

    b. Wählen Sie Ihren Server aus, und klicken Sie dann auf **EINSTELLUNGEN** > **Firewall**.

    c. Legen Sie auf der Seite **Firewalleinstellungen** die Option **Zugriff auf Azure-Dienste erlauben** auf **EIN** fest.

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

1. Klicken Sie auf der Seite **Erste Schritte** auf die Kachel **Daten kopieren**, um das Tool zum Kopieren von Daten zu starten. 

   ![Kachel für das Tool zum Kopieren von Daten](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Geben Sie auf der Seite **Eigenschaften** unter **Taskname** den Namen **CopyFromBlobToSqlPipeline** ein. Klicken Sie anschließend auf **Weiter**. Über die Data Factory-Benutzeroberfläche wird eine Pipeline mit dem angegebenen Tasknamen erstellt. 

    ![Eigenschaftenseite](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. Führen Sie auf der Seite **Quelldatenspeicher** die folgenden Schritte aus:

    a. Klicken Sie auf **+ Neue Verbindung erstellen**, um eine Verbindung hinzuzufügen.

    ![Neuer verknüpfter Quelldienst](./media/tutorial-copy-data-tool/new-source-linked-service.png)

    b. Wählen Sie im Katalog **Azure Blob Storage** aus, und klicken Sie dann auf **Weiter**.

    ![Auswählen der Blobquelle](./media/tutorial-copy-data-tool/select-blob-source.png)

    c. Wählen Sie auf der Seite **New Linked Service** (Neuer verknüpfter Dienst) in der Liste **Speicherkontoname** Ihr Speicherkonto aus, und klicken Sie auf **Fertig stellen**.

    ![Konfigurieren des Azure-Speichers](./media/tutorial-copy-data-tool/configure-azure-storage.png)

    d. Wählen Sie den neu erstellten verknüpften Dienst als Quelle aus, und klicken Sie auf **Weiter**.

    ![Auswählen des verknüpften Quelldiensts](./media/tutorial-copy-data-tool/select-source-linked-service.png)

4. Führen Sie auf der Seite **Choose the input file or folder** (Eingabedatei oder -ordner auswählen) die folgenden Schritte aus:
    
    a. Klicken Sie auf **Durchsuchen**, um zum Ordner **adfv2tutorial/input** zu navigieren, wählen Sie die Datei **inputEmp.txt** aus, und klicken Sie dann auf **Auswählen**.

    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/tutorial-copy-data-tool/specify-source-path.png)

    b. Klicken Sie auf **Weiter**, um mit dem nächsten Schritt fortzufahren.

5. Auf der Seite **Dateiformateinstellungen** können Sie sehen, dass das Tool die Spalten- und Zeilentrennzeichen automatisch erkennt. Klicken Sie auf **Weiter**. Auf dieser Seite können Sie außerdem eine Vorschau der Daten und das Schema der Eingabedaten anzeigen. 

    ![Dateiformateinstellungen](./media/tutorial-copy-data-tool/file-format-settings-page.png)
6. Führen Sie auf der Seite **Zieldatenspeicher** die folgenden Schritte aus:

    a. Klicken Sie auf **+ Neue Verbindung erstellen**, um eine Verbindung hinzuzufügen.

    ![Neuer verknüpfter Senkendienst](./media/tutorial-copy-data-tool/new-sink-linked-service.png)

    b. Wählen Sie im Katalog **Azure Blob Storage** aus, und klicken Sie dann auf **Weiter**.

    ![Auswählen der Azure SQL-Datenbank](./media/tutorial-copy-data-tool/select-azure-sql-db.png)

    c. Wählen Sie auf der Seite **New Linked Service** (Neuer verknüpfter Dienst) in der Dropdownliste Ihren Server- und Datenbanknamen aus, und geben Sie den Benutzernamen und das Kennwort an. Klicken Sie anschließend auf **Fertig stellen**.    

    ![Konfigurieren von Azure SQL-Datenbank](./media/tutorial-copy-data-tool/config-azure-sql-db.png)

    d. Wählen Sie den neu erstellten verknüpften Dienst als Senke aus, und klicken Sie auf **Weiter**.

    ![Auswählen des verknüpften Senkendiensts](./media/tutorial-copy-data-tool/select-sink-linked-service.png)

7. Wählen Sie auf der Seite **Tabellenmapping** die Tabelle **[dbo].[emp]** aus, und klicken Sie dann auf **Weiter**. 

    ![Tabellenmapping](./media/tutorial-copy-data-tool/table-mapping.png)
8. Auf der Seite **Schemazuordnung** sehen Sie, dass die erste und zweite Spalte in der Eingabedatei den Spalten **FirstName** und **LastName** der Tabelle **emp** zugeordnet werden. Klicken Sie auf **Weiter**.

    ![Seite „Schemazuordnung“](./media/tutorial-copy-data-tool/schema-mapping.png)
9. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**. 
10. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie anschließend auf **Weiter**.

    ![Seite „Zusammenfassung“](./media/tutorial-copy-data-tool/summary-page.png)
11. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline (Task) zu überwachen.

    ![Bereitstellungsseite](./media/tutorial-copy-data-tool/deployment-page.png)
12. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. In der Spalte **Aktionen** werden Links zum Anzeigen von Aktivitätsausführungsdetails und zum erneuten Ausführen der Pipeline angezeigt. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. 

    ![Überwachen der Pipelineausführungen](./media/tutorial-copy-data-tool/pipeline-monitoring.png)
13. Klicken Sie in der Spalte **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen), um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie auf den Link **Details** (Brillensymbol) in der Spalte **Aktionen**. Klicken Sie oben auf den Link **Pipelines**, um zurück zur Ansicht **mit den Pipelineausführungen** zu wechseln. Klicken Sie zum Aktualisieren der Ansicht auf **Aktualisieren**. 

    ![Überwachung der Aktivitätsausführungen](./media/tutorial-copy-data-tool/activity-monitoring.png)

    ![Kopieren der Aktivitätsdetails](./media/tutorial-copy-data-tool/copy-execution-details.png)

14. Stellen Sie sicher, dass die Daten in die Tabelle **emp** in Ihrer SQL-Datenbank eingefügt werden.

    ![Überprüfen der SQL-Ausgabe](./media/tutorial-copy-data-tool/verify-sql-output.png)

15. Klicken Sie im linken Bereich auf die Registerkarte **Autor**, um in den Bearbeitungsmodus zu wechseln. Sie können die vom Tool erstellten verknüpften Dienste, Datasets und Pipelines mit dem Editor aktualisieren. Ausführliche Informationen zum Bearbeiten dieser Entitäten über die Data Factory-Benutzeroberfläche finden Sie in der [Azure-Portal-Version dieses Tutorials](tutorial-copy-data-portal.md).

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten aus Blob Storage in eine SQL-Datenbank. Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten
> * Überwachen der Pipeline- und Aktivitätsausführungen.

Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Daten von einem lokalen Speicherort in die Cloud kopieren: 

> [!div class="nextstepaction"]
>[Tutorial: Kopieren von Daten aus einer lokalen SQL Server-Datenbank nach Azure Blob Storage](tutorial-hybrid-copy-data-tool.md)

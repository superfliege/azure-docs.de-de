---
title: Massenkopieren von Daten mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: In diesem Artikel wird erklärt, wie Sie Azure Data Factory und Kopieraktivität zum Kopieren von Daten per Massenvorgang aus einem Quelldatenspeicher in einen Zieldatenspeicher verwenden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/22/2018
ms.author: jingwang
ms.openlocfilehash: 444269aa7ca2b0a82b78e8437b7884ef8833c665
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59279787"
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Massenkopieren von mehreren Tabellen mithilfe von Azure Data Factory
In diesem Tutorial wird das **Kopieren von mehreren Tabellen aus einer Azure SQL-Datenbank in Azure SQL Data Warehouse** veranschaulicht. Sie können dieses Muster auch in anderen Kopierszenarios anwenden. So können Sie z.B. Tabellen aus SQL Server/Oracle in Azure SQL-Datenbank/Data Warehouse/Azure Blob kopieren oder verschiedene Pfade aus Blob in Azure SQL-Datenbanktabellen.

> [!NOTE]
> - Falls Sie noch nicht mit Azure Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](introduction.md) zu lesen.

Das Tutorial umfasst die folgenden Schritte:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen von Azure SQL-Datenbank, Azure SQL Data Warehouse und mit Azure Storage verknüpften Diensten.
> * Erstellen von Datasets für Azure SQL-Datenbank und Azure SQL Data Warehouse.
> * Erstellen einer Pipeline zum Abrufen der zu kopierenden Tabellen und einer weiteren Pipeline zur Durchführung des eigentlichen Kopiervorgangs. 
> * Starten einer Pipelineausführung
> * Überwachen der Pipeline- und Aktivitätsausführungen.

In diesem Tutorial wird das Azure-Portal verwendet. Informationen zur Verwendung von anderen Tools/SDKs zum Erstellen einer Data Factory finden Sie unter [Schnellstarts](quickstart-create-data-factory-dot-net.md). 

## <a name="end-to-end-workflow"></a>Kompletter Workflow
In diesem Szenario sollen mehrere Tabellen aus der Azure SQL-Datenbank in SQL Data Warehouse kopiert werden. Nachfolgend ist der logische Ablauf eines Workflows dargestellt, der in Pipelines ausgeführt wird:

![Workflow](media/tutorial-bulk-copy-portal/tutorial-copy-multiple-tables.png)

* Die erste Pipeline ruft die Liste mit den Tabellen ab, die in die Senkendatenspeicher kopiert werden sollen.  Sie können stattdessen auch eine Metadatentabelle mit den Tabellen verwalten, die in die Senkendatenspeicher kopiert werden sollen. Die Pipeline löst anschließend eine weitere Pipeline aus, die wiederum jede Tabelle in der Datenbank durchläuft und den Datenkopiervorgang ausführt.
* Die zweite Pipeline führt den eigentlichen Kopiervorgang aus. Dazu wird die Liste mit den Tabellen als Parameter verwendet. Kopieren Sie für jede Tabelle in der Liste die jeweilige Tabelle aus der Azure SQL-Datenbank in die entsprechende Tabelle in SQL Data Warehouse. Verwenden Sie für eine optimale Leistung das [gestaffelte Kopieren über Blob Storage und PolyBase](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). In diesem Beispiel wird die Liste mit den Tabellen von der ersten Pipeline als Wert für den Parameter übergeben. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure Storage-Konto**. Das Azure Storage-Konto wird im Massenkopiervorgang als Staging-Blobspeicher verwendet. 
* **Azure SQL-Datenbank**. Diese Datenbank enthält die Quelldaten. 
* **Azure SQL Data Warehouse**. Dieses Data Warehouse enthält die Daten, die aus der SQL-Datenbank kopiert werden. 

### <a name="prepare-sql-database-and-sql-data-warehouse"></a>Vorbereiten von SQL-Datenbank und SQL Data Warehouse

**Vorbereiten der Azure SQL-Quelldatenbank**:

Erstellen Sie eine Azure SQL-Datenbank mit Adventure Works LT-Beispieldaten, indem Sie den Anweisungen im Artikel [Erstellen einer Azure SQL-Datenbank](../sql-database/sql-database-get-started-portal.md) folgen. In diesem Tutorial werden alle Tabellen aus der Beispieldatenbank in SQL Data Warehouse kopiert.

**Vorbereiten des Senkenwarehouses Azure SQL Data Warehouse**:

1. Wenn Sie noch kein Azure SQL Data Warehouse erstellt haben, finden Sie die Anleitung dazu im Artikel [Erstellen eines SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).

1. Erstellen Sie in SQL Data Warehouse übereinstimmende Tabellenschemas. Verwenden Sie das [Hilfsprogramm für die Migration](https://www.microsoft.com/download/details.aspx?id=49100) zum **Migrieren von Schemas** aus einer Azure SQL-Datenbank zu Azure SQL Data Warehouse. In einem späteren Schritt können Sie Daten mit Azure Data Factory migrieren/kopieren.

## <a name="azure-services-to-access-sql-server"></a>Azure-Dienste für den Zugriff auf SQL-Server

Erlauben Sie Azure-Diensten den Zugriff auf SQL-Server. Das gilt sowohl für die SQL-Datenbank als auch für SQL Data Warehouse. Stellen Sie sicher, dass die Einstellung **Zugriff auf Azure-Dienste erlauben** für Ihren Azure SQL-Server auf **EIN** festgelegt ist. Mit dieser Einstellung wird dem Data Factory-Dienst erlaubt, Daten aus Ihrer Azure SQL-Datenbank zu lesen und in Ihr Azure SQL Data Warehouse zu schreiben. Führen Sie folgende Schritte aus, um diese Einstellung zu überprüfen und zu aktivieren:

1. Klicken Sie links auf den Hub **Weitere Dienste** und dann auf **SQL Server**.
1. Wählen Sie Ihren Server aus, und klicken Sie unter **EINSTELLUNGEN** auf **Firewall**.
1. Klicken Sie auf der Seite **Firewalleinstellungen** für **Zugriff auf Azure-Dienste erlauben** auf **EIN**.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
1. Starten Sie den Webbrowser **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.
1. Wählen Sie im Menü auf der linken Seite **Ressource erstellen** > **Daten + Analysen** > **Data Factory** aus: 
   
   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

1. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **ADFTutorialBulkCopyDF** ein. 
      
     ![Seite „Neue Data Factory“](./media/tutorial-bulk-copy-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte der unten angegebene Fehler für das Feld „Name“ auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialBulkCopyDF“). Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
       `Data factory name “ADFTutorialBulkCopyDF” is not available`
1. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
1. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
   - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
   - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
     Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
1. Wählen Sie **V2** als **Version** aus.
1. Wählen Sie den **Standort** für die Data Factory aus. Eine Liste der Azure-Regionen, in denen Data Factory derzeit verfügbar ist, finden Sie, indem Sie die für Sie interessanten Regionen auf der folgenden Seite auswählen und dann **Analysen** erweitern, um **Data Factory** zu finden: [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/). Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.
1. Wählen Sie die Option **An Dashboard anheften** aus.     
1. Klicken Sie auf **Create**.
1. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Deploying data factory** (Data Factory wird bereitgestellt...). 

     ![Kachel „Die Data Factory wird bereitgestellt“](media//tutorial-bulk-copy-portal/deploying-data-factory.png)
1. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
     ![Data Factory-Startseite](./media/tutorial-bulk-copy-portal/data-factory-home-page.png)
1. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Anwendung für die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten.
1. Wechseln Sie auf der Seite **Erste Schritte** im Bereich links zur Registerkarte **Bearbeiten**, wie in der folgenden Abbildung gezeigt:  

     ![Seite für die ersten Schritte](./media/tutorial-bulk-copy-portal/get-started-page.png)

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Sie erstellen verknüpfte Dienste, um Ihre Datenspeicher und Computes mit einer Data Factory zu verknüpfen. Ein verknüpfter Dienst enthält die Verbindungsinformationen, die der Data Factory-Dienst zur Laufzeit zum Herstellen der Verbindung mit dem Datenspeicher verwendet. 

In diesem Tutorial verknüpfen Sie Ihre Azure SQL-Datenbank-, Azure SQL Data Warehouse- und Azure Blob Storage-Datenspeicher mit Ihrer Data Factory. Die Azure SQL-Datenbank ist der Quelldatenspeicher. Das Azure SQL Data Warehouse ist der Senken-/Zieldatenspeicher. Azure Blob Storage dient zum Bereitstellen der Daten per Staging, bevor sie mit PolyBase in die SQL Data Warehouse-Instanz geladen werden. 

### <a name="create-the-source-azure-sql-database-linked-service"></a>Erstellen des verknüpften Quelldiensts Azure SQL-Datenbank
In diesem Schritt erstellen Sie einen verknüpften Dienst, um Ihre Azure SQL-Datenbank mit der Data Factory zu verbinden. 

1. Klicken Sie unten im Fenster auf **Verbindungen** und dann in der Symbolleiste auf **+ Neu**. 

    ![Schaltfläche „Neuer verknüpfter Dienst“](./media/tutorial-bulk-copy-portal/new-linked-service-button.png)
1. Wählen Sie im Fenster **Neuer verknüpfter Dienst** die Option **Azure SQL-Datenbank**, und klicken Sie auf **Weiter**. 

    ![Auswählen einer Azure SQL-Datenbank](./media/tutorial-bulk-copy-portal/select-azure-sql-database.png)
1. Führen Sie im Fenster **Neuer verknüpfter Dienst** die folgenden Schritte aus: 

    1. Geben Sie unter **Name** den Namen **AzureSqlDatabaseLinkedService** ein. 
    1. Wählen Sie unter **Servername** Ihre Azure SQL Server-Instanz aus.
    1. Wählen Sie unter **Datenbankname** Ihre Azure SQL-Datenbank aus. 
    1. Geben Sie den **Namen des Benutzers** ein, um eine Verbindung mit der Azure SQL-Datenbank herzustellen. 
    1. Geben Sie das **Kennwort** für den Benutzer ein. 
    1. Klicken Sie auf **Verbindung testen**, um die Verbindung mit der Azure SQL-Datenbank mit den angegebenen Informationen zu testen.
    1. Klicken Sie auf **Speichern**.

        ![Einstellungen für die Azure SQL-Datenbank](./media/tutorial-bulk-copy-portal/azure-sql-database-settings.png)

### <a name="create-the-sink-azure-sql-data-warehouse-linked-service"></a>Erstellen des verknüpften Senkendiensts Azure SQL Data Warehouse

1. Klicken Sie auf der Registerkarte **Verbindungen** in der Symbolleiste erneut auf **+ Neu**. 
1. Wählen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die Option **Azure SQL Data Warehouse**, und klicken Sie auf **Weiter**. 
1. Führen Sie im Fenster **Neuer verknüpfter Dienst** die folgenden Schritte aus: 

    1. Geben Sie unter **Name** den Namen **AzureSqlDWLinkedService** ein. 
    1. Wählen Sie unter **Servername** Ihre Azure SQL Server-Instanz aus.
    1. Wählen Sie unter **Datenbankname** Ihre Azure SQL-Datenbank aus. 
    1. Geben Sie den **Namen des Benutzers** ein, um eine Verbindung mit der Azure SQL-Datenbank herzustellen. 
    1. Geben Sie das **Kennwort** für den Benutzer ein. 
    1. Klicken Sie auf **Verbindung testen**, um die Verbindung mit der Azure SQL-Datenbank mit den angegebenen Informationen zu testen.
    1. Klicken Sie auf **Speichern**.

### <a name="create-the-staging-azure-storage-linked-service"></a>Erstellen des verknüpften Stagingdiensts Azure Storage
In diesem Tutorial wird Azure Blob Storage als vorläufiger Stagingbereich zur Aktivierung von PolyBase verwendet, um eine bessere Leistung zu erzielen.

1. Klicken Sie auf der Registerkarte **Verbindungen** in der Symbolleiste erneut auf **+ Neu**. 
1. Wählen Sie im Fenster **Neuer verknüpfter Dienst** die Option **Azure Blob Storage**, und klicken Sie dann auf **Weiter**. 
1. Führen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die folgenden Schritte aus: 

    1. Geben Sie unter **Name**  den Namen **AzureStorageLinkedService** ein. 
    1. Wählen Sie unter **Speicherkontoname** Ihr **Azure Storage-Konto** aus.
    1. Klicken Sie auf **Speichern**.


## <a name="create-datasets"></a>Erstellen von Datasets
In diesem Tutorial werden Quell- und Senkendatasets erstellt, die den Speicherort der Daten angeben. 

Das Eingabedataset **AzureSqlDatabaseDataset** verweist auf **AzureSqlDatabaseLinkedService**. Der verknüpfte Dienst gibt die Verbindungszeichenfolge für die Herstellung der Verbindung mit der Datenbank an. Das Dataset gibt den Namen der Datenbank und die Tabelle an, in der die Quelldaten enthalten sind. 

Das Ausgabedataset **AzureSqlDWDataset** verweist auf **AzureSqlDWLinkedService**. Der verknüpfte Dienst gibt die Verbindungszeichenfolge für die Herstellung der Verbindung mit dem Data Warehouse an. Das Dataset gibt die Datenbank und die Tabelle an, in die die Daten kopiert werden. 

In diesem Tutorial sind die SQL-Quell- und -Zieltabellen in den Datasetdefinitionen nicht hartcodiert. Stattdessen übergibt die ForEach-Aktivität den Namen der Tabelle zur Laufzeit an die Copy-Aktivität. 

### <a name="create-a-dataset-for-source-sql-database"></a>Erstellen eines Datasets für die SQL-Quelldatenbank

1. Klicken Sie im Bereich auf der linken Seite auf **+** (Pluszeichen) und dann auf **Dataset**. 

    ![Menü „Neues Dataset“](./media/tutorial-bulk-copy-portal/new-dataset-menu.png)
1. Wählen Sie im Fenster **Neues Dataset** die Option **Azure SQL-Datenbank**, und klicken Sie auf **Fertig stellen**. Eine neue Registerkarte mit dem Titel **AzureSqlTable1** wird angezeigt. 
    
    ![Auswählen einer Azure SQL-Datenbank](./media/tutorial-bulk-copy-portal/select-azure-sql-database-dataset.png)
1. Geben Sie unten im Eigenschaftenfenster unter **Name** den Namen **AzureSqlDatabaseDataset** ein.

1. Wechseln Sie zur Registerkarte **Verbindung**, und führen Sie die folgenden Schritte aus: 

   1. Wählen Sie unter **Verknüpfter Dienst** die Option **AzureSqlDatabaseLinkedService**.
   1. Wählen Sie für **Tabelle** eine beliebige Tabelle aus. Diese Tabelle ist eine Dummytabelle. Sie geben beim Erstellen einer Pipeline im Quelldataset eine Abfrage ein. Die Abfrage wird verwendet, um Daten aus der Azure SQL-Datenbank zu extrahieren. Alternativ hierzu können Sie auf das Kontrollkästchen **Bearbeiten** klicken und als Tabellenname **dummyName** eingeben. 

      ![Seite für Quelldatasetverbindung](./media/tutorial-bulk-copy-portal/source-dataset-connection-page.png)
 

### <a name="create-a-dataset-for-sink-sql-data-warehouse"></a>Erstellen eines Datasets für das Senkenwarehouse SQL Data Warehouse

1. Klicken Sie im Bereich auf der linken Seite auf **+** (Pluszeichen) und dann auf **Dataset**. 
1. Wählen Sie im Fenster **Neues Dataset** die Option **Azure SQL Data Warehouse**, und klicken Sie auf **Fertig stellen**. Eine neue Registerkarte mit dem Titel **AzureSqlDWTable1** wird angezeigt. 
1. Geben Sie unten im Eigenschaftenfenster unter **Name** den Namen **AzureSqlDWDataset** ein.
1. Wechseln Sie zur Registerkarte **Parameter**, klicken Sie auf **+ Neu**, und geben Sie **DWTableName** als Parameternamen ein. Stellen Sie beim Kopieren dieses Namens von der Seite (und beim Einfügen) sicher, dass am Ende von **DWTableName** keine **nachgestellte Leerstelle** angefügt ist. 

    ![Seite für Quelldatasetverbindung](./media/tutorial-bulk-copy-portal/sink-dataset-new-parameter.png)

1. Wechseln Sie zur Registerkarte **Verbindung**. 

    a. Wählen Sie unter **Verknüpfter Dienst** die Option **AzureSqlDatabaseLinkedService**.

    b. Aktivieren Sie unter **Tabelle** die Option **Bearbeiten**, klicken Sie in das Feld für die Eingabe des Tabellennamens, und klicken Sie dann unten auf den Link **Dynamischen Inhalt hinzufügen**. 
    
    ![Parametername](./media/tutorial-bulk-copy-portal/table-name-parameter.png)

    c. Klicken Sie auf der Seite **Dynamischen Inhalt hinzufügen** unter **Parameter** auf **DWTAbleName**. Das oberste Textfeld `@dataset().DWTableName` für Ausdrücke wird automatisch gefüllt. Klicken Sie anschließend auf **Fertig stellen**. Die **tableName**-Eigenschaft des Datasets ist auf den Wert festgelegt, der als Argument für den Parameter **DWTableName** übergeben wird. Die ForEach-Aktivität durchläuft eine Liste mit Tabellen und übergibt diese einzeln an die Copy-Aktivität. 

    ![Dataset-Parameter-Generator](./media/tutorial-bulk-copy-portal/dataset-parameter-builder.png)

## <a name="create-pipelines"></a>Erstellen von Pipelines
In diesem Tutorial werden zwei Pipelines erstellt: **IterateAndCopySQLTables** und **GetTableListAndTriggerCopyData**. 

Die Pipeline **GetTableListAndTriggerCopyData** führt zwei Schritte aus:

* Abrufen der Systemtabelle für die Azure SQL-Datenbank, um die Liste mit den Tabellen abzurufen, die kopiert werden sollen.
* Auslösen der Pipeline **IterateAndCopySQLTables**, um den eigentlichen Kopiervorgang der Daten durchzuführen.

**IterateAndCopySQLTables** akzeptiert eine Liste von Tabellen als Parameter. Für jede Tabelle in der Liste werden Daten aus der Tabelle in der Azure SQL-Datenbank in Azure SQL Data Warehouse kopiert. Dazu wird das gestaffelte Kopieren und PolyBase verwendet.

### <a name="create-the-pipeline-iterateandcopysqltables"></a>Erstellen der Pipeline „IterateAndCopySQLTables“

1. Klicken Sie im linken Bereich auf **+** (Pluszeichen) und dann auf **Pipeline**.

    ![Menü für neue Pipeline](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
1. Geben Sie auf der Registerkarte **Allgemein** als Name **IterateAndCopySQLTables** an. 

1. Wechseln Sie zur Registerkarte **Parameter**, und führen Sie die folgenden Aktionen durch: 

    1. Klicken Sie auf **+ NEU**. 
    1. Geben Sie **tableList** für den Parameter **name** ein.
    1. Wählen Sie unter **Typ** die Option **Array**.

        ![Pipelineparameter](./media/tutorial-bulk-copy-portal/first-pipeline-parameter.png)
1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Iteration & Conditions** (Iteration und Bedingungen), und ziehen Sie die **ForEach**-Aktivität in die Oberfläche zum Entwerfen von Pipelines. Sie können in der Toolbox **Aktivitäten** auch nach Aktivitäten suchen. 

    a. Geben Sie unten auf der Registerkarte **Allgemein** unter **Name** den Namen **IterateSQLTables** ein. 

    b. Wechseln Sie auf die Registerkarte **Einstellungen**, klicken Sie auf das Eingabefeld für **Elemente**, und klicken Sie anschließend unten auf den Link **Dynamischen Inhalt hinzufügen**. 

    ![Einstellungen für ForEach-Aktivitäten](./media/tutorial-bulk-copy-portal/for-each-activity-settings.png)

    c. Reduzieren Sie auf der Seite **Dynamischen Inhalt hinzufügen** die Abschnitte „Systemvariablen“ und „Funktionen“, und klicken Sie unter **Parameter** auf **tableList**. Das oberste Textfeld für Ausdrücke wird automatisch mit `@pipeline().parameter.tableList` gefüllt. Klicken Sie anschließend auf **Fertig stellen**. 

    ![Foreach-Parameter-Generator](./media/tutorial-bulk-copy-portal/for-each-parameter-builder.png)
    
    d. Wechseln Sie zur Registerkarte **Aktivitäten**, und klicken Sie auf **Aktivität hinzufügen**, um der Aktivität **ForEach** eine untergeordnete Aktivität hinzuzufügen.

1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **DataFlow**, und ziehen Sie die Aktivität **Copy** auf die Oberfläche des Pipeline-Designers. Beachten Sie das Breadcrumb-Menü im oberen Bereich. IterateAndCopySQLTable ist der Pipelinename, und IterateSQLTables ist der Name der ForEach-Aktivität. Der Designer ist Teil des Aktivitätsbereichs. Klicken Sie im Breadcrumb-Menü auf den Link, um vom ForEach-Editor zurück zum Pipeline-Editor zu wechseln. 

    ![Kopieren in ForEach](./media/tutorial-bulk-copy-portal/copy-in-for-each.png)
1. Wechseln Sie zur Registerkarte **Quelle**, und führen Sie die folgenden Schritte aus:

    1. Wählen Sie unter **Source Dataset** (Quelldataset) die Option **AzureSqlDatabaseDataset**. 
    1. Wählen Sie unter **Abfrage verwenden** die Option **Abfrage**. 
    1. Klicken Sie auf das Eingabefeld **Abfrage**, wählen Sie unten die Option **Dynamischen Inhalt hinzufügen**, geben Sie den folgenden Ausdruck für **Abfrage** ein, und wählen Sie **Fertig stellen**.

        ```sql
        SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ``` 

        ![Einstellungen für die Kopierquelle](./media/tutorial-bulk-copy-portal/copy-source-settings.png)
1. Wechseln Sie zur Registerkarte **Senke**, und führen Sie die folgenden Schritte aus: 

    1. Wählen Sie unter **Sink Dataset** (Senkendataset) die Option **AzureSqlDWDataset**.
    1. Klicken Sie auf das Eingabefeld für den Wert (VALUE) des Parameters „DWTableName“, wählen Sie unten die Option **Dynamischen Inhalt hinzufügen**, geben Sie den Ausdruck `[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]` als Skript ein, und wählen Sie **Fertig stellen**.
    1. Erweitern Sie die Option **Polybase Settings** (PolyBase-Einstellungen), und wählen Sie **Allow polybase** (PolyBase zulassen). 
    1. Deaktivieren Sie die Option **Use Type default** (Typstandard verwenden). 
    1. Klicken Sie auf das Eingabefeld **Pre-copy Script** (Skript für Vorabkopieren), wählen Sie unten die Option **Dynamischen Inhalt hinzufügen**, geben Sie den folgenden Ausdruck als Skript ein, und wählen Sie **Fertig stellen**. 

        ```sql
        TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ```

        ![Einstellungen für die Kopiersenke](./media/tutorial-bulk-copy-portal/copy-sink-settings.png)

1. Wechseln Sie zur Registerkarte **Einstellungen**, und führen Sie die folgenden Schritte aus: 

    1. Wählen Sie unter **Enable Staging** (Staging aktivieren) die Option **True**.
    1. Wählen Sie unter **Store Account Linked Service** (Verknüpfter Dienst des Speicherkontos) die Option **AzureStorageLinkedService**.

        ![Staging aktivieren](./media/tutorial-bulk-copy-portal/copy-sink-staging-settings.png)

1. Klicken Sie zum Überprüfen der Pipelineeinstellungen auf der Symbolleiste für die Pipeline auf **Überprüfen**. Vergewissern Sie sich, dass keine Validierungsfehler vorliegen. Klicken Sie zum Schließen des **Pipeline Validation Report** (Pipelineüberprüfungsbericht) auf **>>**.

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>Erstellen der Pipeline „GetTableListAndTriggerCopyData“

Mit dieser Pipeline werden zwei Schritte ausgeführt:

* Abrufen der Systemtabelle für die Azure SQL-Datenbank, um die Liste mit den Tabellen abzurufen, die kopiert werden sollen.
* Auslösen der Pipeline „IterateAndCopySQLTables“, um den eigentlichen Kopiervorgang der Daten auszuführen.

1. Klicken Sie im linken Bereich auf **+** (Pluszeichen) und dann auf **Pipeline**.

    ![Menü „Neue Pipeline“](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
1. Ändern Sie im Eigenschaftenfenster den Namen der Pipeline in **GetTableListAndTriggerCopyData**. 

1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Allgemein**, und ziehen Sie die **Lookup**-Aktivität auf die Oberfläche des Pipeline-Designers. Führen Sie anschließend die folgenden Schritte aus:

    1. Geben Sie unter **Name** den Namen **LookupTableList** ein. 
    1. Geben Sie unter **Beschreibung** den Text **Retrieve the table list from Azure SQL database** (Tabellenliste aus Azure SQL-Datenbank abrufen) ein.

        ![Lookup-Aktivität – Seite „Allgemein“](./media/tutorial-bulk-copy-portal/lookup-general-page.png)
1. Wechseln Sie zur Seite **Einstellungen**, und führen Sie die folgenden Schritte aus:

    1. Wählen Sie unter **Source Dataset** (Quelldataset) die Option **AzureSqlDatabaseDataset**. 
    1. Wählen Sie unter **Abfrage verwenden** die Option **Abfrage**. 
    1. Geben Sie unter **Abfrage** die folgende SQL-Abfrage ein:

        ```sql
        SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'
        ```
    1. Deaktivieren Sie das Kontrollkästchen für das Feld **First row only** (Nur erste Zeile).

        ![Lookup-Aktivität – Seite „Einstellungen“](./media/tutorial-bulk-copy-portal/lookup-settings-page.png)
1. Ziehen Sie die **Execute Pipeline**-Aktivität aus der Toolbox „Aktivitäten“ in die Oberfläche des Pipeline-Designers, und legen Sie den Namen auf **TriggerCopy** fest.

    ![Execute Pipeline-Aktivität – Seite „Allgemein“](./media/tutorial-bulk-copy-portal/execute-pipeline-general-page.png)    
1. Wechseln Sie zur Seite **Einstellungen**, und führen Sie die folgenden Schritte aus: 

    1. Wählen Sie unter **Invoked pipeline** (Aufgerufene Pipeline) die Option **IterateAndCopySQLTables**. 
    1. Erweitern Sie den Abschnitt **Erweitert**. 
    1. Klicken Sie im Abschnitt **Parameter** auf **+ Neu**. 
    1. Geben Sie **tableList** für den Parameter **name** ein.
    1. Klicken Sie auf das Eingabefeld für den Wert, wählen Sie unten die Option **Dynamischen Inhalt hinzufügen**, geben Sie `@activity('LookupTableList').output.value` als Wert für den Tabellennamen ein, und wählen Sie **Fertig stellen**. Sie legen die Ergebnisliste der Lookup-Aktivität als Eingabe für die zweite Pipeline fest. Die Ergebnisliste enthält die Liste mit den Tabellen, deren Daten auf das Ziel kopiert werden müssen. 

        ![Execute Pipeline-Aktivität – Seite „Einstellungen“](./media/tutorial-bulk-copy-portal/execute-pipeline-settings-page.png)
1. **Verbinden** Sie die **Lookup**-Aktivität mit der **Execute Pipeline**-Aktivität, indem Sie das **grüne Feld**, das der Lookup-Aktivität zugeordnet ist, in den Bereich links von der Execute Pipeline-Aktivität ziehen.

    ![Verbinden von Lookup- und Execute Pipeline-Aktivitäten](./media/tutorial-bulk-copy-portal/connect-lookup-execute-pipeline.png)
1. Klicken Sie zum Überprüfen der Pipeline in der Symbolleiste auf **Überprüfen**. Vergewissern Sie sich, dass keine Validierungsfehler vorliegen. Klicken Sie zum Schließen des **Pipeline Validation Report** (Pipelineüberprüfungsbericht) auf **>>**.

1. Klicken Sie zum Veröffentlichen von Entitäten (Datasets, Pipelines usw.) im Data Factory-Dienst oben im Fenster auf **Alle veröffentlichen**. Warten Sie, bis die Veröffentlichung erfolgreich durchgeführt wurde. 

## <a name="trigger-a-pipeline-run"></a>Auslösen einer Pipelineausführung

Navigieren Sie zur Pipeline **GetTableListAndTriggerCopyData**, und klicken Sie auf **Trigger** (Auslösen) und dann auf **Trigger Now** (Jetzt auslösen). 

![Manuelles Auslösen](./media/tutorial-bulk-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

1. Wechseln Sie zur Registerkarte **Überwachen**. Klicken Sie auf **Aktualisieren**, bis Ausführungen für beide Pipelines Ihrer Lösung angezeigt werden. Fahren Sie mit dem Aktualisieren der Liste fort, bis der Status **Erfolgreich** angezeigt wird. 

    ![Pipelineausführungen](./media/tutorial-bulk-copy-portal/pipeline-runs.png)
1. Klicken Sie unter dem Link „Aktionen“ auf den ersten Link für diese Pipeline, um die Aktivitätsausführungen anzuzeigen, die der Pipeline GetTableListAndTriggerCopyData zugeordnet sind. Es sollten zwei Aktivitätsausführungen für diese Pipelineausführung angezeigt werden. 

    ![Aktivitätsausführungen](./media/tutorial-bulk-copy-portal/activity-runs-1.png)    
1. Klicken Sie zum Anzeigen der Ausgabe der **Lookup**-Aktivität in der Spalte **Ausgabe** auf die Spalte für diese Aktivität. Sie können das Fenster **Ausgabe** maximieren und wiederherstellen. Klicken Sie auf **X**, nachdem Sie die Prüfung durchgeführt haben, um das Fenster **Ausgabe** zu schließen.

    ```json
    {
        "count": 9,
        "value": [
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Customer"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Product"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductModelProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductCategory"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Address"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "CustomerAddress"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderDetail"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderHeader"
            }
        ],
        "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)",
        "effectiveIntegrationRuntimes": [
            {
                "name": "DefaultIntegrationRuntime",
                "type": "Managed",
                "location": "East US",
                "billedDuration": 0,
                "nodes": null
            }
        ]
    }
    ```    
1. Klicken Sie oben auf den Link **Pipelines**, um zurück zur Ansicht **Pipeline Runs** (Pipelineausführungen) zu wechseln. Klicken Sie auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen) (erster Link in der Spalte **Aktionen**) für die Pipeline **IterateAndCopySQLTables**. Die Ausgabe sollte wie in der folgenden Abbildung aussehen: Beachten Sie, dass für jede Tabelle in der Ausgabe der **Lookup**-Aktivität eine Ausführung der **Copy**-Aktivität enthalten ist. 

    ![Aktivitätsausführungen](./media/tutorial-bulk-copy-portal/activity-runs-2.png)
1. Vergewissern Sie sich, dass die Daten in die SQL Data Warehouse-Zielinstanz kopiert wurden, die Sie in diesem Tutorial verwendet haben. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie die folgenden Schritte ausgeführt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen von Azure SQL-Datenbank, Azure SQL Data Warehouse und mit Azure Storage verknüpften Diensten.
> * Erstellen von Datasets für Azure SQL-Datenbank und Azure SQL Data Warehouse.
> * Erstellen einer Pipeline zum Abrufen der zu kopierenden Tabellen und einer weiteren Pipeline zur Durchführung des eigentlichen Kopiervorgangs. 
> * Starten einer Pipelineausführung
> * Überwachen der Pipeline- und Aktivitätsausführungen.

Fahren Sie nun mit dem folgenden Tutorial fort, um mehr über das inkrementelle Kopieren von Daten aus einer Quelle in ein Ziel zu erfahren:
> [!div class="nextstepaction"]
>[Inkrementelles Kopieren von Daten](tutorial-incremental-copy-portal.md)

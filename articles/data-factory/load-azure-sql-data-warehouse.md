---
title: Laden von Daten in Azure SQL Data Warehouse mit Azure Data Factory | Microsoft Docs
description: Verwenden von Azure Data Factory zum Kopieren von Daten in Azure SQL Data Warehouse
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 36e24da50386d1abc441e2beb09f570a9612a346
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-sql-data-warehouse-using-azure-data-factory"></a>Laden von Daten in Azure SQL Data Warehouse mit Azure Data Factory

[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) ist eine cloudbasierte Datenbank für das horizontale Hochskalieren, mit der sehr große Datenvolumen verarbeitet werden können, und zwar sowohl relational als auch nicht relational.  SQL Data Warehouse basiert auf MPP-Architektur (Massively Parallel Processing), und ist für Data Warehouse-Workloads für Unternehmen optimiert.  Es bietet Cloudelastizität mit der Flexibilität, Speicher zu skalieren und unabhängig zu berechnen.

Die ersten Schritte mit Azure SQL Data Warehouse sind jetzt mithilfe von **Azure Data Factory** einfacher als je zuvor.  Azure Data Factory ist ein vollständig verwalteter cloudbasierter Daten-Integrationsdienst, der verwendet werden kann, um SQL Data Warehouse mit den Daten Ihres bestehenden Systems aufzufüllen. Dieser Dienst spart Ihnen wertvolle Zeit beim Bewerten von SQL Data Warehouse und Erstellen von Analyselösungen. Die wichtigsten Vorteile beim Laden von Daten in Azure SQL Data Warehouse mithilfe von Azure Data Factory sind:

* **Mühelose Einrichtung**: Intuitiver Assistent mit 5 Schritten. Keine Skripterstellung erforderlich.
* **Unterstützung für umfangreiche Datenspeicher**: Integrierte Unterstützung für umfangreiche lokale und cloudbasierte Datenspeicher. Eine ausführliche Liste finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
* **Sicher und kompatibel**: Daten werden über HTTPS oder ExpressRoute übertragen, und globale Dienste stellen sicher, dass Ihre Daten nie die geografische Grenze verlassen.
* **Beispiellose Leistung mithilfe von PolyBase**: Die Verwendung von PolyBase ist die effizienteste Methode zum Verschieben von Daten in Azure SQL Data Warehouse. Mit der Funktion „Stagingblob“ sinkt die Ladezeit für alle Arten von Datenspeicher drastisch. Dies gilt nicht für Azure Blob Storage und Data Lake Store, die von Polybase standardmäßig unterstützt werden. Weitere Informationen finden Sie unter [Leistung der Kopieraktivität](copy-activity-performance.md).

Diesem Artikel erfahren Sie, wie Sie das Tool zum Kopieren von Daten von Data Factory zum **Laden von Daten aus Azure SQL-Datenbank in Azure SQL Data Warehouse** verwenden können. Sie können ähnliche Schritte zum Kopieren von Daten aus anderen Typen von Datenspeichern befolgen.

> [!NOTE]
>  Im Artikel [Kopieren von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory](connector-azure-sql-data-warehouse.md) finden Sie allgemeine Informationen zu den Möglichkeiten von Data Factory beim Kopieren von Daten aus/in Azure SQL Data Warehouse.
>
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Kopieraktivität in V1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure SQL Data Warehouse**. Dieses Data Warehouse enthält die Daten, die aus der SQL-Datenbank kopiert werden. Wenn Sie noch kein Azure SQL Data Warehouse erstellt haben, finden Sie die Anleitung dazu im Artikel [Erstellen eines SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* **Azure SQL-Datenbank**. In diesem Tutorial werden Daten aus einer Azure SQL-Datenbank mit Adventure Works LT-Beispieldaten kopiert. Sie können eine solche Datenbank erstellen, indem Sie die Anweisungen im Artikel [Erstellen einer Azure SQL-Datenbank](../sql-database/sql-database-get-started-portal.md) befolgen. 
* **Azure Storage-Konto**. Azure Storage wird im Massenkopiervorgang als **Staging**blob verwendet. Wenn Sie kein Azure Storage-Konto besitzen, finden Sie im Artikel [Erstellen eines Speicherkontos](../storage/common/storage-create-storage-account.md#create-a-storage-account) Schritte zum Erstellen eines solchen Kontos.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im Menü auf der linken Seite auf **NEU**, und klicken Sie auf **Data + Analytics** und dann auf **Data Factory**. 
   
   ![Neu -> Data Factory](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** die Werte wie im folgenden Screenshot gezeigt an:
      
     ![Seite „Neue Data Factory“](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
   * **Name**. Geben Sie einen globalen, eindeutigen Namen für die Data Factory ein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
            `Data factory name "LoadSQLDWDemo" is not available`

    * **Abonnement**. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
    * **Ressourcengruppe**. Wählen Sie eine vorhandene Ressourcengruppe aus der Dropdownliste aus, oder wählen Sie die Option **Neu erstellen** aus, und geben Sie dann den Namen einer Ressourcengruppe ein. Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
    * **Version**. Wählen Sie **V2 (Vorschau)** aus.
    * **Standort**. Wählen Sie den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) können sich an anderen Standorten bzw. in anderen Regionen befinden. 

3. Klicken Sie auf **Create**.
4. Nach Abschluss der Erstellung navigieren Sie zu Ihrer Data Factory. Die Seite **Data Factory** wird wie in der Abbildung gezeigt angezeigt. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Datenintegrationsanwendung auf einer separaten Registerkarte zu starten.
   
   ![Data Factory-Startseite](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

## <a name="load-data-into-azure-sql-data-warehouse"></a>Laden von Daten in Azure SQL Data Warehouse

1. Klicken Sie auf der Seite mit den ersten Schritten auf die Kachel **Daten kopieren**, um das Tool zum Kopieren von Daten zu starten. 

   ![Kachel für das Tool zum Kopieren von Daten](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. Geben Sie auf der Seite **Eigenschaften** des Tools zum Kopieren von Daten als **Aufgabenname** den Namen **CopyFromSQLToSQLDW** ein, und klicken Sie dann auf **Weiter**. 

    ![Tool zum Kopieren von Daten – Seite „Eigenschaften“](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. Wählen Sie auf der Seite **Quelldatenspeicher** die Option **Azure SQL-Datenbank** aus, und klicken Sie dann auf **Weiter**.

    ![Seite „Quelldatenspeicher“](./media/load-azure-sql-data-warehouse/specify-source.png)
4. Führen Sie auf der Seite **Azure SQL-Datenbank angeben** die folgenden Schritte aus: 
    1. Wählen Sie unter **Servername** Ihre Azure SQL Server-Instanz aus.
    2. Wählen Sie unter **Datenbankname** Ihre Azure SQL-Datenbank aus.
    3. Geben Sie im Feld **Benutzername** den Namen des Benutzers ein.
    4. Geben Sie im Feld **Kennwort** das Kennwort des Benutzers ein.
    5. Klicken Sie auf **Weiter**. 

        ![Angeben der Azure SQL-Datenbank](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. Filtern Sie die Tabellen auf der Seite **Tabellen auswählen, aus denen die Daten kopiert werden sollen, oder eine benutzerdefinierte Abfrage verwenden**, indem Sie **SalesLT** im Eingabefeld angeben. Aktivieren Sie dann das Kontrollkästchen **(Alles auswählen)**, um alle Tabellen auszuwählen, und klicken Sie anschließend auf **Weiter**. 

    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. Wählen Sie auf der Seite **Zieldatenspeicher** die Option **Azure SQL Data Warehouse** aus, und klicken Sie dann auf **Weiter**. 

    ![Seite „Zieldatenspeicher“](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. Führen Sie auf der Seite **Azure SQL Data Warehouse angeben** die folgenden Schritte aus: 

    1. Wählen Sie unter **Servername** Ihre Azure SQL Server-Instanz aus.
    2. Wählen Sie im Feld **Datenbankname** Ihr Azure SQL Data Warehouse aus.
    3. Geben Sie im Feld **Benutzername** den Namen des Benutzers ein.
    4. Geben Sie im Feld **Kennwort** das Kennwort des Benutzers ein.
    5. Klicken Sie auf **Weiter**. 

        ![Angeben von Azure SQL Data Warehouse](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. Überprüfen Sie die Angaben auf der Seite **Tabellenmapping**, und klicken Sie dann auf **Weiter**. Eine intelligente Tabellenzuordnung wird angezeigt, die auf der Grundlage von Tabellennamen die Quell- und Zieltabellen zuordnet. Wenn die Tabelle im Ziel nicht vorhanden ist, wird sie standardmäßig von Azure Data Factory mit dem gleichen Namen erstellt. Sie können auch eine Zuordnung zu einer vorhandenen Tabelle wählen. 

    > [!NOTE]
    > Das automatische Erstellen der Tabelle für die SQL Data Warehouse-Senke tritt auf, wenn SQL Server oder Azure SQL-Datenbank als Quelle fungieren. Wenn Sie Daten aus einem anderen Quelldatenspeicher kopieren, müssen Sie das Schema vorab in der Azure SQL Data Warehouse-Senke erstellen, bevor die Daten kopiert werden.

    ![Seite „Tabellenmapping“](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. Überprüfen Sie die Angaben auf der Seite **Schemazuordnung**, und klicken Sie dann auf **Weiter**. Die intelligente Zuordnung basiert auf Spaltennamen. Wenn Sie Data Factory die Tabellen automatisch erstellen lassen möchten, kann bei Bedarf eine ordnungsgemäße Datentypkonvertierung erfolgen, um die Inkompatibilität zwischen Quell- und Zielspeichern zu beseitigen. Wenn es eine nicht unterstützte Datentypkonvertierung zwischen der Quell- und Zielspalte gibt, wird eine Fehlermeldung zusammen mit der entsprechenden Tabelle angezeigt.

    ![Seite „Schemazuordnung“](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. Wählen Sie auf der Seite **Einstellungen** einen Azure Storage-Speicher aus der Dropdownliste **Speicherkontoname** aus. Er wird für das Staging der Daten verwendet, bevor diese mit PolyBase in SQL Data Warehouse geladen werden. Nach Abschluss des Kopiervorgangs werden die vorläufigen Daten im Speicher automatisch bereinigt. 

    Deaktivieren Sie unter „Erweiterte Einstellungen“ die Option „Typstandard verwenden“.

    ![Seite "Einstellungen"](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie auf **Weiter**.

    ![Seite „Zusammenfassung“](./media/load-azure-sql-data-warehouse/summary-page.png)
13. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline (Aufgabe) zu überwachen.

    ![Seite „Bereitstellung“](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. In der Spalte **Aktionen** werden Links zum Anzeigen von Aktivitätsausführungsdetails und zum erneuten Ausführen der Pipeline angezeigt. 

    ![Überwachen der Pipelineausführungen](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Wenn Sie mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf **View Activity Runs** (Aktivitätsausführungen anzeigen). Es sind 10 Kopieraktivitäten in der Pipeline vorhanden, die jeweils eine Tabelle mit Daten kopieren. Klicken Sie auf **Pipelines**, um zurück zur Ansicht mit den Pipelineausführungen zu wechseln. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. 

    ![Überwachung der Aktivitätsausführungen](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Sie können die Ausführungsdetails jeder Kopieraktivität weiter überwachen, indem Sie in der Aktivitätsüberwachungsansicht unter **Aktionen** auf den Link **Details** klicken. Es werden Informationen wie z.B. die Menge der Daten, die aus der Quelle in die Senke kopiert wurden, der Durchsatz, die Schritte, die durchlaufen werden mit der entsprechenden Dauer sowie die verwendeten Konfigurationen angezeigt.

    ![Überwachen der Details zur Aktivitätsausführung](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den folgenden Artikel, um mehr über die Unterstützung von Azure SQL Data Warehouse zu erfahren: 

> [!div class="nextstepaction"]
>[Azure SQL Data Warehouse-Connector](connector-azure-sql-data-warehouse.md)
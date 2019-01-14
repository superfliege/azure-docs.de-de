---
title: Migrieren von SQL Server Integration Services-Paketen in Azure | Microsoft Dokumentation
description: 'Lernen Sie Folgendes: Migrieren von SQL Server Integration Services-Paketen in Azure'
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 12/15/2018
ms.openlocfilehash: 2fa37d31e984399c1b676cf6c6112617fac43fd9
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53721936"
---
# <a name="migrate-sql-server-integration-services-packages-to-azure"></a>Migrieren von SQL Server Integration Services-Paketen in Azure
Wenn Sie SQL Server Integration Services (SSIS) verwenden und Ihre SSIS-Projekte/-Pakete aus der vom SQL Server gehosteten SSIS-Quelldatenbank in die vom Azure SQL Datenbankserver oder von einer verwaltete Azure SQL-Datenbank-Instanz gehostete SSIS-Zieldatenbank migrieren möchten, können Sie diese mithilfe des Bereitstellungs-Assistenten für Integration Services erneut bereitstellen. Den Assistenten können Sie direkt in SQL Server Management Studio (SSMS) starten.

Wenn Sie eine ältere Version als SSIS 2012 verwenden, müssen Sie Ihre SSIS-Projekte/-Pakete mithilfe des Bereitstellungs-Assistenten für Integration Services konvertieren, bevor Sie die Projekte/Pakete im Projektbereitstellungsmodell erneut bereitstellen können. Der Bereitstellungs-Assistent kann ebenfalls direkt in SSMS gestartet werden. Weitere Informationen finden Sie im Artikel [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Azure Database Migration Service (DMS) unterstützt momentan die Migration einer SSIS-Quelldatenbank nicht. Sie können Ihre SSIS-Projekte/-Pakete jedoch mithilfe des folgenden Prozesses erneut bereitstellen. 

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
> * Bewerten von SSIS-Quellprojekten/-paketen.
> * Migrieren von SSIS-Projekten/-Paketen zu Azure

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen dieser Schritte benötigen Sie Folgendes:

- SSMS in der Version 17.2 oder höher.
- Eine Instanz Ihres Datenbankzielservers, um die SSIS-Datenbank zu hosten.
 
  Wenn Sie noch keine haben:
    - Erstellen Sie einen Azure SQL-Datenbankserver (ohne eine Datenbank) für die Azure SQL-Datenbank mithilfe des Azure-Portals, indem Sie zum SQL-Server-[Formular](https://ms.portal.azure.com/#create/Microsoft.SQLServer) (nur logischer Server) navigieren.
    - Eine verwaltete Azure SQL-Datenbank-Instanz erstellen Sie, indem Sie die Anweisungen im Artikel [Schnellstart: Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started) befolgen.

- SSIS muss in Azure Data Factory (ADF) bereitgestellt werden. Dieser Dienst beinhaltet Azure-SSIS Integration Runtime (IR), wobei die SSIS-Zieldatenbank von der Instanz des Azure SQL-Datenbankservers (wie im Artikel [Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) beschrieben) oder von der verwaltete Azure SQL-Datenbank-Instanz (wie im Artikel [Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime) beschrieben) gehostet wird. 

## <a name="assess-source-ssis-projectspackages"></a>Bewerten von SSIS-Quellprojekten/-paketen
Während das Bewerten von SSIS-Quelldatenbanken momentan weder im Datenmigrations-Assistent (DMA) noch im Azure Database Migration Service (DMS) integriert ist, werden Ihre SSIS-Projekte/-Pakete bewertet/überprüft, wenn Sie in der vom Azure SQL-Datenbankserver oder von der verwaltete Azure SQL-Datenbank-Instanz gehosteten SSIS-Zieldatenbank erneut bereitgestellt werden.

## <a name="migrate-ssis-projectspackages"></a>Migrieren von SSIS-Projekten/-Paketen
Wenn Sie SSIS-Projekte/-Pakete in den Azure SQL-Datenbankserver oder in die verwaltete Azure SQL-Datenbank-Instanz migrieren möchten, führen Sie die folgenden Schritte aus.

1.  Öffnen Sie SSMS, und wählen Sie dann **Optionen** aus, um das Dialogfeld **Mit Server verbinden** anzuzeigen.

2.  Geben Sie auf dem Tab **Login** die Informationen an, die nötig sind, um eine Verbindung zum Azure SQL-Datenbankserver oder der verwalteten Azure SQL-Datenbank-Instanz herzustellen, die die SSIS Zieldatenbank hostet.

    ![SSIS-Tab „Login“](media/how-to-migrate-ssis-packages/dms-ssis-login-tab.png)
 
3.  Wählen Sie auf dem Tab **Verbindungseigenschaften** im Textfeld **Mit Datenbank verbinden** **SSISDB** aus oder geben Sie dies ein, und klicken Sie dann auf **Verbinden**.

    ![SSIS-Tab „Verbindungseigenschaften“](media/how-to-migrate-ssis-packages/dms-ssis-conncetion-properties-tab.png)

4.  Erweitern Sie im SSMS-Objekt-Explorer den Knoten **Integration Services-Kataloge**, erweitern Sie dann **SSISDB**, und klicken Sie dann – sofern es keine bereits vorhandenen Ordner gibt – mit der rechten Maustaste auf **SSISDB**, und erstellen Sie einen neuen Ordner.

5.  Erweitern Sie unter **SSISDB** einen beliebigen Ordner, klicken Sie mit der rechten Maustaste auf **Projekte**, und wählen Sie dann **Projekt bereitstellen** aus.

    ![Erweiterter SSISDB-Knoten in SSIS](media/how-to-migrate-ssis-packages/dms-ssis-ssisdb-node-expanded.png)

6.  Lesen Sie sich die Informationen auf der Seite **Einführung** im Bereitstellungs-Assistent für Integration Services durch, und klicken Sie dann auf **Weiter**.

    ![Seite „Einführung“ im Bereitstellungs-Assistenten](media/how-to-migrate-ssis-packages/dms-deployment-wizard-introduction-page.png)

7.  Bestimmen Sie auf der Seite **Quelle auswählen** das vorhandene SSIS-Projekt, das Sie bereitstellen möchten.

    Wenn SSMS auch mit dem SQL Server verbunden ist, der die SSIS Quelldatenbank hostet, wählen Sie **Integration Services-Katalog** aus, und geben Sie dann in Ihrem Katalog den Servernamen und den Projektpfad ein, um Ihr Projekt direkt bereitzustellen.

    Alternativ können Sie auch die **Projektbereitstellungsdatei** auswählen und dann den Pfad zu einer vorhandenen Projektbereitstellungsdatei (.ispac) festlegen, um Ihr Projekt bereitzustellen.

    ![Seite „Quelle auswählen“ im Bereitstellungs-Assistenten](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-source-page.png)
 
8.  Klicken Sie auf **Weiter**.
9.  Geben Sie auf der Seite **Ziel auswählen** das Ziel für Ihr Projekt an.

       a. Geben Sie im Textfeld „Servername“ den vollqualifizierten Servernamen der Azure SQL-Datenbank (<server_name>.database.windows.net) oder den Namen der verwalteten Azure SQL-Datenbank-Instanz ein (<server_name.dns_prefix>.database.windows.net).
 
       b. Geben Sie die Authentifizierungsinformationen ein, und klicken Sie dann auf **Verbinden**.
    
       ![Seite „Ziel auswählen“ im Bereitstellungs-Assistenten](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-destination-page.png)

    c. Klicken Sie auf „Durchsuchen“, um den Zielordner in der SSIS-Datenbank zu bestimmen, und klicken Sie dann auf „Weiter“.

    > [!NOTE]
    > Die Schaltfläche **Weiter** wird erst aktiviert, wenn Sie zuvor auf **Verbinden** geklickt haben. 

10. Sehen Sie sich auf der Seite **Review** (Überprüfen) alle Fehler/Warnungen an, und passen Sie Ihre Pakete ggf. entsprechend an.

    ![Seite „Review“ (Überprüfen) im Bereitstellungs-Assistenten](media/how-to-migrate-ssis-packages/dms-deployment-wizard-validate-page.png)

11. Klicken Sie auf **Weiter**.

12. Sehen Sie sich auf der Seite **Review** Ihre Bereitstellungseinstellungen noch einmal an.

    > [!NOTE]
    > Sie können die Einstellungen ändern, indem Sie entweder auf „Zurück“ klicken, oder indem Sie im linken Bereich einen der Links zurück zu den einzelnen Schritten auswählen.

13. Klicken Sie auf **Bereitstellen**, um den Bereitstellungsprozess zu starten.

14. Wenn der Bereitstellungsprozess beendet ist, können Sie sich die Ergebnisseite ansehen, auf der für jede einzelne Bereitstellungsaktion angezeigt wird, ob diese erfolgreich war oder fehlgeschlagen ist.
    a. Sollte eine Aktion fehlgeschlagen sein, klicken Sie in der Spalte **Ergebnisse** auf **Fehlgeschlagen**, um eine Erklärung des Fehlers anzuzeigen.
    b. Optional können Sie auf **Bericht speichern** klicken, um die Ergebnisse in einer XML-Datei zu speichern.

15. Klicken Sie abschließend auf **Schließen**, um den Bereitstellungs-Assistenten für Integration Services zu verlassen.

Wenn die Bereitstellung Ihres Projekts problemlos durchgeführt werden konnte, können Sie alle im Projekt enthaltenen Pakete auswählen, um diese auf Ihrer Azure-SSIS IR auszuführen.

## <a name="next-steps"></a>Nächste Schritte
- Überprüfen der Migrationsanleitung im [Leitfaden zur Datenbankmigration von Microsoft](https://datamigration.microsoft.com/)
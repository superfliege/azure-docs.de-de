---
title: Bereitstellen einer SSIS Integration Runtime per Azure Data Factory | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie eine Azure-SSIS Integration Runtime mithilfe von Azure Data Factory erstellen.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: hero-article
ms.date: 01/29/2018
ms.author: spelluru
ms.openlocfilehash: 9e0b0efdf28e6d8b99d1cdf702dd0698ad87da7b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="provision-an-azure-ssis-integration-runtime-by-using-the-azure-data-factory-ui"></a>Bereitstellen einer Azure-SSIS Integration Runtime über die Azure Data Factory-Benutzeroberfläche
In diesem Tutorial werden die Schritte zum Bereitstellen einer Azure SSIS Integration Runtime (IR) in Azure Data Factory mithilfe des Azure-Portals beschrieben. Anschließend können Sie SQL Server Data Tools oder SQL Server Management Studio zum Bereitstellen von SQL Server Integration Services-Paketen (SSIS) für diese Runtime in Azure verwenden. Konzeptionelle Informationen zu Azure-SSIS IRs finden Sie unter [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime).

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen und Starten einer Azure-SSIS Integration Runtime

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, hilft Ihnen die [Einführung in Data Factory für Version 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) weiter.


## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. 
- **Azure SQL-Datenbankserver**. Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. Azure Data Factory erstellt den SSIS-Katalog (SSISDB) auf diesem Datenbankserver. Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in die SSISDB zu schreiben, ohne die Grenzen von Azure-Regionen zu überschreiten. 
   - Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für den Datenbankserver aktiviert ist. Weitere Informationen finden Sie unter [Schützen der Azure SQL-Datenbank](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Informationen zum Aktivieren dieser Einstellung mithilfe von PowerShell finden Sie unter [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
   - Fügen Sie der Client-IP-Adressenliste in den Firewalleinstellungen für den Datenbankserver die IP-Adresse des Clientcomputers oder einen IP-Adressbereich hinzu, der die IP-Adresse des Clientcomputers enthält. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md).
   - Vergewissern Sie sich, dass Ihr Azure SQL-Datenbankserver keinen SSIS-Katalog (SSISDB) aufweist. Für die Bereitstellung einer Azure-SSIS IR wird die Verwendung eines vorhandenen SSIS-Katalogs nicht unterstützt.
 
## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.    
2. Klicken Sie im linken Menü auf **Neu** und anschließend auf **Daten + Analysen** und **Data Factory**. 
   
   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **MyAzureSsisDataFactory** ein. 
      
   ![Seite „Neue Data Factory“](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss *global eindeutig*sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in **&lt;<IhrName>MyAzureSsisDataFactory&gt;**), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
   `Data factory name “MyAzureSsisDataFactory” is not available`
4. Wählen Sie unter **Abonnement** Ihr Azure-Abonnement aus, in dem die Data Factory erstellt werden soll. 
5. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
     
   - Wählen Sie die Option **Use existing** (Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
   - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
   Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
6. Wählen Sie **V2 (Vorschau)** als **Version** aus.
7. Wählen Sie unter **Standort** den Standort für die Data Factory aus. Die Liste enthält nur Standorte, die für die Erstellung von Data Factorys geeignet sind.
8. Wählen Sie die Option **An Dashboard anheften** aus.     
9. Klicken Sie auf **Erstellen**.
10. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status **Deploying data factory** (Data Factory wird bereitgestellt...): 

   ![Kachel „Deploying Data Factory“ (Data Factory wird bereitgestellt...)](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
11. Nach Abschluss der Erstellung wird die Seite **Data Factory** angezeigt.
   
   ![Startseite für die Data Factory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
12. Wählen Sie **Überwachung und Verwaltung**, um die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu öffnen. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Bereitstellen einer Azure-SSIS Integration Runtime

1. Wählen Sie auf der Seite **Let's get started** (Erste Schritte) die Kachel **Configure SSIS Integration Runtime** (SSIS Integration Runtime konfigurieren). 

   ![Kachel „Configure SSIS Integration Runtime“ (SSIS Integration Runtime konfigurieren)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Führen Sie auf der Seite **Allgemeine Einstellungen** von **Integration Runtime Setup** (Integration Runtime-Setup) die folgenden Schritte aus: 

   ![Allgemeine Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. Geben Sie unter **Name** einen Namen für die Integration Runtime ein.

   b. Wählen Sie unter **Standort** den Standort für die Integration Runtime aus. Es werden nur unterstützte Standorte angezeigt.

   c. Wählen Sie unter **Größe des Knotens** die Größe des Knotens aus, der mit der SSIS Runtime konfiguriert werden soll.

   d. Geben Sie unter **Node Number** (Anzahl von Knoten) die Anzahl von im Cluster enthaltenen Knoten an.
   
   e. Klicken Sie auf **Weiter**. 
3. Führen Sie auf der Seite **SQL-Einstellungen** die folgenden Schritte aus: 

   ![SQL-Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Geben Sie unter **Abonnement** das Azure-Abonnement an, das über den Azure-Datenbankserver verfügt.

   b. Wählen Sie unter **Catalog Database Server Endpoint** (Katalogdatenbank-Serverendpunkt) Ihren Azure-Datenbankserver aus.

   c. Geben Sie unter **Administratorbenutzername** den Benutzernamen des Administrators ein.

   d. Geben Sie unter **Administratorkennwort** das Kennwort für den Administrator ein.

   e. Wählen Sie unter **Catalog Database Server Tier** (Katalogdatenbank-Serverebene) die Dienstebene für die SSISDB aus. Der Standardwert lautet **Basic**.

   f. Klicken Sie auf **Weiter**. 
4. Wählen Sie auf der Seite **Erweiterte Einstellungen** einen Wert für **Maximum Parallel Executions Per Node** (Maximale Anzahl von parallelen Ausführungen pro Knoten) aus.   

   ![Erweiterte Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Dieser Schritt ist *optional*. Wenn Sie über ein virtuelles Netzwerk verfügen (Erstellung per klassischem Bereitstellungsmodell oder Azure Resource Manager), mit dem die Integration Runtime verknüpft werden soll, aktivieren Sie das Kontrollkästchen **Select a VNet for your Azure-SSIS integration runtime to join and allow Azure services to configure VNet permissions/settings** (VNET für den Beitritt der Azure-SSIS Integration Runtime auswählen und Azure-Diensten die Konfiguration von VNET-Berechtigungen/-Einstellungen erlauben). Führen Sie anschließend die folgenden Schritte aus: 

   ![Erweiterte Einstellungen mit einem virtuellen Netzwerk](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

   a. Geben Sie unter **Abonnement** das Abonnement an, das über das virtuelle Netzwerk verfügt.

   b. Wählen Sie unter **VNET-NAME** den Namen des virtuellen Netzwerks aus.

   c. Wählen Sie unter **Subnetzname** den Namen des Subnetzes im virtuellen Netzwerk aus. 
6. Wählen Sie **Fertig stellen**, um die Erstellung der Azure-SSIS Integration Runtime zu starten. 

   > [!IMPORTANT]
   > Dieser Prozess dauert etwa 20 Minuten.
   >
   > Der Data Factory-Dienst stellt die Verbindung mit Ihrer Azure SQL-Datenbank her, um den SSIS-Katalog (SSISDB) vorzubereiten. Mit dem Skript werden, falls angegeben, auch Berechtigungen und Einstellungen für Ihr virtuelles Netzwerk konfiguriert. Außerdem wird die neue Instanz der Azure-SSIS Integration Runtime mit dem virtuellen Netzwerk verknüpft.
   > 
   > Wenn Sie eine Instanz einer Azure-SSIS IR bereitstellen, werden auch das Azure Feature Pack für SSIS und die Access Redistributable-Komponente installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die von den integrierten Komponenten unterstützt werden. Es ist derzeit nicht möglich, für SSIS Drittanbieterkomponenten zu installieren. (Diese Einschränkung gilt auch für Drittanbieterkomponenten von Microsoft, z.B. Oracle- und Teradata-Komponenten von Attunity und die SAP BI-Komponenten).

7. Wechseln Sie auf der Registerkarte **Verbindungen** ggf. zu **Integration Runtimes**. Wählen Sie zum Aktualisieren des Status die Option **Aktualisieren**. 

   ![Erstellungsstatus, Schaltfläche „Aktualisieren“](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Über die Links in der Spalte **Aktionen** können Sie die Integration Runtime beenden/starten, bearbeiten oder löschen. Über den letzten Link können Sie den JSON-Code für die Integration Runtime anzeigen. Die Schaltflächen zum Bearbeiten und Löschen sind nur verfügbar, wenn die IR beendet wird. 

   ![Links in der Spalte „Aktionen“](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        

## <a name="create-an-azure-ssis-integration-runtime"></a>Erstellen einer Azure SSIS Integration Runtime

1. Wechseln Sie auf der Azure Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, wählen Sie die Option **Verbindungen**, und wechseln Sie dann zur Registerkarte **Integration Runtimes**, um vorhandene Integration Runtimes in Ihrer Data Factory anzuzeigen. 
   ![Auswahl zum Anzeigen von vorhandenen IRs](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
2. Wählen Sie **Neu**, um eine Azure-SSIS IR zu erstellen. 

   ![Integration Runtime über das Menü](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
3. Wählen Sie im Fenster für die **Integration Runtime-Einrichtung** die Option **Lift-and-shift existing SSIS packages to execute in Azure** (Migrieren vorhandener SSIS-Pakete per Lift & Shift für die Ausführung in Azure) und dann **Weiter**.

   ![Angeben des Integration Runtime-Typs](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Die übrigen Schritte zum Einrichten einer Azure-SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure-SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

 
## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen
Verwenden Sie jetzt SQL Server Data Tools oder SQL Server Management Studio, um Ihre SSIS-Pakete in Azure bereitzustellen. Stellen Sie eine Verbindung mit Ihrem Azure-Datenbankserver her, auf dem der SSIS-Katalog (SSISDB) gehostet wird. Der Name des Azure-Datenbankservers hat das folgende Format: `<servername>.database.windows.net` (für Azure SQL-Datenbank). 

Informationen finden Sie in den folgenden Artikeln der SSIS-Dokumentation: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (Bereitstellen, Ausführen und Überwachen eines SSIS-Pakets in Azure)   
- [Herstellen einer Verbindung mit der SSISDB-Katalogdatenbank in Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Schedule the execution of an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Planen der Ausführung eines SSIS-Pakets in Azure)
- [Connect to on-premises data sources with Windows Authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Herstellen einer Verbindung mit lokalen Datenquellen mit Windows-Authentifizierung) 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen und Starten einer Azure-SSIS Integration Runtime

Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Daten von einem lokalen Speicherort in die Cloud kopieren: 

> [!div class="nextstepaction"]
> [Kopieren von Daten in die Cloud](tutorial-copy-data-portal.md)

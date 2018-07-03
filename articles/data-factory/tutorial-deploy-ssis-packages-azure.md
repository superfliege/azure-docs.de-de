---
title: Bereitstellen der Azure-SSIS-Integration Runtime | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie zur Bereitstellung und Ausführung von SSIS-Paketen in Azure die Azure-SSIS-Integration Runtime in Azure Data Factory bereitstellen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: hero-article
ms.date: 06/27/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 2b9644b8cfdcca9c9ded7890e325fe9488c9511f
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061109"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory
In diesem Tutorial werden die Schritte zum Bereitstellen einer Azure SSIS Integration Runtime (IR) in Azure Data Factory mithilfe des Azure-Portals beschrieben. Anschließend können Sie SQL Server Data Tools oder SQL Server Management Studio zum Bereitstellen und Ausführen von SQL Server Integration Services-Paketen (SSIS) in dieser Runtime in Azure verwenden. Konzeptionelle Informationen zu Azure-SSIS IRs finden Sie unter [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime).

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Bereitstellen einer Azure-SSIS Integration Runtime

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. 
- **Azure SQL-Datenbankserver**. Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. Azure Data Factory erstellt den SSIS-Katalog (SSISDB) auf diesem Datenbankserver. Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in die SSISDB zu schreiben, ohne die Grenzen von Azure-Regionen zu überschreiten. 
- Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als einzelne Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz (Vorschau) erstellt werden und in einem öffentlichen Netzwerk oder durch Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. Wenn Sie Azure SQL-Datenbank mit Dienstendpunkten im virtuellen Netzwerk oder einer verwalteten Instanz (Vorschau) zum Hosten von SSISDB verwenden oder Zugriff auf lokale Daten benötigen, müssen Sie Ihre Azure-SSIS-IR zu einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
- Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für den Datenbankserver aktiviert ist. Dies gilt nicht, wenn Sie Azure SQL-Datenbank mit Dienstendpunkten im virtuellen Netzwerk oder einer verwalteten Instanz (Vorschau) zum Hosten von SSISDB verwenden. Weitere Informationen finden Sie unter [Schützen der Azure SQL-Datenbank](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Informationen zum Aktivieren dieser Einstellung mithilfe von PowerShell finden Sie unter [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1). 
- Fügen Sie der Client-IP-Adressenliste in den Firewalleinstellungen für den Datenbankserver die IP-Adresse des Clientcomputers oder einen IP-Adressbereich hinzu, der die IP-Adresse des Clientcomputers enthält. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md). 
- Sie können eine Verbindung mit dem Datenbankserver herstellen, indem Sie die SQL-Authentifizierung mit Ihren Serveradministrator-Anmeldeinformationen oder die Azure Active Directory-Authentifizierung (AAD) mit der verwalteten Dienstidentität (Managed Service Identity, MSI) von Azure Data Factory (ADF) verwenden.  Für letztere müssen Sie die verwaltete Dienstidentität von Azure Data Factory zu einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Erstellen einer Azure-SSIS-Integration Runtime in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
- Vergewissern Sie sich, dass Ihr Azure SQL-Datenbankserver keinen SSIS-Katalog (SSISDB) aufweist. Für die Bereitstellung einer Azure-SSIS IR wird die Verwendung eines vorhandenen SSIS-Katalogs nicht unterstützt. 

> [!NOTE]
> - Eine Data Factory kann in folgenden Regionen erstellt werden: „USA, Osten“, „USA, Osten 2“, „Asien, Südosten“ und „Europa, Westen“. 
> - Eine Azure-SSIS-IR kann in folgenden Regionen erstellt werden: „USA, Osten, „USA, Osten 2“, „USA, Mitte“, „USA, Westen 2“, Europa, Norden“, „Europa, Westen“, „Vereinigtes Königreich, Süden“ und „Australien, Osten“. 

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Starten Sie den Webbrowser **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt. 
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. 
3. Klicken Sie im linken Menü auf **Neu** und anschließend auf **Daten + Analysen** und **Data Factory**. 

   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

4. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **MyAzureSsisDataFactory** ein. 

   ![Seite „Neue Data Factory“](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   Der Name der Azure Data Factory muss *global eindeutig*sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in **&lt;<IhrName>MyAzureSsisDataFactory&gt;**), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md). 

   `Data factory name “MyAzureSsisDataFactory” is not available`

5. Wählen Sie unter **Abonnement** Ihr Azure-Abonnement aus, in dem die Data Factory erstellt werden soll. 
6. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus: 

   - Wählen Sie die Option **Use existing** (Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
   - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein. 

   Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md). 
7. Wählen Sie **V2 (Vorschau)** als **Version** aus. 
8. Wählen Sie unter **Standort** den Standort für die Data Factory aus. Die Liste enthält nur Standorte, die für die Erstellung von Data Factorys geeignet sind. 
9. Wählen Sie die Option **An Dashboard anheften** aus. 
10. Klicken Sie auf **Erstellen**. 
11. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status **Deploying data factory** (Data Factory wird bereitgestellt...): 

   ![Kachel „Deploying Data Factory“ (Data Factory wird bereitgestellt...)](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

12. Nach Abschluss der Erstellung wird die Seite **Data Factory** angezeigt. 

   ![Startseite für die Data Factory](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

13. Wählen Sie **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu öffnen. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Bereitstellen einer Azure-SSIS Integration Runtime

1. Wählen Sie auf der Seite **Let's get started** (Erste Schritte) die Kachel **Configure SSIS Integration Runtime** (SSIS Integration Runtime konfigurieren). 

   ![Kachel „Configure SSIS Integration Runtime“ (SSIS Integration Runtime konfigurieren)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. Führen Sie auf der Seite **Allgemeine Einstellungen** von **Integration Runtime Setup** (Integration Runtime-Setup) die folgenden Schritte aus: 

   ![Allgemeine Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. Geben Sie unter **Name** den Namen Ihrer Integration Runtime ein. 

   b. Geben Sie unter **Beschreibung** die Beschreibung Ihrer Integration Runtime ein. 

   c. Wählen Sie unter **Standort** den Standort für Ihre Integration Runtime aus. Es werden nur unterstützte Standorte angezeigt. Sie sollten den gleichen Standort wie für den Datenbankserver zum Hosten von SSISDB auswählen. 

   d. Wählen Sie unter **Knotengröße** die Größe des Knotens in Ihrem Integration Runtime-Cluster aus. Nur unterstützte Knotengrößen werden angezeigt. Wählen Sie eine große Knotengröße (zentrales Hochskalieren) aus, wenn Sie zahlreiche compute-/arbeitsspeicherintensive Pakete ausführen möchten. 

   e. Wählen Sie unter **Knotenzahl** die Anzahl von Knoten in Ihrer Integration Runtime aus. Nur unterstützte Knotenzahlen werden angezeigt. Wählen Sie einen großen Cluster mit zahlreichen Knoten (horizontales Hochskalieren) aus, wenn Sie viele Pakete parallel ausführen möchten. 

   f. Wählen Sie unter **Edition/Lizenz** die SQL Server-Edition/-Lizenz für Ihre Integration Runtime aus: Standard oder Enterprise. Wählen Sie Enterprise aus, wenn Sie erweiterte Funktionen oder Premium-Funktionen für Ihre Integration Runtime verwenden möchten. 

   g. Wählen Sie unter **Sparen Sie Geld** die Option „Azure-Hybridvorteil“ (Azure Hybrid Benefit, AHB) für Ihre Integration Runtime aus: „Ja“ oder „Nein“. Wählen Sie „Ja“, wenn Sie eine eigene SQL Server-Lizenz mit Software Assurance verwenden möchten, um bei der Hybridnutzung von Kostenersparnissen zu profitieren. 

   h. Klicken Sie auf **Weiter**. 

3. Führen Sie auf der Seite **SQL-Einstellungen** die folgenden Schritte aus: 

   ![SQL-Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Geben Sie unter **Abonnement** das Azure-Abonnement mit dem Azure-Datenbankserver zum Hosten von SSISDB an. 

   b. Wählen Sie als **Standort** den Standort des Datenbankservers zum Hosten von SSISDB aus. Sie sollten denselben Standort wie den für die Integration Runtime auswählen. 

   c. Wählen Sie unter **Katalogdatenbank-Serverendpunkt** den Endpunkt Ihres Datenbankservers zum Hosten von SSISDB aus. Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als eigenständige Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz (Vorschau) erstellt werden und in einem öffentlichen Netzwerk oder durch Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten von SSISDB finden Sie unter [Erstellen der Azure-SSIS Integration Runtime in Azure Data Factory](create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview). Wenn Sie Azure SQL-Datenbank mit Dienstendpunkten im virtuellen Netzwerk oder einer verwalteten Instanz (Vorschau) zum Hosten von SSISDB auswählen oder Zugriff auf lokale Daten benötigen, müssen Sie Ihre Azure-SSIS-IR zu einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Erstellen einer Azure-SSIS-Integration Runtime in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 

   d. Wählen Sie mit dem Kontrollkästchen **Use AAD authentication...** (AAD-Authentifizierung mit der MSI von ADF verwenden) die Authentifizierungsmethode für Ihren Datenbankserver zum Hosten von SSISDB: SQL oder Azure Active Directory (AAD) mit Ihrer verwalteten Dienstidentität von Azure Data Factory. Aktivieren Sie dieses Kontrollkästchen, müssen Sie ADF-MSI zu einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Informationen finden Sie unter [Erstellen einer Azure-SSIS-Integration Runtime in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. Geben Sie unter **Administratorbenutzername** den Benutzernamen für die SQL-Authentifizierung für den Datenbankserver zum Hosten von SSISDB an. 

   f. Geben Sie unter **Administratorbenutzerkennwort** das Kennwort für die SQL-Authentifizierung für den Datenbankserver zum Hosten von SSISDB an. 

   g. Wählen Sie unter **Katalogdatenbank-Dienstebene** die Dienstebene für Ihren Datenbankserver zum Hosten von SSISDB aus: Basic/Standard/Premium oder Name des Pools für elastische Datenbanken. 

   h. Klicken Sie auf **Verbindung testen** und bei erfolgreichem Test auf **Weiter**. 

4. Führen Sie auf der Seite **Erweiterte Einstellungen** die folgenden Schritte aus: 

   ![Erweiterte Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. Legen Sie unter **Maximale Anzahl von parallelen Ausführungen pro Knoten** die maximale Anzahl von Paketen fest, die in Ihrem Integration Runtime-Cluster pro Knoten gleichzeitig ausgeführt werden sollen. Nur unterstützte Paketzahlen werden angezeigt. Wählen Sie eine niedrigere Anzahl aus, wenn Sie mehrere Kerne verwenden möchten, um ein einzelnes großes/umfangreiches Paket auszuführen, das compute-/arbeitsspeicherintensiv ist. Wählen Sie eine hohe Anzahl aus, wenn Sie mindestens ein kleines Paket in einem einzelnen Kern ausführen möchten. 

   b. Geben Sie unter **Container-SAS-URI des benutzerdefinierten Setups** optional den SAS-URI (Shared Access Signature, Uniform Resource Identifier) Ihres Azure Storage Blob-Containers an, in dem Ihr Einrichtungsskript und die zugehörigen Dateien gespeichert sind. Weitere Informationen finden Sie unter [Benutzerdefiniertes Setup von Azure-SSIS-Integration Runtime](https://docs.microsoft.com/en-us/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. Geben Sie mithilfe des Kontrollkästchens **Select a VNet...** (VNET für den Beitritt der Azure-SSIS-Integration Runtime auswählen und Azure-Diensten die Konfiguration von VNET-Berechtigungen/-Einstellungen erlauben) an, ob Ihre Integration Runtime zu einem virtuellen Netzwerk hinzugefügt werden soll. Sie sollten überprüfen, ob Sie Azure SQL-Datenbank mit Dienstendpunkten im virtuellen Netzwerk oder einer verwalteten Instanz (Vorschau) zum Hosten von SSISDB verwenden oder Zugriff auf lokale Daten benötigen. Weitere Informationen finden Sie unter [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 

5. Klicken Sie auf **Fertig stellen**, um die Erstellung der Integration Runtime zu starten. 

   > [!IMPORTANT]
   > Dieser Prozess dauert etwa 20 bis 30 Minuten.
   >
   > Der Data Factory-Dienst stellt die Verbindung mit Ihrem Azure SQL-Datenbankserver her, um den SSIS-Katalog (SSISDB) vorzubereiten. 
   > 
   > Wenn Sie eine Instanz einer Azure-SSIS IR bereitstellen, werden auch das Azure Feature Pack für SSIS und die Access Redistributable-Komponente installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die von den integrierten Komponenten unterstützt werden. Sie können auch weitere Komponenten installieren. Weitere Informationen finden Sie unter [Benutzerdefiniertes Setup von Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md). 

6. Wechseln Sie auf der Registerkarte **Verbindungen** ggf. zu **Integration Runtimes**. Wählen Sie zum Aktualisieren des Status die Option **Aktualisieren**. 

   ![Erstellungsstatus, Schaltfläche „Aktualisieren“](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

7. Über die Links in der Spalte **Aktionen** können Sie die Integration Runtime beenden/starten, bearbeiten oder löschen. Über den letzten Link können Sie den JSON-Code für die Integration Runtime anzeigen. Die Schaltflächen zum Bearbeiten und Löschen sind nur verfügbar, wenn die IR beendet wird. 

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
Verwenden Sie jetzt SQL Server Data Tools (SSDT) oder SQL Server Management Studio (SSMS), um Ihre SSIS-Pakete in Azure bereitzustellen. Stellen Sie eine Verbindung mit Ihrem Azure SQL-Datenbankserver her, auf dem der SSIS-Katalog (SSISDB) gehostet wird. Der Name des Azure SQL-Datenbankservers hat das folgende Format: `<servername>.database.windows.net`. 

Informationen finden Sie in den folgenden Artikeln der SSIS-Dokumentation: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (Bereitstellen, Ausführen und Überwachen eines SSIS-Pakets in Azure) 
- [Herstellen einer Verbindung mit der SSISDB-Katalogdatenbank in Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Schedule the execution of an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Planen der Ausführung eines SSIS-Pakets in Azure) 
- [Connect to on-premises data sources with Windows Authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Herstellen einer Verbindung mit lokalen Datenquellen mit Windows-Authentifizierung) 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Bereitstellen einer Azure-SSIS Integration Runtime

Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Daten von einem lokalen Speicherort in die Cloud kopieren: 

> [!div class="nextstepaction"]
> [Kopieren von Daten in die Cloud](tutorial-copy-data-portal.md)

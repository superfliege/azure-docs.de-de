---
title: Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft Docs
description: Hier erfahren Sie, wie Sie zur Bereitstellung und Ausführung von SSIS-Paketen in Azure die Azure-SSIS-Integration Runtime in Azure Data Factory erstellen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/23/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: e8fbc579d28f0aa3d5a19af66ecbe435156de6b1
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387651"
---
# <a name="create-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Erstellen der Azure-SSIS Integration Runtime in Azure Data Factory
In diesem Artikel werden die Schritte für die Bereitstellung einer Azure-SSIS-Integrationslaufzeit (Integration Runtime, IR) in Azure Data Factory beschrieben. Anschließend können Sie SQL Server Data Tools (SSDT) oder SQL Server Management Studio (SSMS) zum Bereitstellen und Ausführen von SQL Server Integration Services-Paketen (SSIS) in dieser Runtime in Azure verwenden. 

Im Tutorial [Bereitstellen von SQL Server Integration Services-Paketen in Azure](tutorial-create-azure-ssis-runtime-portal.md) wird erläutert, wie Sie eine Azure-SSIS-Integrationslaufzeit (Integration Runtime, IR) unter Verwendung von Azure SQL-Datenbank zum Hosten des SSIS-Katalogs erstellen. In diesem Artikel wird das Tutorial vertieft und erläutert, wie Sie die folgenden Aktionen ausführen: 

- Verwenden Sie optional Azure SQL-Datenbank mit Dienstendpunkten virtueller Netzwerke oder mit verwalteter Instanz als Datenbankserver zum Hosten Ihres SSIS-Katalogs (SSISDB-Datenbank). Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten von SSISDB finden Sie unter [Vergleich zwischen einem logischen SQL-Datenbank-Server und einer verwalteten SQL-Datenbank-Instanz](create-azure-ssis-integration-runtime.md#compare-sql-database-logical-server-and-sql-database-managed-instance). Als Voraussetzung müssen Sie Ihre Azure SSIS-IR mit einem virtuellen Netzwerk verknüpfen und Einstellungen und Berechtigungen für virtuelle Netzwerke wie erforderlich konfigurieren. Weitere Informationen finden Sie unter [Verknüpfen der Azure SSIS-IR mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

- Verwenden Sie optional AAD-Authentifizierung (Azure Active Directory) mit Ihren von Azure Data Factory verwalteten Identitäten für Azure-Ressourcen für Azure-SSIS IR, um eine Verbindung mit dem Datenbankserver herzustellen. Als Voraussetzung müssen Sie Ihre Data Factory-MSI einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Informationen finden Sie unter [Aktivieren der Azure Active Directory-Authentifizierung für die Azure SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

## <a name="overview"></a>Übersicht
Dieser Artikel zeigt verschiedene Methoden für die Bereitstellung einer Azure-SSIS IR auf: 

- [Azure-Portal](#azure-portal) 
- [Azure PowerShell](#azure-powershell) 
- [Azure Resource Manager-Vorlage](#azure-resource-manager-template) 

Data Factory stellt die Verbindung mit Ihrer Azure SQL-Datenbank her, um die SSIS-Katalogdatenbank (SSISDB) vorzubereiten, wenn Sie eine Azure-SSIS IR erstellen. Außerdem konfiguriert das Skript Berechtigungen und Einstellungen für das virtuelle Netzwerk – sofern angegeben – und verknüpft die neue Instanz der Azure SSIS Integration Runtime mit dem virtuellen Netzwerk. 

Wenn Sie eine Azure SSIS IR-Instanz bereitstellen, werden auch das Azure Feature Pack für SSIS und die Access Redistributable-Komponente installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die von den integrierten Komponenten unterstützt werden. Sie können auch weitere Komponenten installieren. Weitere Informationen finden Sie unter [Benutzerdefiniertes Setup von Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md). 

## <a name="prerequisites"></a>Voraussetzungen 
- **Azure-Abonnement**. Falls Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Testkonto](http://azure.microsoft.com/pricing/free-trial/) erstellen. 

- **Logischer Server mit Azure SQL-Datenbank oder verwaltete Instanz**. Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. Dieser Server hostet die SSIS-Katalog-Datenbank (SSISDB). Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in SSISDB zu schreiben, ohne Azure-Regionen zu überschreiten. Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als einzelne Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz erstellt werden und in einem öffentlichen Netzwerk oder durch das Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. Eine Liste mit den unterstützten Tarifen für Azure SQL-Datenbank finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](../sql-database/sql-database-resource-limits.md). 

    Vergewissern Sie sich, dass der logische Azure SQL-Datenbank-Server oder die verwaltete Instanz nicht bereits über einen SSIS-Katalog (SSIDB-Datenbank) verfügt. Für die Bereitstellung der Azure-SSIS IR wird die Verwendung eines vorhandenen SSIS-Katalogs nicht unterstützt. 

- **Klassisches virtuelles Netzwerk oder virtuelles Azure Resource Manager-Netzwerk (optional)**. Sie benötigen ein virtuelles Azure-Netzwerk, wenn mindestens eine der folgenden Bedingungen zutrifft: 
    - Sie hosten die SSIS-Katalogdatenbank in Azure SQL-Datenbank mit Dienstendpunkten virtueller Netzwerke/einer verwalteten Instanz, die sich in einem virtuellen Netzwerk befindet. 
    - Sie möchten aus SSIS-Paketen, die in einer Azure-SSIS-Integrationslaufzeit ausgeführt werden, eine Verbindung mit lokalen Datenspeichern herstellen. 

- **Azure PowerShell**. Befolgen Sie die Anweisungen in [Installieren von Azure PowerShell mit PowerShellGet](/powershell/azure/install-azurerm-ps), wenn Sie PowerShell verwenden, um ein Skript zum Bereitstellen von Azure-SSIS-Integration Runtime auszuführen, die SSIS-Pakete in der Cloud ausführt. 

### <a name="region-support"></a>Unterstützung für Regionen
Eine Liste der Azure-Regionen, in denen Data Factory derzeit verfügbar ist, finden Sie, indem Sie die für Sie interessanten Regionen auf der folgenden Seite auswählen und dann **Analysen** erweitern, um **Data Factory** zu finden: [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/).

Sie können eine Liste mit den Azure-Regionen anzeigen, in denen die Azure-SSIS Integration Runtime derzeit verfügbar ist, indem Sie die für Sie interessanten Regionen auf der folgenden Seite auswählen und dann **Analytics** erweitern, um auf **SSIS Integration Runtime**: [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/) zuzugreifen.### Vergleich zwischen SQL-Datenbank und verwalteter Instanz

### <a name="compare-sql-database-logical-server-and-sql-database-managed-instance"></a>Vergleich zwischen logischem Azure SQL-Datenbank-Server und verwalteter SQL-Datenbank-Instanz

In der folgenden Tabelle werden bestimmte Features von logischen SQL-Datenbank-Servern und der verwalteten SQL-Datenbank-Instanz in Bezug auf die Azure SSIR-IR verglichen:

| Feature | Logischer SQL-Datenbank-Server| Verwaltete SQL-Datenbank-Instanz |
|---------|--------------|------------------|
| **Zeitplanung** | SQL Server-Agent ist nicht verfügbar.<br/><br/>Weitere Informationen finden Sie unter [Planen eines Pakets als Teil einer Azure Data Factory-Pipeline](/sql/integration-services/lift-shift/ssis-azure-schedule-packages#activity).| SQL Server-Agent ist verfügbar. |
| **Authentifizierung** | Sie können eine Datenbank mit einem Benutzerkonto für eine eigenständige Datenbank erstellen, das einen beliebigen Azure Active Directory-Benutzer der Rolle **dbmanager** darstellt.<br/><br/>Weitere Informationen finden Sie unter [Aktivieren von Azure AD in Azure SQL-Datenbank](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | Sie können eine Datenbank mit einem Benutzerkonto für eine eigenständige Datenbank erstellen, das einen beliebigen Azure Active Directory-Benutzer darstellt, der nicht Azure AD-Administrator ist. <br/><br/>Weitere Informationen finden Sie unter [Aktivieren von Azure AD in der verwalteten Azure SQL-Datenbank-Instanz](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Dienstebene** | Wenn Sie die Azure SSIS-IR in SQL-Datenbank erstellen, können Sie die Dienstebene für SSISDB auswählen. Es gibt mehrere Dienstebenen. | Wenn Sie die Azure SSIS-IR in einer verwalteten Instanz erstellen, können Sie die Dienstebene für SSISDB nicht auswählen. Alle Datenbanken in derselben verwalteten Instanz nutzen dieselbe Ressource, die dieser Instanz zugeordnet ist, gemeinsam. |
| **Virtuelles Netzwerk** | Die Lösung unterstützt sowohl virtuelle Azure Resource Manager-Netzwerke als auch klassische virtuelle Netzwerke. | Die Lösung unterstützt nur virtuelle Azure Resource Manager-Netzwerke. Das virtuelle Netzwerk ist erforderlich.<br/><br/>Wenn Sie Ihre Azure SSIS-IR mit demselben virtuellen Netzwerk verknüpfen wie die verwaltete Instanz, stellen Sie sicher, dass sich die Azure SSIS-IR in einem anderen Subnetz als die verwaltete Instanz befindet. Wenn Sie die Azure SSIS-IR mit einem anderen virtuellen Netzwerk verknüpfen als die verwaltete Instanz, empfiehlt sich das Peering virtueller Netzwerke (das auf die gleiche Region begrenzt ist) oder eine Verbindung zwischen virtuellem Netzwerk und virtuellem Netzwerk. Weitere Informationen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Verteilte Transaktionen** | MS DTC-Transaktionen (Microsoft Distributed Transaction Coordinator) werden nicht unterstützt. Wenn Ihre Pakete MS DTC zum Koordinieren verteilter Transaktionen verwenden, können Sie möglicherweise mithilfe von elastischen Transaktionen für SQL-Datenbank eine vorübergehende Lösung implementieren. Zu diesem Zeitpunkt ist in SSIS keine Unterstützung für elastische Transaktionen integriert. Damit Sie in Ihrem SSIS-Paket elastische Transaktionen verwenden können, müssen Sie in einem Skripttask einen benutzerdefinierten ADO.NET-Code schreiben. Dieser Skripttask muss Anfang und Ende der Transaktion sowie alle Aktionen enthalten, die innerhalb der Transaktion auftreten.<br/><br/>Weitere Informationen zum Codieren elastischer Transaktionen finden Sie unter [Elastische Transaktionen mit Azure SQL-Datenbank](https://azure.microsoft.com/blog/elastic-database-transactions-with-azure-sql-database/). Weitere Informationen zu elastischen Transaktionen im Allgemeinen finden Sie unter [Verteilte Transaktionen über Clouddatenbanken hinweg](../sql-database/sql-database-elastic-transactions-overview.md). | Nicht unterstützt. |
| | | |

## <a name="azure-portal"></a>Azure-Portal
In diesem Abschnitt erstellen Sie mithilfe des Azure-Portals (insbesondere der Data Factory-Benutzeroberfläche) eine Azure-SSIS IR. 

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory 
1. Starten Sie den Webbrowser **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt. 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an. 
1. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 

   ![Neu -> Data Factory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)

1. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **MyAzureSsisDataFactory** ein. 

   ![Seite „Neue Data Factory“](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)

   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>MyAzureSsisDataFactory“), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md). 

   `Data factory name “MyAzureSsisDataFactory” is not available`

1. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
1. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus: 

   - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
   - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein. 

   Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md). 

1. Wählen Sie **V2** als **Version** aus. 
1. Wählen Sie den **Standort** für die Data Factory aus. Die Liste enthält nur Standorte, die für die Erstellung von Data Factorys geeignet sind. 
1. Wählen Sie die Option **An Dashboard anheften** aus. 
1. Klicken Sie auf **Create**. 
1. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Die Data Factory wird bereitgestellt**. 

    ![Kachel „Die Data Factory wird bereitgestellt“](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)

1. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt. 

    ![Data Factory-Startseite](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)

1. Klicken Sie auf **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Bereitstellen einer Azure SSIS Integration Runtime 
1. Klicken Sie auf der Seite „Erste Schritte“ auf die Kachel **Configure SSIS Integration Runtime** (SSIS Integration Runtime konfigurieren). 

   ![Kachel „Configure SSIS Integration Runtime“ (SSIS Integration Runtime konfigurieren)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Führen Sie auf der Seite **Allgemeine Einstellungen** von **Integration Runtime Setup** (Integration Runtime-Setup) die folgenden Schritte aus: 

   ![Allgemeine Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. Geben Sie unter **Name** den Namen Ihrer Integration Runtime ein. 

    b. Geben Sie unter **Beschreibung** die Beschreibung Ihrer Integration Runtime ein. 

    c. Wählen Sie unter **Standort** den Standort für Ihre Integration Runtime aus. Es werden nur unterstützte Standorte angezeigt. Sie sollten den gleichen Standort wie für den Datenbankserver zum Hosten von SSISDB auswählen. 

    d. Wählen Sie unter **Knotengröße** die Größe des Knotens in Ihrem Integration Runtime-Cluster aus. Nur unterstützte Knotengrößen werden angezeigt. Wählen Sie eine große Knotengröße (zentrales Hochskalieren) aus, wenn Sie zahlreiche compute-/arbeitsspeicherintensive Pakete ausführen möchten. 

    e. Wählen Sie unter **Knotenzahl** die Anzahl von Knoten in Ihrer Integration Runtime aus. Nur unterstützte Knotenzahlen werden angezeigt. Wählen Sie einen großen Cluster mit zahlreichen Knoten (horizontales Hochskalieren) aus, wenn Sie viele Pakete parallel ausführen möchten. 

    f. Wählen Sie unter **Edition/Lizenz** die SQL Server-Edition/-Lizenz für Ihre Integration Runtime aus: Standard oder Enterprise. Wählen Sie Enterprise aus, wenn Sie erweiterte Funktionen oder Premium-Funktionen für Ihre Integration Runtime verwenden möchten. 

    g. Wählen Sie unter **Sparen Sie Geld** die Option „Azure-Hybridvorteil“ (Azure Hybrid Benefit, AHB) für Ihre Integration Runtime aus: „Ja“ oder „Nein“. Wählen Sie „Ja“, wenn Sie eine eigene SQL Server-Lizenz mit Software Assurance verwenden möchten, um bei der Hybridnutzung von Kostenersparnissen zu profitieren. 

    h. Klicken Sie auf **Weiter**. 

1. Führen Sie auf der Seite **SQL-Einstellungen** die folgenden Schritte aus: 

   ![SQL-Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. Geben Sie unter **Abonnement** das Azure-Abonnement mit dem Azure-Datenbankserver zum Hosten von SSISDB an. 

    b. Wählen Sie als **Standort** den Standort des Datenbankservers zum Hosten von SSISDB aus. Sie sollten denselben Standort wie den für die Integration Runtime auswählen. 

    c. Wählen Sie unter **Katalogdatenbank-Serverendpunkt** den Endpunkt Ihres Datenbankservers zum Hosten von SSISDB aus. Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als einzelne Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz erstellt werden und in einem öffentlichen Netzwerk oder durch das Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. 

    d. Wählen Sie mit dem Kontrollkästchen **AAD-Authentifizierung verwenden...** die Authentifizierungsmethode für Ihren Datenbankserver zum Hosten von SSISDB: SQL oder Azure Active Directory (AAD) mit Ihrer über Azure Data Factory verwalteten Identität für Azure-Ressourcen aus. Wenn Sie dies aktivieren, müssen Sie Ihre Data Factory-MSI einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Informationen finden Sie unter [Aktivieren der Azure Active Directory-Authentifizierung für die Azure SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    e. Geben Sie unter **Administratorbenutzername** den Benutzernamen für die SQL-Authentifizierung für den Datenbankserver zum Hosten von SSISDB an. 

    f. Geben Sie unter **Administratorbenutzerkennwort** das Kennwort für die SQL-Authentifizierung für den Datenbankserver zum Hosten von SSISDB an. 

    g. Wählen Sie unter **Katalogdatenbank-Dienstebene** die Dienstebene für Ihren Datenbankserver zum Hosten von SSISDB aus: Basic/Standard/Premium oder Name des Pools für elastische Datenbanken. 

    h. Klicken Sie auf **Verbindung testen** und bei erfolgreichem Test auf **Weiter**. 

1.  Führen Sie auf der Seite **Erweiterte Einstellungen** die folgenden Schritte aus: 

    ![Erweiterte Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. Legen Sie unter **Maximale Anzahl von parallelen Ausführungen pro Knoten** die maximale Anzahl von Paketen fest, die in Ihrem Integration Runtime-Cluster pro Knoten gleichzeitig ausgeführt werden sollen. Nur unterstützte Paketzahlen werden angezeigt. Wählen Sie eine niedrigere Anzahl aus, wenn Sie mehrere Kerne verwenden möchten, um ein einzelnes großes/umfangreiches Paket auszuführen, das compute-/arbeitsspeicherintensiv ist. Wählen Sie eine hohe Anzahl aus, wenn Sie mindestens ein kleines Paket in einem einzelnen Kern ausführen möchten. 

    b. Geben Sie unter **Container-SAS-URI des benutzerdefinierten Setups** optional den SAS-URI (Shared Access Signature, Uniform Resource Identifier) Ihres Azure Storage Blob-Containers an, in dem Ihr Einrichtungsskript und die zugehörigen Dateien gespeichert sind. Weitere Informationen finden Sie unter [Benutzerdefiniertes Setup von Azure-SSIS-Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

1. Geben Sie mithilfe des Kontrollkästchens **Virtuelles Netzwerk auswählen...** an, ob Ihre Integration Runtime einem virtuellen Netzwerk hinzugefügt werden soll. Aktivieren Sie es, wenn Sie Azure SQL-Datenbank mit Dienstendpunkten im virtuellen Netzwerk/mit verwalteter Instanz zum Hosten von SSISDB verwenden oder Zugriff auf lokale Daten benötigen, d.h. in Ihren SSIS-Paketen über lokale Datenquellen/-ziele verfügen. Weitere Informationen finden Sie unter [Verknüpfen einer Azure-SSIS Integration Runtime mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Falls Sie es nicht aktivieren, führen Sie die folgenden Schritte aus: 

   ![Erweiterte Einstellungen mit virtuellem Netzwerk](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Ihr virtuelles Netzwerk enthält. 

    b. Für **Standort** ist derselbe Standort wie für Ihre Integration Runtime ausgewählt. 

    c. Wählen Sie für **Typ** den Typ des virtuellen Netzwerks aus: „Klassisch“ oder „Azure Resource Manager“. Sie sollten das virtuelle Azure Resource Manager-Netzwerk auswählen, da das klassische virtuelle Netzwerk bald veraltet sein wird. 

    d. Wählen Sie unter **VNET-Name** den Namen des virtuellen Netzwerks aus. Dieses virtuelle Netzwerk muss dasselbe virtuelle Netzwerk, das für Azure SQL-Datenbank mit virtuellem Netzwerk mit Dienstendpunkten/mit verwalteter Instanz zum Hosten von SSISDB verwendet wird, und/oder das mit dem lokalen Netzwerk verbundene sein. 

    e. Wählen Sie unter **Subnetzname** den Namen des Subnetzes für Ihr virtuelles Netzwerk aus. Dies muss ein anderes Subnetz als das für die verwaltete Instanz zum Hosten von SSISDB verwendete sein. 

1. Klicken Sie auf **VNET-Überprüfung**, und falls diese erfolgreich verläuft, auf **Fertig stellen**, um die Erstellung Ihrer Azure-SSIS-Integration Runtime zu starten. 

    > [!IMPORTANT]
    > - Dieser Prozess dauert etwa 20 bis 30 Minuten.
    > - Der Data Factory-Dienst stellt die Verbindung mit Ihrer Azure SQL-Datenbank her, um die SSIS-Katalogdatenbank (SSISDB) vorzubereiten. Außerdem konfiguriert das Skript Berechtigungen und Einstellungen für das virtuelle Netzwerk – sofern angegeben – und verknüpft die neue Instanz der Azure SSIS Integration Runtime mit dem virtuellen Netzwerk. 

1. Wechseln Sie im Fenster **Verbindungen** ggf. zu **Integration Runtimes**. Klicken Sie zum Aktualisieren des Status auf **Aktualisieren**. 

   ![Status der Erstellung](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Verwenden Sie die Links in der Spalte **Aktionen**, um die Integration Runtime zu überwachen, zu beenden und zu starten, zu bearbeiten oder zu löschen. Über den letzten Link können Sie den JSON-Code für die Integration Runtime anzeigen. Die Schaltflächen zum Bearbeiten und Löschen sind nur verfügbar, wenn die IR beendet wird. 

   ![Azure SSIS IR – Aktionen](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS Integration Runtimes im Portal
1. Wechseln Sie auf der Azure Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, klicken Sie auf **Verbindungen**, und wechseln Sie dann zur Registerkarte **Integration Runtimes**, um vorhandene Integration Runtimes in Ihrer Data Factory anzuzeigen. 

   ![Anzeigen vorhandener IRs](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Klicken Sie auf **Neu**, um eine neue Azure SSIS IR zu erstellen. 

   ![Integration Runtime über das Menü](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. Klicken Sie zum Erstellen einer Azure SSIS Integration Runtime auf **Neu** wie in der Abbildung gezeigt. 
1. Klicken Sie im Fenster für die Integration Runtime-Einrichtung auf **Lift-and-shift existing SSIS packages to execute in Azure** (Migrieren vorhandener SSIS-Pakete per Lift & Shift für die Ausführung in Azure), und klicken Sie auf **Weiter**. 

   ![Angeben des Integration Runtime-Typs](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Die übrigen Schritte zum Einrichten einer Azure SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

## <a name="azure-powershell"></a>Azure PowerShell
In diesem Abschnitt verwenden Sie Azure PowerShell zum Erstellen einer Azure-SSIS IR.

### <a name="create-variables"></a>Erstellen von Variablen
Definieren Sie Variablen zur Verwendung im Skript dieses Tutorials:

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS" 
# Only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# Only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8 
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon    
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"
```

### <a name="log-in-and-select-subscription"></a>Anmelden und Auswählen des Abonnements
Fügen Sie zum Anmelden bei Ihrem Azure-Abonnement und Auswählen des Abonnements den folgenden Code zum Skript hinzu: 

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Überprüfen der Verbindung mit der Datenbank
Fügen Sie das folgende Skript hinzu, um den Azure SQL-Datenbank-Serverendpunkt zu überprüfen. 

```powershell
# Validate only when you do not use VNet nor AAD authentication
if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
{
    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
            Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
            $yn = Read-Host
            if(!($yn -ieq "Y"))
            {
                Return;
            } 
        }
    }
}
```

### <a name="configure-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks
Fügen Sie das folgende Skript hinzu, um Berechtigungen/Einstellungen für das virtuelle Netzwerk für die Azure SSIS Integration Runtime automatisch zu konfigurieren, die Sie einbinden möchten.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory
Führen Sie den folgenden Befehl aus, um eine Data Factory zu erstellen:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Erstellen einer Integration Runtime
Führen Sie die folgenden Befehle aus, um eine Azure-SSIS-Integration Runtime zu erstellen, die SSIS-Pakete in Azure ausführt. 

Wenn Sie weder Azure SQL-Datenbank mit Dienstendpunkten im virtuellen Netzwerk/mit verwalteter Instanz zum Hosten von SSISDB verwenden noch Zugriff auf lokale Daten benötigen, können Sie die Parameter „VNetId“ und „Subnetz“ weglassen oder dafür leere Werte übergeben. Andernfalls können Sie sie nicht weglassen und müssen gültige Werte aus der Konfiguration des virtuellen Netzwerks übergeben, wie unter [Verknüpfen einer Azure-SSIS Integration Runtime mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) beschrieben. 

Wenn Sie eine verwaltete Instanz zum Hosten von SSISDB verwenden, können Sie den Parameter CatalogPricingTier weglassen oder dafür einen leeren Wert übergeben. Andernfalls können Sie ihn nicht weggelassen und müssen einen gültigen Wert aus der Liste der für Azure SQL-Datenbank unterstützten Tarife übergeben, wie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](../sql-database/sql-database-resource-limits.md) beschrieben. 

Wenn Sie AAD-Authentifizierung (Azure Active Directory) mit Ihrer verwalteten Azure Data Factory-Identität verwenden, um die Verbindung mit dem Datenbankserver herzustellen, können Sie den Parameter CatalogAdminCredential auslassen, jedoch müssen Sie Ihre Data Factory-MSI in einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen, wie unter [Aktivieren der Azure Active Directory-Authentifizierung für die Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir) beschrieben. Andernfalls können Sie ihn nicht auslassen, sondern müssen ein gültiges Objekt, gebildet aus Ihrem Administratorbenutzername und dem zugehörigen Kennwort für den Server, für die SQL-Authentifizierung übergeben.

```powershell               
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Type Managed `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}
```

### <a name="start-integration-runtime"></a>Starten der Integration Runtime
Führen Sie den folgenden Befehl aus, um die Azure SSIS Integration Runtime zu starten: 

```powershell
write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```

Die Ausführung dieses Befehls dauert ca. **20 bis 30 Minuten**. 

### <a name="full-script"></a>Vollständiges Skript
Hier ist das vollständige Skript, das eine Azure-SSIS-Integration Runtime erstellt. 

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

### Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS" 
# Only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D4_v2"
# Only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8 
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon    
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"

### Log in and select subscription
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database
# Validate only when you do not use VNet nor AAD authentication
if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
{
    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
    {
        $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
        $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
        Try
        {
            $sqlConnection.Open();
        }
        Catch [System.Data.SqlClient.SqlException]
        {
            Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
            Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
            $yn = Read-Host
            if(!($yn -ieq "Y"))
            {
                Return;
            } 
        }
    }
}

### Configure virtual network
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Type Managed `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogAdminCredential $serverCreds
}

### Start integration runtime   
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage
In diesem Abschnitt verwenden Sie eine Azure Resource Manager-Vorlage, um eine Azure-SSIS-Integration Runtime zu erstellen. Die folgende exemplarische Vorgehensweise kann als Beispiel dienen: 

1. Erstellen Sie eine JSON-Datei mit der folgenden Azure Resource Manager-Vorlage. Ersetzen Sie die Werte in den eckigen Klammern (Platzhalter) durch Ihre eigenen Werte. 

    ```json
    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2017-09-01-preview",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2017-09-01-preview",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D4_v2",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```
    
1. Führen Sie zum Bereitstellen der Azure Resource Manager-Vorlage den Befehl New-AzureRmResourceGroupDeployment wie im folgenden Beispiel gezeigt aus, wobei ADFTutorialResourceGroup der Name der Ressourcengruppe und „ADFTutorialARM.json“ die Datei ist, die die JSON-Definition für Ihre Data Factory und Azure-SSIS-IR enthält. 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Dieser Befehl erstellt die Data Factory und darin eine Azure-SSIS-IR. Er startet die IR aber nicht. 

1. Führen Sie zum Starten der Azure-SSIS-IR den Befehl Start-AzureRmDataFactoryV2IntegrationRuntime aus: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ``` 

## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen
Verwenden Sie jetzt SQL Server Data Tools (SSDT) oder SQL Server Management Studio (SSMS), um Ihre SSIS-Pakete in Azure bereitzustellen. Stellen Sie eine Verbindung mit Ihrem Datenbankserver her, der den SSIS-Katalog (SSISDB) hostet. Der Name des Datenbankservers hat folgendes Format: „&lt;Name der Azure SQL-Datenbank-Serverinstanz&gt;.database.windows.net“ oder „&lt;Name der verwalteten Instanz.DNS-Präfix&gt;.database.windows.net“. Anweisungen finden Sie im Artikel [Bereitstellen von Paketen](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). 

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich auch die anderen Themen zur Azure-SSIS-IR in dieser Dokumentation an: 

- [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime): Dieser Artikel enthält konzeptionelle Informationen zu Integrationslaufzeiten, die im Allgemeinen die Azure-SSIS-Integrationslaufzeit einschließen. 
- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-create-azure-ssis-runtime-portal.md): Dieser Artikel enthält schrittweise Anweisungen zum Erstellen einer Azure-SSIS-Integrationslaufzeit und verwendet eine Azure SQL-Datenbank zum Hosten des SSIS-Katalogs. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel wird das Abrufen von Informationen zu einer Azure-SSIS-Integrationslaufzeit veranschaulicht, und er enthält Beschreibungen der Status in den zurückgegebenen Informationen. 
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md): In diesem Artikel wird beschrieben, wie Sie eine Azure-SSIS-Integrationslaufzeit beenden, starten oder entfernen. Es wird zudem gezeigt, wie Sie Ihre Azure-SSIS-Integrationslaufzeit horizontal hochskalieren, indem Sie der Integrationslaufzeit weitere Knoten hinzufügen. 
- [Verknüpfen einer Azure-SSIS-Integration Runtime mit einem virtuellen Netzwerk:](join-azure-ssis-integration-runtime-virtual-network.md). Dieser Artikel enthält grundlegende Informationen zum Verknüpfen Ihrer Azure-SSIS-IR mit einem virtuellen Azure-Netzwerk. Darüber hinaus enthält er Schritte zur Verwendung des Azure-Portals zum Konfigurieren des virtuellen Netzwerks, damit die Azure-SSIS-IR dem virtuellen Netzwerk beitreten kann. 

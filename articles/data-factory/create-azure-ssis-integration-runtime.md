---
title: Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft Docs
description: "Erfahren Sie, wie eine Azure-SSIS Integration Runtime erstellt wird, damit Sie das SSIS-Paket in der Azure-Cloud ausführen können."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
ms.openlocfilehash: 35883890b330588415290295815ad55cf5021afb
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft-Dokumentation
In diesem Artikel werden die Schritte für die Bereitstellung einer Azure-SSIS-Integrationslaufzeit (Integration Runtime, IR) in Azure Data Factory beschrieben. Anschließend können Sie SQL Server Data Tools (SSDT) oder SQL Server Management Studio (SSMS) zum Bereitstellen von SQL Server Integration Services-Paketen (SSIS) für diese Runtime in Azure verwenden.

Im Tutorial [Bereitstellen von SQL Server Integration Services-Paketen in Azure](tutorial-deploy-ssis-packages-azure.md) wurde erläutert, wie Sie eine Azure-SSIS Integration Runtime unter Verwendung von Azure SQL-Datenbank als Speicher für den SSIS-Katalog erstellen können. In diesem Artikel wird das Tutorial vertieft und erläutert, wie Sie die folgenden Aktionen ausführen: 

- Verwenden der verwalteten Azure SQL-Datenbank-Instanz (private Vorschauversion) zum Hosten eines SSIS-Katalogs (SSISDB-Datenbank)
- Einbinden einer Azure-SSIS-IR in ein virtuelles Azure-Netzwerk (VNET) Konzeptionelle Informationen zum Einbinden einer Azure-SSIS-IR in ein VNET und zum Konfigurieren eines VNET im Azure-Portal finden Sie unter [Beitreten einer Azure-SSIS-Integrationslaufzeit zu einem virtuellen Netzwerk](join-azure-ssis-integration-runtime-virtual-network.md). 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-introduction.md) weiter.


## <a name="overview"></a>Übersicht
Dieser Artikel zeigt verschiedene Methoden für die Bereitstellung einer Azure-SSIS IR auf:

- [Azure-Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure Resource Manager-Vorlage](#azure-resource-manager-template)

Data Factory stellt die Verbindung mit Ihrer Azure SQL-Datenbank her, um die SSIS-Katalogdatenbank (SSISDB) vorzubereiten, wenn Sie eine Azure-SSIS IR erstellen. Außerdem konfiguriert das Skript Berechtigungen und Einstellungen für das VNET – sofern angegeben – und verknüpft die neue Instanz der Azure SSIS Integration Runtime mit dem VNET.

Wenn Sie eine Instanz der SQL-Datenbank zum Hosten der SSISDB bereitstellen, werden auch das Azure Feature Pack für SSIS und die Access Redistributable-Komponente installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die von den integrierten Komponenten unterstützt werden. Zurzeit können Sie für SSIS keine Komponenten von Drittanbietern installieren (einschließlich Drittanbieterkomponenten von Microsoft, z.B. die Oracle- und Teradata-Komponenten von Attunity und die SAP BI-Komponenten).

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**. Falls Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Testkonto](http://azure.microsoft.com/pricing/free-trial/) erstellen.
- **Azure SQL-Datenbankserver** oder **verwaltete SQL Server-Instanz (private Vorschau) (erweiterte private Vorschau)**. Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. Dieser Server hostet die SSIS-Katalog-Datenbank (SSISDB). Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in SSISDB zu schreiben, ohne Azure-Regionen zu überschreiten. Notieren Sie den Tarif Ihrer Azure SQL Server-Instanz. Eine Liste mit den unterstützten Tarifen für Azure SQL-Datenbank finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](../sql-database/sql-database-resource-limits.md).

    Vergewissern Sie sich, dass der Azure SQL-Datenbankserver oder die verwaltete SQL Server-Instanz (erweiterte private Preview) nicht über einen SSIS-Katalog (SSIDB-Datenbank) verfügt. Für die Bereitstellung der Azure SSIS IR wird die Verwendung eines vorhandenen SSIS-Katalogs nicht unterstützt.
- **Klassisches oder virtuelles Azure Resource Manager-Netzwerk (VNet) (optional)**. Sie benötigen ein virtuelles Azure-Netzwerk (VNET), wenn mindestens eine der folgenden Bedingungen zutrifft:
    - Sie hosten die SSIS-Katalogdatenbank auf einer verwalteten SQL Server-Instanz (private Vorschau), die zu einem virtuellen Netzwerk gehört.
    - Sie möchten aus SSIS-Paketen, die in einer Azure-SSIS-Integrationslaufzeit ausgeführt werden, eine Verbindung mit lokalen Datenspeichern herstellen.
- **Azure PowerShell**. Befolgen Sie die Anweisungen unter [Get started with Azure PowerShell cmdlets](/powershell/azure/install-azurerm-ps) (Erste Schritte mit Azure PowerShell-Cmdlets). Sie verwenden PowerShell zum Ausführen eines Skripts, um eine Azure SSIS Integration Runtime bereitzustellen, die SSIS-Pakete in der Cloud ausführt. 

> [!NOTE]
> Eine Liste mit Regionen, die von Azure Data Factory V2 und Azure-SSIS Integration Runtime unterstützt werden, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/). Erweitern Sie **Daten + Analysen**, um **Data Factory V2** und **SSIS Integration Runtime** anzuzeigen.

## <a name="azure-portal"></a>Azure-Portal
In diesem Abschnitt erstellen Sie mithilfe des Azure-Portals (insbesondere der Data Factory-Benutzeroberfläche) eine Azure-SSIS IR. 

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.    
2. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **MyAzureSsisDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>MyAzureSsisDataFactory“), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
4. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
      - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
      - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
      Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
4. Wählen Sie **V2 (Vorschau)** als **Version** aus.
5. Wählen Sie den **Standort** für die Data Factory aus. Die Liste enthält nur Standorte, die für die Erstellung von Data Factorys geeignet sind.
6. Wählen Sie die Option **An Dashboard anheften** aus.     
7. Klicken Sie auf **Create**.
8. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Die Data Factory wird bereitgestellt**. 

    ![Kachel „Die Data Factory wird bereitgestellt“](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
   ![Data Factory-Startseite](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Klicken Sie auf **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Bereitstellen einer Azure SSIS Integration Runtime

1. Klicken Sie auf der Seite „Erste Schritte“ auf die Kachel **Configure SSIS Integration Runtime** (SSIS Integration Runtime konfigurieren). 

   ![Kachel „Configure SSIS Integration Runtime“ (SSIS Integration Runtime konfigurieren)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Führen Sie auf der Seite **Allgemeine Einstellungen** von **Integration Runtime Setup** (Integration Runtime-Setup) die folgenden Schritte aus: 

   ![Allgemeine Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Geben Sie unter **Name** einen Namen für die Integration Runtime ein.
    2. Geben Sie den **Standort** für die Integration Runtime an. Es werden nur unterstützte Standorte angezeigt.
    3. Wählen Sie die **Größe des Knotens** aus, der mit der SSIS Runtime konfiguriert werden soll.
    4. Geben Sie die **Anzahl von Knoten** im Cluster an.
    5. Klicken Sie auf **Weiter**.
 
1. Gehen Sie unter **SQL-Einstellungen** wie folgt vor: 

    ![SQL-Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Geben Sie das Azure-**Abonnement** an, das die Azure SQL Server-Instanz enthält. 
    2. Wählen Sie für **Catalog Database Server Endpoint** (Katalogdatenbank-Serverendpunkt) Ihre Azure SQL Server-Instanz aus
    3. Geben Sie den **Administratorbenutzernamen** ein.
    4. Geben Sie das **Administratorkennwort** ein.  
    5. Wählen Sie die **Dienstebene** für die SSISDB-Datenbank aus. Der Standardwert lautet „Basic“.
    6. Klicken Sie auf **Weiter**.
 
1.  Wählen Sie auf der Seite **Erweiterte Einstellungen** einen Wert für **Maximum Parallel Executions Per Node** (Maximale Anzahl von parallelen Ausführungen pro Knoten) aus.   

    ![Erweiterte Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Dieser Schritt ist **optional**. Aktivieren Sie bei einem klassischen virtuellen Netzwerk (VNET), dem die Integration Runtime hinzugefügt werden soll, die Option **Select a VNet for your Azure-SSIS integration runtime to join and allow Azure services to configure VNet permissions/settings** (VNET für den Beitritt der Azure SSIS Integration Runtime auswählen und Azure-Diensten die Konfiguration von VNET-Berechtigungen/-Einstellungen erlauben). Führen Sie anschließend die folgenden Schritte aus: 

    ![Erweiterte Einstellungen bei einem VNET](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Geben Sie das **Abonnement** an, das das klassische VNET enthält. 
    2. Wählen Sie das **VNET** aus. <br/>
    4. Wählen Sie das **Subnetz** aus.<br/> 
1. Klicken Sie auf **Fertig stellen**, um die Erstellung der Azure SSIS Integration Runtime zu starten. 

    > [!IMPORTANT]
    > - Dieser Prozess dauert etwa 20 Minuten.
    > - Der Data Factory-Dienst stellt die Verbindung mit Ihrer Azure SQL-Datenbank her, um die SSIS-Katalogdatenbank (SSISDB) vorzubereiten. Außerdem konfiguriert das Skript Berechtigungen und Einstellungen für das VNET – sofern angegeben – und verknüpft die neue Instanz der Azure SSIS Integration Runtime mit dem VNET.
7. Wechseln Sie im Fenster **Verbindungen** ggf. zu **Integration Runtimes**. Klicken Sie zum Aktualisieren des Status auf **Aktualisieren**. 

    ![Status der Erstellung](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Über die Links in der Spalte **Aktionen** können Sie die Integration Runtime überwachen, beenden/starten, bearbeiten oder löschen. Über den letzten Link können Sie den JSON-Code für die Integration Runtime anzeigen. Die Schaltflächen zum Bearbeiten und Löschen sind nur verfügbar, wenn die IR beendet wird. 

    ![Azure SSIS IR – Aktionen](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Klicken Sie unter **Aktionen** auf den Link **Überwachen**.  

    ![Azure SSIS IR – Aktionen](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Wenn im Zusammenhang mit der Azure SSIS IR ein **Fehler** auftritt, werden auf dieser Seite die Anzahl von Fehlern und der Link zum Anzeigen von Fehlerdetails angezeigt. Beispiel: Wenn der SSIS-Katalog bereits auf dem Datenbankserver vorhanden ist, wird ein Fehler angezeigt, der angibt, dass die SSISDB-Datenbank vorhanden ist.  
11. Klicken Sie oben auf **Integration Runtimes**, um zurück zur vorherigen Seite zu wechseln und alle mit der Data Factory verknüpften Integration Runtimes anzuzeigen.  

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS Integration Runtimes im Portal

1. Wechseln Sie auf der Azure Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, klicken Sie auf **Verbindungen**, und wechseln Sie dann zur Registerkarte **Integration Runtimes**, um vorhandene Integration Runtimes in Ihrer Data Factory anzuzeigen. 
    ![Anzeigen vorhandener IRs](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Klicken Sie auf **Neu**, um eine neue Azure SSIS IR zu erstellen. 

    ![Integration Runtime über das Menü](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Klicken Sie zum Erstellen einer Azure SSIS Integration Runtime auf **Neu** wie in der Abbildung gezeigt. 
3. Klicken Sie im Fenster für die Integration Runtime-Einrichtung auf **Lift-and-shift existing SSIS packages to execute in Azure** (Migrieren vorhandener SSIS-Pakete per Lift & Shift für die Ausführung in Azure), und klicken Sie auf **Weiter**.

    ![Angeben des Integration Runtime-Typs](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Die übrigen Schritte zum Einrichten einer Azure SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime).

## <a name="azure-powershell"></a>Azure PowerShell
In diesem Abschnitt verwenden Sie Azure PowerShell zum Erstellen einer Azure-SSIS IR.

### <a name="create-variables"></a>Erstellen von Variablen
Definieren Sie Variablen zur Verwendung im Skript dieses Tutorials:

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D3_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

# Remove these the following two OPTIONAL variables if you are using Azure SQL Database. 
# These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# OPTIONAL: specify your VNet ID and the subnet name. 
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

```
### <a name="log-in-and-select-subscription"></a>Anmelden und Auswählen des Abonnements
Fügen Sie zum Anmelden bei Ihrem Azure-Abonnement und Auswählen des Abonnements den folgenden Code zum Skript hinzu: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Überprüfen der Verbindung mit der Datenbank
Fügen Sie das folgende Skript hinzu, um das Netzwerk „server.database.windows.net“ Ihres Azure SQL-Datenbankservers oder den Serverendpunkt Ihrer verwalteten Azure SQL-Datenbank-Instanz (private Vorschauversion) zu überprüfen. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

### <a name="configure-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks
Fügen Sie das folgende Skript hinzu, um Berechtigungen/Einstellungen für das virtuelle Netzwerk für die Azure SSIS Integration Runtime automatisch zu konfigurieren, die Sie einbinden möchten.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope` erstellt.

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
Führen Sie den folgenden Befehl aus, um eine Azure SSIS Intergration Runtime zu erstellen, die SSIS-Pakete in Azure ausführt. Verwenden Sie das Skript aus dem Abschnitt, das auf dem Datenbanktyp basiert (Azure SQL-Datenbank im Vergleich zu verwalteter Azure SQL-Instanz [Private Preview]), den Sie verwenden. 

#### <a name="azure-sql-database-to-host-the-ssisdb-database-ssis-catalog"></a>Azure SQL-Datenbank zum Hosten der SSISDB-Datenbank (SSIS-Katalog) 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode
```

Für „VNetId“ und „Subnet“ müssen nur Werte übergeben werden, wenn Sie lokalen Datenzugriff benötigen (sprich: wenn Ihre SSIS-Pakete lokale Datenquellen/-ziele enthalten). Sie müssen den Wert für den CatalogPricingTier-Parameter übergeben. Eine Liste mit den unterstützten Tarifen für Azure SQL-Datenbank finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](../sql-database/sql-database-resource-limits.md).

#### <a name="azure-sql-managed-instance-private-preview-to-host-the-ssisdb-database"></a>Verwaltete Azure SQL-Instanz (Private Preview) zum Hosten der SSISDB-Datenbank

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

Bei einer verwalteten Azure SQL-Datenbank-Instanz (private Vorschauversion), die in ein VNET eingebunden wird, müssen Werte für die Parameter „VnetId“ und „Subnet“ übergeben werden. Der CatalogPricingTier-Parameter gilt nicht für die verwaltete Azure SQL-Datenbank-Instanz. 

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
Hier ist das vollständige Skript, das eine Azure SSIS IR erstellt und sie in ein VNet einbindet. Dieses Skript geht davon aus, dass Sie die verwaltete Azure SQL-Instanz verwenden, um den SSIS-Katalog zu hosten. 

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_D3_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## Remove these two OPTIONAL variables if you are using Azure SQL Database. 
## These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# Specify information about your classic or Azure Resource Manager virtual network (VNet).
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName

write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage
In diesem Abschnitt verwenden Sie eine Azure Resource Manager-Vorlage, um eine Azure-SSIS Integration Runtime zu erstellen. Die folgende exemplarische Vorgehensweise kann als Beispiel dienen: 

1. Erstellen Sie eine JSON-Datei mit der folgenden Resource Manager-Vorlage. Ersetzen Sie die Werte in den eckigen Klammern (Platzhalter) durch Ihre eigenen Werte. 

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
                "name": "<Specify a name for the Azure SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2017-09-01-preview",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D1_v2",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 1
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL server>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL user",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Password>"
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
2. Führen Sie zum Bereitstellen der Resource Manager-Vorlage den Befehl „New-AzureRmResourceGroupDeployment“ wie im folgenden Beispiel gezeigt aus. In diesem Beispiel ist „ADFTutorialResourceGroup“ der Name der Ressourcengruppe. „ADFTutorialARM.json“ ist die Datei, die die JSON-Definition für die Data Factory und die Azure-SSIS IR enthält. 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Dieser Befehl erstellt die Data Factory und darin eine Azure-SSIS IR. Er startet die IR aber nicht. 
3. Führen Sie zum Starten der Azure-SSIS IR den Befehl „Start-AzureRmDataFactoryV2IntegrationRuntime“ aus: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name> `
                                             -DataFactoryName <Data Factory Name> `
                                             -Name <Azure SSIS IR Name> `
                                             -Force
    ``` 

## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen
Verwenden Sie jetzt SQL Server Data Tools (SSDT) oder SQL Server Management Studio (SSMS), um Ihre SSIS-Pakete in Azure bereitzustellen. Stellen Sie eine Verbindung mit der Azure SQL Server-Instanz her, die den SSIS-Katalog (SSISDB) hostet. Der Name der Azure SQL Server-Instanz hat das folgende Format: &lt;servername&gt;. database.windows.net (für Azure SQL-Datenbank). Anweisungen finden Sie im Artikel [Bereitstellen von Paketen](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). 

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich auch die anderen Themen zur Azure-SSIS-IR in dieser Dokumentation an:

- [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime): Dieser Artikel enthält konzeptionelle Informationen zu Integrationslaufzeiten, die im Allgemeinen die Azure-SSIS-Integrationslaufzeit einschließen. 
- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-deploy-ssis-packages-azure.md): Dieser Artikel enthält schrittweise Anweisungen zum Erstellen einer Azure-SSIS-Integrationslaufzeit und verwendet eine Azure SQL-Datenbank zum Hosten des SSIS-Katalogs. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel wird das Abrufen von Informationen zu einer Azure-SSIS-Integrationslaufzeit veranschaulicht, und er enthält Beschreibungen der Status in den zurückgegebenen Informationen. 
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md): In diesem Artikel wird beschrieben, wie Sie eine Azure-SSIS-Integrationslaufzeit beenden, starten oder entfernen. Es wird zudem gezeigt, wie Sie Ihre Azure-SSIS-Integrationslaufzeit horizontal hochskalieren, indem Sie der Integrationslaufzeit weitere Knoten hinzufügen. 
- [Verknüpfen einer Azure-SSIS-Integrationslaufzeit mit einem VNet](join-azure-ssis-integration-runtime-virtual-network.md): Dieser Artikel enthält grundlegende Informationen zum Verknüpfen einer Azure-SSIS-Integrationslaufzeit mit einem Azure Virtual Network (VNet). Darüber hinaus enthält er Schritte zur Verwendung des Azure-Portals zum Konfigurieren des VNet, damit die Azure-SSIS-Integrationslaufzeit dem VNet beitreten kann. 
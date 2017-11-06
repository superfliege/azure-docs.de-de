---
title: Bereitstellen von SSIS-Paketen in Azure | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie Sie SSIS-Pakete für die Azure SSIS Integration Runtime bereitstellen, die von Azure Data Factory bereitgestellt wird."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 10/06/2017
ms.author: spelluru
ms.openlocfilehash: a225b9285294f32fd7183390a73f55ae64bf232a
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="deploy-sql-server-integration-services-packages-to-azure"></a>Bereitstellen von SQL Server Integration Services-Paketen in Azure

[!INCLUDE [data-factory-what-is-include-md](../../includes/data-factory-what-is-include.md)]

#### <a name="this-tutorial"></a>Dieses Tutorial

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) weiter.

In diesem Tutorial werden die Schritte für die Bereitstellung einer Azure-SSIS-Integrationslaufzeit (Integration Runtime, IR) in Azure Data Factory beschrieben. Anschließend können Sie SQL Server Data Tools (SSDT) oder SQL Server Management Studio (SSMS) zum Bereitstellen von SQL Server Integration Services-Paketen (SSIS) für diese Runtime in Azure verwenden. In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Azure SSIS Integration Runtime
> * Starten der Azure SSIS Integration Runtime
> * Bereitstellen von SSIS-Paketen
> * Überprüfen des kompletten Skripts

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Konzeptionelle Informationen zur Azure-SSIS-IR finden Sie unter [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime).

## <a name="prerequisites"></a>Voraussetzungen
- **Azure SQL-Datenbankserver**. Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. Dieser Server hostet die SSIS-Katalog-Datenbank (SSISDB). Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in SSISDB zu schreiben, ohne Azure-Regionen zu überschreiten. 
    - Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für den Datenbankserver **aktiviert** ist. Weitere Informationen finden Sie unter [Schützen der Azure SQL-Datenbank](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Informationen zum Aktivieren dieser Einstellung mithilfe von PowerShell finden Sie unter [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
    - Fügen Sie der Client-IP-Adressenliste in den Firewalleinstellungen für den Datenbankserver die IP-Adresse des Clientcomputers oder einen IP-Adressbereich hinzu, der die IP-Adresse des Clientcomputers enthält. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md). 
- **Azure PowerShell**. Befolgen Sie die Anweisungen unter [Get started with Azure PowerShell cmdlets](/powershell/azure/install-azurerm-ps) (Erste Schritte mit Azure PowerShell-Cmdlets). Sie verwenden PowerShell zum Ausführen eines Skripts, um eine Azure SSIS Integration Runtime bereitzustellen, die SSIS-Pakete in der Cloud ausführt. 

## <a name="launch-windows-powershell-ise"></a>Starten von Windows PowerShell ISE
Starten Sie **Windows PowerShell ISE** mit Administratorrechten. 

## <a name="create-variables"></a>Erstellen von Variablen
Kopieren Sie das folgende Skript, und fügen Sie es ein. Geben Sie Werte für die Variablen an. Eine Liste mit den unterstützten **Tarifen** für Azure SQL-Datenbank finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](../sql-database/sql-database-resource-limits.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
# In public preview, only EastUS amd EastUS2 are supported.
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS and NorthEurope are supported.
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_A4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>" 
```

## <a name="validate-the-connection-to-database"></a>Überprüfen der Verbindung mit der Datenbank
Fügen Sie das folgende Skript hinzu, um das Netzwerk „server.database.windows.net“ des Azure SQL-Datenbankservers zu überprüfen. 

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

Das folgende Beispiel veranschaulicht die Erstellung einer Azure SQL-Datenbank-Instanz im Rahmen des Skripts: 

Legen Sie Werte für die noch nicht definierten Variablen fest. Beispiel: FirewallIPAddress. 

```powershell
New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName `
  -ServerName $SQLServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SQLServerAdmin, $(ConvertTo-SecureString -String $SQLServerPass -AsPlainText -Force))

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SQLServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SQLServerName -AllowAllAzureIPs
```


## <a name="log-in-and-select-subscription"></a>Anmelden und Auswählen des Abonnements
Fügen Sie dem Skript zum Anmelden bei Ihrem Azure-Abonnement und Auswählen des Abonnements den folgenden Code hinzu: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope` erstellt.

Wenn die Ressourcengruppe bereits vorhanden ist, müssen Sie diesen Code nicht in das Skript kopieren. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
Führen Sie den folgenden Befehl aus, um eine Data Factory zu erstellen:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                        -Location $DataFactoryLocation `
                        -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Erstellen einer Integration Runtime
Führen Sie den folgenden Befehl aus, um eine Azure SSIS Integration Runtime zu erstellen, die SSIS-Pakete in Azure ausführt: 

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

## <a name="start-integration-runtime"></a>Starten der Integration Runtime
Führen Sie den folgenden Befehl aus, um die Azure SSIS Integration Runtime zu starten: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Die Ausführung dieses Befehls dauert ca. **20 bis 30 Minuten**. 

## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen
Verwenden Sie jetzt SQL Server Data Tools (SSDT) oder SQL Server Management Studio (SSMS), um Ihre SSIS-Pakete in Azure bereitzustellen. Stellen Sie eine Verbindung mit der Azure SQL Server-Instanz her, die den SSIS-Katalog (SSISDB) hostet. Der Name der Azure SQL Server-Instanz hat das folgende Format: `<servername>.database.windows.net` (für Azure SQL-Datenbank). 

Informationen finden Sie in den folgenden Artikeln der SSIS-Dokumentation: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (Bereitstellen, Ausführen und Überwachen eines SSIS-Pakets in Azure)   
- [Connect to the SSISDB Catalog database on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Herstellen einer Verbindung mit der SSISDB-Katalogdatenbank in Azure)
- [Schedule the execution of an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Planen der Ausführung eines SSIS-Pakets in Azure)
- [Connect to on-premises data sources with Windows Authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Herstellen einer Verbindung mit lokalen Datenquellen mit Windows-Authentifizierung) 

## <a name="full-script"></a>Vollständiges Skript
In diesem Release müssen Sie mit PowerShell eine Instanz der Azure SSIS Integration Runtime bereitstellen, die SSIS-Pakete in der Cloud ausführt. Derzeit ist es nicht möglich, diese Runtime im Azure-Portal bereitzustellen. 

Das PowerShell-Skript in diesem Abschnitt konfiguriert eine Instanz der Azure SSIS Integration Runtime in der Cloud, die SSIS-Pakete ausführt. Nachdem Sie dieses Skript erfolgreich ausgeführt haben, können Sie SSIS-Pakete in der Microsoft Azure-Cloud bereitstellen und ausführen, wobei SSISDB in Azure SQL-Datenbank gehostet wird.

1. Starten Sie die Windows PowerShell Integrated Scripting Environment (ISE).
2. Führen Sie in der ISE den folgenden Befehl an der Eingabeaufforderung aus.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Kopieren Sie das PowerShell-Skript in diesem Abschnitt, und fügen Sie es in die ISE ein.
4. Geben Sie im Abschnitt für die Spezifikationen von SSIS in Azure am Anfang des Skripts geeignete Werte für die Skriptparameter ein. Diese Parameter werden im nächsten Abschnitt beschrieben.
5. Führen Sie das Skript aus. Die Ausführung des Befehls `Start-AzureRmDataFactoryV2IntegrationRuntime` am Ende des Skripts dauert **20 bis 30 Minuten**.

> [!NOTE]
> Das Skript stellt die Verbindung mit Ihrer Azure SQL-Datenbank her, um die SSIS-Katalogdatenbank (SSISDB) vorzubereiten. Außerdem konfiguriert das Skript Berechtigungen und Einstellungen für das VNET – sofern angegeben – und verknüpft die neue Instanz der Azure SSIS Integration Runtime mit dem VNET.

Eine Liste mit den unterstützten **Tarifen** für Azure SQL-Datenbank finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](../sql-database/sql-database-resource-limits.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
# In public preview, only EastUS amd EastUS2 are supported.
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS (IR)>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS and NorthEurope are supported.
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_A4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>" 

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

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

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
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-vnet"></a>Verknüpfen einer Azure-SSIS-IR mit einem VNET
Wenn Sie eine verwaltete Azure SQL-Datenbank-Instanz (private Vorschauversion) verwenden, um den SQL Server Integration Services-Katalog (SSIS) in einem virtuellen Netzwerk (VNET) zu hosten, müssen Sie diesem virtuellen Netzwerk auch die Azure-SSIS-IR hinzufügen. Mit Azure Data Factory Version 2 (Vorschau) können Sie eine Azure-SSIS-Integrationslaufzeit mit einem klassischen VNet verknüpfen. Weitere Informationen finden Sie unter [Beitreten einer Azure-SSIS-Integrationslaufzeit zu einem virtuellen Netzwerk](join-azure-ssis-integration-runtime-virtual-network.md).

Ein vollständiges Skript zum Erstellen einer Azure-SSIS-Laufzeit, die mit einem VNET verknüpft wird, finden Sie unter [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft-Dokumentation](create-azure-ssis-integration-runtime.md).

## <a name="monitor-and-manage-azure-ssis-ir"></a>Überwachen und Verwalten einer Azure-SSIS-IR
In den folgenden Artikeln finden Sie ausführliche Informationen zum Überwachen und Verwalten einer Azure-SSIS-IR. 

- [Überwachen einer Integrationslaufzeit in Azure Data Factory](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Azure SSIS Integration Runtime
> * Starten der Azure SSIS Integration Runtime
> * Bereitstellen von SSIS-Paketen
> * Überprüfen des kompletten Skripts

Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie Daten von einem lokalen Speicherort in die Cloud kopieren: 

> [!div class="nextstepaction"]
>[Kopieren von Daten in die Cloud](tutorial-copy-data-dot-net.md)

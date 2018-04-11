---
title: Verknüpfen einer Azure SSIS-Integration Runtime mit einem virtuellen Netzwerk | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Azure-SSIS-Integrationslaufzeit mit einem Azure Virtual Network verknüpfen.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: douglasl
ms.openlocfilehash: 2372b6bd91dfb1c33456b42e91aa2496532796ef
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Beitreten einer Azure-SSIS-Integrationslaufzeit zu einem virtuellen Netzwerk
Verknüpfen Sie in folgenden Szenarien Ihre Azure SSIS-Integration Runtime (IR) mit einem virtuellen Azure-Netzwerk: 

- Sie hosten die SQL Server Integration Services-Katalogdatenbank (SSIS) auf einer verwalteten Azure SQL-Datenbank-Instanz (Vorschau) in einem virtuellen Netzwerk.
- Sie möchten aus SSIS-Paketen, die in einer Azure-SSIS-Integrationslaufzeit ausgeführt werden, eine Verbindung mit lokalen Datenspeichern herstellen.

 Mit Azure Data Factory Version 2 (Vorschauversion) können Sie Ihre Azure SSIS-Integration Runtime über das klassische Bereitstellungsmodell oder das Azure Resource Manager-Bereitstellungsmodell mit einem virtuellen Netzwerk verknüpfen. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, hilft Ihnen die [Dokumentation zu Data Factory Version 1](v1/data-factory-introduction.md) weiter.

## <a name="access-to-on-premises-data-stores"></a>Zugriff auf lokale Datenspeicher
Wenn SSIS-Pakete nur auf Datenspeicher in öffentlichen Clouds zugreifen, müssen Sie die Azure SSIS-IR nicht mit einem virtuellen Netzwerk verknüpfen. Wenn SSIS-Pakete auf lokale Datenspeicher zugreifen, müssen Sie die Azure SSIS-IR mit einem virtuellen Netzwerk verknüpfen, das mit dem lokalen Netzwerk verbunden ist. 

Wenn der SSIS-Katalog in einer Azure SQL-Datenbank-Instanz gehostet wird, die nicht im virtuellen Netzwerk enthalten ist, müssen Sie entsprechende Ports öffnen. 

Wenn der SSIS-Katalog in verwalteten SQL-Datenbank-Instanzen (Vorschau) in einem virtuellen Netzwerk gehostet wird, können Sie eine Azure SSIS-IR mit folgenden Netzwerken verknüpfen:

- Demselben virtuellen Netzwerk
- Einem anderen virtuellen Netzwerk, das über eine Netzwerk-zu-Netzwerk-Verbindung mit dem Netzwerk verfügt, in dem sich die verwaltete SQL-Datenbank-Instanz (Vorschau) befindet. 

Das virtuelle Netzwerk kann mit dem klassischen Bereitstellungsmodell oder dem Azure Resource Manager-Bereitstellungsmodell bereitgestellt werden. Wenn Sie die Azure SSIS-IR mit *demselben virtuellen Netzwerk* verknüpfen möchten, in dem sich eine verwaltete SQL-Datenbank-Instanz (Vorschau) befindet, stellen Sie sicher, dass sich die Azure SSIS-IR in einem *anderen Subnetz* als die verwaltete SQL-Datenbank-Instanz (Vorschau) befindet.   

Die folgenden Abschnitte enthalten hierzu weitere Informationen.

Folgende wichtige Punkte sind zu beachten: 

- Wenn kein vorhandenes virtuelles Netzwerk mit Ihrem lokalen Netzwerk verbunden ist, erstellen Sie zunächst ein [virtuelles Azure Resource Manager-Netzwerk](../virtual-network/quick-create-portal.md#create-a-virtual-network) oder ein [klassisches virtuelles Netzwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md), in das Ihre Azure SSIS-Integration Runtime eingebunden werden kann. Konfigurieren Sie dann eine Site-to-Site-[VPN-Gateway-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) oder eine [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md)-Verbindung von diesem virtuellen Netzwerk zu Ihrem lokalen Netzwerk.
- Wenn ein vorhandenes virtuelles Azure Resource Manager-Netzwerk oder ein klassisches virtuelles Netzwerk an demselben Standort wie Ihre Azure SSIS-IR mit Ihrem lokalen Netzwerk verbunden ist, können Sie die IR mit diesem virtuellen Netzwerk verknüpfen.
- Wenn ein klassisches virtuelles Netzwerk an einem anderen Standort als Ihre Azure SSIS-IR mit Ihrem lokalen Netzwerk verbunden ist, können Sie zuerst ein [klassisches virtuelles Netzwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) erstellen, mit der dann die Azure SSIS-IR verknüpft werden kann. Konfigurieren Sie dann eine [Verbindung zwischen den klassischen virtuellen Netzwerken](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md). Alternativ können Sie auch ein [virtuelles Azure Resource Manager-Netzwerk](../virtual-network/quick-create-portal.md#create-a-virtual-network) für die Verknüpfung Ihrer Azure SSIS-Integration Runtime erstellen. Konfigurieren Sie dann eine [Verbindung zwischen dem klassischen virtuellen Netzwerk und dem virtuellen Azure Resource Manager-Netzwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).
- Wenn ein virtuelles Azure Resource Manager-Netzwerk an einem anderen Standort als Ihre Azure SSIS-IR mit Ihrem lokalen Netzwerk verbunden ist, können Sie zuerst ein [virtuelles Azure Resource Manager-Netzwerk](../virtual-network/quick-create-portal.md##create-a-virtual-network) erstellen, in das dann die Azure SSIS-IR eingebunden werden kann. Konfigurieren Sie dann eine Verbindung zwischen den virtuellen Azure Resource Manager-Netzwerken. Alternativ können Sie auch ein [klassisches virtuelles Netzwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) für die Verknüpfung Ihrer Azure SSIS-IR erstellen. Konfigurieren Sie dann eine [Verbindung zwischen dem klassischen virtuellen Netzwerk und dem virtuellen Azure Resource Manager-Netzwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="domain-name-services-server"></a>Domain Name Service-Server 
Wenn Sie Ihren eigenen DNS-Server (Domain Name Services) in einem virtuellen Netzwerk verwenden müssen, dem auch Ihre Azure-SSIS-Integration Runtime angehört, gehen Sie gemäß der Anleitung im Abschnitt „Namensauflösung mithilfe eines eigenen DNS-Servers“ des Artikels [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) vor.

## <a name="network-security-group"></a>Netzwerksicherheitsgruppe
Wenn Sie eine Netzwerksicherheitsgruppe (NSG) in einem virtuellen Netzwerk implementieren müssen, mit dem Ihre Azure SSIS-Integration Runtime verknüpft ist, lassen Sie den eingehenden/ausgehenden Datenverkehr über die folgenden Ports zu:

| Ports | Richtung | Transportprotokoll | Zweck | Eingehende Quelle/ausgehendes Ziel |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100, 20100, 30100 (wenn Sie die IR mit einem klassischen virtuellen Netzwerk verknüpfen)<br/><br/>29876, 29877 (wenn Sie die IR mit einem virtuellen Azure Resource Manager-Netzwerk verknüpfen) | Eingehend | TCP | Azure-Dienste verwenden diese Ports für die Kommunikation mit den Knoten Ihrer Azure SSIS-Integration Runtime im virtuellen Netzwerk. | Internet | 
| 443 | Ausgehend | TCP | Die Knoten Ihrer Azure SSIS-Integration Runtime im virtuellen Netzwerk verwenden diesen Port für den Zugriff auf Azure-Dienste wie Azure Storage oder Azure Event Hubs. | Internet | 
| 1433<br/>11000-11999<br/>14000-14999  | Ausgehend | TCP | Die Knoten für Ihre Azure SSIS-Integration Runtime im virtuellen Netzwerk verwenden diese Ports für den Zugriff auf die SSISDB, die von Ihrem Azure SQL-Datenbank-Server gehostet wird. (Dieser Zweck gilt nicht für die SSISDB, die von der verwalteten SQL-Datenbank-Instanz [Vorschau] gehostet wird.) | Internet | 

## <a name="azure-portal-data-factory-ui"></a>Azure-Portal (Data Factory-Benutzeroberfläche)
In diesem Abschnitt erfahren Sie, wie Sie mithilfe des Azure-Portals und der Data Factory-Benutzeroberfläche eine vorhandene Azure SSIS Runtime mit einem virtuellen Netzwerk (klassisch oder Azure Resource Manager) verknüpfen. Zunächst müssen Sie das virtuelle Netzwerk entsprechend konfigurieren, bevor Ihre Azure SSIS-IR mit dem virtuellen Netzwerk verknüpft werden kann. Führen Sie die in einem der nächsten beiden Abschnitte beschriebenen Schritte aus, abhängig davon, welchen Typ Ihr virtuelles Netzwerk aufweist (klassisch oder Azure Resource Manager). Fahren Sie dann mit dem dritten Abschnitt fort, um Ihre Azure SSIS-IR mit dem virtuellen Netzwerk zu verknüpfen. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Verwenden des Portals zum Konfigurieren eines klassischen virtuellen Netzwerks
Sie müssen ein virtuelles Netzwerk konfigurieren, bevor Sie eine Azure SSIS-IR mit ihm verknüpfen können.

1. Starten Sie Microsoft Edge oder Google Chrome. Derzeit wird die Data Factory-Benutzeroberfläche nur in diesen Webbrowsern unterstützt.
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
3. Wählen Sie **Weitere Dienste**. Filtern Sie nach **Virtuelle Netzwerke (klassisch)**, und wählen Sie die Option aus.
4. Filtern Sie nach Ihrem virtuellen Netzwerk, und wählen Sie es in der Liste aus. 
5. Wählen Sie auf der Seite **Virtuelles Netzwerk (klassisch)** die Option **Eigenschaften** aus. 

    ![Ressourcen-ID des klassischen virtuellen Netzwerks](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Wählen Sie für **RESSOURCEN-ID** die Schaltfläche „Kopieren“ aus, um die Ressourcen-ID für das klassische Netzwerk in die Zwischenablage zu kopieren. Speichern Sie die ID aus der Zwischenablage in OneNote oder in einer Datei.
6. Wählen Sie im linken Menü **Subnetze** aus. Stellen Sie sicher, dass die Anzahl der **verfügbaren Adressen** größer ist als die Anzahl der Knoten in Ihrer Azure SSIS-Integration Runtime.

    ![Anzahl der verfügbaren Adressen im virtuellen Netzwerk](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Verknüpfen Sie **MicrosoftAzureBatch** mit der Rolle **Mitwirkender für klassische virtuelle Computer** für das virtuelle Netzwerk.

    a. Klicken Sie im linken Menü auf **Zugriffssteuerung (IAM)**, und wählen Sie dann auf der Symbolleiste **Hinzufügen** aus. 
       ![Schaltflächen „Zugriffssteuerung“ und „Hinzufügen“](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Wählen Sie auf der Seite **Berechtigungen hinzufügen** die Option **Mitwirkender für klassische virtuelle Computer** für **Rolle** aus. Fügen Sie **ddbf3205-c6bd-46ae-8127-60eb93363864** in das Textfeld **Auswählen** ein, und wählen Sie dann in der Liste der Suchergebnisse **Microsoft Azure Batch** aus.   
       ![Durchsuchen der Ergebnisse auf der Seite „Berechtigungen hinzufügen“](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    c. Wählen Sie **Speichern** aus, um die Einstellungen zu speichern und die Seite zu schließen.  
       ![Speichern der Zugriffseinstellungen](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    d. Vergewissern Sie sich, dass **Microsoft Azure Batch** in der Liste der Mitwirkenden angezeigt wird.  
       ![Bestätigen des Azure Batch-Zugriffs](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

5. Vergewissern Sie sich, dass dieser Azure Batch-Anbieter in dem Azure-Abonnement, das über das virtuelle Netzwerk verfügt, registriert ist. Alternativ können Sie auch den Azure Batch-Anbieter registrieren. Wenn in Ihrem Abonnement bereits ein Azure Batch-Konto enthalten ist, wird Ihr Abonnement für Azure Batch registriert.

   a. Wählen Sie im Azure-Portal im linken Menü **Abonnements** aus.

   b. Wählen Sie Ihr Abonnement aus.

   c. Wählen Sie auf der linken Seite **Ressourcenanbieter** aus, und bestätigen Sie, dass **Microsoft.Batch** ein registrierter Anbieter ist.     
      ![Bestätigung des Status „Registriert“](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Wenn **Microsoft.Batch** nicht in der Liste angezeigt wird, erstellen Sie zum Registrieren [ein leeres Azure Batch-Konto](../batch/batch-account-create-portal.md) in Ihrem Abonnement. Sie können es später wieder löschen. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Verwenden des Portals zum Konfigurieren eines virtuellen Azure Resource Manager-Netzwerks
Sie müssen ein virtuelles Netzwerk konfigurieren, bevor Sie eine Azure SSIS-IR mit ihm verknüpfen können.

1. Starten Sie Microsoft Edge oder Google Chrome. Derzeit wird die Data Factory-Benutzeroberfläche nur in diesen Webbrowsern unterstützt.
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
3. Wählen Sie **Weitere Dienste**. Filtern Sie nach **Virtuelle Netzwerke**, und wählen Sie die Option aus.
4. Filtern Sie nach Ihrem virtuellen Netzwerk, und wählen Sie es in der Liste aus. 
5. Wählen Sie auf der Seite **Virtuelles Netzwerk** die Option **Eigenschaften** aus. 
6. Wählen Sie für die **RESSOURCEN-ID** die Schaltfläche „Kopieren“ aus, um die Ressourcen-ID für das virtuelle Netzwerk in die Zwischenablage zu kopieren. Speichern Sie die ID aus der Zwischenablage in OneNote oder in einer Datei.
7. Wählen Sie im linken Menü **Subnetze** aus. Stellen Sie sicher, dass die Anzahl der **verfügbaren Adressen** größer ist als die Anzahl der Knoten in Ihrer Azure SSIS-Integration Runtime.
8. Vergewissern Sie sich, dass dieser Azure Batch-Anbieter in dem Azure-Abonnement, das über das virtuelle Netzwerk verfügt, registriert ist. Alternativ können Sie auch den Azure Batch-Anbieter registrieren. Wenn in Ihrem Abonnement bereits ein Azure Batch-Konto enthalten ist, wird Ihr Abonnement für Azure Batch registriert.

   a. Wählen Sie im Azure-Portal im linken Menü **Abonnements** aus.

   b. Wählen Sie Ihr Abonnement aus. 
   
   c. Wählen Sie auf der linken Seite **Ressourcenanbieter** aus, und bestätigen Sie, dass **Microsoft.Batch** ein registrierter Anbieter ist.     
      ![Bestätigung des Status „Registriert“](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Wenn **Microsoft.Batch** nicht in der Liste angezeigt wird, erstellen Sie zum Registrieren [ein leeres Azure Batch-Konto](../batch/batch-account-create-portal.md) in Ihrem Abonnement. Sie können es später wieder löschen.

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Verknüpfen der Azure SSIS-IR mit einem virtuellen Netzwerk


1. Starten Sie Microsoft Edge oder Google Chrome. Derzeit wird die Data Factory-Benutzeroberfläche nur in diesen Webbrowsern unterstützt.
2. Klicken Sie im [Azure-Portal](https://portal.azure.com) links im Menü auf **Data Factorys**. Wenn die Option **Data Factorys** nicht im Menü angezeigt wird, wählen Sie **Weitere Dienste** und dann im Abschnitt **INTELLIGENCE + ANALYSE** die Option **Data Factorys** aus. 
    
   ![Liste von Data Factorys](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)
2. Wählen Sie Ihre Data Factory mit der Azure SSIS-Integration Runtime in der Liste aus. Die Startseite für Ihre Data Factory wird angezeigt. Wählen Sie die Kachel **Verfassen und bereitstellen** aus. Die Data Factory-Benutzeroberfläche wird auf einer separaten Registerkarte angezeigt. 

   ![Data Factory-Startseite](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)
3. Wechseln Sie in der Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, klicken Sie auf **Verbindungen**, und wechseln Sie zur Registerkarte **Integration Runtimes**. 

   ![Registerkarte „Integration Runtimes“](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)
4. Wenn Ihre Azure SSIS IR ausgeführt wird, wählen Sie in der Integration Runtime-Liste in der Spalte **Aktionen** für Ihre Azure SSIS-IR die Schaltfläche **Beenden** aus. Sie können eine IR erst bearbeiten, wenn sie nicht länger ausgeführt wird. 

   ![Beenden der IR](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)
1. Wählen Sie in der Integration Runtime-Liste in der Spalte **Aktionen** für Ihre Azure SSIS-IR die Schaltfläche **Bearbeiten** aus.

   ![Bearbeiten der Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)
5. Klicken Sie auf der Seite **Allgemeine Einstellungen** des Fensters **Integration Runtime-Setup** auf **Weiter**. 

   ![Allgemeine Einstellungen für IR-Setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)
6. Geben Sie auf der Seite **SQL-Einstellungen** das Administratorkennwort ein, und wählen Sie **Weiter** aus.

   ![SQL Server-Einstellungen für IR-Setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)
7. Gehen Sie auf der Seite **Erweiterte Einstellungen** folgendermaßen vor: 

   a. Aktivieren Sie das Kontrollkästchen **Select a VNet for your Azure-SSIS Integration Runtime to join and allow Azure services to configure VNet permissions/settings** (VNET für Ihre Azure SSIS Integration Runtime zum Verknüpfen auswählen und Azure-Diensten das Konfigurieren von VNET-Berechtigungen/-Einstellungen erlauben).

   b. Geben Sie unter **Typ** an, ob es sich um ein klassisches virtuelles Netzwerk oder ein virtuelles Azure Resource Manager-Netzwerk handeln soll. 

   c. Wählen Sie unter **VNET-Name** Ihr virtuelles Netzwerk aus.

   d. Wählen Sie unter **Subnetzname** Ihr Subnet in dem virtuellen Netzwerk aus.

   e. Wählen Sie **Update** aus. 

   ![Erweiterte Einstellungen für IR-Setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)
8. Jetzt können Sie die IR starten, indem Sie in der Spalte **Aktionen** für Ihre Azure SSIS-IR die Schaltfläche **Starten** auswählen. Es dauert etwa 20 Minuten, bis eine Azure SSIS-IR gestartet wird. 


## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks
Sie müssen ein virtuelles Netzwerk konfigurieren, bevor Sie eine Azure SSIS-IR mit ihm verknüpfen können. Um Berechtigungen/Einstellungen für das virtuelle Netzwerk für die Azure SSIS-Integration Runtime, die mit dem Netzwerk verknüpft werden soll, automatisch zu konfigurieren, fügen Sie das folgende Skript hinzu:

```powershell
# Register to the Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
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

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Erstellen einer Azure SSIS-IR und Verknüpfen dieser mit einem virtuellen Netzwerk
Sie können eine Azure SSIS-IR erstellen und gleichzeitig mit einem virtuellen Netzwerk verknüpfen. Das vollständige Skript und Anweisungen finden Sie unter [Erstellen einer Azure SSIS-Integration Runtime](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Verknüpfen einer Azure SSIS-Integration Runtime mit einem virtuellen Netzwerk
Das Skript im Artikel [Erstellen einer Azure SSIS Integration Runtime](create-azure-ssis-integration-runtime.md) veranschaulicht, wie eine Azure SSIS-Integration Runtime im selben Skript erstellt und mit einem virtuellen Netzwerk verknüpft wird. Wenn Sie über eine Azure SSIS-IR verfügen, führen Sie die folgenden Schritte aus, um sie mit dem virtuellen Netzwerk zu verknüpfen: 

1. Beenden Sie die Azure SSIS IR.
2. Konfigurieren Sie die Azure SSIS-IR so, dass Sie mit dem virtuellen Netzwerk verknüpft wird. 
3. Starten Sie die Azure SSIS IR. 

### <a name="define-the-variables"></a>Definieren der Variablen

```powershell
$ResourceGroupName = "<Azure resource group name>"
$DataFactoryName = "<Data factory name>" 
$AzureSSISName = "<Specify Azure-SSIS IR name>"
## These two parameters apply if you are using a virtual network and Azure SQL Database Managed Instance (Preview) 
# Specify information about your classic or Azure Resource Manager virtual network.
$VnetId = "<Name of your Azure virtual network>"
$SubnetName = "<Name of the subnet in the virtual network>"
```

#### <a name="guidelines-for-selecting-a-subnet"></a>Richtlinien für das Auswählen eines Subnetzes
-   Wählen Sie nicht „GatewaySubnet“ für die Bereitstellung einer Azure-SSIS-Integrationslaufzeit aus, da es für Gateways des virtuellen Netzwerks vorgesehen ist.
-   Stellen Sie sicher, dass das ausgewählte Subnetz ausreichend verfügbaren Adressraum für die Verwendung durch die Azure-SSIS-Integrationslaufzeit aufweist. Belassen Sie mindestens die zweifache Anzahl der Integrationslaufzeitknoten in verfügbaren IP-Adressen. Einige IP-Adressen innerhalb jedes Subnetzes sind in Azure reserviert und können deshalb nicht genutzt werden. Die erste und letzte IP-Adresse der Subnetze sind aus Gründen der Protokollkonformität reserviert. Darüber hinaus sind drei weitere für Azure-Dienste verwendete IP-Adressen reserviert. Weitere Informationen finden Sie unter [Unterliegen die in den Subnetzen verwendeten IP-Adressen bestimmten Beschränkungen?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)


### <a name="stop-the-azure-ssis-ir"></a>Beenden der Azure SSIS IR
Beenden Sie die Azure SSIS-Integration Runtime, bevor Sie sie mit einem virtuellen Netzwerk verknüpfen. Dieser Befehl gibt alle Knoten frei und beendet die Abrechnung:

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force 
```
### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Konfigurieren der Einstellungen des virtuellen Netzwerks für das Verknüpfen der Azure SSIS-IR
Registrieren Sie sie beim Azure Batch-Ressourcenanbieter:

```powershell
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

### <a name="configure-the-azure-ssis-ir"></a>Konfigurieren der Azure SSIS IR
Um die Azure SSIS-Integration Runtime für die Verknüpfung mit dem virtuellen Netzwerk zu konfigurieren, führen Sie den Befehl `Set-AzureRmDataFactoryV2IntegrationRuntime` aus: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Starten der Azure SSIS IR
Führen Sie den folgenden Befehl aus, um die Azure SSIS-Integration Runtime zu starten: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```
Das Abschließen dieses Befehls dauert 20 bis 30 Minuten.

## <a name="use-azure-expressroute-with-the-azure-ssis-ir"></a>Verwenden von Azure ExpressRoute mit der Azure-SSIS-Integrationslaufzeit

Sie können eine [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)-Verbindung mit der Infrastruktur Ihres virtuellen Netzwerks verbinden und so Ihr lokales Netzwerk auf Azure ausdehnen. 

Eine gemeinsame Konfiguration muss Tunnelerzwingung verwenden (Ankündigen einer BGP-Route, 0.0.0.0/0 an das VNET), wodurch ausgehender Internetdatenverkehr gezwungen wird, aus dem VNET zur Überprüfung und Protokollierung an die lokale Netzwerk-Appliance zu fließen. Dieser Datenfluss unterbricht die Konnektivität zwischen der Azure-SSIS-Integrationslaufzeit im VNET mit abhängigen Azure Data Factory-Diensten. Die Lösung besteht darin, mindestens eine [benutzerdefinierte Route](../virtual-network/virtual-networks-udr-overview.md) (User-Defined Routes, UDRs) in dem Subnetz zu definieren, das die Azure-SSIS-Integrationslaufzeit enthält. Eine benutzerdefinierte Route definiert subnetzspezifische Routen, die anstelle der BGP-Route berücksichtigt werden.

Verwenden Sie wenn möglich die folgende Konfiguration:
-   Die ExpressRoute-Konfiguration kündigt 0.0.0.0/0 an und tunnelt standardmäßig allen ausgehenden Datenverkehr zwangsweise lokal.
-   Die für das Subnetz mit der Azure-SSIS-Integrationslaufzeit geltende UDR definiert die Route 0.0.0.0/0 mit „Internet“ als Typ des nächsten Hops.
- 
Gemeinsam führen diese Schritte dazu, dass die benutzerdefinierte Route auf Subnetzebene Vorrang vor erzwungenen ExpressRoute-Tunneln hat. Dadurch wird ausgehender Internetzugriff aus der Azure-SSIS-Integrationslaufzeit sichergestellt.

Wenn Sie sich Sorgen um den Verlust der Fähigkeit machen, ausgehenden Internetdatenverkehr von diesem Subnetz zu untersuchen, können Sie auch eine NSG-Regel im Subnetz hinzufügen, um ausgehende Ziele auf [IP-Adressen von Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653) zu beschränken.

Ein Beispiel finden Sie in [diesem PowerShell-Skript](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c). Sie müssen das Skript wöchentlich ausführen, um die IP-Adressenliste der Azure-Rechenzentren auf dem neuesten Stand zu halten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure SSIS-Runtime finden Sie in den folgenden Themen: 

- [Azure SSIS-Integration Runtime:](concepts-integration-runtime.md#azure-ssis-integration-runtime) Dieser Artikel enthält allgemeine konzeptionelle Informationen zu Integration Runtimes, einschließlich der Azure SSIS-IR. 
- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-create-azure-ssis-runtime-portal.md): Dieser Artikel enthält ausführliche Anweisungen zum Erstellen einer Azure SSIS-IR. Dabei wird eine Azure SQL-Datenbank zum Hosten des SSIS-Katalogs verwendet. 
- [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft-Dokumentation](create-azure-ssis-integration-runtime.md). In diesem Artikel wird das Tutorial vertieft, und er enthält Anweisungen zur Verwendung einer verwalteten Azure SQL-Datenbank-Instanz (Vorschau) und zum Verknüpfen der IR mit einem virtuellen Netzwerk. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel wird das Abrufen von Informationen zu einer Azure-SSIS-Integrationslaufzeit veranschaulicht, und er enthält Beschreibungen der Status in den zurückgegebenen Informationen. 
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md): In diesem Artikel wird beschrieben, wie Sie eine Azure-SSIS-Integrationslaufzeit beenden, starten oder entfernen. Außerdem wird gezeigt, wie Sie Ihre Azure SSIS-IR horizontal hochskalieren, indem Sie weitere Knoten hinzufügen. 

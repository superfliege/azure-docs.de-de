---
title: Tutorial – Durchführen eines Lastenausgleichs bei virtuellen Windows-Computern in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure PowerShell zum Erstellen eines Load Balancers für eine hoch verfügbare und sichere Anwendung über drei virtuelle Windows-Computer verwenden.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 35de71f3cc7d865cf2235a21bebf1cfcd7f1850d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application-with-azure-powershell"></a>Tutorial: Durchführen eines Lastenausgleichs bei virtuellen Windows-Computern in Azure zum Erstellen einer hoch verfügbaren Anwendung mit Azure PowerShell
Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit. In diesem Tutorial lernen Sie die verschiedenen Komponenten von Azure Load Balancer kennen, die den Datenverkehr verteilen und Hochverfügbarkeit bereitstellen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer Azure Load Balancer-Instanz
> * Erstellen des Integritätstests für den Load Balancer
> * Erstellen von Load Balancer-Regeln
> * Verwenden der Benutzerdefinierten Skripterweiterung zum Erstellen einer einfachen IIS-Website
> * Erstellen und Anfügen von virtuellen Computern an einen Load Balancer
> * Anzeigen eines Load Balancers im Betrieb
> * Hinzufügen und Entfernen von virtuellen Computern zu bzw. aus einem Load Balancer

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.


## <a name="azure-load-balancer-overview"></a>Übersicht über den Azure Load Balancer
Ein Azure Load Balancer ist ein Load Balancer der Schicht 4 (TCP, UDP), der Hochverfügbarkeit durch Verteilen des eingehenden Datenverkehrs auf fehlerfreie virtuelle Computer bietet. Der Integritätstest eines Load Balancers überwacht einen bestimmten Port auf jedem virtuellen Computer und verteilt Datenverkehr nur an einen betriebsbereiten virtuellen Computer.

Sie definieren eine Front-End-IP-Konfiguration, die eine oder mehrere öffentliche IP-Adressen enthält. Mit dieser Front-End-IP-Konfiguration sind der Load Balancer und Ihre Anwendungen über das Internet zugänglich. 

Virtuelle Computer werden über die zugehörige virtuelle Netzwerkschnittstelle (NIC) mit einem Load Balancer verbunden. Zum Verteilen von Datenverkehr auf die virtuellen Computer enthält ein Back-End-Adresspool die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind.

Um den Fluss des Datenverkehrs zu steuern, definieren Sie Load Balancer-Regeln für bestimmte Ports und Protokolle, die Ihren virtuellen Computern zugeordnet sind.


## <a name="create-azure-load-balancer"></a>Erstellen einer Azure Load Balancer-Instanz
In diesem Abschnitt wird erläutert, wie Sie jede Komponente des Load Balancers erstellen und konfigurieren. Bevor Sie eine Load Balancer-Instanz erstellen können, müssen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort *EastUS* eine Ressourcengruppe mit dem Namen *myResourceGroupLoadBalancer*:

```azurepowershell-interactive
New-AzureRmResourceGroup `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS"
```

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
Um über das Internet auf Ihre App zugreifen zu können, benötigen Sie eine öffentliche IP-Adresse für den Load Balancer. Erstellen Sie mit [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) eine öffentliche IP-Adresse. Das folgende Beispiel erstellt in der Ressourcengruppe *MyResourceGroupLoadBalancer* eine öffentliche IP-Adresse mit dem Namen *MyPublicIP*:

```azurepowershell-interactive
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS" `
  -AllocationMethod "Static" `
  -Name "myPublicIP"
```

### <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers
Erstellen Sie einen Front-End-IP-Pool mit [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Im folgenden Beispiel wird ein Front-End-IP-Pools namens *myFrontEndPool* erstellt und die Adresse *myPublicIP* angefügt: 

```azurepowershell-interactive
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
```

Erstellen Sie einen Back-End-Adresspool mit [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). In den verbleibenden Schritten werden die virtuellen Computer an diesen Back-End-Pool angefügt. Im folgenden Beispiel wird ein Back-End-Adresspool namens *myBackEndPool* erstellt:

```azurepowershell-interactive
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"
```

Erstellen Sie nun mit [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) den Load Balancer. Im folgenden Beispiel wird mithilfe der in den vorherigen Schritten erstellten Front-End- und Back-End-IP-Pools ein Load Balancer namens *myLoadBalancer* erstellt:

```azurepowershell-interactive
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myLoadBalancer" `
  -Location "EastUS" `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests
Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Falls bei einem virtuellen Computer innerhalb eines Intervalls von 15 Sekunden zwei aufeinanderfolgende Fehler auftreten, wird er standardmäßig aus der Load Balancer-Verteilung entfernt. Integritätstests werden auf der Grundlage eines Protokolls oder einer spezifischen Integritätsprüfungsseite für Ihre App erstellt. 

Im folgenden Beispiel wird ein TCP-Test erstellt. Sie können auch benutzerdefinierte HTTP-Tests für differenziertere Integritätsprüfungen erstellen. Bei Verwendung eines benutzerdefinierten HTTP-Tests müssen Sie die Integritätsprüfungsseite erstellen, z.B. *healthcheck.aspx*. Der Test muss die HTTP-Antwort **200 OK** zurückgeben, damit der Load Balancer den Host nicht aus der Rotation entfernt.

Um einen TCP-Integritätstest zu erstellen, verwenden Sie [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Im folgenden Beispiel wird ein Integritätstest namens *myHealthProbe* erstellt, der die einzelnen virtuellen Computer an *TCP*-Port *80* überwacht:

```azurepowershell-interactive
Add-AzureRmLoadBalancerProbeConfig `
  -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Aktualisieren Sie zum Anwenden des Integritätstests den Load Balancer mit [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```azurepowershell-interactive
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Um sicherzustellen, dass nur fehlerfreie VMs Datenverkehr empfangen, definieren Sie auch den zu verwendenden Integritätstest.

Erstellen Sie mit [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) eine Load Balancer-Regel. Im folgenden Beispiel wird eine Load Balancer-Regel mit dem Namen *myLoadBalancerRule* erstellt und der Datenverkehr an *TCP*-Port *80* ausgeglichen:

```azurepowershell-interactive
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name "myHealthProbe"

Add-AzureRmLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Aktualisieren Sie den Load Balancer mit [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```azurepowershell-interactive
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="configure-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks
Vor der Bereitstellung mehrerer virtueller Computer und dem Testen des Load Balancers müssen Sie zunächst die unterstützenden virtuellen Netzwerkressourcen erstellen. Weitere Informationen zu virtuellen Netzwerken finden Sie im Tutorial [Verwalten von virtuellen Azure-Netzwerken](tutorial-virtual-network.md).

### <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen
Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVnet* mit einem Subnetz namens *mySubnet* erstellt:

```azurepowershell-interactive
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS" `
  -Name "myVnet" `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Virtuelle NICs werden mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) erstellt. Im folgenden Beispiel werden drei virtuelle NICs erstellt (jeweils eine virtuelle NIC pro virtuellem Computer, den Sie in den folgenden Schritten für Ihre App erstellen). Sie können jederzeit weitere virtuelle NICs und virtuelle Computer erstellen und dem Load Balancer hinzufügen:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName "myResourceGroupLoadBalancer" `
     -Name myVM$i `
     -Location "EastUS" `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```


## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern
Fügen Sie die virtuellen Computer in eine Verfügbarkeitsgruppe ein, um die Hochverfügbarkeit Ihrer App zu optimieren.

Erstellen Sie mithilfe von [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) eine Verfügbarkeitsgruppe. Im folgenden Beispiel wird eine Verfügbarkeitsgruppe namens *myAvailabilitySet* erstellt:

```azurepowershell-interactive
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myAvailabilitySet" `
  -Location "EastUS" `
  -Sku aligned `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2
```

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort des Administrators der VMs fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Nun können Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) die VMs erstellen. Im folgenden Beispiel werden drei VMs und die erforderlichen Komponenten des virtuellen Netzwerks erstellt, falls sie nicht bereits vorhanden sind:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
    New-AzureRmVm `
        -ResourceGroupName "myResourceGroupLoadBalancer" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -OpenPorts 80 `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred `
        -AsJob
}
```

Mit dem Parameter `-AsJob` wird die VM als Hintergrundaufgabe erstellt, sodass die PowerShell-Aufforderungen für Sie zurückgegeben werden. Sie können die Details von Hintergrundaufträgen mit dem `Job`-Cmdlet anzeigen. Die Erstellung und Konfiguration aller drei virtuellen Computer dauert einige Minuten.


### <a name="install-iis-with-custom-script-extension"></a>Installieren von IIS mit benutzerdefinierter Skripterweiterung
In einem vorherigen Tutorial zum [Anpassen eines virtuellen Windows-Computers](tutorial-automate-vm-deployment.md) haben Sie erfahren, wie die Anpassung für virtuelle Computer mit der benutzerdefinierten Skripterweiterung für Windows automatisiert wird. Sie können den gleichen Ansatz verwenden, um IIS für Ihre virtuellen Computer zu installieren und konfigurieren.

Verwenden Sie [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) zum Installieren der benutzerdefinierten Skripterweiterung. Die Erweiterung führt `powershell Add-WindowsFeature Web-Server` zum Installieren des IIS-Webservers aus und aktualisiert dann die Seite *Default.htm* mit dem Hostnamen der VM:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName "myResourceGroupLoadBalancer" `
     -ExtensionName "IIS" `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.8 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Testen des Load Balancers
Rufen Sie mit [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für *myPublicIP* abgerufen, die wir zuvor erstellt haben:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myPublicIP" | select IpAddress
```

Geben Sie die öffentliche IP-Adresse in einem Webbrowser ein. Die Website wird mit dem Hostnamen der VM angezeigt, an die der Load Balancer den Datenverkehr wie im folgenden Beispiel verteilt hat:

![Ausführen der IIS-Website](./media/tutorial-load-balancer/running-iis-website.png)

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf alle drei virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird.


## <a name="add-and-remove-vms"></a>VMs hinzufügen und entfernen
Für die virtuellen Computer, auf denen Ihre App ausgeführt wird, sind unter Umständen gelegentlich Wartungsarbeiten erforderlich (etwa die Installation von Betriebssystemupdates). Zur Bewältigung eines höheren Datenverkehrsaufkommens für Ihre App müssen gegebenenfalls weitere virtuelle Computer hinzugefügt werden. In diesem Abschnitt erfahren Sie, wie Sie einen virtuellen Computer aus dem Load Balancer entfernen oder dem Load Balancer einen virtuellen Computer hinzufügen.

### <a name="remove-a-vm-from-the-load-balancer"></a>Entfernen eines virtuellen Computers aus dem Load Balancer
Rufen Sie die Netzwerkkarte mit [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) ab, und legen Sie dann die *LoadBalancerBackendAddressPools*-Eigenschaft der virtuellen NIC auf *$null* fest. Zum Schluss aktualisieren Sie die virtuelle NIC:

```azurepowershell-interactive
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName "myResourceGroupLoadBalancer" `
    -Name "myVM2"
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die beiden verbleibenden virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird. Nun können Sie Wartungsarbeiten für den virtuellen Computer durchführen und beispielsweise Betriebssystemupdates installieren oder den virtuellen Computer neu starten.

### <a name="add-a-vm-to-the-load-balancer"></a>Hinzufügen eines virtuellen Computers zum Load Balancer
Nach dem Ausführen der VM-Wartung, oder wenn Sie die Kapazität erweitern müssen, legen Sie die *LoadBalancerBackendAddressPools*-Eigenschaft der virtuellen NIC auf den *BackendAddressPool* von [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) fest:

Rufen Sie den Load Balancer ab:

```azurepowershell-interactive
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Load Balancer erstellt und ihm einen virtuellen Computer angefügt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer Azure Load Balancer-Instanz
> * Erstellen des Integritätstests für den Load Balancer
> * Erstellen von Load Balancer-Regeln
> * Verwenden der Benutzerdefinierten Skripterweiterung zum Erstellen einer einfachen IIS-Website
> * Erstellen und Anfügen von virtuellen Computern an einen Load Balancer
> * Anzeigen eines Load Balancers im Betrieb
> * Hinzufügen und Entfernen von virtuellen Computern zu bzw. aus einem Load Balancer

Im nächsten Tutorial erfahren Sie, wie Sie VM-Netzwerke verwalten.

> [!div class="nextstepaction"]
> [Verwalten virtueller Computer und virtueller Netzwerke](./tutorial-virtual-network.md)

---
title: Herstellen von Verbindungen zwischen virtuellen Netzwerken durch Peering virtueller Netzwerke – PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie durch Peering virtueller Netzwerke Verbindungen zwischen virtuellen Netzwerken hergestellt werden.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: c7b3fa2b566ab02e7fb4a03055db83f1545895e8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>Herstellen von Verbindungen zwischen virtuellen Netzwerken durch Peering virtueller Netzwerke mit PowerShell

Sie können durch Peering virtueller Netzwerke Verbindungen zwischen virtuellen Netzwerken herstellen. Sobald ein Peering zwischen virtuellen Netzwerken eingerichtet wurde, können Ressourcen in beiden virtuellen Netzwerken untereinander mit der gleichen Latenz und Bandbreite kommunizieren, als befänden sie sich im selben virtuellen Netzwerk. In diesem Artikel werden die Erstellung von virtuellen Netzwerken und das Peering zwischen diesen behandelt. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen zweier virtueller Netzwerke
> * Einrichten des Peerings zwischen virtuellen Netzwerken
> * Testen des Peerings

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 3.6 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen. 

## <a name="create-virtual-networks"></a>Erstellen virtueller Netzwerke

Vor der Erstellung eines virtuellen Netzwerks müssen Sie eine Ressourcengruppe für das virtuelle Netzwerk und alle anderen in diesem Artikel erstellten Ressourcen erstellen. Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVirtualNetwork1* mit dem Adresspräfix *10.0.0.0/16* erstellt.

```azurepowershell-interactive
$virtualNetwork1 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) eine Subnetzkonfiguration. Das folgende Beispiel erstellt eine Subnetzkonfiguration mit dem Adresspräfix 10.0.0.0/24:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

Schreiben Sie die Subnetzkonfiguration mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) in das virtuelle Netzwerk. Dabei wird das folgende Subnetz erstellt:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzureRmVirtualNetwork
```

Erstellen Sie ein virtuelles Netzwerk mit dem Adresspräfix 10.1.0.0/16 und einem Subnetz:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzureRmVirtualNetwork
```

Das Adresspräfix für das virtuelle Netzwerk *myVirtualNetwork2* überschneidet sich nicht mit dem Adresspräfix des virtuellen Netzwerks *myVirtualNetwork1*. Sie können kein Peering zwischen virtuellen Netzwerken mit sich überschneidenden Adresspräfixen erstellen.

## <a name="peer-virtual-networks"></a>Einrichten eines Peerings von virtuellen Netzwerken

Richten Sie das Peering mit [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) ein. Das folgende Beispiel legt ein Peering für *myVirtualNetwork1* mit *myVirtualNetwork2* fest.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

In der Ausgabe, die nach der Ausführung des vorherigen Befehl zurückgegeben wird, können Sie sehen, dass **PeeringState** den Wert *Initiiert* aufweist. Das Peering verbleibt im Zustand *Initiiert*, bis Sie das Peering zwischen *myVirtualNetwork2* und *myVirtualNetwork1* einrichten. Richten Sie das Peering zwischen *myVirtualNetwork2* und *myVirtualNetwork1* ein. 

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

In der Ausgabe, die nach der Ausführung des vorherigen Befehl zurückgegeben wird, können Sie sehen, dass **PeeringState** den Wert *Verbunden* aufweist. Azure hat zudem den Peeringstatus des Peerings *myVirtualNetwork1-myVirtualNetwork2* in *Verbunden* geändert. Überprüfen Sie mit [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering), ob der Peeringstatus für das Peering *myVirtualNetwork1-myVirtualNetwork2* in *Verbunden* geändert wurde.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Ressourcen in einem virtuellen Netzwerk können erst dann mit Ressourcen in anderen virtuellen Netzwerk kommunizieren, wenn **PeeringState** für die Peerings in beiden virtuellen Netzwerken den Wert *Verbunden* aufweist. 

Peerings werden zwischen zwei virtuellen Netzwerken erstellt, sie sind jedoch nicht transitiv. Wenn Sie daher z.B. ein Peering zwischen *myVirtualNetwork2* und *myVirtualNetwork3* einrichten möchten, müssen Sie ein zusätzliches Peering zwischen den virtuellen Netzwerken *myVirtualNetwork2* und *myVirtualNetwork3* erstellen. Auch wenn ein Peering zwischen *myVirtualNetwork1* und *myVirtualNetwork2* besteht, können Ressourcen in *myVirtualNetwork1* auf Ressourcen in *myVirtualNetwork3* nur dann zugreifen, wenn auch ein Peering zwischen *myVirtualNetwork1* und *myVirtualNetwork3* besteht. 

Vor dem Einrichten eines Peerings zwischen virtuellen Produktionsnetzwerken wird empfohlen, sich gründlich mit der [Übersicht über Peering](virtual-network-peering-overview.md), der [Verwaltung von Peering](virtual-network-manage-peering.md) und den [Grenzwerten für virtuelle Netzwerke](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vertraut zu machen. In diesem Artikel wird zwar ein Peering zwischen zwei virtuellen Netzwerken im selben Abonnement und am selben Standort veranschaulicht, Sie können jedoch auch ein Peering zwischen virtuellen Netzwerken in [unterschiedlichen Regionen](#register) und [verschiedenen Azure-Abonnements](create-peering-different-subscriptions.md#powershell) einrichten. Darüber hinaus können Sie [Hub-Spoke-Netzwerkentwürfe](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) mit Peering erstellen.

## <a name="test-peering"></a>Testen des Peerings

Um die Netzwerkkommunikation zwischen virtuellen Computern in verschiedenen virtuellen Netzwerken durch Peering zu testen, stellen Sie in jedem Subnetz einen virtuellen Computer bereit und kommunizieren dann zwischen den virtuellen Computern. 

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie in jedem virtuellen Netzwerk einen virtuellen Computer, damit Sie die private Kommunikation zwischen ihnen in einem späteren Schritt überprüfen können.

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) einen virtuellen Computer. Im folgenden Beispiel wird ein virtueller Computer namens *myVm1* im virtuellen Netzwerk *myVirtualNetwork1* erstellt. Mit der Option `-AsJob` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können. Geben Sie bei entsprechender Aufforderung den Benutzernamen und das Kennwort ein, mit denen Sie sich beim virtuellen Computer anmelden möchten.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

Azure weist automatisch 10.0.0.4 als private IP-Adresse des virtuellen Computers zu, da 10.0.0.4 die erste verfügbare IP-Adresse in *Subnet1* von *myVirtualNetwork1* ist. 

Erstellen Sie einen virtuellen Computer im virtuellen Netzwerk *myVirtualNetwork2*.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

Das Erstellen des virtuellen Computers dauert einige Minuten. Azure weist 10.1.0.4 als private IP-Adresse des virtuellen Computers zu, da 10.1.0.4 die erste verfügbare IP-Adresse im Subnetz *Subnet1* von *myVirtualNetwork2* ist. In der zurückgegebenen Ausgabe ist diese jedoch nicht enthalten. 

Fahren Sie nicht mit späteren Schritten fort, bis Azure den virtuellen Computer erstellt und die Ausgabe an PowerShell zurückgegeben hat.

### <a name="test-virtual-machine-communication"></a>Testen der Kommunikation zwischen virtuellen Computern

Sie können über das Internet eine Verbindung mit der öffentlichen IP-Adresse eines virtuellen Computers herstellen. Geben Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Das folgende Beispiel gibt die öffentliche IP-Adresse des virtuellen Computers *myVm1* zurück:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Erstellen Sie mit dem folgenden Befehl auf Ihrem lokalen Computer eine Remotedesktopsitzung mit dem virtuellen Computer *myVm1*. Ersetzen Sie `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene IP-Adresse.

```
mstsc /v:<publicIpAddress>
```

Eine RDP-Datei (Remotedesktopprotokoll) wird erstellt, auf Ihren Computer heruntergeladen und geöffnet. Geben Sie den Benutzernamen und das Kennwort ein. (Unter Umständen müssen Sie auf **Weitere Optionen** und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie dann auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

Aktivieren Sie an einer Eingabeaufforderung Ping über die Windows-Firewall, damit Sie diesen virtuellen Computer in einem späteren Schritt von *myVm2* aus pingen können.

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

In diesem Artikel wird für Testzwecke zwar Ping verwendet, das Zulassen von ICMP-Datenverkehr durch die Windows-Firewall wird jedoch für Produktionsbereitstellungen nicht empfohlen.

Um eine Verbindung mit dem virtuellen Computer *myVm2* herzustellen, geben Sie an einer Eingabeaufforderung auf dem virtuellen Computer *myVm1* den folgenden Befehl ein:

```
mstsc /v:10.1.0.4
```

Da Sie Ping auf *myVm1* aktiviert haben, können Sie diesen virtuellen Computer nun über eine Eingabeaufforderung auf dem virtuellen Computer *myVm2* anhand der IP-Adresse pingen:

```
ping 10.0.0.4
```

Sie erhalten vier Antworten. Wenn Sie anhand des Namens des virtuellen Computers (*myVm1*) anstatt der IP-Adresse pingen, tritt bei Ping ein Fehler auf, da *myVm1* ein unbekannter Hostname ist. Die Standardnamensauflösung von Azure funktioniert zwischen virtuellen Computern im selben virtuellen Netzwerk, jedoch nicht zwischen virtuellen Computern in verschiedenen virtuellen Netzwerken. Um Namen über verschiedene virtuelle Netzwerke hinweg aufzulösen, müssen Sie [einen eigenen DNS-Server bereitstellen](virtual-networks-name-resolution-for-vms-and-role-instances.md) oder [private Azure DNS-Domänen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verwenden.

Trennen Sie Ihre RDP-Sitzungen auf *myVm1* und *myVm2*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

**<a name="register"></a>Registrieren für die globale Vorschauversion für das Peering virtueller Netzwerke**

Das Peering von virtuellen Netzwerken in derselben Region befindet sich in der Phase der allgemeinen Verfügbarkeit. Das Peering von virtuellen Netzwerken in unterschiedlichen Regionen befindet sich derzeit in der Vorschauphase. Verfügbare Regionen finden Sie unter [Updates für virtuelle Netzwerke](https://azure.microsoft.com/updates/?product=virtual-network). Für ein regionsübergreifendes Peering virtueller Netzwerke müssen Sie sich zunächst für die Vorschauversion registrieren. Führen Sie dazu (innerhalb des Abonnements mit den einzelnen virtuellen Netzwerken, für die Sie ein Peering einrichten möchten) die folgenden Schritte aus:

1. Geben Sie die folgenden Befehle ein, um das Abonnement, in dem sich die einzelnen virtuellen Netzwerke befinden, für die Sie das Peering durchführen möchten, für die Vorschauversion zu registrieren:

    ```powershell-interactive
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
2. Bestätigen Sie, dass Sie für die Vorschauversion registriert sind, indem Sie folgenden Befehl eingeben:

    ```powershell-interactive    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    Wenn Sie versuchen, ein Peering virtueller Netzwerke in verschiedenen Regionen einzurichten, bevor die **RegistrationState**-Ausgabe nach Eingabe des vorherigen Befehls bei beiden Abonnements zu **Registriert** wechselt, tritt beim Peering ein Fehler auf.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie zwei Netzwerke durch Peering virtueller Netzwerke verbunden werden. Sie können über ein VPN [eine Verbindung zwischen Ihrem eigenen Computer und einem virtuellen Netzwerk herstellen](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und mit Ressourcen in einem virtuellen Netzwerk oder in durch Peering verbundenen virtuellen Netzwerken interagieren.

Fahren Sie mit den Beispielen für wiederverwendbare Skripts fort, um viele der in den Artikeln zu virtuellen Netzwerken behandelten Aufgaben durchzuführen.

> [!div class="nextstepaction"]
> [Skriptbeispiele für virtuelle Netzwerke](../networking/powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

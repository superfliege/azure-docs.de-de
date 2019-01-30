---
title: Erstellen und Verwalten des Azure-VPN-Gateways mit PowerShell | Microsoft-Dokumentation
description: 'Tutorial: Erstellen und Verwalten des VPN-Gateways mit dem Azure PowerShell-Modul'
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 05/14/2018
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: 8e3cdd99c99a300d7f1198826ae881373e179414
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54433695"
---
# <a name="create-and-manage-vpn-gateway-with-the-azure-powershell-module"></a>Erstellen und Verwalten des VPN-Gateways mit dem Azure PowerShell-Modul

Azure VPN-Gateways ermöglichen standortübergreifende Konnektivität zwischen lokalen Kundenumgebungen und Azure. In diesem Tutorial werden grundlegende Aufgaben der Azure VPN-Gatewaybereitstellung beschrieben, z.B. Erstellen und Verwalten eines VPN-Gateways. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines VPN-Gateways
> * Ändern der Größe eines VPN-Gateways
> * Zurücksetzen eines VPN-Gateways

Im folgenden Diagramm sind das virtuelle Netzwerk und das VPN-Gateway dargestellt, die im Rahmen dieses Tutorials erstellt wurden.

![VNet und VPN-Gateway](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="azure-cloud-shell-and-azure-powershell"></a>Azure Cloud Shell und Azure PowerShell

[!INCLUDE [working with cloudshell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.3 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen. 

## <a name="common-network-parameter-values"></a>Allgemeine Netzwerkparameterwerte

Ändern Sie die Werte unten gemäß Ihrer Umgebung und Netzwerkeinrichtung.

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$VNet1ASN    = 65010
$DNS1        = "8.8.8.8"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Zuerst muss eine Ressourcengruppe erstellt werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *TestRG1* in der Region *East US* (USA, Osten) erstellt:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Das Azure VPN-Gateway ermöglicht standortübergreifende Konnektivität und P2S-VPN-Serverfunktionalität für Ihr virtuelles Netzwerk. Fügen Sie das VPN-Gateway einem vorhandenen virtuellen Netzwerk hinzu, oder erstellen Sie ein neues virtuelles Netzwerk und das Gateway. In diesem Beispiel wird ein neues virtuelles Netzwerk mit drei Subnetzen erstellt: Front-End, Back-End und Gatewaysubnetz (unter Verwendung von [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) und [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)):

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubnet1 -AddressPrefix $GwPrefix1
$vnet   = New-AzureRmVirtualNetwork `
            -Name $VNet1 `
            -ResourceGroupName $RG1 `
            -Location $Location1 `
            -AddressPrefix $VNet1Prefix `
            -Subnet $fesub1,$besub1,$gwsub1
```

## <a name="request-a-public-ip-address-for-the-vpn-gateway"></a>Anfordern einer öffentlichen IP-Adresse für das VPN Gateway

Azure VPN-Gateways kommunizieren mit Ihren lokalen VPN-Geräten über das Internet, um die IKE-Aushandlung (Internet Key Exchange, Internetschlüsselaustausch) und IPsec-Tunnel einzurichten. Erstellen Sie eine öffentliche IP-Adresse, und weisen Sie sie Ihrem VPN-Gateway zu. Dies ist im Beispiel unten mit [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) und [New-AzureRmVirtualNetworkGatewayIpConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayipconfig) dargestellt:

> [!IMPORTANT]
> Derzeit können Sie nur eine dynamische öffentliche IP-Adresse für das Gateway verwenden. Die statische IP-Adresse wird auf Azure VPN-Gateways nicht unterstützt.

```azurepowershell-interactive
$gwpip    = New-AzureRmPublicIpAddress -Name $GwIP1 -ResourceGroupName $RG1 `
              -Location $Location1 -AllocationMethod Dynamic
$subnet   = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' `
              -VirtualNetwork $vnet
$gwipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 `
              -Subnet $subnet -PublicIpAddress $gwpip
```

## <a name="create-vpn-gateway"></a>Erstellen eines VPN-Gateways

Das Erstellen eines VPN-Gateways kann 45 Minuten oder länger dauern. Nachdem die Erstellung des Gateways abgeschlossen wurde, können Sie eine Verbindung Ihres virtuellen Netzwerks mit einem anderen virtuellen Netzwerk herstellen. Alternativ können Sie eine Verbindung zwischen dem virtuellen Netzwerk und einem lokalen Standort herstellen. Erstellen Sie ein VPN-Gateway mit dem [New-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/New-AzureRmVirtualNetworkGateway)-Cmdlet.

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Wichtige Parameterwerte:
* GatewayType: Verwenden Sie **Vpn** für Site-to-Site- und VNET-zu-VNET-Verbindungen.
* VpnType: Verwenden Sie **RouteBased**, um mit einem größeren Bereich von VPN-Geräten und mehr Routingfeatures zu interagieren.
* GatewaySku: **VpnGw1** ist die Standardeinstellung. Ändern Sie diese in „VpnGw2“ oder „VpnGw3“, wenn Sie einen höheren Durchsatz oder mehr Verbindungen benötigen. Weitere Informationen finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

Nachdem die Erstellung des Gateways abgeschlossen ist, können Sie eine Verbindung zwischen Ihrem virtuellen Netzwerk und einem anderen VNet oder eine Verbindung zwischen Ihrem virtuellen Netzwerk und einem lokalen Speicherort herstellen. Außerdem können Sie von einem Clientcomputer aus eine P2S-Verbindung mit Ihrem VNet konfigurieren.

## <a name="resize-vpn-gateway"></a>Ändern der Größe des VPN-Gateways

Sie können die VPN-Gateway-SKU ändern, nachdem das Gateway erstellt wurde. Unterschiedliche Gateway-SKUs unterstützen verschiedene Spezifikationen, z.B. Durchsätze, Verbindungsanzahl usw. Im folgenden Beispiel wird [Resize-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/Resize-AzureRmVirtualNetworkGateway) verwendet, um die Größe für Ihr Gateway von VpnGw1 in VpnGw2 zu ändern. Weitere Informationen finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

```azurepowershell-interactive
$gw = Get-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzureRmVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

Eine Änderung der VPN-Gatewaygröße dauert ca. 30 bis 45 Minuten, aber bei diesem Vorgang werden vorhandene Verbindungen und Konfigurationen **nicht** unterbrochen oder entfernt.

## <a name="reset-vpn-gateway"></a>Zurücksetzen des VPN-Gateways

Im Rahmen der Schritte für die Problembehandlung können Sie Ihr Azure VPN-Gateway zurücksetzen, um für das VPN-Gateway die IPsec-/IKE-Tunnelkonfigurationen neu zu starten. Verwenden Sie [Reset-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/Reset-AzureRmVirtualNetworkGateway), um Ihr Gateway zurückzusetzen.

```azurepowershell-interactive
$gw = Get-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Weitere Informationen finden Sie unter [Zurücksetzen eines VPN-Gateways](vpn-gateway-resetgw-classic.md).

## <a name="get-the-gateway-public-ip-address"></a>Abrufen der öffentlichen IP-Adresse des Gateways

Wenn Ihnen der Name der öffentlichen IP-Adresse bekannt ist, können Sie [Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress?view=azurermps-6.8.1) verwenden, um die öffentliche IP-Adresse anzuzeigen, die dem Gateway zugewiesen ist.

```azurepowershell-interactive
$myGwIp = Get-AzureRmPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="delete-vpn-gateway"></a>Löschen des VPN-Gateways

Für eine vollständige Konfiguration der standortübergreifenden und VNet-zu-VNet-Konnektivität sind zusätzlich zum VPN-Gateway mehrere Ressourcentypen erforderlich. Löschen Sie die Verbindungen, die dem VPN-Gateway zugeordnet sind, bevor Sie das Gateway selbst löschen. Nachdem das Gateway gelöscht wurde, können Sie die öffentlichen IP-Adressen für das Gateway löschen. Die ausführlichen Schritte finden Sie unter [Löschen eines VPN-Gateways](vpn-gateway-delete-vnet-gateway-powershell.md).

Wenn das Gateway Teil einer Prototyp- oder Proof-of-Concept-Bereitstellung ist, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe, das VPN-Gateway und alle dazugehörigen Ressourcen zu entfernen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zur grundlegenden Erstellung und Verwaltung von VPN-Gateways erhalten, z.B.:

> [!div class="checklist"]
> * Erstellen eines VPN-Gateways
> * Ändern der Größe eines VPN-Gateways
> * Zurücksetzen eines VPN-Gateways

Fahren Sie mit den folgenden Tutorials fort, um sich über S2S-, VNET-zu-VNET- und P2S-Verbindungen zu informieren.

> [!div class="nextstepaction"]
> * [Erstellen von S2S-Verbindungen](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [Erstellen von VNet-zu-VNet-Verbindungen](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Erstellen von P2S-Verbindungen](vpn-gateway-howto-point-to-site-resource-manager-portal.md)

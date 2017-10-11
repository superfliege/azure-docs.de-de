---
title: "Konfigurieren von ExpressRoute und ein Standort-zu-Standort-VPN-Verbindungen, die gleichzeitig ausgeführt werden können: Ressourcen-Manager: Azure | Microsoft Docs"
description: "Dieser Artikel führt Sie durch Konfigurieren von ExpressRoute und ein Standort-zu-Standort-VPN-Verbindung, die für Ressourcen-Manager-Modell parallel verwendet werden kann."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: charwen,cherylmc
ms.openlocfilehash: b29147a37f9a90fc80e16b350ac9b91daac1d7f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a>Konfigurieren Sie Koexistenz ExpressRoute und ein Standort-zu-Standort-Verbindungen
> [!div class="op_single_selector"]
> * [PowerShell - Ressourcen-Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell - Classic](expressroute-howto-coexist-classic.md)
> 
> 

Zum Konfigurieren der Koexistenz Standort-zu-Standort-VPN- und ExpressRoute-Verbindungen hat mehrere Vorteile. Sie können konfigurieren ein Standort-zu-Standort-VPNs als einen sicheren failoverpfad für ExressRoute oder verwenden Standort-zu-Standort-VPNs zur Verbindung mit Standorten, die nicht über ExpressRoute verbunden sind. Die Schritte zum Konfigurieren der beiden Szenarien in diesem Artikel beschrieben. Dieser Artikel gilt für die Ressourcen-Manager-Bereitstellungsmodell und PowerShell verwendet. Diese Konfiguration ist nicht im Azure-Portal verfügbar.

> [!IMPORTANT]
> ExpressRoute-Verbindungen müssen bereits konfiguriert sein, bevor Sie die unten aufgeführten Anleitungen befolgen. Stellen Sie sicher, dass Sie ausgeführt haben, die Führungslinien auf [ExpressRoute-Verbindung erstellen](expressroute-howto-circuit-arm.md) und [Konfigurieren des Routings](expressroute-howto-routing-arm.md) bevor Sie fortfahren.
> 
> 

## <a name="limits-and-limitations"></a>Limits und Einschränkungen
* **Transitrouting wird nicht unterstützt.** Sie können nicht zwischen Ihrem lokalen Netzwerk über Standort-zu-Standort-VPN verbunden sind und Ihr lokales Netzwerk über ExpressRoute verbunden sind (über Azure) weiterleiten.
* **Gateway Basic SKU wird nicht unterstützt.** Sie müssen ein nicht - SKUs vom Typ Basic-Gateway für beide verwenden die [ExpressRoute-Gateway](expressroute-about-virtual-network-gateways.md) und die [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Nur weiterleitungsbasierte VPN-Gateway wird unterstützt.** Verwenden Sie eine Route basierenden [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Statische Route sollten für Ihre VPN-Gateway konfiguriert werden.** Wenn Ihr lokale Netzwerk mit ExpressRoute und ein Standort-zu-Standort-VPN verbunden ist, benötigen Sie einen statischen Routen, die in Ihrem lokalen Netzwerk zum Weiterleiten der Standort-zu-Standort-VPN-Verbindungs mit dem öffentlichen Internet konfiguriert.
* **ExpressRoute-Gateway muss zunächst konfiguriert und mit einer Verbindung verknüpft werden.** Sie müssen zuerst das ExpressRoute-Gateway erstellen und mit einer Verbindung zu verknüpfen, bevor Sie das Standort-zu-Standort-VPN-Gateway hinzufügen.

## <a name="configuration-designs"></a>Konfiguration-designs
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurieren eines Standort-zu-Standort-VPNs als einen failoverpfad für ExpressRoute
Sie können eine Standort-zu-Standort-VPN-Verbindung als Sicherung für ExpressRoute konfigurieren. Dies gilt nur für virtuelle Netzwerke, die mit den Azure privaten peeringpfad verknüpft. Es ist keine VPN-basierten Failoverlösung für Dienste über das öffentliche Azure und Microsoft kann zugegriffen werden kann. Die ExpressRoute-Verbindung ist immer der primären Link. Die Daten fließen über den Pfad für die Standort-zu-Standort-VPN, nur, wenn die ExpressRoute-Verbindung ein Fehler auftritt.

> [!NOTE]
> Während der ExpressRoute-Verbindung wird über eine Standort-zu-Standort-VPN-bevorzugt, wenn beide Routen identisch sind, wird Azure die Abgleichs des längsten Präfix verwenden, um die Route für das Paket Ziel auszuwählen.
> 
> 

![Zusammen verwendet werden](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>Konfigurieren eines Standort-zu-Standort-VPNs zur Verbindung mit Standorten, die nicht über ExpressRoute verbunden
Sie können Ihr Netzwerk, in dem einige Websites, die direkt in Azure über eine Standort-zu-Standort-VPN-Verbindung, und einige Standorte, die eine Verbindung herstellen über Expressroute, konfigurieren. 

![Zusammen verwendet werden](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> Sie können nicht als Router während der Übertragung ein virtuelles Netzwerk konfigurieren.
> 
> 

## <a name="selecting-the-steps-to-use"></a>Verwenden Sie die Schritte auswählen
Es gibt zwei verschiedene Sätzen von Verfahren zur Auswahl. Die Vorgehensweise, die Sie auswählen, hängt davon ab, ob Sie ein vorhandenes virtuelles Netzwerk, die Sie herstellen möchten haben, oder Sie ein neues virtuelles Netzwerk erstellen möchten.

* Ich eine VNet besitzen, und erstellen müssen.
  
    Wenn Sie ein virtuelles Netzwerk noch nicht haben, erläutert dieser Prozedur erstellen ein neues virtuelles Netzwerk mithilfe von Ressourcen-Manager-Bereitstellungsmodell und zum Erstellen neuer ExpressRoute und ein Standort-zu-Standort-VPN-Verbindungen. Um ein virtuelles Netzwerk zu konfigurieren, befolgen Sie die Schritte im [zum Erstellen eines neuen virtuellen Netzwerks und Koexistenz Verbindungen](#new).
* Ich habe bereits ein Ressourcen-Manager-Bereitstellungsmodell VNet.
  
    Möglicherweise verfügen Sie bereits ein virtuelles Netzwerk mit einem vorhandenen Standort-zu-Standort-VPN-Verbindung oder eine ExpressRoute-Verbindung. Die [so konfigurieren Sie die Koexistenz Verbindungen für eine bereits vorhandene VNet](#add) Abschnitt führt Sie durch das Gateway löschen und erstellen anschließend neue ExpressRoute und ein Standort-zu-Standort-VPN-Verbindungen. Wenn Sie die neue Verbindungen zu erstellen, müssen die Schritte in einer bestimmten Reihenfolge ausgeführt werden. Verwenden Sie keine der Anweisungen in anderen Artikeln die Gateways und Verbindungen zu erstellen.
  
    In diesem Verfahren erfordert Erstellen von Verbindungen, die gleichzeitig ausgeführt werden können Sie Ihr Gateway löschen und konfigurieren Sie dann auf neuer-Gateways. Sie müssen Ausfallzeiten für Ihre standortübergreifende Verbindungen, während Sie gelöscht und neu erstellt, dem Gateway und Verbindungen, aber Sie keine Migration von Ihrem virtuellen Computer oder Dienste, ein neues virtuelles Netzwerk müssen. Ihre virtuellen Computer und Dienste werden, über den Lastenausgleich zu kommunizieren, während das Gateway konfigurieren, wenn sie zu diesem Zweck konfiguriert sind.

## <a name="new"></a>So erstellen ein neues virtuelles Netzwerk und Koexistenz Verbindungen
Dieses Verfahren führt Sie durch die Erstellung einem VNet und Standort-zu-Standort- und ExpressRoute-Verbindungen, die gleichzeitig vorhanden sein werden.

1. Installieren Sie die neueste Version der Azure-PowerShell-Cmdlets. Informationen zum Installieren der Cmdlets finden Sie unter [zum Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). Die Cmdlets, mit denen Sie für diese Konfiguration ist möglicherweise unterscheidet sich etwas davon ab, was Sie mit vertraut sein könnte. Achten Sie darauf, dass Sie die Cmdlets, die in diesen Anweisungen angegeben verwenden.
2. Melden Sie sich mit Ihrem Konto und Einrichten der Umgebung.

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. Erstellen eines virtuellen Netzwerks, einschließlich der Gateway-Subnetz an. Weitere Informationen zur Konfiguration virtuellen Netzwerks finden Sie unter [Azure Virtual Network Configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).
   
   > [!IMPORTANT]
   > Das Gatewaysubnetz muss es sich um /27 oder einem kürzeren Präfix (z. B. /26 oder /25) sein.
   > 
   > 
   
    Erstellen Sie ein neues VNet.

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    Subnetze hinzufügen.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Die VNet-Konfiguration zu speichern.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <a name="gw"></a>Erstellen Sie eine ExpressRoute-Gateway. Weitere Informationen über die ExpressRoute-Gateway-Konfiguration finden Sie unter [ExpressRoute-Gateway-Konfiguration](expressroute-howto-add-gateway-resource-manager.md). Die GatewaySKU muss *Standard*, *HighPerformance*, oder *UltraPerformance*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. Verknüpfen Sie das ExpressRoute-Gateway mit der ExpressRoute-Verbindung. Nachdem dieser Schritt abgeschlossen wurde, wird die Verbindung zwischen Ihrem lokalen Netzwerk und Azure über ExpressRoute, hergestellt. Weitere Informationen zu den Verknüpfungsvorgang, finden Sie unter [Link VNets an ExpressRoute](expressroute-howto-linkvnet-arm.md).

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <a name="vpngw"></a>Als Nächstes erstellen Sie das Standort-zu-Standort-VPN-Gateway. Weitere Informationen über die VPN-Gateway-Konfiguration finden Sie unter [Konfigurieren einer VNet mit einer Standort-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). Die GatewaySKU muss *Standard*, *HighPerformance*, oder *UltraPerformance*. Das VPN muss *als routebased festgelegt*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    Azure-VPN-Gateway unterstützt BGP-Routingprotokoll. Sie können ASN (AS-Nummer) für dieses virtuelle Netzwerk angeben, indem Sie die Asn - Switch in den folgenden Befehl hinzufügen. Nicht angeben, dass diese Parameter als Nummer 65515 standardmäßig generiert.

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    Sie finden das BGP-peering IP- und AS-Nummer, die Azure für das VPN-Gateway in $azureVpn.BgpSettings.BgpPeeringAddress und $azureVpn.BgpSettings.Asn verwendet. Weitere Informationen finden Sie unter [konfigurieren BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) für Azure-VPN-Gateway.
7. Erstellen Sie einen lokalen Standort-VPN-Gateway-Entität. Mit diesem Befehl nicht auf Ihr lokales VPN-Gateway konfigurieren. Stattdessen können Sie das lokale Gateway-Einstellungen bereitzustellen, z. B. die öffentliche IP-Adresse und das lokale, Adressraums, damit das Azure-VPN-Gateway hergestellt werden kann.
   
    Wenn Ihr lokale VPN-Gerät nur statisches routing unterstützt, können Sie die statischen Routen wie folgt konfigurieren:

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    Wenn Ihr lokale VPN-Gerät das BGP unterstützt, und Sie dynamisches routing aktivieren möchten, müssen Sie wissen, das BGP-peering IP- und AS-Nummer, die das lokale VPN-Gerät verwendet.

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for the BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. Konfigurieren Sie Ihres lokalen VPN-Geräts für die Verbindung zum neuen Azure-VPN-Gateway. Weitere Informationen zu VPN-Gerätekonfiguration, finden Sie unter [VPN-Gerätekonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
9. Verknüpfen Sie das Standort-zu-Standort-VPN-Gateway in Azure für das lokale Gateway.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <a name="add"></a>So konfigurieren Sie die Koexistenz Verbindungen für eine bereits vorhandene VNet
Wenn Sie ein vorhandenes virtuelles Netzwerk verfügen, überprüfen Sie die Größe der Gateway-Subnetz. Ist das gatewaysubnetz /28 oder /29, müssen Sie das virtuelle Netzwerkgateway löschen und erhöhen Sie die Größe der Gateway-Subnetz. Die Schritte in diesem Abschnitt veranschaulichen, wie nachholen.

Wenn das Gateway-Subnetz /27 ist groß und das virtuelle Netzwerk über ExpressRoute verbunden ist, überspringen Sie die folgenden Schritte aus und fahren Sie mit ["Schritt 6: Erstellen einer Standort-zu-Standort-VPN-Gateway"](#vpngw) im vorherigen Abschnitt. 

> [!NOTE]
> Wenn Sie das vorhandene Gateway löschen, verlieren lokal vor Ort die Verbindung mit dem virtuellen Netzwerk, während der Arbeit für diese Konfiguration. 
> 
> 

1. Sie müssen die neueste Version der Azure-PowerShell-Cmdlets zu installieren. Weitere Informationen zum Installieren der Cmdlets finden Sie unter [zum Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). Die Cmdlets, mit denen Sie für diese Konfiguration ist möglicherweise unterscheidet sich etwas davon ab, was Sie mit vertraut sein könnte. Achten Sie darauf, dass Sie die Cmdlets, die in diesen Anweisungen angegeben verwenden. 
2. Löschen Sie das vorhandene Expressroute- oder Standort-zu-Standort-VPN-Gateway.

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. Gateway-Subnetz zu löschen.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. Fügen Sie ein Gateway-Subnetz, das /27 ist groß.
   
   > [!NOTE]
   > Wenn Sie nicht über genügend IP-Adressen in Ihrem virtuellen Netzwerk so erhöhen Sie die Größe der Gateway-Subnetz Links verfügen, müssen Sie weitere IP-Adressbereich hinzufügen.
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Die VNet-Konfiguration zu speichern.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. An diesem Punkt haben Sie eine VNet mit keine Gateways. Zur neuen Gateways erstellen, und schließen Sie die Verbindungen, können Sie mit der Fortfahren [Schritt 4 – Erstellen Sie eine ExpressRoute-Gateway](#gw), in der vorherigen Gruppe von Schritten gefunden.

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a>So fügen Sie Punkt-zu-Standort-Konfiguration für das VPN-Gateway hinzu
Sie können die nachfolgenden Schritte zum Hinzufügen von Punkt-zu-Standort-Konfiguration mit dem VPN-Gateway in einem Koexistenz-Setup ausführen.

1. Fügen Sie die VPN-clientadresspool hinzu.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. Hochladen Sie das VPN-Stammzertifikat in Azure, für das VPN-Gateway. In diesem Beispiel wird davon ausgegangen, dass das Stammzertifikat auf dem lokalen Computer gespeichert sind, in denen die folgenden PowerShell-Cmdlets ausgeführt werden.

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

Weitere Informationen zu Punkt-zu-Standort-VPN, finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu ExpressRoute finden Sie unter der [ExpressRoute – häufig gestellte Fragen](expressroute-faqs.md).

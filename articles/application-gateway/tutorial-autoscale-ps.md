---
title: 'Tutorial: Erstellen einer zonenredundanten Application Gateway-Instanz mit automatischer Skalierung und reservierter IP-Adresse – Azure PowerShell'
description: In diesem Tutorial erfahren Sie, wie Sie mit Azure PowerShell eine zonenredundante Anwendungsgatewayinstanz mit automatischer Skalierung und einer reservierten IP-Adresse erstellen.
services: application-gateway
author: amitsriva
ms.service: application-gateway
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: dd6cc65fca98bc435a8cfea575ba10e3cff376be
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54424675"
---
# <a name="tutorial-create-an-application-gateway-that-improves-web-application-access"></a>Tutorial: Erstellen eines Anwendungsgateways, das den Zugriff auf die Webanwendung verbessert

Wenn Sie als IT-Administrator für die Verbesserung des Zugriffs auf die Webanwendung verantwortlich sind, können Sie Ihr Anwendungsgateway optimieren, sodass es der Kundennachfrage gemäß skaliert wird und mehrere Verfügbarkeitszonen abdeckt. Dieses Tutorial hilft Ihnen beim Konfigurieren der Azure Application Gateway-Features für automatische Skalierung, Zonenredundanz und reservierte virtuellen IP-Adressen (VIPs) bzw. statische IP-Adressen. Sie verwenden Azure PowerShell-Cmdlets und das Azure Resource Manager-Bereitstellungsmodell, um das Problem zu lösen.

> [!IMPORTANT] 
> Die Application Gateway-SKU mit automatischer Skalierung und Zonenredundanz ist derzeit als öffentliche Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks mit automatischer Skalierung
> * Erstellen einer reservierten öffentlichen IP-Adresse
> * Einrichten der Anwendungsgatewayinfrastruktur
> * Konfigurieren der automatischen Skalierung
> * Erstellen des Anwendungsgateways
> * Testen des Anwendungsgateways

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial müssen Sie Azure PowerShell lokal ausführen. Sie müssen das Azure PowerShell-Modul Version 6.9.0 oder höher installiert haben. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu. Führen Sie nach dem Überprüfen der PowerShell-Version `Login-AzureRmAccount` aus, um eine Verbindung mit Azure zu erstellen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -Subscription "<sub name>"
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie eine Ressourcengruppe in einer der verfügbaren Regionen.

```azurepowershell
$location = "East US 2"
$rg = "<rg name>"

#Create a new Resource Group
New-AzureRmResourceGroup -Name $rg -Location $location
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie ein virtuelles Netzwerk mit einem dedizierten Subnetz für ein Anwendungsgateway mit automatischer Skalierung. Derzeit kann in jedem dedizierten Subnetz nur ein Anwendungsgateway mit automatischer Skalierung bereitgestellt werden.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzureRmVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Erstellen einer reservierten öffentlichen IP-Adresse

Geben Sie als Zuordnungsmethode für PublicIPAddress **Statisch** an. Die virtuelle IP eines Anwendungsgateways mit automatischer Skalierung kann nur statisch sein. Dynamische IPs werden nicht unterstützt. Es wird nur die standardmäßige „PublicIpAddress“-SKU unterstützt.

```azurepowershell
#Create static public IP
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Abrufen von Details

Rufen Sie Details zur Ressourcengruppe, zum Subnetz und zur IP-Adresse in einem lokalen Objekt ab, um die IP-Konfigurationsdetails für das Anwendungsgateway zu erstellen.

```azurepowershell
$resourceGroup = Get-AzureRmResourceGroup -Name $rg
$publicip = Get-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```

## <a name="configure-the-infrastructure"></a>Konfigurieren der Infrastruktur

Konfigurieren Sie IP, Front-End-IP, Back-End-Pool, HTTP-Einstellungen, Zertifikat, Port, Listener und Regel im gleichen Format wie das vorhandene Standardanwendungsgateway. Die neue SKU folgt dem gleichen Objektmodell wie die Standard-SKU.

```azurepowershell
$ipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzureRmApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "SSLPort" -Port 443
$fp02 = New-AzureRmApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$securepfxpwd = ConvertTo-SecureString -String "scrap" -AsPlainText -Force
$sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -Password $securepfxpwd `
            -CertificateFile "D:\Networking\ApplicationGateway\scrap.pfx"
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "SSLListener" `
             -Protocol Https -FrontendIPConfiguration $fip -FrontendPort $fp01 -SslCertificate $sslCert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp02

$setting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule2" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener02 -BackendAddressPool $pool
```

## <a name="specify-autoscale"></a>Konfigurieren der automatischen Skalierung

Jetzt können Sie die Konfiguration der automatischen Skalierung für das Anwendungsgateway angeben. Zwei Konfigurationstypen werden für die automatische Skalierung unterstützt:

* **Modus mit fester Kapazität**: In diesem Modus kann die Application Gateway-Instanz keine automatische Skalierung vornehmen. Sie wird mit einer festen Kapazität für die Skalierungseinheit ausgeführt.

   ```azurepowershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2
   ```

* **Modus mit automatischer Skalierung**: In diesem Modus nimmt das Anwendungsgateway basierend auf dem Muster des Anwendungsdatenverkehrs eine automatische Skalierung vor.

   ```azurepowershell
   $autoscaleConfig = New-AzureRmApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Erstellen Sie das Anwendungsgateway, und beziehen Sie Redundanzzonen und die Konfiguration der automatischen Skalierung ein.

```azurepowershell
$appgw = New-AzureRmApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 `
  -ResourceGroupName $rg -Location $location -BackendAddressPools $pool `
  -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig `
  -FrontendIpConfigurations $fip -FrontendPorts $fp01, $fp02 `
  -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 `
  -Sku $sku -sslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Verwenden Sie Get-AzureRmPublicIPAddress, um die öffentliche IP-Adresse des Anwendungsgateways abzurufen. Kopieren Sie die öffentliche IP-Adresse oder den DNS-Namen, und fügen Sie den kopierten Inhalt in die Adressleiste des Browsers ein.

`Get-AzureRmPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP`

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Untersuchen Sie zuerst die Ressourcen, die mit dem Anwendungsgateway erstellt wurden. Wenn sie nicht mehr benötigt werden, können Sie Ressourcengruppe, Anwendungsgateway und alle zugehörigen Ressourcen mit dem Befehl `Remove-AzureRmResourceGroup` entfernen.

`Remove-AzureRmResourceGroup -Name $rg`

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines Anwendungsgateways mit Routingregeln auf URL-Pfadbasis](./tutorial-url-route-powershell.md)

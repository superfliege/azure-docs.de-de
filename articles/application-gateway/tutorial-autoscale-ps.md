---
title: Erstellen einer zonenredundanten Application Gateway-Instanz mit automatischer Skalierung und reservierter IP-Adresse – Azure PowerShell
description: Erfahren Sie, wie Sie Azure PowerShell eine zonenredundante Application Gateway-Instanz mit automatischer Skalierung und einer reservierten IP-Adresse erstellen.
services: application-gateway
author: amitsriva
ms.service: application-gateway
ms.topic: tutorial
ms.date: 9/26/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 6c54706f45653c43e6b41d0adb3132583079e6b6
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47167525"
---
# <a name="tutorial-create-an-autoscaling-zone-redundant-application-gateway-with-a-reserved-virtual-ip-address-using-azure-powershell"></a>Tutorial: Erstellen einer zonenredundanten Application Gateway-Instanz mit automatischer Skalierung und reservierter virtueller IP-Adresse mit Azure PowerShell

Dieses Tutorial beschreibt, wie Sie eine Azure Application Gateway-Instanz mit Azure PowerShell-Cmdlets und dem Azure Resource Manager-Bereitstellungsmodell erstellen. Dieses Tutorial konzentriert sich auf die Unterschiede zwischen der neuen SKU mit automatischer Skalierung und der vorhandenen Standard-SKU. Vor allem gehen wir auf die Funktionen zur Unterstützung der automatischen Skalierung, Zonenredundanz und reservierten virtuellen IPs (statische IP) ein.

Weitere Informationen zu Anwendungsgateways mit automatischer Skalierung und Zonenredundanz finden Sie unter [Application Gateway mit automatischer Skalierung und Zonenredundanz (Public Preview)](application-gateway-autoscaling-zone-redundant.md).

> [!IMPORTANT]
> Die Application Gateway-SKU mit automatischer Skalierung und Zonenredundanz ist derzeit als öffentliche Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten des Konfigurationsparameters für die automatische Skalierung
> * Verwenden des Zeitzonenparameters
> * Verwenden einer statischen virtuellen IP
> * Erstellen des Anwendungsgateways


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für dieses Tutorial müssen Sie Azure PowerShell lokal ausführen. Sie müssen das Azure PowerShell-Modul Version 6.9.0 oder höher installiert haben. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) Informationen dazu. Führen Sie nach dem Überprüfen der PowerShell-Version `Login-AzureRmAccount` aus, um eine Verbindung mit Azure zu erstellen.

## <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

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

## <a name="create-a-vnet"></a>Erstellen eines VNET
Erstellen Sie ein VNET mit einem dedizierten Subnetz für ein Anwendungsgateway mit automatischer Skalierung. Derzeit kann in jedem dedizierten Subnetz nur ein Anwendungsgateway mit automatischer Skalierung bereitgestellt werden.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzureRmVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Erstellen einer reservierten öffentlichen IP-Adresse

Geben Sie die Zuordnungsmethode für „PublicIPAddress“ als **Statisch** an. Die virtuelle IP eines Anwendungsgateways mit automatischer Skalierung kann nur statisch sein. Dynamische IPs werden nicht unterstützt. Es wird nur die standardmäßige „PublicIpAddress“-SKU unterstützt.

```azurepowershell
#Create static public IP
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Abrufen von Details

Rufen Sie Details zur Ressourcengruppe, zum Subnetz und zur IP-Adresse in einem lokalen Objekt ab, um die IP-Konfigurationsdetails des Anwendungsgateways zu erstellen.

```azurepowershell
$resourceGroup = Get-AzureRmResourceGroup -Name $rg
$publicip = Get-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```
## <a name="configure-application-gateway-infrastructure"></a>Konfigurieren der Application Gateway-Infrastruktur
Konfigurieren Sie IP, Front-End-IP, Back-End-Pool, HTTP-Einstellungen, Zertifikat, Port, Listener und Regel im gleichen Format wie die vorhandene Standard-SKU von Application Gateway. Die neue SKU folgt dem gleichen Objektmodell wie die Standard-SKU.

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

Jetzt können Sie die Konfiguration der automatischen Skalierung für die Application Gateway-Instanz angeben. Zwei Konfigurationstypen werden für die automatische Skalierung unterstützt:

- **Modus mit fester Kapazität**: In diesem Modus kann die Application Gateway-Instanz keine automatische Skalierung vornehmen. Sie wird mit einer festen Kapazität für die Skalierungseinheit ausgeführt.

   ```azurepowershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```
- **Modus mit automatischer Skalierung**: In diesem Modus nimmt das Anwendungsgateway basierend auf dem Muster des Anwendungsdatenverkehrs eine automatische Skalierung vor.

   ```azurepowershell
   $autoscaleConfig = New-AzureRmApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Erstellen Sie ein Anwendungsgateway und integrieren Sie redundante Zonen. 

Die Zonenkonfiguration wird nur in Regionen unterstützt, in denen Azure-Zonen verfügbar sind. In Regionen, in denen Azure-Zonen nicht verfügbar sind, darf der Zonenparameter nicht verwendet werden. Ein Anwendungsgateway kann in einer einzelnen Zone, in zwei Zonen und sogar in drei Zonen bereitgestellt werden. „PublicIPAddress“ muss für ein Anwendungsgateway mit einer einzelnen Zone an die gleiche Zone gebunden sein. Für zwei oder drei zonenredundante Anwendungsgateways muss „PublicIPAddress“ ebenfalls zonenredundant sein. Das heißt, es wird keine Zone angegeben.

```azurepowershell
$appgw = New-AzureRmApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 `
  -ResourceGroupName $rg -Location $location -BackendAddressPools $pool `
  -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig `
  -FrontendIpConfigurations $fip -FrontendPorts $fp01, $fp02 `
  -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 `
  -Sku $sku -sslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Verwenden Sie [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress), um die öffentliche IP-Adresse des Anwendungsgateways abzurufen. Kopieren Sie die öffentliche IP-Adresse oder den DNS-Namen, und fügen Sie den kopierten Inhalt in die Adressleiste des Browsers ein.

`Get-AzureRmPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP`

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Untersuchen Sie zuerst die Ressourcen, die mit dem Anwendungsgateway erstellt wurden. Sie können dann den Befehl `Remove-AzureRmResourceGroup` verwenden, um die Ressourcengruppe, das Anwendungsgateway und alle dazugehörigen Ressourcen zu entfernen, wenn diese nicht mehr benötigt werden.

`Remove-AzureRmResourceGroup -Name $rg`

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Verwenden einer statischen virtuellen IP
> * Einrichten des Konfigurationsparameters für die automatische Skalierung
> * Verwenden des Zeitzonenparameters
> * Erstellen des Anwendungsgateways

> [!div class="nextstepaction"]
> [Erstellen eines Anwendungsgateways mit Routingregeln auf URL-Pfadbasis](./tutorial-url-route-powershell.md)

---
title: Erstellen eines Anwendungsgateways mit externer Umleitung – Azure PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Azure PowerShell ein Anwendungsgateway erstellen, das Webdatenverkehr an eine externe Website umleitet.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: victorh
ms.openlocfilehash: efa44b319fcfb68e0d309a7dc10288b4ea4bd784
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425841"
---
# <a name="create-an-application-gateway-with-external-redirection-using-azure-powershell"></a>Erstellen eines Anwendungsgateways mit externer Umleitung durch Azure PowerShell

Sie können mit Azure PowerShell [eine Umleitung des Webdatenverkehrs](application-gateway-multi-site-overview.md) konfigurieren, wenn Sie ein [Anwendungsgateway](application-gateway-introduction.md) erstellen. In diesem Tutorial konfigurieren Sie einen Listener und eine Regel, die den am Anwendungsgateway eingehenden Webdatenverkehr an eine externe Website umleitet.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Einrichten des Netzwerks
> * Erstellen eines Listeners und einer Umleitungsregel
> * Erstellen eines Anwendungsgateways

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 3.6 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Erstellen Sie mithilfe von [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe.  

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) die Subnetzkonfiguration *myAGSubnet*. Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) und der Subnetzkonfiguration das virtuelle Netzwerk *myVNet*. Erstellen Sie zum Schluss mithilfe von [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) die öffentliche IP-Adresse. Diese Ressourcen werden verwendet, um Netzwerkkonnektivität für das Anwendungsgateway und die zugehörigen Ressourcen bereitzustellen.

```azurepowershell-interactive
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

### <a name="create-the-ip-configurations-and-frontend-port"></a>Erstellen der IP-Konfigurationen und des Front-End-Ports

Ordnen Sie das zuvor erstellte Subnetz *myAGSubnet* mithilfe von [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) dem Anwendungsgateway zu. Weisen Sie dem Anwendungsgateway die öffentliche IP-Adresse mit [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) zu. Danach können Sie mithilfe von [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) den HTTP-Port erstellen.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzureRmApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool-and-settings"></a>Erstellen des Back-End-Pools und der Einstellungen

Erstellen Sie mit [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) den Back-End-Pool *defaultPool* für das Anwendungsgateway. Konfigurieren Sie mit [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) die Einstellungen für den Pool.

```azurepowershell-interactive
$defaultPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name defaultPool 
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-rule"></a>Erstellen des Listeners und der Regel

Ein Listener ist erforderlich, damit das Anwendungsgateway Datenverkehr in geeigneter Weise weiterleiten kann. Erstellen Sie einen Listener mit [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener), der zuvor erstellten Front-End-Konfiguration und dem zuvor erstellten Front-End-Port. Sie leiten Datenverkehr um, indem Sie eine Umleitungskonfiguration erstellen, die zu der von Ihnen zu erstellenden Routingregel hinzugefügt werden kann.

Für den Listener ist eine Routingregel erforderlich, damit bekannt ist, an welches Ziel eingehender Datenverkehr gesendet werden soll. Erstellen Sie mit [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) und der Umleitungskonfiguration eine grundlegende Regel namens *redirectRule*.

```azurepowershell-interactive
$defaultListener = New-AzureRmApplicationGatewayHttpListener `
  -Name defaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$redirectConfig = New-AzureRmApplicationGatewayRedirectConfiguration `
  -Name myredirect `
  -RedirectType Temporary `
  -TargetUrl "https://bing.com"
$redirectRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name redirectRule `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -RedirectConfiguration $redirectConfig
```

### <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Sie haben die erforderlichen unterstützenden Ressourcen erstellt. Geben Sie nun mit [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) Parameter für das Anwendungsgateway *myAppGateway* an, und erstellen Sie es mit [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway).

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
$appgw = New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $redirectRule `
  -RedirectConfigurations $redirectConfig `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Um die öffentliche IP-Adresse des Anwendungsgateways abzurufen, können Sie [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) verwenden. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

Daraufhin sollte *bing.com* in Ihrem Browser aufgerufen werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Einrichten des Netzwerks
> * Erstellen eines Listeners und einer Umleitungsregel
> * Erstellen eines Anwendungsgateways

> [!div class="nextstepaction"]
> [Erfahren Sie mehr darüber, was Sie mit dem Anwendungsgateway tun können.](./application-gateway-introduction.md)

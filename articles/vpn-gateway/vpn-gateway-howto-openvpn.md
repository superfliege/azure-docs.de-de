---
title: 'Konfigurieren von OpenVPN in Azure VPN Gateway: PowerShell | Microsoft-Dokumentation'
description: Schritte zum Konfigurieren von OpenVPN für Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: cherylmc
ms.openlocfilehash: 07dae60d1d4ab43194f88f44bde498d9fa19ce81
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55508537"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway-preview"></a>Konfigurieren von OpenVPN-Clients für Azure VPN Gateway (Point-to-Site) (Vorschau)

In diesem Artikel erfahren Sie, wie Sie OpenVPN für Azure VPN Gateway einrichten. In diesem Artikel wird davon ausgegangen, dass Sie in einer Point-to-Site-Umgebung arbeiten. Falls nicht, befolgen Sie die Anweisungen in Schritt 1, um ein Point-to-Site-VPN zu erstellen.

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register"></a>Registrieren dieses Features

Klicken Sie in diesen Schritten auf **Ausprobieren**, um dieses Feature mit Azure Cloud Shell auf einfache Weise zu registrieren.

>[!NOTE]
>Wenn Sie dieses Feature nicht registrieren, können Sie es nicht nutzen.
>

Nach dem Klicken auf **Ausprobieren** zum Öffnen von Azure Cloud Shell können Sie die folgenden Befehle kopieren und einfügen:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```
 
```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Registrieren Sie das Abonnement unter dem Microsoft.Network-Namespace neu, nachdem das Feature als registriert angezeigt wird.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Erstellen eines Point-to-Site-VPN

Wenn Sie noch keine funktionsfähige Point-to-Site-Umgebung haben, folgen Sie der Anweisung zu deren Erstellung. Unter [Erstellen eines Point-to-Site-VPN](vpn-gateway-howto-point-to-site-resource-manager-portal.md) erfahren Sie, wie Sie ein Point-to-Site-VPN-Gateway mit nativer Azure-Zertifikatauthentifizierung erstellen und konfigurieren. 

> [!IMPORTANT]
> Die Basic-SKU wird für OpenVPN nicht unterstützt.

## <a name="cmdlets"></a>2. Installieren von PowerShell-Cmdlets

Installieren Sie die aktuelle Version der PowerShell-Cmdlets für Resource Manager. Weitere Informationen zum Installieren von PowerShell-Cmdlets finden Sie unter [Overview of Azure PowerShell](/powershell/azure/overview) (Übersicht über Azure PowerShell). Dies ist wichtig, da frühere Versionen der Cmdlets nicht die aktuellen Werte enthalten, die Sie für diese Übung benötigen.

## <a name="enable"></a>3. Aktivieren von OpenVPN auf dem Gateway

Aktivieren Sie OpenVPN auf Ihrem Gateway. Vergewissern Sie sich, dass das Gateway bereits für Point-to-Site (IKEv2 oder SSTP) konfiguriert ist, bevor Sie die folgenden Befehle ausführen:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von OpenVPN finden Sie unter [Konfigurieren von OpenVPN-Clients](vpn-gateway-howto-openvpn-clients.md).

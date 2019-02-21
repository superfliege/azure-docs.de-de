---
title: 'Konfigurieren von OpenVPN in Azure VPN Gateway: PowerShell | Microsoft-Dokumentation'
description: Schritte zum Konfigurieren von OpenVPN für Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: cherylmc
ms.openlocfilehash: 50a8c30831ba806d0ea02d4f67b4e672e71e6325
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415084"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway-preview"></a>Konfigurieren von OpenVPN-Clients für Azure VPN Gateway (Point-to-Site) (Vorschau)

In diesem Artikel erfahren Sie, wie Sie OpenVPN für Azure VPN Gateway einrichten. In diesem Artikel wird davon ausgegangen, dass Sie in einer Point-to-Site-Umgebung arbeiten. Falls nicht, befolgen Sie die Anweisungen in Schritt 1, um ein Point-to-Site-VPN zu erstellen.

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="register"></a>Registrieren dieses Features

Klicken Sie in diesen Schritten auf **Ausprobieren**, um dieses Feature mit Azure Cloud Shell auf einfache Weise zu registrieren.

>[!NOTE]
>Wenn Sie dieses Feature nicht registrieren, können Sie es nicht nutzen.
>

Nach dem Klicken auf **Ausprobieren** zum Öffnen von Azure Cloud Shell können Sie die folgenden Befehle kopieren und einfügen:

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```
 
```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Registrieren Sie das Abonnement unter dem Microsoft.Network-Namespace neu, nachdem das Feature als registriert angezeigt wird.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
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
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von OpenVPN finden Sie unter [Konfigurieren von OpenVPN-Clients](vpn-gateway-howto-openvpn-clients.md).

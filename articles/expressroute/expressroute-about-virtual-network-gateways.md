---
title: Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke | Microsoft-Dokumentation
description: Informieren Sie sich über Gateways für virtuelle Netzwerke für ExpressRoute. Dieser Artikel enthält Informationen zu Gateway-SKUs und -Typen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: d9c607114d6c6c56c25303a88dcc11f4ab804eb4
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2019
ms.locfileid: "57404335"
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>Informationen zu Gateways für virtuelle Netzwerke für ExpressRoute
Ein Gateway für virtuelle Netzwerke dient zum Senden von Netzwerkdatenverkehr zwischen virtuellen Azure-Netzwerken und lokalen Standorten. Sie können ein Gateway für virtuelle Netzwerke entweder für ExpressRoute- oder VPN-Datenverkehr verwenden. Dieser Artikel konzentriert sich auf die Gateways virtueller ExpressRoute-Netzwerke und enthält Informationen zu SKUs, geschätzter Leistung nach SKU und Gatewaytypen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>Gatewaytypen

Beim Erstellen eines Gateways für virtuelle Netzwerke müssen mehrere Einstellungen angegeben werden. Eine dieser erforderlichen Einstellungen, „-GatewayType“, gibt an, ob das Gateway für ExpressRoute- oder für VPN-Datenverkehr verwendet wird. Die zwei Gatewaytypen sind:

* **Vpn** – Wenn verschlüsselter Netzwerkdatenverkehr über das öffentliche Internet gesendet wird, verwenden Sie den Gatewaytyp „Vpn“. Dies wird auch als VPN Gateway bezeichnet. Bei Site-to-Site-, Point-to-Site- und VNet-zu-VNet-Verbindungen wird jeweils VPN Gateway verwendet.

* **ExpressRoute** – Verwenden Sie den Gatewaytyp „ExpressRoute“, wenn Netzwerkdatenverkehr über eine private Verbindung gesendet wird. Dies wird auch als ExpressRoute-Gateway bezeichnet und ist der Gatewaytyp, der auch zum Konfigurieren von ExpressRoute verwendet wird.

Ein virtuelles Netzwerk kann pro Gatewaytyp immer nur über ein einzelnes virtuelles Netzwerkgateway verfügen. So können Sie beispielsweise ein virtuelles Netzwerkgateway mit „-GatewayType Vpn“ und ein virtuelles Netzwerkgateway mit „-GatewayType ExpressRoute“ verwenden.

## <a name="gwsku"></a>Gateway-SKUs
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Wenn Sie das Gateway auf eine leistungsfähigere Gateway-SKU aktualisieren möchten, können Sie in den meisten Fällen das PowerShell-Cmdlet „Resize-AzVirtualNetworkGateway“ verwenden. Mit diesem können Sie Upgrades von Standard- auf HighPerformance-SKUs durchführen. Für ein Upgrade auf die UltraPerformance-SKU müssen Sie jedoch das Gateway neu erstellen. Das Neuerstellen eines Gateways führt zu Ausfällen.

### <a name="aggthroughput"></a>Geschätzte Leistungen nach Gateway-SKU
In der folgenden Tabelle sind die Gatewaytypen und die geschätzten Leistungen angegeben. Diese Tabelle betrifft sowohl das Resource Manager-Bereitstellungsmodell als auch das klassische Bereitstellungsmodell.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Die Anwendungsleistung hängt von mehreren Faktoren ab, z.B. der End-to-End-Latenz und der Anzahl der Datenflüsse, die die Anwendung öffnet. Die Zahlen in der Tabelle stellen die Obergrenze dar, die die Anwendung theoretisch in einer idealen Umgebung erzielen kann.
>
>

### <a name="zrgw"></a>SKUs für zonenredundante Gateways

Sie können VPN- und ExpressRoute-Gateways auch in Azure-Verfügbarkeitszonen bereitstellen. Dadurch werden sie physisch und logisch in verschiedene Verfügbarkeitszonen unterteilt, wodurch Ihre lokale Netzwerkverbindung zu Azure vor Ausfällen auf Zonenebene geschützt wird.

![Zonenredundante ExpressRoute-Gateway](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Zonenredundante Gateways verwenden bestimmte neue Gateway-SKUs für ExpressRoute-Gateway.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

Die neuen Gateway-SKUs unterstützen zudem andere Bereitstellungsoptionen, die Ihre Anforderungen am besten zu erfüllen. Wenn Sie ein virtuelles Netzwerkgateway mit den neuen Gateway-SKUs erstellen, haben Sie auch die Möglichkeit, das Gateway in einer bestimmten Zone bereitzustellen. Dies wird als zonenbasiertes Gateway bezeichnet. Wenn Sie ein zonenbasiertes Gateway bereitstellen, werden alle Instanzen des Gateways in derselben Verfügbarkeitszone bereitgestellt.

## <a name="resources"></a>REST-APIs und PowerShell-Cmdlets
Zusätzliche technische Ressourcen und spezielle Syntaxanforderungen bei der Verwendung von REST-APIs und PowerShell-Cmdlets für Gatewaykonfigurationen für virtuelle Netzwerke finden Sie auf den folgenden Seiten:

| **Klassisch** | **Ressourcen-Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST-API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST-API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu verfügbaren Verbindungskonfigurationen finden Sie in der [ExpressRoute-Übersicht](expressroute-introduction.md) .

Weitere Informationen zum Erstellen von ExpressRoute-Gateways finden Sie unter [Erstellen eines Gateways für ein virtuelles Netzwerk für ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

Unter [Erstellen eines zonenredundanten Gateways für virtuelle Netzwerke](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md) finden Sie weitere Informationen zur Konfiguration zonenredundanter Gateways.

---
title: Migration von Netzwerkressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Netzwerkressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: c6842bcad076fa106886198b56a01c9e6cc8a182
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2018
ms.locfileid: "51975610"
---
# <a name="migration-of-network-resources-from-azure-germany-to-global-azure"></a>Migration von Netzwerkressourcen von Azure Deutschland zu Azure weltweit

Die meisten Netzwerkdienste unterstützen keine Migration von Azure Deutschland zu Azure weltweit. Allerdings können Sie Ihre Netzwerke in beiden Cloudumgebungen verbinden, indem ein Site-to-Site-VPN verwendet wird. Die Schritte ähneln dem Bereitstellen eines Site-to-Site-VPN zwischen Ihrem lokalen Netzwerk und Azure: Definieren Sie ein Gateway in beiden Clouds, und geben Sie an, wie die Kommunikation zwischen ihnen erfolgen soll. Es ist ein [Artikel zu Site-to-Site-VPNs](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) verfügbar, der die Schritte zum Bereitstellen eines Site-to-Site-VPN beschreibt:

1. Definieren eines virtuellen Netzwerks
2. Definieren des Adressraums
3. Definieren von Subnetzen
4. Definieren des Gatewaysubnetzes
5. Definieren des Gateways für VNET
6. Definieren des Gateways für das lokale Netzwerk (Ihr lokales VPN-Gerät)
7. Konfigurieren des lokalen VPN-Geräts
8. Herstellen der Verbindung

Um virtuelle Netzwerke zwischen Azure weltweit und Azure Deutschland zu verbinden, wiederholen Sie die Schritts 1 bis 5 für beide Umgebungen, und verbinden sie beide Netzwerke im letzten Schritt:

- Führen Sie die Schritte 1 bis 5 in Azure weltweit aus.
- Führen Sie die Schritte 1 bis 5 in Azure Deutschland aus.
- Führen Sie Schritt 6 in Azure weltweit aus.
  - Geben Sie die öffentliche IP-Adresse des VPN Gateways in Azure Deutschland ein.
- Führen Sie Schritt 6 in Azure Deutschland aus.
  - Geben Sie die öffentliche IP-Adresse des VPN Gateways in Azure weltweit ein.
- Lassen Sie Schritt 7 aus.
- Führen Sie Schritt 8 zum Fertigstellen aus.


## <a name="virtual-network-vnet"></a>Virtual Network (VNet)

Die Migration von VNETs zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Es wird empfohlen, neue virtuelle Netzwerke in der Zielregion zu erstellen und Ressourcen zu diesen VNETs zu migrieren.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Virtual Networks anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/virtual-network/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Weitere Informationen zu virtuellen Netzwerken](../virtual-network/virtual-networks-overview.md)
- [Planen virtueller Netzwerke](../virtual-network/virtual-network-vnet-plan-design-arm.md)








## <a name="network-security-groups-nsg"></a>Netzwerksicherheitsgruppen

Die Migration von Netzwerksicherheitsgruppen zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Der empfohlene Ansatz besteht darin, neue Netzwerksicherheitsgruppen in der Zielregion zu erstellen und die NSG-Regeln auf die neue Anwendungsumgebung anzuwenden.

Sie können die aktuelle Konfiguration einer beliebigen Netzwerksicherheitsgruppe über das Portal oder mit den folgenden PowerShell-Befehlen abrufen:

```powershell
$nsg=Get-AzureRmNetworkSecurityGroup -ResourceName <nsg-name> -ResourceGroupName <resourcegroupname>
Get-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg
```

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre [Kenntnisse zu Netzwerksicherheitsgruppen](../virtual-network/security-overview.md#network-security-groups) auf.

### <a name="references"></a>Referenzen

- [Informationen zu Netzwerksicherheitsgruppen](../virtual-network/security-overview.md)
- [Verwalten von Netzwerksicherheitsgruppen](../virtual-network/manage-network-security-group.md)







## <a name="expressroute"></a>ExpressRoute

Die Migration von ExpressRoute zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Die empfohlene Vorgehensweise besteht im Erstellen neuer ExpressRoute-Verbindungen und eines neuen ExpressRoute-Gateways.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu ExpressRoute anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/expressroute/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Erstellen eines neuen ExpressRoute-Gateways](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)
- [ExpressRoute-Standorte und -Dienstanbieter](../expressroute/expressroute-locations.md)
- [Weitere Informationen zum ExpressRoute-Gateway](../expressroute/expressroute-about-virtual-network-gateways.md)





## <a name="vpn-gateway"></a>VPN Gateway

Die Migration von VPN Gateways zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Es wird empfohlen, ein neues VPN Gateway zu erstellen und zu konfigurieren.

Erfassen Sie Informationen zu Ihrer aktuellen VPN Gatewaykonfiguration über das Portal oder mithilfe von PowerShell. Es gibt eine Reihe von Cmdlets, die mit `Get-AzureRmVirtualNetworkGateway*` beginnen.

Vergessen Sie nicht, Ihre lokale Konfiguration zu aktualisieren und vorhandene Regeln für die alten IP-Bereiche zu löschen, sobald Sie Ihre Azure-Netzwerkumgebung aktualisiert haben.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu VPN Gateways anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/vpn-gateway/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Erstellen einer Site-to-Site-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) mit dem VPN Gateway
- PowerShell-Cmdlets [Get-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/get-azurermvirtualnetworkgateway?view=azurermps-6.5.0)
- Blog: [Erstellen einer Site-to-Site-Verbindung](https://blogs.technet.microsoft.com/ralfwi/2017/02/02/connecting-clouds/) zwischen Azure Deutschland und Azure weltweit
 






## <a name="application-gateway"></a>Application Gateway

Die Migration von Application Gateways zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Es wird empfohlen, ein neues Gateway zu erstellen und zu konfigurieren.

Erfassen Sie Informationen zu Ihrer aktuellen Gatewaykonfiguration über das Portal oder mithilfe von PowerShell. Es gibt eine Reihe von Cmdlets, die mit `Get-AzureRmApplicationGateway*` beginnen.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Application Gateways anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/application-gateway/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Erstellen eines Application Gateways](../application-gateway/quick-create-portal.md)
- PowerShell-Cmdlets [Get-AzureRmApplicationGateway](/powershell/module/azurerm.network/get-azurermapplicationgateway?view=azurermps-6.5.0)














## <a name="azure-dns"></a>Azure DNS

Um Ihre DNS-Konfiguration von Azure Deutschland zu Azure weltweit zu migrieren, exportieren Sie die DNS-Zonendatei, und importieren Sie sie dann unter dem neuen Abonnement. Zurzeit ist das einzige Verfahren zum Exportieren der Zonendatei die Verwendung der Azure CLI.

Wenn Sie in Azure Deutschland bei Ihrem Quellabonnement angemeldet sind, konfigurieren Sie die CLI so, dass der Resource Manager-Modus verwendet wird. Exportieren Sie die Zone:

```azurecli
az network dns zone export -g <resource group> -n <zone name> -f <zone file name>
```

Beispiel:

```azurecli
az network dns zone export -g "myresourcegroup" -n "contoso.com" -f "contoso.com.txt"
```

Dieser Befehl ruft den Azure DNS-Dienst zum Exportieren der Zone „contoso.com“ in der Ressourcengruppe „myresourcegroup“ auf. Die Ausgabe wird als eine mit BIND kompatible Zonendatei in „contoso.com.txt“ im aktuellen Ordner gespeichert.

Nachdem der Export abgeschlossen wurde, löschen Sie die NS-Einträge aus der Zonendatei, weil NS-Einträge für die neue Region und das Abonnement neu erstellt werden.

Melden Sie sich nun in Ihrer Zielumgebung an, erstellen Sie eine neue Ressourcengruppe (oder wählen Sie eine vorhandene aus), und importieren Sie dann die zuvor erstellte Zonendatei:

```azurecli
az network dns zone import -g <resource group> -n <zone name> -f <zone file name>
```

Nachdem die Zone importiert wurde, müssen Sie die Zone überprüfen, indem Sie den folgenden Befehl ausführen:

```azurecli
az network dns record-set list -g <resource group> -z <zone name>
```

Nachdem die Überprüfung abgeschlossen wurde, wenden Sie sich an Ihre Domänenregistrierungsstelle, und delegieren Sie die NS-Einträge erneut. Rufen Sie Informationen zum NS-Eintrag ab, indem Sie den folgenden Befehl ausführen:

```azurecli
az network dns record-set ns list -g <resource group> -z --output json
```

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Application Gateways anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/dns/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Azure DNS – Übersicht](../dns/dns-overview.md)
- [Azure DNS-Import und -Export](../dns/dns-import-export.md)










## <a name="network-watcher"></a>Network Watcher

Die Migration von Network Watcher zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Es wird empfohlen, einen neuen Network Watcher zu erstellen und zu konfigurieren. Vergleichen Sie anschließend die Ergebnisse zwischen der alten und der neuen Umgebung:

- [NSG-Datenflussprotokolle](../network-watcher/network-watcher-nsg-flow-logging-portal.md)
- [Verbindungsmonitor](../network-watcher/connection-monitor.md)

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Network Watcher anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/network-watcher/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Network Watcher – Übersicht](../network-watcher/network-watcher-monitoring-overview.md)













## <a name="traffic-manager"></a>Traffic Manager

In Azure Deutschland erstellte Traffic Manager-Profile können nicht zu Azure weltweit migriert werden. Da Sie auch alle Traffic Manager-Endpunkte zur Zielumgebung migrieren, müssen Sie das Traffic Manager-Profil ohnehin aktualisieren.

Traffic Manager kann Sie bei einer reibungslosen Migration unterstützen. Wenn Traffic Manager noch in der alten Umgebung ausgeführt wird, können Sie bereits zusätzliche Endpunkte in der Zielumgebung definieren. Sobald Traffic Manager in der neuen Umgebung ausgeführt wird, können Sie weiterhin Endpunkte in der alten Umgebung definieren, die Sie bisher noch nicht migriert haben. Dies wird als [das blau-grüne Szenario](https://azure.microsoft.com/blog/blue-green-deployments-using-azure-traffic-manager/) bezeichnet. Kurz gesagt:

- Erstellen Sie einen neuen Traffic Manager in Azure weltweit.
- Definieren Sie die Endpunkte (immer noch in Azure Deutschland).
- Ändern Sie Ihren DNS CNAME in den neuen Traffic Manager.
- Deaktivieren Sie den alten Traffic Manager
- für jeden Endpunkt in Azure Deutschland:
  - Migrieren Sie den Endpunkt zu Azure weltweit.
  - Ändern Sie das Traffic Manager-Profil, um den neuen Endpunkt zu verwenden.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Traffic Manager anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/traffic-manager/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Traffic Manager – Übersicht](../traffic-manager/traffic-manager-overview.md)
- [Erstellen eines Traffic Manager-Profils](../traffic-manager/traffic-manager-create-profile.md)
- [Blau-grünes Szenario](https://azure.microsoft.com/blog/blue-green-deployments-using-azure-traffic-manager/)



















## <a name="load-balancer"></a>Load Balancer

Die Migration von Load Balancer zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Es wird empfohlen, einen neuen Load Balancer zu erstellen und zu konfigurieren.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Load Balancer anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Load Balancer: Übersicht](../load-balancer/load-balancer-overview.md)
- [Erstellen eines neuen Load Balancers](../load-balancer/quickstart-load-balancer-standard-public-portal.md)
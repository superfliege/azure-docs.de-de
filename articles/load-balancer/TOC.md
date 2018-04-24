# [Dokumentation zu Load Balancer](index.md)

# Übersicht
## [Was ist der Load Balancer?](load-balancer-overview.md)
## [Was ist Load Balancer Standard?](load-balancer-standard-overview.md)
## [Load Balancer-Tests](load-balancer-custom-probe-overview.md)
## [Hochverfügbarkeitsports](load-balancer-ha-ports-overview.md)
## [IPv6-Unterstützung](load-balancer-ipv6-overview.md)
## [Mehrere Front-Ends](load-balancer-multivip-overview.md)
## [Ausgehende Verbindungen](load-balancer-outbound-connections.md)
## [Load Balancer im Standard-Tarif und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md)
## [Metriken und Diagnosen für Load Balancer im Standard-Tarif](load-balancer-standard-diagnostics.md)

# Erste Schritte
## [Einrichten eines Load Balancers im Tarif „Basic“](quickstart-create-basic-load-balancer-portal.md)
### [Einrichten eines Load Balancers im Tarif „Basic“ (CLI)](quickstart-create-basic-load-balancer-cli.md)
### [Einrichten eines Load Balancers im Tarif „Basic“ (PowerShell)](quickstart-create-basic-load-balancer-powershell.md)
## [Einrichten eines Load Balancers im Tarif „Standard“](quickstart-load-balancer-standard-public-portal.md)
### [Einrichten eines Load Balancers im Tarif „Standard“ (CLI)](quickstart-load-balancer-standard-public-cli.md)

# Anleitung

## [Erstellen eines zonenredundanten öffentlichen Load Balancers im Standard-Tarif](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
### [Erstellen eines zonenredundanten öffentlichen Load Balancers im Standard-Tarif (PowerShell)](load-balancer-get-started-internet-az-powershell.md)
### [Erstellen eines zonenredundanten öffentlichen Load Balancers im Standard-Tarif (CLI)](load-balancer-get-started-internet-az-cli.md)
## [Erstellen eines zonalen öffentlichen Load Balancers im Standard-Tarif](load-balancer-get-started-internet-availability-zones-zonal-portal.md)
### [Erstellen eines zonalen öffentlichen Load Balancers im Standard-Tarif (PowerShell)](load-balancer-get-started-internet-availability-zones-zonal-powershell.md)
### [Erstellen eines zonalen öffentlichen Load Balancers im Standard-Tarif (CLI)](load-balancer-get-started-internet-availability-zones-zonal-cli.md)
## [Lastenausgleich für virtuelle Computer über Verfügbarkeitszonen hinweg](load-balancer-standard-public-availability-zones-portal.md)
###  [Lastenausgleich für virtuelle Computer über Verfügbarkeitszonen hinweg (CLI)](load-balancer-standard-public-zone-redundant-cli.md)
## [Lastenausgleich für virtuelle Computer innerhalb einer Zone](tutorial-load-balancer-standard-public-zonal-portal.md)  
###  [Lastenausgleich für virtuelle Computer innerhalb einer Zone (CLI)](load-balancer-standard-public-zonal-cli.md)   
## [Einrichten eines Load Balancers im Tarif „Basic“ (Vorlage)](load-balancer-get-started-internet-arm-template.md)
## [Erstellen des öffentlichen Lastenausgleichs mit IPv6](load-balancer-ipv6-internet-ps.md)
### [Erstellen des öffentlichen Lastenausgleichs mit IPv6 (CLI)](load-balancer-ipv6-internet-cli.md)
### [Erstellen des öffentlichen Lastenausgleichs mit IPv6 (Vorlage)](load-balancer-ipv6-internet-template.md)
## [Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-portal.md)
### [Konfigurieren des internen Lastenausgleichs (PowerShell)](load-balancer-get-started-ilb-arm-ps.md)
### [Konfigurieren des internen Lastenausgleichs (CLI)](load-balancer-get-started-ilb-arm-cli.md)
### [Konfigurieren des internen Lastenausgleichs (Vorlage)](load-balancer-get-started-ilb-arm-template.md)
## [Konfigurieren des TCP-Leerlauftimeouts für den Load Balancer](load-balancer-tcp-idle-timeout.md)
## [Konfigurieren des Verteilungsmodus für den Load Balancer](load-balancer-distribution-mode.md)
## [Kombinieren von Lastenausgleichsdiensten](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fazure%2fload-balancer%2ftoc.json)
## [Verwenden mehrerer IP-Konfigurationen](load-balancer-multiple-ip.md)
### [Verwenden mehrerer IP-Konfigurationen (CLI)](load-balancer-multiple-ip-cli.md)
### [Verwenden mehrerer IP-Konfigurationen (PowerShell)](load-balancer-multiple-ip-powershell.md)
## [Protokollanalysen für den Azure Load Balancer](load-balancer-monitor-log.md)
## [Konfigurieren von Hochverfügbarkeitsports für internen Lastenausgleich](load-balancer-configure-ha-ports.md)

## Problembehandlung
### [Beheben von Problemen mit Azure Load Balancer](load-balancer-troubleshoot.md)

## Artikel zum klassischen Bereitstellungsmodell
### [Ausgehende Verbindungen (klassisch)](load-balancer-outbound-connections-classic.md)
## [Konfigurieren mehrerer VIPs für Clouddienst](load-balancer-multivip.md)
### [Konfigurieren des internen Lastenausgleichs für Clouddienste](load-balancer-get-started-ilb-classic-cloud.md)
#### [Konfigurieren des internen Lastenausgleichs für Clouddienste (PowerShell)](load-balancer-get-started-ilb-classic-ps.md)
#### [Konfigurieren des internen Lastenausgleichs für Clouddienste (CLI)](load-balancer-get-started-ilb-classic-cli.md)
### [Konfigurieren des öffentlichen Lastenausgleichs (klassische PowerShell)](load-balancer-get-started-internet-classic-ps.md)
#### [Konfigurieren des öffentlichen Lastenausgleichs (klassische Cloud)](load-balancer-get-started-internet-classic-cloud.md)
#### [Konfigurieren des öffentlichen Lastenausgleichs (klassische CLI)](load-balancer-get-started-internet-classic-cli.md)

# Verweis
## [Codebeispiele](https://azure.microsoft.com/en-us/resources/samples/?service=load-balancer)
## [Azure PowerShell](/powershell/module/azurerm.network)
## [Azure-Befehlszeilenschnittstelle](/cli/azure/network/lb)
## [.NET](/dotnet/api/microsoft.azure.management.network.models)
## [Java](/java/api/com.microsoft.azure.management.network)
## [Node.js](http://azure.github.io/azure-sdk-for-node/azure-arm-network/latest/LoadBalancers.html)
## [Ruby](http://www.rubydoc.info/gems/azure_mgmt_network/Azure/ARM/Network/LoadBalancers)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.mgmt.network.operations.html#azure.mgmt.network.operations.LoadBalancersOperations)
## [REST](https://msdn.microsoft.com/library/azure/mt163651.aspx)

# Verwandte Themen
## [Application Gateway](/azure/application-gateway/)
## [ExpressRoute](/azure/expressroute/)
## [Virtual Network](/azure/virtual-network/)
## [VPN Gateway](/azure/vpn-gateway/)
## [Virtueller Computer](/azure/virtual-machines/)
## [Traffic Manager](/azure/traffic-manager/)
## [DNS](/azure/dns/)

# angeben
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=networking)
## [Preise](https://azure.microsoft.com/pricing/details/load-balancer/)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=load-balancer)

# Übersicht
## [Virtuelle Netzwerke](virtual-networks-overview.md)
## [Routing](virtual-networks-udr-overview.md)
## [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md)
## [Virtual network service endpoints](virtual-network-service-endpoints-overview.md) (Virtuelles Netzwerk – Dienstendpunkte)
## [Virtual network for Azure services](virtual-network-for-azure-services.md) (Dienste für virtuelle Netzwerke in Azure)
## [Sicherheit](security-overview.md)
## [Geschäftskontinuität](virtual-network-disaster-recovery-guidance.md)
## [IP-Adressierung](virtual-network-ip-addresses-overview-arm.md)
## [DDoS-Schutz](ddos-protection-overview.md)
## [Häufig gestellte Fragen](virtual-networks-faq.md)
## Klassisch
### [IP-Adressierung](virtual-network-ip-addresses-overview-classic.md)
### [Zugriffssteuerungslisten](virtual-networks-acl.md)

# Erste Schritte
## [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md)

# Anleitung
## Planen und Entwerfen
### [Virtuelle Netzwerke](virtual-network-vnet-plan-design-arm.md)
### [Netzwerksicherheitsgruppen](virtual-networks-nsg.md)

## Bereitstellen
### [Virtuelle Netzwerke](virtual-networks-create-vnet-arm-pportal.md)
#### [Azure PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md)
#### [Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md)
#### [Vorlage](virtual-networks-create-vnet-arm-template-click.md)

### Netzwerksicherheitsgruppen
#### [Azure-Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [Azure PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md)
#### [Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md)
#### [Vorlage](virtual-networks-create-nsg-arm-template.md)
#### [Application security groups](create-network-security-group-preview.md) (Anwendungssicherheitsgruppen)
#### Klassisch
##### [Azure PowerShell](virtual-networks-create-nsg-classic-ps.md)
##### [Azure CLI 1.0](virtual-networks-create-nsg-classic-cli.md)

### Benutzerdefinierte Routen
#### [Azure-Portal](create-user-defined-route-portal.md)
#### [Azure PowerShell](virtual-network-create-udr-arm-ps.md)
#### [Azure CLI 2.0](virtual-network-create-udr-arm-cli.md)
#### [Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md)
#### [Vorlage](virtual-network-create-udr-arm-template.md)
#### Klassisch
##### [Azure PowerShell](virtual-network-create-udr-classic-ps.md)
##### [Azure-Befehlszeilenschnittstelle](virtual-network-create-udr-classic-cli.md)

### Peering in virtuellen Netzwerken
#### [Gleiches Bereitstellungsmodell – gleiches Abonnement](virtual-network-create-peering.md)
#### [Gleiches Bereitstellungsmodell – andere Abonnements](create-peering-different-subscriptions.md)
#### [Andere Bereitstellungsmodelle – gleiches Abonnement](create-peering-different-deployment-models.md)
#### [Andere Bereitstellungsmodelle – andere Abonnements](create-peering-different-deployment-models-subscriptions.md)

### [Virtual network service endpoints](virtual-network-service-endpoints-configure.md) (Virtuelles Netzwerk – Dienstendpunkte)

### Öffentliche IP-Adresse – Verfügbarkeitszone
#### [Azure-Portal](create-public-ip-availability-zone-portal.md)
#### [Azure-Befehlszeilenschnittstelle](create-public-ip-availability-zone-cli.md)
#### [PowerShell](create-public-ip-availability-zone-powershell.md)

### Virtuelle Computer
#### [Virtual machine network throughput (VM-Netzwerkdurchsatz)](virtual-machine-network-throughput.md)
#### Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse
##### [Azure-Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [Azure PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md)
##### [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md)
##### [Vorlage](virtual-network-deploy-static-pip-arm-template.md)
##### Klassisch
###### [Azure PowerShell](virtual-networks-reserved-public-ip.md)

#### Erstellen eines virtuellen Computers mit einer statischen privaten IP-Adresse
##### [Azure-Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [Azure PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [Azure-Befehlszeilenschnittstelle](virtual-networks-static-private-ip-arm-cli.md)
##### Klassisch
###### [Azure-Portal](virtual-networks-static-private-ip-classic-pportal.md)
###### [Azure PowerShell](virtual-networks-static-private-ip-classic-ps.md)
###### [Azure-Befehlszeilenschnittstelle](virtual-networks-static-private-ip-classic-cli.md)

#### Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellen
##### [Azure PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure CLI 2.0](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure CLI 1.0](../virtual-machines/linux/multiple-nics-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Vorlage](virtual-network-deploy-multinic-arm-template.md)

##### Klassisch
###### [Azure PowerShell](virtual-network-deploy-multinic-classic-ps.md)
###### [Azure-Befehlszeilenschnittstelle](virtual-network-deploy-multinic-classic-cli.md)

#### Erstellen eines virtuellen Computers mit mehreren IP-Adressen
##### [Azure-Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [Azure PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md)
##### [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md)
##### [Vorlage](virtual-network-multiple-ip-addresses-template.md)

#### [Erstellen eines virtuellen Computers mit beschleunigten Netzwerken](virtual-network-create-vm-accelerated-networking.md)

### Konnektivitätsszenarien
#### [Virtuelles Netzwerk (VNET) zu VNET](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNET (Resource Manager) zu VNET (klassisch)](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNET zu lokalem Netzwerk (VPN)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNET zu lokalem Netzwerk (ExpressRoute)](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Hoch verfügbare Hybrid-Netzwerkarchitektur](../guidance/guidance-hybrid-network-expressroute-vpn-failover.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

### Sicherheitsszenarien
#### [Schützen von Netzwerken mit virtuellen Geräten](virtual-network-scenario-udr-gw-nva.md)
#### [DMZ zwischen Azure und dem Internet](../guidance/guidance-iaas-ra-secure-vnet-dmz.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Clouddienst und Netzwerksicherheit](../best-practices-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Erstellen einer DMZ mit Netzwerksicherheitsgruppen](virtual-networks-dmz-nsg.md)
##### [Erstellen einer DMZ mit Netzwerksicherheitsgruppen (klassisch)](virtual-networks-dmz-nsg-asm.md)
##### [Erstellen einer DMZ mit Firewall und Netzwerksicherheitsgruppen (klassisch)](virtual-networks-dmz-nsg-fw-asm.md)
##### [DMZ mit Firewall, UDR und NSGs (klassisch)](virtual-networks-dmz-nsg-fw-udr-asm.md)

##### [Beispielanwendung](virtual-networks-sample-app.md)

### Klassisch
#### [Virtuelles Netzwerk](create-virtual-network-classic.md)
##### [Azure-Portal](virtual-networks-create-vnet-classic-pportal.md)
##### [Azure PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)
##### [Azure-Befehlszeilenschnittstelle](virtual-networks-create-vnet-classic-cli.md)
#### [Angeben von DNS-Einstellungen in der Konfigurationsdatei eines virtuellen Netzwerks](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
#### [Angeben von DNS-Einstellungen in einer Dienstkonfigurationsdatei](virtual-networks-specifying-dns-settings-in-a-service-configuration-file.md)

## Konfigurieren
### Virtuelle Computer
#### [Hinzufügen oder Entfernen von Netzwerkschnittstellen](virtual-network-network-interface-vm.md)
#### [Namensauflösung für virtuelle Computer und Clouddienste](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Registrieren von Hostnamen mithilfe von dynamischem DNS auf Ihrem eigenen DNS-Server](virtual-networks-name-resolution-ddns.md)
#### [Optimieren des Netzwerkdurchsatzes](virtual-network-optimize-network-bandwidth.md)
#### [Anzeigen und Ändern von Hostnamen](virtual-networks-viewing-and-modifying-hostnames.md)
#### Klassisch
##### Statische IP-Adressen
###### [PowerShell](virtual-networks-reserved-private-ip.md)
###### [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-static-private-ip-cli-nodejs.md)
##### [Öffentliche IP-Adressen auf Instanzebene](virtual-networks-instance-level-public-ip.md)

### Klassisch
#### Zugriffssteuerungslisten
##### [Azure-Portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure PowerShell](virtual-networks-acl-powershell.md)

## Verwalten
### [Virtuelle Netzwerke](virtual-network-manage-network.md)
#### [Subnetze](virtual-network-manage-subnet.md)
#### [Peerings](virtual-network-manage-peering.md)
#### Klassisch
##### [Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md)
##### [Migrieren von einer Affinitätsgruppe in eine Region](virtual-networks-migrate-to-regional-vnet.md)
### Netzwerksicherheitsgruppen
#### [Azure-Portal](virtual-network-manage-nsg-arm-portal.md)
#### [Azure PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md)
#### [Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md)

#### [Protokolle](virtual-network-nsg-manage-log.md)
### Netzwerkschnittstellen (NICs)
#### [Erstellen, Ändern oder Löschen von NICs](virtual-network-network-interface.md)
#### [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md)
### Virtuelle Computer
#### [Verschieben einer VM in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md)
### [Öffentliche IP-Adressen](virtual-network-public-ip-address.md)
### DDoS-Schutz
#### [Azure-Portal](ddos-protection-manage-portal.md)
#### [Azure PowerShell](ddos-protection-manage-ps.md)

## Problembehandlung
### Netzwerksicherheitsgruppen
#### [Azure-Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [Azure PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Routen
#### [Azure-Portal](virtual-network-routes-troubleshoot-portal.md)
#### [Azure PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Durchsatztests](virtual-network-bandwidth-testing.md)
### [Löschen virtueller Netzwerke nicht möglich](virtual-network-troubleshoot-cannot-delete-vnet.md)
### [Konnektivitätsprobleme zwischen virtuellen Computern](virtual-network-troubleshoot-connectivity-problem-between-vms.md)

# Referenz
## [Codebeispiele](https://azure.microsoft.com/en-us/resources/samples/?service=virtual-network)
## [Azure PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [Azure PowerShell (Classic)](/powershell/module/azure/) (Azure PowerShell [klassisch])
## [Azure-Befehlszeilenschnittstelle](/cli/azure/network)
## [Java](/java/api/)
## [REST (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [REST (klassisch)](https://msdn.microsoft.com/library/jj157182.aspx)


# Verwandte Themen
## [Virtuelle Computer](/azure/virtual-machines/)
## [Application Gateway](/azure/application-gateway/)
## [Azure DNS](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Load Balancer](/azure/load-balancer/)
## [VPN Gateway](/azure/vpn-gateway/)
## [ExpressRoute](/azure/expressroute/)

# Ressourcen
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=networking)
## [Netzwerkblog](http://azure.microsoft.com/blog/topics/networking)
## [Netzwerkforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Preise](https://azure.microsoft.com/pricing/details/virtual-network)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Netzwerkressourcenanbieter](resource-groups-networking.md)

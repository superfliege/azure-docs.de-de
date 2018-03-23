---
title: Azure Virtual Network – häufig gestellte Fragen | Microsoft-Dokumentation
description: Enthält Antworten auf häufig gestellte Fragen zu virtuellen Microsoft Azure-Netzwerken.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.openlocfilehash: a5b4bac9e0d8bc10defaff251557129a70d8a022
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Azure Virtual Network – häufig gestellte Fragen

## <a name="virtual-network-basics"></a>Grundlagen zu Virtual Networks

### <a name="what-is-an-azure-virtual-network-vnet"></a>Was ist ein Azure Virtual Network (VNet)?
Ein Azure Virtual Network (VNet) ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Es ist eine logische Isolierung von der Azure-Cloud für Ihr Abonnement. Mit VNets können Sie virtuelle private Netzwerke (VPNs) in Azure bereitstellen und verwalten und die VNets optional mit anderen VNets in Azure oder mit Ihrer lokalen IT-Infrastruktur verbinden, um hybride oder standortübergreifende Lösungen zu erstellen. Jedes erstellte VNet verfügt über einen eigenen CIDR-Block und kann mit anderen VNets und lokalen Netzwerken verbunden werden, solange sich die CIDR-Blöcke nicht überlappen. Sie können zudem die DNS-Servereinstellungen für VNets steuern und das VNet in Subnetze segmentieren.

Verwenden Sie VNets für Folgendes:

* Erstellen eines dedizierten VNet, das auf die Cloud beschränkt ist: Manchmal benötigen Sie keine standortübergreifende Konfiguration für Ihre Lösung. Wenn Sie ein VNet erstellen, können die Dienste und virtuellen Computer des VNet direkt und sicher in der Cloud miteinander kommunizieren. Sie können trotzdem in Ihrer Lösung Endpunktverbindungen für die virtuellen Computer und Dienste konfigurieren, die eine Internetkommunikation erfordern.
* Sicheres Erweitern des Rechenzentrums: Mit VNets können Sie herkömmliche Standort-zu-Standort-VPNs (S2S) erstellen, um die Kapazität des Rechenzentrums sicher zu skalieren. S2S-VPNs verwenden IPsec, um eine sichere Verbindung zwischen dem unternehmenseigenen VPN-Gateway und Azure bereitzustellen.
* Unterstützung von Hybrid Cloud-Szenarios: VNets bieten flexible Möglichkeiten, verschiedene Hybrid Cloud-Szenarios zu unterstützen. Sie können cloudbasierte Anwendungen auf sichere Weise mit beliebigen lokalen Systemen wie z. B. Mainframes oder Unix-Systemen verbinden.

### <a name="how-do-i-get-started"></a>Wie fange ich an?
Auf der Webseite [Dokumentation zu virtuellen Netzwerken](https://docs.microsoft.com/azure/virtual-network/) finden Sie Informationen zu den ersten Schritten. Hierbei handelt es sich um Übersichts- und Bereitstellungsinformationen für alle VNET-Features.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Können VNets ohne standortübergreifende Konnektivität verwendet werden?
Ja. Sie können ein VNET verwenden, ohne es mit Ihrer lokalen Umgebung verbinden zu müssen. Sie können beispielsweise Microsoft Windows Server-Active Directory-Domänencontroller und SharePoint-Farmen nur in einem Azure-VNET ausführen.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Kann ich eine WAN-Optimierung zwischen VNets oder einem VNet und meinem lokalen Rechenzentrum durchführen?

Ja. Sie können ein [virtuelles Netzwerkgerät für die WAN-Optimierung](https://azure.microsoft.com/marketplace/?term=wan+optimization) von mehreren Anbietern über den Azure Marketplace bereitstellen.

## <a name="configuration"></a>Konfiguration

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Welche Tools werden zum Erstellen eines VNet verwendet?
Sie können die folgenden Tools zum Erstellen oder Konfigurieren eines VNet verwenden:

* Azure-Portal
* PowerShell
* Azure-Befehlszeilenschnittstelle
* Eine Netzwerkkonfigurationsdatei (NETCFG-Datei, nur für klassische VNets). Weitere Informationen finden Sie im Artikel [Konfigurieren eines Virtual Network mit einer Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Welche Adressbereiche kann ich in meinen VNets verwenden?
Alle in [RFC 1918](http://tools.ietf.org/html/rfc1918) definierten IP-Adressbereiche. Beispiel: 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Können öffentliche IP-Adressen in VNets verwendet werden?
Ja. Weitere Informationen zu öffentlichen IP-Adressbereichen finden Sie unter [Create, change, or delete a virtual network](manage-virtual-network.md#create-a-virtual-network) (Erstellen, Ändern oder Löschen eines virtuellen Netzwerks). Auf öffentliche IP-Adressen kann nicht direkt über das Internet zugegriffen werden.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>Ist die Anzahl von Subnetzen im VNet begrenzt?
Ja. Ausführliche Informationen finden Sie im Artikel zu den [Einschränkungen für Azure-Abonnements](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Die Adressräume von Subnetzen können sich nicht überlappen.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Unterliegen die in den Subnetzen verwendeten IP-Adressen bestimmten Beschränkungen?
Ja. Einige IP-Adressen innerhalb jedes Subnetzes sind in Azure reserviert. Die erste und letzte IP-Adresse der Subnetze sind aus Gründen der Protokollkonformität reserviert. Darüber hinaus sind die Adressen x.x.x.1 bis x.x.x.3 der Subnetze reserviert, die für Azure-Dienste verwendet werden.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Wie ist die minimale und maximale Größe von VNets und Subnetzen?
Das kleinste unterstützte Subnetz weist die Netzmaske /29 und das größte die Netzmaske /8 (gemäß CIDR-Subnetzdefinitionen) auf.

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Können VLANs mithilfe von VNets in Azure integriert werden?
Nein. VNets sind Layer-3-Overlays. Layer-2-Semantik wird in Azure nicht unterstützt.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Können benutzerdefinierte Routingrichtlinien für VNets und Subnetze angegeben werden?
Ja. Sie können eine Routingtabelle erstellen und einem Subnetz zuordnen. Weitere Informationen zum Routing in Azure finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md#custom-routes).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Unterstützen VNets Multicasting oder Broadcasting?
Nein. Multicast und Broadcast werden nicht unterstützt.

### <a name="what-protocols-can-i-use-within-vnets"></a>Welche Protokolle werden innerhalb von VNets unterstützt?
Sie können die Protokolle TCP, UDP und ICMP TCP/IP in VNets verwenden. Unicast wird innerhalb von VNETs unterstützt, mit Ausnahme von DHCP (Dynamic Host Configuration Protocol) per Unicast (Quellport UDP/68, Zielport UDP/67). Verkapselte Multicast-, Broadcast- und IP-in-IP-Pakete sowie GRE-Pakete (Generic Routing Encapsulation) werden blockiert. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kann ich meine Standardrouter innerhalb eines VNet mit "ping" abfragen?
Nein.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Können Verbindungsdiagnosen mit "tracert" durchgeführt werden?
Nein.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Können Subnetze hinzugefügt werden, nachdem das VNet erstellt wurde?
Ja. Subnetze können jederzeit zu VNETs hinzugefügt werden, sofern der Subnetzadressbereich nicht Teil eines anderen Subnetzes und ausreichend Platz im Adressbereich des virtuellen Netzwerks verfügbar ist.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Kann die Größe des Subnetzes nach dessen Erstellung geändert werden?
Ja. Sie können ein Subnetz hinzufügen, entfernen, erweitern oder verkleinern, wenn darin keine VMs oder Dienste bereitgestellt werden.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Können Subnetze nach dem Erstellen geändert werden?
Ja. Sie können die in einem VNet verwendeten CIDR-Blöcke hinzufügen, entfernen und ändern.

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>Kann eine Verbindung mit dem Internet hergestellt werden, wenn Dienste in einem VNET ausgeführt werden?
Ja. Alle Dienste, die in einem VNET bereitgestellt werden, können eine ausgehende Verbindung mit dem Internet herstellen. Weitere Informationen zu ausgehenden Internetverbindungen in Azure finden Sie unter [Ausgehende Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Wenn Sie mit einer Ressource eine Verbindung in eingehender Richtung herstellen möchten, die über Resource Manager bereitgestellt wurde, muss der Ressource eine öffentliche IP-Adresse zugewiesen sein. Weitere Informationen zu öffentlichen IP-Adressen finden Sie unter [Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse](virtual-network-public-ip-address.md). Jeder in Azure bereitgestellte Azure-Clouddienst verfügt über eine öffentlich adressierbare, zugewiesene VIP-Adresse. Sie müssen Eingabeendpunkte für PaaS-Rollen und Endpunkte für virtuelle Computer definieren, damit diese Dienste Verbindungen über das Internet annehmen können.

### <a name="do-vnets-support-ipv6"></a>Unterstützen VNets IPv6?
Nein. Zum gegenwärtigen Zeitpunkt können Sie IPv6 mit VNets nicht verwenden. Sie können Azure-Lastenausgleichsmodulen jedoch IPv6-Adressen zuweisen, um einen Lastausgleich für virtuelle Computer vorzunehmen. Ausführliche Informationen finden Sie unter [Übersicht über IPv6 für Azure Load Balancer](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="can-a-vnet-span-regions"></a>Kann sich ein VNet über mehrere Regionen erstrecken?
Nein. Ein VNet ist auf eine Region beschränkt. Ein virtuelles Netzwerk erstreckt sich jedoch über Verfügbarkeitszonen. Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Sie können mithilfe von Peering in virtuellen Netzwerken Verbindungen zwischen virtuellen Netzwerken in verschiedenen Regionen herstellen. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md).

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Kann ein VNet mit einem anderen VNet in Azure verbunden werden?
Ja. Sie können zwei VNETs miteinander verbinden, indem Sie Folgendes verwenden:
- **Peering in virtuellen Netzwerken**: Ausführliche Informationen finden Sie unter [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md).
- **Eine Azure VPN Gateway-Instanz**: Ausführliche Informationen finden Sie unter [Konfigurieren einer VNET-zu-VNET-VPN-Gatewayverbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

## <a name="name-resolution-dns"></a>Namensauflösung (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Welche DNS-Optionen sind für VNets verfügbar?
Eine Übersicht über die verfügbaren DNS-Optionen finden Sie in der Entscheidungstabelle auf der Seite [Namensauflösung für virtuelle Computer und Rolleninstanzen](virtual-networks-name-resolution-for-vms-and-role-instances.md) .

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Können DNS-Server für ein VNet angegeben werden?
Ja. Sie haben die Möglichkeit, IP-Adressen von DNS-Servern in den Einstellungen des VNet anzugeben. Die Einstellung gilt als DNS-Standardserver für alle virtuellen Computer im VNET.

### <a name="how-many-dns-servers-can-i-specify"></a>Wie viele DNS-Server können angegeben werden?
Siehe [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits)

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Können DNS-Server geändert werden, nachdem das Netzwerk erstellt wurde?
Ja. Sie können die Liste der DNS-Server für das VNet jederzeit ändern. Wenn Sie die Liste der DNS-Server ändern, müssen Sie jeden virtuellen Computer im VNet neu starten, damit der neue DNS-Server übernommen wird.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Was ist der von Azure bereitgestellte DNS-Dienst, und wie wird er bei VNets verwendet?
Der von Azure bereitgestellte DNS-Dienst ist ein von Microsoft angebotener mehrinstanzenfähiger DNS-Dienst. In Azure werden Ihre gesamten VMs und Clouddienst-Rolleninstanzen dieses Diensts registriert. Dieser Dienst stellt die Namensauflösung nach dem Hostnamen für virtuelle Computer und Rolleninstanzen, die im gleichen Clouddienst enthalten sind, und nach dem FQDN für virtuelle Computer und Rolleninstanzen im gleichen VNet zur Verfügung. Weitere Informationen zum DNS finden Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](virtual-networks-name-resolution-for-vms-and-role-instances.md).

Die mandantenübergreifende Namensauflösung mithilfe des von Azure bereitgestellten DNS-Diensts ist auf die ersten 100 Clouddienste in einem VNET beschränkt. Diese Einschränkung gilt nicht, wenn Sie einen eigenen DNS-Server verwenden.

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>Können DNS-Einstellungen für einzelne virtuelle Computer oder Clouddienste überschrieben werden?
Ja. Sie können DNS-Server pro VM oder Clouddienst festlegen, um die standardmäßigen Netzwerkeinstellungen zu überschreiben. Es wird jedoch empfohlen, nach Möglichkeit den netzwerkweiten DNS-Server zu verwenden.

### <a name="can-i-bring-my-own-dns-suffix"></a>Können benutzerdefinierte DNS-Suffixe angegeben werden?
Nein. Sie können kein benutzerdefiniertes DNS-Suffix für die VNets angeben.

## <a name="connecting-virtual-machines"></a>Verbinden von virtuellen Computern

### <a name="can-i-deploy-vms-to-a-vnet"></a>Können virtuelle Computer in einem VNet bereitgestellt werden?
Ja. Alle Netzwerkschnittstellen (NICs) einer VM, die mit dem Resource Manager-Bereitstellungsmodell bereitgestellt wurden, müssen mit einem VNet verbunden sein. VMs, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden, können optional mit einem VNet verbunden werden.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>Wie lauten die unterschiedlichen Typen von IP-Adressen, die ich VMs zuweisen kann?
* **Privat:** Wird allen NICs jeder einzelnen VM zugewiesen. Die Adresse wird entweder mit der statischen oder dynamischen Methode zugewiesen. Private IP-Adressen werden aus dem Bereich zugewiesen, den Sie in den Subnetzeinstellungen Ihres VNet angegeben haben. Mit dem klassischen Bereitstellungsmodell bereitgestellten Ressourcen werden private IP-Adressen zugewiesen, selbst wenn keine Verbindung mit einem VNet besteht. Das Verhalten der Zuordnungsmethode unterscheidet sich abhängig davon, ob eine Ressource mit dem Resource Manager-Bereitstellungsmodell oder mit dem klassischen Bereitstellungsmodell bereitgestellt wurde: 

  - **Resource Manager:** Eine mit der dynamischen oder statischen Methode zugewiesene private IP-Adresse bleibt einem virtuellen Computer (Resource Manager) zugewiesen, bis die Ressource gelöscht wird. Der Unterschied besteht darin, dass bei Verwendung der statischen Methode Sie die zuzuweisende Adresse auswählen und bei Verwendung der dynamischen Methode Azure die Adresse auswählt. 
  - **Klassisch:** Eine mit der dynamischen Methode zugewiesene private IP-Adresse ändert sich möglicherweise, wenn ein virtueller Computer (klassisch) neu gestartet wird, nachdem er im Zustand „Beendet“ (Zuordnung aufgehoben) war. Wenn Sie sicherstellen müssen, dass sich die private IP-Adresse einer Ressource, die über das klassische Bereitstellungsmodell bereitgestellt wurde, nie ändert, weisen Sie eine private IP-Adresse mit der statischen Methode zu.

* **Öffentlich:** Kann optional NICs von VMs zugewiesen werden, die über das Azure Resource Manager-Bereitstellungsmodell bereitgestellt wurden. Die Adresse kann mit der statischen oder der dynamischen Zuteilungsmethode zugewiesen werden. Alle VMs und Cloud Services-Rolleninstanzen, die über das klassische Bereitstellungsmodell bereitgestellt werden, sind in einem Clouddienst vorhanden, dem eine *dynamische* öffentliche virtuelle IP-Adresse (VIP) zugewiesen ist. Eine öffentliche *statische* IP-Adresse, die als [reservierte IP-Adresse](virtual-networks-reserved-public-ip.md) bezeichnet wird, kann optional als VIP zugewiesen werden. Sie können öffentliche IP-Adressen einzelnen VMs oder Cloud Services-Rolleninstanzen zuweisen, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden. Diese Adressen werden als [ILPIP-Adressen](virtual-networks-instance-level-public-ip.md) (Instance Level Public IP, öffentliche IP auf Instanzebene) bezeichnet und können dynamisch zugewiesen werden.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kann ich eine private IP-Adresse für einen virtuellen Computer reservieren, den ich zu einem späteren Zeitpunkt erstelle?
Nein. Eine private IP-Adresse kann nicht reserviert werden. Wenn eine private IP-Adresse verfügbar ist, wird sie einem virtuellen Computer oder einer Rolleninstanz durch den DHCP-Server zugewiesen. Der virtuelle Computer kann der Computer sein, dem Sie die interne IP-Adresse zuweisen möchten. Es kann aber auch ein anderer Computer sein. Sie können aber die private IP-Adresse eines bereits erstellten virtuellen Computers in eine verfügbare private IP-Adresse ändern.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Ändern sich private IP-Adressen für virtuelle Computer in einem VNet?
Das ist unterschiedlich. Wenn der virtuelle Computer über Resource Manager bereitgestellt wurde, ändern sie sich nicht, unabhängig davon, ob die IP-Adresse mit der statischen oder der dynamischen Zuordnungsmethode zugewiesen wurde. Wenn der virtuelle Computer über das klassische Bereitstellungsmodell bereitgestellt wurde, können sich dynamische IP-Adressen ändern, wenn ein virtueller Computer gestartet wird, nachdem er im beendeten Zustand (Zuordnung aufgehoben) war. Die Adresse eines virtuellen Computers, der über eins der beiden Bereitstellungsmodelle bereitgestellt wurde, wird freigegeben, wenn der virtuelle Computer gelöscht wird.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>Kann ich IP-Adressen den NICs im VM-Betriebssystem manuell zuordnen?
Ja, aber dies wird nur empfohlen, wenn es unbedingt notwendig ist, etwa beim Zuweisen mehrerer IP-Adressen zu einem virtuellen Computer. Ausführliche Informationen finden Sie unter [Zuweisen von mehreren IP-Adressen zu virtuellen Computern mithilfe des Azure-Portals](virtual-network-multiple-ip-addresses-portal.md#os-config). Falls sich die IP-Adresse ändert, die einer an einen virtuellen Computer angefügten Azure-NIC zugewiesen ist, und die IP-Adresse im VM-Betriebssystem sich davon unterscheidet, geht die Konnektivität zum virtuellen Computer verloren.

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>Was passiert mit meinen IP-Adressen, wenn ich einen Clouddienst-Bereitstellungsslot beende oder einen virtuellen Computer über das Betriebssystem herunterfahre?
Nichts. Die IP-Adressen (öffentliche VIP, öffentlich und privat) bleiben dem Clouddienst-Bereitstellungsslot bzw. der VM zugewiesen.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>Können virtuelle Computer ohne erneute Bereitstellung zwischen Subnetzen in einem VNET verschoben werden?
Ja. Weitere Informationen finden Sie im Artikel [Verschieben eines virtuellen Computers oder einer Rolleninstanz in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kann eine statische MAC-Adresse für einen virtuellen Computer konfiguriert werden?
Nein. Eine MAC-Adresse kann nicht statisch konfiguriert werden.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>Bleibt die MAC-Adresse eines virtuellen Computers nach ihrer Erstellung unverändert?
Ja. Die MAC-Adresse bleibt für eine VM so lange erhalten, bis sie gelöscht wird. Dabei spielt es keine Rolle, ob die VM über das Resource Manager- oder das klassische Bereitstellungsmodell bereitgestellt wurde. Bisher wurde die MAC-Adresse freigegeben, wenn die VM beendet (Aufhebung der Zuordnung) wurde, aber jetzt wird die MAC-Adresse auch dann beibehalten, wenn sich die VM im nicht zugeordneten Zustand befindet.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Kann ich von einem virtuellen Computer in einem VNet eine Verbindung mit dem Internet herstellen?
Ja. Für alle in einem VNet bereitgestellten VMs und Cloud Services-Rolleninstanzen kann eine Verbindung mit dem Internet hergestellt werden.

## <a name="azure-services-that-connect-to-vnets"></a>Azure-Dienste mit Verbindung mit VNets

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Kann ich Azure App Service-Web-Apps mit einem VNet verwenden?
Ja. Sie können Web-Apps in einem VNet mit einer ASE (App Service Environment, App Service-Umgebung) bereitstellen. Wenn Sie für Ihr VNET eine Point-to-Site-Verbindung konfiguriert haben, können alle Web-Apps eine sichere Verbindung herstellen und auf Ressourcen im VNET zugreifen. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Erstellen von Web-Apps in einer App Service-Umgebung](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Integrieren Ihrer App in ein Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Using VNet Integration and Hybrid Connections with Web Apps (in englischer Sprache)](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Können Cloud Services mit Web- und Workerrollen (PaaS) in einem VNet bereitgestellt werden?
Ja. Sie können Cloud Services-Rolleninstanzen (optional) in VNets bereitstellen. Hierfür geben Sie den Namen des VNet und die Rollen-/Subnetzzuordnungen im Netzwerkkonfigurationsabschnitt der Dienstkonfiguration an. Sie müssen keine Binärdateien aktualisieren.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-to-a-vnet"></a>Kann ich für eine VM-Skalierungsgruppe (Virtual Machine Scale Set, VMSS) eine Verbindung mit einem VNet herstellen?
Ja. Sie müssen für eine VM-Skalierungsgruppe eine Verbindung mit einem VNet herstellen.

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>Gibt es eine vollständige Liste der Azure-Dienste, über die ich Ressourcen in einem VNET bereitstellen kann?

Ja. Ausführliche Informationen finden Sie unter [Integration virtueller Netzwerke für Azure-Dienste](virtual-network-for-azure-services.md).

### <a name="which-azure-paas-resources-can-i-restrict-access-to-from-a-vnet"></a>Für welche Azure-PaaS-Ressourcen kann ich den Zugriff über ein VNET beschränken?

Über manche Azure-PaaS-Dienste (etwa Azure Storage und Azure SQL-Datenbank) bereitgestellte Ressourcen können durch die Verwendung von Dienstendpunkten im virtuellen Netzwerk den Netzwerkzugriff ausschließlich auf Ressourcen in einem VNET beschränken. Weitere Informationen finden Sie unter [Dienstendpunkte im virtuellen Netzwerk](virtual-network-service-endpoints-overview.md).

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Können Dienste in und aus VNets verschoben werden?
Nein. Sie können Dienste nicht in oder aus VNets verschieben. Wenn Sie eine Ressource in ein anderes VNET verschieben möchten, müssen Sie die Ressource löschen und neu bereitstellen.

## <a name="security"></a>Sicherheit

### <a name="what-is-the-security-model-for-vnets"></a>Welches Sicherheitsmodell wird für VNets verwendet?
VNETs werden von anderen VNETs und von anderen in der Azure-Infrastruktur gehosteten Diensten isoliert ausgeführt. Die Vertrauensstellungsgrenze entspricht der Begrenzung des VNet.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>Kann ich den Flow von eingehendem oder ausgehendem Datenverkehr auf Ressourcen mit VNet-Verbindung beschränken?
Ja. Sie können [Netzwerksicherheitsgruppen](security-overview.md) auf einzelne Subnetze in einem VNet, an ein VNet angefügte NICs oder beides anwenden.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Kann ich zwischen Ressourcen mit VNet-Verbindung eine Firewall einrichten?
Ja. Sie können ein [virtuelles Netzwerkgerät für eine Firewall](https://azure.microsoft.com/marketplace/?term=firewall) von mehreren Anbietern über den Azure Marketplace bereitstellen.

### <a name="is-there-information-available-about-securing-vnets"></a>Sind Informationen zum Schützen von VNets verfügbar?
Ja. Ausführliche Informationen finden Sie im Artikel [Die Netzwerksicherheit in Azure im Überblick](../security/security-network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="apis-schemas-and-tools"></a>APIs, Schemas und Tools

### <a name="can-i-manage-vnets-from-code"></a>Können VNets programmgesteuert verwaltet werden?
Ja. Sie können REST-APIs für VNETs im Rahmen des [Azure Resource Manager-Bereitstellungsmodells](/rest/api/virtual-network) und des [klassischen Bereitstellungsmodells (Dienstverwaltung)](http://go.microsoft.com/fwlink/?LinkId=296833) verwenden.

### <a name="is-there-tooling-support-for-vnets"></a>Sind Tools zur Unterstützung von VNets verfügbar?
Ja. Weitere Informationen zur Verwendung von folgenden Tools:
- Azure-Portal: Bereitstellen von VNets über das [Azure Resource Manager](manage-virtual-network.md#create-a-virtual-network)- und [klassische](virtual-networks-create-vnet-classic-pportal.md) Bereitstellungsmodell.
- PowerShell: Verwalten von VNets, die über das [Resource Manager](/powershell/module/azurerm.network)- und [klassische](/powershell/module/azure/?view=azuresmps-3.7.0) Bereitstellungsmodell bereitgestellt werden.
- Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI) zum Bereitstellen und Verwalten von VNETs, die über das [Resource Manager-Modell](/cli/azure/network/vnet) und das [klassische](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-commands-to-manage-network-resources) Bereitstellungsmodell bereitgestellt werden  

---
title: Virtuelles Azure-Netzwerk | Microsoft-Dokumentation
description: "Enthält Informationen zu den Konzepten und Features des virtuellen Azure-Netzwerks (Azure Virtual Network)."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/03/2017
ms.author: jdial
ms.openlocfilehash: dc6916bd25c5a020fdcef0707fe28a1e34fb0f88
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

Der Microsoft Azure Virtual Network-Dienst ermöglicht Azure-Ressourcen die sichere Kommunikation mit anderen in einem virtuellen Netzwerk. Ein virtuelles Netzwerk ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Ein virtuelles Netzwerk ist eine logische Isolierung von der Azure Cloud für Ihr Abonnement. Sie können virtuelle Netzwerke mit anderen virtuellen Netzwerken oder mit Ihrem lokalen Netzwerk verbinden. In der folgenden Abbildung sind einige Funktionen des Diensts für virtuelle Azure-Netzwerke dargestellt:

![Netzwerkdiagramm](./media/virtual-networks-overview/virtual-network-overview.png)

Sie können jeweils auf eine der folgenden Funktionen klicken, um weitere Informationen dazu anzuzeigen:
- **[Isolation](#isolation)**: Virtuelle Netzwerke sind voneinander isoliert. Sie können separate virtuelle Netzwerke für Entwicklung, Tests und Produktion erstellen, die die gleichen CIDR-Adressblöcke (zum Beispiel, 10.0.0.0/0) verwenden. Umgekehrt können Sie mehrere virtuelle Netzwerke erstellen, die unterschiedliche CIDR-Adressblöcke verwenden, und die Netzwerke verbinden. Sie können ein virtuelles Netzwerk in mehrere Subnetze segmentieren. Azure bietet interne Namensauflösung für virtuelle Computer und Azure Cloud Services-Rolleninstanzen, die in einem virtuellen Netzwerk bereitgestellt werden. Sie können wahlweise ein virtuelles Netzwerk konfigurieren, um Ihre eigenen DNS-Server anstelle der internen Namensauflösung von Azure zu verwenden.
- **[Internetkommunikation:](#internet)** Alle Azure Virtual Machines und Cloud Services-Rolleninstanzen in einem virtuellen Netzwerk haben standardmäßig Zugriff auf das Internet. Bei Bedarf können Sie auch den Zugriff in eingehender Richtung auf bestimmte Ressourcen ermöglichen.
- **[Azure-Ressourcenkommunikation:](#within-vnet)** Azure-Ressourcen, z.B. Cloud Services und virtuelle Computer, können im selben virtuellen Netzwerk bereitgestellt werden. Die Ressourcen können miteinander kommunizieren, indem Sie private IP-Adressen verwenden, auch wenn sie sich in unterschiedlichen Subnetzen befinden. Azure verfügt über Standardrouting zwischen Subnetzen, VNets und lokalen Netzwerken, sodass Sie keine Routen konfigurieren und verwalten müssen. Sie können jedoch das Routing von Azure anpassen, falls gewünscht.
- **[Virtuelle Netzwerkkonnektivität:](#connect-vnets)** Virtuelle Netzwerke können miteinander verbunden sein. Somit wird ermöglicht, dass Ressourcen in einem beliebigen virtuellen Netzwerk mit Ressourcen in einem beliebigen anderen virtuellen Netzwerk kommunizieren.
- **[Lokale Konnektivität:](#connect-on-premises)** Ein virtuelles Netzwerk kann privat mit einem lokalen Netzwerk oder mithilfe einer Standort-zu-Standort-VPN-Verbindung über das Internet verbunden sein.
- **[Filtern des Datenverkehrs:](#filtering)** Netzwerkdatenverkehr von virtuellen Computern und Cloud Services-Rolleninstanzen kann in eingehender und ausgehender Richtung nach IP-Quelladresse und -port, IP-Zieladresse und -port und Protokoll gefiltert werden.
- **[Routing:](#routing)** Sie können das Standardrouting von Azure optional außer Kraft setzen, indem Sie Ihre eigenen Routen konfigurieren oder BGP-Routen über ein Netzwerkgateway verbreiten.

## <a name = "isolation"></a>Netzwerkisolation und -segmentierung

Sie können in jedem Azure-[Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und in jeder Azure-[Region](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) mehrere virtuelle Netzwerke implementieren. Jedes virtuelle Netzwerk wird von den anderen virtuellen Netzwerken isoliert. Für jedes virtuelle Netzwerk können Sie folgende Aktionen ausführen:
- Geben Sie einen benutzerdefinierten privaten IP-Adressraum ein, indem Sie öffentliche und private Adressen (RFC 1918) verwenden. Azure weist Ressourcen in einem virtuellen Netzwerk eine private IP-Adresse aus dem von Ihnen zugewiesenen Adressraum zu.
- Segmentieren Sie das virtuelle Netzwerk in ein oder mehrere Subnetze, und weisen Sie jedem Subnetz einen Teil des Adressraums von jedem virtuellen Netzwerk zu.
- Verwenden Sie die in Azure enthaltene Namensauflösung, oder geben Sie Ihren eigenen DNS-Server an, der von Ressourcen in einem virtuellen Netzwerk genutzt werden kann. Weitere Informationen zur Namensauflösung in virtuellen Netzwerken finden Sie im Artikel [Namensauflösung für virtuelle Computer und Clouddienste](virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name = "internet"></a>Internetkommunikation
Alle Ressourcen in einem virtuellen Netzwerk können standardmäßig in ausgehender Richtung mit dem Internet kommunizieren. Die private IP-Adresse der Ressource wird von der Azure-Infrastruktur per SNAT-Vorgang (Source Network Address Translated) in eine öffentliche IP-Adresse übersetzt. Weitere Informationen zur Internetkonnektivität in ausgehender Richtung finden Sie im Artikel [Grundlegendes zu ausgehenden Verbindungen in Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address). Um die ausgehende Internetkonnektivität zu verhindern, können Sie benutzerdefinierte Routen oder eine Filterung des Datenverkehrs implementieren.

Um über das Internet in eingehender Richtung mit Azure-Ressourcen bzw. in ausgehender Richtung ohne SNAT über das Internet kommunizieren zu können, muss einer Ressource eine öffentliche IP-Adresse zugewiesen sein. Weitere Informationen zu öffentlichen IP-Adressen finden Sie im Artikel [Öffentliche IP-Adressen](virtual-network-public-ip-address.md).

## <a name="within-vnet"></a>Sichere Kommunikation zwischen Azure-Ressourcen

Sie können virtuelle Computer in einem virtuellen Netzwerk bereitstellen. Virtuelle Computer kommunizieren mit anderen Ressourcen in einem virtuellen Netzwerk über eine Netzwerkschnittstelle. Weitere Informationen zu Netzwerkschnittstellen finden Sie unter [Netzwerkschnittstellen](virtual-network-network-interface.md).

Sie können auch verschiedene andere Typen von Azure-Ressourcen mit einem virtuellen Netzwerk, z.B. Azure Virtual Machines, Azure Cloud Services, Azure App Service-Umgebungen und Azure-VM-Skalierungsgruppen bereitstellen. Eine vollständige Liste der Azure-Ressourcen, die Sie in einem virtuellen Netzwerk bereitstellen können, finden Sie unter [Virtual Network Service-Integration für Azure-Dienste](virtual-network-for-azure-services.md).

Einige Ressourcen können nicht in einem virtuellen Netzwerk bereitgestellt werden, ermöglichen es Ihnen jedoch, die Kommunikation von Ressourcen nur innerhalb eines virtuellen Netzwerks einzuschränken. Weitere Informationen, um den Zugriff auf Ressourcen zu beschränken, finden Sie unter [Dienstendpunkte von virtuellen Netzwerken](virtual-network-service-endpoints-overview.md). 

## <a name="connect-vnets"></a>Herstellen von Verbindungen zwischen virtuellen Netzwerken

Sie können virtuelle Netzwerke miteinander verbinden, indem Sie Ressourcen in beiden virtuellen Netzwerken ermöglichen, mithilfe des Peerings virtueller Netzwerke, miteinander zu kommunizieren. Die Bandbreite und die Latenz der Kommunikation zwischen Ressourcen in unterschiedlichen virtuellen Netzwerken ist dieselbe wie bei Ressourcen, die im selben virtuellen Netzwerk miteinander verbunden sind. Weitere Informationen zum Peering finden Sie im Artikel [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md).

## <a name="connect-on-premises"></a>Herstellen einer Verbindung mit einem lokalen Netzwerk

Sie können Ihr lokales Netzwerk mit einem virtuellen Netzwerk verbinden, indem Sie irgendeine Kombination von folgenden Optionen verwenden:
- **Point-to-Site virtuelles privates Netzwerk (VPN):** wird zwischen einem virtuellen Netzwerk und einem einzelnen PC in Ihrem Netzwerk eingerichtet. Jeder PC, der eine Verbindung mit einem virtuellen Netzwerk herstellen möchte, muss seine Verbindungen unabhängig konfigurieren. Dieser Verbindungstyp ist gut geeignet, wenn Azure noch neu für Sie ist, oder wenn Sie ein Entwickler sind, da keine oder nur sehr geringe Änderungen Ihres vorhandenen Netzwerks erforderlich sind. Für die Verbindung wird das SSTP-Protokoll verwendet, um zwischen dem PC und einem virtuellen Netzwerk eine verschlüsselte Kommunikation über das Internet zu ermöglichen. Die Dauer der Wartezeit für ein Point-to-Site-VPN ist unvorhersehbar, weil der Datenverkehr über das Internet übertragen wird.
- **Site-to-Site-VPN:** wird zwischen Ihrem VPN-Gerät und einem Azure VPN Gateway eingerichtet, das in einem virtuellen Netzwerk bereitgestellt ist. Bei diesem Verbindungstyp können alle lokalen Ressourcen, die von Ihnen autorisiert werden, auf ein virtuelles Netzwerk zugreifen. Die Verbindung ist ein IPSec/IKE-VPN, mit dem die verschlüsselte Kommunikation über das Internet zwischen Ihrem lokalen Gerät und dem Azure VPN Gateway ermöglicht wird. Die Dauer der Wartezeit für eine Site-to-Site-Verbindung ist unvorhersehbar, da der Datenverkehr über das Internet übertragen wird.
- **Azure ExpressRoute:** Wird zwischen Ihrem Netzwerk und Azure über einen ExpressRoute-Partner eingerichtet. Diese Verbindung ist privat. Der Datenverkehr wird nicht über das Internet übertragen. Die Dauer der Wartezeit für eine ExpressRoute-Verbindung ist vorhersehbar, da der Datenverkehr nicht über das Internet verläuft.

Weitere Informationen zu allen vorherigen Verbindungsoptionen finden Sie im Artikel [Diagramme zur Verbindungstopologie](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).

## <a name="filtering"></a>Filtern des Netzwerkdatenverkehrs
Sie können den Netzwerkdatenverkehr zwischen Subnetzen filtern, indem Sie eine oder beide folgenden Optionen verwenden:
- **Netzwerksicherheitsgruppen:** Eine Netzwerksicherheitsgruppe kann mehrere Sicherheitsregeln für die eingehende und ausgehende Richtung enthalten, mit denen Sie den Datenverkehr nach Quell- und Ziel-IP-Adresse, Port und Protokoll filtern können. Sie können eine Netzwerksicherheitsgruppe auf jede Netzwerkschnittstelle auf einem virtuellen Computer anwenden. Sie können auch eine Netzwerksicherheitsgruppe auf das Subnetz anwenden, mit dem eine Netzwerkschnittstelle oder eine andere Azure-Ressource verbunden ist. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppen](security-overview.md#network-security-groups).
- **Virtuelle Netzwerkgeräte:** Ein virtuelles Netzwerkgerät ist ein virtueller Computer, auf dem eine Software für eine Netzwerkfunktion ausgeführt wird, z.B. eine Firewall. Eine Liste von virtuellen Netzwerkgeräten finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Es sind auch virtuelle Netzwerkgeräte verfügbar, die eine WAN-Optimierung und andere Funktionen für den Netzwerkdatenverkehr ermöglichen. Virtuelle Netzwerkgeräte werden im Allgemeinen mit benutzerdefinierten oder BGP-Routen verwendet. Sie können auch ein virtuelles Netzwerkgerät nutzen, um den Datenverkehr zwischen virtuellen Netzwerken zu filtern.

## <a name="routing"></a>Weiterleiten von Netzwerkdatenverkehr

Azure erstellt Routingtabellen, über die Ressourcen, die mit einem beliebigen Subnetz in einem beliebigen virtuellen Netzwerk verbunden sind, standardmäßig miteinander kommunizieren können. Sie können eine oder beide der folgenden Optionen implementieren, um die von Azure erstellten Standardrouten außer Kraft zu setzen:
- **Benutzerdefinierte Routen:** Sie können benutzerdefinierte Routentabellen mit Routen erstellen, über die gesteuert wird, wohin der Datenverkehr für die einzelnen Subnetze geleitet wird. Weitere Informationen zu benutzerdefinierten Routen finden Sie in [Benutzerdefinierte Routen](virtual-networks-udr-overview.md#user-defined).
- **BGP-Routen:** Wenn Sie Ihr virtuelles Netzwerk mit Ihrem lokalen Netzwerk über ein Azure-VPN-Gateway oder eine ExpressRoute-Verbindung verbinden, können Sie Ihren virtuellen Netzwerken BGP-Routen weitergeben.

## <a name="pricing"></a>Preise

Für virtuelle Netzwerke, Subnetze, Routentabellen oder Netzwerksicherheitsgruppen werden keine Gebühren berechnet. Für Internetbandbreite in ausgehender Richtung, öffentliche IP-Adressen, Peering in virtuellen Netzwerken, VPN Gateways und ExpressRoute werden jeweils eigene Preisstrukturen verwendet. Weitere Informationen hierzu finden Sie auf den Preisseiten zu [virtuellen Netzwerken](https://azure.microsoft.com/pricing/details/virtual-network), [VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) und [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute).

## <a name="faq"></a>Häufig gestellte Fragen

Wenn Sie sich die häufig gestellten Fragen zu Azure Virtual Network ansehen möchten, lesen Sie den Artikel [Virtual Network FAQ](virtual-networks-faq.md).


## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie Ihr erstes virtuelles Netzwerk, und stellen Sie einige virtuelle Computer darin bereit, indem Sie die Schritte des Artikels [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md) ausführen.
- Erstellen Sie eine Point-to-Site-Verbindung mit einem virtuellen Netzwerk, indem Sie die Schritte im Artikel [Konfigurieren einer Point-to-Site-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ausführen.
- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) von Azure.

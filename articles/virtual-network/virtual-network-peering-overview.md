---
title: Peering in virtuellen Azure-Netzwerken | Microsoft-Dokumentation
description: Weitere Informationen zum Peering in virtuellen Netzwerken in Azure
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: jdial
ms.openlocfilehash: 69264ac7ffe8124b964a10c11aacaca5c712fd63
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="virtual-network-peering"></a>Peering in virtuellen Netzwerken

Das Peering virtueller Netzwerke ermöglicht das nahtlose Verbinden zweier [virtueller Azure-Netzwerke](virtual-networks-overview.md). Nach dem Peering werden die virtuellen Netzwerke für Verbindungszwecke als einzelnes Element angezeigt. Der Datenverkehr zwischen virtuellen Computern in den virtuellen Netzwerken, die mittels Peering verknüpft sind, wird ähnlich wie der Datenverkehr zwischen virtuellen Computern im gleichen Netzwerk nur über *private* IP-Adressen über die Microsoft-Backbone-Infrastruktur geleitet. 

Die Verwendung von VNET-Peering bietet unter anderem folgende Vorteile:

* Netzwerkdatenverkehr zwischen virtuellen Netzwerken, die mittels Peering verknüpft sind, ist privat. Datenverkehr zwischen den virtuellen Netzwerken bleibt innerhalb des Microsoft-Backbone-Netzwerks. Die Kommunikation zwischen den virtuellen Netzwerken kommt ganz ohne öffentliches Internet, Gateways oder Verschlüsselung aus.
* Niedrige Latenz, Verbindung mit hoher Bandbreite zwischen Ressourcen in unterschiedlichen virtuellen Netzwerken
* Möglichkeit zur Kommunikation zwischen Ressourcen in verschiedenen virtuellen Netzwerken nach dem Peering der virtuellen Netzwerke
* Möglichkeit zur Übertragung von Daten über Azure-Abonnements, Bereitstellungsmodelle und Azure-Regionen hinweg
* Möglichkeit zum Peering von virtuellen Netzwerken, die mit dem Azure Resource Manager erstellt wurden, oder eines virtuellen Netzwerks, das mit dem Azure Resource Manager-Modell erstellt wurde, mit einem virtuellen Netzwerk, das auf dem klassischen Bereitstellungsmodell basiert. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Keine Ausfallzeiten für Ressourcen in beiden virtuellen Netzwerken beim Erstellen des Peerings oder nachdem das Peering erstellt wurde

## <a name="connectivity"></a>Konnektivität

Nachdem das Peering für virtuelle Netzwerke durchgeführt wurde, kann für Ressourcen in einem der virtuellen Netzwerke eine direkte Verbindung mit den Ressourcen im virtuellen Peernetzwerk hergestellt werden.

Die Netzwerklatenz zwischen virtuellen Computern in per Peering verknüpften virtuellen Netzwerken in der gleichen Region entspricht der Netzwerklatenz in einem einzelnen virtuellen Netzwerk. Der Netzwerkdurchsatz basiert auf der Bandbreite, die für den virtuellen Computer proportional zu seiner Größe zulässig ist. Im Peering bestehen keine weiteren Bandbreiteneinschränkungen.

Der Datenverkehr zwischen virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken wird nicht über ein Gateway oder das öffentliche Internet, sondern direkt über die Microsoft-Backbone-Infrastruktur geleitet.

Netzwerksicherheitsgruppen können bei Bedarf in beiden virtuellen Netzwerken angewendet werden, um den Zugriff auf andere virtuelle Netzwerke oder Subnetze zu blockieren.
Beim Konfigurieren des VNET-Peerings können Sie die Regeln für Netzwerksicherheitsgruppen zwischen den virtuellen Netzwerken öffnen oder schließen. Wenn Sie sich für das Öffnen der vollständigen Konnektivität zwischen den mittels Peering verknüpften virtuellen Netzwerken entscheiden (Standardoption), können Sie Netzwerksicherheitsgruppen in bestimmten Subnetzen oder auf virtuellen Computern verwenden, um den Zugriff jeweils spezifisch zu blockieren oder zu verweigern. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppen – Übersicht](virtual-networks-nsg.md).

## <a name="service-chaining"></a>Dienstverkettung

Sie können benutzerdefinierte Routingtabellen konfigurieren, die auf virtuelle Computer in mittels Peering verknüpften virtuellen Netzwerken als IP-Adresse für den *nächsten Hop* (oder auf virtuelle Netzwerkgateways) verweisen, um die Dienstverkettung zu aktivieren. Eine Dienstverkettung ermöglicht es, Datenverkehr über benutzerdefinierte Routingtabellen aus einem virtuellen Netzwerk an ein virtuelles Gerät (oder virtuelles Netzwerkgateway) in einem mittels Peering verknüpften virtuellen Netzwerk zu leiten.

Sie können auch Hub-and-Spoke-Netzwerke bereitstellen, in denen das virtuelle Hubnetzwerk Infrastrukturkomponenten wie etwa ein virtuelles Netzwerkgerät oder ein VPN-Gateway hosten kann. Alle virtuellen Spoke-Netzwerke können dann mittels Peering mit dem virtuellen Hubnetzwerk verknüpft werden. Datenverkehr kann virtuelle Netzwerkgeräte oder VPN-Gateways im virtuellen Hubnetzwerk durchlaufen. 

Beim VNET-Peering kann es sich bei dem nächsten Hop einer benutzerdefinierten Route um die IP-Adresse eines virtuellen Computers im mittels Peering verknüpften virtuellen Netzwerk oder um ein VPN-Gateway handeln. Zwischen virtuellen Netzwerken ist jedoch kein Routing über eine benutzerdefinierte Route möglich, die als Art des nächsten Hops ein ExpressRoute-Gateway angibt. Für weitere Informationen zu benutzerdefinierten Routen können Sie sich die [Übersicht zu benutzerdefinierten Routen](virtual-networks-udr-overview.md#user-defined) ansehen. Informationen zum Erstellen einer Hub-Spoke-Netzwerktopologie finden Sie unter [Implement a hub-spoke network topology in Azure](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering) (Implementieren einer Hub-Spoke-Netzwerktopologie in Azure).

## <a name="gateways-and-on-premises-connectivity"></a>Gateways und lokale Konnektivität

Jedes virtuelle Netzwerk kann unabhängig davon, ob eine mittels Peering hergestellte Verknüpfung mit einem anderen virtuellen Netzwerk besteht, weiterhin über ein eigenes Gateway verfügen und dieses zum Herstellen einer Verbindung mit einem lokalen Netzwerk verwenden. Sie können auch dann [VNET-zu-VNET-Verbindungen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) unter Verwendung von Gateways konfigurieren, wenn die virtuellen Netzwerke mittels Peering miteinander verknüpft sind.

Wenn beide Optionen für Verbindungen zwischen virtuellen Netzwerken konfiguriert sind, fließt der Datenverkehr zwischen den virtuellen Netzwerken über die Peeringkonfiguration (also über den Azure-Backbone).

Wenn virtuelle Netzwerke mittels Peering in derselben Region verknüpft sind, können Sie auch das Gateway im mittels Peering verknüpften virtuellen Netzwerk als Transitpunkt für ein lokales Netzwerk konfigurieren. In diesem Fall kann das virtuelle Netzwerk, das ein Remotegateway verwendet, kein eigenes Gateway besitzen. Ein virtuelles Netzwerk kann immer nur ein einzelnes Gateway aufweisen. Bei diesem Gateway kann es sich um ein lokales Gateway oder ein Remotegateway (im mittels Peering verknüpften virtuellen Netzwerk) handeln, wie im folgenden Bild zu sehen ist:

![VNET-Peering – Transit](./media/virtual-networks-peering-overview/figure04.png)

Der Gatewaytransit wird in der Peeringbeziehung zwischen virtuellen Netzwerken, die mit unterschiedlichen Bereitstellungsmodellen oder in unterschiedlichen Regionen erstellt wurden, nicht unterstützt. Sie müssen beide virtuellen Netzwerke in der Peeringbeziehung mit dem Resource Manager-Bereitstellungsmodell erstellen, und diese müssen sich in derselben Region befinden, um den Gatewaytransit verwenden zu können.

Wenn die virtuellen Netzwerke, die gemeinsam eine einzelne ExpressRoute-Verbindung nutzen, mittels Peering verknüpft sind, fließt der Datenverkehr zwischen ihnen über die Peeringbeziehung (also über das Azure-Backbonenetzwerk). Sie können in den einzelnen virtuellen Netzwerken weiterhin lokale Gateways verwenden, um eine Verbindung mit der lokalen Umgebung herzustellen. Alternativ können Sie ein gemeinsam genutztes Gateway verwenden und den Transit für lokale Konnektivität konfigurieren.

## <a name="troubleshoot"></a>Problembehandlung

Zur Bestätigung eines VNET-Peerings können Sie für eine Netzwerkschnittstelle in einem beliebigen Subnetz eines virtuellen Netzwerks [effektive Routen überprüfen](virtual-network-routes-troubleshoot-portal.md). Ist ein VNET-Peering vorhanden, verfügen alle Subnetze innerhalb des virtuellen Netzwerks über Routen, deren Art des nächsten Hops *VNET-Peering* lautet. Dies gilt für jeden Adressraum in jedem mittels Peering verknüpften virtuellen Netzwerk.

Probleme mit der Konnektivität eines virtuellen Computers in einem mittels Peering verknüpften virtuellen Netzwerk können auch mithilfe der [Konnektivitätsprüfung](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) von Network Watcher behandelt werden. Mit der Konnektivitätsprüfung können Sie ermitteln, wie Datenverkehr von der Netzwerkschnittstelle eines virtuellen Quellcomputers an die Netzwerkschnittstelle eines virtuellen Zielcomputers geleitet wird.

## <a name="requirements-and-constraints"></a>Anforderungen und Einschränkungen

Informationen zu Anforderungen und Einschränkungen für das Peering virtueller Netzwerke finden Sie [hier](virtual-network-manage-peering.md#requirements-and-constraints). Informationen zu den Grenzwerten für die Anzahl von Peerings, die für ein virtuelles Netzwerk erstellt werden können, finden Sie unter [Netzwerkgrenzwerte – Azure Resource Manager](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). 

## <a name="permissions"></a>Berechtigungen

Informationen zu erforderlichen Berechtigungen für die Erstellung eines Peerings virtueller Netzwerke finden Sie unter [Berechtigungen](virtual-network-manage-peering.md#permissions).

## <a name="pricing"></a>Preise

Für ein- und ausgehenden Datenverkehr, der eine VNET-Peeringverbindung verwendet, fällt eine Gebühr an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Nächste Schritte

* Ein Peering für virtuelle Netzwerke wird zwischen virtuellen Netzwerken erstellt, die mit dem gleichen oder unterschiedlichen Bereitstellungsmodellen unter demselben oder unterschiedlichen Abonnements erstellt wurden. Arbeiten Sie ein Tutorial für eines der folgenden Szenarien durch:

    |Azure-Bereitstellungsmodell             | Abonnement  |
    |---------                          |---------|
    |Beide mit Resource Manager              |[Gleich](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Unterschiedlich](create-peering-different-subscriptions.md)|
    |Einmal Resource Manager, einmal klassisch  |[Gleich](create-peering-different-deployment-models.md)|
    |                                   |[Unterschiedlich](create-peering-different-deployment-models-subscriptions.md)|

* Informieren Sie sich über das Erstellen einer [Hub-and-Spoke-Netzwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering).
* Erfahren Sie mehr über alle [Einstellungen für das VNET-Peering und deren Änderung](virtual-network-manage-peering.md).
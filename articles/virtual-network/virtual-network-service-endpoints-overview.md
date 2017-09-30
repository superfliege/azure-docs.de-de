---
title: Dienstendpunkte von virtuellen Azure-Netzwerken | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie den direkten Zugriff auf Azure-Ressourcen aus einem virtuellen Netzwerk über Dienstendpunkte ermöglichen."
services: virtual-network
documentationcenter: na
author: anithaa
manager: narayan
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: anithaa
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 297c51cca31d981ffadd20cccce1cd3dd69dc81c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="virtual-network-service-endpoints-preview"></a>Dienstendpunkte von virtuellen Netzwerken (Vorschauversion)

Mit Dienstendpunkten von virtuellen Netzwerken (VNETs) werden der Bereich privater Adressen Ihres virtuellen Netzwerks und die Identität Ihres VNET über eine direkte Verbindung auf die Azure-Dienste erweitert. Endpunkte ermöglichen es Ihnen, Ihre kritischen Ressourcen von Azure-Diensten auf Ihre virtuellen Netzwerke zu beschränken und so zu schützen. Der Datenverkehr aus Ihrem VNET an den Azure-Dienst verbleibt immer im Backbone-Netzwerk von Microsoft Azure.

Dieses Feature ist für die folgenden Azure-Dienste und -Regionen als __Vorschauversion__ verfügbar:

__Azure Storage__: WestCentralUS, WestUS2, EastUS, WestUS, AustraliaEast, AustraliaSouthEast.

__Azure SQL-Datenbank__: WestCentralUS, WestUS2, EastUS.

Aktuelle Benachrichtigungen zur Vorschauversion finden Sie auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/?product=virtual-network).

>[!NOTE]
Während der Vorschauphase ist das Feature unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="key-benefits"></a>Hauptvorteile

Dienstendpunkte bieten folgende Vorteile:

- __Verbesserte Sicherheit für Ihre Azure-Dienstressourcen__

  Mit Dienstendpunkten können Ressourcen von Azure-Diensten auf Ihr virtuelles Netzwerk beschränkt und so geschützt werden. Auf diese Weise erhöhen Sie die Sicherheit, da der Zugriff über das öffentliche Internet auf diese Ressourcen vollständig verhindert und nur Datenverkehr aus Ihrem virtuellen Netzwerk zugelassen wird.

- __Optimale Weiterleitung des Datenverkehrs für Azure-Dienste aus Ihrem virtuellen Netzwerk__

  Heutzutage wird für alle Routen Ihres VNET, die für Internet-Datenverkehr den Weg über lokale bzw. virtuelle Geräte erzwingen (als Tunnelerzwingung bezeichnet), auch für den Datenverkehr von Azure-Diensten die gleiche Route wie für den Internet-Datenverkehr erzwungen. Dienstendpunkte ermöglichen eine optimale Weiterleitung für Azure-Datenverkehr. 

  Endpunkte leiten den Datenverkehr der Dienste direkt aus Ihrem VNET an den Dienst im Microsoft Azure-Backbone-Netzwerk. Sie können also mit dem Überprüfen und Überwachen des ausgehenden Internet-Datenverkehrs aus Ihren VNETs fortfahren, indem Sie die Tunnelerzwingung nutzen, ohne dass sich dies auf den Datenverkehr der Dienste auswirkt. Informieren Sie sich über [benutzerdefinierte Routen und die Tunnelerzwingung](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- __Einfache Einrichtung mit weniger Verwaltungsaufwand__

  Sie benötigen in Ihren VNETs keine reservierten öffentlichen IP-Adressen mehr, um Azure-Ressourcen über die IP-Firewall zu schützen. Es sind keine NAT- oder Gatewaygeräte erforderlich, um die Dienstendpunkte einzurichten. Der Dienstendpunkt einfach per Klick in einem Subnetz konfiguriert werden. Es entsteht kein zusätzlicher Aufwand für die Verwaltung der Endpunkte.

## <a name="limitations"></a>Einschränkungen

- Das Feature ist nur für VNETs verfügbar, für die das Azure Resource Manager-Bereitstellungsmodell verwendet wird.
- Endpunkte sind für Subnetze aktiviert, die in Azure VNETs konfiguriert sind. Endpunkte können nicht für Datenverkehr verwendet werden, der aus Ihrer lokalen Umgebung an Azure-Dienste fließt. Ausführliche Informationen finden Sie unter [Schützen des Zugriffs auf Azure-Dienste aus der lokalen Umgebung](#securing-azure-services-to-virtual-networks).
- Ein Dienstendpunkt gilt nur für Datenverkehr von Azure-Diensten in der VNET-Region. Damit Azure Storage Datenverkehr vom Typ RA-GRS und GRS unterstützen kann, gilt dies zusätzlich auch für die gekoppelte Region, in der das VNET bereitgestellt wird. Informieren Sie sich über [Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions).

## <a name="securing-azure-services-to-virtual-networks"></a>Schützen von Azure-Diensten in virtuellen Netzwerken

- Der VNET-Dienstendpunkt stellt die Identität Ihres VNET für den Azure-Dienst bereit. Nachdem Dienstendpunkte in Ihrem VNET aktiviert wurden, können Sie Ressourcen von Azure-Diensten auf Ihr VNET beschränken und auf diese Weise schützen, indem Sie den Ressourcen eine Regel für das virtuelle Netzwerk hinzufügen.
- Heutzutage werden für den Datenverkehr der Azure-Dienste aus Ihrem VNET öffentliche IPs als Quell-IPs verwendet. Bei Verwendung von Dienstendpunkten wird für den Dienstdatenverkehr zu privaten VNET-Adressen als Quell-IPs gewechselt, wenn aus Ihrem VNET auf den Azure-Dienst zugegriffen wird. Dieser Wechsel ermöglicht Ihnen den Zugriff auf die Dienste, ohne dass reservierte öffentliche IP-Adressen in IP-Firewalls verwendet werden müssen.
- __Schützen des Zugriffs auf Azure-Dienste aus der lokalen Umgebung__:
 
   Standardmäßig sind Azure-Dienstressourcen, die auf virtuelle Netzwerke beschränkt und so geschützt sind, über lokale Netzwerke nicht erreichbar. Wenn Sie Datenverkehr aus der lokalen Umgebung zulassen möchten, müssen Sie NAT-IP-Adressen aus der lokalen Umgebung bzw. für ExpressRoute-Verbindungen zulassen. NAT-IP-Adressen können über die Konfiguration der IP-Firewall für Azure-Dienstressourcen hinzugefügt werden.
  
   __ExpressRoute__: 

    Wenn Sie [ExpressRoute](/azure/expressroute/expressroute-introduction) (ER) in Ihrer lokalen Umgebung nutzen, werden für jede ExpressRoute-Verbindung zwei NAT-IP-Adressen verwendet. Diese werden auf den Datenverkehr der Azure-Dienste angewendet, wenn der Datenverkehr im Microsoft Azure-Netzwerk-Backbone eintrifft.  Um den Zugriff auf Ihre Dienstressourcen zuzulassen, müssen Sie diese beiden IP-Adressen in der Ressourceneinstellung der IP-Firewall zulassen.  [Öffnen Sie über das Azure-Portal ein Supportticket für ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview), um die IP-Adressen Ihrer ExpressRoute-Verbindung zu ermitteln.

![Schützen von Azure-Diensten in virtuellen Netzwerken](media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Konfiguration

- Dienstendpunkte werden in einem Subnetz eines VNET konfiguriert. Für Endpunkte können alle Arten von Computeinstanzen verwendet werden, die in diesem Subnetz ausgeführt werden.
- Nur ein Dienstendpunkt kann für einen bestimmten Dienst aus einem Subnetz aktiviert werden. Sie können mehrere Dienstendpunkte für alle unterstützten Azure-Dienste (z.B. Storage, SQL) eines Subnetzes konfigurieren.
- VNETs sollten sich in derselben Region wie die Ressource des Azure-Diensts befinden. Für Azure Storage sollte sich bei Verwendung von GRS- und RA-GRS-Konten das primäre Konto in derselben Region wie das VNET befinden.
- Das VNET, in dem der Endpunkt konfiguriert ist, kann sich unter demselben oder einem anderen Abonnement wie die Ressource des Azure-Diensts befinden. Weitere Informationen zu den erforderlichen Berechtigungen zum Einrichten von Endpunkten und Schützen von Azure-Diensten finden Sie unten im Abschnitt [Bereitstellung](#Provisioning).
- Für unterstützte Dienste können Sie neue oder vorhandene Ressourcen in VNETs schützen, indem Sie Dienstendpunkte verwenden.

### <a name="considerations"></a>Überlegungen

- Bei der Aktivierung eines Dienstendpunkts wird für die IP-Quelladressen von virtuellen Computern im Subnetz von der Verwendung öffentlicher IPv4-Adressen jeweils zur Verwendung der entsprechenden privaten IPv4-Adresse gewechselt, wenn aus diesem Subnetz mit dem Dienst kommuniziert wird.
  - Alle vorhandenen geöffneten TCP-Verbindungen mit dem Dienst werden während dieses Wechselvorgangs geschlossen. Achten Sie darauf, dass keine kritischen Aufgaben ausgeführt werden, wenn Sie einen Dienstendpunkt eines Diensts für ein Subnetz aktivieren oder deaktivieren. Stellen Sie außerdem sicher, dass Ihre Anwendungen nach der Umstellung dieser IP-Adresse automatisch eine Verbindung mit Azure-Diensten herstellen können.
  - Die Umstellung der IP-Adresse wirkt sich nur auf Dienstdatenverkehr aus Ihrem VNET aus. Es ergeben sich keine Auswirkungen auf anderen Datenverkehr an bzw. von öffentlichen IPv4-Adressen, die Ihren VMs zugewiesen sind.
  - Wenn Sie über vorhandene Firewallregeln mit öffentlichen Azure-IP-Adressen verfügen, funktionieren diese Regeln für Azure-Dienste nicht mehr, nachdem die Umstellung auf private VNET-Adressen durchgeführt wurde.
- Bei Verwendung von Dienstendpunkten bleiben DNS-Einträge für Azure-Dienste unverändert und werden weiterhin in IP-Adressen aufgelöst, die dem Azure-Dienst zugewiesen werden.
- Netzwerksicherheitsgruppen (NSGs) mit Dienstendpunkten:
  - Da für NSGs ausgehender Internet-Datenverkehr standardmäßig zugelassen wird, ist auch der Datenverkehr aus Ihrem VNET zu Azure-Diensten zulässig. In Verbindung mit Dienstendpunkten funktioniert dies weiter wie bisher.
  - Falls Sie den gesamten ausgehenden Internet-Datenverkehr verweigern und nur Datenverkehr für bestimmte Azure-Dienste zulassen möchten, können Sie hierfür __Azure-Diensttags__ in Ihren NSGs verwenden. Sie können unterstützte Azure-Dienste als Ziel in Ihren NSG-Regeln angeben, und die Wartung der zugrunde liegenden IP-Adressen der einzelnen Tags wird von Azure bereitgestellt. Weitere Informationen finden Sie unter [Azure Service tags for NSGs](https://aka.ms/servicetags) (Azure-Diensttags für NSGs).

### <a name="scenarios"></a>Szenarien
- Mittels Peering verknüpfte, verbundene oder mehrere VNETs:

Zum Schützen von Azure-Diensten in mehreren Subnetzen innerhalb eines VNET oder mehrerer VNETs können Sie Dienstendpunkte unabhängig voneinander in den einzelnen Subnetzen aktivieren und Ressourcen von Azure-Diensten in allen diesen Subnetzen schützen.

- Filtern von ausgehendem Datenverkehr, der aus dem VNET an Azure-Dienste fließt:

Wenn Sie den Datenverkehr, der aus dem virtuellen Netzwerk an einen Azure-Dienst fließen soll, untersuchen und filtern möchten, können Sie in diesem VNET ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) bereitstellen. Anschließend können Sie Dienstendpunkte auf das Subnetz anwenden, in dem das virtuelle Netzwerkgerät bereitgestellt wurde, und Ressourcen des Azure-Diensts auf dieses Subnetz beschränken und so schützen. Dieses Szenario kann hilfreich sein, wenn Sie den Zugriff auf den Azure-Dienst aus Ihrem VNET per NVA-Filterung nur auf bestimmte Azure-Ressourcen beschränken möchten. Weitere Informationen finden Sie im Artikel [Egress with NVAs](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/nva-ha#egress-with-layer-7-nvas) (Ausgehender Datenverkehr mit NVAs).

- Schützen von Azure-Ressourcen für Dienste, die direkt in VNETs bereitgestellt werden:

Verschiedene Azure-Dienste können in Ihren VNETs direkt in bestimmten Subnetzen bereitgestellt werden. Sie können Ressourcen von Azure-Diensten für Subnetze mit [verwalteten Diensten](virtual-network-for-azure-services.md) schützen, indem Sie im Subnetz des verwalteten Diensts einen Dienstendpunkt einrichten.

### <a name="logging-and-troubleshooting"></a>Protokollierung und Problembehandlung

Nachdem Dienstendpunkte für einen bestimmten Dienst konfiguriert wurden, können Sie wie folgt überprüfen, ob die Dienstendpunktroute eingerichtet wurde: 

- __Überwachen der Diagnose für Azure-Dienste__: Sie können bestätigen, dass der Anforderungszugriff aus Ihrem privaten Netzwerk (VNET) stammt, indem Sie die Quell-IP einer Dienstanforderung in der Dienstdiagnose überprüfen. Alle neuen Anforderungen mit Dienstendpunkten zeigen die Quell-IP für die Anforderung als private VNET-Adresse an, die dem Client zugewiesen ist, über den die Anforderung aus Ihrem VNET gesendet wird. Ohne den Endpunkt handelt es sich bei dieser Adresse um eine öffentliche Azure-IP-Adresse.

- __Effektive Routen__ auf NICs eines Subnetzes zeigen eine spezifischere Standardroute für den Adresspräfixbereich des Diensts an. Der „nextHopType“ der Route lautet „VirtualNetworkServiceEndpoint“. Diese Route gibt an, dass – gegenüber Routen mit Tunnelerzwingung – eine direktere Verbindung mit dem Dienst vorhanden ist.

>[!NOTE]
Die Dienstendpunktroute setzt BGP- oder UDR-Routen für die Übereinstimmung mit dem Adresspräfix als Azure-Dienst außer Kraft. Informieren Sie sich über das [Verwenden von effektiven Routen zur Problembehandlung des Datenverkehrsflusses auf virtuellen Computern](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#using-effective-routes-to-troubleshoot-vm-traffic-flow).

## <a name="provisioning"></a>Bereitstellung

Dienstendpunkte können in virtuellen Netzwerken einzeln von einem Benutzer konfiguriert werden, der über Schreibzugriff auf das virtuelle Netzwerk verfügt.

Zum Schützen der Ressourcen von Azure-Diensten in einem VNET muss der Benutzer die Berechtigung „Microsoft.Network/JoinServicetoaSubnet“ für die hinzuzufügenden Subnetze haben. Diese Berechtigung ist standardmäßig in die integrierten Dienstadministratorrollen integriert und kann durch die Erstellung von benutzerdefinierten Rollen geändert werden.

Erfahren Sie mehr über [integrierte Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles).

VNETs und Ressourcen von Azure-Diensten können sich in demselben oder in unterschiedlichen Abonnements befinden. Wenn das VNET und die Ressourcen von Azure-Diensten in unterschiedlichen Abonnements enthalten sind, sollten sich die Ressourcen im Rahmen dieser Vorschauversion unter demselben Active Directory-Mandanten befinden. 

## <a name="pricing-and-limits"></a>Preise und Einschränkungen

Für die Nutzung von Dienstendpunkten fallen keine zusätzlichen Gebühren an. Das aktuelle Preismodell für Azure-Dienste (Azure Storage, Azure SQL-Datenbank) gilt unverändert.

Für die Gesamtzahl von Dienstendpunkten in einem VNET gilt keine Beschränkung.

Für die Ressource eines Azure-Diensts (z.B. Storage-Konto) können Dienste Beschränkungen in Bezug auf die Anzahl von Subnetzen erzwingen, die zum Schützen der Ressource verwendet werden. Ausführliche Informationen finden Sie in der Dokumentation zu den verschiedenen Diensten unter [Nächste Schritte](#next%20steps).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Konfigurieren von VNET-Dienstendpunkten](virtual-network-service-endpoints-configure.md).
- Informieren Sie sich über [Securing Azure Storage accounts to Virtual Networks](https://docs.microsoft.com/azure/storage/common/storage-network-security) (Schützen von Azure Storage-Konten in virtuellen Netzwerken).
- Informieren Sie sich über [Securing Azure SQL Database to Virtual networks](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview) (Schützen von Azure SQL-Datenbank in virtuellen Netzwerken).
- [Azure service integration overview for virtual networks](virtual-network-for-azure-services.md) (Übersicht über die Azure-Dienstintegration für virtuelle Netzwerke)



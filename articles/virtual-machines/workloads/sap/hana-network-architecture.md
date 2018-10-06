---
title: Netzwerkarchitektur von SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Netzwerkarchitektur zur Bereitstellung von SAP HANA in Azure (große Instanzen).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1262ed841fe8f6f9c2d5339d79abf06c1ab15a25
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392872"
---
# <a name="sap-hana-large-instances-network-architecture"></a>SAP HANA-Netzwerkarchitektur (große Instanzen)

Die Architektur von Azure-Netzwerkdiensten ist eine wichtige Komponente der erfolgreichen Bereitstellung von SAP-Anwendungen unter HANA (große Instanz). In der Regel weisen Bereitstellungen von SAP HANA in Azure (große Instanzen) eine größere SAP-Landschaft mit mehreren verschiedenen SAP-Lösungen mit Datenbanken unterschiedlicher Größe sowie variierender CPU-Ressourcen- und Arbeitsspeichernutzung auf. Wahrscheinlich basieren nicht all diese SAP-Systeme auf SAP HANA. Ihre SAP-Landschaft ist wahrscheinlich eine hybride Landschaft, die Folgendes umfasst:

- Lokal bereitgestellte SAP-Systeme. Aufgrund ihrer Größe können diese Systeme derzeit nicht in Azure gehostet werden. Ein Beispiel ist ein SAP ERP-Produktionssystem, das auf SQL Server (als Datenbank) ausgeführt wird und mehr CPU- oder Arbeitsspeicherressourcen erfordert als VMs bereitstellen können.
- Lokal bereitgestellte, auf SAP HANA basierende SAP-Systeme.
- Auf VMs bereitgestellte SAP-Systeme. Bei diesen Systemen kann es sich um Entwicklungs-, Test-, Sandbox- oder Produktionsinstanzen beliebiger SAP NetWeaver-basierter Anwendungen handeln, die je nach Ressourcennutzung und Arbeitsspeicherbedarf erfolgreiche in Azure (auf VMs) bereitgestellt werden können. Diese Systeme können auch auf Datenbanken wie SQL Server basieren. Weitere Informationen finden Sie im [SAP-Hinweis 1928533 – SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen](https://launchpad.support.sap.com/#/notes/1928533/E). Diese Systeme können auch auf Datenbanken wie SAP HANA basieren. Weitere Informationen finden Sie unter [SAP HANA certified IaaS platforms](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html) (SAP HANA zertifizierte IaaS-Plattformen).
- In Azure (auf VMs) bereitgestellte SAP-Anwendungsserver, die SAP HANA in Azure (große Instanzen) in Azure-Umfeldern der großen Instanz nutzen.

Eine hybride SAP-Landschaft mit vier oder mehr unterschiedlichen Bereitstellungsszenarien ist typisch. Es gibt auch viele Kunden, die vollständige SAP-Landschaften in Azure ausführen. Da VMs immer leistungsstärker werden, nimmt die Anzahl von Kunden zu, die ihre sämtlichen SAP-Lösungen zu Azure migrieren.

Das Azure-Netzwerk im Kontext von in Azure bereitgestellten SAP-Systemen ist nicht kompliziert. Es beruht auf den folgenden Prinzipien:

- Virtuelle Azure-Netzwerke müssen mit der ExpressRoute-Leitung verbunden sein, die eine Verbindung mit dem lokalen Netzwerk herstellt.
- Eine ExpressRoute-Leitung, die lokale Systeme mit Azure verbindet, sollte in der Regel über eine Bandbreite von mindestens 1 GBit/s verfügen. Diese minimale Bandbreite ist ausreichend für die Übertragung von Daten zwischen lokalen Systemen und auf VMs ausgeführten Systemen. Sie bietet auch eine angemessene Bandbreite für die Verbindung von lokalen Benutzern mit Azure-Systemen.
- Alle SAP-Systeme in Azure müssen in virtuellen Netzwerken eingerichtet werden, um miteinander kommunizieren zu können.
- Die lokal gehostete Active Directory-Instanz und der lokale DNS-Dienst werden über ExpressRoute von der lokalen Umgebung auf Azure ausgedehnt.


> [!NOTE] 
> Hinsichtlich der Abrechnung kann nur ein Azure-Abonnement mit einem einzigen Mandanten in einem Umfeld der großen Instanz in einer bestimmten Azure-Region verknüpft werden. Umgekehrt kann ein einziger Mandant im Umfeld der großen Instanz nur mit einem Azure-Abonnement verknüpft werden. Diese Anforderung gilt auch für andere abrechenbare Objekte in Azure.

Wenn SAP HANA in Azure (große Instanzen) in mehreren verschiedenen Azure-Regionen bereitgestellt wird, wird ein separater Mandant im Umfeld der großen Instanz bereitgestellt. Sie können beide Mandanten im gleichen Azure-Abonnement ausführen, solange diese Instanzen zur gleichen SAP-Landschaft gehören. 

> [!IMPORTANT] 
> Für SAP HANA in Azure (große Instanzen) wird nur die Azure Resource Manager-Bereitstellung unterstützt.

 

## <a name="additional-virtual-network-information"></a>Zusätzliche Informationen zum virtuellen Netzwerk

Zum Verbinden eines virtuellen Netzwerks mit ExpressRoute muss ein Azure-Gateway erstellt werden. Weitere Informationen finden Sie unter [Informationen zu Gateways für virtuelle Netzwerke für ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Ein Azure-Gateway kann mit einer ExpressRoute-Verbindung mit einer Infrastruktur außerhalb von Azure oder mit einem Azure-Umfeld der großen Instanz verwendet werden. Ein Azure-Gateway kann auch verwendet werden, um eine Verbindung zwischen virtuellen Netzwerken herzustellen. Weitere Informationen finden Sie im Artikel zum [Konfigurieren einer Verbindung zwischen Netzwerken für Resource Manager mithilfe von PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können das Azure-Gateway mit bis zu vier verschiedenen ExpressRoute-Verbindungen verbinden, solange diese von unterschiedlichen Microsoft-Unternehmensedgeroutern stammen. Weitere Informationen finden Sie unter [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Der von einem Azure-Gateway bereitgestellte Durchsatz unterscheidet sich für die beiden Anwendungsfälle. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Der maximale Durchsatz, den Sie über eine ExpressRoute-Verbindung mit einem virtuellen Netzwerkgateway erreichen können, beträgt 10 GBit/s. Beim Kopieren von Dateien zwischen einer VM in einem virtuellen Netzwerk und einem lokalen System (als einzelner Kopierdatenstrom) wird nicht der volle Durchsatz der anderen Gateway-SKUs erzielt. Verwenden Sie mehrere Datenströme, um die vollständige Bandbreite des virtuellen Netzwerkgateways zu nutzen. Andernfalls müssen Sie verschiedene Dateien in parallelen Datenströmen einer einzelnen Datei kopieren.


## <a name="networking-architecture-for-hana-large-instance"></a>Netzwerkarchitektur für HANA (große Instanz)
Die Netzwerkarchitektur für HANA (große Instanz) kann in vier verschiedene Teile unterteilt werden:

- Lokales Netzwerk und ExpressRoute-Verbindung mit Azure. Dieser Teil ist die Domäne des Kunden und wird über ExpressRoute mit Azure verbunden. Er ist unten rechts in der folgenden Abbildung dargestellt.
- Azure-Netzwerkdienste (wie zuvor erläutert) mit virtuellen Netzwerken, die wiederum über Gateways verfügen. Dieser Teil ist ein Bereich, für den Sie die geeigneten Entwürfe für Ihre Anwendungs-, Sicherheits- und Konformitätsanforderungen finden müssen. Ob Sie HANA (große Instanz) verwenden, ist ein weiterer Aspekt, der im Hinblick auf die Auswahl der Anzahl virtueller Netzwerke und Azure-Gateway-SKUs zu berücksichtigen ist. Dieser Teil ist oben rechts in der Abbildung dargestellt.
- Konnektivität von HANA (große Instanz) über ExpressRoute-Technologie in Azure. Diesem Teil wird von Microsoft bereitgestellt und gehandhabt. Sie müssen lediglich nach der Bereitstellung Ihrer Ressourcen in HANA (große Instanz) einige IP-Adressbereiche angeben, um die ExpressRoute-Leitung mit den virtuellen Netzwerken zu verbinden. Weitere Informationen finden Sie unter [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- Netzwerk in HANA (große Instanz), das für Sie größtenteils transparent ist.

![Mit SAP HANA in Azure (große Instanzen) und lokal verbundenes virtuelles Netzwerk](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Die Anforderung, dass Ihre lokalen Ressourcen über ExpressRoute mit Azure verbunden werden müssen, ändert sich durch die Verwendung von HANA (große Instanz) nicht. Die Notwendigkeit von mindestens einem virtuellen Netzwerk zur Ausführung der VMs, die die Anwendungsschicht hosten, die mit den in Einheiten von HANA (große Instanz) gehosteten HANA-Instanzen verbunden sind, ändert sich ebenfalls nicht. 

Für SAP-Bereitstellungen in Azure gelten folgende Unterschiede:

- Die Einheiten von HANA (große Instanz) Ihres Kundenmandanten sind über eine andere ExpressRoute-Leitung mit Ihren virtuellen Netzwerken verbunden. Zur Trennung der Ladebedingungen nutzen die ExpressRoute-Verbindungen zwischen lokalen Systemen und virtuellen Netzwerken und die Verbindungen zwischen virtuellen Netzwerken und HANA (große Instanz) nicht die gleichen Router.
- Das Workloadprofil zwischen der SAP-Anwendungsschicht und HANA (große Instanz) unterscheidet sich insofern, dass viele kleine Anforderungen und Bursts wie Datenübertragungen (Resultsets) von SAP HANA an die Anwendungsschicht gesendet werden.
- Die SAP-Anwendungsarchitektur ist empfindlicher gegenüber der Netzwerklatenz als typische Szenarien, in denen Daten zwischen lokalen Systemen und Azure ausgetauscht werden.
- Das virtuelle Netzwerkgateway verfügt über mindestens zwei ExpressRoute-Verbindungen. Beide Verbindungen nutzen gemeinsam die maximale Bandbreite für eingehende Daten des virtuellen Netzwerkgateways.

Die Netzwerklatenz zwischen VMs und Einheiten von HANA (große Instanz) kann höher sein als die typische Roundtriplatenz in einem Netzwerk zwischen VMs. Je nach Azure-Region können die gemessenen Werte die Roundtriplatenz von 0,7 ms übersteigen, die im [SAP-Hinweis 1100926 – Häufig gestellte Fragen: Netzwerkleistung](https://launchpad.support.sap.com/#/notes/1100926/E) als unterdurchschnittlich klassifiziert ist. Je nach Azure-Region und Tool zum Messen der Netzwerkroundtrip-Wartezeit zwischen einem virtuellen Azure-Computer und einer HANA-Einheit (große Instanzen) kann die gemessene Wartezeit bis zu ca. 2 Millisekunden betragen. Trotzdem stellen Kunden SAP HANA-basierte SAP-Produktionsanwendungen erfolgreich unter SAP HANA (große Instanz) bereit. Sie sollten Ihre Geschäftsprozesse unbedingt gründlich in SAP HANA in Azure (große Instanzen) testen.
 
Die Auswahl der SKU für das virtuelle Netzwerkgateway ist entscheidend, um eine deterministische Netzwerklatenz zwischen VMs und HANA (große Instanz) bereitzustellen. Im Gegensatz zu den Datenverkehrsmustern zwischen lokalen Systemen und VMs kann das Datenverkehrsmuster zwischen VMs und HANA (große Instanz) kleine, aber hohe Bursts von zu übertragenden Anforderungen und Datenvolumen hervorbringen. Damit solche Bursts korrekt verarbeitet werden, empfehlen wir die Verwendung der UltraPerformance-Gateway-SKU. Für die Typ-II-Klasse-SKUs von HANA (große Instanz) muss die UltraPerformance-Gateway-SKU als virtuelles Netzwerkgateway verwendet werden.

> [!IMPORTANT] 
> Beim gesamten Netzwerkdatenverkehr zwischen den SAP-Anwendungs- und Datenbankschichten werden zum Herstellen einer Verbindung mit SAP HANA in Azure (große Instanzen) nur die HighPerformance- oder UltraPerformance-Gateway-SKUs für virtuelle Netzwerke unterstützt. Für Typ-II-SKUs von HANA (große Instanz) wird nur die UltraPerformance-Gateway-SKU als virtuelles Netzwerkgateway unterstützt.



## <a name="single-sap-system"></a>Einzelnes SAP-System

Die zuvor gezeigte lokale Infrastruktur ist über ExpressRoute mit Azure verbunden. Die ExpressRoute-Leitung ist mit einem Unternehmensedgerouter verbunden. Weitere Informationen finden Sie in der [technischen Übersicht über ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nachdem die Route eingerichtet wurde, stellt sie eine Verbindung mit dem Azure-Backbone her, und es kann auf alle Azure-Regionen zugegriffen werden.

> [!NOTE] 
> Stellen Sie zum Ausführen von SAP-Landschaften in Azure eine Verbindung mit dem Unternehmensedgerouter her, der der Azure-Region in der SAP-Landschaft am nächsten ist. Azure-Umfelder der großen Instanz werden über dedizierte Unternehmensedgerouter-Geräte verbunden, um die Netzwerklatenz zwischen VMs in Azure-IaaS und Umfeldern der großen Instanz zu minimieren.

Das virtuelle Netzwerkgateway für die VMs, die die SAP-Anwendungsinstanzen hosten, ist mit der ExpressRoute-Leitung verbunden. Dasselbe virtuelle Netzwerk ist mit einem separaten Unternehmensedgerouter verbunden, der zum Herstellen einer Verbindung mit Umfeldern der großen Instanz vorgesehen ist.

Dieses System ist ein einfaches Beispiel für ein einzelnes SAP-System. Die SAP-Anwendungsschicht wird in Azure gehostet. Die SAP HANA-Datenbank wird in SAP HANA in Azure (große Instanzen) ausgeführt. Dabei wird davon ausgegangen, dass die Bandbreite des virtuellen Netzwerkgateways mit einem Durchsatz von 2 GBit/s oder 10 GBit/s keinen Engpass darstellt.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Mehrere SAP-Systeme oder große SAP-Systeme

Wenn mehrere SAP-Systeme oder große SAP-Systeme zur Verbindung mit SAP HANA in Azure (große Instanzen) bereitgestellt werden, kann sich der Durchsatz des virtuellen Netzwerkgateways als Engpass erweisen. Teilen Sie in diesem Fall die Anwendungsschichten auf mehrere virtuelle Netzwerke auf. In Fällen wie den folgenden können Sie auch ein spezielles virtuelles Netzwerk erstellen, das mit HANA (große Instanz) verbunden wird:

- Direktes Ausführen von Sicherungen über die HANA-Instanzen in HANA (große Instanz) auf einer VM, die NFS-Freigaben hostet.
- Kopieren großer Sicherungen oder anderer Dateien aus Einheiten von HANA (große Instanzen) auf Festplattenspeicher, der in Azure verwaltet wird.

Verwenden Sie ein separates virtuelles Netzwerk zum Hosten von VMs, die Speicher verwalten. Dadurch vermeiden Sie, dass sich große Datei- oder Datenübertragungen von HANA (große Instanz) an Azure auf das virtuelle Netzwerkgateway für die VMs auswirken, die die SAP-Anwendungsschicht ausführen. 

Gehen Sie für eine stärker skalierbare Netzwerkarchitektur folgendermaßen vor:

- Nutzen Sie mehrere virtuelle Netzwerke für eine einzelne, größere SAP-Anwendungsschicht.
- Stellen Sie für jedes bereitgestellte SAP-System ein separates virtuelles Netzwerk bereit, anstatt diese SAP-Systeme in separaten Subnetzen unter demselben virtuellen Netzwerk zu kombinieren.

 Eine stärker skalierbare Netzwerkarchitektur für SAP HANA in Azure (große Instanzen):

![Bereitstellen einer SAP-Anwendungsschicht über mehrere virtuelle Netzwerke](./media/hana-overview-architecture/image4-networking-architecture.png)

Die Abbildung zeigt die Bereitstellung der SAP-Anwendungsschicht bzw. -Komponenten über mehrere virtuelle Netzwerke. Diese Konfiguration führt zu einer unvermeidbaren höheren Wartezeit während der Kommunikation zwischen den in diesen virtuellen Netzwerken gehosteten Anwendungen. Der Netzwerkdatenverkehr zwischen VMs in verschiedenen virtuellen Netzwerken wird in dieser Konfiguration standardmäßig über die Unternehmensedgerrouter geleitet. Die Methode zum Optimieren und Verkürzen der Wartezeit bei der Kommunikation zwischen zwei virtuellen Netzwerken ist das Peering virtueller Netzwerke innerhalb derselben Region. Diese Methode funktioniert auch, wenn die virtuellen Netzwerke in verschiedenen Abonnements enthalten sind. Beim Peering virtueller Netzwerke kann für die Kommunikation zwischen VMs in zwei verschiedenen virtuellen Netzwerken der Azure-Netzwerkbackbone zur direkten Kommunikation verwendet werden. Die Wartezeit ist mit der bei VMs im selben virtuellen Netzwerk identisch. Datenverkehr an IP-Adressbereiche, die über das virtuelle Azure-Netzwerkgateway verbunden sind, wird über das individuelle virtuelle Netzwerkgateway des virtuellen Netzwerks weitergeleitet. 

Weitere Informationen zum Peering virtueller Netzwerke finden Sie unter [Peering in virtuellen Netzwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


## <a name="routing-in-azure"></a>Routing in Azure

Es gibt drei wichtige Überlegungen zum Netzwerkrouting für SAP HANA in Azure (große Instanzen):

* SAP HANA in Azure (große Instanzen) ist nur über VMs und die dedizierte ExpressRoute-Verbindung und nicht direkt vom lokalen System aus zugänglich. Der direkte Zugriff vom lokalen System auf die Einheiten von HANA (große Instanz) ist in der von Microsoft bereitgestellten Konfiguration nicht sofort möglich. Die Einschränkungen beim transitiven Routing sind auf die aktuelle Azure-Netzwerkarchitektur zurückzuführen, die für SAP HANA (große Instanz) verwendet wird. Manche Verwaltungsclients und alle Anwendungen, die direkten Zugriff erfordern (beispielsweise eine lokale Instanz von SAP Solution Manager), können keine Verbindung mit der SAP HANA-Datenbank herstellen.

* Wenn Sie für die Notfallwiederherstellung Einheiten von HANA (große Instanz) in zwei verschiedenen Azure-Regionen bereitgestellt haben, gelten dieselben vorübergehenden Routingeinschränkungen. Mit anderen Worten: Die IP-Adressen einer Einheit von HANA (große Instanz) in einer Region (z. B. „USA, Westen“) werden nicht an eine Einheit von HANA (große Instanz) in einer anderen Region (z.B. „USA, Osten“) weitergeleitet. Diese Einschränkung gilt unabhängig von der Verwendung des Azure-Netzwerkpeerings zwischen Regionen oder der Querverbindung von ExpressRoute-Leitungen, die Einheiten von HANA (große Instanz) mit virtuellen Netzwerken verbinden. Eine grafische Darstellung finden Sie in der Abbildung im Abschnitt „Verwenden von Einheiten von HANA (große Instanz) in mehreren Regionen“. Diese Einschränkung, die auf die bereitgestellte Architektur zurückzuführen ist, verhindert die unmittelbare Verwendung der HANA-Systemreplikation als Funktionalität zur Notfallwiederherstellung.

* Einheiten von SAP HANA in Azure (große Instanzen) verfügen über eine zugewiesene IP-Adresse aus dem IP-Adressbereich des Serverpools, den Sie angegeben haben. Weitere Informationen finden Sie unter [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Der Zugriff auf diese IP-Adresse ist über die Azure-Abonnements und die ExpressRoute-Verbindung möglich, die virtuelle Netzwerke mit HANA in Azure (große Instanzen) verbindet. Die zugewiesene IP-Adresse aus dem IP-Adressbereich des Serverpools wird der Hardwareeinheit direkt zugewiesen. Anders als bei den ersten Bereitstellungen dieser Lösung wird sie *nicht* mehr über NAT zugewiesen. 

> [!NOTE] 
> Verwenden Sie zusätzliche Komponenten für das Routing, um die in den ersten beiden Listeneinträgen erläuterte Einschränkung beim vorübergehenden Routing zu umgehen. Zu diesem Zweck können folgende Komponenten verwendet werden:

> * Ein Reverseproxy zum Weiterleiten von Daten in beide Richtungen. Beispiel: F5 BIG-IP, NGINX mit Traffic Manager, in Azure als virtuelle Firewall-/Datenverkehrsroutinglösung bereitgestellt.
> * [IPTables-Regeln](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) auf einer Linux-VM, um das Routing zwischen lokalen Standorten und Einheiten von HANA (große Instanz) oder zwischen Einheiten von HANA (große Instanz) in unterschiedlichen Regionen zu ermöglichen.

> Beachten Sie, dass Microsoft keine Implementierung und keinen Support für benutzerdefinierte Lösungen mit Netzwerkgeräten von Drittanbietern oder IPTables bietet. Der Support muss durch den Hersteller der verwendeten Komponente oder den Integrator bereitgestellt werden. 

## <a name="internet-connectivity-of-hana-large-instance"></a>Internetkonnektivität von HANA (große Instanz)
HANA (große Instanz) verfügt *nicht* über direkte Internetkonnektivität. Daher können Sie das Betriebssystemimage möglicherweise nicht direkt beim Hersteller des Betriebssystems registrieren. Gegebenenfalls müssen Sie dazu den lokalen SUSE Linux Enterprise Server Subscription Management Tool-Server oder Red Hat Enterprise Linux Subscription Manager verwenden.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>Datenverschlüsselung zwischen VMs und HANA (große Instanz)
Zwischen HANA (große Instanz) und VMs übertragene Daten werden nicht verschlüsselt. Sie können jedoch für den Austausch zwischen der HANA DBMS-Seite und JDBC-/ODBC-basierten Anwendungen die Verschlüsselung des Datenverkehrs aktivieren. Weitere Informationen finden Sie in [dieser Dokumentation von SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>Verwenden von Einheiten von HANA (große Instanz) in mehreren Regionen

Möglicherweise haben Sie neben der Notfallwiederherstellung auch andere Gründe, SAP HANA in Azure (große Instanzen) in mehreren Azure-Regionen bereitzustellen. Vielleicht möchten Sie von jeder der VMs, die in den verschiedenen virtuellen Netzwerken in den Regionen bereitgestellt werden, auf HANA (große Instanz) zugreifen. Die zugewiesenen IP-Adressen der verschiedenen Einheiten von HANA (große Instanz) werden nicht über die virtuellen Netzwerke, die direkt über ihr Gateway mit den Instanzen verbunden sind, hinaus verteilt. Dies führt zu einer geringfügigen Änderung im Entwurf des virtuellen Netzwerks. Ein virtuelles Netzwerkgateway kann vier verschiedene ExpressRoute-Leitungen von verschiedenen Unternehmensedgeroutern unterstützen. Jedes virtuelle Netzwerk, das mit einem der Umfelder von HANA (große Instanz) verbunden ist, kann mit dem Umfeld von HANA (große Instanz) in einer anderen Azure-Region verbunden werden.

![Mit Azure-Umfeldern der großen Instanz in verschiedenen Azure-Regionen verbundene virtuelle Netzwerke](./media/hana-overview-architecture/image8-multiple-regions.png)

Die Abbildung zeigt, wie die verschiedenen virtuellen Netzwerke in beiden Regionen mit zwei verschiedenen ExpressRoute-Leitungen verbunden sind, die zum Herstellen einer Verbindung mit SAP HANA in Azure (große Instanzen) in beiden Azure-Regionen verwendet werden. Die neu eingeführten Verbindungen werden durch die rechteckigen roten Linien dargestellt. Mit diesen ausgehenden Verbindungen der virtuellen Netzwerke können die in einem dieser virtuellen Netzwerke ausgeführten VMs auf jede der in den beiden Regionen bereitgestellten verschiedenen Einheiten von HANA (große Instanz) zugreifen. Wie die Abbildung zeigt, werden dabei zwei ExpressRoute-Verbindungen zwischen den lokalen Systemen und den beiden Azure-Regionen vorausgesetzt. Diese Konfiguration wird aus Gründen der Notfallwiederherstellung empfohlen.

> [!IMPORTANT] 
> Falls Sie mehrere ExpressRoute-Leitungen verwendet haben, sollten das Voranstellen von AS Path und lokale BGP-Einstellungen verwendet werden, um ein korrektes Routing des Datenverkehrs sicherzustellen.

**Nächste Schritte**
- Siehe [SAP HANA-Speicherarchitektur (große Instanzen)](hana-storage-architecture.md)
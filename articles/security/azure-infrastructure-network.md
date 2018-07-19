---
title: Azure-Netzwerkarchitektur
description: Dieser Artikel enthält eine allgemeine Beschreibung des Microsoft Azure-Infrastrukturnetzwerks.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 67781f196b445c9330e0dcb1fc7d8b0a1a53cbc0
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101432"
---
# <a name="azure-network-architecture"></a>Azure-Netzwerkarchitektur
Die Netzwerkarchitektur von Azure beruht auf einer geänderten Version des als Branchenstandard geltenden Kern-, Verteilungs- und Zugriffsmodells und verfügt über unterschiedliche Hardwareschichten (Layer). Zu diesen Layern zählen:

- Core-oder Kernlayer (Rechenzentrumrouter)
- Distribution- oder Verteilungslayer (Accessrouter und L2-Aggregation): Der Verteilungslayer trennt L3-Routing von L2-Switching.
- Access- oder Zugriffslayer (L2-Host-Switches)

Die Netzwerkarchitektur verfügt über zwei verschiedene Layer-2-Switches. Ein Layer-2-Switch aggregiert Datenverkehr vom anderen, während der andere Schleifen erzeugt, um Redundanz zu integrieren. Dies bietet Vorteile wie einen flexibleren VLAN-Speicherbedarf und verbessert die Portskalierung. Die Architektur sorgt für eine klare Trennung zwischen L2 und L3 und ermöglicht dadurch die Verwendung von Hardware in jedem einzelnen unterschiedlichen Layer im Netzwerk. Fehler auf einem Layer, die sich auf die anderen Layer auswirken, können so minimiert werden. Mithilfe von Trunks lassen sich Ressourcen freigeben, wie z.B. die Konnektivität zur L3-Infrastruktur.

## <a name="network-configuration"></a>Network Configuration
Die Netzwerkarchitektur eines Microsoft Azure-Clusters in einem Rechenzentrum besteht aus den folgenden Geräten:

- Router (Rechenzentrum-, Access- und Leafrouter am Netzwerkrand)
- Switches (Aggregation- und Top-of-Rack-Switches)
- Digi-CMs
- PDUs

Die folgende Abbildung bietet einen allgemeinen Überblick über die Azure-Netzwerkarchitektur in einem Cluster. Azure verfügt über zwei getrennte Architekturen. Einige Azure-Kunden und die gemeinsamen Dienste befinden sich auf der Standard-LAN-Architektur (DLA), während der Zugriff auf neue Regionen und virtuelle Kunden über die Quantum-10-Architektur (Q10) erfolgt. Die DLA-Architektur ist ein herkömmliches Strukturdesign mit Aktiv-Passiv-Accessroutern und Sicherheits-ACLs, die auf die Accessrouter angewendet werden. Die Quantum-10-Architektur ist ein vermaschtes Routerdesign (Clos-Architektur). Dabei werden ACLs nicht bei den Routern angewendet, sondern unterhalb der Routingebene mithilfe von Software Load Balancing (SLB) oder softwaredefinierten VLANs.

![Azure-Netzwerk][1]

### <a name="quantum-10-devices"></a>Quantum-10-Geräte
Beim Quantum-10-Design wird Layer-3-Switching über mehrere Geräte verteilt in einem CLOS-/Mesh-Netzwerk durchgeführt. Zu den Vorteilen des Q10-Designs gehören mehr Funktionen und mehr Möglichkeiten zum Skalieren der vorhandenen Netzwerkinfrastruktur. Das Design verwendet Leafrouter am Netzwerkrand, Spineswitches und Top-of-Rack-Router, um Datenverkehr über mehrere Routen an den Cluster zu übergeben und Fehlertoleranz zu ermöglichen. Sicherheitsdienste wie die Netzwerkadressübersetzung werden über den Lastenausgleich für Software anstatt über Hardwaregeräte ausgeführt.

### <a name="access-routers"></a>Accessrouter
Die Distribution-/Access-L3-Router (ARs) übernehmen das Hauptrouting für die Distribution- und Accesslayer. Diese Geräte werden zusammen bereitgestellt und sind das Standardgateway für Subnetze. Jedes AR-Paar kann je nach Kapazität mehrere L2-Aggregation-Switch-Paare unterstützen. Die maximale Anzahl hängt sowohl von der Kapazität des Geräts als auch den Fehlerdomänen ab. Als typische Anzahl auf der Basis der erwarteten Kapazität gelten drei L2-Aggregation-Switch-Paare pro AR-Paar.

### <a name="l2-aggregation-switches"></a>L2-Aggregation-Switches  
Diese Geräte dienen als Sammelpunkt für L2-Datenverkehr. Sie sind der Distributionlayer für das L2-Fabric und können große Mengen an Datenverkehr verarbeiten. Da diese Geräte Datenverkehr aggregieren, werden 802.1Q-Funktionalität und Technologien mit hoher Bandbreite vorausgesetzt, wie z.B. Portaggregation und 10GbE.

### <a name="l2-host-switches"></a>L2-Hostswitches
Hosts erstellen eine direkte Verbindung zu diesen Switches. Dabei kann es sich um im Rack eingebaute Switches oder Bereitstellungen für Gehäuse handeln. Der 802.1Q-Standard ermöglicht das Definieren eines VLANs als natives VLAN, das als normales (nicht gekennzeichnetes) Ethernet-Frame behandelt wird. Unter normalen Umständen werden Frames auf dem nativen VLAN an einen Trunkport gemäß 802. 1Q-Standard ohne Tags übertragen und empfangen. Dieses Feature wurde für die Migration zu 802. 1Q entwickelt und ist auch mit Geräten kompatibel, die nicht den 802.1Q-Standard erfüllen. In dieser Architektur verwendet nur die Netzwerkinfrastruktur das native VLAN.

Diese Architektur gibt einen Standard für eine native VLAN-Auswahl an. Dadurch wird, je nach Möglichkeit, sichergestellt, dass die AR-Geräte über ein eindeutiges natives VLAN für jeden Trunk und die L2-Aggregation-zu-L2-Aggregation-Trunks verfügen. Die L2-Aggregation-zu-L2-Host-Switch-Trunks verfügen über ein nicht standardmäßiges natives VLAN.

### <a name="link-aggregation-8023ad"></a>Link Aggregation (802.3ad)
Mithilfe von Link Aggregation (LAG) können mehrere einzelne Links gebündelt und als ein einziger logischer Link behandelt werden. Die Zahl, die zum Kennzeichnen von Portkanalschnittstellen verwendet wurde, muss standardisiert werden, um das operationale Debuggen zu vereinfachen. Das übrige Netzwerk verwendet die gleiche Zahl an beiden Seiten eines Portkanals. Dies erfordert einen Standard, der vorgibt, welche Seite des Portkanals verwendet werden soll, um die als Nächstes verfügbare Zahl zu definieren.

Die Zahlen für den L2-Aggregation-zu-L2-Host-Switch sind die Portkanalzahlen, die auf der L2-Aggregation-Seite verwendet werden. Da die Zahlenauswahl auf der L2-Host-Seite stärker eingeschränkt ist, werden standardmäßig die Zahlen 1 und 2 auf dem L2-Host verwendet, um auf den Portkanal zu verweisen, der jeweils auf die Seiten „a“ und „b“ wechselt.

### <a name="vlans"></a>VLANs
Die Netzwerkarchitektur verwendet VLANs, um Server zu einer einzigen Broadcastdomäne zu gruppieren. Die VLAN-Zahlen entsprechen dem 802.1Q-Standard, der VLANs mit der Nummerierung von 1 bis 4094 unterstützt.

### <a name="customer-vlans"></a>Kunden-VLANs
Kunden haben verschiedene Optionen zur VLAN-Implementierung, die sie über das Azure-Portal bereitstellen können, um die Anforderungen ihrer Lösung in Bezug auf Segmentierung und Architektur zu erfüllen. Diese Lösungen werden durch virtuelle Computer bereitgestellt. Beispiele für Kundenreferenzarchitektur finden Sie auf der Seite [Azure-Referenzarchitekturen](https://docs.microsoft.com/azure/architecture/reference-architectures/).

### <a name="edge-architecture"></a>Edgearchitektur
Azure-Rechenzentren basieren auf stark redundanten und gut bereitgestellten Netzwerkinfrastrukturen. Netzwerke innerhalb der Azure-Rechenzentren werden mit Redundanzarchitekturen bereitgestellt, die immer über den Anforderungen liegen. Vollständige Failoverfeatures innerhalb und zwischen Rechenzentren tragen dazu bei, die Netzwerk- und Dienstverfügbarkeit sicherzustellen. Extern werden Rechenzentren durch dedizierte Netzwerkverbindungen mit hoher Bandbreite unterstützt, die redundant Eigenschaften mit mehr als 1.200 Internetdienstanbietern global an mehreren Peeringpunkten verbinden und mehr als 2.000 Gbit an potenzieller Edgekapazität im gesamten Netzwerk zur Verfügung stellen.

Filterungsrouter auf dem Edge- und Accesslayer des Microsoft Azure-Netzwerks bieten anerkannte Sicherheit auf Paketebene und verhindern nicht autorisierte Versuche, eine Verbindung zu Azure herzustellen. Sie sorgen dafür, dass die Pakete auch tatsächlich Daten im erwarteten Format enthalten und das Client-/Server-Kommunikationsschema erfüllen. Azure implementiert eine mehrstufige Architektur, die aus den folgenden Netzwerkisolierungs- und Zugriffssteuerungskomponenten besteht:

- Edgerouter: Isolieren der Anwendungsumgebung vom Internet. Edgerouter bieten Spoofing-Schutz und beschränken den Zugriff mithilfe von Zugriffssteuerungslisten (ACLs).
- Distributionrouter: Accessrouter lassen nur durch Microsoft genehmigte IP-Adressen zu, bieten Spoofingschutz und anerkannte Verbindungen mithilfe von ACLs.

### <a name="a10-ddos-mitigation-architecture"></a>A10-DDoS-Architektur zur Risikominderung
DoS-Angriffe stellen weiterhin ein echtes Risiko für die Zuverlässigkeit der Microsoft-Onlinedienste dar. Angesichts immer gezielterer und ausgefeilterer Angriffe sowie der zunehmenden geografischen Diversifizierung von Microsoft-Diensten haben effiziente Mechanismen, die diese Angriffe identifizieren und ihre Auswirkungen reduzieren können, oberste Priorität.

Die folgenden Details erläutern, wie das A10-DDoS-System zur Risikominderung vom Standpunkt der Netzwerkarchitektur implementiert wird.  
Microsoft Azure nutzt A10-Netzwerkgeräte am DCR (Rechenzentrumsrouter), die automatische Erkennung und Risikominderung bieten. Die A10-Lösung verwendet die Azure-Netzwerküberwachung, um Netflow-Pakete stichprobenhaft zu überprüfen und zu ermitteln, ob ein Angriff stattfindet. Wenn ein Angriff erkannt wird, werden A10-Geräte zum Scrubbing eingesetzt, um das Risiko von Angriffen zu reduzieren. Danach wird der weitere ordnungsgemäße Datenverkehr direkt aus dem DCR an das Azure-Rechenzentrum weitergeleitet. Die A10-Lösung wird verwendet, um die Azure-Netzwerkinfrastruktur zu schützen.

Zu den DDoS-Schutzmechanismen der A10-Lösung gehören:

- UDP IPv4- und IPv6-Überlastungsschutz
- ICM IPv4- und IPv6-Überlastungsschutz
- TCP IPv4- und IPv6-Überlastungsschutz
- TCP SYN-Angriffsschutz für IPv4 und IPv6
- Fragmentierungsangriff

> [!NOTE]
> DDoS-Schutz steht Azure-Kunden standardmäßig zur Verfügung.
>
>

## <a name="network-connection-rules"></a>Netzwerkverbindungsregeln
Azure stellt Edgerouter im Netzwerk bereit, die für Sicherheit auf Paketebene sorgen, um nicht autorisierte Versuche zu verhindern, eine Verbindung zu Microsoft Azure herzustellen. Sie tragen dazu bei, dass die Pakete auch tatsächlich Daten im erwarteten Format enthalten und das Client-/Server-Kommunikationsschema erfüllen.

Edgerouter isolieren die Anwendungsumgebung vom Internet. Edgerouter bieten Spoofing-Schutz und beschränken den Zugriff mithilfe von Zugriffssteuerungslisten (ACLs). Diese Edgerouter werden mithilfe eines ACL-Stufenansatzes konfiguriert und beschränken Netzwerkprotokolle, die die Edge- und Accessrouter durchlaufen.

Netzwerkgeräte werden an Zugriffs- und Edgestandorten positioniert und fungieren als Grenzpunkte, auf die Eingangs- und Ausgangsfilter angewendet werden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Information dazu, wie Microsoft die Azure-Infrastruktur schützt, finden Sie in den folgenden Artikeln:

- [Azure facilities, premises, and physical security (Azure-Einrichtungen, -Gelände und physische Sicherheit)](azure-physical-security.md)
- [Availability of Azure infrastructure (Verfügbarkeit der Azure-Infrastruktur)](azure-infrastructure-availability.md)
- [Azure information system components and boundaries (Komponenten und Grenzen des Azure-Informationssystems)](azure-infrastructure-components.md)
- [Azure-Produktionsnetzwerk](azure-production-network.md)
- [Sicherheitsfeatures für Microsoft Azure SQL-Datenbank](azure-infrastructure-sql.md)
- [Azure production operations and management (Produktionsvorgänge und Verwaltung von Azure)](azure-infrastructure-operations.md)
- [Monitoring of Azure infrastructure (Überwachen der Azure-Infrastruktur)](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure (Integrität der Azure-Infrastruktur)](azure-infrastructure-integrity.md)
- [Protection of customer data in Azure (Schutz von Kundendaten in Azure)](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-network/network-arch.png

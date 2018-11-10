---
title: Was ist Azure Firewall?
description: Erfahren Sie mehr über Azure Firewall-Features.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 9/26/2018
ms.author: victorh
ms.openlocfilehash: 868c20e6f0244794299678214902adf3e6e95f14
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241411"
---
# <a name="what-is-azure-firewall"></a>Was ist Azure Firewall?

Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall als ein Dienst mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit. 

![Firewallübersicht](media/overview/firewall-overview.png)

Sie können Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren. Azure Firewall verwendet eine statische öffentliche IP-Adresse für Ihre virtuellen Netzwerkressourcen, die es außenstehenden Firewalls ermöglicht, Datenverkehr aus Ihrem virtuellen Netzwerk zu identifizieren.  Der Dienst ist für Protokollierung und Analyse vollständig in Azure Monitor integriert.

## <a name="features"></a>Features

Azure Firewall bietet die folgenden Features:

### <a name="built-in-high-availability"></a>Integrierte Hochverfügbarkeit
Hochverfügbarkeit ist integriert, sodass keine zusätzlichen Lastenausgleichsmodule erforderlich sind und Sie nichts konfigurieren müssen.

### <a name="unrestricted-cloud-scalability"></a>Uneingeschränkte Cloudskalierbarkeit 
Azure Firewall kann entsprechend Ihren Anforderungen zentral hochskaliert werden, um einem sich ändernden Netzwerkdatenverkehr zu entsprechen, sodass Sie nicht für Ihre Spitzenlasten budgetieren müssen.

### <a name="application-fqdn-filtering-rules"></a>FQDN-Anwendungsfilterregeln

Sie können den ausgehenden HTTP/S-Datenverkehr auf eine angegebene Liste vollständig qualifizierter Domänennamen (FQDN) einschließlich Platzhalter beschränken. Diese Funktion erfordert keine SSL-Beendigung.

### <a name="network-traffic-filtering-rules"></a>Filterregeln für den Netzwerkdatenverkehr

Sie können Netzwerkfilterregeln zum *Zulassen* oder *Verweigern* nach Quell- und Ziel-IP-Adresse, Port und Protokoll zentral erstellen. Azure Firewall ist vollständig zustandsbehaftet, sodass zwischen legitimen Paketen für verschiedene Arten von Verbindungen unterschieden werden kann. Regeln werden übergreifend für mehrere Abonnements und virtuelle Netzwerke erzwungen und protokolliert.

### <a name="fqdn-tags"></a>FQDN-Tags

FQDN-Tags erleichtern es Ihnen, Netzwerkdatenverkehr bekannter Azure-Dienste durch die Firewall zuzulassen. Angenommen, Sie möchten Netzwerkdatenverkehr von Windows Update durch die Firewall zulassen. Sie erstellen eine entsprechende Anwendungsregel, und schließen das Windows Update-Tag ein. Jetzt kann der Netzwerkdatenverkehr von Windows Update durch Ihre Firewall fließen.

### <a name="outbound-snat-support"></a>SNAT-Unterstützung für ausgehenden Datenverkehr

Alle IP-Adressen für ausgehenden Datenverkehr des virtuellen Netzwerks werden in die öffentliche IP-Adresse der Azure Firewall übersetzt (Source Network Address Translation). Sie können Datenverkehr aus Ihrem virtuellen Netzwerk an Remoteziele im Internet identifizieren und zulassen.

### <a name="inbound-dnat-support"></a>DNAT-Unterstützung für eingehenden Datenverkehr

Der eingehende Netzwerkdatenverkehr zur öffentlichen IP-Adresse Ihrer Firewall wird in die privaten IP-Adressen in Ihren virtuellen Netzwerken übersetzt (Destination Network Address Translation) und gefiltert. 

### <a name="azure-monitor-logging"></a>Azure Monitor-Protokollierung

Alle Ereignisse sind in Azure Monitor integriert, sodass Sie Protokolle in einem Speicherkonto archivieren sowie Ereignisse an Ihren Event Hub streamen oder an Log Analytics senden können.

## <a name="known-issues"></a>Bekannte Probleme

Azure Firewall weist die folgenden bekannten Probleme auf:


|Problem  |BESCHREIBUNG  |Lösung  |
|---------|---------|---------|
|Konflikt mit Just-in-Time (JIT)-Funktion von Azure Security Center (ASC)|Wenn auf einen virtuellen Computer mithilfe von JIT zugegriffen wird und sich dieser in einem Subnetz mit einer benutzerdefinierten Route befindet, die auf Azure Firewall als ein Standardgateway verweist, funktioniert ASC JIT nicht. Dies ist eine Folge des asymmetrischen Routings: Ein Paket trifft über die öffentliche IP-Adresse des virtuellen Computers ein (JIT hat den Zugriff geöffnet), aber der Rückgabepfad verläuft über die Firewall, die das Paket verwirft, da keine Sitzung in der Firewall erstellt wird.|Um dieses Problem zu umgehen, platzieren Sie die virtuellen JIT-Computer in einem separaten Subnetz, das keine benutzerdefinierte Route zur Firewall aufweist.|
|Hub und Spoke mit globalem Peering wird nicht unterstützt.|Das Hub- und Spoke-Modell, bei dem Hub und Firewall in einer Azure-Region und die Spokes in einer anderen Azure-Region bereitgestellt werden, wird nicht unterstützt. Verbindungen mit dem Hub über globales VNET-Peering werden nicht unterstützt.|Dies ist beabsichtigt. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen](../azure-subscription-service-limits.md#azure-firewall-limits).|
Netzwerkfilterregeln für andere Protokolle als TCP/UDP (z.B. ICMP) funktionieren nicht für den Internetdatenverkehr|Netzwerkfilterregeln für andere Protokolle als TCP/UDP funktionieren nicht mit SNAT für Ihre öffentliche IP-Adresse. Nicht-TCP/UDP-Protokolle werden zwischen Spoke-Subnetzen und VNets unterstützt.|Azure Firewall verwendet Standard Load Balancer, [das SNAT für IP-Protokolle derzeit nicht unterstützt](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#limitations). Wir prüfen Möglichkeiten, um dieses Szenario in einer zukünftigen Version zu unterstützen.|
|DNAT funktioniert nicht für Port 80 und Port 22.|Das Zielportfeld in der NAT-Regelsammlung kann Port 80 oder Port 22 nicht einbeziehen.|Wir arbeiten daran, dieses Problem in der nahen Zukunft zu beheben. Verwenden Sie in der Zwischenzeit einen anderen Port als Zielport in NAT-Regeln. Port 80 oder Port 22 kann weiterhin als übersetzter Port verwendet werden (z. B. können Sie „public ip:81“ und „private ip:80“ einander zuordnen).|
|Fehlende PowerShell- und CLI-Unterstützung für ICMP|Azure PowerShell und CLI weisen keine Unterstützung von ICMP als gültiges Protokoll in Netzwerkregeln auf.|Es ist weiterhin möglich, ICMP als Protokoll über das Portal und die REST-API zu verwenden. Wir arbeiten daran, ICMP in Kürze in PowerShell und CLI hinzuzufügen.|
|Für FQDN-Tags muss ein Protokoll/Port festgelegt werden|Für Anwendungsregeln mit FQDN-Tags ist eine port:-Protokolldefinition erforderlich.|Sie können **https** als port:-Protokollwert verwenden. Wir arbeiten daran, dieses Feld optional zu machen, wenn FQDN-Tags verwendet werden.|
|Das Verschieben einer Firewall in eine andere Ressourcengruppe oder ein Abonnement wird nicht unterstützt.|Das Verschieben einer Firewall in eine andere Ressourcengruppe oder ein Abonnement wird nicht unterstützt.|Die Unterstützung dieser Funktionalität ist Teil unserer Roadmap. Um eine Firewall in eine andere Ressourcengruppe oder ein anderes Abonnement zu verschieben, müssen Sie die aktuelle Instanz löschen und in der neuen Ressourcengruppe bzw. im Abonnement neu erstellen.|

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](tutorial-firewall-deploy-portal.md)
- [Bereitstellen von Azure Firewall mit einer Vorlage](deploy-template.md)
- [Erstellen einer Azure Firewall-Testumgebung](scripts/sample-create-firewall-test.md)


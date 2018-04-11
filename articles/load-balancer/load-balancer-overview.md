---
title: Übersicht über Azure Load Balancer | Microsoft-Dokumentation
description: Übersicht über Features, Architektur und Implementierung des Azure Load Balancers. Erfahren Sie, wie der Load Balancer funktioniert, und nutzen Sie ihn in der Cloud.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: f4410932f00f8505ae5a894caa002e1223196d95
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="azure-load-balancer-overview"></a>Übersicht über Azure Load Balancer

Mit Azure Load Balancer können Sie Ihre Anwendungen skalieren und Ihre Dienste mit Hochverfügbarkeit bereitstellen. Load Balancer unterstützt sowohl Szenarien mit eingehenden als auch ausgehenden Verbindungen, bietet niedrige Latenzen und einen hohen Durchsatz und unterstützt Millionen Datenflüsse für alle TCP- und UDP-Anwendungen.   

Load Balancer verteilt neue eingehende Datenströme, die beim Front-End des Load Balancers eintreffen, gemäß den Regeln und Integritätstests auf die Back-End-Poolinstanzen.  

Zusätzlich kann ein öffentlicher Load Balancer auch ausgehende Verbindungen für virtuelle Computer in Ihrem virtuellen Netzwerk bereitstellen, indem er deren private IP-Adressen in öffentliche IP-Adressen übersetzt.

Azure Load Balancer ist in zwei verschiedenen SKUs verfügbar: Basic und Standard.  Diese unterscheiden sich hinsichtlich Staffelung, Funktionen und Preisen.  Jedes Szenario, das mit Basic Load Balancer möglich ist, kann auch mit Standard Load Balancer erstellt werden, auch wenn sich die Vorgehensweise leicht unterscheiden kann.  Wenn Sie sich intensiver mit Load Balancer beschäftigen, sollten Sie sich mit den Grundlagen und den SKU-spezifischen Unterschieden vertraut machen.

## <a name="why-use-load-balancer"></a>Gründe für die Verwendung von Load Balancer 

Azure Load Balancer kann für Folgendes verwendet werden:

* Lastenausgleich des eingehenden Internetdatenverkehrs für virtuelle Computer. Diese Konfiguration wird als ein [öffentlicher Load Balancer](#publicloadbalancer) bezeichnet.
* Lastenausgleich für Datenverkehr zwischen virtuellen Computern in einem virtuellen Netzwerk. Sie können ein Load Balancer-Front-End auch aus einem lokalen Netzwerk in einem Hybridszenario erreichen. Für beide Szenarien wird eine Konfiguration verwendet, die als ein [interner Load Balancer](#internalloadbalancer) bezeichnet wird.
* Portweiterleitung des Datenverkehrs an einen bestimmten Port auf bestimmten virtuellen Computern mit NAT-Regeln für eingehenden Datenverkehr.
* Bereitstellung von [ausgehenden Verbindungen](load-balancer-outbound-connections.md) für virtuelle Computer in Ihrem virtuellen Netzwerk durch Verwenden eines öffentlichen Load Balancers.


>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarien bereit.  Wenn Sie nach Informationen zu TLS-Terminierung („SSL-Auslagerung“) oder Verarbeitung pro HTTP/HTTPS-Anforderungsanwendungsschicht suchen, lesen Sie [Application Gateway](../application-gateway/application-gateway-introduction.md).  Wenn Sie nach Informationen zu globalem DNS-Lastenausgleich suchen, lesen Sie [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Für Ihre End-to-End-Szenarien kann es vorteilhaft sein, diese Lösungen nach Bedarf zu kombinieren.

## <a name="what-is-load-balancer"></a>Was ist der Lastenausgleich?

Eine Load Balancer-Ressource (Lastenausgleichsressource) kann entweder als ein öffentlicher Load Balancer oder als ein interner Load Balancer vorhanden sein. Die Funktionen einer Load Balancer-Ressource werden als Front-End-, Regel-, Integritätstest- und Back-End-Pool-Definition ausgedrückt.  Virtuelle Computer werden im Back-End-Pool platziert, indem der Back-End-Pool vom virtuellen Computers angegeben wird.

Load Balancer-Ressourcen sind Objekte, in denen Sie formulieren können, wie Azure seine mehrinstanzenfähige Infrastruktur programmieren sollte, um das Szenario zu erzielen, das Sie erstellen möchten.  Es gibt keine direkte Beziehung zwischen Load Balancer-Ressourcen und tatsächlicher Infrastruktur. Mit dem Erstellen eines Load Balancers wird keine Instanz erstellt, und die Kapazität ist immer verfügbar. 

## <a name="fundamental-load-balancer-features"></a>Grundlegende Load Balancer-Features

Load Balancer stellt die folgenden grundlegenden Funktionen für TCP und UDP-Anwendungen bereit:

* **Lastenausgleich**

    Mit Azure Load Balancer können Sie eine Lastausgleichsregel erstellen, gemäß der Datenverkehr, der bei einem Front-End eintrifft, auf Back-End-Poolinstanzen verteilt wird.  Load Balancer verwendet einen hashbasierten Algorithmus für die Verteilung von eingehenden Datenflüssen und schreibt die Header der Datenflüsse so um, dass sie den Back-End-Poolinstanzen entsprechen. Wenn der Integritätstest einen fehlerfreien Back-End-Endpunkt angibt, ist ein Server verfügbar, der neue Datenflüsse empfangen kann.
    
    Standardmäßig wird ein Fünf-Tupel-Hash (IP-Quelladresse, Quellport, IP-Zieladresse, Zielport und IP-Protokollnummer) verwendet, um Datenflüsse den verfügbaren Servern zuzuordnen.  Sie können eine Affinität zu einer bestimmten IP-Quelladresse erstellen, indem Sie für eine bestimmte Regel zu einem Zwei- oder Drei-Tupel-Hash wechseln.  Alle Pakete desselben Paketdatenflusses treffen bei derselben Instanz hinter dem Front-End mit Lastenausgleich ein.  Wenn der Client einen neuen Datenfluss von derselben IP-Quelladresse initiiert, wird der Quellport gewechselt. Das resultierende Fünf-Tupel kann dazu führen, dass der Datenverkehr an einen anderen Back-End-Endpunkt gesendet wird.

    Weitere Informationen hierzu finden Sie unter [Load Balancer-Verteilungsmodus](load-balancer-distribution-mode.md). Die folgende Grafik zeigt die hashbasierte Verteilung:

    ![Hash-basierte Verteilung](./media/load-balancer-overview/load-balancer-distribution.png)

    *Abbildung: Hashbasierte Verteilung*

* **Portweiterleitung**

    Azure Load Balancer ermöglicht es Ihnen, eine NAT-Regel für eingehenden Datenverkehr zu erstellen, um den Datenverkehr von einem bestimmten Port einer bestimmten Front-End-IP-Adresse auf einen bestimmten Port einer bestimmten Back-End-Instanz im Microsoft Azure Virtual Network zu übertragen. Dies wird auch durch dieselbe hashbasierte Verteilung wie beim Lastausgleich erreicht.  Allgemeine Szenarien für diese Fähigkeit sind RDP- (Remote Desktop Protocol) oder SSH-Sitzungen (Secure Shell) für einzelne VM-Instanzen im Microsoft Azure Virtual Network.  Sie können den unterschiedliche Ports unter derselben Front-End-IP-Adresse mehrere interne Endpunkte zuordnen. Diese können Sie dazu verwenden, Ihre virtuellen Computer remote über das Internet zu verwalten, ohne dass Sie eine zusätzliche Jumpbox benötigen.

* **Anwendungsunabhängig und transparent**

    Load Balancer interagiert nicht direkt mit TCP oder UDP oder der Anwendungsschicht, und jedes TCP- oder UDP-basierte Anwendungsszenario kann unterstützt werden.  Zum Beispiel können Sie, weil Load Balancer TLS nicht selbst beendet, TLS-Anwendungen mit Load Balancer erstellen und horizontal skalieren und die TLS-Verbindung auf dem virtuellen Computer beenden. Load Balancer beendet keinen Datenfluss, und Protokoll-Handshakes werden immer direkt zwischen dem Client und der per Hash ausgewählten Back-End-Poolinstanz ausgeführt. Zum Beispiel wird ein TCP-Handshake immer zwischen dem Client und dem ausgewählten virtuellen Back-End-Computer ausgeführt.  Außerdem ist eine Antwort auf eine Anforderung an einen Front-End eine Antwort, die vom virtuellen Back-End-Computer generiert wurde.  Die Load Balancer-Netzwerkleistung für ausgehenden Datenverkehr ist nur durch die von Ihnen ausgewählte SKU eines virtuellen Computers begrenzt, und die Datenströme bleiben für lange Zeit aktiv, wenn das Leerlauftimeout nie erreicht wird.

* **Automatische Neukonfiguration**

    Der Azure Load Balancer konfiguriert sich selbst sofort, wenn Sie Instanzen nach oben oder unten skalieren. Ein Hinzufügen oder Entfernen von virtuellen Computern aus dem Back-End-Pool bewirkt, dass der Load Balancer neu konfiguriert wird, es werden aber keine zusätzlichen Vorgänge für die Load Balancer-Ressource ausgeführt.

* **Integritätstests**

    Azure Load Balancer verwendet von Ihnen definierte Integritätstests, um die Integrität von Instanzen im Back-End-Pool zu ermitteln. Wenn ein Test nicht reagiert, beendet der Load Balancer das Senden neuer Verbindungen an die fehlerhaften Instanzen. Vorhandene Verbindungen sind nicht betroffen und werden fortgesetzt, bis die Anwendung den Datenfluss beendet, ein Leerlauftimeout auftritt oder der virtuelle Computer heruntergefahren wird.

    Drei Testarten werden unterstützt:

    - **Benutzerdefinierter HTTP-Test:** Sie können damit Ihre eigene Logik zum Ermitteln der Integrität einer Back-End-Poolinstanz erstellen. Der Load Balancer testet regelmäßig Ihren Endpunkt (standardmäßig alle 15 Sekunden). Die Instanz wird als fehlerfrei angesehen, wenn sie innerhalb des Zeitlimits (standardmäßig 31 Sekunden) mit einem HTTP-Code 200 reagiert. Jeder von HTTP 200 abweichende Status bewirkt, dass dieser Test fehlschlägt.  Dies ist auch nützlich, wenn Sie zum Entfernen von Instanzen aus der Rotation des Load Balancers Ihre eigene Logik implementieren. Beispielsweise können Sie die Instanz so konfigurieren, dass sie einen anderen Status als 200 zurückgibt, wenn die Instanz bei über 90 % CPU-Auslastung liegt.   Dieser Test setzt den Gast-Agent-Standardtest außer Kraft.

    - **Benutzerdefinierter TCP-Test:** TCP-Tests basieren auf der erfolgreichen Einrichtung von TCP-Sitzungen an einem definierten Testport.  Solange der angegebene Listener auf dem virtuellen Computer vorhanden ist, wird dieser Test erfolgreich ausgeführt. Wenn die Verbindung abgelehnt wird, schlägt der Test fehl. Dieser Test setzt den Gast-Agent-Standardtest außer Kraft.

    - **Gast-Agent-Test (nur auf PaaS-VMs):** Der Load Balancer (Lastenausgleich) kann auch den Gast-Agent im virtuellen Computer nutzen. Der Gast-Agent lauscht und antwortet nur dann mit einer HTTP-OK-200-Antwort, wenn sich die Instanz im Zustand „Bereit“ befindet. Wenn der Agent nicht mit dem HTTP-Code „200 OK“ antwortet, kennzeichnet das Lastenausgleichsmodul die Instanz als nicht reagierend und sendet keinen Datenverkehr mehr an diese Instanz. Der Load Balancer versucht weiterhin, die Instanz zu erreichen. Wenn der Gast-Agent mit dem HTTP-Code 200 antwortet, sendet der Load Balancer wieder Datenverkehr an diese Instanz.  Gast-Agent-Tests sind der letzte Ausweg und sollten nicht verwendet werden, wenn benutzerdefinierte HTTP- oder TCP-Testkonfigurationen möglich sind. 
    
* **Ausgehende Verbindungen (Quell-NAT)**

    Alle ausgehenden Datenströme von privaten IP-Adressen in Ihrem Microsoft Azure Virtual Network zu öffentlichen IP-Adressen im Internet können in eine Front-End-IP-Adresse des Load Balancers übersetzt werden. Wenn ein öffentliches Front-End über eine Lastenausgleichsregel an einen virtuellen Back-End-Computer gebunden ist, programmiert Azure ausgehende Verbindungen so, dass sie automatisch in die IP-Adresse des öffentlichen Front-Ends übersetzt werden. Dies wird auch als Quell-NAT (Source NAT, SNAT) bezeichnet. SNAT bietet wichtige Vorteile:

    + Sie ermöglicht einfache Upgrades und Notfallwiederherstellung von Diensten, da das Front-End dynamisch einer anderen Instanz des Diensts zugeordnet werden kann.
    + Sie vereinfacht die Verwaltung von Zugriffssteuerungslisten. Zugriffssteuerungslisten, die als Front-End-IP-Adressen ausgedrückt werden, ändern sich nicht, wenn Dienste zentral hoch- oder herunterskaliert oder erneut bereitgestellt werden.

    Eine ausführliche Beschreibung dieser Möglichkeit finden Sie im Artikel [Ausgehende Verbindungen in Azure](load-balancer-outbound-connections.md).

Standard Load Balancer hat zusätzliche SKU-spezifische Fähigkeiten über diese Grundlagen hinaus.  Ausführlichere Informationen finden Sie im Rest dieses Artikels.

## <a name="skus"></a> Vergleich der Load Balancer-SKUs

Azure Load Balancer unterstützt zwei verschiedene SKUs: Basic und Standard.  Diese unterscheiden sich hinsichtlich Szenariostaffelung, -funktionen und -preisen.  Jedes Szenario, das mit Basic Load Balancer möglich ist, kann auch mit Standard Load Balancer erstellt werden.  Tatsächlich sind die APIs für beide SKUs ähnlich, und sie werden über die Spezifikation einer SKU aufgerufen.  Die API zur Unterstützung von SKUs für Load Balancer und öffentliche IP-Adressen ist ab der API-Version 2017-08-01 verfügbar.  Beide SKUs haben die gleiche allgemeine API und Struktur.

Je nachdem, welche SKU ausgewählt wird, kann die komplette Szenariokonfiguration im Detail jedoch leicht abweichen. In der Load Balancer-Dokumentation wird darauf hingewiesen, wenn ein Artikel nur für eine bestimmte SKU gilt. Lesen Sie die folgende Tabelle, um die Unterschiede zu vergleichen und zu verstehen.  Ausführlichere Informationen finden Sie unter [Übersicht: Azure Standard Load Balancer](load-balancer-standard-overview.md).

>[!NOTE]
> Für neue Entwürfe sollte Standard Load Balancer verwendet werden. 

Eigenständige virtuelle Computer, Verfügbarkeitsgruppen und VM-Skalierungsgruppen können nur mit einer SKU, nie mit beiden verbunden werden. Wenn sie mit öffentlichen IP-Adressen verwendet werden, müssen Load Balancer-SKU und SKU für öffentliche IP-Adressen übereinstimmen. Load Balancer-SKUs und SKUs für öffentliche IP-Adressen sind nicht änderbar.

_Es ist eine bewährte Methode, SKUs explizit anzugeben, obwohl dies noch nicht obligatorisch ist._  Zu diesem Zeitpunkt werden notwendige Änderungen auf ein Minimum beschränkt. Ist keine SKU angegeben, wird dies so interpretiert, dass die Basic-SKU in der API-Version 2017-08-01 verwendet werden soll.

>[!IMPORTANT]
>Standard Load Balancer ist ein neues Load Balancer-Produkt und zum größten Teil eine Obermenge von Basic Load Balancer.  Es gibt wichtige und behutsame Unterschiede zwischen den beiden Produkten.  Jedes End-to-End-Szenario, das mit Basic Load Balancer möglich ist, kann mit Standard Load Balancer erstellt werden.  Wenn Sie den Basic Load Balancer bereits kennen, sollten Sie sich mit dem Standard Load Balancer vertraut machen, um die wesentlichen Änderungen im Verhalten zwischen Standard und Basic und deren Auswirkungen zu verstehen. Lesen Sie diesem Abschnitt sorgfältig.

| | [Standard-SKU](load-balancer-standard-overview.md) | Basic-SKU |
| --- | --- | --- |
| Größe des Back-End-Pools | Bis zu 1000 Instanzen | Bis zu 100 Instanzen |
| Back-End-Pool-Endpunkte | Jeder virtuelle Computer in einem einzelnen virtuellen Netzwerk, einschließlich Kombinationen aus virtuellen Computern, Verfügbarkeitsgruppen, VM-Skalierungsgruppen | Virtuelle Computer in einer einzelnen Verfügbarkeitsgruppe oder VM-Skalierungsgruppe |
| Verfügbarkeitszonen | Zonenredundante und zonale Front-Ends für eingehende und ausgehende Datenflüsse, Zuordnungen für ausgehende Datenflüsse bleiben bei Zonenfehlern erhalten, zonenübergreifender Lastenausgleich | / |
| Diagnose | Azure Monitor, mehrdimensionale Metriken einschließlich Byte- und Paketleistungsindikatoren, Integritätsteststatus, Verbindungsversuche (TCP SYN), Integrität ausgehender Verbindungen (erfolgreiche und fehlgeschlagene SNAT-Datenströme), Messungen für die aktive Datenebene | Azure Log Analytics nur für öffentlichen Load Balancer, SNAT-Überlastungswarnung, Integritätsanzahl für Back-End-Pool |
| HA-Ports | Interner Load Balancer | / |
| Standardmäßig sicher | Standardmäßig geschlossen für öffentliche IP- und Load Balancer-Endpunkte, und es muss eine Netzwerksicherheitsgruppe verwendet werden, über die explizit eine Whitelist verwendet wird, damit Datenverkehr möglich ist | Standardmäßig geöffnet, Netzwerksicherheitsgruppe ist optional |
| Ausgehende Verbindungen | Mehrere Front-Ends mit Deaktivierung über Regel. Es _muss_ explizit ein Szenario für ausgehenden Datenverkehr erstellt werden, damit der virtuelle Computer ausgehende Verbindungen verwenden kann.  [VNet-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) können ohne ausgehende Verbindungen erreicht werden und werden nicht für verarbeitete Daten angerechnet.  Alle öffentlichen IP-Adressen, einschließlich der Azure-PaaS-Dienste, die nicht als VNet-Dienstendpunkte verfügbar sind, müssen über ausgehende Verbindungen erreicht werden und werden als verarbeitete Daten angerechnet. Wenn nur ein interner Load Balancer einen virtuellen Computer versorgt, sind ausgehende Verbindungen nicht über Standard-SNAT verfügbar. Ausgehende SNAT-Programmierung ist transportprotokollspezifisch entsprechend dem Protokoll der Regel für eingehenden Lastenausgleich. | Einzelnes Front-End, ausgewählt nach dem Zufallsprinzip, wenn mehrere Front-Ends vorhanden sind.  Wenn Sie nur ein interner Load Balancer einen virtuellen Computer versorgt, wird Standard-SNAT verwendet. |
| Mehrere Front-Ends | Eingehend und ausgehend | Nur eingehend |
| Verwaltungsvorgänge | Die meisten Vorgänge < 30 Sekunden | Meist 60 bis 90+ Sekunden |
| SLA | 99,99 % für Datenpfad mit zwei fehlerfreien virtuellen Computern | Implizit in VM SLA | 
| Preise | Berechnet anhand der Anzahl der Regeln, Daten, die eingehend oder ausgehend verarbeitet werden, werden der Ressource zugeordnet  | Keine Berechnung |

Lesen Sie [Diensteinschränkungen für Load Balancer](https://aka.ms/lblimits).  Lesen Sie für Standard Load Balancer auch die ausführlicheren Informationen unter [Übersicht](load-balancer-standard-overview.md), [Preise](https://aka.ms/lbpricing) und [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Konzepte

### <a name = "publicloadbalancer"></a>Öffentlicher Load Balancer

Der öffentliche Load Balancer führt die Zuordnung zwischen öffentlicher IP-Adresse und Portnummer des eingehenden Datenverkehrs zu den privaten IP-Adressen und Portnummern der virtuellen Computer durch und ordnet umgekehrt den Antwortdatenverkehr aus dem virtuellen Computer zu. Mithilfe von Lastenausgleichsregeln können Sie bestimmte Arten von Datenverkehr auf mehrere virtuelle Computer oder Dienste verteilen. Sie können zum Beispiel die Netzwerklast von Webanforderungen auf mehrere Webserver verteilen.

Die folgende Abbildung zeigt einen Endpunkt für Webdatenverkehr mit Lastenausgleich, der von drei virtuellen Computern für den öffentlichen und privaten TCP-Port 80 genutzt wird. Diese drei virtuellen Computer bilden einen Satz mit Lastenausgleich.

![Beispiel für öffentlichen Lastenausgleich](./media/load-balancer-overview/IC727496.png)

*Abbildung 1: Vornehmen eines Lastenausgleichs für Webdatenverkehr mit einem öffentlichen Load Balancer*

Wenn Internetclients Webseitenanfragen über den TCP-Port 80 an die öffentliche IP-Adresse einer Web-App senden, verteilt der Azure Load Balancer diese Anfragen auf die drei virtuellen Computer in der Lastenausgleichsgruppe. Weitere Informationen zu Lastenausgleichsalgorithmen finden Sie auf der [Übersichtsseite zum Lastenausgleich](load-balancer-overview.md#load-balancer-features).

Standardmäßig verteilt Azure Load Balancer Netzwerkdatenverkehr gleichmäßig auf mehrere Instanzen virtueller Computer. Sie können auch Sitzungsaffinität konfigurieren. Weitere Informationen hierzu finden Sie unter [Load Balancer-Verteilungsmodus](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Interner Load Balancer

Ein interner Load Balancer leitet Datenverkehr nur an Ressourcen weiter, die sich innerhalb eines virtuellen Netzwerks befinden oder ein VPN für den Zugriff auf die Azure-Infrastruktur verwenden. In dieser Hinsicht unterscheidet sich ein interner Load Balancer von einem öffentlichen Load Balancer. Die Azure-Infrastruktur schränkt den Zugriff auf die Front-End-IP-Adressen eines virtuellen Netzwerks ein, für die Lastenausgleich durchgeführt wird. Front-End-Adressen und virtuelle Netzwerke werden nie direkt für einen Internetendpunkt verfügbar gemacht. Interne Branchenanwendungen werden in Azure ausgeführt. Auf sie wird aus Azure oder von lokalen Ressourcen aus zugegriffen.

Ein interner Load Balancer ermöglicht die folgenden Arten von Lastenausgleich:

* In einem virtuellen Netzwerk: Lastenausgleich zwischen virtuellen Computern im virtuellen Netzwerk und einer Gruppe von virtuellen Computern, die sich im selben virtuellen Netzwerk befinden.
* Für ein standortübergreifendes virtuelles Netzwerk: Lastenausgleich zwischen lokalen Computern und einer Gruppe von virtuellen Computern, die sich im selben virtuellen Netzwerk befinden. 
* Für Anwendungen mit mehreren Ebenen: Lastenausgleich für Anwendungen mit mehreren Ebenen mit Internetzugriff, bei denen die Back-End-Ebenen nicht über Internetzugriff verfügen. Die Back-End-Ebenen erfordern einen Lastenausgleich des Datenverkehrs aus einer mit dem Internet verbundenen Ebene (siehe Abbildung 2).
* Für Branchenanwendungen: Lastenausgleich für Branchenanwendungen, die in Azure ohne zusätzliche Hardware oder Software für den Lastenausgleich gehostet werden. Dieses Szenario enthält lokale Server, die sich in der Gruppe der Computer befinden, für deren Datenverkehr Lastenausgleich stattfindet.

![Beispiel für internen Load Balancer](./media/load-balancer-overview/IC744147.png)

*Abbildung: Durchführen eines Lastenausgleichs für mehrschichtige Anwendungen mit öffentlichem und internem Load Balancer*

## <a name="pricing"></a>Preise
Standard Load Balancer ist ein Produkt, das auf Basis einer Reihe von konfigurierten Lastenausgleichsregeln und aller verarbeiteten eingehenden und ausgehenden Daten in Rechnung gestellt wird. Informationen zu den Standard Load Balancer-Preisen finden Sie auf der Seite [Load Balancer – Preise](https://azure.microsoft.com/pricing/details/load-balancer/).

Basic Load Balancer wird kostenlos angeboten.

## <a name="sla"></a>SLA

Informationen zum SLA für Standard Load Balancer finden Sie auf der Seite [SLA für Load Balancer](https://aka.ms/lbsla). 

## <a name="next-steps"></a>Nächste Schritte

- Lesen von [Standard Load Balancer im Detail](load-balancer-standard-overview.md)
- Weitere Informationen zu [Standard Load Balancer und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md)
- Informationen zur Verwendung von [Load Balancer für ausgehende Verbindungen](load-balancer-outbound-connections.md)
- Weitere Informationen zu [Hochverfügbarkeitsports für Load Balancer](load-balancer-ha-ports-overview.md)
- Informationen zur Verwendung von [Load Balancer mit mehreren Front-Ends](load-balancer-multivip-overview.md)
- Weitere Informationen zu [VNET-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md)
- Informationen zum Erstellen eines [öffentlichen Basic Load Balancers](load-balancer-get-started-internet-portal.md)


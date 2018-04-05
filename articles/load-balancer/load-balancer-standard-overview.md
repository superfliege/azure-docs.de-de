---
title: 'Übersicht: Azure Standard Load Balancer | Microsoft-Dokumentation'
description: 'Übersicht: Features von Azure Standard Load Balancer'
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: d7ee74a19f806faed0bcfcfa5f1c5de3937d9f31
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="azure-load-balancer-standard-overview"></a>Übersicht: Azure Standard Load Balancer

Mit Azure Load Balancer können Sie Ihre Anwendungen skalieren und Ihre Dienste mit Hochverfügbarkeit bereitstellen. Load Balancer kann sowohl für Szenarien mit eingehenden als auch ausgehenden Verbindungen verwendet werden, bietet niedrige Latenzen und einen hohen Durchsatz und unterstützt Millionen Datenflüsse für alle TCP- und UDP-Anwendungen. 

In diesem Artikel geht es hauptsächlich um Standard Load Balancer.  Eine allgemeinere Übersicht über Azure Load Balancer finden Sie unter [Übersicht über Azure Load Balancer](load-balancer-overview.md).

## <a name="what-is-standard-load-balancer"></a>Was ist Standard Load Balancer?

Standard Load Balancer ist ein neues Load Balancer-Produkt für alle TCP- und UDP-Anwendungen und hat im Vergleich zu Basic Load Balancer einen erweiterten und detaillierteren Featuresatz.  Zwar gibt es viele Ähnlichkeiten, es ist aber unverzichtbar, dass Sie sich mit den Unterschieden vertraut machen, die in diesem Artikel skizziert sind.

Sie können Standard Load Balancer als öffentlichen oder internen Load Balancer verwenden. Ein virtueller Computer kann mit einer öffentlichen und einer internen Load Balancer-Ressource verbunden werden.

Die Funktionen einer Load Balancer-Ressource werden immer als Front-End-, Regel-, Integritätstest- und Back-End-Pool-Definition ausgedrückt.  Eine Ressource kann mehrere Regeln enthalten. Sie können virtuelle Computer im Back-End-Pool platzieren, indem Sie den Back-End-Pool aus der NIC-Ressource des virtuellen Computers angeben.  Liegt eine VM-Skalierungsgruppe vor, wird dieser Parameter über das Netzwerkprofil übergeben und erweitert.

Ein Hauptaspekt ist der Bereich des virtuellen Netzwerks für die Ressource.  Während ein Basic Load Balancer im Bereich einer Verfügbarkeitsgruppe vorhanden ist, ist ein Standard Load Balancer vollständig in den Bereich eines virtuellen Netzwerks integriert, und es gelten alle Konzepte für ein virtuelles Netzwerk.

Load Balancer-Ressourcen sind Objekte, in denen Sie formulieren können, wie Azure seine mehrinstanzenfähige Infrastruktur programmieren sollte, um das Szenario zu erzielen, das Sie erstellen möchten.  Es gibt keine direkte Beziehung zwischen Load Balancer-Ressourcen und tatsächlicher Infrastruktur. Durch Erstellen eines Load Balancers wird keine Instanz erstellt, Kapazität ist immer verfügbar, und es sind weder Start- noch Skalierungsverzögerungen zu berücksichtigen. 

>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarien bereit.  Wenn Sie nach Informationen zu TLS-Terminierung („SSL-Auslagerung“) oder Verarbeitung pro HTTP/HTTPS-Anforderungsanwendungsschicht suchen, lesen Sie [Application Gateway](../application-gateway/application-gateway-introduction.md).  Wenn Sie nach Informationen zu globalem DNS-Lastenausgleich suchen, lesen Sie [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Für Ihre End-to-End-Szenarien kann es vorteilhaft sein, diese Lösungen nach Bedarf zu kombinieren.

## <a name="why-use-standard-load-balancer"></a>Gründe zum Verwenden von Standard Load Balancer

Sie können Standard Load Balancer für das gesamte Spektrum virtueller Rechenzentren von kleinen Bereitstellungen bis hin zu großen und komplexen Architekturen mit mehreren Zonen verwenden.

In der folgenden Tabelle finden Sie einen Überblick über die Unterschiede zwischen Standard Load Balancer und Basic Load Balancer:

>[!NOTE]
> Für neue Entwürfe sollte Standard Load Balancer verwendet werden. 

| | Standard-SKU | Basic-SKU |
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

Lesen Sie [Dienstgrenzwerte für Load Balancer](https://aka.ms/lblimits) sowie [Preise](https://aka.ms/lbpricing) und [SLA](https://aka.ms/lbsla).


### <a name="backend"></a>Back-End-Pool

Ein Standard Load Balancer-Back-End-Pool wird um jede Ressource eines virtuellen Computers in einem virtuellen Netzwerk erweitert.  Er kann bis zu 1000 Back-End-Instanzen enthalten.  Eine Back-End-Instanz ist eine IP-Konfiguration, die eine Eigenschaft einer NIC-Ressource ist.

Der Back-End-Pool kann eigenständige virtuelle Computer, Verfügbarkeitsgruppen oder VM-Skalierungsgruppen enthalten.  Sie können Ressourcen in den Back-End-Pool einbringen, und dieser kann eine beliebige Kombination dieser Ressourcen enthalten(bis zu 150 insgesamt).

Wenn Sie überlegen, wie Sie Ihren Back-End-Pool gestalten, können Sie die Anzahl der einzelnen Back-End-Pool-Ressourcen so gering wie möglich halten, um die Dauer von Verwaltungsvorgängen weiter zu optimieren.  Es gibt keinen Unterschied in der Datenebenenleistung oder -skalierung.

## <a name="az"></a>Verfügbarkeitszonen

>[!NOTE]
> Wenn Sie die [Vorschauversion für Verfügbarkeitszonen](https://aka.ms/availabilityzones) mit Standard Load Balancer verwenden möchten, müssen Sie [sich für Verfügbarkeitszonen](https://aka.ms/availabilityzones) registrieren.

Standard Load Balancer unterstützt zusätzliche Funktionen in Regionen, in denen Verfügbarkeitszonen verfügbar sind.  Diese Funktionen ergänzen alle Funktionen, die Standard Load Balancer bereitstellt.  Verfügbarkeitszonenkonfigurationen sind für einen öffentlichen und internen Standard Load Balancer verfügbar.

Nicht zonale Front-Ends werden standardmäßig zonenredundant, wenn sie in einer Region mit Verfügbarkeitszonen bereitgestellt werden.   Ein zonenredundantes Front-End überdauert Zonenfehler und wird von dedizierter Infrastruktur in allen Zonen gleichzeitig versorgt. 

Darüber hinaus können Sie ein Front-End für eine bestimmte Zone garantieren. Ein zonales Front-End ist direkt an die jeweilige Zone gekoppelt und wird nur durch dedizierte Infrastruktur in einer einzelnen Zone versorgt.

Für den Back-End-Pool steht zonenübergreifender Lastenausgleich zur Verfügung, und jede Ressource eines virtuellen Computers in einem virtuellen Netzwerk kann Bestandteil eines Back-End-Pools sein.

Lesen Sie die [ausführliche Erläuterung der Funktionalität im Zusammenhang mit Verfügbarkeitszonen](load-balancer-standard-availability-zones.md).

### <a name="diagnostics"></a>Diagnose

Standard Load Balancer stellt mehrdimensionale Metriken über Azure Monitor bereit.  Diese Metriken können gefiltert und gruppiert werden und stellen aktuelle und zurückliegende Einblicke in die Leistung und Integrität Ihres Diensts bereit.  Resource Health (Ressourcenintegrität) wird ebenfalls unterstützt.  Es folgt eine kurze Übersicht über unterstützte Diagnosen:

| Metrik | BESCHREIBUNG |
| --- | --- |
| VIP-Verfügbarkeit | Load Balancer Standard wendet kontinuierlich den Datenpfad aus einer Region auf das Load Balancer-Front-End bis hin zum SDN-Stapel an, der Ihren virtuellen Computer unterstützt. Solange integre Instanzen verbleiben, folgt die Messung demselben Pfad wie der Datenverkehr mit Lastenausgleich Ihrer Anwendungen. Der Datenpfad, der von Ihren Kunden verwendet wird, wird ebenfalls überprüft. Die Messung ist für Ihre Anwendung nicht sichtbar und bewirkt keine Beeinträchtigung bei anderen Vorgängen.|
| DIP-Verfügbarkeit | Load Balancer Standard verwendet einen verteilten Integritätsprüfungsdienst, der die Integrität Ihres Anwendungsendpunkts gemäß Ihren Konfigurationseinstellungen überwacht. Diese Metrik stellt eine Aggregat- oder nach Endpunkt gefilterte Ansicht jedes einzelnen Instanzendpunkts im Load Balancer-Pool bereit.  Sie können sehen, wie Load Balancer die Integrität Ihrer Anwendung gemäß Ihrer Integritätsprüfungskonfiguration beurteilt.
| SYN-Pakete | Load Balancer Standard beendet keine TCP-Verbindungen und interagiert nicht mit TCP- oder UDP-Paketdatenflüssen. Datenflüsse und deren Handshakes erfolgen immer zwischen der Quelle und der VM-Instanz. Für eine bessere Problembehandlung Ihrer TCP-Protokollszenarien können Sie SYN-Paketzähler verwenden, um zu verstehen, wie viele TCP-Verbindungsversuche vorgenommen werden. Die Metrik gibt die Anzahl der TCP-SYN-Pakete an, die empfangen wurden.|
| SNAT-Verbindungen | Load Balancer Standard meldet die Anzahl von maskierten ausgehenden Flows an das Front-End mit der öffentlichen IP-Adresse. SNAT Ports stellen eine erschöpfbare Ressource dar. Diese Metrik kann einen Hinweis darauf geben, in welchem Umfang Ihre Anwendung SNAT für ausgehende Flows nutzt.  Die Zählerstände für erfolgreiche und fehlgeschlagene ausgehende SNAT-Flows werden gemeldet und können zur Problembehebung und Analyse Ihrer ausgehenden Flows verwendet werden.|
| Byteleistungsindikatoren | Load Balancer Standard meldet die pro Front-End verarbeiteten Daten.|
| Paketleistungsindikatoren | Load Balancer Standard meldet die pro Front-End verarbeiteten Pakete.|

Lesen Sie die [ausführliche Erläuterung zu Standard Load Balancer-Diagnosen](load-balancer-standard-diagnostics.md).

### <a name="haports"></a>HA-Ports

Standard Load Balancer unterstützt einen neuen Typ von Regel.  

Sie können Lastenausgleichsregeln konfugurieren, damit die Anwendung skaliert werden kann und hohe Zuverlässigkeit bietet. Wenn Sie eine HA-Ports-Lastenausgleichsregel verwenden, stellt Standard Load Balancer einen Lastausgleich pro Datenfluss an jedem kurzlebigen Port der Front-End-IP-Adresse eines internen Standard Load Balancers bereit.  Das Feature ist auch für andere Szenarien hilfreich, in denen es unpraktisch oder nicht erwünscht ist, einzelne Port anzugeben.

Eine HA-Ports-Lastenausgleichsregel ermöglicht es Ihnen, aktive-passive oder aktive-aktive n+1-Szenarien für virtuelle Netzwerkgeräte und jede Anwendung zu erstellen, die große Bereiche von eingehenden Ports erfordert.  Ein Integritätstest kann verwendet werden, um zu bestimmen, welche Back-Ends neue Datenflüsse empfangen sollen.  Sie können eine Netzwerksicherheitsgruppe verwenden, um ein Portbereichsszenario zu emulieren.

>[!IMPORTANT]
> Wenn Sie beabsichtigen, ein virtuelles Netwerkgerät zu verwenden, fragen Sie Ihren Anbieter danach, ob sein Produkt mit HA-Ports getestet wurde, und befolgen Sie dessen spezielle Anweisungen für die Implementierung. 

Lesen Sie die [ausführliche Erläuterung zu HA-Ports](load-balancer-ha-ports-overview.md).

### <a name="securebydefault"></a>Standardmäßig sicher

Standard Load Balancer ist vollständig in das virtuelle Netzwerk integriert.  Das virtuelle Netzwerk ist ein privates geschlossenes Netzwerk.  Da Standard Load Balancer und öffentliche Standard-IP-Adressen so ausgelegt sind, dass sie Zugriff auf dieses virtuelle Netzwerk von außerhalb des virtuellen Netzwerks zulassen, werden diese Ressourcen nun standardmäßig geschlossen, es sei denn, Sie öffnen sie. Dies bedeutet, dass nun Netzwerksicherheitsgruppen (NSGs) verwendet werden, um zulässigen Datenverkehr explizit zu gestatten und whitelistgemäß zu verarbeiten.  Sie können Ihr gesamtes virtuelles Rechenzentrum erstellen und über NSGs entscheiden, was verfügbar sein soll und wann es verfügbar sein soll.  Wenn Sie keine NSG in einem Subnetz oder für eine Netzwerkkarte Ihrer Ressource auf einem virtuellen Computer haben, gibt es keine Erlaubnis für Datenverkehr, diese Ressource zu erreichen.

Weitere Informationen zu NSGs und ihrer Verwendung in Ihrem Szenario finden Sie unter [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

### <a name="outbound"></a> Ausgehende Verbindungen

Load Balancer unterstützt Szenarien mit eingehenden und ausgehenden Verbindungen.  Standard Load Balancer unterscheidet sich hinsichtlich ausgehender Verbindungen deutlich von Basic Load Balancer.

SNAT (Source Network Address Translation) wird verwendet, um interne private IP-Adressen in Ihrem virtuellen Netzwerk auf öffentliche IP-Adressen auf Load Balancer-Front-Ends abzubilden.

Mit Standard Load Balancer wird ein neuer Algorithmus für einen [robusten sowie besser skalierbaren und vorhersagbaren SNAT-Algorithmus](load-balancer-outbound-connections.md#snat) eingeführt, und Standard Load Balancer ermöglicht neue Fähigkeiten, beseitigt Mehrdeutigkeit und erzwingt explizite Konfigurationen anstelle von Nebeneffekten. Diese Änderungen sind erforderlich, damit neue Features eingebunden werden können. 

Dies sind die wichtigsten Grundsätze, die beim Arbeiten mit Standard Load Balancer zu beachten sind:

- Der Abschluss einer Regel steuert die Load Balancer-Ressource.  Die gesamte Azure-Programmierung leitet sich aus ihrer Konfiguration ab.
- Wenn mehrere Front-Ends verfügbar sind, werden alle Front-Ends verwendet, und jedes Front-End multipliziert die Anzahl der verfügbaren SNAT-Ports.
- Sie können für ein bestimmtes Front-End festlegen, dass es nicht für ausgehende Verbindungen verwendet werden darf.
- Szenarien mit ausgehenden Verbindungen sind explizit, und ausgehende Verbindungen sind nicht vorhanden, bis sie angegeben wurden.
- Lastenausgleichsregeln folgen dem, wie SNAT programmiert ist. Lastenausgleichsregeln sind protokollspezifisch. SNAT ist protokollspezifisch, und dies sollte sich in der Konfiguration wiederfinden, statt einen Nebeneffekt zu erstellen.

#### <a name="multiple-frontends"></a>Mehrere Front-Ends
Wenn Sie weitere SNAT-Ports benötigen, weil Sie einen hohen Bedarf an ausgehenden Verbindungen erwarten oder bereits feststellen, können Sie auch einen inkrementellen SNAT Portvorrat hinzufügen, indem Sie zusätzliche Front-Ends, Regeln und Back-End-Pools für dieselben Ressourcen der virtuellen Computer konfigurieren.

#### <a name="control-which-frontend-is-used-for-outbound"></a>Steuern, welches Front-End für ausgehende Verbindungen verwendet wird
Wenn Sie ausgehende Verbindungen so einschränken möchten, dass sie nur von einer bestimmten Front-End-IP-Adresse stammen, können Sie ausgehende SNAT in der Regel deaktivieren, in der die ausgehende Zuordnung formuliert ist.

#### <a name="control-outbound-connectivity"></a>Steuern von ausgehenden Verbindungen
Standard Load Balancer ist im Kontext des virtuellen Netzwerks vorhanden.  Ein virtuelles Netzwerk ist ein isoliertes privates Netzwerk.  Solange es keine Zuordnung zu einer öffentlichen IP-Adresse gibt, sind öffentliche Verbindungen nicht zulässig.  Sie können [VNet-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) erreichen, da sie sich in Ihrem virtuellen Netzwerk befinden und zu diesem lokal sind.  Wenn Sie eine ausgehende Verbindung zu einem Ziel außerhalb Ihres virtuellen Netzwerks herstellen möchten, haben Sie zwei Möglichkeiten:
- Sie können der Ressource des virtuellen Computers eine öffentliche Standard-SKU-IP-Adresse als öffentliche IP-Adresse auf Instanzebene zuweisen, oder
- Sie können die Ressource des virtuellen Computers im Back-End-Pool eines öffentlichen Standard Load Balancers platzieren.

Auf beide Arten werden ausgehende Verbindungen aus dem virtuellen Netzwerk ermöglicht. 

Wenn Sie _nur_ einen internen Standard Load Balancer mit dem Back-End-Pool verknüpft haben, in dem sich die Ressource des virtuellen Computers befindet, kann Ihr virtueller Computer nur virtuelle Netzwerkressourcen und [VNet-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) erreichen.  Sie können die im vorherigen Abschnitt beschriebenen Schritte ausführen, um ausgehende Verbindungen zu erstellen.

Ausgehende Verbindungen für eine Ressource eines virtuellen Computers, die keinen Standard-SKUs zugeordnet ist, bleiben ungeändert.

Lesen Sie die [ausführliche Erläuterung zu ausgehenden Verbindungen](load-balancer-outbound-connections.md).

### <a name="multife"></a>Mehrere Front-Ends
Load Balancer unterstützt mehrere Regeln mit mehreren Front-Ends.  Für Standard Load Balancer wird dies auf Szenarien mit ausgehenden Verbindungen erweitert.  Szenarien mit ausgehenden Verbindungen sind im Prinzip die Umkehrung einer Regel für eingehenden Lastenausgleich.  Die Regel für eingehenden Lastenausgleich erstellt auch eine Zuordnung für ausgehende Verbindungen. Standard Load Balancer verwendet alle Front-Ends, die einer Ressource eines virtuellen Computers zugeordnet sind, über eine Lastenausgleichsregel.  Darüber hinaus gibt es einen Parameter für Lastenausgleichsregeln, mit dem Sie eine Lastenausgleichsregel bezüglich ausgehender Verbindungen unterdrücken können, wodurch es möglich ist, bestimmte Front-Ends auszuwählen.

Zum Vergleich: Basic Load Balancer wählt ein einzelnes Front-End nach dem Zufallsprinzip aus, und es gibt keine Möglichkeit, zu steuern, welches ausgewählt wird.

Lesen Sie die [ausführliche Erläuterung zu ausgehenden Verbindungen](load-balancer-outbound-connections.md).

### <a name="operations"></a> Verwaltungsvorgänge

Standard Load Balancer-Ressourcen sind nur auf einer vollständig neuen Infrastrukturplattform vorhanden.  Dies ermöglicht deutlich schnellere Verwaltungsvorgänge für Standard-SKUs, und die Ausführungszeiten sind in der Regel kürzer als 30 Sekunden pro Standard-SKU-Ressource.  Es ist zu beachten, dass sich mit der Vergrößerung von Back-End-Pools auch die Dauer verlängert, die für Back-End-Pool-Änderungen erforderlich ist.

Sie können viel schneller Standard Load Balancer-Ressourcen ändern und eine öffentliche Standard-IP-Adresse von einem virtuellen Computer zu einem anderen verschieben.

## <a name="migration-between-skus"></a>Migration zwischen SKUs

SKUs sind nicht änderbar. Führen Sie die Schritte in diesem Abschnitt aus, um aus einer Ressourcen-SKU in eine andere zu wechseln.

### <a name="migrate-from-basic-to-standard-sku"></a>Migrieren von der SKU „Basic“ zu „Standard“

1. Erstellen Sie eine neue Standard-Ressource (Load Balancer bzw. Public IP nach Bedarf). Erstellen Sie Ihre Regeln und Prüfdefinitionen erneut.

2. Erstellen Sie eine neue Netzwerksicherheitsgruppe (NSG) für die Netzwerkschnittstelle (NIC) (bzw. aktualisieren Sie eine vorhandene NSG) oder ein Subnetz für die Whitelist mit dem vom Lastenausgleich verteilten Datenverkehr, eine Prüfung sowie beliebigen anderen Datenverkehr, den Sie zulassen möchten.

3. Entfernen Sie die Basic-SKU-Ressourcen (Load Balancer und Public IPs nach Bedarf) aus allen VM-Instanzen. Stellen Sie sicher, dass auch alle VM-Instanzen einer Verfügbarkeitsgruppe entfernt werden.

4. Fügen Sie alle VM-Instanzen an die neuen SKU-Ressourcen vom Typ „Standard“ an.

### <a name="migrate-from-standard-to-basic-sku"></a>Migrieren von der SKU „Standard“ zu „Basic“

1. Erstellen Sie eine neue Basic-Ressource (Load Balancer bzw. Public IP nach Bedarf). Erstellen Sie Ihre Regeln und Prüfdefinitionen erneut. 

2. Entfernen Sie die Standard-SKU-Ressourcen (Load Balancer und Public IPs nach Bedarf) aus allen VM-Instanzen. Stellen Sie sicher, dass auch alle VM-Instanzen einer Verfügbarkeitsgruppe entfernt werden.

3. Fügen Sie alle VM-Instanzen an die neuen SKU-Ressourcen vom Typ „Basic“ an.

>[!IMPORTANT]
>
>Es bestehen Einschränkungen hinsichtlich der Nutzung der SKUs „Basic“ und „Standard“.
>
>HA-Ports und Diagnose der Standard-SKU sind nur in der SKU „Standard“ verfügbar. Sie können nicht von der SKU „Standard“ zur SKU „Basic“ migrieren und diese Funktionen beibehalten.
>
>Basic- und Standard-SKU haben eine Reihe von Unterschieden, wie sie in diesem Artikel beschrieben sind.  Sie sollten versuchen, diese zu verstehen und sich darauf vorzubereiten.
>
>Für Load Balancer- und Public IP-Ressourcen müssen übereinstimmende SKUs verwendet werden. Eine Kombination von Ressourcen vom SKU-Typ „Basic“ und „Standard“ ist nicht möglich. Sie können eigenständige virtuelle Computer, virtuelle Computer in einer Ressource einer Verfügbarkeitsgruppe oder eine Ressource einer VM-Skalierungsgruppe an beide SKUs gleichzeitig anfügen.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Load Balancer Standard ist zurzeit in allen öffentlichen Cloudregionen verfügbar.

## <a name="sla"></a>SLA

Standard Load Balancer sind mit einer SLA von 99,99 % verfügbar.  Lesen Sie die [SLA für Standard Load Balancer](https://aka.ms/lbsla), wenn Sie ausführliche Informationen wünschen.

## <a name="pricing"></a>Preise

Standard Load Balancer ist ein Produkt, das auf Basis einer Reihe von konfigurierten Lastenausgleichsregeln und aller verarbeiteten eingehenden und ausgehenden Daten in Rechnung gestellt wird. Informationen zu den Standard Load Balancer-Preisen finden Sie auf der Seite [Load Balancer – Preise](https://aka.ms/lbpricing).

## <a name="limitations"></a>Einschränkungen

- Load Balancer-Back-End-Instanzen dürfen sich zurzeit nicht in virtuellen Peernetzwerken befinden. Alle Back-End-Instanzen müssen sich in der gleichen Region befinden.
- SKUs sind nicht änderbar. Sie können die SKU einer vorhandenen Ressource nicht ändern.
- Eine Ressource eines eigenständigen virtuellen Computers, eine Verfügbarkeitsgruppenressource oder eine Ressource einer VM-Skalierungsgruppe kann auf eine SKU verweisen, nie auf beide.
- [Azure Monitor-Warnungen](../monitoring-and-diagnostics/monitoring-overview-alerts.md) werden zu diesem Zeitpunkt nicht unterstützt.
- [Vorgänge zum Verschieben von Abonnements](../azure-resource-manager/resource-group-move-resources.md) werden für Standard-SKU-LB- und -PIP-Ressourcen nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Standard Load Balancer und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md).
- Weitere Informationen zu [Verfügbarkeitszonen](../availability-zones/az-overview.md).
- Weitere Informationen zu [Diagnosen für Standard Load Balancer](load-balancer-standard-diagnostics.md).
- Weitere Informationen zu [unterstützten mehrdimensionalen Metriken](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkloadbalancers) für Diagnosen in [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).
- Informationen zur Verwendung von [Load Balancer für ausgehende Verbindungen](load-balancer-outbound-connections.md).
- Weitere Informationen zu [Standard Load Balancer mit Lastenausgleichsregeln für HA-Ports](load-balancer-ha-ports-overview.md).
- Informationen zur Verwendung von [Load Balancer mit mehreren Front-Ends](load-balancer-multivip-overview.md).
- Weitere Informationen zu [virtuellen Netzwerken](../virtual-network/virtual-networks-overview.md).
- Weitere Informationen zu [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).
- Weitere Informationen zu [VNET-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md).
- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) in Azure.
- Weitere Informationen zu [Load Balancer](load-balancer-overview.md).

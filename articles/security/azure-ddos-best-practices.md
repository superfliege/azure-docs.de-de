---
title: Azure DDoS Protection – bewährte Methoden und Referenzarchitektur | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie auf der Grundlage von Protokollierungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen.
services: security
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2018
ms.author: barclayn
ms.openlocfilehash: 4b2d785f5b9095a2decfc65ec46808ff6f65c38e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="azure-ddos-protection-best-practices-and-reference-architecture"></a>Azure DDoS Protection – bewährte Methoden und Referenzarchitektur

Dieses Dokument richtet sich an IT-Entscheidungsträger und Sicherheitspersonal. Vertrautheit mit Azure, Netzwerktechnik und Sicherheit werden erwartet.

Konzeptionen für Resilienz gegenüber Distributed Denial of Service (DDoS) setzen Planungen und Entwürfe für eine Vielzahl von Fehlerzuständen voraus. Dieses Dokument enthält bewährte Methoden zum Entwerfen von Anwendungen in Azure für die Resilienz gegenüber DDoS-Angriffen.

## <a name="types-of-attacks"></a>Arten von Angriffen

DDoS ist ein Angriffstyp, der das Ziel hat, Anwendungsressourcen zu erschöpfen. Die Verfügbarkeit der Anwendung und ihre Fähigkeit, legitime Anfragen zu verarbeiten, sollen beeinträchtigt werden. Die Angriffe werden anspruchsvoller, ihr Ausmaß größer und ihre Auswirkungen gravierender. Jeder Endpunkt, der öffentlich über das Internet erreichbar ist, kann Ziel von DDoS-Angriffen werden.

Azure bietet dauerhaften Schutz gegen DDoS-Angriffe. Dieser Schutz ist standardmäßig und ohne zusätzliche Kosten in der Azure-Plattform integriert. Zusätzlich zum in der Plattform bereitgestellten grundsätzlichen DDoS-Schutz haben wir auch ein neues Angebot mit dem Namen „[Azure DDoS Protection Standard](https://azure.microsoft.com/services/ddos-protection/)“ mit erweiterten DDoS-Entschärfungsfunktionen für Netzwerkangriffe, das automatisch für den Schutz Ihrer spezifischen Azure-Ressourcen optimiert ist. Der Schutz lässt sich einfach während der Erstellung des neuen virtuellen Netzwerks aktivieren. Dies ist auch nach der anfänglichen Erstellung möglich und erfordert keine Anwendungs- oder Ressourcenänderungen.

![](media/azure-ddos-best-practices/image1.png)

DDoS-Angriffe können grob in drei (3) verschiedene Kategorien klassifiziert werden

### <a name="volumetric-attacks"></a>Volumetrische Angriffe

Volumetrische Angriffe sind die am häufigsten auftretenden Typen von DDoS-Angriffen. Volumetrische Angriffe sind Brute-Force-Angriffe, die auf die Vermittlungs- und Transportschichten abzielen. Sie versuchen, Ressourcen solcher Netzwerklinks zu erschöpfen. Bei diesen Angriffen werden in der Regel mehrere infizierte Systeme verwendet, um die Vermittlungsschichten mit einer beträchtlichen Menge scheinbar berechtigten Datenverkehrs zu überfluten. Verschiedene Netzwerkschichtprotokolle wie z.B. Internet Control Message-Protokoll (ICMP), User Datagram-Protokoll (UDP) und Transmission Control-Protokoll (TCP) werden bei dieser Art von Angriff verwendet.

Die häufigsten DDoS-Angriffe auf Vermittlungsschichten sind TCP-SYN-Überflutung, ICMP-Echo, UDP-Überflutung, DNS-Angriff und NTP-Verstärkungsangriff. Mit derartigen Angriffen kann nicht nur der Dienst unterbrochen werden, sondern sie dienen auch häufig als Deckmantel für weitaus schädlicheres und gezielteres Eindringen in das Netzwerk. Ein Beispiel für aktuelle volumetrische Angriffe ist der [Memcached-Exploit](https://www.wired.com/story/github-ddos-memcached/), der GitHub beeinträchtigt hat. Dieser Angriff betraf UDP-Port 11211 und hatte ein Volumen von 1,35Tb/s.

### <a name="protocol-attacks"></a>Protokollangriffe

Protokollangriffe zielen auf Anwendungsprotokolle. Sie versuchen, alle verfügbaren Ressourcen in Infrastrukturgeräten wie Firewalls, Anwendungsservern und Lastenausgleichsmodulen zu verwenden. Protokollangriffe verwenden Pakete, die im falschen Format vorliegen oder Abnormitäten aufweisen. Bei diesen Angriffen werden offene Anforderungen in großer Zahl gesendet, sodass Server und andere Kommunikationsgeräte antworten und auf eine Paketantwort warten. Das Ziel versucht, auf die offenen Anforderungen zu antworten, was schließlich zum Absturz des Zielsystems führt.

Das häufigste Beispiel für einen protokollbasierten DDoS-Angriff ist die TCP-SYN-Flut. Bei einem solchen Angriff wird eine Folge von TCP-SYN-Anforderungen zu einem Ziel weitergeleitet und kann verwendet werden, um es zu überlasten. Die Absicht ist, das Ziel reaktionsunfähig zu machen. Der Dyn-Ausfall von 2016 war nicht nur ein Anwendungsschichtangriff, sondern umfasste auch auf Port 53 der DNS-Server von Dyn zielende TCP-SYN-Überflutungen.

### <a name="resource-attacks"></a>Ressourcenangriffe

Ressourcenangriffe zielen auf die Anwendungsschicht. Sie lösen Back-End-Prozesse aus, um das Zielsystem zu überlasten. Ressourcenangriffe täuschen normalen Datenverkehr vor, der jedoch Abfragen an den Server richtet, die die CPU massiv belasten. Das zum Erschöpfen der Ressourcen benötigte Datenverkehrsvolumen ist vergleichsweise niedriger als bei anderen Angriffstypen. Der Datenverkehr eines Ressourcenangriffs lässt sich nicht von legitimem Datenverkehr unterscheiden und ist darum nur schwer zu erkennen. Ressourcenangriffe richten sich am häufigsten gegen HTTP/HTTPS- und DNS-Dienste.

## <a name="shared-responsibility-in-the-cloud"></a>Gemeinsame Verantwortung in der Cloud

Eine ausführliche Verteidigungsstrategie ist erforderlich, um der wachsenden Zahl von Angriffstypen und zunehmenden Raffinesse der Angriffe zu begegnen. Für die Sicherheit sind der Kunde und Microsoft gemeinsam verantwortlich. Microsoft bezeichnet dies als das [Modell für gemeinsame Verantwortung](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). Die folgende Abbildung zeigt diese Aufteilung der Verantwortungsbereiche.

![](media/azure-ddos-best-practices/image2.png)

Azure-Kunden können unsere bewährten Methoden studieren und global verteilte Anwendungen erstellen, die auf Fehler getestet wurden.

## <a name="fundamental-best-practices"></a>Grundlegende bewährte Methoden

DDoS-Angriffe liegen im Trend. Der folgende Abschnitt enthält eine ausführliche Anleitung zum Erstellen DDoS-resilienter Dienste in Azure.

### <a name="design-for-security"></a>Sicherheitsorientiertes Design

Kunden müssen sicherstellen, dass die Sicherheit im gesamten Lebenszyklus einer Anwendung Priorität hat – von Entwurf und Implementierung bis hin zu Bereitstellung und Betrieb. Anwendungen können Fehler enthalten, die einer relativ geringen Anzahl entsprechend gestalteter Anforderungen die übermäßige Verwendung von Ressourcen erlauben, was zu einem Dienstausfall führt. Um einen in Microsoft Azure ausgeführten Dienst zu schützen, sollten Kunden ihre Anwendungsarchitektur gut kennen und müssen sich auf die [5 Säulen der Softwarequalität](https://docs.microsoft.com/azure/architecture/guide/pillars) konzentrieren.
Kunden sollten Folgendes kennen: das typische Datenverkehrsvolumen, das Modell der Konnektivität zwischen der Anwendung und anderen Anwendungen sowie die Dienstendpunkte, die dem öffentlichen Internet verfügbar gemacht werden.

Darüber hinaus ist von größter Wichtigkeit, sicherzustellen, dass eine Anwendung stabil genug ist, um mit einem Denial-of-Service-Angriff fertig zu werden. Sicherheit und Datenschutz sind direkt in der Azure-Plattform integriert, beginnend mit dem [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). Die SDL spricht Sicherheit in jeder Entwicklungsphase an und sorgt dafür, dass Azure kontinuierlich aktualisiert wird, um noch sicherer zu sein.

### <a name="design-for-scalability"></a>Skalierbarkeitsorientiertes Design

Skalierbarkeit ist die Fähigkeit eines Systems, eine höhere Last zu verarbeiten. Kunden müssen ihre Anwendungen so entwerfen, dass sie [horizontal hochskalieren](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) können, um die Anforderungen einer verstärkten Auslastung zu erfüllen – insbesondere im Falle eines DDoS-Angriffs. Wenn Ihre Anwendung von einer einzelnen Instanz eines Diensts abhängig ist, entsteht dadurch ein Single Point of Failure. Durch die Bereitstellung von mehreren Instanzen werden Resilienz und Skalierbarkeit verbessert.

Wählen Sie für [Azure App Service](../app-service/app-service-value-prop-what-is.md) einen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) aus, der mehrere Instanzen bietet. Konfigurieren Sie für Azure Cloud Services alle Rollen so, dass sie [mehrere Instanzen](../cloud-services/cloud-services-choose-me.md) verwenden. Stellen Sie für [Azure Virtual Machines (VMs)](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) sicher, dass die VM-Architektur mehr als eine VM enthält und dass jede VM zu einer [Verfügbarkeitsgruppe](../virtual-machines/virtual-machines-windows-manage-availability.md) gehört. Sie sollten [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) für Funktionen zur automatischen Skalierung verwenden.

### <a name="defense-in-depth"></a>Tiefgehende Verteidigung

Hinter der tiefgehenden Verteidigung steht die Idee, dem Risiko mit verschiedenen Verteidigungsstrategien zu begegnen. Abwehrmaßnahmen in die Schichten einer Anwendung zu integrieren reduziert die Wahrscheinlichkeit eines erfolgreichen Angriffs. Kunden sollten sichere Entwürfe für ihre Anwendungen über die integrierten Funktionen der Azure-Plattform implementieren.

So steigt z.B. das Risiko von Angriffen mit der Größe oder dem Oberflächenbereich der Anwendung. Der Oberflächenbereich sollte durch Whitelists reduziert werden, um die verfügbar gemachten IP-Adressräume und Überwachungsports zu schließen, die nicht auf den Lastenausgleichsmodulen ([ALB](../load-balancer/load-balancer-get-started-internet-portal.md)/[App Gateway](../application-gateway/application-gateway-create-probe-portal.md)) oder über [Netzwerksicherheitsgruppen (Network Security Groups, NSG)](../virtual-network/virtual-networks-nsg.md) benötigt werden.
Sie können mit [Diensttags](/virtual-network/security-overview.md) und [Anwendungssicherheitsgruppen](/virtual-network/security-overview.md) die Komplexität des Erstellens von Sicherheitsregeln minimieren und Netzwerksicherheit als natürliche Erweiterung der Struktur einer Anwendung konfigurieren.

Kunden sollten Azure-Dienste nach Möglichkeit in einem [virtuellen Netzwerk](../virtual-network/virtual-networks-overview.md) bereitstellen. So können Dienstressourcen über private IP-Adressen kommunizieren. Standardmäßig werden für Datenverkehr von Azure-Diensten aus einem virtuellen Netzwerk öffentliche IP-Adressen als Quell-IP-Adressen verwendet. Bei Verwendung von [Dienstendpunkten](../virtual-network/virtual-network-service-endpoints-overview.md) wechselt der Dienstdatenverkehr zu privaten Adressen im virtuellen Netzwerk als Quell-IP-Adressen, wenn aus einem virtuellen Netzwerk auf den Azure-Dienst zugegriffen wird.

Häufig werden lokale Ressourcen von Kunden zusammen mit ihren Azure-Ressourcen angegriffen. Wenn Sie die Verbindung einer lokalen Umgebung mit Azure herstellen, sollten Sie lokale Ressourcen so wenig wie möglich dem öffentlichen Internet verfügbar machen. Sie können die Skalierungsfunktionen und erweiterten Funktionen zum DDoS-Schutz von Azure durch Bereitstellung Ihrer gut bekannten öffentlichen Entitäten in Azure verwenden. Da diese öffentlich zugänglichen Entitäten häufig ein Ziel von DDoS-Angriffen sind, reduziert ihre Unterbringung in Azure die Auswirkungen auf Ihre lokalen Ressourcen.

## <a name="azure-ddos-protection"></a>Azure-DDoS-Schutz

Zwei DDoS-Dienstangebote von Azure bieten Schutz vor Netzwerkangriffen (Schicht 3 und 4) – DDoS-Basisschutz und DDoS-Standardschutz. 

### <a name="azure-ddos-basic-protection"></a>Azure-DDoS-Basisschutz

Basisschutz ist standardmäßig und ohne zusätzliche Kosten in der Azure-Plattform integriert. Die vollständige Skalierung und Kapazität des global bereitgestellten Azure-Netzwerks bietet über AlwaysOn-Datenverkehrsüberwachung und Risikominderung in Echtzeit Schutz vor häufigen Vermittlungsschichtangriffen. Der DDoS-Basisschutz erfordert keine Konfiguration oder Anwendungsänderungen durch den Benutzer. Alle Azure-Dienste einschließlich der PaaS-Dienste wie Azure DNS werden vom Basisschutz geschützt.

![](media/azure-ddos-best-practices/image3.png)

Der Azure-DDoS-Basisschutz besteht sowohl aus Software- als auch Hardwarekomponenten. Eine Softwaresteuerungsebene entscheidet, wann wo welche Art von Datenverkehr über Hardwareappliances gelenkt werden soll, die Angriffsdatenverkehr analysieren und entfernen. Die Steuerungsebene trifft diese Entscheidung basierend auf einer infrastrukturweit gültigen *Richtlinie* für den DDoS-Schutz, die statisch festgelegt ist und für alle Azure-Kunden universell angewendet wird.

Die Richtlinie für DDoS Protection gibt z.B. an, bei welchem Datenverkehrsvolumen der Schutz *ausgelöst* werden sollte (d.h. der Datenverkehr des Mandanten über Datenbereinigungsappliances geleitet werden sollte), und wie die Datenbereinigungsappliances das Risiko eines Angriffs *mindern* sollten.

Der Azure-DDoS-Basisschutz-Dienst ist auf Infrastruktur und Azure-Plattform ausgerichtet. Er verringert den Datenverkehr, wenn er eine Rate überschreitet, die so signifikant ist, dass sie sich in einer mehrinstanzenfähigen Umgebung auf mehrere Kunden auswirken kann. Er stellt keine Warnungen oder individuell für einen Kunden angepasste Richtlinien bereit.

### <a name="azure-ddos-standard-protection"></a>Azure-DDoS-Standardschutz

Der Standardschutz bietet verbesserte DDoS-Entschärfungsfunktionen und wird automatisch optimiert, um Ihre spezifischen Azure-Ressourcen in einem virtuellen Netzwerk zu schützen. Der Schutz kann einfach in jedem neuen oder vorhandenen virtuellen Netzwerk aktiviert werden und erfordert keine Änderung von Anwendung oder Ressource. Er hat mehrere Vorteile gegenüber dem Basisdienst, z.B. Protokollierung, Warnungen und Telemetrie. Im Folgenden werden die Hauptunterscheidungsmerkmale des Azure DDoS Protection Standard-Diensts dargelegt.

#### <a name="adaptive-realtime-tuning"></a>Adaptive Echtzeitoptimierung

Der Azure DDoS Protection Basic-Dienst schützt Kunden, jedoch nur, um Auswirkungen auf andere Kunden zu verhindern. Wenn z.B. ein Dienst für ein typisches Volumen legitimen eingehenden Datenverkehrs bereitgestellt wird, das kleiner ist als die *Triggerrate* der infrastrukturweiten DDoS-Schutzrichtlinie, könnte ein DDoS-Angriff auf die Ressourcen dieses Kunden unbemerkt bleiben. Allgemeiner gesagt: Die komplexe Natur aktueller Angriffe (z.B. Multi-Vektor-DDoS) als auch das anwendungsspezifische Verhaltensweisen von Mandanten erfordern kundenspezifische, angepasste Schutzrichtlinien.
Dies erfolgt mithilfe von zwei Einblicken:

1. Automatische Ermittlung der Datenverkehrsmuster der Schichten 3 und 4 pro Kunde (pro IP) und
2. Minimieren falsch positiver Ergebnisse – davon ausgehend, dass die Skalierung in Azure das Absorbieren signifikanter Datenverkehrsmengen erlaubt.

![](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring--alerting"></a>DDoS-Schutz: Telemetriedaten, Überwachung und Warnungen

Mit DDoS Protection Standard stellen wir während der Dauer eines DDoS-Angriffs umfangreiche Telemetriedaten über [Azure Monitor](/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) zur Verfügung. Warnungen können für beliebige durch den DDoS-Schutz verfügbar gemachte Azure Monitor-Metriken konfiguriert werden. Protokollierung kann mit Splunk (Azure Event Hubs), Azure Log Analytics und Azure Storage für die erweiterte Analyse über die Schnittstelle für die Azure Monitor-Diagnose integriert werden.

##### <a name="ddos-mitigation-policies"></a>DDoS-Entschärfungsrichtlinien

Klicken Sie im Azure-Portal auf **Überwachen** \> **Metriken**. Wählen Sie in der Anzeige **Metriken** die Ressourcengruppe, den Ressourcentyp „Öffentliche IP-Adresse“ und Ihre öffentliche Azure-IP-Adresse aus. DDoS-Metriken werden in dem verfügbaren Metrikenbereich angezeigt.

DDoS Protection Standard wendet **drei automatisch optimierte Entschärfungsrichtlinien (TCP-SYN, TCP und UDP)** für jede öffentliche IP-Adresse der geschützten Ressource in dem VNET an, das DDoS aktiviert hat. Sie können die Schwellenwerte der Richtlinien anzeigen, indem Sie die Metrik **„Eingehende Pakete zum Auslösen einer DDoS-Entschärfung“** auswählen.

![](media/azure-ddos-best-practices/image7.png)

Die Schwellenwerte der Richtlinien werden automatisch über unsere auf Machine Learning basierende Netzwerkdatenverkehrs-Profilerstellung konfiguriert. Die DDoS-Entschärfung wird nur dann für eine IP-Adresse durchgeführt, die einem Angriff ausgesetzt ist, wenn der Richtlinienschwellenwert überschritten wird.

##### <a name="under-ddos-attack"></a>Während eines DDoS-Angriffs

Wenn die öffentliche IP-Adresse einem Angriff ausgesetzt ist, wird der Metrikwert „einem DDoS Angriff ausgesetzt oder nicht“ auf 1 gesetzt, wenn eine Entschärfung des Datenverkehrsangriffs ausgeführt wird.

![](media/azure-ddos-best-practices/image8.png)

Sie sollten eine Warnung für diese Metrik konfigurieren, um benachrichtigt zu werden, wenn eine aktive DDoS-Entschärfung für Ihre öffentliche IP-Adresse durchgeführt wird.

Weitere Informationen finden Sie unter [Verwalten des Azure DDoS-Standardschutzs mithilfe des Azure-Portals](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="resource-attacks"></a>Ressourcenangriffe

Speziell für Ressourcenangriffe auf die Anwendungsschicht sollten Kunden Web Application Firewall (WAF) konfigurieren, um Webanwendungen zu schützen. WAF untersucht eingehenden Webdatenverkehr, um SQL-Einfügungen, Cross-Site Scripting, DDoS und andere Schicht 7-Angriffe zu blockieren. Azure stellt [WAF als Feature von Application Gateway](../application-gateway/application-gateway-web-application-firewall-overview.md) bereit, das zentralisierten Schutz Ihrer Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken bietet. Es sind über den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1) Varianten von WAF-Angeboten bei Azure-Partnern erhältlich, die möglicherweise für Ihre Anforderungen besser geeignet sind.

Auch Web Application Firewalls sind anfällig für volumetrische und Zustandserschöpfungsangriffe. Sie sollten zum Schutz vor volumetrischen und Protokollangriffen unbedingt den DDoS-Standardschutz auf dem virtuellen WAF-Netzwerk aktivieren. Weitere Informationen finden Sie im Referenzarchitekturabschnitt.

### <a name="protection-planning"></a>Planen des Schutzes

Planung und Vorbereitung sind entscheidend, um zu verstehen, wie ein System sich während eines DDoS-Angriffs verhalten wird. Diese Planung und Vorbereitung hilft Ihnen auch beim Entwurf eines Incident Management -Reaktionsplans.

Kunden sollten sicherstellen, dass DDoS Protection Standard für das virtuelle Netzwerk der dem Internet zugewandten Endpunkte aktiviert ist. Das Konfigurieren von DDoS-Warnungen hilft Ihnen, ständig ein wachsames Auge auf mögliche Angriffe auf Ihre Infrastruktur zu richten. Kunden müssen ihre Anwendungen unabhängig überwachen. Sie müssen das normale Verhalten der Anwendung verstehen. Wenn die Anwendung sich während eines DDoS-Angriffs nicht erwartungsgemäß verhält, müssen Maßnahmen ergriffen werden.

#### <a name="ddos-attacks-orchestration"></a>DDoS-Angriffsorchestrierung

Sie sollten Ihre Annahmen über die Reaktionen Ihrer Dienste auf einen Angriff unbedingt noch vor dem Ernstfall mithilfe regelmäßiger Simulationen testen. Überprüfen Sie während der Tests, ob Ihre Dienste oder Anwendungen weiterhin wie erwartet funktionieren und der Endbenutzer keine Unterbrechung merkt. Identifizieren Sie Lücken sowohl vom technologischen als auch Prozessstandpunkt, und beziehen Sie sie in die DDoS-Reaktionsstrategie ein. Generell sollten Sie derartige Tests in Stagingumgebungen oder außerhalb der Spitzenzeiten ausführen, um Auswirkungen auf die Produktionsumgebung zu minimieren.

Wir haben mit [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) eine Partnerschaft zum Erstellen einer Schnittstelle gebildet, wo Kunden von Azure für Simulationen Datenverkehr für unter DDoS-Schutz stehende öffentliche Endpunkte generieren können. Die [BreakPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud)-Simulation erlaubt Ihnen Folgendes:

- Überprüfen, wie Microsoft Azure DDoS Protection Ihre Azure-Ressourcen vor DDoS-Angriffen schützt

- Optimieren Ihres Prozesses der Reaktion auf Incidents während DDoS-Angriffen

- Dokumentieren der DDoS-Kompatibilität

- Schulen Ihrer Netzwerksicherheitsteams

Cybersicherheit ist eine unablässige Schlacht, die ständige Verteidigungsinnovationen erfordert. Der DDoS-Standardschutz von Azure ist ein Angebot nach neuestem Entwicklungsstand für Kunden mit einer effektiven Lösung zur Entschärfung zunehmend komplexerer DDoS-Angriffe.

## <a name="components-of-a-ddos-response-strategy"></a>Komponenten einer DDoS-Reaktionsstrategie

Wenn ein DDoS-Angriff sich gegen Ihre Azure-Ressourcen richtet, ist in den meisten Fällen vom Standpunkt des Endbenutzers aus nur minimale Intervention erforderlich. Dennoch garantiert die Einbeziehung der DDoS-Entschärfung im Rahmen der Strategie der Organisation zur Reaktion auf Incidents minimale Auswirkungen auf die Geschäftskontinuität.

### <a name="microsoft-threat-intelligence"></a>Microsoft Threat Intelligence

Microsoft bietet ein umfassendes Threat Intelligence-Netzwerk, das die kollektiven Kenntnisse einer ausgedehnten Sicherheitscommunity verwendet, die Onlinedienste und Partner von Microsoft sowie Beziehungen in der Internet-Sicherheitscommunity unterstützt. Als wichtiger Infrastrukturanbieter empfängt Microsoft frühzeitig Warnungen zu Bedrohungen und übernimmt Threat Intelligence von anderen Microsoft-Onlinediensten und dem globalen Kundenstamm von Microsoft. Die gesamte Threat Intelligence von Microsoft wird wieder in die Azure DDoS Protection-Produkte integriert.

Darüber hinaus führt die Digital Crimes Unit (DCU) von Microsoft offensive Strategien gegen Botnetze durch, eine häufige Befehls- und Steuerungsquelle bei DDoS-Angriffen.

### <a name="perform-a-risk-evaluation-of-your-azure-resources"></a>Durchführen einer Risikobewertung für Ihre Azure-Ressourcen

Sie müssen unbedingt fortlaufend wissen, wie hoch Ihr Risiko von DDoS-Angriffen ist. Kunden sollten sich in regelmäßigen Abständen selbst fragen: Welche neuen öffentlich verfügbaren Azure-Ressourcen benötigen Schutz? Gibt es Single Points of Failure im Dienst? Wie können Dienste isoliert werden, um die Auswirkungen eines Angriffs einzuschränken, aber die Dienste legitimen Kunden weiterhin verfügbar zu machen? Gibt es virtuelle Netzwerke, in denen DDoS Protection Standard aktiviert werden sollte, aber noch nicht aktiviert ist? Sind meine Dienste mehrere Regionen übergreifend aktiv / aktiv mit Failover?

### <a name="customer-ddos-response-team"></a>Kunden-DDoS-Reaktionsteam

Das Erstellen eines DDoS-Reaktionsteams ist ein wichtiger Schritt, um eine effiziente und schnelle Reaktion auf einen Angriff zu gewährleisten. Kunden müssen verschiedene Kontakte in Ihrer Organisation angeben, die Planung und Ausführung leiten. Das DDoS-Reaktionsteam muss fundierte Kenntnisse des Azure DDoS Protection Standard-Diensts besitzen und in der Lage sein, einen Angriff nach Bedarf in Koordination mit verschiedenen internen und externen Kunden einschließlich des Microsoft-Supportteams zu identifizieren und entschärfen.

Microsoft empfiehlt Ihnen, Planungs- und Simulationsübungen des DDoS-Reaktionsteams, einschließlich Skalierungstests, als normalen Bestandteil Ihrer Dienstverfügbarkeits- und Kontinuitätsplanung einzubeziehen. 

### <a name="during-an-attack"></a>Während eines Angriffs

Azure DDoS Protection Standard identifiziert und entschärft DDoS-Angriffe ohne jegliches Eingreifen des Benutzers. Um benachrichtigt zu werden, wenn eine Entschärfung für eine geschützte öffentliche IP-Adresse aktiv ist, können Sie in der Metrik „einem DDoS Angriff ausgesetzt oder nicht“ [eine Warnung konfigurieren](../virtual-network/ddos-protection-manage-portal.md). Sie können außerdem wie gewünscht Warnungen für andere DDoS-Metriken überprüfen und erstellen, um das Ausmaß des Angriffs, den gelöschten Datenverkehr usw. zu verstehen.

#### <a name="when-to-contact-microsoft-support"></a>Wann Sie den Microsoft-Support kontaktieren sollten

- Wenn Sie während eines DDoS-Angriffs feststellen, dass die Leistung der geschützten Ressource erheblich beeinträchtig wird oder die Ressource nicht verfügbar ist.

- Wenn Sie vermuten, dass der DDoS Protection-Dienst sich nicht wie erwartet verhält. Der DDoS Protection-Dienst initiiert nur dann eine Entschärfung, wenn die folgenden Kriterien erfüllt sind:

    - Der Metrikwert „Richtlinie zum Auslösen der DDoS-Entschärfung“ (TCP/TCP-SYN/UDP) ist niedriger als der Datenverkehr, der von der öffentlichen IP-Adresse empfangen wird, die eine geschützte Ressource ist.

- Wenn Sie wissen, dass ein geplantes virales Ereignis bevorsteht, das zu einem beträchtlichen Anstieg des Netzwerkdatenverkehrs führen wird.

- Wenn ein Täter einen DDoS-Angriff auf Ihre Ressourcen angedroht hat.

Erstellen Sie für Fälle mit gravierenden geschäftlichen Auswirkungen ein Schweregrad-A-[Supportticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>Schritte nach dem Angriff

Nach einem Angriff sollten Sie immer Bilanz ziehen und ggf. die DDoS-Reaktionsstrategie anpassen. Ziehen Sie Folgendes in Betracht:

- Traten aufgrund mangelnder skalierbarer Architektur Unterbrechungen des Dienstes bzw. Unterbrechungen für den Endbenutzer auf?

- Welche Anwendungen oder Dienste waren am meisten betroffen?

- Wie effektiv war die DDoS-Reaktionsstrategie, und wie kann sie verbessert werden?

Wenn Sie vermuten, dass Sie einem DDoS-Angriff ausgesetzt sind, eskalieren Sie dies über die normalen Supportkanäle von Azure.

## <a name="ddos-protection-reference-architectures"></a>DDoS-Schutz-Referenzarchitekturen

DDoS Protection Standard ist vorgesehen [für Dienste, die in einem virtuellen Netzwerk bereitgestellt werden](../virtual-network/virtual-network-for-azure-services.md). Für andere Dienste gilt das vorgegebene DDoS Basic Protection. Die unten beschriebenen spezifischen Referenzarchitekturen sind nach Szenarien angeordnet, wobei Architekturmuster gruppiert sind.

### <a name="virtual-machine-windowslinux-workloads"></a>Die Arbeitsauslastungen virtueller Computer (Windows/Linux)

#### <a name="application-running-on-load-balanced-vms"></a>Anwendung wird auf VMs mit Lastenausgleich ausgeführt

Diese Referenzarchitektur zeigt eine Reihe von bewährten Methoden für die Ausführung mehrerer virtueller Windows-Computer (VMs) in einer Skalierungsgruppe hinter einem Lastenausgleich, um die Verfügbarkeit und Skalierbarkeit zu verbessern. Diese Architektur kann für eine zustandslose Arbeitsauslastung wie einen Webserver verwendet werden.

![](media/azure-ddos-best-practices/image9.png)

In dieser Architektur wird eine Workload auf mehrere VM-Instanzen verteilt. Es gibt eine einzelne öffentliche IP-Adresse, und der Internetdatenverkehr wird durch einen Lastenausgleich auf die VMs verteilt. DDoS Protection Standard ist im virtuellen Netzwerk des Azure-Lastenausgleichsmodul (Internet) aktiviert, dem die öffentliche IP-Adresse zugeordnet wurde.

Beim Lastenausgleich werden die eingehenden Internetanforderungen an die VM-Instanzen verteilt. VM-Skalierungsgruppen ermöglichen das horizontale Hoch- oder Herunterskalieren der Anzahl der VMs – und zwar sowohl manuell als auch auf vordefinierten Regeln basierend automatisch. Dies ist wichtig, wenn die Ressource einem DDoS-Angriff ausgesetzt ist. Lesen Sie diesen [Artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm), um weitere Informationen zu dieser Referenzarchitektur zu erhalten.

#### <a name="applications-running-on-windows-n-tier"></a>Auf einer Windows-N-Schicht ausgeführte Anwendung

Es gibt viele Möglichkeiten für die Implementierung einer n-schichtigen Architektur. Das Diagramm zeigt eine typische 3-schichtige Webanwendung. Diese Architektur baut auf den Informationen unter [Ausführen von VMs mit Lastenausgleich zur Steigerung von Skalierbarkeit und Verfügbarkeit](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm) auf. In der Internet- und Unternehmensschicht werden VMs mit Lastenausgleich verwendet.

![](media/azure-ddos-best-practices/image10.png)

In der obigen Architektur ist DDoS Protection Standard im virtuellen Netzwerk aktiviert. Alle öffentlichen IP-Adressen im virtuellen Netzwerk erhalten DDoS-Schutz für Schicht 3/Schicht 4. Für den Schutz von Layer 7 sollte Application Gateway in der WAF-SKU bereitgestellt werden. Lesen Sie diesen [Artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier), um weitere Informationen zu dieser Referenzarchitektur zu erhalten.

#### <a name="paas-web-application"></a>PaaS-Webanwendung

Diese Referenzarchitektur zeigt die Ausführung einer Azure App Service-Anwendung in einer einzelnen Region. Diese Architektur demonstriert eine Reihe bewährter Verfahren für eine Webanwendung, die [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/) und [Azure SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/) verwendet.
Die Standby-Region ist für Failoverszenarien eingerichtet.

![](media/azure-ddos-best-practices/image11.png)

Traffic Manager leitet eingehende Anforderungen an Application Gateway in einer der Regionen weiter. Während des normalen Betriebs werden Anforderungen an Application Gateway in der aktiven Region weitergeleitet. Wenn diese Region nicht mehr verfügbar ist, führt Traffic Manager ein Failover zu Application Gateway in der Standby-Region aus.

Der gesamte für die Webanwendung bestimmte Datenverkehr aus dem Internet wird über den Traffic Manager an die [öffentliche Application Gateway-IP-Adresse](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) weitergeleitet. In diesem Szenario ist der App-Dienst (Web-Apps) selbst nicht direkt nach außen gerichtet und wird von Application Gateway geschützt. Sie sollten die Application Gateway-WAF-SKU (Verhinderungsmodus) zum Schutz vor Angriffen auf Schicht 7 (HTTP/HTTPS/Websocket) konfigurieren. Darüber hinaus werden die Web-Apps so konfiguriert, dass sie [Datenverkehr nur über die Application Gateway-IP-Adresse akzeptieren](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).

Lesen Sie diesen [Artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region), um weitere Informationen zu dieser Referenzarchitektur zu erhalten.

### <a name="mitigation-for-non-web-paas-services"></a>Entschärfung für Non-Web-PaaS-Dienste

#### <a name="hdinsight-on-azure"></a>HDInsight unter Azure

Diese Referenzarchitektur zeigt das Konfigurieren von DDoS Protection Standard für [HDInsight-Cluster](https://docs.microsoft.com/azure/hdinsight/) in Azure. Sie müssen sicherstellen, dass der HDInsight-Cluster mit einem virtuellen Netzwerk verknüpft und DDoS Protection in diesem virtuellen Netzwerk aktiviert ist.

![](media/azure-ddos-best-practices/image12.png)

![](media/azure-ddos-best-practices/image13.png)

In dieser Architektur wird der für den HDInsight-Cluster bestimmte Datenverkehr aus dem Internet an die öffentliche IP-Adresse weitergeleitet, die dem HDInsight-Gatewaylastenausgleich zugeordnet ist. Der Gatewaylastenausgleich sendet dann den Datenverkehr direkt an die Hauptknoten oder Workerknoten. Wenn DDoS Protection Standard im virtuellen HDInsight-Netzwerk aktiviert wird, erhalten alle öffentlichen IP-Adressen im virtuellen Netzwerk DDoS-Schutz für Schicht 3 / Schicht 4. Die oben genannte Referenzarchitektur kann mit Referenzarchitekturen für N-Schicht/mehrere Regionen kombiniert werden.

Weitere Details zu dieser Referenzarchitektur finden Sie in der Dokumentation [Erweitern von Azure HDInsight per Azure Virtual Network](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="azure-api-management"></a>Azure API Management

Diese Referenzarchitektur schützt den öffentlichen Endpunkt der Ressourcenveröffentlichung-APIs von [API Management](../api-management/api-management-key-concepts.md) für Kunden außerhalb der Organisation. APIM sollte zum Aktivieren des DDoS-Schutzes in einem externen virtuellen Netzwerk bereitgestellt werden.

![](media/azure-ddos-best-practices/image15.png)

Durch Konfigurieren des externen virtuellen Netzwerks ist der Zugriff auf Gateway und Entwicklerportal von API Management aus dem öffentlichen Internet über ein öffentliches Lastenausgleichsmodul möglich. In dieser Architektur ist DDoS Protection Standard im externen virtuellen Netzwerk des virtuellen APIM-Netzwerks aktiviert. Datenverkehr wird über das Internet an die öffentliche IP-Adresse des APIM weitergeleitet, die vor Netzwerkangriffen auf Schicht 3 / Schicht 4 geschützt ist. Zum Schutz vor HTTP/HTTPs-Angriffen auf Schicht 7 können Sie ein Application Gateway im WAF-Modus konfigurieren.

Eine Liste der zusätzlichen Dienste, die in einem virtuellen Netzwerk bereitgestellt werden und für DDoS Protection Standard konfiguriert werden können, finden Sie [hier](../virtual-network/virtual-network-for-azure-services.md). DDoS Protection Standard unterstützt nur Azure Resource Manager-Ressourcen. *Bereitstellung einer eingefügten Anwendungsdienstumgebung (Application Service Environment, ASE) in einem VNET mit einer öffentlichen IP-Adresse wird nicht nativ unterstützt.* Ausführliche Informationen zum Schützen der ASE-Umgebung finden Sie in diesem Abschnitt.

## <a name="next-steps"></a>Nächste Schritte

* [Azure DDoS Protection-Produktseite](https://azure.microsoft.com/services/ddos-protection/)

* [Azure DDoS Protection-Blog](http://aka.ms/ddosblog)

* [Übersicht über Azure DDoS Protection Standard](../virtual-network/ddos-protection-overview.md)

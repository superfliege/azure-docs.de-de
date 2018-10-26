---
title: Häufig gestellte Fragen zu Azure Application Gateway
description: Diese Seite enthält Antworten auf häufig gestellte Fragen zu Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/6/2018
ms.author: victorh
ms.openlocfilehash: 7b2a550c902e85caf02f05fcbbe5dd7b02acd0cc
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868852"
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Häufig gestellte Fragen zu Azure Application Gateway

## <a name="general"></a>Allgemein

**F: Was ist Application Gateway?**

Azure Application Gateway ist ein ADC (Application Delivery Controller) als Dienst und bietet verschiedene Lastenausgleichsfunktionen auf Schicht 7 für Ihre Anwendung. Er bietet einen hoch verfügbaren und skalierbaren Dienst, der vollständig von Azure verwaltet wird.

**F: Welche Funktionen werden von Application Gateway unterstützt?**

Application Gateway unterstützt automatische Skalierung, SSL-Abladung und End-to-End-SSL, Web Application-Firewall, cookiebasierte Sitzungsaffinität, Routing auf URL-Pfadbasis, Multi-Site-Hosting und vieles mehr. Eine vollständige Liste der unterstützten Funktionen finden Sie unter [Einführung in Application Gateway](application-gateway-introduction.md).

**F: Was ist der Unterschied zwischen Application Gateway und Azure Load Balancer?**

Application Gateway ist ein Lastenausgleich auf Ebene 7, was bedeutet, dass es nur mit Webdatenverkehr (HTTP/HTTPS/WebSocket) funktioniert. Es unterstützt Funktionen wie SSL-Beendigung, cookiebasierte Sitzungsaffinität und Roundrobin für den Lastenausgleich von Datenverkehr. Load Balancer bewirkt einen Lastenausgleich des Datenverkehrs auf Schicht 4 (TCP/UDP).

**F: Welche Protokolle werden von Application Gateway unterstützt?**

Application Gateway unterstützt HTTP, HTTPS, HTTP/2 und WebSocket.

**F: Wie unterstützt Application Gateway HTTP/2?**

Die Unterstützung des HTTP/2-Protokolls ist nur für Clients verfügbar, die mit Application Gateway-Listenern verbunden sind. Die Kommunikation mit Back-End-Serverpools erfolgt über HTTP/1.1. 

Die HTTP/2-Unterstützung ist standardmäßig deaktiviert. Das folgende Beispiel eines Azure PowerShell-Codeausschnitts zeigt, wie Sie sie aktivieren:

```
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm
$gw.EnableHttp2 = $true
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

**F: Welche Ressourcen werden derzeit als Teil des Back-End-Pools unterstützt?**

Back-End-Pools können Netzwerkkarten, VM-Skalierungsgruppen, öffentliche IP-Adressen, interne IP-Adressen, vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDN) und Back-Ends mit mehreren Mandanten wie Azure-Web-Apps umfassen. Mitglieder des Application Gateway-Back-End-Pools sind nicht an eine Verfügbarkeitsgruppe gebunden. Mitglieder von Back-End-Pools können auf mehrere Cluster und Rechenzentren verteilt sein oder sich außerhalb von Azure befinden, sofern sie über IP-Konnektivität verfügen.

**F: In welchen Regionen ist der Dienst verfügbar?**

Application Gateway ist in allen Regionen des globalen Azure verfügbar. Er ist auch in [Azure China](https://www.azure.cn/) und [Azure Government](https://azure.microsoft.com/overview/clouds/government/) verfügbar.

**F: Ist dies eine dedizierte Bereitstellung für mein Abonnement, oder wird sie zur gemeinsamen Nutzung für Kunden freigegeben?**

Application Gateway ist eine dedizierte Bereitstellung in Ihrem virtuellen Netzwerk.

**F: Wird die Umleitung von HTTP zu HTTPS unterstützt?**

Die Umleitung wird unterstützt. Weitere Informationen finden Sie in der [Übersicht über die Umleitung in Application Gateway](application-gateway-redirect-overview.md).

**F: In welcher Reihenfolge werden Listener verarbeitet?**

Listener werden gemäß der Anzeigereihenfolge verarbeitet. Wenn also ein grundlegender Listener zu einer eingehenden Anforderung passt, wird diese zuerst verarbeitet.  Listener für mehrere Standorte müssen vor einem grundlegenden Listener konfiguriert werden, um zu gewährleisten, dass der Datenverkehr an das richtige Back-End weitergeleitet wird.

**F: Wo finde ich IP und DNS von Application Gateway?**

Wenn Sie eine öffentliche IP-Adresse als Endpunkt verwenden, können diese Informationen auf der öffentlichen IP-Adressressource oder im Portal auf der Seite „Übersicht“ für Application Gateway gefunden werden. Für die interne IP-Adressen können diese Informationen auf der Seite „Übersicht“ gefunden werden.

**F: Ändert sich die IP-Adresse oder der DNS-Name während der Lebensdauer von Application Gateway?**

Die VIP kann sich ändern, wenn das Application Gateway beendet und gestartet wird. Der zugeordnete DNS-Name von Application Gateway ändert sich während des Lebenszyklus des Gateways nicht. Daher wird empfohlen, einen CNAME-Alias zu verwenden und damit auf die DNS-Adresse von Application Gateway zu verweisen.

**F: Unterstützt Application Gateway statische IP-Adressen?**

Ja, die Application Gateway V2-SKU unterstützt statische öffentliche IP-Adressen. Die V1-SKU unterstützt statische interne IP-Adressen.

**F: Unterstützt Application Gateway mehrere öffentliche IP-Adressen auf dem Gateway?**

Nur eine öffentliche IP-Adresse wird von einem Application Gateway unterstützt.

**F: Wie groß soll ich mein Subnetz für Application Gateway auslegen?**

Application Gateway nutzt eine private IP-Adresse pro Instanz sowie eine weitere private IP-Adresse, wenn eine private Front-End-IP-Konfiguration konfiguriert ist. Darüber hinaus reserviert Azure die ersten vier IP-Adressen sowie die letzte IP-Adresse in jedem Subnetz für die interne Verwendung.
Wenn ein Application Gateway beispielsweise auf drei Instanzen festgelegt und keine private Front-End-IP-Konfiguration vorhanden ist, ist eine Größe von mindestens /29 für das Subnetz erforderlich. In diesem Fall verwendet das Application Gateway drei IP-Adressen. Wenn Sie drei Instanzen und eine IP-Adresse für die private Front-End-IP-Konfiguration haben, ist eine Größe von mindestens /28 für das Subnetz erforderlich, da vier IP-Adressen erforderlich sind.

**F: Werden X-Forwarded-For-Header von Application Gateway unterstützt?**

Ja, Application Gateway fügt X-Forwarded-For-, X-Forwarded-Proto- und X-Forwarded-Port-Header in die Anforderung ein, die an das Back-End weitergeleitet wird. Das Format für den X-Forwarded-For-Header ist eine durch Trennzeichen getrennte Liste der Form „IP:Port“. Die gültigen Werte für X-Forwarded-Proto sind „http“ und „https“. X-Forwarded-Port gibt den Port an, an dem die Anforderung das Application Gateway erreicht hat.

Application Gateway fügt auch den X-Original-Hostheader ein, der den ursprünglichen Hostheader enthält, mit dem die Anforderung eingegangen ist. Dieser Header ist nützlich in Szenarien wie der Azure-Websiteintegration. Dabei wird der eingehende Hostheader modifiziert, bevor der Datenverkehr zum Back-End weitergeleitet wird.

**F: Wie lange dauert das Bereitstellen eines Application Gateway? Funktioniert das Application Gateway nach einer Aktualisierung immer noch?**

Die Ausführung neuer Bereitstellungen von Application Gateway V1-SKU kann bis zu 20 Minuten dauern. Beim Vornehmen von Änderungen an Größe/Anzahl der Instanzen wird der Betrieb nicht unterbrochen, und das Gateway bleibt währenddessen aktiv.

Die Bereitstellung von V2-SKUs nimmt etwa fünf bis sechs Minuten in Anspruch.

## <a name="configuration"></a>Konfiguration

**F: Wird Application Gateway immer in einem virtuellen Netzwerk bereitgestellt?**

Ja, Application Gateway wird immer in einem Subnetz des virtuellen Netzwerks bereitgestellt. Dieses Subnetz kann nur Application Gateway-Instanzen enthalten.

**F: Kann Application Gateway mit Instanzen außerhalb des eigenen virtuellen Netzwerks kommunizieren?**

Application Gateway kann mit Instanzen außerhalb des eigenen virtuellen Netzwerks kommunizieren, sofern eine IP-Verbindung vorhanden ist. Wenn Sie die Verwendung interner IP-Adressen als Back-End-Pool-Mitglieder planen, ist [VNet-Peering](../virtual-network/virtual-network-peering-overview.md) oder [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) erforderlich.

**F: Kann ich im Application Gateway-Subnetz noch etwas anderes bereitstellen?**

Nein, aber Sie können weitere Application Gateway-Instanzen im Subnetz bereitstellen.

**F: Werden Netzwerksicherheitsgruppen im Application Gateway-Subnetz unterstützt?**

Netzwerksicherheitsgruppen werden im Application Gateway-Subnetz mit folgenden Einschränkungen unterstützt:

* Ausnahmen müssen für eingehenden Datenverkehr an den Ports 65503 bis 65534 für die Application Gateway V1-SKU und an den Ports 65200 bis 65535 für die V2-SKU eingerichtet werden. Dieser Portbereich ist für die Kommunikation mit der Azure-Infrastruktur erforderlich. Sie werden von Azure-Zertifikaten geschützt (gesperrt). Ohne entsprechende Zertifikate können externe Entitäten, einschließlich der Kunden dieser Gateways, keine Änderungen an diesen Endpunkten vornehmen.

* Die ausgehende Internetverbindung kann nicht blockiert sein.

* Datenverkehr vom AzureLoadBalancer-Tag muss zulässig sein.

**F: Werden benutzerdefinierte Routen im Application Gateway-Subnetz unterstützt?**

Benutzerdefinierte Routen (User-Defined Routes, UDRs) werden im Application Gateway-Subnetz unterstützt, solange sie die End-to-End-Anforderung/Antwort-Kommunikation nicht ändern.

Beispielsweise können Sie eine UDR im Application Gateway-Subnetz einrichten, um für die Paketüberprüfung auf eine Firewallappliance zu verweisen, aber Sie müssen sicherstellen, dass das Paket die vorgesehene Zielnachprüfung erreichen kann. Ein Unterlassen kann zu einem falschen Integritätstest oder Datenverkehrsrouting-Verhalten führen. Dies schließt gelernte Routen oder standardmäßige 0.0.0.0/0-Routen ein, die durch ExpressRoute oder VPN-Gateways im virtuellen Netzwerk verteilt werden.

**F: Was sind die Grenzwerte für Application Gateway? Kann ich diese Grenzwerte erhöhen?**

Unter [Application Gateway-Grenzwerte](../azure-subscription-service-limits.md#application-gateway-limits) können Sie die Grenzwerte anzeigen.

**F: Kann ich Application Gateway gleichzeitig für externen und internen Datenverkehr verwenden?**

Ja, Application Gateway unterstützt pro Instanz eine interne und eine externe IP-Adresse.

**F: Wird VNet-Peering unterstützt?**

Ja, VNet-Peering wird unterstützt und ist von Vorteil für den Lastenausgleich von Datenverkehr in anderen virtuellen Netzwerken.

**F: Kann ich mit lokalen Servern kommunizieren, wenn sie über ExpressRoute- oder VPN-Tunnel verbunden sind?**

Ja, sofern Datenverkehr zugelassen wird.

**F: Kann ich einen Back-End-Pool für das Bereitstellen mehrerer Anwendungen an unterschiedlichen Ports verwenden?**

Microservice-Architektur wird unterstützt. Dazu benötigen Sie mehrere HTTP-Einstellungen, die für das Testen an unterschiedlichen Ports konfiguriert sind.

**F: Unterstützen benutzerdefinierte Überprüfungen Platzhalter/reguläre Ausdrücke in Antwortdaten?**

Benutzerdefinierte Überprüfungen unterstützen keine Platzhalter/regulären Ausdrücke in Antwortdaten? 

**F: Wie werden Regeln verarbeitet?**

Regeln werden gemäß der Konfigurationsreihenfolge verarbeitet. Es empfiehlt sich, Regeln für mehrere Standorte vor einfachen Regeln zu konfigurieren, um zu vermeiden, dass Datenverkehr an das falsche Back-End weitergeleitet wird, weil die einfache Regel auf der Grundlage des Ports zu dem Datenverkehr passen würde, bevor die Regel für mehrere Standorte ausgewertet wurde.

**F: Was ist im Feld „Host“ für benutzerdefinierte Überprüfungen angegeben?**

Das Feld „Host“ gibt den Namen an, an den die Überprüfung zu senden ist. Nur relevant, wenn in Application Gateway mehrere Standorte konfiguriert sind. Andernfalls verwenden Sie 127.0.0.1. Dieser Wert entspricht nicht dem VM-Hostnamen und weist folgendes Format auf: \<Protokoll\>://\<Host\>:\<Port\>\<Pfad\>.

**F: Kann ich für den Application Gateway-Zugriff einige wenige Quell-IPs in eine Positivliste aufnehmen?**

Dieses Szenario können Sie mithilfe von Netzwerksicherheitsgruppen im Application Gateway-Subnetz umsetzen. Die folgenden Einschränkungen sollten für das Subnetz festgelegt werden (nach ihrer Priorität geordnet):

* Zulassen des eingehenden Datenverkehrs von Quell-IP/-IP-Adressbereich.

* Zulassen von eingehenden Anforderungen aus allen Quellen an den Ports 65503 65534 für die [Back-End-Integrität-Kommunikation](application-gateway-diagnostics.md). Dieser Portbereich ist für die Kommunikation mit der Azure-Infrastruktur erforderlich. Sie werden von Azure-Zertifikaten geschützt (gesperrt). Ohne entsprechende Zertifikate können externe Entitäten, einschließlich der Kunden dieser Gateways, keine Änderungen an diesen Endpunkten vornehmen.

* Zulassen eingehender Azure Load Balancer-Tests (AzureLoadBalancer-Tag) und von eingehendem virtuellem Netzwerkdatenverkehr (VirtualNetwork-Tag) für die [Netzwerksicherheitsgruppe](../virtual-network/security-overview.md).

* Blockieren des gesamten übrigen eingehenden Datenverkehrs mit einer Alle-verweigern-Regel.

* Zulassen von ausgehendem Datenverkehr an das Internet für alle Ziele.

**F: Kann der gleiche Port sowohl für öffentliche als auch für private Listener verwendet werden?**

Nein, dies wird nicht unterstützt.

## <a name="performance"></a>Leistung

**F: Wie unterstützt Application Gateway Hochverfügbarkeit und Skalierbarkeit?**

Die Application Gateway V1-SKU unterstützt Szenarios mit Hochverfügbarkeit, wenn zwei oder mehr Instanzen bereitgestellt werden. Azure verteilt diese Instanzen auf Update- und Fehlerdomänen, um sicherzustellen, dass nicht alle Instanzen gleichzeitig ausfallen. Die V1-SKU unterstützt Skalierbarkeit durch Hinzufügen mehrerer Instanzen des gleichen Gateways, um die Last zu teilen.

Die V2-SKU stellt automatisch sicher, dass neue Instanzen über Fehlerdomänen und Updatedomänen hinweg verteilt werden. Wenn Zonenredundanz gewählt wurde, werden die neuesten Instanzen darüber hinaus über Verfügbarkeitszonen hinweg verteilt, um Resilienz gegen Zonenausfälle zu erreichen.

**F: Wie erziele ich mit Application Gateway ein rechenzentrumsübergreifendes DR-Szenario?**

Kunden können Datenverkehr mithilfe von Traffic Manager auf mehrere Application Gateway-Instanzen in verschiedenen Rechenzentren verteilen.

**F: Wird automatische Skalierung unterstützt?**

Ja, die Application Gateway V2-SKU unterstützt automatische Skalierung. Weitere Informationen finden Sie unter [Automatische Skalierung und zonenredundantes Application Gateway (öffentliche Vorschau)](application-gateway-autoscaling-zone-redundant.md).

**F: Werden durch das manuelle Hoch- oder Herunterskalieren Ausfallzeiten verursacht?**

Es gibt keine Ausfallzeiten, da Instanzen auf Upgrade- und Fehlerdomänen verteilt sind.

**F: Unterstützt Application Gateway den Verbindungsausgleich?**

Ja. Sie können den Verbindungsausgleich konfigurieren, um die Mitglieder in einem Back-End-Pool ohne Unterbrechung zu ändern. Dadurch können vorhandene Verbindungen weiterhin an ihr vorheriges Ziel gesendet werden, bis die jeweilige Verbindung geschlossen wird oder ein konfigurierbares Zeitlimit abläuft. Verbindungsausgleich wartet nur auf die Beendigung der aktuellen aktiven Verbindungen. Application Gateway kennt den Status der Anwendungssitzung nicht.

**F: In welchen Größen ist Application Gatewaxy verfügbar?**

Application Gateway wird derzeit in drei Größen angeboten: **klein**, **mittel** und **groß**. Kleine Instanzen sind für Entwicklungs- und Testszenarien vorgesehen.

Sie können bis zu 50 Application Gateways pro Abonnement erstellen, und jedes Application Gateway kann jeweils bis zu 10 Instanzen aufweisen. Jedes Anwendungsgateway kann aus 20 HTTP-Listenern bestehen. Eine vollständige Liste mit den Einschränkungen von Anwendungsgateways finden Sie unter [Application Gateway service limits (Einschränkungen von Application Gateway)](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Die folgende Tabelle zeigt einen durchschnittlichen Leistungsdurchsatz für jede Anwendungsgatewayinstanz mit aktivierter SSL-Auslagerung:

| Durchschnittliche Größe der Back-End-Seitenantwort | Klein | Mittel | Groß |
| --- | --- | --- | --- |
| 6 KB |7,5 MBit/s |13 MBit/s |50 MBit/s |
| 100 KB |35 MBit/s |100 MBit/s |200 MBit/s |

> [!NOTE]
> Hierbei handelt es sich um ungefähre Werte für den Durchsatz des Anwendungsgateways. Der tatsächliche Durchsatz ist abhängig von verschiedenen Umgebungsdetails wie etwa durchschnittliche Seitengröße, Speicherort der Back-End-Instanzen und Verarbeitungszeit für die Seitenbereitstellung. Für genaue Leistungsangaben sollten Sie Ihre eigenen Tests ausführen. Diese Werte dienen nur als Leitfaden für die Kapazitätsplanung.

**F: Kann ich Instanzgröße ohne Unterbrechung von mittel zu groß ändern?**

Ja, Azure verteilt Instanzen auf Update- und Fehlerdomänen, um sicherzustellen, dass nicht alle Instanzen gleichzeitig ausfallen. Application Gateway unterstützt Skalierung durch Hinzufügen mehrerer Instanzen desselben Gateways, um die Last zu teilen.

## <a name="ssl-configuration"></a>SSL-Konfiguration

**F: Welche Zertifikate werden auf Application Gateway unterstützt?**

Selbstsignierte Zertifikate, Zertifizierungsstellenzertifikate und Platzhalterzertifikate werden unterstützt. Zertifikate für die erweiterte Überprüfung werden nicht unterstützt.

**F: Welche Verschlüsselungssammlungen werden derzeit von Application Gateway unterstützt?**

Von Application Gateway werden derzeit die im Anschluss angegebenen Verschlüsselungssammlungen unterstützt. Informationen zum Anpassen von SSL-Optionen finden Sie unter [Konfigurieren von SSL-Richtlinienversionen und Verschlüsselungssammlungen für Application Gateway](application-gateway-configure-ssl-policy-powershell.md).

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

**F: Unterstützt Application Gateway auch eine erneute Verschlüsselung des Datenverkehrs an das Back-End?**

Ja. Application Gateway unterstützt SSL-Abladung sowie End-to-End-SSL, das den Datenverkehr an das Back-End erneut verschlüsselt.

**F: Kann ich die SSL-Richtlinie für die Steuerung von SSL-Protokollversionen konfigurieren?**

Ja, können Sie Application Gateway zum Verweigern von TLS1.0, TLS1.1 und TLS1.2 konfigurieren. SSL 2.0 und 3.0 sind bereits in der Standardeinstellung deaktiviert und nicht konfigurierbar.

**F: Kann ich Verschlüsselungssammlungen und die Richtlinienreihenfolge konfigurieren?**

Ja, die [Konfiguration von Verschlüsselungssammlungen](application-gateway-ssl-policy-overview.md) wird unterstützt. Wenn Sie eine benutzerdefinierte Richtlinie definieren, muss mindestens eine der folgenden Verschlüsselungssammlungen aktiviert werden. Application Gateway verwendet SHA256 für die Back-End-Verwaltung.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**F: Wie viele SSL-Zertifikate werden unterstützt?**

Bis zu 20 SSL-Zertifikate werden unterstützt.

**F: Wie viele Authentifizierungszertifikate für die erneute Back-End-Verschlüsselung werden unterstützt?**

Bis zu 10 Authentifizierungszertifikate werden unterstützt, der Standardwert ist 5.

**F: Lässt sich Application Gateway systemintern in Azure Key Vault integrieren?**

Nein, Application Gateway ist nicht in Azure Key Vault integriert.

## <a name="web-application-firewall-waf-configuration"></a>Web Application Firewall (WAF)-Konfiguration

**F: Bietet die WAF-SKU alle Funktionen, die bei der Standard-SKU verfügbar sind?**

Ja, WAF unterstützt alle Funktionen in der Standard-SKU.

**F: Welche CRS-Version wird von Application Gateway unterstützt?**

Application Gateway unterstützt CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) und CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**F: Wie überwache ich WAF?**

WAF wird über Diagnoseprotokollierung überwacht wird. Weitere Informationen zu Diagnoseprotokollierung finden Sie unter [Diagnoseprotokollierung und Metriken für Application Gateway](application-gateway-diagnostics.md).

**F: Wird Datenverkehr durch den Erkennungsmodus blockiert?**

Nein, der Erkennungsmodus protokolliert nur Datenverkehr, der eine WAF-Regel ausgelöst hat.

**F: Wie passe ich WAF-Regeln an?**

Ja, WAF-Regeln können angepasst werden. Weitere Informationen zum Anpassen finden Sie unter [Anpassen von WAF-Regelgruppen und -Regeln](application-gateway-customize-waf-rules-portal.md).

**F: Welche Regeln sind derzeit verfügbar?**

WAF unterstützt derzeit CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp30) und [3.0](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013), das grundlegende Sicherheit für die meisten der zehn wichtigsten, vom Open Web Application Security Project (OWASP) identifizierten Sicherheitsrisiken bietet. Diese finden Sie unter [Die 10 wichtigsten OWASP-Sicherheitsrisiken](application-gateway-crs-rulegroups-rules.md#owasp229).

* Schutz vor Einschleusung von SQL-Befehlen

* Schutz vor websiteübergreifenden Skripts

* Schutz vor allgemeinen Webangriffen wie Befehlseinschleusung, HTTP Request Smuggling, HTTP Response Splitting und Remote File Inclusion

* Schutz vor Verletzungen des HTTP-Protokolls

* Schutz vor HTTP-Protokollanomalien, z.B. fehlende user-agent- und accept-Header des Hosts

* Verhindern von Bots, Crawlern und Scannern

 * Erkennung häufiger Fehler bei der Anwendungskonfiguration (d. h. Apache, IIS usw.)

**F: Unterstützt WAF auch DDoS-Verhinderung?**

Ja. Sie können DDos-Schutz im VNET aktivieren, in dem Application Gateway bereitgestellt wird. Dadurch wird sichergestellt, dass die VIP-Adresse des Anwendungsgateways ebenfalls durch den Azure DDos Protection-Dienst geschützt ist.

## <a name="diagnostics-and-logging"></a>Diagnose und Protokollierung

**F: Welche Arten von Protokollen sind bei Application Gateway verfügbar?**

Für Application Gateway sind drei Protokolle verfügbar. Weitere Informationen zu diesen Protokollen und anderen Diagnosefunktionen finden Sie unter [Back-End-Integrität, Diagnoseprotokolle und Metriken für Application Gateway](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog**: Das Zugriffsprotokoll enthält jede an das Application Gateway-Front-End gesendete Anforderung. Die Daten enthalten die IP des Aufrufers, die angeforderte URL, die Antwortlatenz, den Rückgabecode sowie ein- und ausgehenden Bytes. Das Zugriffsprotokoll wird alle 300 Sekunden erstellt. Dieses Protokoll enthält einen Datensatz pro Instanz von Application Gateway.
- **ApplicationGatewayPerformanceLog**: In diesem Leistungsprotokoll werden Leistungsinformationen pro Instanz erfasst, z.B. insgesamt bereitgestellte Anforderungen, Durchsatz in Byte, Anzahl von Anforderungen mit Fehlern, Anzahl von fehlerfreien und fehlerhaften Back-End-Instanzen.
- **ApplicationGatewayFirewallLog**: Dieses Firewallprotokoll enthält Anforderungen, die entweder über den Erkennungs- oder über den Schutzmodus eines Anwendungsgateways protokolliert werden, das mit Web Application Firewall konfiguriert ist.

**F: Wie erkenne ich, dass die Mitglieder meines Back-End-Pools fehlerfrei sind?**

Sie können das PowerShell-Cmdlet `Get-AzureRmApplicationGatewayBackendHealth` verwenden oder die Integrität über das Portal überprüfen; Informationen dazu finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).

**F: Welche Aufbewahrungsrichtlinie gilt für die Diagnoseprotokolle?**

Diagnoseprotokolle gelangen in die Speicherkonten von Kunden, und Kunden können die Aufbewahrungsrichtlinie je nach Präferenz festlegen. Diagnoseprotokolle können auch an einen Event Hub oder Log Analytics gesendet werden. Weitere Details finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).

**F: Wie erhalte ich die Überwachungsprotokolle für Application Gateway?**

Für Application Gateway sind Überwachungsprotokolle verfügbar. Klicken Sie im Portal auf dem Menüblatt einer Application Gateway-Instanz auf **Aktivitätsprotokoll**, um das Überwachungsprotokoll aufzurufen. 

**F: Kann ich mit Application Gateway Warnungen einrichten?**

Ja, Application Gateway unterstützt Warnungen. Warnungen werden für Metriken konfiguriert. Weitere Informationen zu Metriken in Application Gateway finden Sie unter [Application Gateway-Metriken](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics). Weitere Informationen zu Warnungen finden Sie unter [Empfangen von Warnungsbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**F: Wie lassen sich Datenverkehrsstatistiken für Application Gateway analysieren?**

Sie können Zugriffsprotokolle über eine Reihe von Mechanismen wie Azure Log Analytics, Excel, Power BI usw. anzeigen und analysieren.

Wir haben auch eine Resource Manager-Vorlage veröffentlicht, die die beliebte [GoAccess](https://goaccess.io/)-Protokollanalyse für Application Gateway Access Logs installiert und ausführt. GoAccess stellt wertvolle HTTP-Datenverkehrsstatistiken wie eindeutige Besucher, angeforderte Dateien, Hosts, Betriebssysteme, Browser, HTTP-Statuscodes und vieles mehr zur Verfügung. Weitere Informationen finden Sie in der [Infodatei im Resource Manager-Vorlagenordner auf GitHub](https://aka.ms/appgwgoaccessreadme).

**F: Die Back-End-Integrität gibt den Status „Unbekannt“ zurück. Was kann die Ursache sein?**

Der häufigste Grund ist eine Blockierung des Zugriffs auf das Back-End durch eine NSG oder einen benutzerdefinierten DNS. Weitere Informationen finden Sie unter [Back-End-Integrität, Diagnoseprotokollierung und Metriken für Application Gateway](application-gateway-diagnostics.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Application Gateway finden Sie unter [Was ist Azure Application Gateway?](overview.md)

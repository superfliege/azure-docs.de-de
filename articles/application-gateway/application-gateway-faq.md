---
title: Häufig gestellte Fragen zu Azure Application Gateway
description: Diese Seite enthält Antworten auf häufig gestellte Fragen zu Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/20/2019
ms.author: victorh
ms.openlocfilehash: c40f372d3574f940e475a6626f998adae37a6d61
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851157"
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Häufig gestellte Fragen zu Azure Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general"></a>Allgemein

### <a name="what-is-application-gateway"></a>Was ist Application Gateway?

Azure Application Gateway ist ein ADC (Application Delivery Controller) als Dienst und bietet verschiedene Lastenausgleichsfunktionen auf Schicht 7 für Ihre Anwendung. Er bietet einen hoch verfügbaren und skalierbaren Dienst, der vollständig von Azure verwaltet wird.

### <a name="what-features-does-application-gateway-support"></a>Welche Funktionen werden von Application Gateway unterstützt?

Application Gateway unterstützt automatische Skalierung, SSL-Abladung und End-to-End-SSL, Web Application-Firewall, cookiebasierte Sitzungsaffinität, Routing auf URL-Pfadbasis, Hosting für mehrere Standorte und vieles mehr. Eine vollständige Liste der unterstützten Funktionen finden Sie unter [Einführung in Application Gateway](application-gateway-introduction.md).

### <a name="what-is-the-difference-between-application-gateway-and-azure-load-balancer"></a>Was ist der Unterschied zwischen Application Gateway und Azure Load Balancer?

Application Gateway ist ein Layer-7-Load Balancer und somit nur für Webdatenverkehr (HTTP/HTTPS/WebSocket/HTTP/2) geeignet. Es unterstützt Funktionen wie SSL-Beendigung, cookiebasierte Sitzungsaffinität und Roundrobin für den Lastenausgleich von Datenverkehr. Load Balancer bewirkt einen Lastenausgleich des Datenverkehrs auf Schicht 4 (TCP/UDP).

### <a name="what-protocols-does-application-gateway-support"></a>Welche Protokolle werden von Application Gateway unterstützt?

Application Gateway unterstützt HTTP, HTTPS, HTTP/2 und WebSocket.

### <a name="how-does-application-gateway-support-http2"></a>Wie unterstützt Application Gateway HTTP/2?

Informationen zur Unterstützung des HTTP/2-Protokolls durch Application Gateway finden Sie unter [HTTP2-Unterstützung](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support).

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Welche Ressourcen werden derzeit als Teil des Back-End-Pools unterstützt?

Informationen zu den Ressourcen, die von Application Gateway unterstützt werden, finden Sie unter [Backend pool](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pool) (Back-End-Pool).

### <a name="what-regions-is-the-service-available-in"></a>In welchen Regionen ist der Dienst verfügbar?

Application Gateway ist in allen Regionen des globalen Azure verfügbar. Er ist auch in [Azure China 21Vianet](https://www.azure.cn/) und [Azure Government](https://azure.microsoft.com/overview/clouds/government/) verfügbar.

### <a name="is-this-a-dedicated-deployment-for-my-subscription-or-is-it-shared-across-customers"></a>Ist dies eine dedizierte Bereitstellung für mein Abonnement, oder wird sie zur gemeinsamen Nutzung für Kunden freigegeben?

Application Gateway ist eine dedizierte Bereitstellung in Ihrem virtuellen Netzwerk.

### <a name="is-http-https-redirection-supported"></a>Wird die Umleitung von HTTP zu HTTPS unterstützt?

Die Umleitung wird unterstützt. Weitere Informationen finden Sie in der [Übersicht über die Umleitung in Application Gateway](application-gateway-redirect-overview.md).

### <a name="in-what-order-are-listeners-processed"></a>In welcher Reihenfolge werden Listener verarbeitet?

Informationen hierzu finden Sie unter [Verarbeitungsreihenfolge von Listenern](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners).

### <a name="where-do-i-find-application-gateways-ip-and-dns"></a>Wo finde ich IP und DNS von Application Gateway?

Wenn Sie eine öffentliche IP-Adresse als Endpunkt verwenden, können diese Informationen auf der öffentlichen IP-Adressressource oder im Portal auf der Seite „Übersicht“ für Application Gateway gefunden werden. Für die interne IP-Adressen können diese Informationen auf der Seite „Übersicht“ gefunden werden.

### <a name="what-is-keep-alive-timeout-and-tcp-idle-timeout-setting-on-application-gateway"></a>Welche Einstellung wird für das Keep-Alive-Timeout und für das TCP-Leerlauftimeout in Application Gateway verwendet?

Das Keep-Alive-Timeout für die v1-SKU beträgt 120 Sekunden. Das Keep-Alive-Timeout für die v2-SKU beträgt 75 Sekunden. Das TCP-Leerlauftimeout beträgt vier Minuten (Standardwert für die Front-End-VIP-Adresse von Application Gateway).

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>Ändert sich die IP-Adresse oder der DNS-Name während der Lebensdauer von Application Gateway?

Die VIP kann sich ändern, wenn das Application Gateway beendet und gestartet wird. Der zugeordnete DNS-Name von Application Gateway ändert sich während des Lebenszyklus des Gateways nicht. Daher wird empfohlen, einen CNAME-Alias zu verwenden und damit auf die DNS-Adresse von Application Gateway zu verweisen.

### <a name="does-application-gateway-support-static-ip"></a>Unterstützt Application Gateway statische IP-Adressen?

Ja, die Application Gateway v2-SKU unterstützt statische öffentliche IP-Adressen. Die v1-SKU unterstützt statische interne IP-Adressen.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>Unterstützt Application Gateway mehrere öffentliche IP-Adressen auf dem Gateway?

Nur eine öffentliche IP-Adresse wird von einem Application Gateway unterstützt.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>Wie groß soll ich mein Subnetz für Application Gateway auslegen?

Informationen zu der Subnetzgröße, die für Ihre Bereitstellung erforderlich ist, finden Sie unter [Überlegungen zur Subnetzgröße für Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet).

### <a name="q-can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>F: Kann ich mehrere Application Gateway-Ressourcen in einem einzelnen Subnetz bereitstellen?

Ja. Sie können nicht nur mehrere Instanzen einer bestimmten Application Gateway-Bereitstellung besitzen, sondern auch eine weitere eindeutige Application Gateway-Ressource zu einem vorhandenen Subnetz hinzufügen, das eine andere Application Gateway-Ressource enthält.

Das Mischen von Standard_v2 und Standardanwendungsgateway im selben Subnetz wird nicht unterstützt.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>Werden X-Forwarded-For-Header von Application Gateway unterstützt?

Ja. Informationen zu den von Application Gateway unterstützten X-Forwarded-For-Headern finden Sie unter [Änderungen an der Anforderung](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request).

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-does-my-application-gateway-still-work-when-being-updated"></a>Wie lange dauert das Bereitstellen eines Application Gateway? Funktioniert das Application Gateway nach einer Aktualisierung immer noch?

Die Ausführung neuer Bereitstellungen der Application Gateway v1-SKU kann bis zu 20 Minuten dauern. Beim Vornehmen von Änderungen an Größe/Anzahl der Instanzen wird der Betrieb nicht unterbrochen, und das Gateway bleibt währenddessen aktiv.

Die Bereitstellung von V2-SKUs nimmt etwa fünf bis sechs Minuten in Anspruch.

### <a name="can-exchange-server-be-used-as-backend-with-application-gateway"></a>Kann Exchange Server als Back-End mit Application Gateway werden verwendet?

Nein. Application Gateway unterstützt keine E-Mail-Protokolle wie SMTP, IMAP oder POP3. 

## <a name="performance"></a>Leistung

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>Wie unterstützt Application Gateway Hochverfügbarkeit und Skalierbarkeit?

Die Application Gateway v1-SKU unterstützt Szenarien mit Hochverfügbarkeit, wenn zwei oder mehr Instanzen bereitgestellt werden. Azure verteilt diese Instanzen auf Update- und Fehlerdomänen, um sicherzustellen, dass nicht alle Instanzen gleichzeitig ausfallen. Die v1-SKU unterstützt Skalierbarkeit durch Hinzufügen mehrerer Instanzen des gleichen Gateways, um die Last zu teilen.

Die v2-SKU stellt automatisch sicher, dass neue Instanzen über Fehlerdomänen und Updatedomänen hinweg verteilt werden. Wenn Zonenredundanz gewählt wurde, werden die neuesten Instanzen darüber hinaus über Verfügbarkeitszonen hinweg verteilt, um Resilienz gegen Zonenausfälle zu erreichen.

### <a name="how-do-i-achieve-dr-scenario-across-data-centers-with-application-gateway"></a>Wie erziele ich mit Application Gateway ein rechenzentrumsübergreifendes DR-Szenario?

Kunden können Datenverkehr mithilfe von Traffic Manager auf mehrere Application Gateway-Instanzen in verschiedenen Rechenzentren verteilen.

### <a name="is-autoscaling-supported"></a>Wird automatische Skalierung unterstützt?

Ja, die Application Gateway v2-SKU unterstützt automatische Skalierung. Weitere Informationen finden Sie unter [Automatische Skalierung und zonenredundantes Application Gateway (öffentliche Vorschau)](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-manual-scale-updown-cause-downtime"></a>Werden durch das manuelle Hoch- oder Herunterskalieren Ausfallzeiten verursacht?

Es gibt keine Ausfallzeiten. Instanzen sind auf Upgrade- und Fehlerdomänen verteilt.

### <a name="does-application-gateway-support-connection-draining"></a>Unterstützt Application Gateway den Verbindungsausgleich?

Ja. Sie können den Verbindungsausgleich konfigurieren, um die Mitglieder in einem Back-End-Pool ohne Unterbrechung zu ändern. Dadurch können vorhandene Verbindungen weiterhin an ihr vorheriges Ziel gesendet werden, bis die jeweilige Verbindung geschlossen wird oder ein konfigurierbares Zeitlimit abläuft. Verbindungsausgleich wartet nur auf die Beendigung der aktuellen aktiven Verbindungen. Application Gateway kennt den Status der Anwendungssitzung nicht.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>Kann ich Instanzgröße ohne Unterbrechung von mittel zu groß ändern?

Ja, Azure verteilt Instanzen auf Update- und Fehlerdomänen, um sicherzustellen, dass nicht alle Instanzen gleichzeitig ausfallen. Application Gateway unterstützt Skalierung durch Hinzufügen mehrerer Instanzen desselben Gateways, um die Last zu teilen.

## <a name="configuration"></a>Konfiguration

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>Wird Application Gateway immer in einem virtuellen Netzwerk bereitgestellt?

Ja, Application Gateway wird immer in einem Subnetz des virtuellen Netzwerks bereitgestellt. Dieses Subnetz kann nur Application Gateway-Instanzen enthalten. Informationen zu Subnetzaspekten für Application Gateway finden Sie unter [Virtuelles Azure-Netzwerk und dediziertes Subnetz](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet).

### <a name="can-application-gateway-communicate-with-instances-outside-of-the-virtual-network-it-is-in-or-outside-of-the-subscription-it-is-in"></a>Kann Application Gateway mit Instanzen außerhalb des eigenen virtuellen Netzwerks oder des eigenen Abonnements kommunizieren?

Application Gateway kann mit Instanzen außerhalb des eigenen virtuellen Netzwerks oder des eigenen Abonnements kommunizieren, sofern eine IP-Verbindung vorhanden ist. Wenn Sie die Verwendung interner IP-Adressen als Back-End-Pool-Mitglieder planen, ist [VNet-Peering](../virtual-network/virtual-network-peering-overview.md) oder [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) erforderlich.

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>Kann ich im Application Gateway-Subnetz noch etwas anderes bereitstellen?

Nein, aber Sie können weitere Application Gateway-Instanzen im Subnetz bereitstellen.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>Werden Netzwerksicherheitsgruppen im Application Gateway-Subnetz unterstützt?

Informationen über die Netzwerksicherheitsgruppen, die im Application Gateway-Subnetz unterstützt werden, finden Sie unter [Netzwerksicherheitsgruppen im Application Gateway-Subnetz](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-on-the-application-gateway-subnet).

### <a name="are-user-defined-routes-supported-on-the-application-gateway-subnet"></a>Werden benutzerdefinierte Routen im Application Gateway-Subnetz unterstützt?

Informationen zu den benutzerdefinierten Routen, die im Application Gateway-Subnetz unterstützt werden, finden Sie unter [Im Application Gateway-Subnetz unterstützte benutzerdefinierte Routen](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet).

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>Was sind die Grenzwerte für Application Gateway? Kann ich diese Grenzwerte erhöhen?

Unter [Application Gateway-Grenzwerte](../azure-subscription-service-limits.md#application-gateway-limits) können Sie die Grenzwerte anzeigen.

### <a name="can-i-use-application-gateway-for-both-external-and-internal-traffic-simultaneously"></a>Kann ich Application Gateway gleichzeitig für externen und internen Datenverkehr verwenden?

Ja, Application Gateway unterstützt pro Instanz eine interne und eine externe IP-Adresse.

### <a name="is-vnet-peering-supported"></a>Wird VNET-Peering unterstützt?

Ja, VNet-Peering wird unterstützt und ist von Vorteil für den Lastenausgleich von Datenverkehr in anderen virtuellen Netzwerken.

### <a name="can-i-talk-to-on-premises-servers-when-they-are-connected-by-expressroute-or-vpn-tunnels"></a>Kann ich mit lokalen Servern kommunizieren, wenn sie über ExpressRoute- oder VPN-Tunnel verbunden sind?

Ja, sofern Datenverkehr zugelassen wird.

### <a name="can-i-have-one-backend-pool-serving-many-applications-on-different-ports"></a>Kann ich einen Back-End-Pool für das Bereitstellen mehrerer Anwendungen an unterschiedlichen Ports verwenden?

Microservice-Architektur wird unterstützt. Dazu benötigen Sie mehrere HTTP-Einstellungen, die für das Testen an unterschiedlichen Ports konfiguriert sind.

### <a name="do-custom-probes-support-wildcardsregex-on-response-data"></a>Unterstützen benutzerdefinierte Überprüfungen Platzhalter/reguläre Ausdrücke in Antwortdaten?

Benutzerdefinierte Überprüfungen unterstützen keine Platzhalter/regulären Ausdrücke in Antwortdaten?

### <a name="how-are-rules-processed"></a>Wie werden Regeln verarbeitet?

Informationen dazu, wie Routingregeln in Application Gateway verarbeitet werden, finden Sie unter [Verarbeitungsreihenfolge von Regeln](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules).

### <a name="what-does-the-host-field-for-custom-probes-signify"></a>Was ist im Feld „Host“ für benutzerdefinierte Überprüfungen angegeben?

Das Feld „Host“ gibt den Namen an, an den die Überprüfung zu senden ist. Nur relevant, wenn in Application Gateway mehrere Standorte konfiguriert sind. Andernfalls verwenden Sie 127.0.0.1. Dieser Wert entspricht nicht dem VM-Hostnamen und weist folgendes Format auf: \<Protokoll\>://\<Host\>:\<Port\>\<Pfad\>.

### <a name="can-i-whitelist-application-gateway-access-to-a-few-source-ips"></a>Kann ich für den Application Gateway-Zugriff einige wenige Quell-IPs in eine Positivliste aufnehmen?

Ja. Unter [Aufnehmen des Application Gateway-Zugriffs auf einige wenige Quell-IPs in eine Positivliste](https://docs.microsoft.com/azure/application-gateway/configuration-overview#whitelist-application-gateway-access-to-a-few-source-ips) erfahren Sie, wie Sie sicherstellen, dass nur Quell-IP-Adressen aus einer Positivliste auf Application Gateway zugreifen können.

### <a name="can-the-same-port-be-used-for-both-public-and-private-facing-listeners"></a>Kann der gleiche Port sowohl für öffentliche als auch für private Listener verwendet werden?

Nein, dies wird nicht unterstützt.

## <a name="configuration---ssl"></a>Konfiguration: SSL

### <a name="what-certificates-are-supported-on-application-gateway"></a>Welche Zertifikate werden für Application Gateway unterstützt?

Selbstsignierte Zertifikate, Zertifizierungsstellenzertifikate, Zertifikate für die erweiterte Überprüfung und Platzhalterzertifikate werden unterstützt.

### <a name="what-are-the-current-cipher-suites-supported-by-application-gateway"></a>Welche Verschlüsselungssammlungen werden derzeit von Application Gateway unterstützt?

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

### <a name="does-application-gateway-also-support-re-encryption-of-traffic-to-the-backend"></a>Unterstützt Application Gateway auch eine erneute Verschlüsselung des Datenverkehrs an das Back-End?

Ja. Application Gateway unterstützt SSL-Abladung sowie End-to-End-SSL, das den Datenverkehr an das Back-End erneut verschlüsselt.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>Kann ich die SSL-Richtlinie für die Steuerung von SSL-Protokollversionen konfigurieren?

Ja, können Sie Application Gateway zum Verweigern von TLS1.0, TLS1.1 und TLS1.2 konfigurieren. SSL 2.0 und 3.0 sind bereits in der Standardeinstellung deaktiviert und nicht konfigurierbar.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>Kann ich Verschlüsselungssammlungen und die Richtlinienreihenfolge konfigurieren?

Ja, die [Konfiguration von Verschlüsselungssammlungen](application-gateway-ssl-policy-overview.md) wird unterstützt. Wenn Sie eine benutzerdefinierte Richtlinie definieren, muss mindestens eine der folgenden Verschlüsselungssammlungen aktiviert werden. Application Gateway verwendet SHA256 für die Back-End-Verwaltung.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

### <a name="how-many-ssl-certificates-are-supported"></a>Wie viele SSL-Zertifikate werden unterstützt?

Bis zu 100 SSL-Zertifikate werden unterstützt.

### <a name="how-many-authentication-certificates-for-backend-re-encryption-are-supported"></a>Wie viele Authentifizierungszertifikate für die erneute Back-End-Verschlüsselung werden unterstützt?

Bis zu 10 Authentifizierungszertifikate werden unterstützt, der Standardwert ist 5.

### <a name="does-application-gateway-integrate-with-azure-key-vault-natively"></a>Lässt sich Application Gateway nativ in Azure Key Vault integrieren?

Nein, Application Gateway ist nicht in Azure Key Vault integriert.

### <a name="how-to-configure-https-listeners-for-com-and-net-sites"></a>Wie werden HTTPS-Listener für Websites vom Typ „.com“ und „.net“ konfiguriert? 

Für das Routing auf der Grundlage mehrerer Domänen (hostbasiertes Routing) können Sie Listener für mehrere Websites erstellen, in der Listenerkonfiguration HTTPS als Protokoll auswählen und die Listener mit den Routingregeln verknüpfen. Ausführlichere Informationen finden Sie unter [Anwendungsgateways – Hosten mehrerer Websites](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview). 

## <a name="configuration---web-application-firewall-waf"></a>Konfiguration: Web Application Firewall (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-with-the-standard-sku"></a>Bietet die WAF-SKU alle Funktionen, die bei der Standard-SKU verfügbar sind?

Ja, WAF unterstützt alle Funktionen in der Standard-SKU.

### <a name="what-is-the-crs-version-application-gateway-supports"></a>Welche CRS-Version wird von Application Gateway unterstützt?

Application Gateway unterstützt CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) und CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

### <a name="how-do-i-monitor-waf"></a>Wie überwache ich WAF?

WAF wird über Diagnoseprotokollierung überwacht wird. Weitere Informationen zu Diagnoseprotokollierung finden Sie unter [Diagnoseprotokollierung und Metriken für Application Gateway](application-gateway-diagnostics.md).

### <a name="does-detection-mode-block-traffic"></a>Wird Datenverkehr durch den Erkennungsmodus blockiert?

Nein, der Erkennungsmodus protokolliert nur Datenverkehr, der eine WAF-Regel ausgelöst hat.

### <a name="can-i-customize-waf-rules"></a>Kann ich WAF-Regeln anpassen?

Ja, WAF-Regeln sind anpassbar. Weitere Informationen finden Sie unter [Anpassen von WAF-Regelgruppen und -Regeln](application-gateway-customize-waf-rules-portal.md).

### <a name="what-rules-are-currently-available"></a>Welche Regeln sind derzeit verfügbar?

WAF unterstützt derzeit CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp30) und [3.0](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013), das grundlegende Sicherheit für die meisten der zehn wichtigsten, vom Open Web Application Security Project (OWASP) identifizierten Sicherheitsrisiken bietet. Diese finden Sie unter [Die 10 wichtigsten OWASP-Sicherheitsrisiken](application-gateway-crs-rulegroups-rules.md#owasp229).

* Schutz vor Einschleusung von SQL-Befehlen

* Schutz vor websiteübergreifenden Skripts

* Schutz vor allgemeinen Webangriffen wie Befehlseinschleusung, HTTP Request Smuggling, HTTP Response Splitting und Remote File Inclusion

* Schutz vor Verletzungen des HTTP-Protokolls

* Schutz vor HTTP-Protokollanomalien, z.B. fehlende user-agent- und accept-Header des Hosts

* Verhindern von Bots, Crawlern und Scannern

* Erkennung häufiger Fehler bei der Anwendungskonfiguration (d. h. Apache, IIS usw.)

### <a name="does-waf-also-support-ddos-prevention"></a>Unterstützt WAF auch DDoS-Verhinderung?

Ja. Sie können DDos-Schutz im VNET aktivieren, in dem Application Gateway bereitgestellt wird. Dadurch wird sichergestellt, dass die VIP-Adresse des Anwendungsgateways ebenfalls durch den Azure DDos Protection-Dienst geschützt ist.

## <a name="diagnostics-and-logging"></a>Diagnose und Protokollierung

### <a name="what-types-of-logs-are-available-with-application-gateway"></a>Welche Arten von Protokollen sind bei Application Gateway verfügbar?

Für Application Gateway sind drei Protokolle verfügbar. Weitere Informationen zu diesen Protokollen und anderen Diagnosefunktionen finden Sie unter [Back-End-Integrität, Diagnoseprotokolle und Metriken für Application Gateway](application-gateway-diagnostics.md).

* **ApplicationGatewayAccessLog**: Das Zugriffsprotokoll enthält jede an das Application Gateway-Front-End gesendete Anforderung. Die Daten enthalten die IP des Aufrufers, die angeforderte URL, die Antwortlatenz, den Rückgabecode sowie ein- und ausgehenden Bytes. Das Zugriffsprotokoll wird alle 300 Sekunden erstellt. Dieses Protokoll enthält einen Datensatz pro Instanz von Application Gateway.
* **ApplicationGatewayPerformanceLog**: In diesem Leistungsprotokoll werden Leistungsinformationen pro Instanz erfasst, z.B. insgesamt bereitgestellte Anforderungen, Durchsatz in Byte, Anzahl von Anforderungen mit Fehlern, Anzahl von fehlerfreien und fehlerhaften Back-End-Instanzen.
* **ApplicationGatewayFirewallLog**: Dieses Firewallprotokoll enthält Anforderungen, die entweder über den Erkennungs- oder über den Schutzmodus eines Anwendungsgateways protokolliert werden, das mit Web Application Firewall konfiguriert ist.

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>Wie erkenne ich, dass die Mitglieder meines Back-End-Pools fehlerfrei sind?

Sie können das PowerShell-Cmdlet `Get-AzApplicationGatewayBackendHealth` verwenden oder die Integrität über das Portal überprüfen; Informationen dazu finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Welche Aufbewahrungsrichtlinie gilt für die Diagnoseprotokolle?

Diagnoseprotokolle gelangen in die Speicherkonten von Kunden, und Kunden können die Aufbewahrungsrichtlinie je nach Präferenz festlegen. Diagnoseprotokolle können auch an einen Event Hub oder Azure Monitor-Protokolle gesendet werden. Weitere Details finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>Wie erhalte ich die Überwachungsprotokolle für Application Gateway?

Für Application Gateway sind Überwachungsprotokolle verfügbar. Klicken Sie im Portal auf dem Menüblatt einer Application Gateway-Instanz auf **Aktivitätsprotokoll**, um das Überwachungsprotokoll aufzurufen. 

### <a name="can-i-set-alerts-with-application-gateway"></a>Kann ich mit Application Gateway Warnungen einrichten?

Ja, Application Gateway unterstützt Warnungen. Warnungen werden für Metriken konfiguriert. Weitere Informationen zu Metriken in Application Gateway finden Sie unter [Application Gateway-Metriken](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics). Weitere Informationen zu Warnungen finden Sie unter [Empfangen von Warnungsbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>Wie lassen sich Datenverkehrsstatistiken für Application Gateway analysieren?

Sie können Zugriffsprotokolle über eine Reihe von Mechanismen wie Azure Monitor-Protokolle, Excel, Power BI usw. anzeigen und analysieren.

Wir haben auch eine Resource Manager-Vorlage veröffentlicht, die die beliebte [GoAccess](https://goaccess.io/)-Protokollanalyse für Application Gateway Access Logs installiert und ausführt. GoAccess stellt wertvolle HTTP-Datenverkehrsstatistiken wie eindeutige Besucher, angeforderte Dateien, Hosts, Betriebssysteme, Browser, HTTP-Statuscodes und vieles mehr zur Verfügung. Weitere Informationen finden Sie in der [Infodatei im Resource Manager-Vorlagenordner auf GitHub](https://aka.ms/appgwgoaccessreadme).

### <a name="backend-health-returns-unknown-status-what-could-be-causing-this-status"></a>Die Back-End-Integrität gibt den Status „Unbekannt“ zurück. Was kann die Ursache sein?

Der häufigste Grund ist, dass der Zugriff auf das Back-End durch eine NSG oder ein benutzerdefiniertes DNS blockiert ist, oder dass Sie eine benutzerdefinierte Route (User Defined Route, UDR) für das Anwendungsgateway-Subnetz haben. Weitere Informationen finden Sie unter [Back-End-Integrität, Diagnoseprotokollierung und Metriken für Application Gateway](application-gateway-diagnostics.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Application Gateway finden Sie unter [Was ist Azure Application Gateway?](overview.md)

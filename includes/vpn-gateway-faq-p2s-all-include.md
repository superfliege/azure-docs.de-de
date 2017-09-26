### <a name="supportedclientos"></a>Welche Clientbetriebssysteme kann ich bei Point-to-Site-Verbindungen verwenden?

Folgende Clientbetriebssysteme werden unterstützt:

* Windows 7 (32 Bit und 64 Bit)
* Windows Server 2008 R2 (nur 64 Bit)
* Windows 8 (32 Bit und 64 Bit)
* Windows 8.1 (32 Bit und 64 Bit)
* Windows Server 2012 (nur 64 Bit)
* Windows Server 2012 R2 (nur 64 Bit)
* Windows 10
* OSX-Version 10.11 für Mac (El Capitan)
* macOS-Version 10.12 für Mac (Sierra)

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Wie viele VPN-Clientendpunkte kann meine Punkt-zu-Standort-Konfiguration umfassen?

Wir unterstützen bis zu 128 gleichzeitige VPN-Clientverbindungen mit einem virtuellen Netzwerk.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Können Proxys und Firewalls mit der Punkt-zu-Standort-Funktion durchlaufen werden?

Azure unterstützt zwei Arten von P2S-VPN-Optionen:

* Secure Sockets Tunneling Protocol (SSTP)

  SSTP ist eine Microsoft-eigene SSL-basierte Lösung, die Firewalls durchdringen kann, da die meisten Firewalls den von SSL verwendeten TCP-Port 443 öffnen.

* IKEv2-VPN

  IKEv2-VPN ist eine standardbasierte IPsec-VPN-Lösung, die UDP-Port 500 und 4500 und IP-Protokollnummer 50 verwendet. Firewalls öffnen nicht immer diese Ports, daher kann IKEv2-VPN unter Umständen Proxys und Firewalls nicht überwinden.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Wird die VPN-Verbindung eines für „Punkt zu Standort“ konfigurierten Clientcomputers nach einem Neustart automatisch wiederhergestellt?

Standardmäßig wird die VPN-Verbindung des Clientcomputers nicht automatisch wiederhergestellt.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Werden automatische Verbindungswiederherstellung und DDNS bei Punkt-zu-Standort-Verbindungen auf den VPN-Clients unterstützt?

Automatische Verbindungswiederherstellung und DDNS werden in Punkt-zu-Standort-VPNs derzeit nicht unterstützt.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kann ich im gleichen virtuellen Netzwerk sowohl Standort-zu-Standort- als auch Punkt-zu-Standort-Konfigurationen verwenden?

Ja. Beim Resource Manager-Bereitstellungsmodell müssen Sie als VPN-Typ für Ihr Gateway „RouteBased“ festlegen. Für das klassische Bereitstellungsmodell benötigen Sie ein dynamisches Gateway. Point-to-Site-Konfigurationen werden für VPN-Gateways mit statischem Routing oder für richtlinienbasierte Gateways nicht unterstützt.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kann ich einen Punkt-zu-Standort-Client so konfigurieren, dass er gleichzeitig eine Verbindung mit mehreren virtuellen Netzwerken herstellt?

Nein. Ein Point-to-Site-Client kann nur eine Verbindung mit Ressourcen im VNET herstellen, in dem sich das Gateway für virtuelle Netzwerke befindet.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Wie viel Durchsatz kann ich bei einer Standort-zu-Standort- oder bei einer Punkt-zu-Standort-Verbindung erwarten?

Der genaue Durchsatz der VPN-Tunnel lässt sich nur schwer ermitteln. IPsec und SSTP sind VPN-Protokolle mit hohem Kryptografieaufwand. Außerdem wird der Durchsatz durch die Latenz und die Bandbreite zwischen Ihrem Standort und dem Internet eingeschränkt. Bei einem VPN-Gateway nur mit IKEv2-P2S-VPN-Verbindungen hängt der erwartete Gesamtdurchsatz von der Gateway-SKU ab. Weitere Informationen zum Durchsatz finden Sie unter [Gateway-SKUs](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>Kann ich für Point-to-Site-Verbindungen einen beliebigen VPN-Softwareclient mit SSTP- und/oder IKEv2-Unterstützung verwenden?

Nein. Sie können nur den nativen VPN-Client unter Windows für SSTP und den nativen VPN-Client unter Mac für IKEv2 verwenden. Informationen finden Sie in der Liste der unterstützten Clientbetriebssysteme.
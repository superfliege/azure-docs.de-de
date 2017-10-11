### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Verwenden Client-Betriebssystemen kann ich mit Punkt-zu-Standort?

Die folgenden Clientbetriebssysteme werden unterstützt:

* Windows 7 (32-Bit und 64-Bit)
* Windows Server 2008 R2 (nur 64-Bit)
* Windows 8 (32-Bit und 64-Bit)
* Windows 8.1 (32-Bit und 64-Bit)
* WindowsServer 2012 (nur 64-Bit)
* Windows Server 2012 R2 (nur 64-Bit)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kann ich ein VPN-softwareclient für Punkt-zu-Standort verwenden, der SSTP unterstützt?

Nein. Unterstützung ist auf die oben aufgeführten Versionen der Windows-Betriebssystem beschränkt.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Wie viele VPN-Clientendpunkte kann ich in meinem Punkt-zu-Standort-Konfiguration verwenden?

Wir unterstützen bis zu 128 VPN-Clients für die Verwendung mit einem virtuellen Netzwerk zur gleichen Zeit eine Verbindung herstellen können.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kann ich eigene interne PKI-Stammzertifizierungsstelle für Punkt-zu-Standort-Konnektivität verwenden?

Ja. Zuvor konnte nur selbstsignierte Stammzertifikate verwendet werden. Sie können immer noch 20 Zertifikate hochladen.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Können Proxys und Firewalls unter Verwendung der Punkt-zu-Standort-Funktion werden durchlaufen?

Ja. Wir verwenden SSTP (Secure Socket Tunneling-Protokoll), um eine tunnelverbindung durch Firewalls. Dieser Tunnel wird als eine HTTPs-Verbindung angezeigt.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Wenn ich einen für Punkt-zu-Standort konfigurierten Clientcomputer neu starten, wird das VPN automatisch erneut eine Verbindung herstellen?

Standardmäßig wird der Clientcomputer die VPN-Verbindung nicht automatisch wiederherzustellen.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Ist Punkt-zu-Standort-automatische verbindungswiederherstellung und DDNS auf den VPN-Clients?

Die automatische verbindungswiederherstellung und DDNS werden in Punkt-zu-Standort-VPNs derzeit nicht unterstützt.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kann ich habe Standort-zu-Standort- und werden Punkt-zu-Standort-Konfigurationen für das virtuelle Netzwerk betrieben?

Ja. Beide Lösungen funktioniert, wenn Sie für Ihr Gateway einen VPN RouteBased Typ haben. Für dem klassischen Bereitstellungsmodell benötigen Sie ein Gateway mit dynamischem. Wir machen keine Unterstützung Punkt-zu-Standort für statisches routing VPN-Gateways oder -Gateways mit der `-VpnType PolicyBased` Cmdlet.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kann ich einen Punkt-zu-Standort-Client zur gleichen Zeit eine Verbindung mit mehreren virtuellen Netzwerken konfigurieren?

Ja, ist es möglich. Aber die virtuellen Netzwerke sind keine überlappenden IP-Präfixen und die Punkt-zu-Standort-Adressräume dürfen sich nicht zwischen den virtuellen Netzwerken überschneiden.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Wie viel Durchsatz kann ich über Standort-zu-Standort oder Punkt-zu-Standort-Verbindungen erwarten?

Es ist schwierig, die exakten Durchsatz für die VPN-Tunnel zu verwalten. IPsec und SSTP sind stark Crypto-schwere VPN-Protokolle. Durchsatz wird auch durch die Latenz und Bandbreite zwischen lokalem Standort und dem Internet beschränkt.
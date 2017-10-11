## <a name="load-balancer-differences"></a>Load Balancer-Unterschiede

Sie haben verschiedene Optionen zum Verteilen von Netzwerkdatenverkehr, die mit Microsoft Azure. Diese Optionen funktioniert anders voneinander, müssen eine andere Funktion festlegen sowie verschiedene Szenarien zu unterstützen. Sie können jede in Isolation verwendet werden, oder sie kombiniert werden.

* **Azure-Lastenausgleich** auf der Transportschicht (Ebene 4 in der OSI-Netzwerkstapel Verweis) funktioniert. Darüber werden auf Netzwerkebene Verteilung des Datenverkehrs über Instanzen einer Anwendung in der gleichen Azure-Rechenzentrum ausgeführt wird.
* **Application Gateway** auf Anwendungsebene (Layer 7 in der OSI-Netzwerkstapel Verweis) funktioniert. Sie fungiert als Reverseproxy-Dienst beendet die Clientverbindung und Anforderungen an die Back-End-Endpunkte weitergeleitet.
* **Traffic Manager** funktioniert auf dem DNS-Ebene.  DNS-Antworten verwendet für den Endbenutzer Netzwerkverkehr weltweit verteilten Endpunkte. Clients verbinden klicken Sie dann direkt mit diesen Endpunkten.

Die folgende Tabelle fasst die Features, die von jedem Dienst angeboten:

| Dienst | Azure-Lastenausgleich | Application Gateway | Traffic Manager |
| --- | --- | --- | --- |
| Technologie |Transportebene (Ebene 4) |Ebene (Layer 7) |DNS-Ebene |
| Anwendungsprotokolle unterstützt |Any |HTTP, HTTPS und WebSockets |Alle (ein HTTP-Endpunkt für die Webendpunkt-Überwachung erforderlich ist) |
| Endpunkte |Azure-VMs und Cloud-Dienste Rolleninstanzen |Alle Azure interne IP-Adresse, öffentlichen Internet IP-Adresse, Azure-VM oder Azure-Cloud-Dienst |Azure-VMs, Cloud Services, Azure-Web-Apps und externe Endpunkte |
| Vnet-Unterstützung |Kann für beide Anwendungen Internet zugänglichen und internen (Vnet) verwendet werden |Kann für beide Anwendungen Internet zugänglichen und internen (Vnet) verwendet werden |Unterstützt nur Internet verbundene Anwendungen |
| Webendpunkt-Überwachung |Unterstützt über Prüfpunkte |Unterstützt über Prüfpunkte |Unterstützt über HTTP/HTTPS-GET |

Azure-Lastenausgleich und Application Gateway Route Netzwerk-Datenverkehr an Endpunkte verfügen jedoch über verschiedene Verwendungsszenarien, das der Verkehr zu verarbeiten. In der folgenden Tabelle wird der Unterschied zwischen zwei Load balancer:

| Typ | Azure-Lastenausgleich | Application Gateway |
| --- | --- | --- |
| Protokolle |UDP/TCP |HTTP, HTTPS und WebSockets |
| IP-Reservierung |Unterstützt |Nicht unterstützt |
| Laden Sie im lastenausgleichsmodus |5-tuple(source IP, source port, destination IP, destination port, protocol type) |Roundrobin<br>Routing basierend auf URL |
| Lastenausgleichsmodus (Quell-IP / persistente Sitzungen) |2-Tupel (Quell-IP und Ziel-IP), 3-Tupel (Quell-IP, Ziel-IP und Ports). Können zentral hoch- bzw. herunterzuskalieren basierend auf der Anzahl von virtuellen Computern |Cookie-basierte Affinität<br>Routing basierend auf URL |
| Zustandsüberprüfungen |Standard: Testintervall - 15 Sekunden. Erstellt aus der Rotation: 2 fortlaufende Fehler. Unterstützt benutzerdefinierte Prüfpunkte |Im Leerlauf Testintervall 30 Sekunden. Die nach 5 aufeinander folgenden livedatenverkehr Fehler oder einen einzelnen Test Fehler im Leerlaufmodus erstellt. Unterstützt benutzerdefinierte Prüfpunkte |
| SSL-Abladung |Nicht unterstützt |Unterstützt |
| URL-basierte routing | Nicht unterstützt | Unterstützt|
| SSL-Richtlinie | Nicht unterstützt | Unterstützt|

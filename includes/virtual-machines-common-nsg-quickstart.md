Sie einen Port öffnen oder erstellen einen Endpunkt, an einen virtuellen Computer (VM) in Azure durch Erstellen eines Netzwerk-Filters für ein Subnetz oder eine Netzwerkschnittstelle. Diese Filter, mit die ein- und ausgehenden Datenverkehrs zu steuern, kann in einer Netzwerksicherheitsgruppe zugeordnet werden, auf die Ressource, die den Datenverkehr empfängt platziert werden.

Wir verwenden ein allgemeines Beispiel für Web-Datenverkehr an Port 80. Nachdem Sie einen virtuellen Computer haben, die zur Verarbeitung konfiguriert ist port webanforderungen auf die standard-TCP 80 (Beachten Sie die entsprechenden Dienste starten, und öffnen alle Betriebssystem-Firewall-Regeln auf dem virtuellen Computer auch), Sie:

1. Erstellen einer Netzwerksicherheitsgruppe an.
2. Erstellen Sie eine eingehende Regel, die dem Datenverkehr mit:
   * Der Zielbereich für die Port "80"
   * der Quellportbereich von "*" (ermöglicht einem beliebigen Quellport)
   * Priority-Wert, der weniger 65,500 (zum Verweigern werden höhere Priorität als die standardmäßige Catchall-Regel für eingehenden Datenverkehr)
3. Die Netzwerkschnittstelle oder Subnetz Netzwerksicherheitsgruppe zugeordnet.

Sie können komplexe Netzwerkkonfigurationen zum Sichern Ihrer Umgebung mit Netzwerksicherheitsgruppen und Regeln erstellen. Unser Beispiel verwendet nur ein oder zwei Regeln, die HTTP-Datenverkehr oder Remoteverwaltung zulassen. Weitere Informationen finden Sie in der folgenden ["Weitere Informationen"](#more-information-on-network-security-groups) Abschnitt oder [was eine Netzwerksicherheitsgruppe ist?](../articles/virtual-network/virtual-networks-nsg.md)


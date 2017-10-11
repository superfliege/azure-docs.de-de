Wenn Sie Probleme beim Herstellen einer Verbindung zu einem virtuellen Computer über eine VPN-Verbindung auftreten, überprüfen Sie Folgendes:

- Stellen Sie sicher, dass Ihre VPN-Verbindung erfolgreich ist.
- Stellen Sie sicher, dass Sie mit der privaten IP-Adresse für den virtuellen Computer herstellen.
- Wenn Sie eine Verbindung mit dem virtuellen Computer mit der privaten IP-Adresse, jedoch nicht den Computernamen herstellen können, stellen Sie sicher, dass Sie DNS ordnungsgemäß konfiguriert haben. Weitere Informationen zur Funktionsweise der namensauflösung für virtuelle Computer finden Sie unter [namensauflösung für virtuelle Computer](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Wenn Sie über die Punkt-zu-Standort verbinden, überprüfen Sie die folgenden zusätzlichen Elemente:

- Verwenden Sie "Ipconfig", überprüfen Sie die IPv4-Adresse zugewiesen, die Ethernet-Adapter auf dem Computer, von dem Sie eine Verbindung herstellen. Die IP-Adresse innerhalb des Adressbereichs des Vnets, den Sie zum Verbinden oder in den Adressbereich des Ihrer VPNClientAddressPool ist, wird dies als eine überlappende Adressraum bezeichnet. Auf diese Weise der Adressraum überschneidet sich, der Netzwerkdatenverkehr Azure nicht erreichen kann, bleibt er im lokalen Netzwerk.
- Stellen Sie sicher, dass die VPN-clientkonfigurationspaket generiert wurde, nachdem die DNS-Server IP-Adressen für das VNet angegeben wurden. Wenn Sie die IP-Adressen von DNS-Server aktualisiert haben, generieren und ein neues Konfigurationspaket für VPN-Client installieren.

Weitere Informationen zur Behandlung von einer RDP-Verbindungs finden Sie unter [Remotedesktopverbindungen zu einer virtuellen Maschine beheben](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).
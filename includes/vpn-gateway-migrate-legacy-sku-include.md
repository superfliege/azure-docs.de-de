> [!NOTE]
> * Die öffentliche IP-Adresse des VPN-Gateway ändert sich bei der Migration von einer alten SKU zu einer neuen SKU.
> * Klassische VPN-Gateways und neue SKUs können nicht migriert werden. Klassische VPN-Gateways können nur SKUs legacy (alte) verwenden.
> 

Ihre Azure-VPN-Gateways zwischen den alten SKUs und die neue SKU-Familien kann nicht geändert werden. Wenn Sie VPN Gateways im Resource Manager-Bereitstellungsmodell haben, die die ältere Version der SKUs verwenden, können Sie zu den neuen SKUs migrieren. Für die Migration löschen Sie das vorhandene VPN Gateway für Ihr virtuelles Netzwerk und erstellen dann ein neues.

Migrations-Workflow:

1. Entfernen Sie alle Verbindungen mit dem Gateway des virtuellen Netzwerks.
2. Löschen Sie das alte VPN Gateway.
3. Erstellen Sie das neue VPN Gateway.
4. Aktualisieren Sie Ihre lokalen VPN-Geräte mit der IP-Adresse des neuen VPN Gateways (für Standort-zu-Standort-Verbindungen).
5. Aktualisieren Sie den Gateway-IP-Adresswert für alle lokalen VNet-zu-VNet-Netzwerkgateways, die eine Verbindung mit diesem Gateway herstellen.
6. Laden Sie neue Client-VPN-Konfigurationspakete für P2S-Clients herunter, die über dieses VPN Gateway eine Verbindung mit dem virtuellen Netzwerk herstellen.
7. Stellen Sie die Verbindungen mit dem Gateway des virtuellen Netzwerks erneut her.
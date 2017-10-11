## <a name="public-ip-address"></a>Öffentliche IP-Adresse
Eine öffentliche IP-Adressressource enthält entweder einen reserviert oder dynamische Internetzugriff-IP-Adresse. Auch wenn Sie eine öffentliche IP-Adresse als eigenständige Objekt erstellen können, müssen Sie ihn in ein anderes Objekt, verwenden die Adresse eigentlich zuordnen. Sie können eine öffentliche IP-Adresse ein Lastenausgleich, Anwendungsgateway oder eine Netzwerkkarte für den Zugriff auf das Internet auf diese Ressourcen bieten zuordnen.  

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **Zuordnungsmethode für Öffentliche IPS** |Definiert die IP-Adresse *statische* oder *dynamische*. |statische, dynamische |
| **idleTimeoutInMinutes** |Definiert den Timeout im Leerlauf hat den Standardwert von 4 Minuten. Wenn keine weiteren Pakete für eine bestimmte Sitzung innerhalb dieses Zeitraums empfangen wird, wird die Sitzung beendet. |einen beliebigen Wert zwischen 4 und 30 ein. |
| **IP-Adresse** |IP-Adresse zu einem Objekt zugewiesen ist. Dies ist eine schreibgeschützte Eigenschaft. |104.42.233.77 |

### <a name="dns-settings"></a>DNS-Einstellungen
Öffentliche IP-Adressen haben kein untergeordnetes Objekt mit dem Namen **DnsSettings** , enthält die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **"domainnamelabel"** |Host mit dem Namen für die namensauflösung verwendet. |Www, ftp, vm1 |
| **FQDN** |Vollqualifizierte Namen für die öffentliche IP-Adresse. |www.westus.cloudapp.Azure.com |
| **reverseFqdn** |Vollständig qualifizierten Domänennamen, die in die IP-Adresse aufgelöst wird und im DNS als einen PTR-Eintrag registriert ist. |www.contoso.com. |

Beispiel öffentliche IP-Adresse im JSON-Format:

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a>Zusätzliche Ressourcen
* Weitere Informationen zu erhalten [öffentliche IP-Adressen](../articles/virtual-network/virtual-networks-reserved-public-ip.md).
* Erfahren Sie mehr über [-Instanz Ebene öffentliche IP-Adressen](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).
* Lesen der [REST-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt163638.aspx) für die öffentliche IP-Adressen.


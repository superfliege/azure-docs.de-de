## <a name="route-tables"></a>Routentabellen
Route-tabellenressourcen enthält Routen, die zum definieren, wie Datenfluss in Ihrer Azure-Infrastruktur verwendet. Sie können benutzerdefinierte Routen (UDR) verwenden, den um gesamten Datenverkehr auf einem virtuellen Gerät aus einem bestimmten Subnetz zu senden, wie z. B. einer Firewall oder Intrusion Detection (IDS). Sie können eine Routingtabelle mit Subnetzen zuordnen. 

Routentabellen enthalten die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **Routen** |Auflistung benutzerdefinierter Routen in der Routingtabelle |finden Sie unter [benutzerdefinierte Routen](#User-defined-routes) |
| **Subnetze** |Eine Auflistung von Subnetzen wird auf die Routentabelle angewendet. |finden Sie unter [Subnetze](#Subnets) |

### <a name="user-defined-routes"></a>Benutzerdefinierte Routen
Sie können erstellen UDRs, um anzugeben, in denen Datenverkehr, gesendet werden soll basierend auf dessen Zieladresse. Sie können eine Route als die Gateway-Standarddefinition basierend auf der Zieladresse eines Netzwerkpakets vorstellen.

UDRs enthalten die folgenden Eigenschaften. 

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **addressPrefix** |-Adressenpräfix oder vollständige IP-Adresse für das Ziel |192.168.1.0/24, 192.168.1.101 |
| **nextHopType** |Typ des Geräts der Datenverkehr wird an gesendet werden |"Virtualappliance", VPN-Gateway, Internet |
| **nextHopIpAddress** |IP-Adresse für den nächsten hop |192.168.1.4 |

Route-Beispieltabelle im JSON-Format:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>Zusätzliche Ressourcen
* Weitere Informationen zu erhalten [UDRs](../articles/virtual-network/virtual-networks-udr-overview.md).
* Lesen der [REST-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) für Routentabellen.
* Lesen der [REST-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) für Benutzer definierten Routen (UDRs).


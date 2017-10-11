## <a name="network-security-group"></a>Netzwerksicherheitsgruppe
Eine NSG-Ressource ermöglicht die Erstellung von Sicherheitsgrenze für Arbeitslasten, durch die Implementierung zulassen und verweigern Regeln. Diese Regeln können auf einer virtuellen Maschine, einer NIC oder einem Subnetz angewendet werden.

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **Subnetze** |Liste der Subnetz-Ids, denen die NSG angewendet wird. |/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/Providers/Microsoft.Network/virtualNetworks/TestVNet/Subnets/Frontend |
| **securityRules** |Liste der Sicherheitsregeln für die, die der NSG bilden |Finden Sie unter [Sicherheitsregel](#Security-rule) unten |
| **defaultSecurityRules** |Liste der Standard-Sicherheitsregeln in jede NSG vorhanden |Finden Sie unter [Standard Sicherheitsregeln](#Default-security-rules) unten |

* **Sicherheitsregel** -eine NSG können mehrere Sicherheitsregeln definiert haben. Jede Regel kann erlauben oder Verweigern von verschiedenen Arten von Datenverkehr.

### <a name="security-rule"></a>Sicherheitsregel
Eine Sicherheitsregel ist eine untergeordnete Ressource einer NSG mit den folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **Beschreibung** |Beschreibung für die Regel |Für alle virtuellen Computer im Subnetz X eingehenden Datenverkehr zulassen |
| **Protokoll** |Protokoll für die Regel entsprechend |TCP, UDP oder * |
| **sourcePortRange** |Quellportbereich entsprechend für die Regel |80, 100-200, * |
| **destinationPortRange** |Zielportbereich entsprechend für die Regel |80, 100-200, * |
| **sourceAddressPrefix** |Source-Adresspräfix entsprechend für die Regel |10.10.10.1 10.10.10.0/24 VirtualNetwork |
| **destinationAddressPrefix** |Ziel-Adresspräfix entsprechend für die Regel |10.10.10.1 10.10.10.0/24 VirtualNetwork |
| **Richtung** |Richtung des Datenverkehrs für die Regel entsprechend |eingehende oder ausgehende |
| **Priorität** |Die Priorität für die Regel. Regeln werden überprüft, in der Reihenfolge ihrer Priorität, sobald eine Regel gilt, keine weiteren Regeln für den Abgleich getestet werden. |10, 100, 65000 |
| **Zugriff** |Art des Zugriffs anwenden, wenn die Regel eine Übereinstimmung mit |zulassen oder verweigern |

Beispiel NSG im JSON-Format:

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a>Sicherheit-Standardregeln

Standardregeln für die Sicherheit haben die gleichen Eigenschaften, die in Sicherheitsregeln verfügbar. Sie sind vorhanden, um grundlegende Konnektivität zwischen Ressourcen bereitzustellen, die NSGs auf sie angewendet haben. Stellen Sie sicher, dass Sie wissen, welche [Standard Sicherheitsregeln](../articles/virtual-network/virtual-networks-nsg.md#default-rules) vorhanden sind.

### <a name="additional-resources"></a>Zusätzliche Ressourcen
* Weitere Informationen zu erhalten [NSGs](../articles/virtual-network/virtual-networks-nsg.md).
* Lesen der [REST-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt163615.aspx) für Netzwerksicherheitsgruppen.
* Lesen der [REST-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt163580.aspx) für Sicherheitsregeln.

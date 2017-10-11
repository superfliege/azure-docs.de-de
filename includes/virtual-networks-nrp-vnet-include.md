## <a name="virtual-network"></a>Virtuelles Netzwerk
Virtuelle Netzwerke (VNETS) und die Subnetze Ressourcen helfen definieren eine Sicherheitsgrenze für arbeitsauslastungen, die in Azure ausgeführt wird. Eine VNet wird durch eine Auflistung von Adressräume, definiert als CIDR-Blöcke gekennzeichnet. 

> [!NOTE]
> Netzwerkadministratoren sind mit CIDR-Notation vertraut. Wenn Sie nicht mit CIDR-WERTS vertraut sind [erfahren Sie mehr über diese](http://whatismyipaddress.com/cidr).
> 
> 

![VNet mit mehreren Subnetzen](./media/resource-groups-networking/Figure4.png)

VNets werden die folgenden Eigenschaften enthalten.

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **addressSpace** |Auflistung von-Adresspräfixen, die denen des virtuellen Netzwerks in CIDR-Notation besteht |192.168.0.0/16 |
| **Subnetze** |Eine Auflistung von Subnetzen, die das VNet bilden |finden Sie unter [Subnetze](#Subnets) unten. |
| **IP-Adresse** |IP-Adresse zu einem Objekt zugewiesen ist. Dies ist eine schreibgeschützte Eigenschaft. |104.42.233.77 |

### <a name="subnets"></a>Subnetze
Ein Subnetz ist eine untergeordnete Ressource von einem VNet, und hilft definieren Segmente in einem CIDR-Block, mit der IP-Adresspräfixen Adressräume. NICs können Subnetze hinzugefügt, und auf virtuellen Computern, die Konnektivität bereitgestellt wird, für verschiedene Arbeitslasten verbunden werden.

Subnetze enthalten die folgenden Eigenschaften. 

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **addressPrefix** |Einzelne Adresspräfix, aus denen das Subnetz im CIDR-Notation besteht |192.168.1.0/24 |
| **networkSecurityGroup** |NSG dem Subnetz angewendet |finden Sie unter [Netzwerksicherheitsgruppen](#Network-Security-Group) |
| **routeTable** |Routentabelle angewendet, die dem Subnetz |finden Sie unter [UDR](#Route-table) |
| **IP-Konfigurationen** |Sammlung von IP-bei-Objekten verwendet, die für das Subnetz mit verbundenen NICs. |finden Sie unter [UDR](#Route-table) |

Beispiel-VNet im JSON-Format:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Zusätzliche Ressourcen
* Weitere Informationen zu erhalten [VNet](../articles/virtual-network/virtual-networks-overview.md).
* Lesen der [REST-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt163650.aspx) für VNets.
* Lesen der [REST-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt163618.aspx) für Subnetze.


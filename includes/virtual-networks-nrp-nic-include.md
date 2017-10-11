## <a name="nic"></a>NETZWERKKARTE
Netzwerkschnittstellenkarte (NIC) eine Netzwerkressource-Schnittstelle stellt über eine Netzwerkverbindung mit einem vorhandenen Subnetz in einem VNet-Ressource. Obwohl Sie eine NIC als eigenständige Objekt erstellen können, müssen Sie ihn in ein anderes Objekt für die tatsächlich Verbindung zuordnen. Eine Netzwerkkarte kann Verbindung ein virtuellen Computers mit einem Subnetz, eine öffentliche IP-Adresse oder einen Lastenausgleich verwendet werden.  

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **VirtualMachine.** |VM die NIC zugeordnet ist. |/Subscriptions/{GUID}/../Microsoft.Compute/virtualMachines/VM1 |
| **macAddress** |MAC-Adresse für die NIC an |einen beliebigen Wert zwischen 4 und 30 ein. |
| **networkSecurityGroup** |NSG, die NIC zugeordnet sind |/Subscriptions/{GUID}/../Microsoft.Network/networkSecurityGroups/myNSG1 |
| **dnsSettings** |DNS-Einstellungen für die NIC an |finden Sie unter [PIP](#Public-IP-address) |

Eine Netzwerkschnittstellenkarte oder NIC, stellt eine Netzwerkschnittstelle aus, die an einen virtuellen Computer (VM) zugeordnet werden kann. Einen virtuellen Computer kann eine oder mehrere NICs haben.

![NIC auf einem einzelnen virtuellen Computer](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a>IP-Konfigurationen
NICs haben kein untergeordnetes Objekt mit dem Namen **IP-Konfigurationen** , enthält die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Beispielwerte |
| --- | --- | --- |
| **Subnetz** |Der NIC-Subnetz ist verbunden. |/Subscriptions/{GUID}/../Microsoft.Network/virtualNetworks/myvnet1/Subnets/mysub1 |
| **Netzwerkschnittstelle** |IP-Adresse für die Netzwerkkarte in dem Subnetz |10.0.0.8 |
| **privateIPAllocationMethod** |IP-Zuordnungsmethode |Dynamische oder statische |
| **enableIPForwarding** |Gibt an, ob die Netzwerkkarte für das routing verwendet werden kann |"True" oder "false" |
| **primäre** |Gibt an, ob die Netzwerkkarte für die primäre NIC für den virtuellen Computer ist |"True" oder "false" |
| **publicIPAddress** |Der NIC zugeordnet PIP |finden Sie unter [DNS-Einstellungen](#DNS-settings) |
| **"loadbalancerbackendaddresspools"** |Sichern von End-Adresspools, denen die NIC zugeordnet sind | |
| **loadBalancerInboundNatRules** |Eingehende Load Balancer NAT-Regeln, denen die NIC zugeordnet ist | |

Beispiel öffentliche IP-Adresse im JSON-Format:

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a>Zusätzliche Ressourcen
* Lesen der [REST-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt163579.aspx) für Netzwerkkarten.


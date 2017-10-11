## <a name="traffic-manager-profile"></a>Traffic Manager-Profil
Traffic Manager und dessen untergeordneten endpunktressource aktivieren Sie die DNS-Weiterleitung an Endpunkte in Azure und außerhalb von Azure. Diese Verteilung des Netzwerkdatenverkehrs unterliegt Verteilermethoden für die Richtlinie. Traffic Manager ermöglicht auch das endpunktzuständen überwacht werden soll, und entsprechend umgeleitet Datenverkehr auf Grundlage der Integritäts eines Endpunkts. 

| Eigenschaft | Beschreibung |
| --- | --- |
| **trafficRoutingMethod** |Mögliche Werte sind *Leistung*, *Weighted*, und *Priorität* |
| **dnsConfig** |FQDN für das Profil |
| **Protokoll** |Überwachung von Protokoll, mögliche Werte sind *HTTP* und *HTTPS* |
| **Port** |Überwachung von port |
| **Pfad** |Überwachungspfad |
| **Endpunkte** |Container für Ressourcen |

### <a name="endpoint"></a>Endpunkt
Ein Endpunkt ist eine untergeordnete Ressource eines Traffic Manager-Profils. Webendpunkt, mit denen die Benutzer des Datenverkehrs Verteilung, auf der Grundlage von der konfigurierten Richtlinie in der Traffic Manager-Profilressource, oder er stellt einen Dienst dar. 

| Eigenschaft | Beschreibung |
| --- | --- |
| **Typ** |Der Typ des Endpunkts, um mögliche Werte sind *Azure-Endpunkt*, *externe Endpunkt*, und *Endpunkt geschachtelt* |
| **"targetresourceid"** |öffentliche IP-Adresse eines Endpunkts Dienst oder einer Website. Dabei kann es sich um einen Azure oder externen Endpunkt handeln. |
| **Gewichtung** |Endpunkt-Gewichtung in der datenverkehrsverwaltung verwendet. |
| **Priorität** |Priorität des Endpunkts, verwendet, um eine failoveraktion definieren |

Beispiel von Traffic Manager im Json-Format: 

        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }


## <a name="additional-resources"></a>Zusätzliche Ressourcen
Lesen [REST-API-Dokumentation für Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) für Weitere Informationen.


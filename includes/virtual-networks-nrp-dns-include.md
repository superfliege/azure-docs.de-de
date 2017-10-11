## <a name="azure-dns"></a>Azure-DNS
Azure-DNS-ist einem Hostingdienst für Domänen mit DNS-namensauflösung mithilfe des Microsoft Azure-Infrastruktur bereitstellen.

| Eigenschaft | Beschreibung | Beispielwert |
| --- | --- | --- |
| **DNSzones** |Domäne Zeitzoneninformationen zu DNS-Hosteinträge einer bestimmten Domäne |/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com " |

### <a name="dns-record-sets"></a>DNS-Datensatzgruppen
DNS-Zonen haben kein untergeordnetes Objekt mit dem Namen Ressourceneintragssatz. Datensatzgruppen sind eine Auflistung von Hosteinträgen nach Typ für eine DNS-Zone. Datensatztypen sind A, AAAA, CNAME, MX, NS, SOA, SRV und TXT.

| Eigenschaft | Beschreibung | Beispielwert |
| --- | --- | --- |
| A |IPv4-Datensatztyp |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/A/www |
| AAAA |IPv6-Datensatztyp |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord |
| CNAME |Datensatztyp kanonischen Namen <sup>1</sup> |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/CNAME/www |
| MX |e-Mail-Datensatztyp |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/MX/Mail |
| NOTIFICATION SERVICES |Server-Datensatztyp Name |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/NS/ |
| SOA |Beginn der Autorität für die Datensatztyp <sup>2</sup> |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SOA |
| SRV |Dienst-Datensatztyp |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SRV |

<sup>1</sup> lässt nur einen Wert pro Datensatz.

<sup>2</sup> lässt nur einen Datensatztyp SOA pro DNS-Zone. 

Beispiel-DNS-Zone im Json-Format:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "The name of the DNS zone to be created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "The name of the DNS record to be created.  The name is relative to the zone, not the FQDN."
          }
        }
      },
      "resources": 
      [
        {
          "type": "microsoft.network/dnszones",
          "name": "[parameters('newZoneName')]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": {
          }
        },
        {
          "type": "microsoft.network/dnszones/a",
          "name": "[concat(parameters('newZoneName'), concat('/', parameters('newRecordName')))]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": 
          {
            "TTL": 3600,
            "ARecords": 
            [
                {
                    "ipv4Address": "1.2.3.4"
                },
                {
                    "ipv4Address": "1.2.3.5"
                }
            ]
          },
          "dependsOn": [
            "[concat('Microsoft.Network/dnszones/', parameters('newZoneName'))]"
          ]
        }
          ]
    }

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Lesen der [REST-API-Dokumentation für DNS-Zonen ](https://msdn.microsoft.com/library/azure/mt130626.aspx) für Weitere Informationen.

Lesen der [REST-API-Dokumentation für DNS-Datensatzgruppen](https://msdn.microsoft.com/library/azure/mt130627.aspx) für Weitere Informationen.


---
title: VNET-Dienstendpunkte und -Regeln für Azure Event Hubs | Microsoft-Dokumentation
description: Fügen Sie einem virtuellen Netzwerk einen Microsoft.EventHub-Dienstendpunkt hinzu.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: b8c3270149c254898ad3180b92a4ff398f3efb6c
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745908"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Verwenden von VNET-Dienstendpunkten mit Azure Event Hubs

Die Integration von Event Hubs mit [VNET-Dienstendpunkten][vnet-sep] ermöglicht den sicheren Zugriff auf Messagingfunktionen für Workloads, z.B. an virtuelle Netzwerke (VNETs) gebundene virtuelle Computer, wobei der Pfad für den Netzwerkdatenverkehr an beiden Enden geschützt ist. 

Nachdem die Konfiguration der Bindung an mindestens einen Dienstendpunkt des VNET-Subnetzes durchgeführt wurde, akzeptiert der entsprechende Event Hubs-Namespace nur noch Datenverkehr von autorisierten virtuellen Netzwerken. Aus Sicht des virtuellen Netzwerks wird durch die Bindung eines Event Hubs-Namespace an einen Dienstendpunkt ein isolierter Netzwerktunnel vom Subnetz des virtuellen Netzwerks zum Messagingdienst konfiguriert.

Das Ergebnis ist eine private und isolierte Beziehung zwischen den Workloads, die an das Subnetz gebunden sind, und dem entsprechenden Event Hubs-Namespace, obwohl sich die beobachtbare Netzwerkadresse des Messaging-Dienstendpunkts in einem öffentlichen IP-Bereich befindet.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Erweiterte Sicherheitsszenarien basierend auf der VNET-Integration 

Lösungen, für die eine strikte und auf mehrere Bereiche aufgeteilte Sicherheit erforderlich ist und bei denen die VNET-Subnetze die Segmentierung zwischen den einzelnen aufgeteilten Diensten bereitstellen, benötigen im Allgemeinen weiterhin Kommunikationspfade zwischen den Diensten, die sich in diesen Bereichen befinden.

Für alle direkten IP-Routen zwischen den Bereichen (auch für HTTPS per TCP/IP) besteht die Gefahr, dass ab der Vermittlungsschicht Sicherheitsrisiken ausgenutzt werden. Bei Messagingdiensten werden vollständig isolierte Kommunikationspfade bereitgestellt, für die Nachrichten während des Übergangs zwischen Parteien sogar auf Datenträger geschrieben werden. Workloads in zwei einzelnen virtuellen Netzwerken, die beide an dieselbe Event Hubs-Instanz gebunden sind, können über Nachrichten effizient und zuverlässig kommunizieren, während die Integrität der jeweiligen Netzwerkisolationsgrenze aufrechterhalten wird.
 
Dies bedeutet, dass Ihre sicherheitsrelevanten Cloudlösungen nicht nur Zugriff auf zuverlässige und skalierbare Azure-Funktionen für asynchrones Messaging eines Branchenführers haben, sondern dass das Messaging jetzt auch genutzt werden kann, um Kommunikationspfade zwischen sicheren Lösungsbereichen zu erstellen. Diese sind deutlich sicherer als alle Optionen des Peer-to-Peer-Kommunikationsmodus, einschließlich HTTPS und andere per TLS geschützte Socketprotokolle.

## <a name="bind-event-hubs-to-virtual-networks"></a>Binden von Event Hubs an virtuelle Netzwerke

*VNET-Regeln* sind das Feature für die Firewallsicherheit, mit dem gesteuert wird, ob Ihr Azure Event Hubs-Server Verbindungen eines bestimmten VNET-Subnetzes akzeptiert.

Das Binden eines Event Hubs-Namespace an ein virtuelles Netzwerk ist ein Prozess mit zwei Schritten. Zuerst müssen Sie einen **Dienstendpunkt des virtuellen Netzwerks** in einem Subnetz eines virtuellen Netzwerks erstellen und für „Microsoft.EventHub“ aktivieren, wie in der [Übersicht über Dienstendpunkte][vnet-sep] beschrieben. Nachdem Sie den Dienstendpunkt hinzugefügt haben, binden Sie den Event Hubs-Namespace mit einer *Regel für virtuelle Netzwerke* daran.

Die Regel für virtuelle Netzwerke ist eine benannte Zuordnung des Event Hubs-Namespace zu einem Subnetz eines virtuellen Netzwerks. Während die Regel vorhanden ist, wird allen Workloads, die an das Subnetz gebunden sind, Zugriff auf den Event Hubs-Namespace gewährt. Event Hubs stellt selbst niemals ausgehende Verbindungen her, muss keinen Zugriff erhalten und erhält daher niemals die Gewährung des Zugriffs auf Ihr Subnetz, indem diese Regel aktiviert wird.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Erstellen einer Regel für virtuelle Netzwerke mit Azure Resource Manager-Vorlagen

Mithilfe der folgenden Resource Manager-Vorlage können Sie einem vorhandenen Event Hubs-Namespace eine VNET-Regel hinzufügen.

Vorlagenparameter:

* **namespaceName**: Event Hubs-Namespace
* **vnetRuleName**: Name für die zu erstellende VNET-Regel
* **virtualNetworkingSubnetId**: Vollqualifizierter Resource Manager-Pfad für das Subnetz des virtuellen Netzwerks, z.B. `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` für das Standardsubnetz eines virtuellen Netzwerks

```json
{  
   "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{     
          "namespaceName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the namespace"
             }
          },
          "vnetRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "virtualNetworkSubnetId":{  
             "type":"string",
             "metadata":{  
                "description":"subnet Azure Resource Manager ID"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('vnetRuleName'))]",
            "type":"Microsoft.EventHub/namespaces/VirtualNetworkRules",         
            "properties": {             
                "virtualNetworkSubnetId": "[parameters('virtualNetworkSubnetId')]"  
            }
        } 
    ]
}
```

Gehen Sie zum Bereitstellen der Vorlage gemäß der Anleitung für [Azure Resource Manager][lnk-deploy] vor.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu virtuellen Netzwerken finden Sie unter den folgenden Links:

- [Dienstendpunkte von virtuellen Netzwerken (Vorschauversion)][vnet-sep]
- [Azure Event Hubs IP filtering][ip-filtering] (Azure Event Hubs – IP-Filterung)

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md
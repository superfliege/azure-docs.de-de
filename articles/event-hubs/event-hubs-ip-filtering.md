---
title: Azure Event Hubs – IP-Verbindungsfilter | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der IP-Filterung Verbindungen von bestimmten IP-Adressen mit Azure Event Hubs blockieren.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: shvija
ms.openlocfilehash: 0ecce667584f522b5bd6aac28291bda427f37608
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005351"
---
# <a name="use-ip-filters"></a>Verwenden von IP-Filtern

Für Szenarien, in denen nur von bestimmten bekannten Websites aus auf Azure Event Hubs zugegriffen werden kann, können Sie mithilfe des Features *IP-Filter* Regeln zum Ablehnen oder Akzeptieren von Datenverkehr konfigurieren, der von bestimmten IPv4-Adressen stammt. Bei diesen Adressen kann es sich beispielsweise um die Adressen eines unternehmenseigenen NAT-Gateways handeln.

## <a name="when-to-use"></a>Einsatzgebiete

Das Blockieren bestimmter IP-Adressen für Event Hubs-Endpunkte ist in zwei wichtigen Anwendungsfällen hilfreich:

- Ihre Event Hubs sollen nur Datenverkehr aus einem bestimmten IP-Adressbereich empfangen und jeglichen anderen Datenverkehr ablehnen. Ein Beispiel wäre etwa, wenn Sie Event Hubs mit [Azure Express Route][express-route] verwenden, um private Verbindungen mit Ihrer lokalen Infrastruktur zu erstellen. 
- Sie müssen Datenverkehr von IP-Adressen ablehnen, die vom Event Hubs-Administrator als verdächtig eingestuft wurden.

## <a name="how-filter-rules-are-applied"></a>Anwenden von Filterregeln

Die IP-Filterregeln werden auf der Event Hubs-Namespaceebene angewendet. Daher gelten die Regeln für alle Clientverbindungen mit einem beliebigen unterstützten Protokoll.

Alle Verbindungsversuche von einer IP-Adresse, die der IP-Ablehnungsregel für den Event Hubs-Namespace entspricht, werden als nicht autorisiert abgelehnt. In der Antwort wird die IP-Regel nicht erwähnt.

## <a name="default-setting"></a>Standardeinstellung

Das Raster **IP-Filter** im Portal ist für Event Hubs standardmäßig leer. Diese Standardeinstellung bedeutet, dass Ihr Event Hub Verbindungen von allen IP-Adressen akzeptiert. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist.

## <a name="ip-filter-rule-evaluation"></a>Auswertung von IP-Filterregeln

IP-Filterregeln werden der Reihe nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

Wenn Sie beispielsweise Adressen im Bereich 70.37.104.0/24 zulassen und alle anderen Adressen ablehnen möchten, sollte die erste Regel im Raster lauten, dass der Adressbereich 70.37.104.0/24 zulässig ist. Mit der nächsten Regel sollten alle Adressen abgelehnt werden, indem der Bereich 0.0.0.0/0 verwendet wird.

> [!NOTE]
> Das Ablehnen von IP-Adressen kann andere Azure-Dienste (etwa Azure Stream Analytics, Azure Virtual Machines oder den Device Explorer im Portal) an der Interaktion mit Event Hubs hindern.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Erstellen einer VNet-Regel mit Azure Resource Manager-Vorlagen

Mithilfe der folgenden Resource Manager-Vorlage können Sie einem vorhandenen Event Hubs-Namespace eine VNet-Regel hinzufügen.

Vorlagenparameter:

- **ipFilterRuleName** muss eine eindeutige alphanumerische Zeichenfolge ohne Beachtung von Groß-/Kleinschreibung sein, die bis zu 128 Zeichen umfassen kann.
- **ipFilterAction** muss entweder auf **Reject** (Ablehnen) oder auf **Accept** (Akzeptieren) festgelegt werden, um die gewünschte Aktion für die IP-Filterregel anzugeben.
- **ipMask** ist eine einzelne IPv4-Adresse oder ein Block von IP-Adressen in CIDR-Notation. In CIDR-Notation steht beispielsweise 70.37.104.0/24 für die 256 IPv4-Adressen von 70.37.104.0 bis 70.37.104.255. „24“ gibt dabei die Anzahl signifikanter Präfixbits für den Bereich an.

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
          "ipFilterRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "ipFilterAction":{  
             "type":"string",
             "allowedValues": ["Reject", "Accept"],
             "metadata":{  
                "description":"IP Filter Action"
             }
          },
          "IpMask":{  
             "type":"string",
             "metadata":{  
                "description":"IP Mask"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('ipFilterRuleName'))]",
            "type": "Microsoft.EventHub/Namespaces/IPFilterRules",
            "properties": {
                "FilterName":"[parameters('ipFilterRuleName')]",
                "Action":"[parameters('ipFilterAction')]",              
                "IpMask": "[parameters('IpMask')]"
            }
        } 
    ]
}
```

Gehen Sie zum Bereitstellen der Vorlage gemäß der Anleitung für [Azure Resource Manager][lnk-deploy] vor.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Einschränken des Event Hubs-Zugriffs für virtuelle Azure-Netzwerke finden Sie unter dem folgenden Link:

- [Dienstendpunkte virtueller Netzwerke für Event Hubs][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md
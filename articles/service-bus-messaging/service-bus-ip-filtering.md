---
title: 'Azure Service Bus: IP-Verbindungsfilter | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mithilfe der IP-Filterung Verbindungen von bestimmten IP-Adressen mit Azure Service Bus blockieren.
services: service-bus
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: clemensv
ms.openlocfilehash: ccb759a9151d734aa99a6f6b9c68e6072874dd84
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394827"
---
# <a name="use-ip-filters"></a>Verwenden von IP-Filtern

Für Szenarien, in denen nur von bestimmten bekannten Websites aus auf Azure Service Bus zugegriffen werden kann, können Sie mithilfe des Features *IP-Filter* Regeln zum Ablehnen oder Akzeptieren von Datenverkehr konfigurieren, der von bestimmten IPv4-Adressen stammt. Bei diesen Adressen kann es sich beispielsweise um die Adressen eines unternehmenseigenen NAT-Gateways handeln.

## <a name="when-to-use"></a>Einsatzgebiete

Das Blockieren bestimmter IP-Adressen für Service Bus-Endpunkte ist in zwei spezifischen Anwendungsfällen hilfreich:

- Service Bus soll nur Datenverkehr aus einem bestimmten IP-Adressbereich empfangen und jeglichen anderen Datenverkehr ablehnen. Ein Beispiel wäre etwa, wenn Sie Service Bus mit [Azure Express Route][express-route] verwenden, um private Verbindungen mit Ihrer lokalen Infrastruktur zu erstellen.
- Sie müssen Datenverkehr von IP-Adressen ablehnen, die der Service Bus-Administrator als verdächtig eingestuft hat.

## <a name="how-filter-rules-are-applied"></a>Anwenden von Filterregeln

Die IP-Filterregeln werden auf der Service Bus-Namespaceebene angewendet. Daher gelten die Regeln für alle Clientverbindungen mit einem beliebigen unterstützten Protokoll.

Alle Verbindungsversuche von einer IP-Adresse, die der IP-Ablehnungsregel für den Service Bus-Namespace entspricht, werden als nicht autorisiert abgelehnt. In der Antwort wird die IP-Regel nicht erwähnt.

## <a name="default-setting"></a>Standardeinstellung

Das Raster **IP-Filter** im Portal ist für Service Bus standardmäßig leer. Diese Standardeinstellung bedeutet, dass Ihr Namespace Verbindungen von allen IP-Adressen akzeptiert. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist.

## <a name="ip-filter-rule-evaluation"></a>Auswertung von IP-Filterregeln

IP-Filterregeln werden der Reihenfolge nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

Wenn Sie beispielsweise Adressen im Bereich 70.37.104.0/24 zulassen und alle anderen Adressen ablehnen möchten, sollte die erste Regel im Raster lauten, dass der Adressbereich 70.37.104.0/24 zulässig ist. Mit der nächsten Regel sollten alle Adressen abgelehnt werden, indem der Bereich 0.0.0.0/0 verwendet wird.

> [!NOTE]
> Das Ablehnen von IP-Adressen kann andere Azure-Dienste (etwa Azure Stream Analytics, Azure Virtual Machines oder den Device Explorer im Portal) an der Interaktion mit dem Service Bus hindern.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Erstellen einer VNet-Regel mit Azure Resource Manager-Vorlagen

Mithilfe der folgenden Resource Manager-Vorlage können Sie einem vorhandenen Service Bus-Namespace eine VNET-Regel hinzufügen.

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
            "type": "Microsoft.ServiceBus/Namespaces/IPFilterRules",
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

Informationen zum Einschränken des Service Bus-Zugriffs für virtuelle Azure-Netzwerke finden Sie unter dem folgenden Link:

- [Use Virtual Network service endpoints with Azure Service Bus][lnk-vnet] (Verwenden von Dienstendpunkten im virtuellen Netzwerk für Service Bus)

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
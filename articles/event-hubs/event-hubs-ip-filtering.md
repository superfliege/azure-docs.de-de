---
title: Firewallregeln für Azure Event Hubs | Microsoft-Dokumentation
description: Verwenden von Firewallregeln zum Zulassen von Verbindungen von bestimmten IP-Adressen mit Azure Event Hubs.
services: event-hubs
documentationcenter: ''
author: spelluru
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.custom: seodec18
ms.topic: article
ms.date: 12/06/2018
ms.author: spelluru
ms.openlocfilehash: 707290d7bf453ca71dd3c5cf8b39c917b3a1c479
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53268273"
---
# <a name="use-firewall-rules"></a>Verwenden Sie Firewallregeln

Für Szenarien, in denen Azure Event Hubs nur von bestimmten bekannten Sites aus zugänglich sein soll, können Sie mithilfe von Firewallregeln Regeln für die Annahme von Datenverkehr konfigurieren, der von bestimmten IPv4-Adressen stammt. Bei diesen Adressen kann es sich beispielsweise um die Adressen eines unternehmenseigenen NAT-Gateways handeln.

## <a name="when-to-use"></a>Einsatzgebiete

Wenn Sie den Event-Hub-Namespace so einrichten möchten, dass er nur Datenverkehr aus einem bestimmten Bereich von IP-Adressen empfängt und allen anderen Datenverkehr, können Sie eine *Firewallregel* verwenden, um Event Hub-Endpunkte von anderen IP-Adressen zu blockieren. Sie verwenden z.B. Event Hubs mit [Azure Express Route][express-route], um private Verbindungen mit Ihrer lokalen Infrastruktur herzustellen.

## <a name="how-filter-rules-are-applied"></a>Anwenden von Filterregeln

Die IP-Filterregeln werden auf der Event Hubs-Namespaceebene angewendet. Daher gelten die Regeln für alle Clientverbindungen mit einem beliebigen unterstützten Protokoll.

Jeder Verbindungsversuch von einer IP-Adresse, die nicht mit einer zulässigen IP-Regel im Event Hubs-Namespace übereinstimmt, wird als nicht autorisiert abgelehnt. In der Antwort wird die IP-Regel nicht erwähnt.

## <a name="default-setting"></a>Standardeinstellung

Das Raster **IP-Filter** im Portal ist für Event Hubs standardmäßig leer. Diese Standardeinstellung bedeutet, dass Ihr Event Hub Verbindungen von allen IP-Adressen akzeptiert. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist.

## <a name="ip-filter-rule-evaluation"></a>Auswertung von IP-Filterregeln

IP-Filterregeln werden der Reihe nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

>[!WARNING]
> Durch das Implementieren von Firewalls kann verhindert werden, dass andere Azure-Dienste mit Event Hubs interagieren.
>
> Vertrauenswürdige Microsoft-Dienste werden bei der Implementierung von IP-Filterung (Firewalls) nicht unterstützt und werden in Kürze bereitgestellt.
>
> Allgemeine Azure-Szenarien, die nicht mit IP-Filterung funktionieren (beachten Sie, dass die Liste **NICHT** vollständig ist):
> - Azure Monitor
> - Azure Stream Analytics
> - Integration in Azure Event Grid
> - Azure IoT Hub-Routen
> - Azure IoT Device Explorer
> - Azure-Daten-Explorer
>
> Die folgenden Microsoft-Dienste müssen in einem virtuellen Netzwerk ausgeführt werden:
> - Azure Web Apps
> - Azure Functions

### <a name="creating-a-firewall-rule-with-azure-resource-manager-templates"></a>Erstellen einer Firewallregel mit Azure Resource Manager-Vorlagen

> [!IMPORTANT]
> Firewallregeln werden in den Tarifen **Standard** und **Dediziert** von Event Hubs unterstützt. Sie werden nicht im Basic-Tarif unterstützt.

Mithilfe der folgenden Resource Manager-Vorlage können Sie einem vorhandenen Event Hubs-Namespace eine IP-Filterregel hinzufügen.

Vorlagenparameter:

- **ipMask** ist eine einzelne IPv4-Adresse oder ein Block von IP-Adressen in CIDR-Notation. In CIDR-Notation steht beispielsweise 70.37.104.0/24 für die 256 IPv4-Adressen von 70.37.104.0 bis 70.37.104.255. „24“ gibt dabei die Anzahl signifikanter Präfixbits für den Bereich an.

> [!NOTE]
> Obwohl keine Verweigerungsregeln möglich sind, ist in der Azure Resource Manager-Vorlage die Standardaktion auf **„Zulassen“** festgelegt. Dies schränkt die Verbindungen nicht ein.
> Wenn Sie Regeln für virtuelle Netzwerke oder Firewalls erstellen, müssen Sie die ***„defaultAction“*** ändern.
> 
> from
> ```json
> "defaultAction": "Allow"
> ```
> zu
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
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
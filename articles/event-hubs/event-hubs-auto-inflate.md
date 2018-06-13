---
title: Automatische Erweiterung der Azure Event Hub-Durchsatzeinheiten | Microsoft-Dokumentation
description: Aktivieren der automatischen Vergrößerung in einem Namespace zur automatischen Erweiterung der Durchsatzeinheiten
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 20ee0e6cff2a07cbd62a79799eada5708c7a0f07
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28018608"
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automatische Erweiterung der Azure Event Hub-Durchsatzeinheiten

Azure Event Hubs ist eine extrem skalierbare Datenstreamingplattform. Daher nimmt die Event Hubs-Nutzung häufig zu, nachdem mit der Nutzung des Diensts begonnen wurde. Für solche Nutzung müssen die vordefinierten Durchsatzeinheiten zur Skalierung von Event Hubs und zur Verarbeitung größerer Übertragungsraten erweitert werden. Mit der Funktion *Automatische Vergrößerung* von Event Hubs wird die Anzahl von Durchsatzeinheiten automatisch den Nutzungsanforderungen angepasst. Zusätzliche Durchsatzeinheiten verhindern Drosselungsszenarien, bei denen Folgendes vorliegt:

* Die Datenerfassungsrate überschreitet die festgelegten Durchsatzeinheiten.
* Die Raten für ausgehende Datenanforderungen überschreiten die festgelegten Durchsatzeinheiten.

## <a name="how-auto-inflate-works"></a>Funktionsweise der automatischen Vergrößerung

Der Datenverkehr von Event Hubs wird von Durchsatzeinheiten gesteuert. Eine einzelne Durchsatzeinheit lässt eingehenden Datenverkehr von 1 MB pro Sekunde und die doppelte Menge an ausgehendem Datenverkehr zu. Event Hubs Standard kann mit 1 bis 20 Durchsatzeinheiten konfiguriert werden. Dank der automatischen Vergrößerung können Sie zunächst mit den mindestens erforderlichen Durchsatzeinheiten beginnen. Je nach der Zunahme des Datenverkehrs passt die Funktion die Durchsatzeinheiten automatisch bis zur maximal erforderlichen Anzahl an. Die automatische Vergrößerung hat folgende Vorteile:

- Effizienter Skalierungsmechanismus, der eine Vergrößerung entsprechend der Zunahme des Datenverkehrs ermöglicht
- Automatische Skalierung bis zur festgelegten Obergrenze ohne Probleme aufgrund einer Drosselung
- Mehr Kontrolle über die Skalierung, da Sie bestimmen, wann in welchem Umfang skaliert wird

## <a name="enable-auto-inflate-on-a-namespace"></a>Aktivieren der automatischen Vergrößerung in einem Namespace

Sie können die automatische Vergrößerung in einem Event Hubs-Namespace mit einer der folgenden Methoden aktivieren oder deaktivieren:

1. [Azure-Portal](https://portal.azure.com)
2. Azure Resource Manager-Vorlage

### <a name="enable-auto-inflate-through-the-portal"></a>Aktivieren der automatischen Vergrößerung über das Portal

Sie können das Feature zur automatischen Vergrößerung beim Erstellen eines Event Hubs-Namespace aktivieren:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Wenn diese Option aktiviert ist, können Sie mit wenigen Durchsatzeinheiten beginnen und die Anzahl mit zunehmenden Nutzungsanforderungen anpassen. Die Obergrenze für die Vergrößerung hat keine direkte Auswirkungen auf den Preis. Dieser hängt von der Anzahl von genutzten Durchsatzeinheiten pro Stunde ab.

Sie können die automatische Vergrößerung auch über die Option **Skalieren** im Bereich für Einstellungen im Portal aktivieren:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Aktivieren der automatischen Vergrößerung mithilfe einer Azure Resource Manager-Vorlage

Sie können die automatische Vergrößerung bei der Bereitstellung einer Azure Resource Manager-Vorlage aktivieren. Legen Sie beispielsweise die Eigenschaft `isAutoInflateEnabled` auf **TRUE** und `maximumThroughputUnits` auf 10 fest.

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

Die vollständige Vorlage finden Sie unter [Create Event Hubs namespace and enable inflate (Erstellen eines Event Hubs-Namespace und aktivieren der Vergrößerung)](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) auf GitHub.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)


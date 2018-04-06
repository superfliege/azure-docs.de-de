---
title: Konfigurieren von Hochverfügbarkeitsports für Azure Load Balancer | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Hochverfügbarkeitsports zum Lastenausgleich des internen Datenverkehrs an allen Ports verwenden können.
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/20178
ms.author: kumud
ms.openlocfilehash: 7f7f8e254e0ed0556446e7b08eaf46ec59977f62
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="configure-high-availability-ports-for-an-internal-load-balancer"></a>Konfigurieren von Hochverfügbarkeitsports für internen Lastenausgleich

Dieser Artikel umfasst eine Beispielbereitstellung von Hochverfügbarkeitsports für einen internen Lastenausgleich. Weitere Informationen zu Konfigurationen, die speziell für virtuelle Netzwerkgeräte (Network Virtual Appliances, NVAs) gelten, finden Sie auf den Websites der entsprechenden Anbieter.

>[!NOTE]
>Azure Load Balancer unterstützt zwei verschiedene Typen: Basic und Standard. In diesem Artikel wird der Load Balancer vom Typ „Standard“ beschrieben. Weitere Informationen zum Load Balancer vom Typ „Basic“ finden Sie unter [Übersicht über Azure Load Balancer](load-balancer-overview.md).

Die Abbildung zeigt die folgende Konfiguration des in diesem Artikel beschriebenen Bereitstellungsbeispiels:

- Die virtuellen Netzwerkgeräte werden im Back-End-Pool eines internen Lastenausgleichs hinter der Konfiguration der Hochverfügbarkeitsports bereitgestellt. 
- Die auf die DMZ-Subnetz angewendete benutzerdefinierte Route (UDR) leitet den gesamten Datenverkehr an die virtuellen Netzwerkgeräte weiter, indem der nächste Hop als virtuelle IP-Adresse des internen Lastenausgleichsmoduls durchgeführt wird. 
- Der interne Lastenausgleich verteilt den Datenverkehr entsprechend dem Lastenausgleichsalgorithmus auf eines der aktiven virtuellen Netzwerkgeräte.
- Das virtuelle Netzwerkgerät verarbeitet den Datenverkehr und leitet ihn an das ursprüngliche Ziel im Back-End-Subnetz weiter.
- Der Rückgabepfad kann die gleiche Route nehmen, wenn eine entsprechende UDR im Back-End-Subnetz konfiguriert ist. 

![Beispielbereitstellung für Hochverfügbarkeitsports](./media/load-balancer-configure-ha-ports/haports.png)


## <a name="preview-sign-up"></a>Registrierung für die Vorschauversion

Für die Teilnahme an der Vorschau des Features für Hochverfügbarkeitsports im Azure Load Balancer (Standard) registrieren Sie Ihr Abonnement mithilfe von Azure CLI 2.0 oder PowerShell, um Zugriff zu erhalten. Registrieren Sie Ihr Abonnement für [Load Balancer im Standard-Tarif (Vorschau)](https://aka.ms/lbpreview#preview-sign-up).

>[!NOTE]
>Die Registrierung der Vorschauversionen von Load Balancer im Standard-Tarif kann bis zu einer Stunde dauern.

## <a name="configure-high-availability-ports"></a>Konfigurieren von Hochverfügbarkeitsports

Richten Sie zur Konfiguration von Hochverfügbarkeitsports einen internen Lastenausgleich mit den virtuellen Netzwerkgeräten im Back-End-Pool ein. Richten Sie eine entsprechende Konfiguration des Integritätstests des Lastenausgleichs ein, um die Integrität der virtuellen Netzwerkgeräte und die Lastenausgleichsregel mit Hochverfügbarkeitsports zu ermitteln. Die auf den allgemeinen Lastenausgleich bezogene Konfiguration ist im Artikel [Erstellen eines internen Lastenausgleichs über das Azure-Portal](load-balancer-get-started-ilb-arm-portal.md) abgedeckt. Dieser Artikel hebt die Konfiguration von Hochverfügbarkeitsports hervor.

Die Konfiguration umfasst im Wesentlichen das Festlegen des Werts des Front-End- und Back-End-Ports auf **0**. Legen Sie als Protokollwert **Alle** fest. In diesem Artikel wird beschrieben, wie Sie Hochverfügbarkeitports mithilfe des Azure-Portals, PowerShell und Azure CLI 2.0 konfigurieren.

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-the-azure-portal"></a>Konfigurieren einer Lastenausgleichsregel für Hochverfügbarkeitsports mithilfe des Azure-Portals

Aktivieren Sie zum Konfigurieren von Hochverfügbarkeitsports mithilfe des Azure-Portals das Kontrollkästchen **HA-Ports**. Wenn diese ausgewählt ist, wird die entsprechende Port- und Protokollkonfiguration automatisch ausgefüllt. 

![Konfiguration von Hochverfügbarkeitsports über das Azure-Portal](./media/load-balancer-configure-ha-ports/haports-portal.png)


### <a name="configure-a-high-availability-ports-load-balancing-rule-via-the-resource-manager-template"></a>Konfigurieren einer Lastenausgleichsregel für Hochverfügbarkeitsports mithilfe der Resource Manager-Vorlage

Sie können die Hochverfügbarkeitsports mit der API-Version 2017-08-01 für Microsoft.Network/loadBalancers in der Load Balancer-Ressource konfigurieren. Der folgende JSON-Codeausschnitt zeigt die Änderungen an der Konfiguration für das Lastenausgleichsmodul für Hochverfügbarkeitsports über die REST-API:

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-powershell"></a>Konfigurieren einer Lastenausgleichsregel für Hochverfügbarkeitsports mithilfe von PowerShell

Verwenden Sie den folgenden Befehl, um die Lastenausgleichsregel für Hochverfügbarkeitsports während der Erstellung des internen Lastenausgleichs mithilfe von PowerShell zu erstellen:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-azure-cli-20"></a>Konfigurieren einer Lastenausgleichsregel für Hochverfügbarkeitsports mithilfe von Azure CLI 2.0

Verwenden Sie in Schritt 4 von [Erstellen eines internen Lastenausgleichs mithilfe der Azure-Befehlszeilenschnittstelle](load-balancer-get-started-ilb-arm-cli.md) den folgenden Befehl, um die Lastenausgleichsregel für Hochverfügbarkeitsports zu erstellen:

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Hochverfügbarkeitsports](load-balancer-ha-ports-overview.md).

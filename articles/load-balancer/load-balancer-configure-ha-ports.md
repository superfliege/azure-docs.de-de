---
title: "Konfigurieren von Hochverfügbarkeitsports für Azure Load Balancer | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Hochverfügbarkeitsports für den internen Lastenausgleichsdatenverkehr auf allen Ports verwenden."
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: 7256548b988812c64ca9a9f8a84fec377646635d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-high-availability-ports-for-internal-load-balancer"></a>Konfigurieren von Hochverfügbarkeitsports für einen internen Lastenausgleich

Dieser Artikel beinhaltet eine Beispielbereitstellung von Hochverfügbarkeitsports (High Availability, HA) auf einem internen Lastenausgleich. Für virtuelle Netzwerkgeräte spezifische Konfigurationen finden Sie auf den entsprechenden Anbieterwebsites.

>[!NOTE]
> Das Feature für Hochverfügbarkeitsports ist derzeit als Vorschauversion verfügbar. Während der Vorschauphase weist das Feature unter Umständen nicht die gleiche Verfügbarkeit und Zuverlässigkeit wie Features in Releases mit allgemeiner Verfügbarkeit auf. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Abbildung 1 zeigt die folgende Konfiguration des in diesem Artikel beschriebenen Bereitstellungsbeispiels:
- Die Netzwerkgeräte werden im Back-End-Pool eines internen Lastenausgleichs hinter der Konfiguration der Hochverfügbarkeitsports bereitgestellt. 
- Die auf die DMZ-Subnetz angewendete UDR leitet den gesamten Datenverkehr an <?> weiter, indem der nächste Hop als virtuelle IP-Adresse des internen Lastenausgleichs durchgeführt wird. 
- Der interne Lastenausgleich verteilt den Datenverkehr entsprechend dem LB-Algorithmus auf eines der aktiven Netzwerkgeräte.
- Das Netzwerkgerät verarbeitet den Datenverkehr und leitet ihn an das ursprüngliche Ziel im Back-End-Subnetz weiter.
- Der Rückgabepfad kann auch die gleiche Route nehmen, wenn eine entsprechende UDR im Back-End-Subnetz konfiguriert ist. 

![Hochverfügbarkeitsports Beispielbereitstellung](./media/load-balancer-configure-ha-ports/haports.png)

Abbildung 1: Netzwerkgeräte, die hinter einem internen Lastenausgleich mit Hochverfügbarkeitsports bereitgestellt sind 

## <a name="preview-sign-up"></a>Registrierung für die Vorschauversion

Für die Teilnahme an der Vorschau des Features für HA-Ports in der Load Balancer Standard-SKU registrieren Sie Ihr Abonnement mithilfe von PowerShell oder Azure CLI 2.0, um Zugriff zu erhalten.

- Registrierung mithilfe von PowerShell

   ```powershell
   Register-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```

- Registrierung mithilfe von Azure CLI 2.0

    ```cli
  az feature register --name AllowILBAllPortsRule --namespace Microsoft.Network  
    ```

## <a name="configuring-ha-ports"></a>Konfigurieren von Hochverfügbarkeitsports

Die Konfiguration der Hochverfügbarkeitsports umfasst die Einrichtung eines internen Lastenausgleichs mit den Netzwerkgeräten im Back-End-Pool, eine entsprechende Konfiguration des Integritätstests des Lastenausgleichs zum Erkennen der Integrität des Netzwerkgeräts sowie die Lastenausgleichsregel mit Hochverfügbarkeitsports. Die auf den allgemeinen Lastenausgleich bezogene Konfiguration ist im Artikel [Erstellen eines internen Lastenausgleichs über das Azure-Portal](load-balancer-get-started-ilb-arm-portal.md) abgedeckt. Dieser Artikel hebt die Konfiguration von Hochverfügbarkeitsports hervor.

Die Konfiguration umfasst im Wesentlichen das Festlegen des Werts des Front-End- und Back-End-Ports auf **0** sowie des Protokollwerts auf **Alle**. In diesem Artikel wird beschrieben, wie Sie Hochverfügbarkeitports mithilfe des Azure-Portals, PowerShell und Azure CLI 2.0 konfigurieren.

### <a name="configure-ha-ports-load-balancer-rule-with-the-azure-portal"></a>Konfigurieren einer Lastenausgleichsregel für Hochverfügbarkeitsports mithilfe des Azure-Portals

Das Azure-Portal umfasst die Option **HA-Ports** über ein Kontrollkästchen für diese Konfiguration. Wenn diese ausgewählt ist, wird die entsprechende Port- und Protokollkonfiguration automatisch ausgefüllt. 

![Konfiguration von Hochverfügbarkeitsports über das Azure-Portal](./media/load-balancer-configure-ha-ports/haports-portal.png)

Abbildung 2: Konfiguration von Hochverfügbarkeitsports über das Azure-Portal

### <a name="configure-ha-ports-load-balancer-rule-with-powershell"></a>Konfigurieren einer Lastenausgleichsregel für Hochverfügbarkeitsports mithilfe von PowerShell

Verwenden Sie den folgenden Befehl, um die Lastenausgleichsregel für Hochverfügbarkeitsports während der Erstellung des internen Lastenausgleichs mithilfe von PowerShell zu erstellen:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-ha-ports-load-balancer-rule-with-azure-cli-20"></a>Konfigurieren einer Lastenausgleichsregel für Hochverfügbarkeitsports mithilfe von Azure CLI 2.0

Verwenden Sie bei Schritt 4 von [Erstellen eines internen Lastenausgleichs mithilfe der Azure-Befehlszeilenschnittstelle](load-balancer-get-started-ilb-arm-cli.md) den folgenden Befehl, um die Lastenausgleichsregel für Hochverfügbarkeitsports zu erstellen.

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Hochverfügbarkeitsports](load-balancer-ha-ports-overview.md).

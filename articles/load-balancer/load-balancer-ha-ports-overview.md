---
title: "Übersicht über Hochverfügbarkeitsports in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über Hochverfügbarkeitsports, die auf einem internen Load Balancer einen Lastenausgleich vornehmen."
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.translationtype: HT
ms.sourcegitcommit: 57278d02a40aa92f07d61684e3c4d74aa0ac1b5b
ms.openlocfilehash: 2219aeb725b207fd92ff3e7603d7ee9c78f2844c
ms.contentlocale: de-de
ms.lasthandoff: 09/28/2017

---

# <a name="high-availability-ports-overview-preview"></a>Übersicht über Hochverfügbarkeitsports (Vorschauversion)

Die Standard-SKU von Azure Load Balancer enthält Hochverfügbarkeitsports (High Availability, HA) – eine Funktion, mit der der Datenverkehr von allen Ports und für alle unterstützten Protokolle verteilt werden kann. Beim Konfigurieren eines internen Load Balancers können Benutzer eine Regel für Hochverfügbarkeitsports konfigurieren, die die Front-End- und Back-End-Ports auf **0** und die Protokolle auf **all** festlegt. So kann der gesamte Datenverkehr den internen Load Balancer passieren.

>[!NOTE]
> Das Feature für Hochverfügbarkeitsports ist derzeit als Vorschauversion verfügbar. Während der Vorschauphase ist das Feature unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Der Algorithmus für den Lastenausgleich bleibt unverändert, und das Ziel wird auf Grundlage des 5-Tupels <IP-Quelladresse, Quellport, IP-Zieladresse, Zielport, Protokoll> ausgewählt. Diese Konfiguration ermöglicht es einer einzelnen Lastenausgleichsregel, den gesamten verfügbaren Datenverkehr zu verarbeiten. Außerdem werden die Komplexität der Konfiguration und die Beschränkungen reduziert, die durch die maximale Anzahl von hinzufügbaren Lastenausgleichsregeln festgelegt werden können.

Eines der wichtigsten Szenarios, das Hochverfügbarkeitsports ermöglichen, ist die Hochverfügbarkeitsbereitstellung der virtuellen Netzwerkgeräte in virtuellen Azure-Netzwerken. Ein weiteres gängiges Szenario für Hochverfügbarkeitsports ist der Lastenausgleich für einen Portbereich. 

## <a name="why-use-high-ha-ports"></a>Welche Vorteile bieten Hochverfügbarkeitsports?

Azure-Kunden nutzen fast ausschließlich virtuelle Netzwerkgeräte (Network Virtual Appliances, NVAs), um ihre Workloads gegen verschiedene Typen von Sicherheitsrisiken zu schützen. NVAs müssen außerdem zuverlässig und hochverfügbar sein.  

Hochverfügbarkeitsports vereinfachen Szenarios mit hochverfügbaren Netzwerkgeräten, indem sie die Notwendigkeit von komplexeren Lösungen wie Zookeeper abschaffen. Darüber hinaus erhöhen sie die Zuverlässigkeit mithilfe von Optionen für ein schnelleres Failover und horizontales Hochskalieren. Sie erhalten hochverfügbare Netzwerkgeräte, indem Sie dem Back-End-Pool des Azure-internen Load Balancers NVAs hinzufügen und anschließend die Load Balancer-Regel für Hochverfügbarkeitsports konfigurieren.

Im folgenden Beispiel wird eine Hub-and-Spoke-Bereitstellung eines virtuellen Netzwerks dargestellt. Die Spokes erzwingen dabei das Tunneln ihres Datenverkehrs an das virtuelle Hub-Netzwerk und über das virtuelle Netzwerkgerät, bevor der vertrauenswürdige Bereich verlassen wird. Die NVAs befinden sich hinter einem internen Load Balancer mit Hochverfügbarkeitsports und können daher den gesamten Datenverkehr verarbeiten und entsprechend weiterleiten. 

![Beispiel für Hochverfügbarkeitsports](./media/load-balancer-ha-ports-overview/nvaha.png)

Abbildung 1: Ein virtuelles Hub-and-Spoke-Netzwerk mit NVAs, die im Hochverfügbarkeitsmodus bereitgestellt wurden


## <a name="region-availability"></a>Regionale Verfügbarkeit

Die Funktion für HA-Ports ist derzeit in den folgenden Regionen verfügbar:
- USA (Ost) 2
- USA (Mitte)
- Nordeuropa
- USA, Westen-Mitte
- Europa, Westen
- Asien, Südosten 

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
## <a name="caveats"></a>Einschränkungen

Hier finden Sie die unterstützten Konfigurationen oder Ausnahmen für Hochverfügbarkeitsports:

- Eine einzelne Front-End-IP-Konfiguration kann nur entweder eine einzelne DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports (alle Ports) oder eine einzelne Nicht-DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports (alle Ports) haben, niemals beides gleichzeitig.
- Eine einzelne Netzwerkschnittstellen-IP-Konfiguration kann nur eine Nicht-DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports haben. Für diese IP-Konfiguration können keine anderen Regeln konfiguriert werden.
- Eine einzelne Netzwerkschnittstellen-IP-Konfiguration kann mindestens eine DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports haben, vorausgesetzt, dass alle ihre jeweiligen Front-End-IP-Konfigurationen eindeutig sind.
- Zwei (oder mehr) Lastenausgleichsregeln, die auf denselben Back-End-Pool verweisen, können gleichzeitig vorhanden sein, wenn alle Lastenausgleichsregeln Hochverfügbarkeitsports (nur DSR) oder alle Regeln nur Nicht-Hochverfügbarkeitsports (DSR und Nicht-DSR) haben. Zwei solcher Lastenausgleichsregeln können nicht gleichzeitig vorhanden sein, wenn Hochverfügbarkeitsport- und Nicht-Hochverfügbarkeitsportregeln kombiniert wurden.
- Auf IPv6-fähigen Mandanten sind Hochverfügbarkeitsports nicht verfügbar.


## <a name="next-steps"></a>Nächste Schritte

[Configure HA Ports on an Internal Load balancer (Konfigurieren von Hochverfügbarkeitsport auf einem internen Load Balancer)](load-balancer-configure-ha-ports.md)



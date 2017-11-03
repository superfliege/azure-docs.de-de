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
ms.openlocfilehash: 62d78e067e50183f25af84e547db2e11c0014f5d
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="high-availability-ports-overview-preview"></a>Übersicht über Hochverfügbarkeitsports (Vorschauversion)

Mit Azure Load Balancer Standard wird eine neue Funktion für den gleichzeitigen Lastenausgleich von TCP- und UDP-Datenflüssen an allen Ports eingeführt, wenn ein interner Lastenausgleich verwendet wird. 

>[!NOTE]
> Das Feature für Hochverfügbarkeitsports ist derzeit als Vorschauversion mit Load Balancer Standard verfügbar. Während der Vorschauphase weist das Feature unter Umständen nicht die gleiche Verfügbarkeit und Zuverlässigkeit wie Features in Releases mit allgemeiner Verfügbarkeit auf. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Damit Sie Hochverfügbarkeitsports mit Load Balancer Standard-Ressourcen verwenden können, müssen Sie sich für die Load Balancer Standard-Vorschauversion registrieren. Befolgen Sie zusätzlich zur Load Balancer [Standard-Vorschauversion](https://aka.ms/lbpreview#preview-sign-up) die Hinweise zur Registrierung.

Eine Regel für Hochverfügbarkeitsports ist eine Variante einer Lastenausgleichsregel, die für eine interne Load Balancer Standard-Instanz konfiguriert wurde.  Szenarien werden durch die Bereitstellung einer einzelnen Lastenausgleichsregel für den Lastenausgleich aller TCP- und UDP-Datenflüsse, die an allen Ports eines internen Load Balancer Standard-Front-Ends eingehen, vereinfacht. Die Entscheidung über den Lastenausgleich erfolgt pro Datenfluss basierend auf dem fünfteiligen Tupel aus Quell-IP-Adresse, Quellport, Ziel-IP-Adresse, Ziel-Port und Protokoll.

Hochverfügbarkeitsports ermöglichen kritische Szenarien wie Hochverfügbarkeit und Skalierung für virtuelle Netzwerkgeräte (Network Virtual Appliances, NVAs) in virtuellen Netzwerken sowie andere Szenarien, in denen für eine große Anzahl von Ports ein Lastenausgleich vorgenommen werden muss. 

Hochverfügbarkeitsports werden durch Festlegen der Front-End- und Back-End-Ports auf **0** und des Protokolls auf **Alle** konfiguriert.  Die interne Ressource für den Lastenausgleich nimmt nun für alle TCP- und UDP-Datenflüsse unabhängig von der Portnummer einen Lastenausgleich vor.

## <a name="why-use-ha-ports"></a>Gründe für die Verwendung von Hochverfügbarkeitsports

### <a name="network-virtual-appliances"></a>Virtuelle Netzwerkgeräte

Sie können virtuelle Netzwerkgeräte (Network Virtual Appliances, NVAs) verwenden, um Ihre Azure-Workload vor verschiedenen Typen von Sicherheitsrisiken zu schützen. Wenn NVAs in diesen Szenarien verwendet werden, müssen sie zuverlässig, hochverfügbar und bedarfsgerecht horizontal hochskalierbar sein.

Sie können diese Ziele in Ihrem Szenario erreichen, indem Sie einfach NVA-Instanzen zum Back-End-Pool des internen Azure-Lastenausgleichsmoduls hinzufügen und eine Lastenausgleichsregel für Hochverfügbarkeitsports konfigurieren.

Hochverfügbarkeitsports bieten mehrere Vorteile für NVA-Hochverfügbarkeitsszenarien:
- Schnelles Failover auf fehlerfreie Instanzen mit Integritätstests pro Instanz
- Höhere Leistung mit horizontalem Hochskalieren auf n aktive Instanzen
- Szenarien mit n aktiven sowie aktiven und passiven Instanzen
- Komplexe Lösungen wie Zookeeper-Knoten für die Überwachung von Geräten werden überflüssig

Im folgenden Beispiel wird eine Hub-and-Spoke-Bereitstellung eines virtuellen Netzwerks dargestellt. Die Spokes erzwingen dabei das Tunneln ihres Datenverkehrs an das virtuelle Hubnetzwerk und über das virtuelle Netzwerkgerät, bevor der vertrauenswürdige Bereich verlassen wird. Die virtuellen Netzwerkgeräte befinden sich hinter einer internen Load Balancer Standard-Instanz mit Konfiguration von Hochverfügbarkeitsports.  Der gesamte Datenverkehr kann entsprechend verarbeitet und weitergeleitet werden. 

![Beispiel für Hochverfügbarkeitsports](./media/load-balancer-ha-ports-overview/nvaha.png)

Abbildung 1: Ein virtuelles Hub-and-Spoke-Netzwerk mit NVAs, die im Hochverfügbarkeitsmodus bereitgestellt wurden

Bei Verwendung von virtuellen Netzwerkgeräten überprüfen Sie beim jeweiligen Anbieter, wie Hochverfügbarkeitsports optimal genutzt und welche Szenarien unterstützt werden.

### <a name="load-balancing-large-numbers-of-ports"></a>Lastenausgleich für eine große Anzahl von Ports

Sie können Hochverfügbarkeitsports auch für Anwendungsszenarien verwenden, in denen ein Lastenausgleich für eine große Anzahl von Ports erforderlich ist. Diese Szenarien können mithilfe einer internen [Load Balancer Standard](https://aka.ms/lbpreview)-Instanz mit Hochverfügbarkeitsports vereinfacht werden. Dabei ersetzt eine einzige Lastenausgleichsregel mehrere einzelne Lastenausgleichsregeln, eine für jeden Port.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Hochverfügbarkeitsports sind in den [gleichen Regionen wie Load Balancer Standard verfügbar](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Registrierung für die Vorschauversion

Für die Teilnahme an der Vorschau des Features für Hochverfügbarkeitsports in Load Balancer Standard registrieren Sie Ihr Abonnement mithilfe von Azure CLI 2.0 oder PowerShell, um Zugriff zu erhalten.  Führen Sie die folgenden drei Schritte aus:

>[!NOTE]
>Um dieses Feature verwenden zu können, müssen Sie sich zusätzlich zu Hochverfügbarkeitsports auch für die Load Balancer [Standard-Vorschauversion](https://aka.ms/lbpreview#preview-sign-up) registrieren. Die Registrierung für die Vorschauversion von Hochverfügbarkeitsports oder Load Balancer Standard kann bis zu einer Stunde dauern.

### <a name="sign-up-using-azure-cli-20"></a>Registrierung mithilfe von Azure CLI 2.0

1. Registrieren des Features beim Anbieter
    ```cli
    az feature register --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
2. Dieser Vorgang kann bis zu 10 Minuten dauern.  Sie können den Status des Vorgangs mithilfe des folgenden Befehls überprüfen:

    ```cli
    az feature show --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
    Fahren Sie mit Schritt 3 fort, wenn für die Featureregistrierung der Status „Registriert“ zurückgegeben wird, wie unten dargestellt:
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowILBAllPortsRule",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
3. Schließen Sie die Registrierung für die Vorschauversion ab, indem Sie Ihr Abonnement erneut beim Ressourcenanbieter registrieren:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    
### <a name="sign-up-using-powershell"></a>Registrierung mithilfe von PowerShell

1. Registrieren des Features beim Anbieter
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    
2. Dieser Vorgang kann bis zu 10 Minuten dauern.  Sie können den Status des Vorgangs mithilfe des folgenden Befehls überprüfen:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    Fahren Sie mit Schritt 3 fort, wenn für die Featureregistrierung der Status „Registriert“ zurückgegeben wird, wie unten dargestellt:
   
    ```
    FeatureName          ProviderName      RegistrationState
    -----------          ------------      -----------------
    AllowILBAllPortsRule Microsoft.Network Registered
    ```
    
3. Schließen Sie die Registrierung für die Vorschauversion ab, indem Sie Ihr Abonnement erneut beim Ressourcenanbieter registrieren:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```


## <a name="limitations"></a>Einschränkungen

Hier finden Sie die unterstützten Konfigurationen oder Ausnahmen für Hochverfügbarkeitsports:

- Eine einzelne Front-End-IP-Konfiguration kann nur entweder eine einzelne DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports oder eine einzelne Nicht-DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports enthalten, niemals beides gleichzeitig.
- Eine einzelne Netzwerkschnittstellen-IP-Konfiguration kann nur eine Nicht-DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports aufweisen. Für diese IP-Konfiguration können keine anderen Regeln konfiguriert werden.
- Eine einzelne Netzwerkschnittstellen-IP-Konfiguration kann mindestens eine DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports haben, vorausgesetzt, dass alle ihre jeweiligen Front-End-IP-Konfigurationen eindeutig sind.
- Wenn alle Lastenausgleichsregeln Hochverfügbarkeitsports (nur DSR) oder alle Regeln nur Nicht-Hochverfügbarkeitsports (DSR und Nicht-DSR) aufweisen, können gleichzeitig zwei (oder mehr) Lastenausgleichsregeln, die auf denselben Back-End-Pool verweisen, vorhanden sein. Zwei solcher Lastenausgleichsregeln können nicht gleichzeitig vorhanden sein, wenn Hochverfügbarkeitsport- und Nicht-Hochverfügbarkeitsportregeln kombiniert wurden.
- Hochverfügbarkeitsports sind für IPv6 nicht verfügbar.


## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Hochverfügbarkeitsports auf einem internen Load Balancer Standard-Modul](load-balancer-configure-ha-ports.md)
- [Erfahren Sie mehr über die Load Balancer Standard-Vorschau](https://aka.ms/lbpreview)


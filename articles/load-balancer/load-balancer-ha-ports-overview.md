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
ms.openlocfilehash: 46e284d1636988390f3533d93bfd07399f45dc92
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/09/2017
---
# <a name="high-availability-ports-overview"></a>Übersicht über Hochverfügbarkeitsports

Azure Load Balancer Standard unterstützt Sie beim gleichzeitigen Lastenausgleich von TCP- und UDP-Datenflüssen an allen Ports, wenn Sie einen internen Load Balancer verwenden. 

>[!NOTE]
> Das Feature für Hochverfügbarkeitsports (HA) ist derzeit als Vorschauversion mit Load Balancer Standard verfügbar. Während der Previewphase weist das Feature unter Umständen nicht die gleiche Verfügbarkeit und Zuverlässigkeit wie Features in Releases mit allgemeiner Verfügbarkeit auf. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Registrieren Sie sich für die Load Balancer Standard-Vorschauversion, damit Sie Hochverfügbarkeitsports mit Load Balancer Standard-Ressourcen verwenden können. Befolgen Sie auch die Hinweise zur Registrierung für die Load Balancer [Standard-Vorschauversion](https://aka.ms/lbpreview#preview-sign-up).

Eine Regel für Hochverfügbarkeitsports ist eine Variante einer Lastenausgleichsregel, die für eine interne Load Balancer Standard-Instanz konfiguriert wurde. Sie können Ihre Verwendung von Load Balancer vereinfachen, indem Sie eine einzelne Lastenausgleichsregel für den Lastenausgleich aller TCP- und UDP-Datenflüsse bereitstellen, die an allen Ports eines internen Load Balancer Standard eingehen. Die Entscheidung über den Lastenausgleich erfolgt pro Datenfluss. Dies basiert auf der folgenden Verbindung, die sich aus einem fünfteiligen Tupel zusammensetzt: Quell-IP-Adresse, Quellport, Ziel-IP-Adresse, Zielport und Protokoll.

Das Feature für Hochverfügbarkeitsports unterstützt Sie bei wichtigen Szenarien, z. B. Hochverfügbarkeit und Skalierung für virtuelle Netzwerkgeräte in virtuellen Netzwerken. Es kann auch hilfreich sein, wenn für eine große Anzahl von Ports ein Lastenausgleich vorgenommen werden muss. 

Das Feature für Hochverfügbarkeitsports ist konfiguriert, wenn Sie für die Front-End- und Back-End-Ports **0** und für das Protokoll **Alle** festlegen. Die interne Load Balancer-Ressource nimmt dann für alle TCP- und UDP-Datenflüsse unabhängig von der Portnummer einen Lastenausgleich vor.

## <a name="why-use-ha-ports"></a>Gründe für die Verwendung von Hochverfügbarkeitsports

### <a name="nva"></a>Virtuelle Netzwerkgeräte

Sie können virtuelle Netzwerkgeräte (Network Virtual Appliances, NVAs) verwenden, um Ihre Azure-Workload vor verschiedenen Typen von Sicherheitsrisiken zu schützen. Wenn virtuelle Netzwerkgeräte in diesen Szenarien verwendet werden, müssen sie zuverlässig, hochverfügbar und bedarfsgerecht horizontal hochskalierbar sein.

Sie können diese Ziele erreichen, indem Sie einfach NVA-Instanzen zum Back-End-Pool des internen Azure Load Balancers hinzufügen und eine Lastenausgleichsregel für Hochverfügbarkeitsports konfigurieren.

Hochverfügbarkeitsports bieten mehrere Vorteile für NVA-Hochverfügbarkeitsszenarien:
- Schnelles Failover auf fehlerfreie Instanzen mit Integritätstests pro Instanz
- Höhere Leistung mit horizontalem Hochskalieren auf *n* aktive Instanzen
- Szenarien mit *n* aktiven sowie aktiven und passiven Instanzen
- Komplexe Lösungen wie Apache ZooKeeper-Knoten für die Überwachung von Geräten werden überflüssig

Das folgende Diagramm zeigt eine Nabe-zu-Speiche-Bereitstellung eines virtuellen Netzwerks. Die Speichen erzwingen das Tunneling für ihren Datenverkehr zum virtuellen Nabennetzwerk und über das virtuelle Netzwerkgerät, bevor der vertrauenswürdige Bereich verlassen wird. Die virtuellen Netzwerkgeräte befinden sich hinter einem internen Load Balancer Standard mit einer Konfiguration für die Hochverfügbarkeitsports. Der gesamte Datenverkehr kann entsprechend verarbeitet und weitergeleitet werden.

![Diagramm eines virtuellen Nabe-zu-Speiche-Netzwerks mit virtuellen Netzwerkgeräten, die im Hochverfügbarkeitsmodus bereitgestellt wurden](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Bei Verwendung von virtuellen Netzwerkgeräten überprüfen Sie beim jeweiligen Anbieter, wie Hochverfügbarkeitsports optimal genutzt und welche Szenarien unterstützt werden.

### <a name="load-balancing-large-numbers-of-ports"></a>Lastenausgleich für eine große Anzahl von Ports

Sie können Hochverfügbarkeitsports auch für Anwendungen verwenden, in denen ein Lastenausgleich für eine große Anzahl von Ports erforderlich ist. Sie können diese Szenarien vereinfachen, indem Sie einen internen [Load Balancer Standard](https://aka.ms/lbpreview) mit Hochverfügbarkeitsports verwenden. Eine einzelne Lastenausgleichsregel ersetzt mehrere einzelne Lastenausgleichsregeln, die jeweils für einen Port verwendet wurden.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Das Feature für Hochverfügbarkeitsports ist in den [gleichen Regionen wie Load Balancer Standard verfügbar](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Registrierung für die Vorschauversion

Für die Teilnahme an der Vorschauversion des Features für Hochverfügbarkeitsports in Load Balancer Standard registrieren Sie Ihr Abonnement für die [Vorschauversion von Load Balancer Standard](https://aka.ms/lbpreview#preview-sign-up). Sie können für die Registrierung entweder Azure CLI 2.0 oder PowerShell verwenden.

>[!NOTE]
>Die Registrierung kann bis zu einer Stunde dauern.

## <a name="limitations"></a>Einschränkungen

Nachfolgend sind die unterstützten Konfigurationen oder Ausnahmen für das Feature für Hochverfügbarkeitsports aufgeführt:

- Eine einzelne Front-End-IP-Konfiguration kann nur eine einzelne DSR-Lastenausgleichsregel (Direct Server Return – Floating IP in Azure) mit Hochverfügbarkeitsports oder eine einzelne Nicht-DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports enthalten, niemals beides gleichzeitig.
- Eine einzelne Netzwerkschnittstellen-IP-Konfiguration kann nur eine Nicht-DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports aufweisen. Sie können für diese IP-Konfiguration keine anderen Regeln konfigurieren.
- Eine IP-Konfiguration einer einzelnen Netzwerkschnittstelle kann mindestens eine DSR-Lastenausgleichsregel mit Hochverfügbarkeitsports aufweisen, vorausgesetzt, dass alle ihre jeweiligen Front-End-IP-Konfigurationen eindeutig sind.
- Wenn alle Lastenausgleichsregeln Hochverfügbarkeitsports (nur DSR) aufweisen, können gleichzeitig zwei (oder mehr) Load Balancer-Regeln, die auf denselben Back-End-Pool verweisen, vorhanden sein. Dasselbe gilt, wenn alle Regeln Nicht-Hochverfügbarkeitsports (DSR und Nicht-DSR) aufweisen. Wenn Hochverfügbarkeitsport- und Nicht-Hochverfügbarkeitsportregeln kombiniert wurden, können zwei dieser Lastenausgleichsregeln jedoch nicht gleichzeitig vorhanden sein.
- Das Feature für Hochverfügbarkeitsports ist für IPv6 nicht verfügbar.
- Flowsymmetrie für NVA-Szenarien wird nur mit einer einzelnen NIC unterstützt. Sehen Sie sich die Beschreibung und das Diagramm unter [Virtuelle Netzwerkgeräte](#nva) an. 



## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Hochverfügbarkeitsports auf einem internen Load Balancer Standard-Modul](load-balancer-configure-ha-ports.md)
- [Erfahren Sie mehr über die Load Balancer Standard-Vorschau](https://aka.ms/lbpreview)


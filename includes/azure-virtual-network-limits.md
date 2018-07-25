---
title: Includedatei
description: Includedatei
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 06/20/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: 9ba9bc993832350f6b6ce1c642e2dc852731b6f0
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39029963"
---
<a name="virtual-networking-limits-classic"></a>Die folgenden Grenzwerte gelten nur für Netzwerkressourcen, die über das klassische Bereitstellungsmodell verwaltet werden (pro Abonnement). Erfahren Sie, wie Sie [die aktuelle Ressourcennutzung anhand der Grenzwerte Ihres Abonnements anzeigen](../articles/networking/check-usage-against-limits.md).

| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Virtuelle Netzwerke |50 |100 |
| Lokale Netzwerksites |20 |an den Support wenden |
| DNS-Server pro virtuellem Netzwerk |20 |100 |
| Private IP-Adressen pro virtuellem Netzwerk |4096 |4096 |
| Parallele TCP-Flüsse pro NIC eines virtuellen Computers bzw. einer Rolleninstanz |500K |500K |
| Netzwerksicherheitsgruppen |100 |200 |
| NSG-Regeln pro NSG |200 |1000 |
| Benutzerdefinierte Routentabellen |100 |200 |
| Benutzerdefinierte Routen pro Routentabelle |100 |400 |
| Öffentliche IP-Adressen (dynamisch) |5 |an den Support wenden |
| Reservierte öffentliche IP-Adressen |20 |an den Support wenden |
| Öffentliche VIP-Adressen pro Bereitstellung |5 |an den Support wenden |
| Private VIP-Adressen (ILB) pro Bereitstellung |1 |1 |
| Endpunkt-Zugriffssteuerungslisten (ACLs) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Netzwerkgrenzwerte – Azure Resource Manager
Die folgenden Grenzwerte gelten nur für Netzwerkressourcen, die über den Azure Resource Manager pro Region pro Abonnement verwaltet werden. Erfahren Sie, wie Sie [die aktuelle Ressourcennutzung anhand der Grenzwerte Ihres Abonnements anzeigen](../articles/networking/check-usage-against-limits.md).

| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Virtuelle Netzwerke |50 |1000 |
| Subnetze pro virtuellem Netzwerk |1000 |10000 |
| VNET-Peerings pro virtuellem Netzwerk |50** |100 |
| DNS-Server pro virtuellem Netzwerk |9 |25 |
| Private IP-Adressen pro virtuellem Netzwerk |16.384** |16384 |
| Private IP-Adressen pro Netzwerkschnittstelle |256 |256 |
| Parallele TCP-Flüsse pro NIC eines virtuellen Computers bzw. einer Rolleninstanz |500K |500K |
| Netzwerkschnittstellen |24.000** |24.000 |
| Netzwerksicherheitsgruppen |100 |5.000 |
| NSG-Regeln pro NSG |1.000** |1000 |
| Angegebene IP-Adressen und -Bereiche für die Quelle oder das Ziel in einer Sicherheitsgruppe |2000 |4000 |
| Anwendungssicherheitsgruppen |500 |3000 |
| Anwendungssicherheitsgruppen pro IP-Konfiguration pro NIC |10 |20 |
| IP-Konfigurationen pro Anwendungssicherheitsgruppe |1000 |4000 |
| Anwendungssicherheitsgruppen, die in allen Sicherheitsregeln einer Netzwerksicherheitsgruppe angegeben werden können |50 |100 |
| Benutzerdefinierte Routentabellen |100 |200 |
| Benutzerdefinierte Routen pro Routentabelle |400** |400 |
| Öffentliche IP-Adressen – dynamisch |(Basic) 60 |an den Support wenden |
| Öffentliche IP-Adressen – statisch |(Basic) 20 |an den Support wenden |
| Öffentliche IP-Adressen – statisch |(Standard) 20 |an den Support wenden |
| Punkt-zu-Standort-Stammzertifikate pro VPN Gateway |20 |20 |

**Diese aktualisierten Standardlimits gelten für Abonnements, für die diese Limits nicht zuvor durch den Support erhöht wurden. Wenn diese Grenzwerte für Sie in der Vergangenheit durch den Support erhöht wurden, und Sie eine Aktualisierung auf neue Standardwerte wünschen, [öffnen Sie bitte eine kostenlose Online-Kundensupportanfrage](../articles/azure-resource-manager/resource-manager-quota-errors.md).

#### <a name="load-balancer"></a>Load Balancer-Grenzwerte
Die folgenden Grenzwerte gelten nur für Netzwerkressourcen, die über den Azure Resource Manager pro Region pro Abonnement verwaltet werden. Erfahren Sie, wie Sie [die aktuelle Ressourcennutzung im Vergleich zu den Grenzwerten Ihres Abonnements anzeigen](../articles/networking/check-usage-against-limits.md).

| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Load Balancer | 100 | 1000 |
| Regeln pro Ressource, Basic | 150 | 250 |
| Regeln pro Ressource, Standard | 1250 | 1500 |
| Regeln pro IP-Konfiguration | 299 |299 |
| Front-End-IP-Konfiguration, Basic | 10 | 200 |
| Front-End-IP-Konfiguration, Standard | 10 | 600 |
| Back-End-Pool, Basic | 100, eine Verfügbarkeitsgruppe | 100, eine Verfügbarkeitsgruppe |
| Back-End-Pool, Standard | 1000, einzelnes VNET | 1000, einzelnes VNET |
| Back-End-Ressourcen pro Lastenausgleich, Standard &ast; | 50 | 150 |
| Hochverfügbarkeitsports, Standard | 1 pro internem Front-End | 1 pro internem Front-End |

&ast; Bis zu 150 Ressourcen, jede Kombination aus eigenständigen virtuellen Computern, Verfügbarkeitsgruppen und VM-Skalierungsgruppen.

[Wenden Sie sich an den Support](../articles/azure-supportability/resource-manager-core-quotas-request.md ), falls Sie Limits der Standardwerte erhöhen müssen.


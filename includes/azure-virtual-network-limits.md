---
title: Includedatei
description: Includedatei
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 08/16/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: 0394d1cc44dae270682691e2091543e5aab321f6
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513937"
---
<a name="virtual-networking-limits-classic"></a>Die folgenden Grenzwerte gelten nur für Netzwerkressourcen, die über das klassische Bereitstellungsmodell verwaltet werden (pro Abonnement). Erfahren Sie, wie Sie [die aktuelle Ressourcennutzung anhand der Grenzwerte Ihres Abonnements anzeigen](../articles/networking/check-usage-against-limits.md).

| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Virtuelle Netzwerke |50 |100 |
| Lokale Netzwerksites |20 |an den Support wenden |
| DNS-Server pro virtuellem Netzwerk |20 |20 |
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

> [!NOTE]
> Vor Kurzem wurden die Standardgrenzwerte auf die maximalen Grenzwerte erhöht. Falls die Spalte **Maximales Limit** nicht vorhanden ist, gelten für die entsprechende Ressource keine anpassbaren Einschränkungen. Wenn diese Grenzwerte für Sie in der Vergangenheit durch den Support erhöht wurden und die aktualisierten Grenzwerte nicht wie unten angegeben angezeigt werden, können Sie [eine kostenlose Online-Kundensupportanfrage erstellen](../articles/azure-resource-manager/resource-manager-quota-errors.md).

| Ressource | Standardlimit | 
| --- | --- |
| Virtuelle Netzwerke |1000 |
| Subnetze pro virtuellem Netzwerk |3000 |
| VNET-Peerings pro virtuellem Netzwerk |100 |
| DNS-Server pro virtuellem Netzwerk |20 |
| Private IP-Adressen pro virtuellem Netzwerk |65536 |
| Private IP-Adressen pro Netzwerkschnittstelle |256 |
| Private IP-Adressen pro virtuellem Computer |256 |
| Parallele TCP-Flüsse pro NIC eines virtuellen Computers bzw. einer Rolleninstanz |500K |
| Netzwerkschnittstellen |65536 |
| Netzwerksicherheitsgruppen |5.000 |
| NSG-Regeln pro NSG |1000 |
| Angegebene IP-Adressen und -Bereiche für die Quelle oder das Ziel in einer Sicherheitsgruppe |4000 |
| Anwendungssicherheitsgruppen |3000 |
| Anwendungssicherheitsgruppen pro IP-Konfiguration pro NIC |20 |
| IP-Konfigurationen pro Anwendungssicherheitsgruppe |4000 |
| Anwendungssicherheitsgruppen, die in allen Sicherheitsregeln einer Netzwerksicherheitsgruppe angegeben werden können |100 |
| Benutzerdefinierte Routentabellen |200 |
| Benutzerdefinierte Routen pro Routentabelle |400 |
| Punkt-zu-Standort-Stammzertifikate pro VPN Gateway |20 |
| TAPs für virtuelle Netzwerke |100 |
| Netzwerkschnittstellen-TAP-Konfigurationen pro virtuellem Netzwerk-TAP |100 |

#### <a name="publicip-address"></a>Grenzwerte für öffentliche IP-Adressen
| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Öffentliche IP-Adressen – dynamisch |(Basic) 1000 |an den Support wenden |
| Öffentliche IP-Adressen – statisch |(Basic) 200 |an den Support wenden |
| Öffentliche IP-Adressen – statisch |(Standard) 200 |an den Support wenden |
| Größe des Präfix für öffentliche IP-Adressen (Vorschau) | /28 | /28 |

#### <a name="load-balancer"></a>Load Balancer-Grenzwerte
Die folgenden Grenzwerte gelten nur für Netzwerkressourcen, die über den Azure Resource Manager pro Region pro Abonnement verwaltet werden. Erfahren Sie, wie Sie [die aktuelle Ressourcennutzung im Vergleich zu den Grenzwerten Ihres Abonnements anzeigen](../articles/networking/check-usage-against-limits.md).

| Ressource | Standardlimit |
| --- | --- | --- |
| Load Balancer | 1000 | 
| Regeln pro Ressource, Basic | 250 |
| Regeln pro Ressource, Standard | 1500 | 
| Regeln pro IP-Konfiguration | 299 |
| Front-End-IP-Konfiguration, Basic | 200 |
| Front-End-IP-Konfiguration, Standard | 600 |
| Back-End-Pool, Basic | 100, eine Verfügbarkeitsgruppe |
| Back-End-Pool, Standard | 1000, einzelnes VNET |
| Back-End-Ressourcen pro Lastenausgleich, Standard * | 150 |
| Hochverfügbarkeitsports, Standard | 1 pro internem Front-End |

** Bis zu 150 Ressourcen, jede Kombination aus eigenständigen virtuellen Computern, Verfügbarkeitsgruppen und VM-Skalierungsgruppen.

[Wenden Sie sich an den Support](../articles/azure-supportability/resource-manager-core-quotas-request.md ), falls Sie Limits der Standardwerte erhöhen müssen.


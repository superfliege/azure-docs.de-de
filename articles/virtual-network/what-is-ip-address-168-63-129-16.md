---
title: Was ist die IP-Adresse 168.63.129.16? | Microsoft-Dokumentation
description: Erfahren Sie mehr zur IP-Adresse 168.63.129.16 und deren Funktionsweise mit Ihren Ressourcen.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: v-jesits
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2019
ms.author: genli
ms.openlocfilehash: 7f0539e7c2f7e5ae8847b35b47d3708c6c5d6a09
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55296037"
---
# <a name="what-is-ip-address-1686312916"></a>Was ist die IP-Adresse 168.63.129.16?

Die IP-Adresse 168.63.129.16 ist eine virtuelle öffentliche IP-Adresse, die verwendet wird, um einen Kommunikationskanal zu den Ressourcen der Azure-Plattform zu ermöglichen. Kunden können in Azure einen beliebigen Adressraum für ihr privates virtuelles Netzwerk definieren. Daher müssen die Ressourcen der Azure-Plattform als eindeutige öffentliche IP-Adresse dargestellt werden. Diese virtuelle öffentliche IP-Adresse erleichtert die folgenden Vorgänge:

- Ermöglicht die Kommunikation zwischen VM-Agent und Azure-Plattform, um zu signalisieren, dass er sich im Zustand „Bereit“ befindet.
- Ermöglicht die Kommunikation mit dem virtuellen DNS-Server, um für die Ressource (wie einen VM) eine gefilterte Namensauflösung bereitzustellen, die keinen benutzerdefinierten DNS-Server hat. Dieser Filter stellt sicher, dass Kunden nur die Hostnamen ihrer Ressourcen auflösen können.
- Ermöglicht Integritätstests des Load Balancers, um den Integritätszustand der VMs in einer Menge mit Lastenausgleich zu ermitteln.
- Ermöglicht Taktmeldungen für einen Gast-Agent für die PaaS-Rolle.

## <a name="scope-of-ip-address-1686312916"></a>Bereich der IP-Adresse 168.63.129.16

Die virtuelle öffentliche IP-Adresse 168.63.129.16 wird in allen Regionen und allen nationalen Clouds verwendet. Diese spezielle öffentliche IP-Adresse wird nicht geändert werden. Sie ist gemäß der Standard-Netzwerksicherheitsgruppe zulässig. Es wird empfohlen, dass Sie diese IP-Adresse in lokalen Firewallrichtlinien zulassen. Die Kommunikation zwischen dieser speziellen IP-Adresse und den Ressourcen ist sicher, da nur die interne Azure-Plattform eine Nachricht von dieser IP-Adresse beziehen kann. Wenn diese Adresse gesperrt ist, kann es in verschiedenen Szenarien zu unerwartetem Verhalten kommen.

Zusätzlich können Sie erwarten, dass der Datenverkehr von der virtuellen öffentlichen IP-Adresse 168.63.129.16 zu dem Endpunkt fließt, der für einen [Load Balancer-Integritätstest](../load-balancer/load-balancer-custom-probe-overview.md) konfiguriert ist. In einem nicht virtuellen Netzwerkszenario wird der Integritätstest von einer privaten IP bezogen. 

## <a name="next-steps"></a>Nächste Schritte

- [Sicherheitsgruppen](security-overview.md)
- [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](manage-network-security-group.md)

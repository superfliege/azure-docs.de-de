---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: b7bbf1fb8ffc1f57a53682d3baf288077b144572
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664584"
---
### <a name="virtual-networks"></a>Virtuelle Netzwerke

|  |  |
|---------|---------|
| [Zulässige Application Gateway-SKUs](../articles/azure-policy/scripts/allowed-app-gate-sku.md) | Verlangt, dass Application Gateways eine zulässige SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Kein Netzwerkpeering in ein ER-Netzwerk](../articles/azure-policy/scripts/no-peering-er-net.md) | Verhindert, dass ein Netzwerkpeering einem Netzwerk in einer angegebenen Ressourcengruppe zugeordnet wird. Verwenden Sie diese Richtlinie, um eine Verbindung zu zentraler verwalteter Netzwerkinfrastruktur zu vermeiden. Sie geben den Namen der Ressourcengruppe an, um eine Zuordnung zu verhindern. |
| [Keine benutzerdefinierten Routentabellen](../articles/azure-policy/scripts/no-user-def-route-table.md)  |Verhindert, dass virtuelle Netzwerke mit einer benutzerdefinierten Routentabelle bereitgestellt werden. |
| [Netzwerksicherheitsgruppe X auf jedem Subnetz](../articles/azure-policy/scripts/nsg-on-subnet.md) | Verlangt, dass eine bestimmte Netzwerksicherheitsgruppe für jedes virtuelle Subnetz verwendet wird. Sie geben die ID der zu verwendenden Netzwerksicherheitsgruppe an. |
| [Verwenden eines zulässigen Subnetzes für VM-Netzwerkschnittstellen](../articles/azure-policy/scripts/use-approved-subnet-vm-nics.md) | Verlangt, dass Netzwerkschnittstellen ein zulässiges Subnetz verwenden. Sie geben die ID des zulässigen Subnetzes an. |
| [Verwenden eines zulässigen virtuellen Netzwerks für VM-Netzwerkschnittstellen](../articles/azure-policy/scripts/use-approved-vnet-vm-nics.md) | Verlangt, dass Netzwerkschnittstellen ein zulässiges virtuelles Netzwerk verwenden. Sie geben die ID des zulässigen virtuellen Netzwerks an. |
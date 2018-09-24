---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: df9c291136fc6a48effb08cd59eeb66486eb641c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003863"
---
### <a name="virtual-networks"></a>Virtuelle Netzwerke

|  |  |
|---------|---------|
| [Zulässige Application Gateway-SKUs](../articles/governance/policy/samples/allowed-app-gate-sku.md) | Verlangt, dass Application Gateways eine zulässige SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Kein Netzwerkpeering in ein ER-Netzwerk](../articles/governance/policy/samples/no-peering-er-net.md) | Verhindert, dass ein Netzwerkpeering einem Netzwerk in einer angegebenen Ressourcengruppe zugeordnet wird. Verwenden Sie diese Richtlinie, um eine Verbindung zu zentraler verwalteter Netzwerkinfrastruktur zu vermeiden. Sie geben den Namen der Ressourcengruppe an, um eine Zuordnung zu verhindern. |
| [Keine benutzerdefinierten Routentabellen](../articles/governance/policy/samples/no-user-def-route-table.md)  |Verhindert, dass virtuelle Netzwerke mit einer benutzerdefinierten Routentabelle bereitgestellt werden. |
| [Netzwerksicherheitsgruppe X auf jedem Subnetz](../articles/governance/policy/samples/nsg-on-subnet.md) | Verlangt, dass eine bestimmte Netzwerksicherheitsgruppe für jedes virtuelle Subnetz verwendet wird. Sie geben die ID der zu verwendenden Netzwerksicherheitsgruppe an. |
| [Verwenden eines zulässigen Subnetzes für VM-Netzwerkschnittstellen](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | Verlangt, dass Netzwerkschnittstellen ein zulässiges Subnetz verwenden. Sie geben die ID des zulässigen Subnetzes an. |
| [Verwenden eines zulässigen virtuellen Netzwerks für VM-Netzwerkschnittstellen](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | Verlangt, dass Netzwerkschnittstellen ein zulässiges virtuelles Netzwerk verwenden. Sie geben die ID des zulässigen virtuellen Netzwerks an. |
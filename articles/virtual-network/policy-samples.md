---
title: Beispiele für Richtlinienvorlagen | Microsoft-Dokumentation
description: Beispiele für Azure-Richtlinienvorlagen für virtuelle Netzwerke.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 05/02/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 2b8766a5353b015030872176e9032034afb7cb9d
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="azure-policy-sample-templates-for-virtual-network"></a>Beispiele für Azure-Richtlinienvorlagen für virtuelle Netzwerke

In der folgenden Tabelle sind Links zu [Azure Policy](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Beispielvorlagen enthalten. Diese Beispiele befinden sich im [Azure Policy-Beispielrepository](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Netzwerk**||
| [Netzwerksicherheitsgruppe X auf jeder Netzwerkschnittstelle](../azure-policy/scripts/nsg-on-nic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt, dass eine bestimmte Netzwerksicherheitsgruppe für jede virtuelle Netzwerkschnittstelle verwendet wird. Sie geben die ID der zu verwendenden Netzwerksicherheitsgruppe an. |
| [Netzwerksicherheitsgruppe X auf jedem Subnetz](../azure-policy/scripts/nsg-on-subnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt, dass eine bestimmte Netzwerksicherheitsgruppe für jedes virtuelle Subnetz verwendet wird. Sie geben die ID der zu verwendenden Netzwerksicherheitsgruppe an. |
| [Keine Routingtabelle](../azure-policy/scripts/no-user-def-route-table.md?toc=%2fazure%2fvirtual-network%2ftoc.json)  |Verhindert, dass virtuelle Netzwerke mit einer Routingtabelle bereitgestellt werden. |
| [Verwenden eines zulässigen Subnetzes für VM-Netzwerkschnittstellen](../azure-policy/scripts/use-approved-subnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt, dass Netzwerkschnittstellen ein zulässiges Subnetz verwenden. Sie geben die ID des zulässigen Subnetzes an. |
| [Verwenden eines zulässigen virtuellen Netzwerks für VM-Netzwerkschnittstellen](../azure-policy/scripts/use-approved-vnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt, dass Netzwerkschnittstellen ein zulässiges virtuelles Netzwerk verwenden. Sie geben die ID des zulässigen virtuellen Netzwerks an. |
|**Überwachung**||
| [Überwachen der Diagnoseeinstellung](../azure-policy/scripts/audit-diag-setting.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Überwacht, wenn Diagnoseeinstellungen für angegebene Ressourcentypen nicht aktiviert sind Sie geben ein Array von Ressourcentypen an, um zu überprüfen, ob die Diagnoseeinstellungen aktiviert sind. |
|**Namens- und Textkonventionen**||
| [Zulassen mehrerer Namensmuster](../azure-policy/scripts/allow-multiple-name-patterns.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Lassen Sie die Verwendung von einem der vielen Namensmuster für Ressourcen zu. |
| [Ähnliches Muster erfordern](../azure-policy/scripts/enforce-like-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Stellen Sie sicher, dass Ressourcennamen eine *ähnliche* Bedingung für ein Muster erfüllen. |
| [Übereinstimmungsmuster erfordern](../azure-policy/scripts/enforce-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Stellen Sie sicher, dass Ressourcennamen einem angegebenen Benennungsmuster entsprechen. |
| [Tagübereinstimmungsmuster erfordern](../azure-policy/scripts/enforce-tag-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Stellen Sie sicher, dass ein Tagwert einem Textmuster entspricht. |
|**Tags**||
| [Richtlinieninitiative zur Abrechnung von Tags](../azure-policy/scripts/billing-tags-policy-init.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt angegebene Tagwerte für die Kostenstelle und den Produktnamen. Verwendet integrierte Richtlinien, um erforderliche Tags anzuwenden und zu erzwingen. Sie geben die erforderlichen Werte für die Tags an.  |
| [Erzwingen eines Tags und dessen Werts für Ressourcengruppen](../azure-policy/scripts/enforce-tag-rg.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Verlangt einen Tag und einen Wert für eine Ressourcengruppe. Sie geben den erforderlichen Tagnamen und -wert an.  |
| [Erzwingen eines Tags und dessen zugehörigen Werts](../azure-policy/scripts/enforce-tag-val.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Erfordert einen angegebenen Tagnamen und-wert. Sie geben den zu erzwingenden Tagnamen und -wert an.  |
| [Tag und Standardwert anwenden](../azure-policy/scripts/apply-tag-def-val.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Fügt einen angegebenen Tagnamen und -wert an, wenn dieses Tag nicht bereitgestellt wird. Sie geben den anzuwendenden Tagnamen und -wert an.  |
|**Allgemein**||
| [Zulässige Speicherorte](../azure-policy/scripts/allowed-locs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Erfordert, dass alle Ressourcen für die zulässigen Speicherorte bereitgestellt werden. Sie geben ein Array von zulässigen Speicherorten an.  |
| [Zulässige Ressourcentypen](../azure-policy/scripts/allowed-res-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Stellt sicher, dass nur genehmigte Ressourcentypen bereitgestellt werden. Sie geben ein Array von Ressourcentypen an, die erlaubt sind.  |
| [Nicht zulässige Ressourcentypen](../azure-policy/scripts/not-allowed-res-type.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Untersagt die Bereitstellung der angegebenen Ressourcentypen. Sie geben ein Array der zu blockierenden Ressourcentypen an.  |
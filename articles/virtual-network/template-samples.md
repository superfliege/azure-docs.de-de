---
title: Azure Resource Manager-Vorlagenbeispiele für virtuelle Netzwerke | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu verschiedenen verfügbaren Azure Resource Manager-Vorlagen, mit denen Sie virtuelle Azure-Netzwerke bereitstellen können.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/13/2018
ms.author: jdial
ms.openlocfilehash: af0affc549afd8b63fe0d566fac198de054554c1
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
ms.locfileid: "31528986"
---
# <a name="azure-resource-manager-template-samples-for-virtual-network"></a>Azure Resource Manager-Vorlagenbeispiele für virtuelle Netzwerke

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagenbeispielen. Sie können Vorlagen über das [Portal](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), mithilfe der [Azure-Befehlszeilenschnittstelle](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder mithilfe von [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bereitstellen. Informationen zum Erstellen eigener Vorlagen finden Sie unter [Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-create-first-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sowie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json).


| | |
|----|----|
|[Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)| Erstellt ein virtuelles Netzwerk mit zwei Subnetzen.|
|[Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](https://github.com/Azure/azure-quickstart-templates/tree/master/201-userdefined-routes-appliance)| Erstellt ein virtuelles Netzwerk mit drei Subnetzen. Stellt in jedem der Subnetze einen virtuellen Computer bereit. Erstellt eine Routingtabelle mit Routen, um Datenverkehr aus einem Subnetz über den virtuellen Computer im dritten Subnetz an ein anderes Subnetz weiterzuleiten. Ordnet die Routingtabelle einem der Subnetze zu.|
|[Erstellen eines VNet-Dienstendpunkts für Azure Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-2subnets-service-endpoints-storage-integration)|Erstellt ein neues virtuelles Netzwerk mit zwei Subnetzen und jeweils eine Netzwerkschnittstelle in den Subnetzen. Aktiviert einen Dienstendpunkt für Azure Storage für eines der Subnetze und bindet ein neues Speicherkonto an dieses Subnetz.|
|[Verbinden zweier virtueller Netzwerke](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering)| Erstellt zwei virtuelle Netzwerke und ein Peering zwischen ihnen.|
|[Erstellen eines virtuellen Computers mit mehreren IP-Adressen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-multiple-ipconfig)| Erstellt einen virtuellen Windows- oder Linux-Computer mit mehreren IP-Adressen.|

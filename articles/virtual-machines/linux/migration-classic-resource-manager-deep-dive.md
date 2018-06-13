---
title: Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager | Microsoft Docs
description: Dieser Artikel bietet ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 29267453-f894-4180-bb67-dce2a0e062bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: b7fccf0655502d2bd023e7ac7f950c8be72e0076
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907373"
---
# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-to-azure-resource-manager"></a>Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager

Wir betrachten nun genau die Migration vom klassischen Azure-Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell. Ressourcen betrachten wir auf Ressourcen- und Featureebene, damit Sie verstehen, wie die Azure-Plattform Ressourcen zwischen den beiden Bereitstellungsmodellen migriert. Weitere Informationen finden Sie im Artikel zur Dienstankündigung: [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-migration-deep-dive](../../../includes/virtual-machines-common-classic-resource-manager-migration-deep-dive.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planen der Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Befehlszeilenschnittstelle](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Communitytools zur Unterstützung beim Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Überprüfen der häufigsten Fehler bei der Migration](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Antworten auf die am häufigsten gestellten Fragen zum Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

---
title: "Technische deep Dive in die Plattform unterstützt Migration von klassischen zum Azure-Ressourcen-Manager | Microsoft Docs"
description: "Dieser Artikel bietet eine technische deep Dive in die Plattform unterstützt Migration der Ressourcen von klassischen zum Azure-Ressourcen-Manager"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 29267453-f894-4180-bb67-dce2a0e062bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 557a61f266c494cb6b8003cff945fa1a14348898
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-to-azure-resource-manager"></a>Technische deep Dive in die Plattform unterstützt Migration von klassischen zum Azure-Ressourcen-Manager

Werfen wir einen tief greifende zum Migrieren von dem klassischen Azure-Bereitstellungsmodell zu den Azure Resource Manager-Bereitstellungsmodell. Betrachten wir die Ressourcen auf einer Ebene Ressource und die Funktion, um besser zu verstehen, wie die Azure-Plattform Ressourcen zwischen den zwei Bereitstellungsmodelle migriert. Weitere Informationen finden Sie im Dienst-Ankündigung-Artikel: [Plattform unterstützt Migration von IaaS-Ressourcen von klassischen zum Azure-Ressourcen-Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-migration-deep-dive](../../../includes/virtual-machines-common-classic-resource-manager-migration-deep-dive.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die Plattform unterstützt Migration von IaaS-Ressourcen von klassischen zum Azure-Ressourcen-Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planen der Migration von IaaS-Ressourcen von klassischen zum Azure-Ressourcen-Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Verwenden von PowerShell zur Migration von IaaS-Ressourcen von klassischen zum Azure-Ressourcen-Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Verwenden Sie CLI zur Migration von IaaS-Ressourcen von klassischen zum Azure-Ressourcen-Manager](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Community-Tools für die Unterstützung bei der Migration von IaaS-Ressourcen von klassischen zum Azure-Ressourcen-Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Überprüfen Sie die häufigsten Fehler bei der migration](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Überprüfen Sie die am häufigsten gestellten Fragen zur Migration von IaaS-Ressourcen von klassischen zum Azure-Ressourcen-Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

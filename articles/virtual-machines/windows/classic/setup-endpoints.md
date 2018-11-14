---
title: Einrichten von Endpunkten auf einem klassischen virtuellen Windows-Computer | Microsoft Docs
description: Erfahren Sie, wie Endpunkte für einen klassischen virtuellen Windows-Computer im Azure-Portal eingerichtet werden, um die Kommunikation mit einem virtuellen Windows-Computer in Azure zu ermöglichen.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/23/2018
ms.author: cynthn
ms.openlocfilehash: 373003eb8be0482ed96d7d11ecd879237f69b47a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50957164"
---
# <a name="set-up-endpoints-on-a-windows-virtual-machine-by-using-the-classic-deployment-model"></a>Einrichten von Endpunkten auf einer Windows-VM mit dem klassischen Bereitstellungsmodell
Virtuelle Windows-Computer (VMs), die Sie mit dem klassischen Bereitstellungsmodell in Azure erstellen, können automatisch über einen privaten Netzwerkkanal mit anderen VMs im gleichen Clouddienst oder virtuellen Netzwerk kommunizieren. Computer im Internet oder in anderen virtuellen Netzwerken benötigen jedoch Endpunkte, um den eingehenden Netzwerkdatenverkehr auf eine VM weiterzuleiten. 

Sie können Endpunkte auch auf [virtuellen Linux-Computern](../../linux/classic/setup-endpoints.md)einrichten.

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../../../resource-manager-deployment-model.md). Dieser Artikel gilt für das klassische Bereitstellungsmodell. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.  
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Im **Resource Manager**-Bereitstellungsmodell werden Endpunkte mithilfe von **Netzwerksicherheitsgruppen (NSGs)** konfiguriert. Weitere Informationen finden Sie unter [Ermöglichen des externen Zugriffs auf eine VM über das Azure-Portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Beim Erstellen einer Windows-VM im Azure-Portal werden allgemeine Endpunkte, z.B. für Remotedesktop und Windows PowerShell-Remoting, in der Regel automatisch erstellt. Sie können später bei Bedarf weitere Endpunkte konfigurieren.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Nächste Schritte
* Ein Azure PowerShell-Cmdlet zum Einrichten eines VM-Endpunkts finden Sie unter [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Informationen dazu, wie Sie ein Azure PowerShell-Cmdlet zum Verwalten einer ACL für einen Endpunkt verwenden, finden Sie unter [Verwalten von Zugriffssteuerungslisten (ACLs) für Endpunkte mithilfe von PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).
* Wenn Sie einen virtuellen Computer im Resource Manager-Bereitstellungsmodell erstellt haben, können Sie Azure PowerShell verwenden, um [Netzwerksicherheitsgruppen](../../../virtual-network/tutorial-filter-network-traffic.md) zum Steuern des Datenverkehrs an den virtuellen Computer zu erstellen.

---
title: Azure PowerShell-Beispielskript – Ändern des RDP-Portbereichs | Microsoft-Dokumentation
description: Azure PowerShell-Beispielskript – ändert den RDP-Portbereich eines bereitgestellten Clusters
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 871fe502a40b4c22f3e57a2f357e940c7f659314
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663926"
---
# <a name="update-the-rdp-port-range-values"></a>Aktualisieren der Werte des RDP-Portbereichs

Dieses Beispielskript ändert die Werte des RDP-Portbereichs auf den Clusterknoten-VMs, nachdem der Cluster bereitgestellt wurde.  Azure PowerShell wird verwendet, damit die zugrunde liegenden VMs nicht heruntergefahren werden.  Das Skript ruft die Ressource `Microsoft.Network/loadBalancers` in der Ressourcengruppe des Clusters ab und aktualisiert die Werte `inboundNatPools.frontendPortRangeStart` und `inboundNatPools.frontendPortRangeEnd`. Passen Sie die Parameter nach Bedarf an.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Ruft die Ressource `Microsoft.Network/loadBalancers` ab |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|Aktualisiert die Ressource `Microsoft.Network/loadBalancers`|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).

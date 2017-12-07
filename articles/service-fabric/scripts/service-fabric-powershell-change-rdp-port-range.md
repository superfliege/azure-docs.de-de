---
title: "Azure PowerShell-Beispielskript – Ändern des RDP-Portbereichs | Microsoft-Dokumentation"
description: "Azure PowerShell-Beispielskript – ändert den RDP-Portbereich eines bereitgestellten Clusters"
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9089d071c69830323a94140a5ebb4e9e865499c6
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-rdp-port-range-values"></a>Aktualisieren der Werte des RDP-Portbereichs

Dieses Beispielskript ändert die Werte des RDP-Portbereichs auf den Clusterknoten-VMs, nachdem der Cluster bereitgestellt wurde.  Azure PowerShell wird verwendet, damit die zugrunde liegenden VMs nicht heruntergefahren werden.  Das Skript ruft die Ressource `Microsoft.Network/loadBalancers` in der Ressourcengruppe des Clusters ab und aktualisiert die Werte `inboundNatPools.frontendPortRangeStart` und `inboundNatPools.frontendPortRangeEnd`. Passen Sie die Parameter nach Bedarf an.

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Ruft die Ressource `Microsoft.Network/loadBalancers` ab |
|[Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource)|Aktualisiert die Ressource `Microsoft.Network/loadBalancers`|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).

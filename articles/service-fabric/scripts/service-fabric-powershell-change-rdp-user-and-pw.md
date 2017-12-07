---
title: "Azure PowerShell-Beispielskript – Aktualisieren des RDP-Benutzernamens und des zugehörigen Kennworts | Microsoft-Dokumentation"
description: "Azure PowerShell-Beispielskript – Aktualisieren des RDP-Benutzernamens und des zugehörigen Kennworts für alle Service Fabric-Clusterknoten eines bestimmten Knotentyps"
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
ms.date: 11/17/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 3b97cee11c9a85cbd60a05bdbdcd010a0f0a106f
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Aktualisieren des Administratorbenutzernamens und des zugehörigen Kennworts für die VMs in einem Cluster

Jeder Knotentyp in einem Service Fabric-Cluster ist eine VM-Skalierungsgruppe. Dieses Beispielskript aktualisiert den Administratorbenutzernamen und das zugehörige Kennwort für die Cluster-VMs in einen bestimmten Knotentyp.  Fügen Sie der Skalierungsgruppe die VMAccessAgent-Erweiterung hinzu, da es sich bei dem Administratorkennwort nicht um eine änderbare Eigenschaft der Skalierungsgruppe handelt.  Änderungen am Benutzernamen und am Kennwort betreffen alle Knoten in der Skalierungsgruppe. Passen Sie die Parameter nach Bedarf an.

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript benutzt die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Dient zum Abrufen der Eigenschaften eines Clusterknotentyps (einer VM-Skalierungsgruppe)   |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)| Fügt der VM-Skalierungsgruppe eine Erweiterung hinzu|
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)|Aktualisiert den Zustand einer VM-Skalierungsgruppe in den Zustand eines lokalen VMSS-Objekts|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).

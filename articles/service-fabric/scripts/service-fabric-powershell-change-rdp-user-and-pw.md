---
title: Azure PowerShell-Beispielskript – Aktualisieren des RDP-Benutzernamens und des zugehörigen Kennworts | Microsoft-Dokumentation
description: Azure PowerShell-Beispielskript – Aktualisieren des RDP-Benutzernamens und des zugehörigen Kennworts für alle Service Fabric-Clusterknoten eines bestimmten Knotentyps
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
ms.openlocfilehash: 1528ce03f548c70d20d8f7af0238019305f01f2e
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660815"
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Aktualisieren des Administratorbenutzernamens und des zugehörigen Kennworts für die VMs in einem Cluster

Jeder [Knotentyp](../service-fabric-cluster-nodetypes.md) in einem Service Fabric-Cluster ist eine VM-Skalierungsgruppe. Dieses Beispielskript aktualisiert den Administratorbenutzernamen und das zugehörige Kennwort für die Cluster-VMs in einen bestimmten Knotentyp.  Fügen Sie der Skalierungsgruppe die VMAccessAgent-Erweiterung hinzu, da es sich bei dem Administratorkennwort nicht um eine änderbare Eigenschaft der Skalierungsgruppe handelt.  Änderungen am Benutzernamen und am Kennwort betreffen alle Knoten in der Skalierungsgruppe. Passen Sie die Parameter nach Bedarf an.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Dient zum Abrufen der Eigenschaften eines Clusterknotentyps (einer VM-Skalierungsgruppe)   |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension)| Fügt der VM-Skalierungsgruppe eine Erweiterung hinzu|
| [Update-AzVmss](/powershell/module/az.compute/update-azvmss)|Aktualisiert den Zustand einer VM-Skalierungsgruppe in den Zustand eines lokalen VMSS-Objekts|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).

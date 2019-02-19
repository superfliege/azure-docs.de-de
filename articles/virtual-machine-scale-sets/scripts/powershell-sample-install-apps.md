---
title: 'Azure PowerShell-Beispiele: Installieren von Apps | Microsoft-Dokumentation'
description: Azure PowerShell-Beispiele
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: a2887eea0e4a43e21c776f7d25a1ee55971d80a1
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981603"
---
# <a name="install-applications-into-a-virtual-machine-scale-set-with-powershell"></a>Installieren von Anwendungen in einer VM-Skalierungsgruppe mit PowerShell
Dieses Skript erstellt eine VM-Skalierungsgruppe unter Windows Server 2016 und installiert eine einfache Webanwendung unter Verwendung der benutzerdefinierten Skripterweiterung. Nach dem Ausführen des Skripts können Sie über einen Webbrowser auf die Web-App zugreifen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/install-apps/install-apps.ps1 "Install apps into a scale set")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts
Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzVmss](/powershell/module/az.compute/new-azvmss) | Erstellt die VM-Skalierungsgruppe und alle unterstützenden Ressourcen, einschließlich des virtuellen Netzwerks, des Lastenausgleichs und der NAT-Regeln. |
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Ruft Informationen zu einer VM-Skalierungsgruppe ab. |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) | Fügt eine VM-Erweiterung für das benutzerdefinierte Skript hinzu, um eine einfache Webanwendung zu installieren. |
| [Update-AzVmss](/powershell/module/az.compute/update-azvmss) | Aktualisiert das Modell der VM-Skalierungsgruppe, um die VM-Erweiterung anzuwenden. |
| [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) | Ruft Informationen zur zugewiesenen öffentlichen IP-Adresse ab, die vom Load Balancer verwendet wird. |
|  [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für VM-Skalierungsgruppen finden Sie in der [Dokumentation zu Azure-VM-Skalierungsgruppen](../powershell-samples.md).

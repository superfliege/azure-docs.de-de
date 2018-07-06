---
title: Azure PowerShell-Skriptbeispiel – IIS | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – IIS
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 657114463ffea0456e26ae133c6dd6882b6a29d0
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37095715"
---
# <a name="create-an-iis-vm-with-powershell"></a>Erstellen einer IIS-VM mit PowerShell

Dieses Skript erstellt einen virtuellen Azure-Computer mit Windows Server 2016 und verwendet dann die benutzerdefinierte Azure-VM-Skripterweiterung, um Internetinformationsdienste (IIS) zu installieren. Nachdem das Skript ausgeführt wurde, können Sie die standardmäßige IIS-Website über die öffentliche IP-Adresse des virtuellen Computers aufrufen.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der Netzwerksicherheitsgruppe. Mit diesem Befehl werden außerdem Port 80 geöffnet und die Administratoranmeldeinformationen festgelegt. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Hinzufügen einer VM-Erweiterung zum virtuellen Computer. In diesem Beispiel wird die benutzerdefinierte Skripterweiterung zum Installieren von IIS verwendet. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

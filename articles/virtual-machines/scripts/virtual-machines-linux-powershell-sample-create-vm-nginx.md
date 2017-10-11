---
title: Azure PowerShell-Skript-Beispiel - NGINX | Microsoft Docs
description: Azure PowerShell-Skript-Beispiel - NGINX
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e4b2a02d6019d7610fc1dce95d94efa764c6f04c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-nginx-vm-with-powershell"></a>Erstellen Sie einen NGINX-VM mit PowerShell

Dieses Skript erstellt einen virtuellen Azure-Computer und verwendet dann die Azure Virtual Machine benutzerdefinierte Skripterweiterung NGINX zu installieren. Nach dem Ausführen des Skripts können Sie eine Demo-Website auf die öffentliche IP-Adresse des virtuellen Computers zugreifen.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[Main](../../../powershell_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.ps1 "VM NGINX erstellen")]

## <a name="clean-up-deployment"></a>Bereitstellung bereinigen 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erklärung der Skripts

Dieses Skript verwendet die folgenden Befehle, um die Bereitstellung zu erstellen. Jedes Element in der Tabelle ist mit dem Befehl spezifische Dokumentation verknüpft werden soll.

| Befehl | Hinweise |
|---|---|
| [Neue AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [Neue AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Erstellt eine Subnetzkonfiguration. Diese Konfiguration wird mit der Erstellungsprozess für den virtuellen Netzwerk verwendet. |
| [Neue AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Erstellt ein virtuelles Netzwerk an. |
| [Neue AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Erstellt eine öffentliche IP-Adresse an. |
| [Neue AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Erstellt eine Netzwerkkonfiguration Security Group Regel an. Diese Konfiguration wird verwendet, um eine NSG-Regel zu erstellen, wenn die NSG erstellt wird. |
| [Neue AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Erstellt eine Netzwerksicherheitsgruppe an. |
| [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | Ruft die Subnetzinformationen ab. Diese Informationen werden verwendet, wenn eine Netzwerkschnittstelle erstellen. |
| [Neue AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Erstellt eine Netzwerkschnittstelle. |
| [Neue AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Erstellt eine VM-Konfiguration. Diese Konfiguration umfasst Informationen wie VM-Namen, Betriebssystem und administrativen Anmeldeinformationen an. Die Konfiguration wird während der Erstellung eines virtuellen Computers verwendet. |
| [Neue AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Erstellen Sie einen virtuellen Computer. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Hinzufügen einer Erweiterungs zum virtuellen Computer an. In diesem Beispiel wird die CustomScript-Erweiterung installieren NGINX verwendet. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Entfernt eine Ressourcengruppe und alle enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell-Modul, finden Sie unter [Azure PowerShell-Dokumentation](/powershell/azure/overview).

PowerShell-Skriptbeispiele zusätzlicher virtueller Computer finden Sie in der [Azure Linux-VM-Dokumentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

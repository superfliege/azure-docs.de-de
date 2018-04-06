---
title: Azure PowerShell-Skriptbeispiel – Filtern des VM-Netzwerkdatenverkehrs | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Filtern ein- und ausgehenden VM-Netzwerkdatenverkehrs
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 13089e0194b62a1f2f7a7c592d3ff21af8495dd1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filtern ein- und ausgehenden VM-Netzwerkverkehrs

In diesem Skriptbeispiel wird ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetz erstellt. Eingehender Netzwerkdatenverkehr zum Front-End-Subnetz ist auf HTTP und HTTPS beschränkt, während ausgehender Datenverkehr zum Internet aus dem Back-End-Subnetz nicht zulässig ist. Nach dem Ausführen des Skripts besitzen Sie einen virtuellen Computer mit zwei NICs. Jede NIC kann mit einem anderen Subnetz verbunden sein.

Sie können das Skript über Azure [Cloud Shell](https://shell.azure.com/powershell) oder eine lokale PowerShell-Installation ausführen. Wenn Sie PowerShell lokal verwenden, müssen Sie für dieses Skript mindestens Version 5.4.1 des AzureRM PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Erstellt ein Subnetzkonfigurationsobjekt |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Erstellt Sicherheitsregeln, die einer Netzwerksicherheitsgruppe zugewiesen werden sollen |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Erstellt NSG-Regeln, die bestimmte Ports bei bestimmten Subnetzen zulassen oder blockieren. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Ordnet NSGs Subnetzen zu. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Erstellt virtuelle Netzwerkschnittstellen und fügt sie an das Front-End- und Back-End-Subnetz des virtuellen Netzwerks an. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Erstellt eine VM-Konfiguration. Diese Konfiguration umfasst Informationen wie VM-Name, Betriebssystem und Administratoranmeldeinformationen. Die Konfiguration wird während der VM-Erstellung verwendet. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Erstellen Sie eine VM. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für virtuelle Netzwerke finden Sie unter [PowerShell-Beispiele für virtuelle Netzwerke](../powershell-samples.md).
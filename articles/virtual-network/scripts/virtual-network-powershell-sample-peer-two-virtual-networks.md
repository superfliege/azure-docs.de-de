---
title: Azure PowerShell-Skriptbeispiel – Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken
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
ms.openlocfilehash: c0efdf759a0bdb87de4dc8ff9566a8e817503c5e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="peer-two-virtual-networks"></a>Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken

Dieses Skript erstellt zwei virtuelle Netzwerke in der gleichen Region und stellt über das Azure-Netzwerk eine Verbindung zwischen ihnen her. Nach dem Ausführen des Skripts erstellen Sie ein Peering zwischen zwei virtuellen Netzwerken.

Sie können das Skript über Azure [Cloud Shell](https://shell.azure.com/powershell) oder eine lokale PowerShell-Installation ausführen. Wenn Sie PowerShell lokal verwenden, müssen Sie für dieses Skript mindestens Version 5.4.1 des AzureRM PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. | 
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)| Erstellt ein virtuelles Azure-Netzwerk und ein Subnetz. |
| [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | Erstellt ein Peering zwischen virtuellen Netzwerken.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für virtuelle Netzwerke finden Sie unter [PowerShell-Beispiele für virtuelle Netzwerke](../powershell-samples.md).
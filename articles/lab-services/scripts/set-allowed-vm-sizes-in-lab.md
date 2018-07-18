---
title: 'PowerShell-Skript: Festlegen zulässiger VM-Größen in Azure Lab Services | Microsoft-Dokumentation'
description: Dieses PowerShell-Skript legt zulässige Größen virtueller Computer (VMs) in Azure Lab Services fest.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: spelluru
ms.openlocfilehash: 159f175e7bb27b2d89001e1eba737c67adb89e50
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638142"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Festlegen zulässiger VM-Größen in Azure Lab Services mithilfe von PowerShell

Dieses PowerShell-Beispielskript legt zulässige Größen virtueller Computer (VMs) in Azure Lab Services fest.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Voraussetzungen
* **Ein Lab**. Das Skript erfordert ein vorhandenes Lab. 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Find-AzureRmResource](/module/azurerm.resources/find-azurermresource) | Dient zum Suchen nach Ressourcen basierend auf den angegebenen Parametern. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Dient zum Abrufen von Ressourcen. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Dient zum Ändern einer Ressource. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Dient zum Erstellen einer Ressource. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Lab Services finden Sie unter [Azure Lab Services – Beispiele für PowerShell](../samples-powershell.md).
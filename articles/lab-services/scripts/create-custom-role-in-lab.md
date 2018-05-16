---
title: 'PowerShell-Skript: Erstellen einer benutzerdefinierte Rolle in einem benutzerdefinierten Lab in Azure Lab Services | Microsoft-Dokumentation'
description: Dieses PowerShell-Skript fügt einen externen Benutzer zu einem benutzerdefinierten Lab in Azure Lab Services hinzu.
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
ms.openlocfilehash: df91c9f842d338e1725fec2734129f2f1f3d3721
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-create-a-custom-role-in-a-custom-lab-in-azure-lab-services"></a>Erstellen einer benutzerdefinierte Rolle in einem benutzerdefinierten Lab in Azure Lab Services mithilfe von PowerShell

Dieses PowerShell-Beispielskript erstellt eine benutzerdefinierte Rolle für ein benutzerdefiniertes Lab in Azure Lab Services. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Voraussetzungen
* **Ein benutzerdefiniertes Lab**. Das Skript erfordert ein vorhandenes benutzerdefiniertes Lab. 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a custom lab")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) | Ruft die Vorgänge für einen Azure-Ressourcenanbieter ab, die mit Azure RBAC abgesichert werden können. |
| [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) | Dient zum Auflisten aller Azure RBAC-Rollen, die für die Zuweisung verfügbar sind. |
| [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) | Dient zum Erstellen einer benutzerdefinierten Rolle. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Lab Services finden Sie unter [Azure Lab Services – Beispiele für PowerShell](../samples-powershell.md).
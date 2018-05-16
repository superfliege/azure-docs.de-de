---
title: 'PowerShell-Skript: Hinzufügen eines externen Benutzers zu einem benutzerdefinierten Lab in Azure Lab Services | Microsoft-Dokumentation'
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
ms.openlocfilehash: b089067a889f0ffd3b317fcc3f0784d176473b91
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-add-an-external-user-to-a-custom-lab"></a>Hinzufügen eines externen Benutzers zu einem benutzerdefinierten Lab mithilfe von PowerShell

Dieses PowerShell-Beispielskript fügt einen externen Benutzer zu einem benutzerdefinierten Lab in Azure Lab Services hinzu. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Voraussetzungen
* **Ein benutzerdefiniertes Lab**. Das Skript erfordert ein vorhandenes benutzerdefiniertes Lab. 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a custom lab")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzureRmADUser](/powershell/module/azurerm.resources/get-azurermaduser) | Wiederholt das Benutzerobjekt aus Azure Active Directory. |
| [New-AzureRmRoleAssignment](/module/azurerm.resources/new-azurermroleassignment) | Weist dem angegebenen Dienstprinzipal die angegebene Rolle im angegebenen Bereich zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Lab Services finden Sie unter [Azure PowerShell samples for Azure Lab Services](../samples-powershell.md) (Azure PowerShell-Beispiele für Azure Lab Services).
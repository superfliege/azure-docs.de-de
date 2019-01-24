---
title: Azure PowerShell-Skriptbeispiel – Hinzufügen eines Benutzers | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Hinzufügen eines Benutzers
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
ms.date: 11/16/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 23395ed545042c2528f782d98b5182132d25980f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438540"
---
# <a name="add-a-user"></a>Hinzufügen eines Benutzers

Dieses Beispielskript erstellt einen Benutzer in API Management und ruft einen Abonnementschlüssel ab.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 3.6 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/api-management/add-user-and-get-subscription-key/add_a_user_and_get_a_subscriptionKey.ps1 "Add a user")]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle zugehörigen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/overview).

Weitere Azure PowerShell-Beispiele für Azure API Management finden Sie in den [PowerShell-Beispielen](../powershell-samples.md).

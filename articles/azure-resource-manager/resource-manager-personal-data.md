---
title: Persönliche Daten und Azure Resource Manager | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie persönliche Daten verwalten, die mit Azure Resource Manager-Vorgängen verknüpft sind.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: 5c176dd185a9d2a245045e9c954279506d713e5f
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Verwalten von mit Azure Resource Manager verknüpften persönlichen Daten

Um vertrauliche Informationen zu schützen, löschen Sie alle persönlichen Daten, die Sie in Bereitstellungen, Ressourcengruppen oder Tags angegeben haben. Mithilfe der Vorgänge in Azure Resource Manager können Sie persönliche Daten verwalten, die Sie in Bereitstellungen, Ressourcengruppen oder Tags bereitgestellt haben.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Löschen von persönlichen Daten im Bereitstellungsverlauf

Für Bereitstellungen werden von Resource Manager Parameterwerte und Statusmeldungen im Bereitstellungsverlauf gespeichert. Diese Werte bleiben enthalten, bis Sie die Bereitstellung aus dem Verlauf löschen. Um festzustellen, ob Sie persönliche Daten für diese Werte angegeben haben, listen Sie die Bereitstellungen auf. Wenn Sie persönliche Daten gefunden haben, löschen Sie die Bereitstellungen aus dem Verlauf.

Verwenden Sie diese Referenzen als Hilfe, um **Bereitstellungen** im Verlauf aufzulisten:

* [List By Resource Group](/rest/api/resources/deployments/listbyresourcegroup) (Nach Ressourcengruppe gruppieren)
* [Get-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Get-AzureRmResourceGroupDeployment)
* [az group deployment list](/cli/azure/group/deployment#az-group-deployment-list)

Verwenden Sie diese Referenzen als Hilfe, um **Bereitstellungen** aus dem Verlauf zu löschen:

* [Delete](/rest/api/resources/deployments/delete) (Löschen)
* [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroupDeployment)
* [az group deployment delete](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Löschen von persönlichen Daten in Ressourcengruppennamen

Der Name der Ressourcengruppe wird gespeichert, bis Sie die Ressourcengruppe löschen. Um festzustellen, ob Sie persönliche Daten in den Namen angegeben haben, listen Sie die Ressourcengruppen auf. Wenn Sie persönliche Daten gefunden haben, [verschieben Sie die Ressourcen](resource-group-move-resources.md) in eine neue Ressourcengruppe und löschen die Ressourcengruppe mit persönlichen Daten im Namen.

Verwenden Sie diese Referenzen als Hilfe, um **Ressourcengruppen** aufzulisten:

* [List](/rest/api/resources/resourcegroups/list) (Auflisten)
* [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/Get-AzureRmResourceGroup)
* [az group list](/cli/azure/group#az-group-list)

Verwenden Sie diese Referenzen als Hilfe, um **Ressourcengruppen** zu löschen:

* [Delete](/rest/api/resources/resourcegroups/delete) (Löschen)
* [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Löschen von persönlichen Daten in Tags

Tagnamen und -werte werden gespeichert, bis Sie das Tag löschen oder ändern. Um festzustellen, ob Sie persönliche Daten in den Tags angegeben haben, listen Sie die Tags auf. Wenn Sie persönliche Daten gefunden haben, löschen Sie die Tags.

Verwenden Sie diese Referenzen als Hilfe, um **Tags** aufzulisten:

* [List](/rest/api/resources/tags/list) (Auflisten)
* [Get-AzureRmTag](/powershell/module/azurerm.tags/get-azurermtag)
* [az tag list](/cli/azure/tag#az-tag-list)

Verwenden Sie diese Referenzen als Hilfe, um **Tags** zu löschen:

* [Delete](/rest/api/resources/tags/delete) (Löschen)
* [Remove-AzureRmTag](/powershell/module/azurerm.tags/remove-azurermtag)
* [az tag delete](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Nächste Schritte
* Einen Überblick über Azure Resource Manager erhalten Sie in der [Übersicht über Azure Resource Manager](resource-group-overview.md).
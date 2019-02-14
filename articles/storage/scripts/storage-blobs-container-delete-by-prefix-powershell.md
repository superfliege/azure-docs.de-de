---
title: Azure PowerShell-Skriptbeispiel – Löschen von Containern nach Präfix | Microsoft-Dokumentation
description: Löschen Sie Azure Storage Blob-Container basierend auf einem Präfix des Containernamens.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: e0c41ed1fc2cdb7559d36a7056ebb7b9a6ecc186
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56242769"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Löschen von Containern basierend auf dem Präfix des Containernamens

Dieses Skript löscht Container in Azure Blob Storage basierend auf einem Präfix im Containernamen.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.ps1 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die verbleibenden Container sowie alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name containerdeletetestrg
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um basierend auf dem Präfix des Containernamens Container löschen. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) | Ruft ein angegebenes Speicherkonto oder alle Speicherkonten in einer Ressourcengruppe oder im Abonnement ab. |
| [Get-AzStorageContainer](/powershell/module/az.storage/Get-AzStorageContainer) | Listet die Speichercontainer auf, die einem Speicherkonto zugeordnet sind. |
| [Remove-AzStorageContainer](/powershell/module/az.storage/Remove-AzStorageContainer) | Entfernt den angegebenen Speichercontainer. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für Speicher finden Sie in den [PowerShell-Beispielen für Azure Blob Storage](../blobs/storage-samples-blobs-powershell.md).

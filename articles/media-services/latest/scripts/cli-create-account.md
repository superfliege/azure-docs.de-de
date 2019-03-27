---
title: 'Erstellen eines Azure Media Services-Kontos: Azure CLI | Microsoft-Dokumentation'
description: Verwenden Sie ein Azure CLI-Skript, um ein Azure Media Services-Konto zu erstellen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/25/2019
ms.author: juliako
ms.openlocfilehash: 7e668852346de1b48bd34658dea001435ba8a625
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57842211"
---
# <a name="cli-example-create-an-azure-media-services-account"></a>CLI-Beispiel: Erstellen eines Azure Media Services-Kontos

Das Azure CLI-Skript in diesem Thema zeigt, wie ein Azure Media Services-Konto erstellt wird. 

Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Es wird empfohlen, Speicherkonten zu verwenden, die sich an demselben Ort wie das Media Services-Konto befinden.

[!INCLUDE [media-services-cli-instructions.md](../../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/media-services-create-account/Create-Account.sh "Create Account")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellt ein Speicherkonto. |
| [az ams account create](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest#az-ams-account-create) | Erstellt ein Media Services-Konto. |
| [az ams account sp create](https://docs.microsoft.com/cli/azure/ams/account/sp?view=azure-cli-latest#az-ams-account-sp-create) | Erstellt einen Dienstprinzipal mit Kennwort und konfiguriert dessen Zugriff auf ein Azure Media Services-Konto. 
| [az group delete](/cli/azure/group#az-group-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).

---
title: Erstellen einer Funktion in Azure, die über Azure DevOps bereitgestellt wird | Microsoft-Dokumentation
description: Erstellen einer Funktions-App und Bereitstellen von Funktionscode über Azure DevOps
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 07/03/2018
ms.topic: sample
ms.service: azure-functions
ms.custom: mvc
ms.openlocfilehash: 680115f64b16059c415d295cf7c6a15301de5a3f
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166099"
---
# <a name="create-a-function-app-and-deploy-function-code-from-azure-devops"></a>Erstellen einer Funktions-App und Bereitstellen von Funktionscode über Azure DevOps

In diesem Thema wird gezeigt, wie sich mit Azure Functions eine [serverlose](https://azure.microsoft.com/solutions/serverless/) Funktions-App über den [Verbrauchsplan](../functions-scale.md#consumption-plan) erstellen lässt. Die Funktions-App, die als Container für Ihre Funktionen fungiert, wird aus einem Azure DevOps-Repository ständig bereitgestellt. 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

Um dieses Thema durchzuarbeiten, benötigen Sie Folgendes:

* Ein Azure DevOps-Repository, das Ihr Funktions-App-Projekt enthält und für das Sie über Administratorberechtigungen verfügen.
* Ein [persönliches Zugriffstoken (PAT)](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate), um auf Ihr Azure DevOps-Repository zuzugreifen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Azure CLI dagegen lokal verwenden, müssen Sie Version 2.0 oder höher verwenden. Um die Azure CLI-Version zu ermitteln, führen Sie `az --version` aus. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript

In diesem Beispiel wird eine Azure-Funktions-App erstellt und Funktionscode über Azure DevOps bereitgestellt.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein Speicherkonto, eine Funktions-App und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Erstellt das für die Funktions-App erforderliche Speicherkonto. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktions-App im serverlosen [Verbrauchsplan](../functions-scale.md#consumption-plan). |
| [az functionapp deployment source config](https://docs.microsoft.com/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config) | Ordnet eine Funktionen-App einem Git- oder Mercurial-Repository zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

---
title: Erstellen einer Funktion in Azure, die aus Visual Studio Team Services bereitgestellt wird | Microsoft-Dokumentation
description: Erstellen einer Funktionen-App und Bereitstellen von Funktionscode über Visual Studio Team Services
services: functions
keywords: ''
author: syntaxc4
ms.author: glenga
ms.date: 07/03/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 1b54cfebd3ae36fc8025aeb4ea9c91d336bc5343
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988951"
---
# <a name="create-a-function-app-and-deploy-function-code-from-visual-studio-team-services"></a>Erstellen einer Funktions-App und Bereitstellen von Funktionscode über Visual Studio Team Services

In diesem Thema wird gezeigt, wie sich mit Azure Functions eine [serverlose](https://azure.microsoft.com/overview/serverless-computing/) Funktions-App über den [Verbrauchsplan](../functions-scale.md#consumption-plan) erstellen lässt. Die Funktions-App, die als Container für Ihre Funktionen fungiert, wird ständig aus einem VSTS-Repository (Visual Studio Team Services) bereitgestellt. 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

Um dieses Thema durchzuarbeiten, benötigen Sie Folgendes:

* Ein VSTS-Repository, das Ihr Funktions-App-Projekt enthält und für das Sie Administratorberechtigungen haben.
* Ein [persönliches Zugriffstoken (PAT)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate), um auf Ihr VSTS-Repository zuzugreifen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Azure CLI dagegen lokal verwenden, müssen Sie Version 2.0 oder höher verwenden. Um die Azure CLI-Version zu ermitteln, führen Sie `az --version` aus. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel erstellt eine Azure-Funktionen-App und stellt Funktionscode über Visual Studio Team Services bereit.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein Speicherkonto, eine Funktions-App und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Erstellt das für die Funktionen-App erforderliche Speicherkonto. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktions-App im serverlosen [Verbrauchsplan](../functions-scale.md#consumption-plan). |
| [az functionapp deployment source config](https://docs.microsoft.com/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config) | Ordnet eine Funktionen-App einem Git- oder Mercurial-Repository zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

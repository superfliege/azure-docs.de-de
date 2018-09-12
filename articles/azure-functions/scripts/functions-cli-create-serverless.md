---
title: Azure CLI-Skriptbeispiel – Erstellen einer Funktionen-App für die serverlose Ausführung | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Erstellen einer Funktionen-App für die serverlose Ausführung
services: functions
documentationcenter: functions
author: ggailey777
manager: jeconnoc
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/03/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 55cb9d318c06ed3ecef87ab457c7405aecbf1238
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162885"
---
# <a name="create-a-function-app-for-serverless-code-execution"></a>Erstellen einer Funktions-App für die serverlose Codeausführung

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App, die als Container für Ihre Funktionen fungiert. Die Funktions-App wird unter Verwendung des [Verbrauchsplans](../functions-scale.md#consumption-plan) erstellt, der sich ideal für ereignisgesteuerte serverlose Workloads eignet.

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt eine Azure-Funktionen-App mithilfe des [Verbrauchsplans](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellt ein Azure Storage-Konto. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktionen-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

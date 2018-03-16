---
title: Erstellen einer Azure-Funktion, die eine Verbindung mit einem Azure-Speicher herstellt | Microsoft-Dokumentation
description: 'Azure CLI-Skriptbeispiel: Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Storage herstellt'
services: functions
documentationcenter: functions
author: ggailey777
manager: cfowler
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: cbe7bf95574ca7a77d666981691da05357ce9a0d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-function-app-that-connects-to-an-azure-storage-account"></a>Erstellen einer Funktions-App, die eine Verbindung mit einem Azure Storage-Konto herstellt

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App und stellt eine Verbindung zwischen der Funktion und einem Azure Storage-Konto her. Die erstellte App-Einstellung mit der Verbindung kann mit einem [Storage-Trigger oder einer Storage-Bindung](..\functions-bindings-storage-blob.md) verwendet werden. 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Verwenden Sie im Fall der lokalen Befehlszeilenschnittstelle mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel erstellt eine Azure-Funktionen-App und fügt einer App-Einstellung die Speicher-Verbindungszeichenfolge hinzu.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels werden mit dem folgenden Befehl die Ressourcengruppe und alle zugehörigen Ressourcen entfernt:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az login](https://docs.microsoft.com/cli/azure/reference-index#az_login) | Melden Sie sich bei Azure an. |
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellen Sie eine Ressourcengruppe mit einem Speicherort. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account) | Speicherkonto erstellen |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Erstellen Sie eine neue Funktionen-App. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Bereinigen |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

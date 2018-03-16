---
title: Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Cosmos DB herstellt | Microsoft-Dokumentation
description: 'Azure CLI-Skriptbeispiel: Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Cosmos DB herstellt'
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
ms.date: 01/22/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 092e0681b0491fc1c54c19e234aafdac6d428fd1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Cosmos DB herstellt

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App und stellt eine Verbindung zwischen der Funktion und einer Azure Cosmos DB-Datenbank her. Die erstellte App-Einstellung mit der Verbindung kann mit einem [Azure Cosmos DB-Trigger oder einer Azure Cosmos DB-Bindung](..\functions-bindings-cosmosdb.md) verwendet werden.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Verwenden Sie im Fall der lokalen Befehlszeilenschnittstelle mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

In diesem Beispiel werden eine Azure-Funktionen-App erstellt und ein Cosmos DB-Endpunkt sowie ein Zugriffsschlüssel zu den App-Einstellungen hinzugefügt.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript benutzt die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az login](https://docs.microsoft.com/cli/azure/reference-index#az_login) | Melden Sie sich bei Azure an. |
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellen Sie eine Ressourcengruppe mit einem Speicherort. |
| [az storage accounts create](https://docs.microsoft.com/cli/azure/storage/account) | Speicherkonto erstellen |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Erstellen Sie eine neue Funktionen-App. |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Erstellen Sie die Cosmos DB-Datenbank. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Bereinigen |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).





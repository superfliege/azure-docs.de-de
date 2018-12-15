---
title: Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Cosmos DB herstellt | Microsoft-Dokumentation
description: 'Azure CLI-Skriptbeispiel: Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Cosmos DB herstellt'
services: functions
documentationcenter: functions
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/03/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 15a7cc1940a01486c6b660ec65b47f072dc7996e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52970676"
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Cosmos DB herstellt

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App und stellt eine Verbindung zwischen der Funktion und einer Azure Cosmos DB-Datenbank her. Die erstellte App-Einstellung mit der Verbindung kann mit einem [Azure Cosmos DB-Trigger oder einer Azure Cosmos DB-Bindung](../functions-bindings-cosmosdb.md) verwendet werden.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Verwenden Sie im Fall der lokalen Befehlszeilenschnittstelle mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript

In diesem Beispiel werden eine Azure-Funktionen-App erstellt und ein Cosmos DB-Endpunkt sowie ein Zugriffsschlüssel zu den App-Einstellungen hinzugefügt.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Erstellen Sie eine Ressourcengruppe mit einem Speicherort. |
| [az storage accounts create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Speicherkonto erstellen |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktions-App im serverlosen [Verbrauchsplan](../functions-scale.md#consumption-plan). |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#az-cosmosdb-create) | Erstellen Sie eine Azure Cosmos DB-Datenbank. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).





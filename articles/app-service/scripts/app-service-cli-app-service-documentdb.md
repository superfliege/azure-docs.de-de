---
title: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung zwischen einer Web-App und MongoDB (Cosmos DB) | Microsoft Docs"
description: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung zwischen einer Web-App und MongoDB (Cosmos DB)"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a7ee310af0e0ec33be2fa936a6ce3ee74369643d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="connect-a-web-app-to-cosmos-db"></a>Herstellen einer Verbindung zwischen einer Web-App und Cosmos DB

Dieses Beispielskript erstellt ein Azure Cosmos DB-Konto mit der MongoDB-API und eine Azure-Web-App. Anschließend wird die MongoDB-Verbindungszeichenfolge mithilfe von App-Einstellungen mit der Web-App verknüpft.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, benötigen Sie die Azure CLI-Version 2.0 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, die Cosmos DB-Instanz und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | Erstellt einen App Service-Plan. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Erstellt eine Azure-Web-App. |
| [`az cosmosdb create`](/cli/azure/cosmosdb?view=azure-cli-latest#az_cosmosdb_create) | Erstellt ein Cosmos DB-Konto. |
| [`az cosmosdb list-connection-strings`](/cli/azure/cosmosdb?view=azure-cli-latest#az_cosmosdb_list_connection_strings) | Listet die Verbindungszeichenfolgen für das angegebene Cosmos DB-Konto auf. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) | Erstellt oder aktualisiert eine App-Einstellung für eine Azure-Web-App. App-Einstellungen werden als Umgebungsvariablen für Ihre App verfügbar gemacht. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).

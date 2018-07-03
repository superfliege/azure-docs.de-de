---
title: Azure CLI-Skriptbeispiel – Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App in Azure.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/26/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7d3fc71bc53e85fa7555dbee5ee79b3f06f27fe8
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960337"
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App

Dieses Beispielskript erstellt eine Funktionen-App mit den zugehörigen Ressourcen und ordnet ihr dann `www.<yourdomain>` zu. Wenn Ihre Funktionen-App unter einem [App Service-Plan](../functions-scale.md#app-service-plan) gehostet wird, können Sie eine benutzerdefinierte Domäne zuordnen, indem Sie entweder einen CNAME- oder A-Eintrag verwenden. Für Funktionen-Apps unter einem [Verbrauchstarif](../functions-scale.md#consumption-plan) wird nur die CNAME-Option unterstützt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, benötigen Sie mindestens die Version 2.0 der Azure CLI. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 


## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript benutzt die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Speicherkonto, das für die Funktionen-App erforderlich ist. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Erstellt einen App Service-Plan, der zum Zuordnen einer benutzerdefinierten Domäne erforderlich ist. |
| [az functionapp create]() | Erstellt eine Funktionen-App. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Ordnet eine benutzerdefinierte Domäne zu einer Funktionen-App zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions]().

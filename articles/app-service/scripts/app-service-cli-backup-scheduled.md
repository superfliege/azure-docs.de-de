---
title: "Azure CLI-Skriptbeispiel – Erstellen einer geplanten Sicherung für eine Web-App | Microsoft Docs"
description: "Azure CLI-Skriptbeispiel – Erstellen einer geplanten Sicherung für eine Web-App"
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 12/11/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: b5d7a1941e963e25111327c7336d7a490e8f14d8
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-scheduled-backup-for-a-web-app"></a>Erstellen einer geplanten Sicherung für eine Web-App

Dieses Beispielskript erstellt in App Service eine Web-App mit den zugehörigen Ressourcen und dann eine geplante Sicherung dafür. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, benötigen Sie die Azure CLI-Version 2.0 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-scheduled/backup-scheduled.sh?highlight=3-7 "Create a scheduled backup for a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create) | Erstellt ein Speicherkonto. |
| [`az storage container create`](/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create) | Erstellt einen Azure-Speichercontainer. |
| [`az storage container generate-sas`](/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_generate_sas) | Generiert ein SAS-Token für einen Azure-Speichercontainer.  |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | Erstellt einen App Service-Plan. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Erstellt eine Azure-Web-App. |
| [`az webapp config backup update`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_update) | Konfiguriert einen neuen Sicherungszeitplan für eine Web-App. |
| [`az webapp config backup show`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_show) | Zeigt den Sicherungszeitplan für eine Web-App an. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_list) | Ruft eine Liste der Sicherungen für eine Web-App ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).

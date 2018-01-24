---
title: "CLI-Beispiel: Überwachen und Skalieren einer einzelnen Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Azure CLI-Beispielskript zum Überwachen und Skalieren einer einzelnen Azure SQL-Datenbank"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 12/14/2017
ms.author: janeng
ms.openlocfilehash: 741c066d62364e34b788883bfc96fba1ea3507c3
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2017
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>Überwachen und Skalieren einer einzelnen SQL-­Datenbank mithilfe der CLI

In diesem Azure CLI-Skriptbeispiel wird eine einzelne Azure SQL-Datenbank nach Abfrage der Größeninformationen der Datenbank auf eine andere Leistungsstufe skaliert. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

> [!TIP]
> Verwenden Sie [az sql db op list](/cli/azure/sql/db/op?#az_sql_db_op_list), um eine Liste von in der Datenbank ausgeführten Vorgängen zu erhalten, und verwenden Sie [az sql db op cancel](/cli/azure/sql/db/op#az_sql_db_op_cancel), um einen Aktualisierungsvorgang in der Datenbank abzubrechen.

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Erstellt einen logischen Server, der eine Datenbank hostet. |
| [az sql db show-usage](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_show_usage) | Zeigt die Größennutzungsinformationen für eine Datenbank an. |
| [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_update) | Aktualisiert Datenbankeigenschaften (z.B. die Dienstebene oder Leistungsstufe) oder verschiebt eine Datenbank in Pools für elastische Datenbanken, daraus hinaus oder zwischen ihnen. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../sql-database-cli-samples.md).

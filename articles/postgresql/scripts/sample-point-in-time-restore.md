---
title: 'Azure CLI-Skript: Wiederherstellen eines Azure Database for PostgreSQL-Servers'
description: "Dieses Beispielskript für die Azure-Befehlszeilenschnittstelle veranschaulicht das Wiederherstellen eines Azure Database for MySQL-Servers und seiner Datenbanken auf einen früheren Zeitpunkt."
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 242dd836a629d3accd0c43a72b4549e93145168f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="restore-an-azure-database-for-postgresql-server-using-azure-cli"></a>Wiederherstellen eines Azure Database for PostgreSQL-Servers mithilfe der Azure-Befehlszeilenschnittstelle
Dieses CLI-Beispielskript stellt den früheren Zustand eines einzelnen Azure Database for PostgreSQL-Servers wieder her.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie es bevorzugen, die Befehlszeilenschnittstelle lokal zu installieren und zu verwenden, müssen Sie für dieses Beispiel die Azure-Befehlszeilenschnittstelle in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript
Ändern Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators anzupassen. Ersetzen Sie die Abonnement-ID, die in den az monitor-Befehlen verwendet wird, durch Ihre eigene Abonnement-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/backup-restore/backup-restore.sh?highlight=15-16 "Restore Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/backup-restore/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az postgresql server create](/cli/azure/postgresql/server#az_msql_server_create) | Erstellt einen PostgreSQL-Server, der die Datenbanken hostet. |
| [az postgresql server restore](/cli/azure/postgresql/server#az_msql_server_restore) | Stellt einen Server aus einer Sicherung wieder her |
| [az group delete](/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie weitere Informationen zur Azure CLI: [Azure CLI-Dokumentation](/cli/azure/overview).
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für PostgreSQL](../sample-scripts-azure-cli.md)
- [Sichern und Wiederherstellen eines Servers in Azure Database for PostgreSQL mit dem Azure-Portal](../howto-restore-server-portal.md)

---
title: 'Azure CLI-Skript: Herunterladen von Serverprotokollen in Azure Database for PostgreSQL'
description: "Dieses Beispielskript für die Azure-Befehlszeilenschnittstelle veranschaulicht das Aktivieren und Herunterladen der Serverprotokolle eines Azure Database for PostgreSQL-Servers."
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 8bd6e193ca52401b16e141162a76d39bdef840f1
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Aktivieren und Herunterladen der Serverprotokolle zu langsamen Abfragen von einem Azure Database for PostgreSQL-Server mithilfe der Azure-Befehlszeilenschnittstelle
Dieses Beispielskript für die Befehlszeilenschnittstelle aktiviert die Serverprotokolle zu langsamen Abfragen auf einem Azure Database for PostgreSQL-Server und lädt sie herunter.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie es bevorzugen, die Befehlszeilenschnittstelle lokal zu installieren und zu verwenden, müssen Sie für dieses Beispiel die Azure-Befehlszeilenschnittstelle in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript
Ändern Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators anzupassen. Ersetzen Sie <log_file_name> in den az monitor-Befehlen durch den Dateinamen Ihres eigenen Serverprotokolls.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az postgres server create](/cli/azure/postgres/server#az_msql_server_create) | Erstellt einen PostgreSQL-Server, der die Datenbanken hostet. |
| [az postgres server configuration list](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Listet die Konfigurationswerte für einen Server auf |
| [az postgres server configuration set](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Aktualisiert die Konfiguration eines Servers |
| [az postgres server-logs list](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) | Listet die Protokolldateien für einen Server auf. |
| [az postgres server-logs download](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) | Lädt Protokolldateien herunter |
| [az group delete](/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie weitere Informationen zur Azure CLI: [Azure CLI-Dokumentation](/cli/azure/overview).
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für PostgreSQL](../sample-scripts-azure-cli.md)
- [Konfigurieren der und Zugreifen auf die Serverprotokolle im Azure-Portal](../howto-configure-server-logs-in-portal.md)

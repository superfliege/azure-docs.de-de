---
title: Konfigurieren der Dienstparameter in Azure Database for PostgreSQL
description: "In diesem Artikel wird beschrieben, wie Sie mit der Azure CLI-Befehlszeile die Dienstparameter in Azure-Datenbank für PostgreSQL konfigurieren."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: df32578c92b1973568c9b811cb7f9e9db067915f
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Anpassen der Serverkonfigurationsparameter mithilfe der Azure CLI
Sie können Konfigurationsparameter für einen Azure PostgreSQL-Server mithilfe der Befehlszeilenschnittstelle (Azure CLI) auflisten, anzeigen und aktualisieren. Auf Serverebene ist eine Teilmenge der Engine-Konfigurationen verfügbar und kann geändert werden. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Erstellen Sie einen Azure Database for PostgreSQL-Server und eine -Datenbank gemäß [Erstellen einer Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).
- Installieren Sie die Befehlszeilenschnittstelle [Azure CLI 2.0](/cli/azure/install-azure-cli) auf Ihrem Computer, oder verwenden Sie die [Azure Cloud Shell](../cloud-shell/overview.md) im Azure-Portal über Ihren Browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Auflisten der Serverkonfigurationsparameter für Server für Azure-Datenbank für PostgreSQL
Führen Sie den Befehl [az postgres server configuration list](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) aus, um alle änderbaren Parameter eines Servers mit ihren Werten aufzulisten.

Sie können die Serverkonfigurationsparameter für den Server **mydemoserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mydemoserver
```
## <a name="show-server-configuration-parameter-details"></a>Anzeigen von Details zu Serverkonfigurationsparametern
Um Details zu einem bestimmten Konfigurationsparameter für einen Server anzuzeigen, führen Sie den Befehl [az postgres server configuration show](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) aus.

Dieses Beispiel zeigt Details des Serverkonfigurationsparameters **log\_min\_messages** für den Server **mydemoserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup**.
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-server-configuration-parameter-value"></a>Ändern des Werts von Serverkonfigurationsparametern
Sie können den Wert eines bestimmten Serverkonfigurationsparameters ändern und dadurch den zugrunde liegenden Konfigurationswert für die PostgreSQL-Server-Engine aktualisieren. Um die Konfiguration zu aktualisieren, führen Sie den Befehl [az postgres server configuration set](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) aus. 

Aktualisieren Sie den Serverkonfigurationsparameter **log\_min\_messages** des Servers **mydemoserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup**.
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver --value INFO
```
Wenn Sie den Wert eines Konfigurationsparameters zurücksetzen möchten, lassen Sie einfach den optionalen Parameter `--value` weg. Der Dienst wendet dann den Standardwert an. Im obigen Beispiel könnte dies so aussehen:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
Mit diesem Befehl wird die Konfiguration **log\_min\_messages** auf den Standardwert **WARNING** zurückgesetzt. Weitere Informationen zur Serverkonfiguration und zu den zulässigen Werten finden Sie in der PostgreSQL-Dokumentation zur [Serverkonfiguration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Nächste Schritte
- Informationen dazu, wie Sie Serverprotokolle konfigurieren und darauf zugreifen, finden Sie unter [Serverprotokolle in Azure-Datenbank für PostgreSQL](concepts-server-logs.md).

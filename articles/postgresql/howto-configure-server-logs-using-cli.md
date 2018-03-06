---
title: "Konfigurieren von Serverprotokollen für PostgreSQL und Zugreifen auf diese mithilfe der Azure-Befehlszeilenschnittstelle"
description: "In diesem Artikel wird beschrieben, wie Sie mit der Azure CLI-Befehlszeile die Serverprotokolle in Azure-Datenbank für PostgreSQL konfigurieren und auf diese zugreifen."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: e12a8907b641b4591ed5ff9fdd5d8458eb75525e
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI
Sie können die PostgreSQL-Serverfehlerprotokolle mithilfe der Befehlszeilenschnittstelle (Azure CLI) herunterladen. Der Zugriff auf Transaktionsprotokolle wird jedoch nicht unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen [Azure-Datenbank für PostgreSQL-Server](quickstart-create-server-database-azure-cli.md)
- Installieren Sie das [Azure CLI 2.0](/cli/azure/install-azure-cli)-Befehlszeilenprogramm, oder verwenden Sie Azure Cloud Shell im Browser.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Konfigurieren der Protokollierung für Azure-Datenbank für PostgreSQL
Sie können den Server für den Zugriff auf Abfrage- und Fehlerprotokolle konfigurieren. Fehlerprotokolle können Informationen über „auto-vacuum“, Verbindungen und Prüfpunkte enthalten.
1. Aktivieren Sie die Protokollierung.
2. Aktualisieren Sie „log\_statement“ und „log\_min\_duration\_statement“, um die Abfrageprotokollierung zu aktivieren.
3. Aktualisieren Sie den Aufbewahrungszeitraum.

Weitere Informationen finden Sie unter [Anpassen der Serverkonfigurationsparameter](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Auflisten der Protokolle für Server für Azure-Datenbank für PostgreSQL
Um die verfügbaren Protokolldateien für Ihren Server aufzulisten, führen Sie den Befehl [az postgres server-logs list](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) aus.

Sie können die Protokolldateien für den Server **mydemoserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten und an diese Textdatei weiterleiten: **log\_files\_list.txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Herunterladen von Protokollen vom Server auf den lokalen Computer
Mit dem Befehl [az postgres server-logs download](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) können Sie einzelne Protokolldateien für Ihren Server herunterladen. 

Dieses Beispiel lädt die spezifische Protokolldatei für den Server **mydemoserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup** in Ihre lokale Umgebung herunter.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu Serverprotokollen finden Sie unter [Serverprotokolle in Azure-Datenbank für PostgreSQL](concepts-server-logs.md).
- Weitere Informationen zu Serverparametern finden Sie unter [Anpassen der Serverkonfigurationsparameter über die Azure CLI](howto-configure-server-parameters-using-cli.md).

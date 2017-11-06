---
title: Zugreifen auf Serverprotokolle in Azure Database for MySQL mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mit der Azure CLI-Befehlszeile auf die Serverprotokolle in Azure Database for MySQL zugreifen.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 10/18/2017
ms.openlocfilehash: 6ee2c2c6e6cff824d3167ea600a1ddc778ad011b
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI
Sie können die Serverprotokolle von Azure Database for MySQL mithilfe der Azure-Befehlszeilenschnittstelle, dem Azure-Befehlszeilenprogramm, herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Azure Database for MySQL-Server](quickstart-create-mysql-server-database-using-azure-cli.md)
- Die [Azure CLI 2.0](/cli/azure/install-azure-cli) oder alternativ Azure Cloud Shell im Browser

## <a name="configure-logging-for-azure-database-for-mysql"></a>Konfigurieren der Protokollierung für Azure-Datenbank for MySQL
Sie können den Server für den Zugriff auf das Protokoll für langsame MySQL-Abfragen konfigurieren.
1. Aktivieren Sie die Protokollierung, indem Sie den Parameter **slow\_query\_log** auf „EIN“ festlegen.
2. Passen Sie andere Parameter an, z.B. **long\_query\_time** und **log\_slow\_admin\_statements**.

Unter [Konfigurieren von Serverparametern](howto-configure-server-parameters-using-cli.md) erfahren Sie, wie Sie den Wert dieser Parameter über die Azure-Befehlszeilenschnittstelle festlegen.

Beispielsweise aktiviert der folgende CLI-Befehl das Protokoll für langsame Abfragen, legt die Mindestlaufzeit für Protokollierung auf 10 Sekunden fest und deaktiviert die Protokollierung für die langsame Administratoranweisung. Schließlich werden die Konfigurationsoptionen für Ihre Überprüfung aufgelistet.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server myserver4demo --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server myserver4demo --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server myserver4demo --value OFF
az mysql server configuration list --resource-group myresourcegroup --server myserver4demo
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>Auflisten der Protokolle für Server für Azure-Datenbank für MySQL
Um die verfügbaren Protokolldateien für Ihren Server aufzulisten, führen Sie den Befehl [az mysql server-logs list](/cli/azure/mysql/server-logs#list) aus.

Sie können die Protokolldateien für den Server **myserver4demo.mysql.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten und an diese Textdatei leiten: **log\_files\_list.txt.**
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server myserver4demo > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Herunterladen von Protokollen vom Server
Mit dem Befehl [az mysql server-logs download](/cli/azure/mysql/server-logs#download) können Sie einzelne Protokolldateien für Ihren Server herunterladen. 

Dieses Beispiel lädt die spezifische Protokolldatei für den Server **myserver4demo.mysql.database.azure.com** in der Ressourcengruppe **myresourcegroup** in Ihre lokale Umgebung herunter.
```azurecli-interactive
az mysql server-logs download --name 20170414-myserver4demo-mysql.log --resource-group myresourcegroup --server myserver4demo
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr zu [Serverprotokollen in Azure Database for MySQL](concepts-server-logs.md)

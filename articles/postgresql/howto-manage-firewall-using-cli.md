---
title: Erstellen und Verwalten von Firewallregeln für die Azure-Datenbank für PostgreSQL mithilfe der Azure CLI
description: In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure CLI-Befehlszeile Firewallregeln für Azure-Datenbank für PostgreSQL erstellen und verwalten.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 05/4/2018
ms.openlocfilehash: 041f1c426f8f181255e315978878d146a14bc88b
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410254"
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Erstellen und Verwalten von Firewallregeln für die Azure-Datenbank für PostgreSQL mithilfe der Azure CLI
Mithilfe von Firewallregeln auf Serverebene können Administratoren den Zugriff auf einen Server für Azure-Datenbank für PostgreSQL über eine bestimmte IP-Adresse oder über einen IP-Adressbereich verwalten. Dank praktischer Azure CLI-Befehle können Sie zum Verwalten Ihres Servers Firewallregeln erstellen, aktualisieren, löschen, auflisten und anzeigen. Eine Übersicht über Firewallregeln für Azure Database for PostgreSQL finden Sie unter [Firewallregeln für Azure Database for PostgreSQL-Server](concepts-firewall-rules.md).

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli), oder verwenden Sie Azure Cloud Shell im Browser.
- Einen [Server und eine Datenbank für Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Konfigurieren von Firewallregeln für Azure-Datenbank für PostgreSQL
Die [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule)-Befehle werden zum Konfigurieren von Firewallregeln verwendet.

## <a name="list-firewall-rules"></a>Auflisten von Firewallregeln 
Führen Sie zum Auflisten der vorhandenen Serverfirewallregeln den [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_list)-Befehl aus.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Die Ausgabe listet die Firewallregeln (sofern vorhanden) standardmäßig im JSON-Format auf. Sie können mit dem Switch `--output table` ein besser lesbares Tabellenformat als die Ausgabe erzeugen.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-firewall-rule"></a>Erstellen einer Firewallregel
Erstellen Sie mit dem Befehl [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) eine neue Firewallregel auf dem Server. 

```
To allow access to a singular IP address, provide the same address in the `--start-ip-address` and `--end-ip-address`, as in this example, replacing the IP shown here with your specific IP.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Um Anwendungen die Verbindung von Azure-IP-Adressen mit dem Azure Database for PostgreSQL-Server zu erlauben, geben Sie wie in diesem Beispiel die IP-Adresse 0.0.0.0 als Start-IP und End-IP an.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowAllAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Diese Option konfiguriert die Firewall derart, dass alle von Azure ausgehenden Verbindungen zugelassen werden, einschließlich Verbindungen von den Abonnements anderer Kunden. Wenn Sie diese Option auswählen, stellen Sie sicher, dass die Anmelde- und die Benutzerberechtigungen den Zugriff nur auf autorisierte Benutzer beschränken.
> 

Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen erstellten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="update-firewall-rule"></a>Aktualisieren einer Firewallregel 
Aktualisieren Sie mit dem Befehl [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_update) eine vorhandene Firewallregel auf dem Server. Geben Sie den Namen der vorhandenen Firewallregel als Eingabe und die zu aktualisierenden Attribute für Start-IP und End-IP ein.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.0
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen aktualisierten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.
> [!NOTE]
> Falls die Firewallregel nicht vorhanden ist, wird sie vom Updatebefehl erstellt.

## <a name="show-firewall-rule-details"></a>Anzeigen der Details einer Firewallregel
Sie können auch die Details einer vorhandenen Firewallregel auf Serverebene anzeigen, indem Sie den Befehl [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_show) ausführen.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen angegebenen Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="delete-firewall-rule"></a>Löschen einer Firewallregel
Um den Zugriff für einen IP-Adressbereich auf den Server zu widerrufen, löschen Sie eine vorhandene Firewallregel durch Ausführen des Befehls [az postgres server firewall-rule delete](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_delete). Geben Sie den Namen der vorhandenen Firewallregel an.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Bei erfolgreicher Ausführung wird keine Ausgabe angezeigt. Bei einem Fehler wird eine Fehlermeldung zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte
- Sie können auch einen Webbrowser zum [Erstellen und Verwalten von Firewallregeln für Azure Database for PostgreSQL mithilfe des Azure-Portals](howto-manage-firewall-using-portal.md) verwenden.
- Weitere Informationen zu den [Firewallregeln des Servers für Azure Database for PostgreSQL](concepts-firewall-rules.md).
- Wenn Sie Unterstützung beim Herstellen einer Verbindung mit einem Server für Azure Database for PostgreSQL benötigen, lesen Sie die Informationen unter [Datenverbindungsbibliotheken für Azure Database for PostgreSQL](concepts-connection-libraries.md).

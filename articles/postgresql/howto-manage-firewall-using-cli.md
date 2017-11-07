---
title: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe der Azure CLI | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure CLI-Befehlszeile Firewallregeln für Azure-Datenbank für PostgreSQL erstellen und verwalten."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 05e645ebafd8c8b9f3078524b76732b3e1be05f8
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Erstellen und Verwalten von Firewallregeln für die Azure-Datenbank für PostgreSQL mithilfe der Azure CLI
Mithilfe von Firewallregeln auf Serverebene können Administratoren den Zugriff auf einen Server für Azure-Datenbank für PostgreSQL über eine bestimmte IP-Adresse oder über einen IP-Adressbereich verwalten. Dank praktischer Azure CLI-Befehle können Sie zum Verwalten Ihres Servers Firewallregeln erstellen, aktualisieren, löschen, auflisten und anzeigen. Eine Übersicht über Firewallregeln für Azure Database for PostgreSQL finden Sie unter [Firewallregeln für Azure Database for PostgreSQL-Server](concepts-firewall-rules.md).

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen [Server und eine Datenbank für Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).
- Installieren Sie das [Azure CLI 2.0](/cli/azure/install-azure-cli)-Befehlszeilenprogramm, oder verwenden Sie Azure Cloud Shell im Browser.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Konfigurieren von Firewallregeln für Azure-Datenbank für PostgreSQL
Die [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule)-Befehle werden zum Konfigurieren von Firewallregeln verwendet.

## <a name="list-firewall-rules"></a>Auflisten von Firewallregeln 
Führen Sie zum Auflisten der vorhandenen Serverfirewallregeln den [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule#list)-Befehl aus.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
Die Ausgabe listet die Firewallregeln (sofern vorhanden) standardmäßig im JSON-Format auf. Sie können mit dem Switch `--output table` ein besser lesbares Tabellenformat als die Ausgabe erzeugen.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Erstellen einer Firewallregel
Erstellen Sie mit dem Befehl [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) eine neue Firewallregel auf dem Server. 

Durch die Angabe von 0.0.0.0 als `--start-ip-address`-Bereich und von 255.255.255.255 als `--end-ip-address`-Bereich wird es dem Bereich aller IP-Adressen in diesem Beispiel erlaubt, auf den Server **mypgserver-20170401.postgres.database.azure.com** zuzugreifen.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
Damit nur der Zugriff auf eine einzelne IP-Adresse gewährt wird, geben Sie wie in diesem Beispiel dieselbe Adresse als `--start-ip-address` und `--end-ip-address` an.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen erstellten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="update-firewall-rule"></a>Aktualisieren einer Firewallregel 
Aktualisieren Sie mit dem Befehl [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule#update) eine vorhandene Firewallregel auf dem Server. Geben Sie den Namen der vorhandenen Firewallregel als Eingabe und die zu aktualisierenden Attribute für Start-IP und End-IP ein.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen aktualisierten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.
> [!NOTE]
> Falls die Firewallregel nicht vorhanden ist, wird sie vom Updatebefehl erstellt.

## <a name="show-firewall-rule-details"></a>Anzeigen der Details einer Firewallregel
Sie können auch die Details einer vorhandenen Firewallregel auf Serverebene anzeigen, indem Sie den Befehl [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule#show) ausführen.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen angegebenen Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="delete-firewall-rule"></a>Löschen einer Firewallregel
Um den Zugriff für einen IP-Adressbereich auf den Server zu widerrufen, löschen Sie eine vorhandene Firewallregel durch Ausführen des Befehls [az postgres server firewall-rule delete](/cli/azure/postgres/server/firewall-rule#delete). Geben Sie den Namen der vorhandenen Firewallregel an.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Bei erfolgreicher Ausführung wird keine Ausgabe angezeigt. Bei einem Fehler wird eine Fehlermeldung zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte
- Sie können auch einen Webbrowser zum [Erstellen und Verwalten von Firewallregeln für Azure Database for PostgreSQL mithilfe des Azure-Portals](howto-manage-firewall-using-portal.md) verwenden.
- Weitere Informationen zu den [Firewallregeln des Servers für Azure Database for PostgreSQL](concepts-firewall-rules.md).
- Wenn Sie Unterstützung beim Herstellen einer Verbindung mit einem Server für Azure Database for PostgreSQL benötigen, lesen Sie die Informationen unter [Datenverbindungsbibliotheken für Azure Database for PostgreSQL](concepts-connection-libraries.md).

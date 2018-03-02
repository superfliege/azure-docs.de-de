---
title: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe der Azure CLI"
description: "In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure CLI-Befehlszeile Firewallregeln für Azure Database for MySQL erstellen und verwalten."
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
<<<<<<< HEAD
ms.date: 02/28/2018
ms.openlocfilehash: b142d08379f8a8fde20178668a4c1343f08aedbc
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
=======
ms.date: 02/12/2018
ms.openlocfilehash: 77254d91bcfa7cbd6070e3baeb98fd7cc5ad44cf
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe der Azure CLI
Mithilfe von Firewallregeln auf Serverebene können Administratoren den Zugriff auf eine Azure Database for MySQL-Serverinstanz über eine bestimmte IP-Adresse oder über einen IP-Adressbereich verwalten. Dank praktischer Azure CLI-Befehle können Sie zum Verwalten Ihres Servers Firewallregeln erstellen, aktualisieren, löschen, auflisten und anzeigen. Eine Übersicht über Firewalls für Azure-Datenbank für MySQL finden Sie unter [Azure Database for MySQL server firewall rules](./concepts-firewall-rules.md) (Firewallregeln des Servers für Azure-Datenbank für MySQL).

## <a name="prerequisites"></a>Voraussetzungen
* [Installieren der Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Einen [Azure Database for MySQL-Server und eine Datenbank](quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="firewall-rule-commands"></a>Befehle für Firewallregeln:
Der Befehl **az mysql server firewall-rule** wird in der Azure CLI zum Erstellen, Löschen, Auflisten, Anzeigen und Aktualisieren von Firewallregeln verwendet.

Befehle:
- **create**: Dient zum Erstellen einer Firewallregel für den Azure MySQL-Server.
- **delete**: Dient zum Löschen einer Firewallregel für den Azure MySQL-Server.
- **list**: Dient zum Auflisten der Serverfirewallregeln für Azure MySQL-Server.
- **show**: Dient zum Anzeigen der Details einer Firewallregel für den Azure MySQL-Server.
- **update**: Dient zum Aktualisieren der Firewallregel für den Azure MySQL-Server.

## <a name="log-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Anmelden bei Azure und Auflisten Ihrer Azure Database for MySQL-Serverinstanzen
Stellen Sie mit dem Befehl **az login** eine sichere Verbindung zwischen der Azure CLI und Ihrem Azure-Konto her.

1. Führen Sie an der Befehlszeile den folgenden Befehl aus:
```azurecli
az login
```
Dieser Befehl gibt einen Code aus, der im nächsten Schritt verwendet wird.

2. Öffnen Sie in einem Webbrowser die Seite [https://aka.ms/devicelogin](https://aka.ms/devicelogin), und geben Sie den Code ein.

3. Melden Sie sich an der Eingabeaufforderung mit Ihren Azure-Anmeldeinformationen an.

4. Nachdem Ihre Anmeldung autorisiert wurde, wird eine Liste der Abonnements in der Konsole ausgegeben. Kopieren Sie die ID des gewünschten Abonnements, um das aktuelle Abonnement festzulegen, das verwendet werden soll. Verwenden Sie den Befehl [az account set](/cli/azure/account#az_account_set).
   ```azurecli-interactive
   az account set --subscription <your subscription id>
   ```

5. Listen Sie die Server für Azure-Datenbanken für MySQL für Ihr Abonnement und die Ressourcengruppe auf, wenn Sie sich in Bezug auf die Namen unsicher sind. Verwenden Sie den Befehl [az mysql server list](/cli/azure/mysql/server#az_mysql_server_list).

   ```azurecli-interactive
   az mysql server list --resource-group myresourcegroup
   ```

   Notieren Sie das Attribut „Name“ in der Auflistung. Mit diesem Attribut wird angegeben, welche Serverinstanz von MySQL verwendet werden soll. Bestätigen Sie ggf. die Details für diesen Server, um mithilfe des name-Attributs die Richtigkeit des Namens zu überprüfen. Verwenden Sie den Befehl [az mysql server show](/cli/azure/mysql/server#az_mysql_server_show).

   ```azurecli-interactive
<<<<<<< HEAD
   az mysql server show --resource-group myresourcegroup --name mydemoserver
=======
   az mysql server show --resource-group myResourceGroup --name mydemoserver
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Auflisten der Firewallregeln für die Azure Database for MySQL-Instanz 
Listen Sie mithilfe des Servernamens und des Ressourcengruppennamens die vorhandenen Firewallregeln auf dem Server auf. Verwenden Sie den Befehl [az mysql server firewall list](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_list).  Beachten Sie, dass das Attribut mit dem Servernamen im Switch **--server** und nicht im Switch**--name** angegeben ist. 
```azurecli-interactive
<<<<<<< HEAD
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Die Ausgabe listet die Regeln (sofern vorhanden) standardmäßig im JSON-Format auf. Sie können mit dem Switch **--output table** Ergebnisse in einem besser lesbaren Tabellenformat ausgeben.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
=======
az mysql server firewall-rule list --resource-group myResourceGroup --server-name mydemoserver
```
Die Ausgabe listet die Regeln (sofern vorhanden) standardmäßig im JSON-Format auf. Sie können mit dem Switch **--output table** Ergebnisse in einem besser lesbaren Tabellenformat ausgeben.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server-name mydemoserver --output table
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Erstellen einer Firewallregel für die Azure Database for MySQL-Serverinstanz
Erstellen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine neue Firewallregel auf dem Server. Verwenden Sie den Befehl [az mysql server firewall create](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_create). Geben Sie einen Namen für die Regel sowie die Start-IP- und End-IP-Adresse ein, um einen Bereich von IP-Adressen anzugeben, denen der Zugriff gewährt wird.
```azurecli-interactive
<<<<<<< HEAD
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
=======
az mysql server firewall-rule create --resource-group myResourceGroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
```

Damit nur einer einzelnen IP-Adresse Zugriff gewährt wird, geben Sie wie in diesem Beispiel dieselbe Adresse als Start-IP- und End-IP-Adresse an.
```azurecli-interactive
<<<<<<< HEAD
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
=======
az mysql server firewall-rule create --resource-group myResourceGroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
```

Um Anwendungen die Verbindung von Azure-IP-Adressen mit dem Azure Database for MySQL-Server zu erlauben, geben Sie wie in diesem Beispiel die IP-Adresse 0.0.0.0 als Start-IP und End-IP an.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mysql --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Diese Option konfiguriert die Firewall derart, dass alle von Azure ausgehenden Verbindungen zugelassen werden, einschließlich Verbindungen von den Abonnements anderer Kunden. Wenn Sie diese Option auswählen, stellen Sie sicher, dass die Anmelde- und die Benutzerberechtigungen den Zugriff nur auf autorisierte Benutzer beschränken.
> 

Bei erfolgreicher Ausführung listet die Befehlsausgabe für die Erstellung die Details der von Ihnen erstellten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>Aktualisieren einer Firewallregel für die Azure Database for MySQL-Serverinstanz 
Aktualisieren Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine vorhandene Firewallregel auf dem Server. Verwenden Sie den Befehl [az mysql server firewall update](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_update). Geben Sie den Namen der vorhandenen Firewallregel als Eingabe sowie die zu aktualisierenden Attribute für die Start-IP- und End-IP-Adresse ein.
```azurecli-interactive
<<<<<<< HEAD
az mysql server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
=======
az mysql server firewall-rule update --resource-group myResourceGroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen aktualisierten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

> [!NOTE]
> Falls die Firewallregel nicht vorhanden ist, wird sie vom Befehl „Update“ erstellt.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Anzeigen von Firewallregeldetails für die Azure Database for MySQL-Serverinstanz
Zeigen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens die Details der vorhandenen Firewallregel auf dem Server an. Verwenden Sie den Befehl [az mysql server firewall show](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_show). Geben Sie den Namen der vorhandenen Firewallregel als Eingabe an.
```azurecli-interactive
<<<<<<< HEAD
az mysql server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
=======
az mysql server firewall-rule show --resource-group myResourceGroup --server-name mydemoserver --name FirewallRule1
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen angegebenen Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>Löschen einer Firewallregel für die Azure Database for MySQL-Serverinstanz
Entfernen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine vorhandene Firewallregel vom Server. Verwenden Sie den Befehl [az mysql server firewall delete](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_delete). Geben Sie den Namen der vorhandenen Firewallregel an.
```azurecli-interactive
<<<<<<< HEAD
az mysql server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
=======
az mysql server firewall-rule delete --resource-group myResourceGroup --server-name mydemoserver --name FirewallRule1
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
```
Bei erfolgreicher Ausführung wird keine Ausgabe angezeigt. Bei einem Fehler wird eine Fehlermeldung angezeigt.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu den [Firewallregeln einer Azure Database for MySQL-Serverinstanz](./concepts-firewall-rules.md).
- [Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)

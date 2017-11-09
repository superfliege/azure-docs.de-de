---
title: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe der Azure CLI | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure CLI-Befehlszeile Firewallregeln für Azure-Datenbank für MySQL erstellen und verwalten."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 66d192287eeaaaa82c0f61f8aa13b8bf7bf8cd47
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe der Azure CLI
Mithilfe von Firewallregeln auf Serverebene können Administratoren den Zugriff auf eine Azure Database for MySQL-Serverinstanz über eine bestimmte IP-Adresse oder über einen IP-Adressbereich verwalten. Dank praktischer Azure CLI-Befehle können Sie zum Verwalten Ihres Servers Firewallregeln erstellen, aktualisieren, löschen, auflisten und anzeigen. Eine Übersicht über Firewalls für Azure-Datenbank für MySQL finden Sie unter [Azure Database for MySQL server firewall rules](./concepts-firewall-rules.md) (Firewallregeln des Servers für Azure-Datenbank für MySQL).

## <a name="prerequisites"></a>Voraussetzungen
* [Installieren der Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Installieren des Azure Python SDK für den PostgreSQL- und den MySQL-Dienst
* Installieren Sie Azure CLI-Komponente für den PostgreSQL- und den MySQL-Dienst.
* Erstellen eines Azure-Datenbank für MySQL-Servers

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

4. Nachdem Ihre Anmeldung autorisiert wurde, wird eine Liste der Abonnements in der Konsole ausgegeben. Kopieren Sie die ID des gewünschten Abonnements, um das aktuelle Abonnement festzulegen, das verwendet werden soll.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Listen Sie die Server für Azure-Datenbanken für MySQL für Ihr Abonnement und die Ressourcengruppe auf, wenn Sie sich in Bezug auf die Namen unsicher sind.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Notieren Sie das Attribut „Name“ in der Auflistung. Mit diesem Attribut wird angegeben, welche Serverinstanz von MySQL verwendet werden soll. Bestätigen Sie ggf. die Details für diesen Server, um mithilfe des Attributs „Name“ die Richtigkeit des Namens zu überprüfen:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Auflisten der Firewallregeln für die Azure Database for MySQL-Instanz 
Listen Sie mithilfe des Servernamens und des Ressourcengruppennamens die vorhandenen Firewallregeln auf dem Server auf. Beachten Sie, dass das Attribut mit dem Servernamen im Switch **--server** und nicht im Switch**--name** angegeben ist.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
Die Ausgabe listet die Regeln (sofern vorhanden) standardmäßig im JSON-Format auf. Sie können mit dem Switch **--output table** Ergebnisse in einem besser lesbaren Tabellenformat ausgeben.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Erstellen einer Firewallregel für die Azure Database for MySQL-Serverinstanz
Erstellen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine neue Firewallregel auf dem Server. Geben Sie einen Namen für die Regel sowie die Start-IP- und End-IP-Adresse ein, um einen Bereich von IP-Adressen anzugeben, denen der Zugriff gewährt wird.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Damit nur einer einzelnen IP-Adresse Zugriff gewährt wird, geben Sie wie in diesem Beispiel dieselbe Adresse als Start-IP- und End-IP-Adresse an.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen erstellten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>Aktualisieren einer Firewallregel für die Azure Database for MySQL-Serverinstanz 
Aktualisieren Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine vorhandene Firewallregel auf dem Server. Geben Sie den Namen der vorhandenen Firewallregel als Eingabe sowie die zu aktualisierenden Attribute für die Start-IP- und End-IP-Adresse ein.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen aktualisierten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

> [!NOTE]
> Falls die Firewallregel nicht vorhanden ist, wird sie vom Befehl „Update“ erstellt.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Anzeigen von Firewallregeldetails für die Azure Database for MySQL-Serverinstanz
Zeigen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens die Details der vorhandenen Firewallregel auf dem Server an. Geben Sie den Namen der vorhandenen Firewallregel als Eingabe an.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen angegebenen Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>Löschen einer Firewallregel für die Azure Database for MySQL-Serverinstanz
Entfernen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine vorhandene Firewallregel vom Server. Geben Sie den Namen der vorhandenen Firewallregel an.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Bei erfolgreicher Ausführung wird keine Ausgabe angezeigt. Bei einem Fehler wird eine Fehlermeldung angezeigt.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu den [Firewallregeln einer Azure Database for MySQL-Serverinstanz](./concepts-firewall-rules.md).
- [Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)

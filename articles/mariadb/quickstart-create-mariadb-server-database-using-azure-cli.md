---
title: 'Schnellstart: Erstellung eines Azure Database for MariaDB-Servers – Azure-Befehlszeilenschnittstelle'
description: Dieser Schnellstart beschreibt die Verwendung der Azure-Befehlszeilenschnittstelle zum Erstellen eines Azure Database for MariaDB-Servers in einer Azure-Ressourcengruppe.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 9e87dacb80aa7fc5f5b073e631fc06ae74b3ad00
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996603"
---
# <a name="create-an-azure-database-for-mariadb-server-using-azure-cli"></a>Erstellen eines Servers für Azure Database for MariaDB mithilfe der Azure-Befehlszeilenschnittstelle
Dieser Schnellstart beschreibt die Verwendung der Azure-Befehlszeilenschnittstelle zum Erstellen eines Azure Database for MariaDB-Servers in einer Azure-Ressourcengruppe in etwa fünf Minuten. Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource vorhanden ist oder in Rechnung gestellt wird. Wählen Sie eine bestimmte Abonnement-ID unter Ihrem Konto mit dem Befehl [az account set](/cli/azure/account#az-account-set) aus.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myresourcegroup` am Standort `westus` erstellt.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mariadb-server"></a>Erstellen eines Azure Database for MariaDB-Servers
Erstellen Sie mit dem Befehl **[az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create)** einen Azure Database for MariaDB-Server. Ein Server kann mehrere Datenbanken verwalten. In der Regel wird für jedes Projekt oder jeden Benutzer eine separate Datenbank verwendet.

**Einstellung** | **Beispielwert** | **Beschreibung**
---|---|---
name | mydemoserver | Wählen Sie einen eindeutigen Namen aus, der Ihren Azure Database for MariaDB-Server identifiziert. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Es muss zwischen drei und 63 Zeichen lang sein.
resource-group | myresourcegroup | Geben Sie den Namen der Azure-Ressourcengruppe an.
sku-name | GP_Gen5_2 | Der Name der SKU. Folgt der Konvention „{Tarif}_{Computegeneration}_{virtuelle Kerne}“ in Kurzform. Unter dieser Tabelle finden Sie weitere Informationen zum sku-name-Parameter.
backup-retention | 7 | Gibt die Aufbewahrungsdauer für eine Sicherung an. Die Einheit ist Tage. Der Bereich umfasst 7 bis 35. 
geo-redundant-backup | Deaktiviert | Gibt an, ob georedundante Sicherungen für diesen Server aktiviert werden sollen. Zulässige Werte: Enabled, Disabled.
location | westus | Der Azure-Standort für den Server.
ssl-enforcement | Aktiviert | Gibt an, ob SSL für diesen Server aktiviert werden soll. Zulässige Werte: Enabled, Disabled.
storage-size | 51200 | Die Speicherkapazität des Servers (Einheit: MB). Die kleinste gültige Speichergröße ist 5.120 MB, die in Schritten von 1.024 MB erhöht wird. Im Dokument [Tarife](./concepts-pricing-tiers.md) finden Sie weitere Informationen zu den Grenzwerten bei der Speichergröße. 
Version | 10.2 | Die Hauptversion der MariaDB-Engine.
admin-user | myadmin | Der Benutzername für die Administratoranmeldung. Diese darf nicht **azure_superuser**, **admin**, **administrator**, **root**, **guest** oder **public** lauten.
admin-password | Password123 | Das Kennwort des Administratorbenutzers. Es muss zwischen acht und 128 Zeichen lang sein. Ihr Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: englische Großbuchstaben, englische Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen.

Für den Parameterwert des SKU-Namens wird die Konvention „{Tarif}\_{Computegeneration}\_{V-Kerne}“ verwendet, wie in den folgenden Beispielen:
+ `--sku-name B_Gen5_4` ist „Basic“, „Gen 5“ und „4 V-Kerne“ zugeordnet.
+ `--sku-name GP_Gen5_32` ist „Universell“, „Gen 5“ und „32 V-Kerne“ zugeordnet.
+ `--sku-name MO_Gen5_2` ist „Arbeitsspeicheroptimiert“, „Gen 5“ und „2 V-Kerne“ zugeordnet.

Informationen zu den gültigen Werten pro Region und Tarif finden Sie in der Dokumentation zu [Tarifen](./concepts-pricing-tiers.md).

Im folgenden Beispiel wird in der Region „USA, Westen“ der Server `mydemoserver` in der Ressourcengruppe `myresourcegroup` mit dem Serveradministrator-Anmeldenamen `myadmin` erstellt. Dies ist ein **Gen 5**-Server vom Typ **Universell** mit zwei **virtuellen Kernen**. Der Name eines Servers wird dem DNS-Namen zugeordnet und muss deshalb in Azure global eindeutig sein. Ersetzen Sie das `<server_admin_password>` durch einen eigenen Wert.
```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.2
```

## <a name="configure-firewall-rule"></a>Konfigurieren der Firewallregel
Erstellen Sie mit dem Befehl **[az mariadb server firewall-rule create](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create)** eine Azure Database for MariaDB-Firewallregel auf Serverebene. Eine Firewallregel auf Serverebene ermöglicht einer externen Anwendung, wie z.B. dem Befehlszeilentool **mysql** oder MySQL Workbench, über die Firewall des Azure-MariaDB-Diensts eine Verbindung mit Ihrem Server herzustellen. 

Das folgende Beispiel erstellt eine Firewallregel namens `AllowMyIP`, die Verbindungen über eine bestimmte IP-Adresse (192.168.0.1) zulässt. Fügen Sie die IP-Adresse oder den IP-Adressbereich des Orts ein, von dem Sie eine Verbindung herstellen möchten. 

```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> Die Kommunikation für Verbindungen mit Azure Database for MariaDB erfolgt über Port 3306. Wenn Sie versuchen, eine Verbindung über ein Unternehmensnetzwerk herzustellen, wird ausgehender Datenverkehr über Port 3306 unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Server herstellen, wenn Ihre IT-Abteilung Port 3306 öffnet.
> 

## <a name="configure-ssl-settings"></a>Konfigurieren der SSL-Einstellungen
Standardmäßig werden SSL-Verbindungen zwischen Ihrem Server und Clientanwendungen erzwungen. Mit dieser Standardeinstellung wird die Sicherheit von Daten während des Betriebs gewährleistet, indem der Datenstrom über das Internet verschlüsselt wird. Um diesen Schnellstart zu vereinfachen, ist es ratsam, die SSL-Verbindungen für Ihren Server zu deaktivieren. Das Deaktivieren von SSL ist für Produktionsserver aber nicht zu empfehlen. Weitere Informationen finden Sie unter [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit Azure Database for MariaDB](./howto-configure-ssl.md).

Im folgenden Beispiel wird das Erzwingen von SSL auf Ihrem MariaDB-Server deaktiviert.
 
 ```azurecli-interactive
 az mariadb server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Zum Herstellen einer Verbindung zum Server müssen Sie Hostinformationen und Anmeldeinformationen für den Zugriff angeben.

```azurecli-interactive
az mariadb server show --resource-group myresourcegroup --name mydemoserver
```

Das Ergebnis liegt im JSON-Format vor. Notieren Sie sich die Werte für **fullyQualifiedDomainName** und **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mariadb.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMariaDB/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMariaDB/servers",
  "userVisibleState": "Ready",
  "version": "10.2"
}
```

## <a name="connect-to-server-using-mysql-command-line"></a>Herstellen einer Verbindung mit dem Server mit dem Befehlszeilentool mysql
Stellen Sie mit dem Befehlszeilentool **mysql** eine Serververbindung her. Sie können das Befehlszeilentool [hier](https://dev.mysql.com/downloads/) herunterladen und auf Ihrem Computer installieren. Stattdessen können Sie auch in den Codebeispielen auf die Schaltfläche **Ausprobieren** oder im Azure-Portal auf der Symbolleiste ganz oben rechts auf die Schaltfläche `>_` klicken und **Azure Cloud Shell** öffnen.

Geben Sie die nächsten Befehle ein: 

1. Stellen Sie eine Verbindung zum Server mit dem Befehlszeilentool **mysql** her:
```azurecli-interactive
 mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

2. Zeigen Sie den Serverstatus an der Eingabeaufforderung „mysql>“ an:
```sql
status
```
Wenn alles funktioniert, sollte das Befehlszeilentool den folgenden Text ausgeben:

```bash
C:\Users\>mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.39.0 MariaDB Server

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64)

Connection id:          64681
Current database:
Current user:           myadmin@40.118.201.21
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MariaDB Server
Protocol version:       10
Connection:             mydemoserver.mariadb.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 1 day 3 hours 28 min 50 sec

Threads: 10  Questions: 29002  Slow queries: 0  Opens: 33  Flush tables: 3  Open tables: 1  Queries per second avg: 0.293
--------------

mysql>
```

> [!TIP]
> Zusätzliche Befehle finden Sie unter [MySQL 5.7-Referenzhandbuch – Kapitel 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-to-server-using-mysql-workbench"></a>Herstellen einer Verbindung mit dem Server unter Verwendung von MySQL Workbench
1.  Starten Sie die Anwendung MySQL Workbench auf dem Clientcomputer. Sie können MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) herunterladen und installieren.

2.  Geben Sie im Dialogfeld **Setup New Connection** (Neue Verbindung einrichten) auf der Registerkarte **Parameter** folgende Informationen ein:

   ![Einrichten einer neuen Verbindung](./media/quickstart-create-mariadb-server-database-using-azure-cli/setup-new-connection.png)

| **Einstellung** | **Empfohlener Wert** | **Beschreibung** |
|---|---|---|
|   Verbindungsname | Eigene Verbindung | Geben Sie eine beliebige Bezeichnung für diese Verbindung ein. |
| Verbindungsmethode | Wählen Sie „Standard (TCP/IP)“ aus | Verwenden des TCP/IP-Protokolls zum Herstellen einer Verbindung mit Azure Database for MariaDB |
| Hostname | mydemoserver.mariadb.database.azure.com | Der zuvor notierte Servername. |
| Port | 3306 | Der Standardport für MariaDB wird verwendet. |
| Username | myadmin@mydemoserver | Die zuvor notierte Serveradministratoranmeldung. |
| Kennwort | **** | Verwenden Sie das Administratorkontokennwort, das Sie zuvor konfiguriert haben. |

Klicken Sie auf **Verbindung testen**, um zu testen, ob alle Parameter richtig konfiguriert wurden.
Nun können Sie auf die Verbindung klicken, um eine Verbindung mit dem Server herzustellen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie diese Ressourcen nicht für einen anderen Schnellstart bzw. ein anderes Tutorial benötigen, können Sie sie mit dem folgenden Befehl löschen: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Wenn Sie nur den neu erstellten Server löschen möchten, können Sie den Befehl **[az mariadb server delete](/cli/azure/mariadb/server#az-mariadb-server-delete)** ausführen.
```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

<!--
## Next steps

> [!div class="nextstepaction"]
> [Design a MariaDB Database with Azure CLI](./tutorial-design-database-using-cli.md)
-->
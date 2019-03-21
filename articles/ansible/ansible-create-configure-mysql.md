---
title: Erstellen und Konfigurieren eines Azure Database for MySQL-Servers mit Ansible
description: Hier wird beschrieben, wie Sie mithilfe von Ansible einen Azure Database for MySQL-Server erstellen und konfigurieren.
ms.service: azure
keywords: Ansible, Azure, DevOps, Bash, Playbook, MySQL, Datenbank
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/23/2018
ms.openlocfilehash: 63472cf9c4b6b16f74ececfb6c6e61cf5f89ff9d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58095397"
---
# <a name="create-and-configure-an-azure-database-for-mysql-server-by-using-ansible"></a>Erstellen und Konfigurieren eines Azure Database for MySQL-Servers mit Ansible
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) ist ein verwalteter Dienst, mit dem Sie hoch verfügbare MySQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. 

In dieser Schnellstartanleitung wird veranschaulicht, wie Sie mit Ansible einen Azure Database for MySQL-Server erstellen und die zugehörige Firewallregel konfigurieren. Über das Azure-Portal können Sie diese Aufgaben in etwa fünf Minuten ausführen.

## <a name="prerequisites"></a>Voraussetzungen
- **Azure-Abonnement:** Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Für die Ausführung der folgenden Beispielplaybooks in diesem Tutorial ist Ansible 2.7 erforderlich. 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.  

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen **myResourceGroup** am Standort **eastus** erstellt:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Speichern Sie das vorherige Playbook unter **rg.yml**. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook rg.yml
```

## <a name="create-a-mysql-server-and-database"></a>Erstellen eines MySQL-Servers und einer Datenbank
Im folgenden Beispiel werden ein MySQL-Server mit dem Namen **mysqlserveransible** und eine Azure Database for MySQL-Instanz mit dem Namen **mysqldbansible** erstellt. Dies ist ein Gen 5-Server vom Typ „Basic“ mit einem virtuellen Kern. 

Der Wert von **mysqlserver_name** muss eindeutig sein. Informationen zu den gültigen Werten pro Region und Tarif finden Sie in der Dokumentation zu [Tarifen](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers). Ersetzen Sie `<server_admin_password>` durch ein Kennwort.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus
    mysqlserver_name: mysqlserveransible
    mysqldb_name: mysqldbansible
    admin_username: mysqladmin
    admin_password: <server_admin_password> 
  tasks:
    - name: Create MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        sku:
          name: B_Gen5_1
          tier: Basic
        location: "{{ location }}"
        version: 5.6
        enforce_ssl: True
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        storage_mb: 51200
    - name: Create instance of MySQL Database
      azure_rm_mysqldatabase:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
        name: "{{ mysqldb_name }}"
```

Speichern Sie das vorherige Playbook unter **mysql_create.yml**. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-a-firewall-rule"></a>Konfigurieren einer Firewallregel
Eine Firewallregel auf Serverebene ermöglicht es einer externen Anwendung, über die Firewall des Azure MySQL-Diensts eine Verbindung mit Ihrem Server herzustellen. Ein Beispiel für eine externe Anwendung ist das Befehlszeilentool **mysql** oder MySQL Workbench.
Im folgenden Beispiel wird eine Firewallregel mit dem Namen **externalaccess** erstellt, die Verbindungen über beliebige externe IP-Adressen zulässt. 

Geben Sie für **startIpAddress** und **endIpAddress** Ihre eigenen Werte ein. Verwenden Sie den IP-Adressbereich des Orts, von dem Sie eine Verbindung herstellen möchten. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
  - name: Open firewall to access MySQL Server from outside
    azure_rm_resource:
      api_version: '2017-12-01'
      resource_group: "{{ resource_group }}"
      provider: dbformysql
      resource_type: servers
      resource_name: "{{ mysqlserver_name }}"
      subresource:
        - type: firewallrules
          name: externalaccess
      body:
        properties:
          startIpAddress: "0.0.0.0"
          endIpAddress: "255.255.255.255"
```

> [!NOTE]
> Die Kommunikation für Verbindungen mit Azure-Datenbank für MySQL erfolgt über Port 3306. Wenn Sie versuchen, eine Verbindung über ein Unternehmensnetzwerk herzustellen, wird ausgehender Datenverkehr über Port 3306 unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Server herstellen, wenn Ihre IT-Abteilung Port 3306 öffnet.
> 

Hier wird das Modul **azure_rm_resource** für diese Aufgabe verwendet. Es ermöglicht die direkte Verwendung der REST-API.

Speichern Sie das vorherige Playbook unter **mysql_firewall.yml**. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server-by-using-the-command-line-tool"></a>Herstellen einer Verbindung mit dem Server mit dem Befehlszeilentool
Sie können [MySQL herunterladen](https://dev.mysql.com/downloads/) und auf Ihrem Computer installieren. Stattdessen können Sie in den Codebeispielen auf die Schaltfläche **Ausprobieren** oder im Azure-Portal auf der Symbolleiste ganz oben rechts auf die Schaltfläche **>_** klicken und **Azure Cloud Shell** öffnen.

Geben Sie die nächsten Befehle ein: 

1. Herstellen einer Verbindung mit dem Server mit dem Befehlszeilentool **mysql**
   ```azurecli-interactive
   mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
   ```

2. Anzeigen des Serverstatus:
   ```sql
   mysql> status
   ```

Wenn alles funktioniert, sollte das Befehlszeilentool den folgenden Text ausgeben:

```
demo@Azure:~$ mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65233
Server version: 5.6.39.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64) using  EditLine wrapper

Connection id:          65233
Current database:
Current user:           mysqladmin@13.76.42.93
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mysqlserveransible.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 36 min 21 sec

Threads: 5  Questions: 559  Slow queries: 0  Opens: 96  Flush tables: 3  Open tables: 10  Queries per second avg: 0.256
--------------
```

## <a name="using-facts-to-query-mysql-servers"></a>Verwenden von Fakten zum Abfragen von MySQL-Servern
Im folgenden Beispiel werden MySQL-Server in **myResourceGroup** und anschließend alle Datenbanken auf dem Server abgefragt:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Query MySQL Servers in current resource group
      azure_rm_mysqlserver_facts:
        resource_group: "{{ resource_group }}"
      register: mysqlserverfacts

    - name: Dump MySQL Server facts
      debug:
        var: mysqlserverfacts

    - name: Query MySQL Databases
      azure_rm_mysqldatabase_facts:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
      register: mysqldatabasefacts

    - name: Dump MySQL Database Facts
      debug:
        var: mysqldatabasefacts
```

Speichern Sie das vorherige Playbook unter **mysql_query.yml**. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:

```bash
ansible-playbook mysql_query.yml
```

Die folgende Ausgabe für den MySQL-Server wird angezeigt: 
```json
"servers": [
    {
        "admin_username": "mysqladmin",
        "enforce_ssl": false,
        "fully_qualified_domain_name": "mysqlserveransible.mysql.database.azure.com",
        "id": "/subscriptions/685ba005-af8d-4b04-8f16-a7bf38b2eb5a/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysqlserveransible",
        "location": "eastus",
        "name": "mysqlserveransible",
        "resource_group": "myResourceGroup",
        "sku": {
            "capacity": 1,
            "family": "Gen5",
            "name": "B_Gen5_1",
            "tier": "Basic"
        },
        "storage_mb": 5120,
        "user_visible_state": "Ready",
        "version": "5.6"
    }
]
```

Darüber hinaus wird die folgende Ausgabe für die MySQL-Datenbank angezeigt:
```json
"databases": [
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "information_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysql",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransibler"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysqldbansible",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "performance_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    }
]
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht benötigen, können Sie sie löschen, indem Sie das folgende Beispiel ausführen. Darin wird eine Ressourcengruppe mit dem Namen **myResourceGroup** gelöscht. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Speichern Sie das vorherige Playbook unter **rg_delete.yml**. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook rg_delete.yml
```

Wenn Sie nur den neu erstellten MySQL-Server löschen möchten, führen Sie das folgende Beispiel aus:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Delete MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        state: absent
```

Speichern Sie das vorherige Playbook unter **mysql_delete.yml**. Verwenden Sie den Befehl **ansible-playbook** wie folgt, um das Playbook auszuführen:
```bash
ansible-playbook mysql_delete.yml
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Ansible unter Azure](https://docs.microsoft.com/azure/ansible/)

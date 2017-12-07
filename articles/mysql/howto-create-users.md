---
title: Erstellen von Benutzern auf einem Azure Database for MySQL-Server | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie neue Benutzerkonten für die Interaktion mit einem Azure Database for MySQL-Server erstellen können."
services: mysql
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: 8adb74e11570ac60ad3b898b737cff4699f2bbf1
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="create-users-in-azure-database-for-mysql-server"></a>Erstellen von Benutzern auf einem Azure Database for MySQL-Server 
In diesem Artikel wird beschrieben, wie Sie Benutzer auf einem Azure Database for MySQL-Server erstellen können.

Bei der anfänglichen Erstellung Ihrer Azure Database for MySQL-Instanz haben Sie einen Serveradministrator-Benutzernamen und das dazugehörige Kennwort angegeben. Weitere Informationen erhalten Sie in der [Schnellstartanleitung](quickstart-create-mysql-server-database-using-azure-portal.md). Sie können Ihren Serveradministrator-Benutzernamen im Azure-Portal ermitteln.

Der Serveradministrator verfügt über die folgenden Berechtigungen für Ihren Server: SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER.

Nach der Erstellung des Azure Database for MySQL-Servers können Sie das erste Serveradministrator-Benutzerkonto verwenden, um weitere Benutzer zu erstellen und ihnen Administratorzugriff zu gewähren. Außerdem kann das Serveradministratorkonto genutzt werden, um Benutzer mit weniger Berechtigungen zu erstellen, die Zugriff auf einzelne Datenbankschemas haben.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>Erstellen von zusätzlichen Administratorbenutzern in Azure Database for MySQL
1. Beschaffen Sie die Verbindungsinformationen und den Administratorbenutzernamen.
   Für die Verbindungsherstellung mit Ihrem Datenbankserver benötigen Sie den vollständigen Servernamen und die Administratoranmeldeinformationen. Der Servername und die Anmeldeinformationen sind auf der Seite **Übersicht** des Servers oder auf der Seite **Eigenschaften** im Azure-Portal leicht zu finden. 

2. Verwenden Sie das Administratorkonto und das Kennwort zum Herstellen einer Verbindung mit Ihrem Datenbankserver. Nutzen Sie Ihr bevorzugtes Clienttool, z.B. MySQL Workbench, mysql.exe, HeidiSQL oder andere. 
   Wenn Sie unsicher sind, wie Sie die Verbindung herstellen sollen, helfen Ihnen die Informationen unter [Verwenden von MySQL Workbench zum Verbinden und Abfragen von Daten](./connect-workbench.md) weiter.

3. Bearbeiten Sie den folgenden SQL-Code, und führen Sie ihn aus. Fügen Sie Ihren neuen Benutzernamen anstelle des Platzhalterwerts `new_master_user` ein. Mit dieser Syntax werden die aufgeführten Berechtigungen aller Datenbankschemas (*.*) für den Benutzernamen (hier: new_master_user) gewährt. 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. Überprüfen der gewährten Berechtigungen 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="how-to-create-database-users-in-azure-database-for-mysql"></a>Erstellen von Datenbankbenutzern in Azure Database for MySQL

1. Beschaffen Sie die Verbindungsinformationen und den Administratorbenutzernamen.
   Für die Verbindungsherstellung mit Ihrem Datenbankserver benötigen Sie den vollständigen Servernamen und die Administratoranmeldeinformationen. Der Servername und die Anmeldeinformationen sind auf der Seite **Übersicht** des Servers oder auf der Seite **Eigenschaften** im Azure-Portal leicht zu finden. 

2. Verwenden Sie das Administratorkonto und das Kennwort zum Herstellen einer Verbindung mit Ihrem Datenbankserver. Nutzen Sie Ihr bevorzugtes Clienttool, z.B. MySQL Workbench, mysql.exe, HeidiSQL oder andere. 
   Wenn Sie unsicher sind, wie Sie die Verbindung herstellen sollen, helfen Ihnen die Informationen unter [Verwenden von MySQL Workbench zum Verbinden und Abfragen von Daten](./connect-workbench.md) weiter.

3. Bearbeiten Sie den folgenden SQL-Code, und führen Sie ihn aus. Ersetzen Sie den Platzhalterwert `db_user` durch den gewünschten neuen Benutzernamen und den Platzhalterwert `testdb` durch Ihren eigenen Datenbanknamen.

   Mit dieser SQL-Codesyntax wird zu Beispielzwecken eine neue Datenbank mit dem Namen testdb erstellt. Anschließend wird im MySQL-Dienst ein neuer Benutzer erstellt, dem dann alle Berechtigungen für das neue Datenbankschema (testdb.\*) gewährt werden. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Überprüfen Sie die gewährten Berechtigungen in der Datenbank.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Melden Sie sich am Server an, und geben Sie die gewünschte Datenbank an, indem Sie den neuen Benutzernamen und das Kennwort verwenden. In diesem Beispiel wird die mysql-Befehlszeile angezeigt. Bei diesem Befehl werden Sie zum Eingeben des Kennworts für den Benutzernamen aufgefordert. Fügen Sie Ihren eigenen Servernamen, Datenbanknamen und Benutzernamen ein.

   ```azurecli-interactive
   mysql --host myserver4demo.mysql.database.azure.com --database testdb --user db_user@myserver4demo -p
   ```

## <a name="next-steps"></a>Nächste Schritte
Öffnen Sie die Firewall für die IP-Adressen, die den Computern der neuen Benutzer zugewiesen sind, damit diese eine Verbindung herstellen können: [Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe des Azure-Portals](howto-manage-firewall-using-portal.md) oder [Azure CLI](howto-manage-firewall-using-cli.md).

Weitere Informationen zur Verwaltung von Benutzerkonten finden Sie in der MySQL-Produktdokumentation in den Abschnitten zu den Themen [Benutzerkontenverwaltung](https://dev.mysql.com/doc/refman/5.7/en/user-account-management.html), [GRANT-Syntax](https://dev.mysql.com/doc/refman/5.7/en/grant.html) und [Berechtigungen](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html).

---
title: Erstellen von Benutzern auf einem Azure Database for PostgreSQL-Server
description: In diesem Artikel wird beschrieben, wie Sie neue Benutzerkonten für die Interaktion mit einem Azure Database for PostgreSQL-Server erstellen können.
services: postgresql
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 87a73929185112190d5dd6698e014db225ebc08e
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
ms.locfileid: "29574183"
---
# <a name="create-users-in-azure-database-for-postgresql-server"></a>Erstellen von Benutzern auf einem Azure Database for PostgreSQL-Server 
In diesem Artikel wird beschrieben, wie Sie Benutzer auf einem Azure Database for PostgreSQL-Server erstellen können.

## <a name="the-server-admin-account"></a>Das Serveradministratorkonto
Bei der anfänglichen Erstellung Ihrer Azure Database for PostgreSQL-Instanz haben Sie einen Serveradministrator-Benutzernamen und das dazugehörige Kennwort angegeben. Weitere Informationen sowie eine Erläuterung der einzelnen Schritte finden Sie in der [Schnellstartanleitung](quickstart-create-server-database-portal.md). Da es sich beim Serveradministrator-Benutzernamen um einen benutzerdefinierten Namen handelt, können Sie den ausgewählten Serveradministrator-Benutzernamen im Azure-Portal ermitteln.

Der Azure Database for PostgreSQL-Server wird erstellt und verfügt über drei definierte Standardrollen. Sie können diese Rollen durch Ausführung des folgenden Befehls anzeigen: `SELECT rolname FROM pg_roles;`.
- azure_pg_admin
- azure_superuser
- Ihr Serveradministratorbenutzer

Der Serveradministratorbenutzer ist ein Mitglied der Rolle „azure_pg_admin“. Das Serveradministratorkonto ist jedoch nicht Mitglied der Rolle „azure_superuser“. Da dieser Dienst ein verwalteter PaaS-Dienst ist, ist nur Microsoft Mitglied der Administratorrolle. 

Die PostgreSQL-Engine nutzt Berechtigungen zum Steuern des Zugriffs auf Datenbankobjekte, wie in der [PostgreSQL-Produktdokumentation](https://www.postgresql.org/docs/current/static/sql-createrole.html) erläutert. In Azure Database for PostgreSQL werden dem Serveradministratorbenutzer die folgenden Berechtigungen gewährt: LOGIN, NOSUPERUSER, INHERIT, CREATEDB, CREATEROLE, NOREPLICATION.

Mit dem Konto des Serveradministratorbenutzers können zusätzliche Benutzer erstellt und der Rolle „azure_pg_admin“ zugeordnet werden. Außerdem kann das Serveradministratorkonto genutzt werden, um Benutzer und Rollen mit weniger Berechtigungen zu erstellen, die Zugriff auf einzelne Datenbanken und Schemas haben.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-postgresql"></a>Erstellen von zusätzlichen Administratorbenutzern in Azure Database for PostgreSQL
1. Beschaffen Sie die Verbindungsinformationen und den Administratorbenutzernamen.
   Für die Verbindungsherstellung mit Ihrem Datenbankserver benötigen Sie den vollständigen Servernamen und die Administratoranmeldeinformationen. Der Servername und die Anmeldeinformationen sind auf der Seite **Übersicht** des Servers oder auf der Seite **Eigenschaften** im Azure-Portal leicht zu finden. 

2. Verwenden Sie das Administratorkonto und das Kennwort zum Herstellen einer Verbindung mit Ihrem Datenbankserver. Verwenden Sie Ihr bevorzugtes Clienttool, etwa pgAdmin oder psql.
   Wenn Sie nicht sicher sind, wie Sie eine Verbindung herstellen können, sehen Sie sich die Informationen unter [Herstellen einer Verbindung mit der PostgreSQL-Datenbank mithilfe von psql in Cloud Shell](./quickstart-create-server-database-portal.md#connect-to-the-postgresql-database-by-using-psql-in-cloud-shell) an.

3. Bearbeiten Sie den folgenden SQL-Code, und führen Sie ihn aus. Fügen Sie für den Platzhalterwert „<new_user>“ den neuen Benutzernamen und für das Platzhalterkennwort Ihr eigenes sicheres Kennwort ein. 

   ```sql
   CREATE ROLE <new_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB CREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT azure_pg_admin TO <new_user>;
   ```

## <a name="how-to-create-database-users-in-azure-database-for-postgresql"></a>Erstellen von Datenbankbenutzern in Azure Database for PostgreSQL

1. Beschaffen Sie die Verbindungsinformationen und den Administratorbenutzernamen.
   Für die Verbindungsherstellung mit Ihrem Datenbankserver benötigen Sie den vollständigen Servernamen und die Administratoranmeldeinformationen. Der Servername und die Anmeldeinformationen sind auf der Seite **Übersicht** des Servers oder auf der Seite **Eigenschaften** im Azure-Portal leicht zu finden. 

2. Verwenden Sie das Administratorkonto und das Kennwort zum Herstellen einer Verbindung mit Ihrem Datenbankserver. Verwenden Sie Ihr bevorzugtes Clienttool, etwa pgAdmin oder psql.

3. Bearbeiten Sie den folgenden SQL-Code, und führen Sie ihn aus. Ersetzen Sie den Platzhalterwert `<db_user>` durch den gewünschten neuen Benutzernamen und den Platzhalterwert `<newdb>` durch Ihren eigenen Datenbanknamen. Ersetzen Sie das Platzhalterkennwort durch Ihr eigenes sicheres Kennwort. 

   Mit dieser SQL-Codesyntax wird zu Beispielzwecken eine neue Datenbank mit dem Namen „testdb2 erstellt. Anschließend wird im PostgreSQL-Dienst ein neuer Benutzer erstellt, dem dann Berechtigungen zum Herstellen einer Verbindung mit der neuen Datenbank gewährt werden. 

   ```sql
   CREATE DATABASE <newdb>;
   
   CREATE ROLE <db_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT CONNECT ON DATABASE testdb TO <db_user>;
   ```

4. Bei der Verwendung eines Administratorkontos müssen Sie unter Umständen zusätzliche Berechtigungen erteilen, um die Objekte in der Datenbank zu schützen. Weitere Einzelheiten zu Datenbankrollen und Berechtigungen finden Sie in der [PostgreSQL-Dokumentation](https://www.postgresql.org/docs/current/static/ddl-priv.html). Beispiel:  
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <newdb> TO <db_user>;
   ```

5. Melden Sie sich beim Server an, und geben Sie unter Verwendung des neuen Benutzernamens und des Kennworts die gewünschte Datenbank an. In diesem Beispiel wird die psql-Befehlszeile gezeigt. Bei diesem Befehl werden Sie zum Eingeben des Kennworts für den Benutzernamen aufgefordert. Fügen Sie Ihren eigenen Servernamen, Datenbanknamen und Benutzernamen ein.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=db_user@mydemoserver --dbname=newdb
   ```

## <a name="next-steps"></a>Nächste Schritte
Öffnen Sie die Firewall für die IP-Adressen, die den Computern der neuen Benutzer zugewiesen sind, damit diese eine Verbindung herstellen können: [Erstellen und Verwalten von Firewallregeln für Azure Database for PostgreSQL mithilfe des Azure-Portals](howto-manage-firewall-using-portal.md) oder [Erstellen und Verwalten von Firewallregeln für Azure Database for PostgreSQL mithilfe der Azure CLI](howto-manage-firewall-using-cli.md).

Weitere Informationen zur Verwaltung von Benutzerkonten finden Sie in der PostgreSQL-Produktdokumentation in den Abschnitten zu den Themen [Datenbankrollen und Berechtigungen](https://www.postgresql.org/docs/current/static/user-manag.html), [GRANT-Syntax](https://www.postgresql.org/docs/current/static/sql-grant.html) und [Berechtigungen](https://www.postgresql.org/docs/current/static/ddl-priv.html).

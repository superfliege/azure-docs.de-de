---
title: Sichern und Wiederherstellen in Azure Database for PostgreSQL
description: Hier wird beschrieben, wie Sie eine PostgreSQL-Datenbank in eine Sicherungsdatei extrahieren und anhand einer mit pg_dump erstellten Datei in Azure Database for PostgreSQL wiederherstellen.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/19/2018
ms.openlocfilehash: 94d196ceecc0b63b9f0b0fe94f71363dc2086c30
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213649"
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migrieren der PostgreSQL-Datenbank durch Sichern und Wiederherstellen
Sie können mit [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) eine PostgreSQL-Datenbank in eine Sicherungsdatei extrahieren und mit [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) die PostgreSQL-Datenbank aus einer mit pg_dump erstellten Archivdatei wiederherstellen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen [Server für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-portal.md) mit Firewallregeln, um den Zugriff und eine Datenbank darunter zu ermöglichen
- Die Befehlszeilenprogramme [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) und [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) müssen installiert sein.

Führen Sie zum Sichern und Wiederherstellen der PostgreSQL-Datenbank diese Schritte aus:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Erstellen einer Sicherungsdatei mit pg_dump, die die zu ladenden Daten enthält
Um eine vorhandene PostgreSQL-Datenbank lokal oder auf einem virtuellen Computer zu sichern, führen Sie folgenden Befehl aus:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Beispiel: Lokaler Server und eine Datenbank namens **testdb**
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

> [!IMPORTANT]
> Kopieren Sie die Sicherungsdateien in einen Azure-Blob/-Speicher, und führen Sie die Wiederherstellung dort aus. Das sollte um einiges schneller sein als die Wiederherstellung über das Internet.
> 

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Wiederherstellen der Daten in der Azure-Zieldatenbank für PostgreSQL mithilfe von pg_restore
Nachdem Sie die Zieldatenbank erstellt haben, können Sie mithilfe des Befehls „pg_restore“ oder des Parameters „-d, --dbname“ die Daten in der Zieldatenbank anhand der Sicherungsdatei wiederherstellen.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Das Einschließen des Parameters „--no-owner“ bewirkt, dass sich alle Objekte, die während der Wiederherstellung erstellt wurden, im Besitz des mit „--username“ angegebenen Benutzers befinden. Weitere Informationen finden Sie in der offiziellen PostgreSQL-Dokumentation auf [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

> [!NOTE]
> Wenn Ihr PostgreSQL-Server SSL-Verbindungen erfordert (auf Azure Database for PostgreSQL-Servern standardmäßig aktiviert), legen Sie eine Umgebungsvariable `PGSSLMODE=require` fest, damit das pg_restore-Tool über SSL eine Verbindung herstellt. Erfolgt die Verbindung nicht über SSL, kann der Fehler möglicherweise wie folgt lauten: `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> Führen Sie in der Windows-Befehlszeile den Befehl `SET PGSSLMODE=require` aus, bevor Sie den Befehl „pg_restore“ ausführen. Führen Sie unter Linux oder Bash den Befehl `export PGSSLMODE=require` aus, bevor Sie den Befehl „pg_restore“ ausführen.
>

In diesem Beispiel stellen Sie die Daten aus der Sicherungsdatei **testdb.dump** in der Datenbank **mypgsqldb** auf dem Zielserver **mydemoserver.postgres.database.azure.com** wieder her. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Migrieren einer PostgreSQL-Datenbank mithilfe von Export und Import finden Sie unter [Migrieren der PostgreSQL-Datenbank mit Import und Export](howto-migrate-using-export-and-import.md).
- Weitere Informationen zum Migrieren von Datenbanken zu Azure Database for PostgreSQL finden Sie im [Leitfaden zur Datenbankmigration](http://aka.ms/datamigration).

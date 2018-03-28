---
title: Sichern und Wiederherstellen in Azure Database for PostgreSQL
description: Hier wird beschrieben, wie Sie eine PostgreSQL-Datenbank in eine Sicherungsdatei extrahieren und anhand einer mit pg_dump erstellten Datei in Azure Database for PostgreSQL wiederherstellen.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: f74c60cb99ee5bae1af8e000ebbd21b41600638d
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
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

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Wiederherstellen der Daten in der Azure-Zieldatenbank für PostgreSQL mithilfe von pg_restore
Nachdem Sie die Zieldatenbank erstellt haben, können Sie mithilfe des Befehls „pg_restore“ oder des Parameters „-d, --dbname“ die Daten in der Zieldatenbank anhand der Sicherungsdatei wiederherstellen.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Das Einschließen des Parameters „--no-owner“ bewirkt, dass sich alle Objekte, die während der Wiederherstellung erstellt wurden, im Besitz des mit „--username“ angegebenen Benutzers befinden. Weitere Informationen finden Sie in der offiziellen PostgreSQL-Dokumentation auf [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

In diesem Beispiel stellen Sie die Daten aus der Sicherungsdatei **testdb.dump** in der Datenbank **mypgsqldb** auf dem Zielserver **mydemoserver.postgres.database.azure.com** wieder her. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Migrieren einer PostgreSQL-Datenbank mithilfe von Export und Import finden Sie unter [Migrieren der PostgreSQL-Datenbank mit Import und Export](howto-migrate-using-export-and-import.md).
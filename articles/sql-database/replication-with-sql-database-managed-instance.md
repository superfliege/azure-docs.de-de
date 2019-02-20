---
title: Konfigurieren der Replikation in einer verwalteten Azure SQL-Datenbank-Instanzdatenbank | Microsoft-Dokumentation
description: Hier finden Sie Informationen zum Konfigurieren der Transaktionsreplikation in einer verwalteten Azure SQL-Datenbank-Instanzdatenbank.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: 038d8c919e68e68f886525a6c78139496edef8e1
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55893012"
---
# <a name="configure-replication-in-an-azure-sql-database-managed-instance-database"></a>Konfigurieren der Replikation in einer verwalteten Azure SQL-Datenbank-Instanzdatenbank

Die Transaktionsreplikation ermöglicht es Ihnen, Daten aus einer SQL Server-Datenbank oder einer anderen Instanzdatenbank in eine verwaltete Azure SQL-Datenbank-Instanzdatenbank zu replizieren. Sie können mithilfe der Transaktionsreplikation auch Änderungen, die in einer Instanzdatenbank einer verwalteten Azure SQL-Datenbank-Instanz vorgenommen wurden, in eine SQL Server-Datenbank, eine einzelne Datenbank in Azure SQL-Datenbank oder in eine in einem Pool zusammengefasste Datenbank eines Pools für elastische Azure SQL-Datenbanken pushen. Die Transaktionsreplikation steht als Public Preview in der [verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md) zur Verfügung. Eine verwaltete Instanz kann Verleger-, Verteiler- und Abonnentendatenbanken hosten. Unter [Konfigurationen für die Transaktionsreplikation](sql-database-managed-instance-transactional-replication.md#common-configurations) finden Sie die verfügbaren Konfigurationen.

## <a name="requirements"></a>Requirements (Anforderungen)

Zum Konfigurieren einer verwalteten Instanz, die als Verleger oder Verteiler fungieren soll, ist Folgendes erforderlich:

- Die verwaltete Instanz ist derzeit nicht an einer Georeplikationsbeziehung beteiligt.

   >[!NOTE]
   >Einzelne Datenbanken und in einem Pool zusammengefasste Datenbanken in Azure SQL-Datenbank können nur Abonnenten sein.

- Alle verwalteten Instanzen müssen sich in demselben VNET befinden.

- Für die Verbindung zwischen den Teilnehmern der Replikation wird SQL-Authentifizierung verwendet.

- Ein Azure-Speicherkonto für das Arbeitsverzeichnis für die Replikation.

- Port 445 (TCP ausgehend) muss in den Sicherheitsregeln des Subnetzes der verwalteten Instanz geöffnet sein, um auf die Azure-Dateifreigabe zugreifen zu können.

## <a name="features"></a>Features

Unterstützt:

- Mischung aus Transaktions- und Momentaufnahmereplikation von lokalen und verwalteten SQL Server-Instanzen in Azure SQL-Datenbank.
- Abonnenten können in lokalen SQL Server-Datenbanken, einzelnen Datenbanken in Azure SQL-Datenbank oder in einem Pool zusammengefassten Datenbanken von Pools für elastische Azure SQL-Datenbanken enthalten sein.
- Unidirektionale oder bidirektionale Replikation.

Folgende Features werden in einer verwalteten Instanz in Azure SLQ-Datenbank nicht unterstützt:

- Aktualisierbare Abonnements.
- Aktive Georeplikation.

## <a name="configure-publishing-and-distribution-example"></a>Konfigurieren eines Beispiels für Veröffentlichung und Verteilung

1. [Erstellen Sie eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-create-tutorial-portal.md) im Portal.
2. [Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) für das Arbeitsverzeichnis.

   Achten Sie darauf, die Speicherkontoschlüssel zu kopieren. Siehe [Anzeigen und Kopieren von Speicherzugriffsschlüssel](../storage/common/storage-account-manage.md#access-keys
).
3. Erstellen Sie eine Instanzdatenbank für den Verleger.

   Ersetzen Sie in den nachstehenden Beispielskripts `<Publishing_DB>` durch den Namen der Instanzdatenbank.

4. Erstellen Sie einen Datenbankbenutzer mit SQL-Authentifizierung für den Verteiler. Verwenden Sie ein sicheres Kennwort.

   Verwenden Sie in den folgenden Beispielskripts `<SQL_USER>` und `<PASSWORD>` als Datenbankbenutzer und Kennwort für dieses SQL Server-Konto.

5. [Stellen Sie eine Verbindung zur verwalteten SQL-Datenbank-Instanz her](sql-database-connect-query-ssms.md).

6. Führen Sie die folgende Abfrage aus, um den Verteiler und die Verteilungsdatenbank hinzuzufügen.

   ```sql
   USE [master]
   GO
   EXEC sp_adddistributor @distributor = @@ServerName;
   EXEC sp_adddistributiondb @database = N'distribution';
   ```

7. Aktualisieren Sie die folgende Abfrage, und führen Sie sie aus, um einen Verleger so zu konfigurieren, dass er eine bestimmte Verteilungsdatenbank verwendet.

   Ersetzen Sie `<SQL_USER>` und `<PASSWORD>` durch das SQL Server-Konto und -Kennwort.

   Ersetzen Sie `\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>` durch den Wert für Ihr Speicherkonto.  

   Ersetzen Sie `<STORAGE_CONNECTION_STRING>` durch die Verbindungszeichenfolge von der Registerkarte **Zugriffsschlüssel** Ihres Microsoft Azure Storage-Kontos.

   Nachdem Sie die folgende Abfrage aktualisiert haben, führen Sie sie aus.

   ```sql
   USE [master]
   EXEC sp_adddistpublisher @publisher = @@ServerName,
                @distribution_db = N'distribution',
                @security_mode = 0,
                @login = N'<SQL_USER>',
                @password = N'<PASSWORD>',
                @working_directory = N'\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>',
                @storage_connection_string = N'<STORAGE_CONNECTION_STRING>';
   GO
   ```

8. Konfigurieren Sie den Verleger für die Replikation.

    Ersetzen Sie in der folgenden Abfrage `<Publishing_DB>` durch den Namen Ihrer Verlegerdatenbank.

    Ersetzen Sie `<Publication_Name>` durch den Namen für die Veröffentlichung.

    Ersetzen Sie `<SQL_USER>` und `<PASSWORD>` durch das SQL Server-Konto und -Kennwort.

    Nachdem Sie die Abfrage aktualisiert haben, führen Sie sie aus, um die Veröffentlichung zu erstellen.

   ```sql
   USE [<Publishing_DB>]
   EXEC sp_replicationdboption @dbname = N'<Publishing_DB>',
                @optname = N'publish',
                @value = N'true';

   EXEC sp_addpublication @publication = N'<Publication_Name>',
                @status = N'active';

   EXEC sp_changelogreader_agent @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>';

   EXEC sp_addpublication_snapshot @publication = N'<Publication_Name>',
                @frequency_type = 1,
                @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   ```

9. Fügen Sie den Artikel, das Abonnement und den Pushabonnement-Agent hinzu.

   Aktualisieren Sie das folgende Skript, um diese Objekte hinzuzufügen.

   - Ersetzen Sie `<Object_Name>` durch den Namen des Objekts, das veröffentlicht werden soll.
   - Ersetzen Sie `<Object_Schema>` durch den Namen des Quellschemas.
   - Ersetzen Sie die anderen Parameter in spitzen Klammern `<>` entsprechend den Werten in den vorherigen Skripts.

   ```sql
   EXEC sp_addarticle @publication = N'<Publication_Name>',
                @type = N'logbased',
                @article = N'<Object_Name>',
                @source_object = N'<Object_Name>',
                @source_owner = N'<Object_Schema>'

   EXEC sp_addsubscription @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @destination_db = N'<Subscribing_DB>',
                @subscription_type = N'Push'

   EXEC sp_addpushsubscription_agent @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @subscriber_db = N'<Subscribing_DB>',
                @subscriber_security_mode = 0,
                @subscriber_login = N'<SQL_USER>',
                @subscriber_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   GO
   ```
   
## <a name="see-also"></a>Siehe auch

- [Transaktionsreplikation](sql-database-managed-instance-transactional-replication.md)
- [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md)

---
title: Konfigurieren der Replikation in verwalteter Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Hier finden Sie Informationen zum Konfigurieren der Transaktionsreplikation in einer verwalteten Azure SQL-Datenbank-Instanz.
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
ms.date: 01/25/2019
ms.openlocfilehash: b0188a0983ea18490f3997b857386e313daa58ed
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55467662"
---
# <a name="configure-replication-in-azure-sql-database-managed-instance"></a>Konfigurieren der Replikation in verwalteter Azure SQL-Datenbank-Instanz

Mit der Transaktionsreplikation können Sie Daten von SQL Server-Datenbanken oder Datenbanken einer verwalteten Azure SQL-Datenbank-Instanz in die verwaltete Instanz replizieren oder Änderungen, die Sie an Ihren Datenbanken in der verwalteten Instanz vornehmen, mithilfe von Push an andere SQL Server-Instanzen, eine Azure SQL-Datenbank-Einzeldatenbank oder einen Pool für elastische Datenbanken oder eine andere verwaltete Instanz übertragen. Die Replikation ist als Public Preview in der [verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md) verfügbar. Eine verwaltete Instanz kann Verleger-, Verteiler- und Abonnentendatenbanken hosten. Unter [Konfigurationen für die Transaktionsreplikation](sql-database-managed-instance-transactional-replication.md#common-configurations) finden Sie die verfügbaren Konfigurationen.

## <a name="requirements"></a>Requirements (Anforderungen)

Für Verleger und Verteiler in Azure SQL-Datenbank ist Folgendes erforderlich:

- Verwaltete Azure SQL-Datenbank-Instanz ohne georedundante Notfallwiederherstellungskonfiguration.

   >[!NOTE]
   >Azure SQL-Datenbanken, die nicht mit „Verwalteter Instanz“ konfiguriert werden, können nur Abonnenten sein.

- Alle Instanzen von SQL Server müssen sich im gleichen vNet befinden.

- Für die Verbindung zwischen den Teilnehmern der Replikation wird SQL-Authentifizierung verwendet.

- Ein Azure-Speicherkonto für das Arbeitsverzeichnis für die Replikation.

- Port 445 (TCP ausgehend) muss in den Sicherheitsregeln des Subnetzes der verwalteten Instanz geöffnet sein, um auf die Azure-Dateifreigabe zugreifen zu können.

## <a name="features"></a>Features

Unterstützt:

- Mischung aus Transaktions- und Momentaufnahmereplikation von lokalen und verwalteten Azure SQL-Datenbank-Instanzen.

- Abonnenten können lokale, Einzeldatenbanken in Azure SQL-Datenbank oder in Pools für elastische Datenbanken in Azure SQL-Datenbank zusammengefasste Datenbanken sein.

- Unidirektionale oder bidirektionale Replikation.

Folgende Funktionen werden nicht unterstützt:

- Aktualisierbare Abonnements.

- Aktive Georeplikation.

## <a name="configure-publishing-and-distribution-example"></a>Konfigurieren eines Beispiels für Veröffentlichung und Verteilung

1. [Erstellen Sie eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-create-tutorial-portal.md) im Azure-Portal.
2. [Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) für das Arbeitsverzeichnis.

   Achten Sie darauf, die Speicherkontoschlüssel zu kopieren. Siehe [Anzeigen und Kopieren von Speicherzugriffsschlüssel](../storage/common/storage-account-manage.md#access-keys
).
3. Erstellen Sie eine Datenbank für den Verleger.

   Ersetzen Sie in den folgenden Beispielskripts `<Publishing_DB>` durch den Namen der Datenbank.

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

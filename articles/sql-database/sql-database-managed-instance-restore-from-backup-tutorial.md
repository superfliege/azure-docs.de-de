---
title: Wiederherstellen einer Sicherung in einer verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Stellen Sie mithilfe von SSMS eine Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz wieder her.
keywords: Tutorial zu SQL-Datenbank, verwaltete SQL-Datenbank-Instanz, Sicherung wiederherstellen
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: 1d8db471a3f8ebf9dbcce77b101a2688f2570fd6
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949883"
---
# <a name="restore-a-database-backup-to-an-azure-sql-database-managed-instance"></a>Wiederherstellen einer Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz

In diesem Tutorial erfahren Sie anhand der Sicherungsdatei „Wide World Importers – Standard“, wie Sie eine in Azure Blob Storage gespeicherte Sicherung einer Datenbank in der verwalteten Instanz wiederherstellen. Die Vorgehensweise ist mit einer gewissen Ausfallzeit verbunden. Ein Tutorial mit Verwendung des Azure Database Migration Service (DMS) für die Migration finden Sie unter [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](../dms/tutorial-sql-server-to-managed-instance.md). Die verschiedenen Migrationsmethoden werden unter [Migration einer SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-migrate.md) behandelt.

> [!div class="checklist"]
> * Herunterladen der Sicherungsdatei „Wide World Importers – Standard“
> * Erstellen des Azure-Speicherkontos und Hochladen der Sicherungsdatei
> * Wiederherstellen der Wide World Importers-Datenbank aus einer Sicherungsdatei

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial basiert auf den Ressourcen, die im Tutorial [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz im Azure-Portal](sql-database-managed-instance-create-tutorial-portal.md) erstellt werden.

## <a name="download-the-wide-world-importers---standard-backup-file"></a>Herunterladen der Sicherungsdatei „Wide World Importers – Standard“

Verwenden Sie die folgenden Schritte, um die Sicherungsdatei „Wide World Importers – Standard“ herunterzuladen.

Geben Sie in Internet Explorer https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak in das Adressfeld für die URL ein, und klicken Sie nach entsprechender Aufforderung auf **Speichern**, um die Datei im Ordner **Downloads** zu speichern.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/#create/Microsoft.SQLManagedInstance)an.

## <a name="create-azure-storage-account-and-upload-backup-file"></a>Erstellen des Azure-Speicherkontos und Hochladen der Sicherungsdatei

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Klicken Sie unter **Speicher** auf **Speicherkonto**, um das Formular für Speicherkonten zu öffnen.

   ![Speicherkonto](./media/sql-database-managed-instance-tutorial/storage-account.png)

3. Füllen Sie das Formular für das Speicherkonto mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | Beschreibung |
   | ------ | --------------- | ----------- |
   |**Name**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Bereitstellungsmodell**|Ressourcenmodell||
   |**Kontoart**|Blob Storage||
   |**Leistung**|Standard oder Premium|Magnetbandlaufwerke oder SSDs|
   |**Replikation**|Lokal redundanter Speicher||
   |** Zugriffsebene (Standard)|„Cool“ oder „Hot“||
   |**Sichere Übertragung erforderlich**|Deaktiviert||
   |**Abonnement**|Ihr Abonnement|Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions).|
   |**Ressourcengruppe**|Die zuvor erstellte Ressourcengruppe|| 
   |**Location**|Der zuvor ausgewählte Standort||
   |**Virtuelle Netzwerke**|Deaktiviert||

4. Klicken Sie auf **Create**.

   ![Speicherkontodetails](./media/sql-database-managed-instance-tutorial/storage-account-details.png)

5. Öffnen Sie Ihr neues Speicherkonto, nachdem die Bereitstellung des Speicherkontos erfolgreich abgeschlossen wurde.
6. Klicken Sie unter **Einstellungen** auf **Shared Access Signature**, um das Formular für Shared Access Signatures (SAS) zu öffnen.

   ![SAS-Formular](./media/sql-database-managed-instance-tutorial/sas-form.png)

7. Ändern Sie die Standardwerte auf dem SAS-Formular wie gewünscht. Beachten Sie, dass die Gültigkeitsdauer standardmäßig nur acht Stunden beträgt.
8. Klicken Sie auf **SAS generieren**.

   ![SAS-Formular – Abgeschlossen](./media/sql-database-managed-instance-tutorial/sas-generate.png)

9. Kopieren und speichern Sie das **SAS-Token** und die **Blob server SAS URL** (Blobserver-SAS-URL).
10. Klicken Sie unter **Einstellungen** auf **Container**.

    ![containers](./media/sql-database-managed-instance-tutorial/containers.png)

11. Klicken Sie auf **+ Container**, um einen Container für Ihre Sicherungsdatei zu erstellen.
12. Füllen Sie das Formular für den Container mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

    | Einstellung| Empfohlener Wert | Beschreibung |
   | ------ | --------------- | ----------- |
   |**Name**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Zugriffsebene „Öffentlich“**|Privat (kein anonymer Zugriff)||

    ![Containerdetails](./media/sql-database-managed-instance-tutorial/container-detail.png)

13. Klicken Sie auf **OK**.
14. Öffnen Sie den Container, nachdem er erstellt wurde.

    ![container](./media/sql-database-managed-instance-tutorial/container.png)

15. Klicken Sie auf **Containereigenschaften**, und kopieren Sie die URL anschließend in den Container.

    ![Container-URL](./media/sql-database-managed-instance-tutorial/container-url.png)

16. Klicken Sie auf **Hochladen**, um das Formular **Blob hochladen** zu öffnen.

    ![upload](./media/sql-database-managed-instance-tutorial/upload.png)

17. Navigieren Sie zu Ihrem Downloadordner, und wählen Sie die Datei **WideWorldIimporters-Standard.bak** aus.

    ![upload](./media/sql-database-managed-instance-tutorial/upload-bak.png)

18. Klicken Sie auf **Hochladen**.
19. Fahren Sie erst fort, nachdem der Upload abgeschlossen wurde.

    ![Upload abgeschlossen](./media/sql-database-managed-instance-tutorial/upload-complete.png)


## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>Wiederherstellen der Wide World Importers-Datenbank aus einer Sicherungsdatei

Verwenden Sie in SSMS die folgenden Schritte, um die Wide World Importers-Datenbank aus der Sicherungsdatei für Ihre verwaltete Instanz wiederherzustellen.

1. Öffnen Sie in SSMS ein neues Abfragefenster.
2. Verwenden Sie das folgende Skript, um SAS-Anmeldeinformationen zu erstellen. Geben Sie die URL für den Speicherkontocontainer und den SAS-Schlüssel wie gezeigt an.

   ```sql
   CREATE CREDENTIAL [https://<storage_account_name>.blob.core.windows.net/<container>] 
      WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
      , SECRET = '<shared_access_signature_key_with_removed_first_?_symbol>' 
   ```

    ![credential](./media/sql-database-managed-instance-tutorial/credential.png)

3. Verwenden Sie das folgende Skript, um die Gültigkeit der SAS-Anmeldeinformationen und der Sicherung zu prüfen. Geben Sie die URL für den Container mit der Sicherungsdatei an:

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'
   ```

    ![Dateiliste](./media/sql-database-managed-instance-tutorial/file-list.png)

4. Verwenden Sie das folgende Skript, um die Wide World Importers-Datenbank aus einer Sicherungsdatei wiederherzustellen. Geben Sie die URL für den Container mit der Sicherungsdatei an:

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'`
   ```

    ![Ausführen der Wiederherstellung](./media/sql-database-managed-instance-tutorial/restore-executing.png)

5. Führen Sie die folgende Abfrage in einer neuen Abfragesitzung aus, um den Status Ihrer Wiederherstellung nachzuverfolgen:

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

    ![Wiederherstellung – Fertigstellung in Prozent](./media/sql-database-managed-instance-tutorial/restore-percent-complete.png)

6. Nachdem die Wiederherstellung abgeschlossen wurde, können Sie sie im Objekt-Explorer anzeigen. 

    ![Wiederherstellung abgeschlossen](./media/sql-database-managed-instance-tutorial/restore-complete.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie anhand der Sicherungsdatei „Wide World Importers – Standard“ gelernt, wie Sie eine in Azure Blob Storage gespeicherte Sicherung einer Datenbank in der verwalteten Instanz wiederherstellen. Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> * Herunterladen der Sicherungsdatei „Wide World Importers – Standard“
> * Erstellen des Azure-Speicherkontos und Hochladen der Sicherungsdatei
> * Wiederherstellen der Wide World Importers-Datenbank aus einer Sicherungsdatei

Im nächsten Tutorial erfahren Sie, wie Sie SQL Server mithilfe von DMS zu einer verwalteten Azure SQL-Datenbank-Instanz migrieren.

> [!div class="nextstepaction"]
>[Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz mithilfe von DMS](../dms/tutorial-sql-server-to-managed-instance.md)

---
title: Verwaltete Azure SQL-Datenbank-Instanz – Überwachung | Microsoft-Dokumentation
description: Lernen Sie die ersten Schritte bei der Überwachung von verwalteten Azure SQL-Datenbank-Instanzen mit T-SQL kennen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: vainolo
ms.author: vainolo
ms.reviewer: vanto
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 045314980d0051e8b5ef71bdf95023084eff1880
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063876"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Erste Schritte bei der Überwachung von verwalteten Azure SQL-Datenbank-Instanzen

Die Überwachung von [verwalteten Azure SQL-Datenbank-Instanzen](sql-database-managed-instance.md) verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto. Die Überwachung ermöglicht außerdem Folgendes:

- Sie kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.
- Sie ermöglicht und unterstützt die Einhaltung von Standards, garantiert diese aber nicht. Weitere Informationen zu Azure-Programmen, die die Einhaltung von Standards unterstützen, finden Sie im [Microsoft Azure-Vertrauenscenter](https://azure.microsoft.com/support/trust-center/compliance/).

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Einrichten der Überwachung für Ihren Server in Azure Storage 

Der folgende Abschnitt beschreibt die Konfiguration der Überwachung für Ihre verwaltete Instanz.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Mithilfe der folgenden Schritte erstellen Sie einen Azure Storage-**Container**, in dem die Überwachungsprotokolle gespeichert werden.

   - Navigieren Sie zu der Azure Storage-Instanz, in der Sie Ihre Überwachungsprotokolle speichern möchten.

     > [!IMPORTANT]
     > Verwenden Sie ein Speicherkonto in derselben Region wie der Server mit der verwalteten Instanz, um regionsübergreifende Lese-/Schreibvorgänge zu vermeiden.

   - Wechseln Sie im Speicherkonto zu **Übersicht**, und klicken Sie auf **Blobs**.

     ![Navigationsbereich][1]

   - Klicken Sie im oberen Menü auf **+ Container**, um einen neuen Container zu erstellen.

     ![Navigationsbereich][2]

   - Geben Sie einen **Namen** für den Container an, legen Sie die öffentliche Zugriffsebene auf **Privat** fest, und klicken Sie dann auf **OK**.

     ![Navigationsbereich][3]

   - Klicken Sie in der Liste der Container auf den neu erstellten Container und dann auf **Containereigenschaften**.

     ![Navigationsbereich][4]

   - Kopieren Sie die Container-URL, indem Sie auf das Symbol „Kopieren“ klicken, und speichern Sie die URL für die zukünftige Verwendung (z.B. in Editor). Das Format der Container-URL sollte dem folgenden entsprechen: `https://<StorageName>.blob.core.windows.net/<ContainerName>`

     ![Navigationsbereich][5]

3. Mit den folgenden Schritten generieren Sie ein Azure Storage-**SAS-Token**, das verwendet wird, um der Überwachung der verwalteten Instanz Zugriffsrechte für das Speicherkonto zu erteilen.

   - Navigieren Sie zu dem Azure Storage-Konto, in dem Sie im vorherigen Schritt den Container erstellt haben.

   - Klicken Sie in Storage im Menü „Einstellungen“ auf **Shared Access Signature**.

     ![Navigationsbereich][6]

   - Konfigurieren Sie die SAS wie folgt:
     - **Zulässige Dienste:** Blob
     - **Startdatum:** Zur Vermeidung von Zeitzonenproblemen wird empfohlen, das gestrige Datum zu verwenden.
     - **Enddatum:** Wählen Sie das Datum aus, an dem dieses SAS-Token abläuft. 

       > [!NOTE]
       > Erneuern Sie das Token nach Ablauf, um Überwachungsfehler zu vermeiden.

     - Klicken Sie auf **SAS generieren**.

       ![Navigationsbereich][7]

   - Nach dem Klicken auf „SAS generieren“ wird das SAS-Token unten angezeigt. Kopieren Sie das Token, indem Sie auf das Symbol „Kopieren“ klicken, und speichern Sie es für die zukünftige Verwendung (z.B. in Editor).

     > [!IMPORTANT]
     > Entfernen Sie das Fragezeichen („?“) am Anfang des Tokens.

     ![Navigationsbereich][8]

4. Verbinden Sie Ihre verwaltete Instanz über SQL Server Management Studio (SSMS).

5. Führen Sie die folgende T-SQL-Anweisung aus, um **neue Anmeldeinformationen zu erstellen**. Verwenden Sie dabei die Container-URL und das SAS-Token, die Sie in den vorherigen Schritten erstellt haben:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Führen Sie die folgende T-SQL-Anweisung aus, um eine neue Serverüberwachung zu erstellen (wählen Sie einen eigenen Namen für die Überwachung aus, und verwenden Sie die Container-URL, die Sie in den vorherigen Schritten erstellt haben):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Wenn keine Angabe erfolgt, hat `RETENTION_DAYS` den Standardwert „0“ (unbegrenzte Aufbewahrungsdauer).

    Weitere Informationen:
    - [Unterschiede bei der Überwachung zwischen verwalteten Instanzen, Azure SQL-Datenbank und SQL Server](#auditing-differences-between-managed-instance-azure-sql-database-and-sql-server)
    - [CREATE SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. Erstellen Sie eine Spezifikation für die Serverüberwachung oder die Datenbanküberwachung wie für SQL Server:
    - [Erstellen einer Spezifikation für die Serverüberwachung – T-SQL-Anleitung](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Erstellen einer Spezifikation für die Datenbanküberwachung – T-SQL-Anleitung](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. Aktivieren Sie die Serverüberwachung, die Sie in Schritt 6 erstellt haben:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="set-up-auditing-for-your-server-to-event-hub-or-log-analytics"></a>Einrichten der Überwachung für Ihren Server in Event Hubs oder Log Analytics

Überwachungsprotokolle einer verwalteten Instanz können mithilfe von Azure Monitor an Event Hubs oder Log Analytics gesendet werden. In diesem Abschnitt wird die zugehörige Konfiguration beschrieben:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zur verwalteten SQL-Instanz.

2. Klicken Sie auf **Diagnoseeinstellungen**.

3. Klicken Sie auf **Diagnose aktivieren**. Wenn die Diagnose bereits aktiviert wurde, wird stattdessen *+ Diagnoseeinstellung hinzufügen* angezeigt.

4. Wählen Sie in der Liste der Protokolle **SQLSecurityAuditEvents** aus.

5. Wählen Sie ein Ziel für die Überwachungsereignisse aus: Event Hubs, Log Analytics oder beide. Konfigurieren Sie für jedes Ziel die erforderlichen Parameter (z.B. den Log Analytics-Arbeitsbereich).

6. Klicken Sie auf **Speichern**.

  ![Navigationsbereich][9]

7. Stellen Sie mit **SQL Server Management Studio (SSMS)** oder einem anderen unterstützten Client eine Verbindung mit der verwalteten Instanz her.

8. Führen Sie die folgende T-SQL-Anweisung aus, um eine Serverüberwachung zu erstellen:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. Erstellen Sie eine Spezifikation für die Serverüberwachung oder die Datenbanküberwachung wie für SQL Server:

   - [Erstellen einer Spezifikation für die Serverüberwachung – T-SQL-Anleitung](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Erstellen einer Spezifikation für die Datenbanküberwachung – T-SQL-Anleitung](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. Aktivieren Sie die Serverüberwachung, die Sie in Schritt 7 erstellt haben:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>] WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Verwenden von Überwachungsprotokollen

### <a name="consume-logs-stored-in-azure-storage"></a>Verwenden von Protokollen, die in Azure Storage gespeichert sind

Es gibt verschiedene Methoden zum Anzeigen von Blobüberwachungsprotokollen.

- Verwenden Sie die Systemfunktion `sys.fn_get_audit_file` (T-SQL), um die Daten der Überwachungsprotokolle im Tabellenformat zurückzugeben. Weitere Informationen zur Verwendung dieser Funktion finden Sie in der [Dokumentation zu „sys.fn_get_audit_file“](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Sie können Überwachungsprotokolle mithilfe eines Tools wie Azure Storage-Explorer untersuchen. In Azure Storage werden Überwachungsprotokolle als Sammlung von Blobdateien in einem Container namens „sqldbauditlogs“ gespeichert. Weitere Informationen zur Hierarchie des Speicherordners, zu Namenskonventionen und zum Protokollformat finden Sie in der Formatreferenz für Blobüberwachungsprotokolle.

- Eine vollständige Liste der Verbrauchsmethoden für Überwachungsprotokolle finden Sie unter [Erste Schritte bei der Überwachung von SQL-Datenbank](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).

> [!IMPORTANT]
> Das Anzeigen von Überwachungsdatensätzen im Azure-Portal (Bereich „Überwachungsdatensätze“) steht derzeit für verwaltete Instanzen nicht zur Verfügung.

### <a name="consume-logs-stored-in-event-hub"></a>Verwenden von Protokollen, die in Event Hubs gespeichert sind

Um die Überwachungsprotokolldaten von Event Hub zu nutzen, müssen Sie einen Stream zum Nutzen von Ereignissen einrichten und diese in ein Ziel schreiben. Weitere Informationen finden Sie in der Dokumentation zu Azure Event Hubs.

### <a name="consume-and-analyze-logs-stored-in-log-analytics"></a>Verwenden und Analysieren von Protokollen, die in Log Analytics gespeichert sind

Wenn die Überwachungsprotokolle in Log Analytics geschrieben werden, stehen sie im Log Analytics-Arbeitsbereich bereit. Sie können dort erweiterte Suchvorgänge über die Überwachungsdaten ausführen. Navigieren Sie zunächst zu Log Analytics, klicken Sie im Abschnitt *Allgemein* auf *Protokolle*, und geben Sie eine einfache Abfrage ein, z.B. `search "SQLSecurityAuditEvents"`, um die Überwachungsprotokolle anzuzeigen.  

Mithilfe integrierter Suchfunktionen und benutzerdefinierter Dashboards gewährt Log Analytics Ihnen Einblicke in Betriebsabläufe in Echtzeit, sodass Sie Millionen von Datensätzen für all Ihre Workloads und Server analysieren können. Weitere nützliche Informationen zu Log Analytics-Suchsprache und -Suchbefehlen finden Sie unter [Referenz zur Log Analytics-Suche](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Unterschiede bei der Überwachung zwischen verwalteten Instanzen, Azure SQL-Datenbank und SQL Server

Wichtigste Unterschiede bei der SQL-Überwachung in einer verwalteten Instanz, Azure SQL-Datenbank und lokalem SQL Server:

- In verwalteten Instanzen wird die SQL-Überwachung auf Serverebene ausgeführt. Die Dateien mit der Endung `.xel` werden in einem Azure Blob Storage-Konto gespeichert.
- In Azure SQL-Datenbank wird die SQL-Überwachung auf Datenbankebene ausgeführt.
- Auf lokalen oder virtuellen SQL Server-Computern wird die SQL-Überwachung auf Serverebene ausgeführt, Ereignisse werden jedoch in Dateisystemprotokollen bzw. Windows-Ereignisprotokollen gespeichert.

Die XEvent-Überwachung in einer verwalteten Instanz unterstützt Azure Blob Storage-Ziele. Dateiprotokolle und Windows-Protokolle werden **nicht unterstützt**.

Wichtigste Unterschiede in der Syntax von `CREATE AUDIT` zur Überwachung in Azure Blob Storage:

- Mit der neuen Syntax `TO URL` können Sie die URL des Azure Blob Storage-Containers angeben, in dem Dateien mit der Endung `.xel` gespeichert werden.
- Eine neue Syntax `TO EXTERNAL MONITOR` wird bereitgestellt, um Event Hubs- und Log Analytics-Ziele zu ermöglichen.
- Die Syntax `TO FILE` wird **nicht unterstützt**, da verwaltete Instanzen nicht auf Windows-Dateifreigaben zugreifen können.
- Die Option zum Herunterfahren wird **nicht unterstützt**.
- Der Wert „0“ für `queue_delay` wird **nicht unterstützt**.

## <a name="next-steps"></a>Nächste Schritte

- Eine vollständige Liste der Verbrauchsmethoden für Überwachungsprotokolle finden Sie unter [Erste Schritte bei der Überwachung von SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).
- Weitere Informationen zu Azure-Programmen, die die Einhaltung von Standards unterstützen, finden Sie im [Microsoft Azure-Vertrauenscenter](https://azure.microsoft.com/support/trust-center/compliance/).

<!--Image references-->
[1]: ./media/sql-managed-instance-auditing/1_blobs_widget.png
[2]: ./media/sql-managed-instance-auditing/2_create_container_button.png
[3]: ./media/sql-managed-instance-auditing/3_create_container_config.png
[4]: ./media/sql-managed-instance-auditing/4_container_properties_button.png
[5]: ./media/sql-managed-instance-auditing/5_container_copy_name.png
[6]: ./media/sql-managed-instance-auditing/6_storage_settings_menu.png
[7]: ./media/sql-managed-instance-auditing/7_sas_configure.png
[8]: ./media/sql-managed-instance-auditing/8_sas_copy.png
[9]: ./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png

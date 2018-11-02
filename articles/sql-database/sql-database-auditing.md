---
title: Erste Schritte bei der Überwachung von Azure SQL-Datenbank | Microsoft-Dokumentation
description: Verwenden Sie die Azure SQL-Datenbanküberwachung, um Datenbankereignisse in einem Überwachungsprotokoll nachzuverfolgen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 2a0bacaf0405a5223afedcd3897e2a1514f7128b
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466680"
---
# <a name="get-started-with-sql-database-auditing"></a>Erste Schritte bei der Überwachung von SQL-Datenbank

Die SQL-Datenbank-Überprüfung in Azure verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure-Speicherkonto. Die Überwachung ermöglicht außerdem Folgendes:

- Sie kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

- Sie ermöglicht und unterstützt die Einhaltung von Standards, garantiert diese aber nicht. Weitere Informationen zu Azure-Programmen, die die Einhaltung von Standards unterstützen, finden Sie im [Microsoft Azure-Vertrauenscenter](https://azure.microsoft.com/support/trust-center/compliance/).

## <a id="subheading-1"></a>Übersicht über die Überwachung von Azure SQL-Datenbank

Sie können die SQL-Datenbank-Überwachung für folgende Zwecke verwenden:

- **Beibehalten** eines Überwachungspfads von ausgewählten Ereignissen. Sie können Kategorien für zu protokollierende Datenbankaktionen und -ereignisse konfigurieren.
- **Melden** von Datenbankaktivitäten. Sie können vorkonfigurierte Berichte und ein Dashboard verwenden, um schnell mit der Berichterstattung über Aktivitäten und Ereignisse zu beginnen.
- **Analysieren** von Berichten. Sie können nach verdächtigen Ereignissen, ungewöhnliche Aktivitäten und Trends suchen.

Sie können die Überwachung für verschiedene Arten von Ereigniskategorien konfigurieren, wie im Abschnitt [Einrichten der Überwachung für Ihre Datenbank](#subheading-2) erläutert.

> [!IMPORTANT]
> Überwachungsprotokolle werden in Ihrem Azure-Abonnement als **Anfügeblobs** in Azure Blob Storage geschrieben.
>
> - Bei **Storage Premium** werden Anfügeblobs derzeit **nicht unterstützt**.
> - Die **Speicherung in VNET** wird derzeit **nicht unterstützt**.

## <a id="subheading-8"></a>Definieren von Überwachungsrichtlinien auf Serverebene und auf Datenbankebene

Eine Überwachungsrichtlinie kann für eine spezifische Datenbank oder als Standardserverrichtlinie definiert werden:

- Eine Serverrichtlinie wird auf alle vorhandenen und neu erstellten Datenbanken auf dem Server angewendet.

- Wenn die *Serverblobüberwachung aktiviert ist*, gilt sie *immer für die Datenbank*. Die Datenbank wird unabhängig von den Datenbanküberwachungseinstellungen überwacht.

- Die Aktivierung der Blobüberwachung in der Datenbank sowie auf dem Server hebt die Einstellungen der Serverblobüberwachung *nicht* auf und ändert sie nicht. Beide Überwachungen existieren nebeneinander. Das heißt, die Datenbank wird zweimal parallel überwacht: einmal anhand der Serverrichtlinie und einmal anhand der Datenbankrichtlinie.

   > [!NOTE]
   > Aktivieren Sie die Serverblobüberwachung und die Datenbankblobüberwachung nicht zusammen, es sei denn:
    > - Sie möchten ein anderes *Speicherkonto* oder eine andere *Beibehaltungsdauer* für eine bestimmte Datenbank verwenden.
    > - Sie möchten andere Ereignistypen oder Kategorien für eine bestimmte Datenbank überwachen als die, die für die restlichen Datenbanken auf dem Server überwacht werden. Beispielsweise sollen Tabelleneinfügungen nur für eine bestimmte Datenbank überwacht werden.
   >
   > Andernfalls wird empfohlen, die Blobüberwachung nur auf Serverebene zu aktivieren und die Überwachung auf Datenbankebene für alle Datenbanken deaktiviert zu lassen.

## <a id="subheading-2"></a>Einrichten der Überwachung für Ihre Datenbank

Der folgende Abschnitt beschreibt die Konfiguration der Überwachung über das Azure-Portal.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie im Bereich für die SQL-Datenbank oder den SQL Server unter der Überschrift „Sicherheit“ zu **Überwachung**.

    <a id="auditing-screenshot"></a>![Navigationsbereich][1]

3. Wenn Sie eine Serverüberwachungsrichtlinie einrichten möchten, wählen Sie auf dem Blatt für die Datenbanküberwachung den Link **Servereinstellungen anzeigen** aus. Anschließend können Sie die Serverüberwachungseinstellungen anzeigen oder ändern. Eine Standardrichtlinie für die Serverüberwachung wird auf alle vorhandenen und neu erstellten Datenbanken auf einem Server angewendet.

    ![Navigationsbereich][2]

4. Wenn Sie die Überwachung auf Datenbankebene aktivieren möchten, ändern Sie **Überwachung** in **EIN**.

    Wenn die Serverüberwachung aktiviert ist, existiert die konfigurierte Datenbanküberwachung parallel zur Serverüberwachung.

    ![Navigationsbereich][3]

5. **Neu**: Sie haben nun mehrere Optionen zur Auswahl, um zu konfigurieren, wohin Überwachungsprotokolle geschrieben werden sollen. Sie können die Protokolle in ein Azure-Speicherkonto, in einen Log Analytics-Arbeitsbereich für die Nutzung durch Log Analytics oder in einen Event Hub für die Nutzung durch den Event Hub schreiben. Sie können eine beliebige Kombination dieser Optionen konfigurieren, und die Überwachungsprotokolle werden in die jeweils angegebenen Speicherorte geschrieben.

    ![Speicheroptionen](./media/sql-database-auditing-get-started/auditing-select-destination.png)

6. Um das Schreiben von Überwachungsprotokollen in ein Speicherkonto zu konfigurieren, wählen Sie **Storage** aus, und öffnen Sie **Speicherdetails**. Wählen Sie das Azure Storage-Konto, in dem die Protokolle gespeichert werden sollen, und dann die Beibehaltungsdauer aus. Die alten Protokolle werden gelöscht. Klicken Sie dann auf **OK**.

    ![Speicherkonto](./media/sql-database-auditing-get-started/auditing_select_storage.png)

7. Um das Schreiben von Überwachungsprotokollen in einen Log Analytics-Arbeitsbereich zu konfigurieren, wählen Sie **Log Analytics (Vorschau)** und öffnen **Log Analytics-Details**. Wählen Sie den Log Analytics-Arbeitsbereich aus (oder erstellen Sie ihn), in den Protokolle geschrieben werden sollen, und klicken Sie dann auf **OK**.

    ![Log Analytics](./media/sql-database-auditing-get-started/auditing_select_oms.png)

8. Um das Schreiben von Überwachungsprotokollen in einen Event Hub zu konfigurieren, wählen Sie **Event Hub (Vorschau)** aus, und öffnen Sie **Event Hub-Details**. Wählen Sie den Event Hub aus, in den die Protokolle geschrieben werden sollen, und klicken Sie dann auf **OK**. Achten Sie darauf, dass sich der Event Hub in derselben Region wie Ihre Datenbank und der Server befindet.

    ![Event Hub](./media/sql-database-auditing-get-started/auditing_select_event_hub.png)

9. Klicken Sie auf **Speichern**.
10. Wenn Sie die überwachten Ereignisse anpassen möchten, können Sie [PowerShell-Cmdlets](#subheading-7) oder die [REST-API](#subheading-9) verwenden.
11. Nachdem Sie Ihre Überwachungseinstellungen konfiguriert haben, können Sie das neue Feature der Bedrohungserkennung aktivieren und die E-Mail-Konten konfigurieren, an die Sicherheitswarnungen gesendet werden sollen. Mit der Bedrohungserkennung können Sie proaktive Warnungen bei anomalen Datenbankaktivitäten erhalten, die auf mögliche Sicherheitsbedrohungen hinweisen können. Weitere Informationen finden Sie unter [Erste Schritte mit der Bedrohungserkennung](sql-database-threat-detection-get-started.md).

## <a id="subheading-3"></a>Analysieren von Überwachungsprotokollen und -berichten

Wenn Sie Überwachungsprotokolle in Log Analytics schreiben möchten:

- Verwenden Sie das [Azure-Portal](https://portal.azure.com).  Öffnen Sie die entsprechende Datenbank. Klicken Sie in der Datenbank oben auf der Seite **Überwachung** auf **Überwachungsprotokolle anzeigen**.

    ![Überwachungsprotokolle anzeigen](./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png)

- Klicken Sie dann am oberen Rand der Seite **Überwachungsdatensätze** auf **In OMS öffnen**. Daraufhin wird die Protokollansicht in Log Analytics geöffnet, wo Sie Zeitbereich und Suchabfrage anpassen können.

    ![Öffnen in Log Analytics](./media/sql-database-auditing-get-started/auditing_open_in_oms.png)

- Alternativ können Sie auch vom Blatt „Log Analytics“ auf die Überwachungsprotokolle zugreifen. Öffnen Sie Ihren Log Analytics-Arbeitsbereich und klicken Sie im Abschnitt **Allgemein** auf **Protokolle**. Beginnen Sie mit einer einfachen Abfrage, wie z. B. *"SQLSecurityAuditEvents" suchen*, um die Überwachungsprotokolle anzuzeigen.
    Hier können Sie auch [Log Analytics](../log-analytics/log-analytics-log-search.md) nutzen, um erweiterte Suchen für Ihre Überwachungsprotokolldaten durchzuführen. Mithilfe integrierter Suchfunktionen und benutzerdefinierter Dashboards gewährt Log Analytics Ihnen Einblicke in Betriebsabläufe in Echtzeit, sodass Sie Millionen von Datensätzen für all Ihre Workloads und Server analysieren können. Weitere nützliche Informationen zu Log Analytics-Suchsprache und -Suchbefehlen finden Sie unter [Referenz zur Log Analytics-Suche](../log-analytics/log-analytics-log-search.md).

Wenn Sie Überwachungsprotokolle in Event Hub schreiben möchten:

- Um die Überwachungsprotokolldaten von Event Hub zu nutzen, müssen Sie einen Stream zum Nutzen von Ereignissen einrichten und diese in ein Ziel schreiben. Weitere Informationen finden Sie in der [Dokumentation zu Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/).

Wenn Sie die Überwachungsprotokolle in ein Azure-Speicherkonto schreiben möchten, gibt es mehrere Methoden zum Anzeigen der Protokolle:

- Überwachungsprotokolle werden in dem Konto aggregiert, das Sie während der Einrichtung ausgewählt haben. Sie können Überwachungsprotokolle mithilfe eines Tools wie [Azure Storage-Explorer](http://storageexplorer.com/) untersuchen. In Azure Storage werden Überwachungsprotokolle als Sammlung von Blobdateien in einem Container namens **sqldbauditlogs** gespeichert. Weitere Informationen zur Hierarchie des Speicherordners, zu Namenskonventionen und zum Protokollformat finden Sie in der [Formatreferenz für Blobüberwachungsprotokolle](https://go.microsoft.com/fwlink/?linkid=829599).

- Verwenden Sie das [Azure-Portal](https://portal.azure.com).  Öffnen Sie die entsprechende Datenbank. Klicken Sie in der Datenbank oben auf der Seite **Überwachung** auf **Überwachungsprotokolle anzeigen**.

    ![Navigationsbereich][7]

    Der Bereich **Überwachungsdatensätze** wird geöffnet, in dem Sie die Protokolle anzeigen können.

  - Sie können Protokolle für einen bestimmten Zeitraum anzeigen, indem Sie im oberen Bereich der Seite **Überwachungsdatensätze** auf **Filter** klicken.
  - Sie können zwischen Überwachungsdatensätzen wechseln, die anhand der *Serverüberwachungsrichtlinie* oder der *Datenbanküberwachungsrichtlinie* erstellt wurden, indem Sie die Option unter **Überwachungsquelle** ändern.
  - Sie können nur Überwachungsdatensätze zur Einschleusung von SQL-Befehlen anzeigen, indem Sie das Kontrollkästchen **Nur Überwachungsdatensätze zur Einschleusung von SQL-Befehlen anzeigen** aktivieren.

       ![Navigationsbereich][8]

- Verwenden Sie die Systemfunktion **sys.fn_get_audit_file** (T-SQL), um die Daten der Überwachungsprotokolle im Tabellenformat zurückzugeben. Weitere Informationen zur Verwendung dieser Funktion finden Sie unter [sys.fn_get_audit_file](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Verwenden von **Überwachungsdateien zusammenführen** in SQL Server Management Studio (ab SSMS 17):
    1. Wählen Sie im SSMS-Menü **Datei** > **Öffnen** > **Überwachungsdateien zusammenführen**.

        ![Navigationsbereich][9]
    2. Das Dialogfeld **Überwachungsdateien hinzufügen** wird geöffnet. Wählen Sie eine der Optionen zum **Hinzufügen** aus, um festzulegen, ob die Überwachungsdateien von einem lokalen Datenträger zusammengeführt oder aus Azure Storage importiert werden sollen. Sie müssen Ihre Azure Storage-Details und Ihren Kontoschlüssel angeben.

    3. Nachdem Sie alle zusammenzuführenden Dateien hinzugefügt haben, klicken Sie auf **OK**, um die Zusammenführung abzuschließen.

    4. Die zusammengeführte Datei wird in SSMS geöffnet. Hier können Sie die Datei anzeigen und analysieren und in eine XEL- oder CSV-Datei oder in eine Tabelle exportieren.

- Verwenden Sie Power BI. Sie können Überwachungsprotokolldateien in Power BI anzeigen und analysieren. Weitere Informationen finden Sie unter [Analysieren von Überwachungsprotokolldaten in Power BI](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/). Dort können Sie auch auf eine herunterladbare Vorlage zugreifen.
- Laden Sie Protokolldateien aus Ihrem Azure Storage-Blobcontainer über das Portal oder mithilfe eines Tools wie [Azure Storage-Explorer](http://storageexplorer.com/) herunter.
  - Nachdem Sie die Protokolldatei lokal heruntergeladen haben, können Sie auf die Datei doppelklicken, um die Protokolle in SSMS zu öffnen, anzuzeigen und zu analysieren.
  - Sie können mit dem Azure Storage-Explorer auch mehrere Dateien gleichzeitig herunterladen. Klicken Sie dazu mit der rechten Maustaste auf einen bestimmten Unterordner, und wählen Sie **Speichern unter** aus, um die Dateien in einem lokalen Ordner zu speichern.

- Weitere Methoden:

  - Nach dem Herunterladen mehrerer Dateien oder eines Unterordners, der Protokolldateien enthält, können Sie die Dateien lokal zusammenführen, wie in den Anweisungen weiter oben für das Zusammenführen von Überwachungsdateien in SSMS beschrieben.
  - Programmgesteuertes Anzeigen von Blobüberwachungsprotokollen:

    - Verwenden Sie die C#-Bibliothek [Leser für erweiterte Ereignisse ](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/).
    - Führen Sie [Abfragen von Dateien mit erweiterten Ereignissen](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) mithilfe von PowerShell durch.

## <a id="subheading-5"></a>Produktionsverfahren

<!--The description in this section refers to preceding screen captures.-->

### <a id="subheading-6">Überwachung von georeplizierten Datenbanken</a>

Wenn Sie bei georeplizierten Datenbanken die Überwachung für die primäre Datenbank aktivieren, verfügt die sekundäre Datenbank über eine identische Überwachungsrichtlinie. Es ist auch möglich, die Überwachung für die sekundäre Datenbank einzurichten, indem Sie die Überwachung auf dem **sekundären Server** unabhängig von der primären Datenbank aktivieren.

- Auf Serverebene (**empfohlen**): Aktivieren Sie die Überwachung sowohl auf dem **primären Server** als auch auf dem **sekundären Server**. Dadurch werden die primäre und die sekundäre Datenbank jeweils unabhängig auf der Grundlage der jeweiligen Richtlinie auf Serverebene überwacht.
- Auf Datenbankebene: Die Überwachung auf Datenbankebene kann für sekundäre Datenbanken nur über die Überwachungseinstellungen der primären Datenbank konfiguriert werden.
  - Die Überwachung muss in der *primären Datenbank selbst* aktiviert werden, nicht auf dem Server.
  - Nachdem die Überwachung in der primären Datenbank aktiviert wurde, wird sie auch in der sekundären Datenbank aktiv.

    >[!IMPORTANT]
    >Bei der Überwachung auf Datenbankebene sind die Speichereinstellungen für die sekundäre Datenbank identisch mit den Einstellungen der primären Datenbank, wodurch regionsübergreifender Datenverkehr generiert wird. Es wird empfohlen, die Überwachung nur auf Serverebene zu aktivieren und die Überwachung auf Datenbankebene für alle Datenbanken deaktiviert zu lassen.

### <a id="subheading-6">Erneute Speicherschlüsselgenerierung</a>

In einer Produktionsumgebung werden Sie Ihre Speicherschlüssel wahrscheinlich regelmäßig aktualisieren. Wenn Sie Überwachungsprotokolle in Azure Storage schreiben, müssen Sie Ihre Überwachungsrichtlinie neu speichern, wenn Sie die Schlüssel aktualisieren. Dieser Prozess verläuft wie folgt:

1. Öffnen Sie **Speicherdetails**. Wählen Sie im Feld **Speicherzugriffsschlüssel** die Option **Sekundär** aus, und klicken Sie dann auf **OK**. Klicken Sie anschließend oben auf der Seite für die Überwachungskonfiguration auf **Speichern**.

    ![Navigationsbereich][5]
2. Wechseln Sie zur Seite für die Speicherkonfiguration, und generieren Sie erneut den primären Zugriffsschlüssel.

    ![Navigationsbereich][6]
3. Wechseln Sie zurück zur Seite für die Überwachungskonfiguration, ändern Sie den Speicherzugriffsschlüssel von „Sekundär“ in „Primär“, und klicken Sie dann auf **OK**. Klicken Sie anschließend oben auf der Seite für die Überwachungskonfiguration auf **Speichern**.
4. Wechseln Sie wieder zurück zur Seite für die Speicherkonfiguration, und generieren Sie den sekundären Zugriffsschlüssel erneut (in Vorbereitung auf den nächsten Schlüsselaktualisierungszyklus).

## <a name="additional-information"></a>Zusätzliche Informationen

- Informationen zum Protokollformat, zur Hierarchie des Speicherordners und zu Namenskonventionen finden Sie in der [Formatreferenz für Blobüberwachungsprotokolle](https://go.microsoft.com/fwlink/?linkid=829599).

    > [!IMPORTANT]
    > Die Azure SQL-Datenbanküberwachung speichert 4000 Datenzeichen für Zeichenfelder in einem Überwachungsdatensatz. Wenn der von einer überwachbaren Aktion zurückgegebene Wert **statement** oder **data_sensitivity_information** mehr als 4000 Zeichen enthält, werden alle Daten über die ersten 4000 Zeichen hinaus **abgeschnitten und nicht überwacht**.

- Überwachungsprotokolle werden in Ihrem Azure-Abonnement als **Anfügeblobs** in Azure Blob Storage geschrieben:
  - Bei **Storage Premium** werden Anfügeblobs derzeit **nicht unterstützt**.
  - Die **Speicherung in VNET** wird derzeit **nicht unterstützt**.

- Die Standardüberwachungsrichtlinie enthält alle Aktionen sowie den folgenden Satz von Aktionsgruppen, mit denen alle Abfragen und gespeicherten Prozeduren, die für die Datenbank ausgeführt werden, sowie erfolgreiche und fehlerhafte Anmeldungen überwacht werden:

    BATCH_COMPLETED_GROUP<br>
    SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP<br>
    FAILED_DATABASE_AUTHENTICATION_GROUP

    Sie können die Überwachung für verschiedene Arten von Aktionen und Aktionsgruppen mithilfe von PowerShell konfigurieren, wie im Abschnitt [Verwalten der Überwachung von SQL-Datenbank mithilfe von Azure PowerShell](#subheading-7) beschrieben.

## <a id="subheading-7"></a>Verwalten der Überwachung von SQL-Datenbank mithilfe von Azure PowerShell

**PowerShell-Cmdlets:**

- [Erstellen oder Aktualisieren der Datenbank-Blobüberwachungsrichtlinie (Set-AzureRMSqlDatabaseAuditing)][105]
- [Erstellen oder Aktualisieren der Server-Blobüberwachungsrichtlinie (Set-AzureRMSqlServerAuditing)][106]
- [Abrufen der Datenbank-Blobüberwachungsrichtlinie (Get-AzureRMSqlDatabaseAuditing)][101]
- [Abrufen der Server-Blobüberwachungsrichtlinie (Get-AzureRMSqlServerAuditing)][102]

Ein Skriptbeispiel finden Sie unter [Konfigurieren von Überwachung von SQL-Datenbank und Bedrohungserkennung mit PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a id="subheading-9"></a>Verwalten der Überwachung von SQL-Datenbank mithilfe der REST-API

**REST-API – Blobüberwachung**:

- [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Datenbankblobs](https://docs.microsoft.com/rest/api/sql/database%20auditing%20settings/createorupdate)
- [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Serverblobs](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/createorupdate)
- [Abrufen einer Richtlinie für die Überwachung von Datenbankblobs](https://docs.microsoft.com/rest/api/sql/database%20auditing%20settings/get)
- [Abrufen einer Richtlinie für die Überwachung von Serverblobs](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/get)

Erweiterte Richtlinie mit Unterstützung der WHERE-Klausel für zusätzliche Filterung:

- [Erstellen oder Aktualisieren einer *erweiterten* Richtlinie für die Überwachung von Datenbankblobs](https://docs.microsoft.com/rest/api/sql/database%20extended%20auditing%20settings/createorupdate)
- [Erstellen oder Aktualisieren einer *erweiterten* Richtlinie für die Überwachung von Serverblobs](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/createorupdate)
- [Abrufen einer *erweiterten* Richtlinie für die Überwachung von Datenbankblobs](https://docs.microsoft.com/rest/api/sql/database%20extended%20auditing%20settings/get)
- [Abrufen einer *erweiterten* Richtlinie für die Überwachung von Serverblobs](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/get)

<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Manage SQL database auditing using Azure PowerShell]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)
[Manage SQL database auditing using REST API]: #subheading-9

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditing
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditing
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditing
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditing

---
title: Überwachung in Azure SQL Data Warehouse | Microsoft Docs
description: Es wird beschrieben, wie Sie die Überwachung durchführen und in Azure SQL Data Warehouse einrichten.
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/11/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 199095c3cffc8df7f9755f1f2c4bb5a1acba3748
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260379"
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Überwachung in Azure SQL Data Warehouse

Es wird beschrieben, wie Sie die Überwachung durchführen und in Azure SQL Data Warehouse einrichten.

## <a name="what-is-auditing"></a>Was ist die Überwachung?
Mit SQL Data Warehouse-Überwachung können Sie Ereignisse in der Datenbank in einem Überwachungsprotokoll in Ihrem Azure Storage-Konto aufzeichnen. Die Überwachung kann Ihnen dabei helfen, gesetzliche Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftliche Probleme oder mutmaßliche Sicherheitsverstöße hinweisen können.

Überwachungstools ermöglichen und unterstützen die Einhaltung von Konformitätsstandards, gewährleisten aber keine Konformität. Weitere Informationen zu Azure-Programmen, die die Einhaltung von Standards unterstützen, finden Sie im [Microsoft Azure-Vertrauenscenter](https://azure.microsoft.com/support/trust-center/compliance/).

## <a id="subheading-1"></a>Grundlagen zur Überwachung
Die Überwachung von SQL Data Warehouse bietet folgende Möglichkeiten:

* **Beibehalten** eines Überwachungspfads von ausgewählten Ereignissen. Sie können Kategorien für zu protokollierende Datenbankaktionen konfigurieren.
* **Melden** von Datenbankaktivitäten. Sie können vorkonfigurierte Berichte und ein Dashboard verwenden, um schnell mit der Berichterstattung über Aktivitäten und Ereignisse zu beginnen.
* **Analysieren** von Berichten. Sie können nach verdächtigen Ereignissen, ungewöhnliche Aktivitäten und Trends suchen.

Überwachungsprotokolle werden in Ihrem Azure-Speicherkonto gespeichert. Sie können eine Aufbewahrungsdauer für ein Überwachungsprotokoll definieren.


## <a id="subheading-4"></a>Definieren von Überwachungsrichtlinien auf Serverebene und auf Datenbankebene

Eine Überwachungsrichtlinie kann für eine spezifische Datenbank oder als Standardserverrichtlinie definiert werden:

* Eine Serverrichtlinie **wird auf alle vorhandenen und neu erstellten Datenbanken auf dem Server angewendet**.

* Wenn die *Serverblobüberwachung aktiviert ist*, gilt sie *immer für die Datenbank*. Die Datenbank wird unabhängig von den Datenbanküberwachungseinstellungen überwacht.

* Die Aktivierung der Überwachung in der Datenbank sowie auf dem Server hebt die Einstellungen der Serverblobüberwachung *nicht* auf und ändert sie nicht. Beide Überwachungen existieren nebeneinander. Das heißt, die Datenbank wird zweimal parallel überwacht: einmal anhand der Serverrichtlinie und einmal anhand der Datenbankrichtlinie.

> [!NOTE]
> Wir empfehlen, die Blobüberwachung **nur auf Serverebene** zu aktivieren und die Überwachung auf Datenbankebene für alle Datenbanken deaktiviert zu lassen.
> Aktivieren Sie die Serverüberwachung und die Datenbanküberwachung nicht zusammen, es sei denn:
> * Sie möchten ein anderes *Speicherkonto* oder eine andere *Beibehaltungsdauer* für eine bestimmte Datenbank verwenden.
> * Sie möchten andere Ereignistypen oder Kategorien für eine bestimmte Datenbank überwachen als die, die für die restlichen Datenbanken auf dem Server überwacht werden. Beispielsweise sollen Tabelleneinfügungen nur für eine bestimmte Datenbank überwacht werden.
> * Sie möchten die Bedrohungserkennung nutzen, die zurzeit nur bei der Überwachung auf Datenbankebene unterstützt wird.

> [!IMPORTANT]
>Das Aktivieren der Überwachung in einem Azure SQL Data Warehouse oder auf einem Server mit einem Azure SQL Data Warehouse **führt dazu, dass das Data Warehouse wieder aufgenommen wird**, auch wenn es zuvor angehalten wurde. **Stellen Sie sicher, dass Sie das Data Warehouse nach dem Aktivieren der Überwachung erneut anhalten**.

## <a id="subheading-5"></a>Einrichten der Überwachung auf Serverebene für alle Datenbanken

Eine Richtlinie für die Serverüberwachung wird auf **alle vorhandenen und neu erstellten Datenbanken** auf einem Server angewendet.

Der folgende Abschnitt beschreibt die Konfiguration der Überwachung über das Azure-Portal.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wechseln Sie zum **SQL-Server**, den Sie überwachen möchten. (Wichtig: Stellen Sie sicher, dass Sie den SQL-Server und keine bestimmte Datenbank/kein bestimmtes Data Warehouse anzeigen). Wählen Sie im Menü **Sicherheit** die Option **Überwachung und Bedrohungserkennung**.

    ![Navigationsbereich][6]
4. Wählen Sie auf dem Blatt *Überwachung und Bedrohungserkennung* für **Überwachung** die Option **EIN**. Diese Überwachungsrichtlinie wird auf alle vorhandenen und neu erstellten Datenbanken auf einem Server angewendet.

    ![Navigationsbereich][7]
5. Wählen Sie **Speicherdetails**, um das Blatt **Speicherung von Überwachungsprotokollen** zu öffnen. Wählen oder erstellen Sie das Azure-Speicherkonto, in dem die Protokolle gespeichert werden sollen, sowie die Aufbewahrungsdauer (nach der die alten Protokolle gelöscht werden). Klicken Sie dann auf **OK**.

    ![Navigationsbereich][8]

    > [!IMPORTANT]
    > Überwachungsprotokolle auf Serverebene werden in Ihrem Azure-Abonnement als **Anfügeblobs** in Azure Blob Storage geschrieben.
    >
    > * Bei **Storage Premium** werden Anfügeblobs derzeit **nicht unterstützt**.
    > * Die **Speicherung in VNET** wird derzeit **nicht unterstützt**.

8. Klicken Sie auf **Speichern**.



## <a id="subheading-2"></a>Einrichten der Überwachung der Datenbankebene für eine einzelne Datenbank

Sie können eine Überwachungsrichtlinie für eine bestimmte Datenbank oder als Standardserverrichtlinie definieren.

Es wird dringend empfohlen, die Überwachung auf Serverebene und nicht auf Datenbankebene zu verwenden, wie in [Überwachung in Azure SQL Data Warehouse](#subheading-4) beschrieben.

Überprüfen Sie vor der Einrichtung der Überwachung, ob Sie einen [kompatiblen Client](sql-data-warehouse-auditing-downlevel-clients.md) verwenden.


1. Starten Sie das [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu **Einstellungen** für die SQL Data Warehouse-Instanz, die Sie überwachen möchten. Wählen Sie **Überwachung und Bedrohungserkennung** aus.

    ![][1]
3. Aktivieren Sie anschließend die Überwachung, indem Sie auf die Schaltfläche **EIN** klicken.

    ![][3]
4. Wählen Sie auf dem Panel für die Überwachungskonfiguration die Option **SPEICHERDETAILS** aus, um das Panel „Überwachungsprotokollspeicher“ zu öffnen. Wählen Sie das Azure-Speicherkonto für die Protokolle und die Aufbewahrungsdauer aus.
    >[!TIP]
    >Verwenden Sie für alle überwachten Datenbanken dasselbe Speicherkonto, um die vorkonfigurierten Berichtvorlagen optimal einzusetzen.

    ![][4]

5. Klicken Sie auf die Schaltfläche **OK** , um die Konfiguration für die Speicherdetails zu speichern.
6. Klicken Sie unter **PROTOKOLLIERUNG NACH EREIGNIS** auf **ERFOLG** und **FEHLER**, um alle Ereignisse zu protokollieren oder einzelne Ereigniskategorien auszuwählen.
7. Wenn Sie die Überwachung für eine Datenbank konfigurieren, müssen Sie zum Ändern der Verbindungszeichenfolge des Clients sicherstellen, dass die Datenüberwachung ordnungsgemäß erfasst wird. Weitere Informationen zu Verbindungen mit kompatiblen Clients finden Sie unter [Ändern des vollqualifizierten Namens eines Servers in der Verbindungszeichenfolge](sql-data-warehouse-auditing-downlevel-clients.md).
8. Klicken Sie auf **OK**.

## <a id="subheading-3"></a>Analysieren von Überwachungsprotokollen und -berichten

### <a name="server-level-policy-audit-logs"></a>Überwachungsprotokolle der Richtlinie auf Serverebene
Überwachungsprotokolle auf Serverebene werden in Ihrem Azure-Abonnement als **Anfügeblobs** in Azure Blob Storage geschrieben. Sie werden als Sammlung von Blobdateien in einem Container namens **sqldbauditlogs** gespeichert.

Weitere Informationen zur Hierarchie des Speicherordners, zu Namenskonventionen und zum Protokollformat finden Sie in der [Formatreferenz für Blobüberwachungsprotokolle](https://go.microsoft.com/fwlink/?linkid=829599).

Es gibt verschiedene Methoden zum Anzeigen von Blobüberwachungsprotokollen:

* Verwenden von **Überwachungsdateien zusammenführen** in SQL Server Management Studio (ab SSMS 17):
    1. Wählen Sie im SSMS-Menü **Datei** > **Öffnen** > **Überwachungsdateien zusammenführen**.

    2. Das Dialogfeld **Überwachungsdateien hinzufügen** wird geöffnet. Wählen Sie eine der Optionen zum **Hinzufügen** aus, um festzulegen, ob die Überwachungsdateien von einem lokalen Datenträger zusammengeführt oder aus Azure Storage importiert werden sollen. Sie müssen Ihre Azure Storage-Details und Ihren Kontoschlüssel angeben.

    3. Nachdem Sie alle zusammenzuführenden Dateien hinzugefügt haben, klicken Sie auf **OK**, um die Zusammenführung abzuschließen.

    4. Die zusammengeführte Datei wird in SSMS geöffnet. Hier können Sie die Datei anzeigen und analysieren und in eine XEL- oder CSV-Datei oder in eine Tabelle exportieren.

* Verwenden Sie die [Synchronisierungsanwendung](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration), die wir zuvor erstellt haben. Diese wird in Azure ausgeführt und nutzt öffentliche Log Analytics-APIs, um SQL-Überwachungsprotokolle in Log Analytics zu verschieben. Die Synchronisierungsanwendung verschiebt SQL-Überwachungsprotokolle für die Nutzung über das Log Analytics-Dashboard in Log Analytics.

* Verwenden Sie Power BI. Sie können Überwachungsprotokolldateien in Power BI anzeigen und analysieren. Erfahren Sie mehr über [Power BI, und erhalten Sie Zugriff eine herunterladbare Vorlage](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Laden Sie Protokolldateien aus Ihrem Azure Storage-Blobcontainer über das Portal oder mithilfe eines Tools wie [Azure Storage-Explorer](http://storageexplorer.com/) herunter.
    * Nachdem Sie die Protokolldatei lokal heruntergeladen haben, können Sie auf die Datei doppelklicken, um die Protokolle in SSMS zu öffnen, anzuzeigen und zu analysieren.
    * Sie können mit dem Azure Storage-Explorer auch mehrere Dateien gleichzeitig herunterladen. Klicken Sie mit der rechten Maustaste auf einen bestimmten Unterordner, und wählen Sie **Speichern unter** aus, um die Dateien in einem lokalen Ordner zu speichern.

* Weitere Methoden:
   * Nach dem Herunterladen mehrerer Dateien oder eines Unterordners, der Protokolldateien enthält, können Sie sie lokal zusammenführen, wie in den Anweisungen weiter oben für das Zusammenführen von Überwachungsdateien in SSMS beschrieben.

   * Programmgesteuertes Anzeigen von Blobüberwachungsprotokollen:

     * Verwenden Sie die C#-Bibliothek [Leser für erweiterte Ereignisse ](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/).
     * Führen Sie [Abfragen von Dateien mit erweiterten Ereignissen](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) mithilfe von PowerShell durch.



<br>
### <a name="database-level-policy-audit-logs"></a>Überwachungsprotokolle der Richtlinie auf Datenbankebene
Überwachungsprotokolle auf Datenbankebene werden im Azure-Speicherkonto, das Sie während der Einrichtung ausgewählt haben, in einer einzelnen Azure-Speichertabelle mit dem Präfix **SQLDBAuditLogs** zusammengefasst. Sie können Protokolldateien mithilfe eines Tools wie dem [Azure Storage-Explorer](http://azurestorageexplorer.codeplex.com) anzeigen.

Eine vorkonfigurierte Dashboardberichtvorlage steht als [herunterladbares Excel-Arbeitsblatt](https://go.microsoft.com/fwlink/?LinkId=403540) zur Verfügung, mit dem Sie Protokolldaten schnell analysieren können. Damit Sie die Vorlage für Ihre Überwachungsprotokolle verwenden können, benötigen Sie Excel 2013 oder höher und Power Query, das [hier](https://www.microsoft.com/download/details.aspx?id=39379)heruntergeladen werden kann.

Die Vorlage enthält fiktionale Beispieldaten, und Sie können Power Query so einrichten, dass das Überwachungsprotokoll direkt aus Ihrem Azure-Speicherkonto importiert wird.

## <a id="subheading-4"></a>Erneute Speicherschlüsselgenerierung
In einer Produktionsumgebung werden Sie Ihre Speicherschlüssel wahrscheinlich regelmäßig aktualisieren. Sie müssen beim Aktualisieren Ihrer Schlüssel die Richtlinie speichern. Dieser Prozess verläuft wie folgt:

1. Ändern Sie im Panel für die Überwachungskonfiguration, das im vorherigen Abschnitt zur Einrichtung der Überwachung beschrieben ist, den **Speicherzugriffsschlüssel** von *Primär* in *Sekundär*, und klicken Sie auf **SPEICHERN**.

   ![][4]
2. Wechseln Sie zum Panel für die Speicherkonfiguration, und **generieren Sie erneut** den *primären Zugriffsschlüssel*.
3. Wechseln Sie zurück zum Panel für die Überwachungskonfiguration,
4. ändern Sie den **Speicherzugriffsschlüssel** von *Sekundär* in *Primär*, und klicken Sie auf **SPEICHERN**.
4. Wechseln Sie zurück zur Speicherbenutzeroberfläche, und **generieren Sie erneut** den *sekundären Zugriffsschlüssel* (als Vorbereitung auf den nächsten Schlüsselaktualisierungszyklus).

## <a id="subheading-5"></a>Automatisierung (PowerShell/REST-API)
Sie können die Überwachung in Azure SQL Data Warehouse auch mit den folgenden Automatisierungstools konfigurieren:

* **PowerShell-Cmdlets:**

   * [Get-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy)
   * [Get-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy)
   * [Remove-AzureRMSqlDatabaseAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing)
   * [Remove-AzureRMSqlServerAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing)
   * [Set-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy)
   * [Set-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy)
   * [Use-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy)


## <a name="downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Unterstützung für Überwachung und dynamische Datenmaskierung durch Vorgängerversionsclients
Überwachung kann mit SQL-Clients genutzt werden, die TDS-Umleitung unterstützen.

Jeder Client, der TDS 7.4 implementiert, sollte auch Umleitung unterstützen. Zu den Ausnahmen gehören JDBC 4.0, in dem die Umleitungsfunktion nicht vollständig unterstützt wird, und Tedious for Node.JSS, in dem die Umleitungsfunktion nicht implementiert wurde.

Für „Vorgängerversionsclients“, die TDS 7.3 und ältere Versionen unterstützen, ändern Sie den vollqualifizierten Domänennamen des Servers wie folgt:

- Ursprünglicher vollqualifizierter Domänenname des Servers in der Verbindungszeichenfolge: <*Servername*>.database.windows.net
- Geänderter vollqualifizierter Domänenname des Servers in der Verbindungszeichenfolge: <*Servername*>.database.**secure**.windows.net

Eine unvollständige Liste der "Vorgängerversionsclients":

* .NET 4.0 und ältere Versionen
* ODBC 10.0 und ältere Versionen
* JDBC (JDBC unterstützt zwar TDS 7.4, aber die TDS-Umleitungsfunktion wird nicht vollständig unterstützt)
* Tedious (for Node.JS)

**Anmerkung:** Die vorherige Änderung des vollqualifizierten Domänennamens des Servers kann sich auch als nützlich erweisen bei der Anwendung einer Richtlinie für die Überwachung auf SQL Server-Ebene, ohne dass ein Konfigurationsschritt in jeder Datenbank ausgeführt werden muss (temporäre Minderung).     




<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Define server-level vs. database-level auditing policy]: #subheading-4
[Set up server-level auditing for all databases]: #subheading-5

<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png
[6]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_1_overview.png
[7]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_2_enable.png
[8]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_3_storage.png

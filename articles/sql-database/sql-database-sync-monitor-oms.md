---
title: "Überwachen der Azure SQL-Datensynchronisierung mit OMS Log Analytics | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie die Azure SQL-Datensynchronisierung mithilfe von OMS Log Analytics überwachen."
services: sql-database
ms.date: 11/7/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e1099d2cd7eeccbe76d762028a0c5d5f95f53026
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="monitor-azure-sql-data-sync-preview-with-oms-log-analytics"></a>Überwachen der Azure SQL-Datensynchronisierung-Vorschauversion mit OMS Log Analytics 

Zur Überprüfung des Aktivitätsprotokolls der SQL-Datensynchronisierung sowie zur Erkennung von Fehlern und Warnungen mussten Sie bislang die SQL-Datensynchronisierung manuell im Azure-Portal überprüfen oder hierfür PowerShell oder die REST-API verwenden. Mit den Schritten in diesem Artikel können Sie eine benutzerdefinierte Lösung konfigurieren, die die Überwachung der Datensynchronisierung verbessert. Diese Lösung kann an Ihr individuelles Szenario angepasst werden.

Eine Übersicht über die SQL-Datensynchronisierung finden Sie unter [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md).

## <a name="monitoring-dashboard-for-all-your-sync-groups"></a>Überwachungsdashboard für alle Synchronisierungsgruppen 

Bei der Suche nach Problemen müssen Sie nun nicht mehr die Protokolle der einzelnen Synchronisierungsgruppen durchgehen. Mit einer benutzerdefinierten OMS-Ansicht (Operations Management Suite) können Sie alle Ihre Synchronisierungsgruppen aus allen Ihren Abonnements zentral überwachen. Diese Ansicht enthält die Informationen, die für Kunden der SQL-Datensynchronisierung relevant sind.

![Dashboard zur Überwachung der Datensynchronisierung](media/sql-database-sync-monitor-oms/sync-monitoring-dashboard.jpg)

## <a name="automated-email-notifications"></a>Automatische E-Mail-Benachrichtigungen

Die Protokolle müssen nicht mehr manuell im Azure-Portal, über PowerShell oder mithilfe der REST-API geprüft werden. Mit [OMS Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) können Sie Warnungen erstellen, die direkt an die E-Mail-Adressen der Benutzer gesendet werden, die im Falle eines Fehlers informiert werden müssen.

![E-Mail-Benachrichtigungen für die Datensynchronisierung](media/sql-database-sync-monitor-oms/sync-email-notifications.jpg)

## <a name="how-do-you-set-this-up"></a>Einrichten dieser Konfiguration 

Mit den folgenden Schritten implementieren Sie in weniger als einer Stunde eine benutzerdefinierte OMS-Überwachungslösung für die SQL-Datensynchronisierung.

Drei Komponenten müssen konfiguriert werden:

-   Ein PowerShell-Runbook zur Übertragung von Protokolldaten der SQL-Datensynchronisierung an die OMS

-   Eine OMS Log Analytics-Warnung für E-Mail-Benachrichtigungen

-   Eine OMS-Ansicht für die Überwachung

### <a name="samples-to-download"></a>Beispiele zum Herunterladen

Laden Sie die beiden folgenden Beispiele herunter:

-   [PowerShell-Runbook für das Protokoll der Datensynchronisierung](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [OMS-Ansicht für das Protokoll der Datensynchronisierung](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

### <a name="prerequisites"></a>Voraussetzungen

Folgendes muss bereits eingerichtet sein:

-   Ein Azure Automation-Konto

-   Log Analytics-Verknüpfung mit einem OMS-Arbeitsbereich

## <a name="powershell-runbook-to-get-sql-data-sync-log"></a>PowerShell-Runbook zum Abrufen des Protokolls der SQL-Datensynchronisierung 

Verwenden Sie ein in Azure Automation gehostetes PowerShell-Runbook, um die Protokolldaten der SQL-Datensynchronisierung abzurufen und an die OMS zu senden. Ein Beispielskript steht zur Verfügung. Sie benötigen ein Azure Automation-Konto. Anschließend müssen Sie ein Runbook erstellen und dessen Ausführung planen. 

### <a name="create-a-runbook"></a>Erstellen eines Runbooks

Weitere Informationen zum Erstellen eines Runbooks finden Sie unter [Mein erstes PowerShell-Runbook](https://docs.microsoft.com/azure/automation/automation-first-runbook-textual-powershell).

1.  Klicken Sie unter Ihrem Azure Automation-Konto unter „Prozessautomatisierung“ auf die Registerkarte **Runbooks**.

2.  Klicken Sie in der linken oberen Ecke der Seite „Runbooks“ auf **Runbook hinzufügen**.

3.  Klicken Sie auf **Vorhandenes Runbook importieren**.

4.  Verwenden Sie unter **Runbookdatei** die angegebene Datei `DataSyncLogPowerShellRunbook`. Legen Sie **Runbooktyp** auf `PowerShell` fest. Benennen Sie das Runbook.

5.  Klicken Sie auf **Erstellen**. Sie verfügen nun über ein Runbook.

6.  Klicken Sie unter Ihrem Azure Automation-Konto unter „Freigegebene Ressourcen“ auf die Registerkarte **Variablen**.

7.  Klicken Sie auf der Seite „Variablen“ auf **Variable hinzufügen**. Wir müssen eine Variable erstellen, um die letzte Ausführungszeit für das Runbook zu speichern. Wenn Sie über mehrere Runbooks verfügen, benötigen Sie für jedes Runbook eine eigene Variable.

8.  Legen Sie den Namen der Variablen auf `DataSyncLogLastUpdatedTime` und den Typ auf „DateTime“ fest.

9.  Wählen Sie das Runbook aus, und klicken Sie am oberen Seitenrand auf die Schaltfläche „Bearbeiten“.

10. Nehmen Sie die erforderlichen Änderungen für Ihr Konto und die Konfiguration der SQL-Datensynchronisierung vor. (Ausführlichere Informationen finden Sie im Beispielskript.)

    1.  Azure-Informationen

    2.  Informationen zur Synchronisierungsgruppe

    3.  OMS-Informationen. Diese Informationen finden Sie unter „OMS-Portal“ > „Einstellungen“ > „Verbundene Quellen“. Weitere Informationen zum Senden von Daten an Log Analytics finden Sie unter [Senden von Daten an Log Analytics mit der HTTP-Datensammler-API (Public Preview)](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api).

11. Führen Sie das Runbook im Testbereich aus. Vergewissern Sie sich, dass es erfolgreich ausgeführt wurde.

    Sollten Fehler aufgetreten sein, vergewissern Sie sich, dass das neueste PowerShell-Modul installiert ist. Verwenden Sie hierzu den **Modulkatalog** Ihres Automation-Kontos.

12. Klicken Sie auf **Veröffentlichen**.

### <a name="schedule-the-runbook"></a>Planen des Runbooks

So planen Sie das Runbook:

1.  Klicken Sie unter dem Runbook unter „Ressourcen“ auf die Registerkarte **Zeitpläne**.

2.  Klicken Sie auf der Seite „Zeitpläne“ auf **Zeitplan hinzufügen**.

3.  Klicken Sie auf **Zeitplan mit Runbook verknüpfen**.

4.  Klicken Sie anschließend auf **Neuen Zeitplan erstellen**.

5.  Legen Sie **Serie** auf „Serie“ fest, und geben Sie das gewünschte Intervall an. Verwenden Sie dieses Intervall hier, im Skript und in der OMS.

6.  Klicken Sie auf **Erstellen**.

### <a name="check-the-automation"></a>Überprüfen der Automatisierung

Um zu prüfen, ob die Automatisierung erwartungsgemäß funktioniert, suchen Sie in der **Übersicht** für Ihr Automation-Konto unter **Überwachung** nach der Ansicht **Auftragsstatistik**. Heften Sie diese Ansicht zur einfacheren Betrachtung an Ihr Dashboard an. Erfolgreiche Ausführungen des Runbooks werden als „Abgeschlossen“ angezeigt, nicht erfolgreiche als „Fehler“.

## <a name="create-an-oms-log-reader-alert-for-email-notifications"></a>Erstellen einer OMS-Protokollleserwarnung für E-Mail-Benachrichtigungen

Gehen Sie wie im Anschluss beschrieben vor, um eine auf OMS Log Analytics basierende Warnung zu erstellen. Hierzu muss Log Analytics bereits mit einem OMS-Arbeitsbereich verknüpft sein.

1.  Klicken Sie im OMS-Portal auf **Protokollsuche**.

2.  Erstellen Sie eine Abfrage, um die Fehler und Warnungen innerhalb des gewählten Intervalls nach Synchronisierungsgruppe auszuwählen. Beispiel:

    `Type=DataSyncLog\_CL LogLevel\_s!=Success| measure count() by SyncGroupName\_s interval 60minute`

3.  Klicken Sie nach Ausführung der Abfrage auf das Glockensymbol mit dem Text **Warnung**.

4.  Wählen Sie unter **Warnung generieren basierend auf** die Option **Metrische Maßeinheit** aus.

    1.  Legen Sie den Aggregatwert auf **Größer als** fest.

    2.  Geben Sie nach **Größer als** den Schwellenwert ein, nach dessen Überschreitung Warnungen ausgegeben werden sollen. Bei der Datensynchronisierung ist mit vorübergehenden Fehlern zu rechnen. Es empfiehlt sich daher, den Schwellenwert auf „5“ festzulegen, um Rauschen zu verringern.

5.  Legen Sie unter **Aktionen** die Option **E-Mail-Benachrichtigung** auf „Ja“ fest. Geben Sie die gewünschten E-Mail-Empfänger ein.

6.  Klicken Sie auf **Speichern**. Die angegebenen Empfänger werden nun per E-Mail benachrichtigt, wenn Fehler auftreten.

## <a name="create-an-oms-view-for-monitoring"></a>Erstellen einer OMS-Ansicht für die Überwachung

In diesem Schritt wird eine OMS-Ansicht für die visuelle Überwachung aller angegebenen Synchronisierungsgruppen erstellt. Die Ansicht umfasst mehrere Komponenten:

-   Eine Übersichtskachel mit der Anzahl von Fehlern, erfolgreichen Vorgängen und Warnungen aller Synchronisierungsgruppen.

-   Eine Kachel für alle Synchronisierungsgruppen mit der Anzahl von Fehlern und Warnungen pro Synchronisierungsgruppe. Gruppen ohne Probleme werden auf dieser Kachel nicht angezeigt.

-   Eine Kachel für jede Synchronisierungsgruppe mit der Anzahl von Fehlern, erfolgreichen Vorgängen und Warnungen sowie mit den neuesten Fehlermeldungen.

Gehen Sie zum Konfigurieren der OMS-Ansicht wie folgt vor:

1.  Klicken Sie links auf der OMS-Startseite auf das Pluszeichen, um den **Ansicht-Designer** zu öffnen.

2.  Klicken Sie auf der oberen Leiste des Ansicht-Designers auf **Importieren**. Wählen Sie die Beispieldatei „DataSyncLogOMSView“ aus.

3.  Die Beispielansicht ermöglicht die Verwaltung von zwei Synchronisierungsgruppen. Bearbeiten Sie diese Ansicht, um sie an Ihr Szenario anzupassen. Klicken Sie auf **Bearbeiten**, und nehmen Sie die folgenden Änderungen vor:

    1.  Erstellen Sie nach Bedarf neue Objekte vom Typ „Ring und Liste“.

    2.  Aktualisieren Sie die Abfragen der einzelnen Kacheln mit Ihren Informationen.

        1.  Legen Sie für jede Kachel das Intervall „TimeStamp_t“ wie gewünscht fest.

        2.  Aktualisieren Sie auf den Kacheln für die einzelnen Synchronisierungsgruppen die Namen der Synchronisierungsgruppen.

    3.  Aktualisieren Sie den Titel der einzelnen Kacheln nach Bedarf.

4.  Klicken Sie abschließend auf **Speichern**. Die Ansicht ist nun bereit.

## <a name="cost-of-this-solution"></a>Kosten dieser Lösung

In den meisten Fällen ist diese Lösung kostenlos.

**Azure Automation:** Je nach Verwendung fallen unter Umständen Kosten im Zusammenhang mit dem Azure Automation-Konto an. Pro Monat sind jeweils die ersten 500 Minuten der Auftragsausführungszeit kostenlos. In den meisten Fällen kommt diese Lösung mit weniger als 500 Minuten pro Monat aus. Verwenden Sie zur Vermeidung von Gebühren ein Runbook-Ausführungsintervall von mindestens zwei Stunden. Weitere Informationen finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).

**OMS Log Analytics:** Je nach Verwendung fallen unter Umständen Kosten für die OMS an. Im Free-Tarif sind 500 MB an erfassten Daten pro Tag enthalten. In den meisten Fällen erfasst diese Lösung weniger als 500 MB pro Tag. Filtern Sie ggf. nur nach Fehlern, um den Verbrauch zu senken. Ein entsprechender Filter ist im Runbook enthalten. Sollten Sie pro Tag mehr als 500 MB benötigen, führen Sie ein Upgrade auf den kostenpflichtigen Tarif durch, um zu vermeiden, dass die Analyse bei Erreichen des Limits beendet wird. Weitere Informationen finden Sie unter [Log Analytics – Preise](https://azure.microsoft.com/pricing/details/log-analytics/).

## <a name="code-samples"></a>Codebeispiele

Die in diesem Artikel beschriebenen Codebeispiele können Sie unter den folgenden Links herunterladen:

-   [PowerShell-Runbook für das Protokoll der Datensynchronisierung](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [OMS-Ansicht für das Protokoll der Datensynchronisierung](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur SQL-Datensynchronisierung finden Sie hier:

-   [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md)
-   [Get Started with Azure SQL Data Sync](sql-database-get-started-sql-data-sync.md) (Erste Schritte mit der Azure SQL-Datensynchronisierung-Vorschauversion)
-   [Troubleshoot issues with SQL Data Sync (Preview)](sql-database-troubleshoot-data-sync.md) (Behandeln von Problemen mit der Azure SQL-Datensynchronisierung-Vorschauversion)

-   Vollständige PowerShell-Beispiele, die die Konfiguration der SQL-Datensynchronisierung veranschaulichen:
    -   [Verwenden von PowerShell zum Synchronisieren von Daten zwischen mehreren Azure SQL-­Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Download the SQL Data Sync REST API documentation (Herunterladen der Dokumentation zur REST-API von SQL-Datensynchronisierung)](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Weitere Informationen zu SQL-Datenbank finden Sie unter:

-   [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)
-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)

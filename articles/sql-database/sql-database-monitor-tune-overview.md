---
title: Überwachen und Optimieren der Datenbankleistung in Azure SQL-Datenbank | Microsoft-Dokumentation
description: Tipps für die Leistungsoptimierung in Azure SQL-Datenbank durch Auswertung und Verbesserung.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 10/16/2018
ms.openlocfilehash: 5ef15b7a757b87c14bf0bd764bdd6ca6e6da64e0
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49379045"
---
# <a name="monitoring-and-performance-tuning"></a>Überwachen und Optimieren der Datenbankleistung

Azure SQL-Datenbank ist ein automatisch verwalteter und flexibler Datendienst, in dem Sie leicht die Nutzung überwachen, Ressourcen (CPU, Arbeitsspeicher, E/A) hinzufügen oder entfernen und Empfehlungen zur Leistungsverbesserung Ihrer Datenbank suchen können. Außerdem können Sie die Datenbank an Ihre Workload anpassen lassen und automatisch die Leistung optimieren.

## <a name="the-state-of-an-active-query"></a>Der Status einer aktiven Abfrage

Um die Leistung der Azure SQL-Datenbank zu verbessern, sollten Sie sich darüber im Klaren sein, dass sich jede aktive Abfrageanforderung Ihrer Anwendung entweder in einem laufenden oder einem wartenden Zustand befindet. Wenn Sie ein Leistungsproblem in der Azure SQL-Datenbank beheben, beachten Sie das folgende Diagramm:

![Workload-Status](./media/sql-database-monitor-tune-overview/workload-states.png)

Bei einem Workload mit Leistungsproblemen kann das Leistungsproblem auf CPU-Konflikte zurückzuführen sein (eine **running-related**-Bedingung) oder einzelne Abfragen warten auf etwas (eine **waitingrelated**-Bedingung).

- **Übermäßige CPU-Auslastung in Ihrer Azure SQL-Datenbank**:

  Unter den folgenden Bedingungen kann es zu einer übermäßigen CPU-Auslastung kommen, die zu Leistungsproblemen führt:

  - Zu viele ausgeführte Abfragen
  - Zu viele kompilierte Abfragen
  - Eine oder mehrere ausgeführte Abfragen verwenden einen suboptimalen Abfrageplan.

  Wenn dies der Fall ist, ist es Ihr Ziel, entweder die zugehörigen Abfragen zu identifizieren und zu optimieren oder die Computegröße oder Dienstschicht zu aktualisieren, um die Kapazität Ihrer Azure SQL-Datenbank zur Aufnahme der CPU-Anforderungen zu erhöhen. Weitere Informationen zur Skalierung von Ressourcen für einzelne Datenbanken finden Sie unter [Skalieren einzelner Datenbankressourcen in Azure SQL-Datenbank](sql-database-single-database-scale.md) und zur Skalierung von Ressourcen für elastische Pools unter [Skalieren von elastischen Poolressourcen in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md).

- **Eine einzelne Abfrage wartet auf etwas**

  Einzelne Abfragen können Leistungsprobleme aufweisen, da die Abfrage auf etwas wartet. In diesem Szenario ist Ihr Ziel die Wartezeit zu entfernen oder zu verringern.

### <a name="determine-if-you-have-a-running-related-performance-issue"></a>Bestimmen, ob ein ausführungsbedingtes Leistungsproblem vorliegt

Ausführungsbedingte Leistungsprobleme können Sie mit mehreren Methoden identifizieren. Die am häufigsten verwendeten Methoden sind:

- Verwenden Sie das [Azure-Portal](#monitor-databases-using-the-azure-portal), um die prozentuale CPU-Auslastung zu überwachen.
- Verwenden Sie die folgenden [dynamische Verwaltungssichten](sql-database-monitoring-with-dmvs.md):

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) gibt die CPU-, E/A- und Arbeitsspeichernutzung für eine Azure SQL-Datenbank zurück. Für alle 15 Sekunden ist eine Zeile vorhanden, selbst wenn keine Aktivität in der Datenbank erfolgt ist. Historische Daten werden eine Stunde lang aufbewahrt.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) gibt CPU-Nutzungs- und Speicherdaten für eine Azure SQL-Datenbank zurück. Die Daten werden in Intervallen von fünf Minuten gesammelt und aggregiert.

> [!TIP]
> Als allgemeine Richtlinie gilt: Wenn Ihre CPU-Auslastung konstant bei oder über 80 % liegt, haben Sie ein ausführungsbedingtes Leistungsproblem.

### <a name="determine-if-you-have-a-waiting-related-performance-issue"></a>Bestimmen, ob ein wartezeitbedingtes Leistungsproblem vorliegt

Stellen Sie zuerst sicher, dass kein ausführungsbedingtes Leistungsproblem aufgrund einer hohen CPU-Auslastung vorliegt. Wenn dies nicht der Fall ist, müssen Sie im nächsten Schritt die mit Ihrem Anwendungs-Workload verbundenen wichtigsten Wartevorgänge identifizieren.  Gängige Methoden zur Anzeige der wichtigsten Wartetypkategorien:

- Der [Abfragedatenspeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) bietet eine Wartestatistik pro Abfrage im Zeitverlauf. Im Abfragedatenspeicher sind Wartetypen mit Wartekategorien kombiniert. Die Zuordnung von Wartekategorien zu Wartetypen finden Sie in [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql?view=sql-server-2017#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) gibt Informationen über alle Wartezeiten zurück, die von Threads auftreten, die während des Betriebs ausgeführt werden. Sie können diese aggregierte Ansicht verwenden, um Leistungsprobleme mit der Azure SQL-Datenbank und auch mit bestimmten Abfragen und Batches zu diagnostizieren.
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) gibt Informationen zur Warteschlange von Aufgaben, die bei einigen Ressource warten.

Wie im vorherigen Diagramm dargestellt, sind die am häufigsten Wartetypen:

- Sperren (Blockieren)
- E/A
- Tempdb-bezogener Konflikt
- Wartetyp „Speicherzuweisung“

Je nachdem, was angezeigt wird, verfügt jede Wartekategorie über einen anderen Pfad für die Problembehandlung.

## <a name="overview-of-monitoring-database-performance-in-azure-sql-database"></a>Übersicht über die Überwachung der Datenbankleistung in Azure SQL-Datenbank

Die Überwachung der Leistung einer SQL-Datenbank in Azure beginnt mit der Überwachung der Ressourcennutzung relativ zur gewählten Datenbankleistung. Dank der Überwachung können Sie ermitteln, ob Ihre Datenbank über Zusatzkapazität verfügt, oder ob Probleme infolge ausgeschöpfter Kapazität auftreten. Anschließend können Sie entscheiden, ob Computegröße und Diensttarife Ihrer Datenbank im [DTU-basierten Kaufmodell](sql-database-service-tiers-dtu.md) oder [V-Kern-basierten Kaufmodell](sql-database-service-tiers-vcore.md) angepasst werden müssen. Sie können die Datenbank im [Azure-Portal](https://portal.azure.com) mithilfe der folgenden grafischen Tools oder in SQL mithilfe [dynamischer Verwaltungssichten](sql-database-monitoring-with-dmvs.md) überwachen.

Mit Azure SQL-Datenbank können Sie Möglichkeiten zur Verbesserung und Optimierung der Abfrageleistung identifizieren, ohne dass Sie Ressourcen durch Überprüfen des [Datenbankratgebers](sql-database-advisor.md) ändern müssen. Fehlende Indizes und falsch optimierte Abfragen sind häufige Ursachen für eine schlechte Datenbankleistung. Sie können diese Empfehlungen zur Optimierung anwenden, um die Leistung Ihrer Workload zu verbessern.
Sie können die Leistung Ihrer Abfragen auch automatisch durch Azure SQL-Datenbank optimieren lassen, indem Sie alle erkannten Empfehlungen anwenden und bestätigen, dass diese die Datenbankleistung verbessern (weitere Informationen finden Sie unter [automatically optimize performance of your queries (Automatisches Optimieren der Leistung Ihrer Abfragen)](sql-database-automatic-tuning.md)). Es gibt folgende Optionen für die Überwachung und Problembehandlung der Datenbankleistung:

- Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **SQL-Datenbanken**, wählen Sie die Datenbank aus, und suchen Sie dann mithilfe des Diagramms „Überwachung“ nach Ressourcen, die sich ihrer maximalen Leistung annähern. „DTU-Verbrauch“ wird standardmäßig angezeigt. Klicken Sie auf **Bearbeiten** , um den Zeitraum und die angezeigten Werte zu ändern.
- Verwenden Sie die [Statistik zur Abfrageleistung](sql-database-query-performance.md), um die Abfragen zu identifizieren, die die meisten Ressourcen verbrauchen.
- Verwenden Sie [SQL Database Advisor](sql-database-advisor-portal.md), um Empfehlungen zum Erstellen und Löschen von Indizes, zum Parametrisieren von Abfragen und zum Beheben von Schemaproblemen anzuzeigen.
- Mit [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md) können Sie die Leistung Ihrer Datenbank automatisch überwachen. Bei Erkennung eines Leistungsproblems wird ein Diagnoseprotokoll mit Details und einer Fehlerursachenanalyse des Problems generiert. Empfehlungen zur Verbesserung der Leistung werden nach Möglichkeit bereitgestellt.
- [Aktivieren Sie die automatische Optimierung](sql-database-automatic-tuning-enable.md), und lassen Sie erkannte Leistungsprobleme automatisch durch Azure SQL-Datenbank verbessern.
- Sie können auch [dynamische Verwaltungsansichten (DMVs)](sql-database-monitoring-with-dmvs.md), erweiterte Ereignisse (`XEvents`) (sql-database/sql-database-xevent-db-diff-from-svr.md) und den [Abfragespeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) verwenden, um Leistungsparameter in Echtzeit abzurufen. Im [Leistungsleitfaden](sql-database-performance-guidance.md) finden Sie Methoden zur Leistungsverbesserung von Azure SQL-Datenbank, wenn Sie mithilfe dieser Berichte oder Ansichten Probleme feststellen.

## <a name="monitor-databases-using-the-azure-portal"></a>Überwachen von Datenbanken über das Azure-Portal

Im [Azure-Portal](https://portal.azure.com/) können Sie die Nutzung einer Einzeldatenbank überwachen, indem Sie die Datenbank auswählen und auf das Diagramm **Überwachung** klicken. Dadurch wird das Fenster **Metrik** geöffnet, in dem Sie durch Klicken auf die Schaltfläche **Diagramm bearbeiten** Änderungen vornehmen können. Fügen Sie die folgenden Metriken hinzu:

- CPU-Prozentsatz
- DTU-Prozentsatz
- E/A-Prozentsatz für Daten
- Datenbankgröße als Prozentsatz

Nachdem Sie diese Metriken hinzugefügt haben, können Sie sie im Diagramm **Überwachung** mit weiteren Informationen im Fenster **Metrik** anzeigen. Alle vier Metriken geben die durchschnittliche prozentuale Nutzung relativ zur **DTU** der Datenbank an. In den Artikeln [DTU-basiertes Kaufmodell für Azure SQL-Datenbank](sql-database-service-tiers-dtu.md) und [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md) finden Sie weitere Informationen zu Dienstebenen.  

![Tarifbezogenes Überwachen der Datenbankleistung.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

Sie können zudem Benachrichtigungen für die Leistungsmetriken konfigurieren. Klicken Sie im Fenster **Metrik** auf die Schaltfläche **Warnung hinzufügen**. Befolgen Sie die Anweisungen des Assistenten, um die Benachrichtigung zu konfigurieren. Sie haben die Möglichkeit, Benachrichtigungen für den Fall zu konfigurieren, dass Metriken einen Schwellenwert überschreiten oder unterschreiten.

Wenn Sie beispielsweise einen Anstieg der Workload Ihrer Datenbank erwarten, können Sie eine E-Mail-Benachrichtigung konfigurieren, die immer dann gesendet wird, wenn eine der Leistungsmetriken der Datenbank 80 % erreicht. Sie können dies als Frühwarnung verwenden, um zu ermitteln, wann Sie eventuell zur nächsthöheren Computegröße wechseln müssen.

Anhand der Leistungsmetriken können Sie auch ermitteln, ob Sie möglicherweise eine Herabstufung auf eine niedrigere Computegröße vornehmen können. Angenommen, Sie verwenden eine Datenbank der Dienstebene "Standard S2", und alle Leistungsmetriken zeigen, dass die Datenbank zu jedem Zeitpunkt durchschnittlich nicht mehr als 10 % nutzt. Hier ist es wahrscheinlich, dass sich die Datenbank auch mit der Dienstebene "Standard S1" verwenden lässt. Bevor Sie sich für einen Wechsel zu einer niedrigeren Computegröße entscheiden, müssen Sie aber auch Workloads berücksichtigen, die Spitzen oder Schwankungen aufweisen können.

## <a name="improving-database-performance-with-more-resources"></a>Verbesserung der Datenbankleistung mit mehr Ressourcen

Schließlich können Sie noch die Anzahl der in Azure SQL-Datenbank verfügbaren Ressourcen ändern, wenn es keine umsetzbaren Elemente gibt, die die Leistung Ihrer Datenbank verbessern können. Sie können mehr Ressourcen zuweisen, indem Sie den [DTU-Diensttarif ](sql-database-service-tiers-dtu.md) einer Einzeldatenbank verändern oder die eDTUs eines Pools für elastische Datenbanken zu einem beliebigen Zeitpunkt erhöhen. Alternativ können Sie bei Verwendung des [V-Kern-basierten Kaufmodells](sql-database-service-tiers-vcore.md) entweder die Dienstebene ändern oder die Ressourcen heraufsetzen, die der Datenbank zugeordnet werden.

1. Bei Einzeldatenbanken können Sie bedarfsgesteuert die [Diensttarife](sql-database-service-tiers-dtu.md) oder [Computeressourcen](sql-database-service-tiers-vcore.md) ändern, um die Datenbankleistung zu steigern.
2. Ziehen Sie bei mehreren Datenbanken [Pools für elastische Datenbanken](sql-database-elastic-pool-guidance.md) in Betracht, um Ressourcen automatisch zu skalieren.

## <a name="tune-and-refactor-application-or-database-code"></a>Optimieren und Ändern von Anwendungs- oder Datenbankcode

Sie können Anwendungscode ändern, um effektiver die Datenbank zu nutzen, Indizes zu verändern, Pläne zu erzwingen oder Hinweise zu nutzen, um die Datenbank manuell an Ihre Workload anzupassen. Im [Leistungsleitfaden](sql-database-performance-guidance.md) finden Sie hilfreiche Informationen und Tipps zur Überwachung und Optimierung.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Aktivierung der automatischen Optimierung in Azure SQL-Datenbank und zur Verwendung für die vollständige Verwaltung Ihrer Workload finden Sie unter [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).
- Bei der Verwendung der manuellen Optimierung helfen Ihnen die Informationen unter [Azure SQL Database Advisor im Azure-Portal](sql-database-advisor-portal.md) weiter. Außerdem können Sie manuell die Empfehlungen anwenden, die zu einer Verbesserung der Leistung Ihrer Abfragen führen.
- Ändern Sie in der Datenbank verfügbare Ressourcen, indem Sie [Dienst- und Leistungsebenen für Azure SQL-Datenbanken](sql-database-performance-guidance.md) ändern.

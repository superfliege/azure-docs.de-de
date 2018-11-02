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
ms.date: 10/23/2018
ms.openlocfilehash: 0d728d81a29c5520938c8553c026727c0f94cc43
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957002"
---
# <a name="monitoring-and-performance-tuning"></a>Überwachen und Optimieren der Datenbankleistung

Azure SQL-Datenbank ist ein automatisch verwalteter und flexibler Datendienst, in dem Sie leicht die Nutzung überwachen, Ressourcen (CPU, Arbeitsspeicher, E/A) hinzufügen oder entfernen und Empfehlungen zur Leistungsverbesserung Ihrer Datenbank suchen können. Außerdem können Sie die Datenbank an Ihre Workload anpassen lassen und automatisch die Leistung optimieren.

## <a name="monitoring-database-performance"></a>Überwachen der Datenbankleistung

Die Überwachung der Leistung einer SQL-Datenbank in Azure beginnt mit der Überwachung der Ressourcennutzung relativ zur gewählten Datenbankleistung. Mit Azure SQL-Datenbank können Sie Möglichkeiten zur Verbesserung und Optimierung der Abfrageleistung identifizieren, ohne dass Sie Ressourcen durch Überprüfen des [Datenbankratgebers](sql-database-advisor.md) ändern müssen. Fehlende Indizes und falsch optimierte Abfragen sind häufige Ursachen für eine schlechte Datenbankleistung. Sie können diese Empfehlungen zur Optimierung anwenden, um die Leistung Ihrer Workload zu verbessern. Sie können die Leistung Ihrer Abfragen auch automatisch durch Azure SQL-Datenbank optimieren lassen, indem Sie alle erkannten Empfehlungen anwenden und bestätigen, dass diese die Datenbankleistung verbessern (weitere Informationen finden Sie unter [automatically optimize performance of your queries (Automatisches Optimieren der Leistung Ihrer Abfragen)](sql-database-automatic-tuning.md)).

Es gibt folgende Optionen für die Überwachung und Problembehandlung der Datenbankleistung:

- Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **SQL-Datenbanken**, wählen Sie die Datenbank aus, und suchen Sie dann mithilfe des Diagramms „Überwachung“ nach Ressourcen, die sich ihrer maximalen Leistung annähern. „DTU-Verbrauch“ wird standardmäßig angezeigt. Klicken Sie auf **Bearbeiten** , um den Zeitraum und die angezeigten Werte zu ändern.
- Verwenden Sie die [Statistik zur Abfrageleistung](sql-database-query-performance.md), um die Abfragen zu identifizieren, die die meisten Ressourcen verbrauchen.
- Verwenden Sie [SQL Database Advisor](sql-database-advisor-portal.md), um Empfehlungen zum Erstellen und Löschen von Indizes, zum Parametrisieren von Abfragen und zum Beheben von Schemaproblemen anzuzeigen.
- Mit [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md) können Sie die Leistung Ihrer Datenbank automatisch überwachen. Bei Erkennung eines Leistungsproblems wird ein Diagnoseprotokoll mit Details und einer Fehlerursachenanalyse des Problems generiert. Empfehlungen zur Verbesserung der Leistung werden nach Möglichkeit bereitgestellt.
- [Aktivieren Sie die automatische Optimierung](sql-database-automatic-tuning-enable.md), und lassen Sie erkannte Leistungsprobleme automatisch durch Azure SQL-Datenbank verbessern.
- Verwenden Sie [dynamische Verwaltungsansichten (DMVs)](sql-database-monitoring-with-dmvs.md), [erweiterte Ereignisse](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-db-diff-from-svr) und den [Abfragespeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store), um detaillierte Informationen zur Behebung von Leistungsproblemen zu erhalten.

> [!TIP]
> Im [Leistungsleitfaden](sql-database-performance-guidance.md) finden Sie Methoden, mit denen Sie die Leistung von Azure SQL-Datenbank verbessern können, nachdem Sie das Leistungsproblem mithilfe einer oder mehrerer der oben genannten Methoden ermittelt haben.

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

## <a name="troubleshoot-performance-issues"></a>Behandeln von Leistungsproblemen

Beim Diagnostizieren und Beheben von Leistungsproblemen besteht der erste Schritt darin, den Status jeder aktiven Abfrage und die Bedingungen zu ermitteln, die zu Leistungsproblemen in Bezug auf den jeweiligen Workloadstatus führen. Um die Leistung der Azure SQL-Datenbank zu verbessern, sollten Sie sich darüber im Klaren sein, dass sich jede aktive Abfrageanforderung Ihrer Anwendung entweder in einem laufenden oder einem wartenden Zustand befindet. Wenn Sie die Informationen in diesem Artikel verwenden, um Leistungsprobleme in Azure SQL-Datenbank zu diagnostizieren und zu beheben, behalten Sie das folgende Diagramm im Kopf.

![Workload-Status](./media/sql-database-monitor-tune-overview/workload-states.png)

Bei einem Workload mit Leistungsproblemen kann das Leistungsproblem auf CPU-Konflikte zurückzuführen sein (eine **running-related**-Bedingung) oder einzelne Abfragen warten auf etwas (eine **waitingrelated**-Bedingung).

## <a name="running-related-performance-issues"></a>Leistungsprobleme in Zusammenhang mit der Ausführung

Als allgemeine Richtlinie gilt: Wenn Ihre CPU-Auslastung konstant bei oder über 80 % liegt, haben Sie ein ausführungsbedingtes Leistungsproblem. Wenn ein Problem in Zusammenhang mit der Ausführung vorliegt, kann dies durch unzureichende CPU-Ressourcen oder durch eine der folgenden Bedingungen verursacht werden:

- Zu viele ausgeführte Abfragen
- Zu viele kompilierte Abfragen
- Eine oder mehrere ausgeführte Abfragen verwenden einen suboptimalen Abfrageplan.

Wenn Sie feststellen, dass ein Problem in Zusammenhang mit der Ausführung vorliegt, besteht Ihr Ziel darin, mithilfe einer oder mehrerer Methoden die genaue Ursache herauszufinden. Die gängigsten Methoden zum Ermitteln von Probleme in Zusammenhang mit der Ausführung sind die folgenden:

- Verwenden Sie das [Azure-Portal](#monitor-databases-using-the-azure-portal), um die prozentuale CPU-Auslastung zu überwachen.
- Verwenden Sie die folgenden [dynamische Verwaltungssichten](sql-database-monitoring-with-dmvs.md):

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) gibt die CPU-, E/A- und Arbeitsspeichernutzung für eine Azure SQL-Datenbank zurück. Für alle 15 Sekunden ist eine Zeile vorhanden, selbst wenn keine Aktivität in der Datenbank erfolgt ist. Historische Daten werden eine Stunde lang aufbewahrt.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) gibt CPU-Nutzungs- und Speicherdaten für eine Azure SQL-Datenbank zurück. Die Daten werden in Intervallen von fünf Minuten gesammelt und aggregiert.

> [!IMPORTANT]
> Unter [Ermitteln von Problemen mit der CPU-Auslastung](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues) finden Sie eine Reihe von T-SQL-Abfragen, bei denen diese DMVs zum Beheben von Problemen mit der CPU-Auslastung verwendet werden.

Sobald Sie das Problem identifiziert haben, können Sie die problematischen Abfragen optimieren oder die Computegröße oder Dienstebene erhöhen, um die Kapazität Ihrer Azure SQL-Datenbank zu steigern, damit die CPU-Anforderungen erfüllt werden können. Informationen zur Skalierung der Ressourcen für einzelne Datenbanken finden Sie unter [Skalieren einzelner Datenbankressourcen in Azure SQL-Datenbank](sql-database-single-database-scale.md). Um die Ressourcen für Pools für elastische Datenbanken zu skalieren, lesen Sie [Skalieren von Ressourcen für Pools für elastische Datenbanken in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md). Informationen zum Skalieren einer verwalteten Instanz finden Sie unter [Ressourcenlimits auf Instanzebene](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).

## <a name="waiting-related-performance-issues"></a>Leistungsprobleme in Zusammenhang mit Wartevorgängen

Wenn Sie sicher sind, dass kein Problem in Zusammenhang mit der Ausführung und CPU-Auslastung vorliegt, wird das Problem durch Wartevorgänge verursacht. Kurz gesagt: Ihre CPU-Ressourcen werden nicht effizient genutzt, weil die CPU auf eine andere Ressource wartet. In diesem Fall müssen Sie herausfinden, worauf Ihre CPU-Ressourcen warten. Hier finden Sie die gängigsten Methoden zur Anzeige der wichtigsten Wartetypkategorien:

- Der [Abfragedatenspeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) bietet eine Wartestatistik pro Abfrage im Zeitverlauf. Im Abfragedatenspeicher sind Wartetypen mit Wartekategorien kombiniert. Die Zuordnung von Wartekategorien zu Wartetypen finden Sie in [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql?view=sql-server-2017#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) gibt Informationen über alle Wartezeiten zurück, die von Threads auftreten, die während des Betriebs ausgeführt werden. Sie können diese aggregierte Ansicht verwenden, um Leistungsprobleme mit der Azure SQL-Datenbank und auch mit bestimmten Abfragen und Batches zu diagnostizieren.
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) gibt Informationen zur Warteschlange von Aufgaben, die bei einigen Ressource warten.

Wie im vorherigen Diagramm dargestellt, sind die am häufigsten Wartetypen:

- Sperren (Blockieren)
- E/A
- Konflikt in Zusammenhang mit `tempdb`
- Wartetyp „Speicherzuweisung“

> [!IMPORTANT]
> In den folgenden Artikeln finden Sie eine Reihe von T-SQL-Abfragen, bei denen diese DMVs verwendet werden, um Probleme aufgrund von Wartevorgängen zu beheben:
>
> - [Identifizieren von Problemen mit der E/A-Leistung](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identifizieren von Leistungsproblemen mit ](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)`tempdb`
> - [Identifizieren von Problemen mit Wartevorgängen aufgrund von Speicherzuweisungen](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)

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

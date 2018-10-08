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
ms.date: 07/16/2018
ms.openlocfilehash: c79285247950510791ede915fcf0e5373792044f
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165040"
---
# <a name="monitoring-and-performance-tuning"></a>Überwachen und Optimieren der Datenbankleistung

Azure SQL-Datenbank ist ein automatisch verwalteter und flexibler Datendienst, in dem Sie leicht die Nutzung überwachen, Ressourcen (CPU, Arbeitsspeicher, E/A) hinzufügen oder entfernen und Empfehlungen zur Leistungsverbesserung Ihrer Datenbank suchen können. Außerdem können Sie die Datenbank an Ihre Workload anpassen lassen und automatisch die Leistung optimieren.

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
Im [Azure-Portal](https://portal.azure.com/)können Sie die Nutzung einer Einzeldatenbank überwachen, indem Sie die Datenbank auswählen und auf das Diagramm **Überwachung** klicken. Dadurch wird das Fenster **Metrik** geöffnet, in dem Sie durch Klicken auf die Schaltfläche **Diagramm bearbeiten** Änderungen vornehmen können. Fügen Sie die folgenden Metriken hinzu:

* CPU-Prozentsatz
* DTU-Prozentsatz
* E/A-Prozentsatz für Daten
* Datenbankgröße als Prozentsatz

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

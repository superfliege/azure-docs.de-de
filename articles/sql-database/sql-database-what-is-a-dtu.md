---
title: 'SQL-Datenbank: Was ist eine DTU? | Microsoft-Dokumentation'
description: Hier wird beschrieben, was eine Azure SQL-Datenbank-Transaktionseinheit ist.
keywords: Datenbankoptionen, Datenbankleistung
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 50b12de2400f0db90108e3eeb158357039ac92ec
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649821"
---
# <a name="database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs)
In diesem Artikel werden Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs) beschrieben. Außerdem wird erläutert, was passiert, wenn die Obergrenze für DTUs oder eDTUs erreicht ist. Konkrete Preisinformationen finden Sie unter [Azure SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/single/).

## <a name="what-are-database-transaction-units-dtus"></a>Was sind Datenbanktransaktionseinheiten (DTUs)?
Für eine einzelne Azure SQL-Datenbank auf einer bestimmten Leistungsebene innerhalb einer [Dienstebene](sql-database-single-database-resources.md) garantiert Microsoft einen bestimmten Ressourcenumfang für diese Datenbank (unabhängig von jeder anderen Datenbank in der Azure-Cloud), wobei eine vorhersagbare Leistungsebene geboten wird. Der Ressourcenumfang wird als Anzahl von Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) berechnet. Es handelt sich dabei um ein Paket aus Compute-, Speicher- und E/A-Ressourcen. Das Verhältnis zwischen diesen Ressourcen wurde ursprünglich anhand einer [OLTP-Benchmark-Workload](sql-database-benchmark-overview.md) ermittelt, die für realistische OLTP-Workloads typisch ist. Wenn Ihre Workload den Umfang einer dieser Ressourcen überschreitet, wird der Durchsatz gedrosselt, wodurch eine niedrigere Leistung und Timeouts verursacht werden. Die Ressourcen, die von Ihrer Workload verwendet werden, wirken sich nicht auf die Ressourcen aus, die für andere SQL-Datenbanken in der Azure-Cloud verfügbar sind, und die von anderen Workloads verwendeten Ressourcen wirken sich nicht auf die Ressourcen aus, die für Ihre SQL-Datenbank verfügbar sind.

![Begrenzungsrahmen](./media/sql-database-what-is-a-dtu/bounding-box.png)

DTUs sind besonders nützlich, um den relativen Umfang von Ressourcen zu verstehen, den es zwischen Azure SQL-Datenbank-Instanzen auf unterschiedlichen Leistungsebenen und Dienstebenen gibt. Beispielsweise entspricht eine Verdoppelung der DTUs durch Erhöhen der Leistungsebene einer Datenbank der Verdoppelung der Ressourcen, die dieser Datenbank zur Verfügung stehen. Beispiel: Eine Premium P11-Datenbank mit 1.750 DTUs bietet eine 350 Mal höhere DTU-Computeleistung als eine Basic-Datenbank mit fünf DTUs.  

Um einen tieferen Einblick in den Ressourcenverbrauch (DTU) Ihrer Workload zu erhalten, verwenden Sie [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md) für Folgendes:

- Ermitteln der häufigsten Abfragen nach den Werten von CPU-Nutzung/Dauer/Ausführungshäufigkeit, bei denen unter Umständen eine Leistungssteigerung erzielt werden kann. Beispielsweise könnte eine E/A-intensive Abfrage davon profitieren, dass [In-Memory-Optimierungstechniken](sql-database-in-memory.md) verwendet werden, um den verfügbaren Arbeitsspeicher auf einer bestimmte Dienst- und Leistungsebene besser zu nutzen.
- Analysieren der Details einer Abfrage sowie Anzeigen von deren Text und deren Verlauf der Ressourcenverwendung.
- Zugreifen auf Empfehlungen zur Leistungsoptimierung, die Aktionen erläutern, die vom [SQL Database Advisor](sql-database-advisor.md) ausgeführt werden.

Sie können [DTU-Dienstebenen](sql-database-service-tiers-dtu.md) jederzeit ändern und die Ausfallzeiten für Ihre Anwendung dabei gering halten (im Durchschnitt meist unter vier Sekunden). Für viele Unternehmen und Apps genügt es, wenn Datenbanken erstellt werden können und sich die Leistung nach oben oder unten anpassen lässt – insbesondere, wenn die Nutzungsmuster relativ gut vorhersagbar sind. Bei unvorhersagbaren Nutzungsmustern kann es jedoch schwer sein, die Kosten und Ihr Geschäftsmodell zu verwalten. In diesem Szenario verwenden Sie einen Pool für elastische Datenbanken mit einer bestimmten Anzahl von eDTUs, die von mehreren Datenbanken im Pool gemeinsam genutzt werden.

![Einführung in SQL-Datenbank: Einzeldatenbank-DTUs nach Dienst- und Leistungsebene](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Was sind elastische Datenbanktransaktionseinheiten (eDTUs)?
Statt einen dedizierten Satz von Ressourcen (DTUs) bereitzustellen, die möglicherweise nicht immer für eine immer verfügbare SQL-Datenbank benötigt werden, können Sie Datenbanken in einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md) auf einem SQL-Datenbankserver platzieren, der für diese Datenbanken einen gemeinsam Pool von Ressourcen freigibt. Die freigegebenen Ressourcen in einem Pool für elastische Datenbanken werden als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) gemessen. Pools für elastische Datenbanken stellen eine einfache und kostengünstige Lösung zum Verwalten der Leistungsziele für mehrere Datenbanken mit unterschiedlichsten und unvorhersehbaren Nutzungsmustern dar. Durch einen Pool für elastische Datenbanken wird sichergestellt, dass Ressourcen nicht von einer einzigen Datenbank im Pool genutzt werden können, wobei jeder Datenbank im Pool immer eine Mindestmenge an erforderlichen Ressourcen zur Verfügung steht. 

![Einführung in SQL-Datenbank: eDTUs nach Dienst- und Leistungsebene](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Ein Pool erhält eine festgelegte Anzahl von eDTUs zu einem festen Preis. Im elastischen Pool können einzelne Datenbanken die automatische Skalierung innerhalb der konfigurierten Grenzen flexibel ausführen. Eine Datenbank mit hoher Auslastung nutzt mehr eDTUs, um die Anforderungen zu erfüllen. Datenbanken mit geringerer Auslastung verwenden weniger eDTUs. Datenbanken ohne Auslastung verwenden keine eDTUs. Durch die Bereitstellung von Ressourcen für den gesamten Pool statt pro Datenbank werden Verwaltungsaufgaben vereinfacht, und Sie verfügen über ein vorhersagbares Budget für den Pool.

Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen und Auswirkungen auf die Datenbanken im Pool kommt. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden. Sie können Datenbanken zum Pool hinzufügen oder daraus entfernen oder die Anzahl von eDTUs beschränken, die eine Datenbank mit hoher Auslastung verwenden kann, um eDTUs für andere Datenbanken zu reservieren. Wenn eine Datenbank die Ressourcen voraussichtlich nicht hinreichend nutzt, können Sie sie aus dem Pool entfernen und als einzelne Datenbank mit der voraussichtlichen Menge an erforderlichen Ressourcen konfigurieren.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Wie kann ich die Anzahl von DTUs bestimmen, die für meine Workload erforderlich sind?
Wenn Sie die vorhandene Workload eines lokalen oder virtuellen SQL Server-Computers zu Azure SQL-Datenbank migrieren möchten, können Sie den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner) nutzen, um die ungefähre Anzahl von benötigten DTUs zu berechnen. Für eine vorhandene Azure SQL-Datenbank-Workload können Sie [Query Performance Insight für SQL-Datenbank](sql-database-query-performance.md) verwenden, um den Ressourcenverbrauch (DTUs) der Datenbank zu verstehen und einen tieferen Einblick zur Optimierung Ihrer Workload zu gewinnen. Sie können auch die DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) verwenden, um den Ressourcenverbrauch der letzten Stunde anzuzeigen. Alternativ dazu wird in der Katalogansicht [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) der Ressourcenverbrauch der letzten 14 Tage angezeigt. Da hierfür Durchschnittswerte für jeweils fünf Minuten genutzt werden, ist jedoch die Genauigkeit geringer.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Wie kann ermittelt werden, ob sich durch einen elastischen Ressourcenpool Vorteile für mich ergeben?
Pools eignen sich gut für eine große Anzahl von Datenbanken mit spezifischen Nutzungsmustern. Im Hinblick auf eine bestimmte Datenbank ist dieses Muster durch eine geringe durchschnittliche Auslastung mit relativ wenigen Nutzungslastspitzen gekennzeichnet. SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbank-Server und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Weitere Informationen finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool.md).

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Was passiert, wenn die Obergrenze der DTUs erreicht ist?
Leistungsebenen werden kalibriert und gesteuert, um die erforderlichen Ressourcen zum Ausführen der Workload Ihrer Datenbank bis zu den maximalen Grenzwerten bereitzustellen, die für die ausgewählte Dienstebene/Leistungsebene zulässig sind. Wenn Ihre Workload einen der CPU-/Daten-E/A/Protokoll-E/A-Grenzwerte erreicht, erhalten Sie weiterhin die maximal zulässigen Ressourcen, aber es kommt wahrscheinlich auch zu höheren Abfragelatenzen. Diese Höchstgrenzen führen nicht zu Fehlern, sondern nur zu einer Verlangsamung Ihrer Workload. Wenn die Verlangsamung aber zu schwerwiegend ist, tritt ein Timeout für die Abfragen auf. Wenn Sie die maximal zulässigen gleichzeitigen Benutzersitzungen/-anforderungen (Arbeitsthreads) erreichen, erhalten Sie explizite Fehler. Informationen zu Ressourcengrenzwerten, die sich nicht auf CPU, Arbeitsspeicher, Daten-E/A oder Transaktionsprotokoll-E/A beziehen, finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank]( sql-database-dtu-resource-limits.md#what-happens-when-database-and-elastic-pool-resource-limits-are-reached).

## <a name="next-steps"></a>Nächste Schritte
* Unter [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) finden Sie Informationen zu den DTUs und eDTUs, die für einzelne Datenbanken und Pools für elastische Datenbanken verfügbar sind, und zu Grenzwerten für andere Ressourcen als CPU, Arbeitsspeicher, Daten-E/A und Transaktionsprotokoll-E/A.
* Informationen zu auf virtuellen Kernen basierenden Ressourcenzuordnungen und Dienstebenen finden Sie unter [V-Kern-basiertes Kaufmodell (Vorschauversion)](sql-database-service-tiers-vcore.md). 
* Informationen zum Verbrauch (DTUs) finden Sie unter [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md).
* Informationen zur Methodik hinter der OLTP-Benchmark-Workload, die zum Ermitteln der DTU-Mischung verwendet wird, finden Sie unter [Übersicht über das Durchführen von Vergleichstests für Azure SQL-Datenbank](sql-database-benchmark-overview.md).

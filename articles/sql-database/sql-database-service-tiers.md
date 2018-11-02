---
title: Kaufmodelle für Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erhalten Sie Informationen zu den Kaufmodellen, die für Datenbanken im Azure SQL-Datenbank-Dienst verfügbar sind.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/19/2018
ms.openlocfilehash: 9dc993b6ba9b90dfa61660df004ef7dae8b93fa7
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49470953"
---
# <a name="azure-sql-database-purchasing-models"></a>Kaufmodelle für Azure SQL-Datenbank

Mit Azure SQL-Datenbank können Sie problemlos eine vollständig verwaltete PaaS-Datenbank-Engine erwerben, die an Ihre Anforderungen an Leistung und Kosten angepasst ist. Je nach Bereitstellungsmodell von Azure SQL-Datenbank können Sie das Kaufmodell auswählen, das Ihren Anforderungen entspricht:

- [Logische Server](sql-database-logical-servers.md) in [Azure SQL-Datenbank](sql-database-technical-overview.md) verfügen über zwei Kaufmodelle für Compute-, Speicher- und E/A-Ressourcen: ein [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) und ein [V-Kern-basiertes Kaufmodel](sql-database-service-tiers-vcore.md). Innerhalb dieses Kaufmodells können Sie [Einzeldatenbanken](sql-database-single-databases-manage.md) oder [Pools für elastische Datenbanken](sql-database-elastic-pool.md) auswählen.
- [Verwaltete Instanzen](sql-database-managed-instance.md) in Azure SQL-Datenbank bieten nur das auf [virtuellen Kernen basierende Kaufmodell](sql-database-service-tiers-vcore.md).

> [!IMPORTANT]
> [Hyperscaledatenbanken (Vorschau)](sql-database-service-tier-hyperscale.md) befinden sich in der öffentlichen Vorschau und gelten nur für Einzeldatenbanken, die das V-Kern-Kaufmodell verwenden.

In der folgenden Tabelle und im Diagramm werden diese Kaufmodelle verglichen und gegenübergestellt.

|**Kaufmodell**|**Beschreibung**|**Am besten geeignet für**|
|---|---|---|
|DTU-basiertes Modell|Dieses Modell basiert auf einem Paket mit Compute-, Speicher- und E/A-Ressourcen. Computegrößen werden für Einzeldatenbanken als Datenbanktransaktionseinheiten (DTUs) und für Pools für elastische Datenbanken als elastische Datenbanktransaktionseinheiten (eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-service-tiers.md#dtu-based-purchasing-model).|Eignet sich am besten für Kunden, die einfache, vorkonfigurierte Ressourcenoptionen benötigen.|
|V-Kern-basiertes Modell|Mit diesem Modell können Sie Compute- und Speicherressourcen einzeln auswählen. Außerdem können Sie den Azure-Hybridvorteil für SQL Server verwenden, um Kosten zu sparen.|Eignet sich am besten für Kunden, für die Flexibilität, Kontrolle und Transparenz im Vordergrund stehen.|
||||  

![Preismodell](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>Auf virtuellen Kernen basierendes Erwerbsmodell

Ein virtueller Kern repräsentiert die logische CPU. Virtuelle Kerne werden für verschiedene Hardwaregenerationen und physische Hardwaremerkmale (z.B. Anzahl der Kerne, Arbeitsspeicher, Speichergröße) angeboten. Beim V-Kern-basierten Kaufmodell erhalten Sie Flexibilität, Kontrolle und Transparenz in Bezug auf den individuellen Ressourcenverbrauch. Außerdem können Sie die lokalen Workloadanforderungen leicht auf die Cloud übertragen. Mit diesem Modell können Sie Computeressourcen, Arbeitsspeicher und Speicher entsprechend den jeweiligen Workloadanforderungen auswählen. Beim V-Kern-basierten Kaufmodell können Sie für [Einzeldatenbanken](sql-database-single-database-scale.md), [verwaltete Instanzen](sql-database-managed-instance.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) zwischen den Diensttarifen [Universell](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) und [Unternehmenskritisch](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) wählen. Für Einzeldatenbanken können Sie auch den Diensttarif [Hyperscale (Vorschau)](sql-database-service-tier-hyperscale.md) auswählen.

Mit dem V-Kern-basierten Kaufmodell können Sie Compute- und Speicherressourcen einzeln auswählen, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis optimieren. Beim V-Kern-basierten Kaufmodell zahlen Kunden für Folgendes:

- Compute (Diensttarif + Anzahl von V-Kernen und Arbeitsspeichermenge + Hardwaregeneration)
- Typ und Menge von Daten und Protokollspeicher
- Sicherungsspeicher (RA-GRS)

> [!IMPORTANT]
> Compute, E/A-Vorgänge, Daten und Protokollspeicher werden pro Datenbank oder Pool für elastische Datenbanken berechnet. Sicherungsspeicher wird pro Datenbank berechnet. Ausführlichere Informationen zur verwalteten SQL-Datenbank-Instanz finden Sie unter [Was ist eine verwaltete Instanz (Vorschauversion)?](sql-database-managed-instance.md).
> **Regionale Einschränkungen:** Das V-Kern-basierte Kaufmodell ist in folgenden Regionen noch nicht verfügbar: „Europa, Westen“, „Frankreich, Mitte“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“ und „Australien, Südosten“.

Wenn Ihre Datenbank oder der Pool für elastische Datenbanken mehr als 300 DTUs verbraucht, können Sie mit der Umstellung auf V-Kerne unter Umständen Ihre Kosten senken. Sie können für die Umstellung die API Ihrer Wahl oder das Azure-Portal verwenden, ohne dass es zu Ausfallzeit kommt. Die Umstellung ist aber nicht zwingend erforderlich. Wenn das DTU-Kaufmodell Ihre Leistungs- und Geschäftsanforderungen erfüllt, sollten Sie es weiter nutzen. Falls Sie sich für die Umstellung vom DTU-Modell auf das Modell mit virtuellen Kernen entscheiden, sollten Sie zum Auswählen der Computegröße die folgende Faustregel anwenden: Für 100 DTUs im Standard-Tarif ist jeweils mindestens ein virtueller Kern im Tarif „Universell“ erforderlich, und für 125 DTUs im Premium-Tarif ist jeweils mindestens ein virtueller Kern im Tarif „Unternehmenskritisch“ erforderlich.

## <a name="dtu-based-purchasing-model"></a>DTU-basiertes Kaufmodell

Mit einer Datenbanktransaktionseinheit (Database Transaction Unit, DTU) wird eine Mischung aus den Werten von CPU, Arbeitsspeicher, Lesevorgängen und Schreibvorgängen dargestellt. Das DTU-basierte Kaufmodell verfügt über einen Satz mit vorkonfigurierten Paketen mit Computeressourcen und integriertem Speicher, um die Anwendungsleistung auf verschiedenen Ebenen zu erhöhen. Wenn Kunden die Einfachheit eines vorkonfigurierten Pakets und festgelegter Monatszahlungen bevorzugen, ist das DTU-basierte Modell unter Umständen besser für ihre Anforderungen geeignet. Beim DTU-basierten Kaufmodell können Kunden zwischen den Diensttarifen **Basic**, **Standard** und **Premium** für [Einzeldatenbanken](sql-database-single-database-scale.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) wählen. Dieses Kaufmodell ist in [verwalteten Instanzen](sql-database-managed-instance.md) nicht verfügbar.

### <a name="database-transaction-units-dtus"></a>Datenbanktransaktionseinheiten (DTUs)

Für eine einzelne Azure SQL-Datenbank in einer bestimmten Computegröße innerhalb eines [Diensttarifs](sql-database-single-database-scale.md) garantiert Microsoft einen bestimmten Ressourcenumfang für diese Datenbank (unabhängig von jeder anderen Datenbank in der Azure-Cloud), wobei eine vorhersagbare Leistungsebene geboten wird. Der Ressourcenumfang wird als Anzahl von Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) berechnet. Es handelt sich dabei um ein Paket aus Compute-, Speicher- und E/A-Ressourcen. Das Verhältnis zwischen diesen Ressourcen wurde ursprünglich anhand einer [OLTP-Benchmark-Workload](sql-database-benchmark-overview.md) ermittelt, die für realistische OLTP-Workloads typisch ist. Wenn Ihre Workload den Umfang einer dieser Ressourcen überschreitet, wird der Durchsatz gedrosselt, wodurch eine niedrigere Leistung und Timeouts verursacht werden. Die Ressourcen, die von Ihrer Workload verwendet werden, wirken sich nicht auf die Ressourcen aus, die für andere SQL-Datenbanken in der Azure-Cloud verfügbar sind, und die von anderen Workloads verwendeten Ressourcen wirken sich nicht auf die Ressourcen aus, die für Ihre SQL-Datenbank verfügbar sind.

![Begrenzungsrahmen](./media/sql-database-what-is-a-dtu/bounding-box.png)

DTUs sind besonders nützlich, um den relativen Umfang von Ressourcen zu verstehen, der zwischen Azure SQL-Datenbank-Instanzen in unterschiedlichen Computegrößen und Diensttarifen vorliegt. Beispielsweise entspricht eine Verdoppelung der DTUs durch Erhöhen der Computegröße einer Datenbank der Verdoppelung der Ressourcen, die dieser Datenbank zur Verfügung stehen. Beispiel: Eine Premium P11-Datenbank mit 1.750 DTUs bietet eine 350 Mal höhere DTU-Computeleistung als eine Basic-Datenbank mit fünf DTUs.  

Um einen tieferen Einblick in den Ressourcenverbrauch (DTU) Ihrer Workload zu erhalten, verwenden Sie [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md) für Folgendes:

- Ermitteln der häufigsten Abfragen nach den Werten von CPU-Nutzung/Dauer/Ausführungshäufigkeit, bei denen unter Umständen eine Leistungssteigerung erzielt werden kann. Beispielsweise könnte eine E/A-intensive Abfrage davon profitieren, dass [In-Memory-Optimierungstechniken](sql-database-in-memory.md) verwendet werden, um den verfügbaren Arbeitsspeicher in einem bestimmten Diensttarif und einer bestimmten Computegröße besser zu nutzen.
- Analysieren der Details einer Abfrage sowie Anzeigen von deren Text und deren Verlauf der Ressourcenverwendung.
- Zugreifen auf Empfehlungen zur Leistungsoptimierung, die Aktionen erläutern, die vom [SQL Database Advisor](sql-database-advisor.md) ausgeführt werden.

### <a name="elastic-database-transaction-units-edtus"></a>Transaktionseinheiten in elastischer Datenbank (eDTUs)

Statt einen dedizierten Satz von Ressourcen (DTUs) bereitzustellen, die möglicherweise nicht immer für eine immer verfügbare SQL-Datenbank benötigt werden, können Sie Datenbanken in einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md) auf einem SQL-Datenbankserver platzieren, der für diese Datenbanken einen gemeinsam Pool von Ressourcen freigibt. Die freigegebenen Ressourcen in einem Pool für elastische Datenbanken werden als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) gemessen. Pools für elastische Datenbanken stellen eine einfache und kostengünstige Lösung zum Verwalten der Leistungsziele für mehrere Datenbanken mit unterschiedlichsten und unvorhersehbaren Nutzungsmustern dar. Durch einen Pool für elastische Datenbanken wird sichergestellt, dass Ressourcen nicht von einer einzigen Datenbank im Pool genutzt werden können, wobei jeder Datenbank im Pool immer eine Mindestmenge an erforderlichen Ressourcen zur Verfügung steht.

Ein Pool erhält eine festgelegte Anzahl von eDTUs zu einem festen Preis. Im elastischen Pool können einzelne Datenbanken die automatische Skalierung innerhalb der konfigurierten Grenzen flexibel ausführen. Eine Datenbank mit hoher Auslastung nutzt mehr eDTUs, um die Anforderungen zu erfüllen. Datenbanken mit geringerer Auslastung verwenden weniger eDTUs. Datenbanken ohne Auslastung verwenden keine eDTUs. Durch die Bereitstellung von Ressourcen für den gesamten Pool statt pro Datenbank werden Verwaltungsaufgaben vereinfacht, und Sie verfügen über ein vorhersagbares Budget für den Pool.

Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen und Auswirkungen auf die Datenbanken im Pool kommt. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden. Sie können Datenbanken zum Pool hinzufügen oder daraus entfernen oder die Anzahl von eDTUs beschränken, die eine Datenbank mit hoher Auslastung verwenden kann, um eDTUs für andere Datenbanken zu reservieren. Wenn eine Datenbank die Ressourcen voraussichtlich nicht hinreichend nutzt, können Sie sie aus dem Pool entfernen und als einzelne Datenbank mit der voraussichtlichen Menge an erforderlichen Ressourcen konfigurieren.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Ermitteln der für eine Workload erforderlichen Anzahl von DTUs

Wenn Sie die vorhandene Workload eines lokalen oder virtuellen SQL Server-Computers zu Azure SQL-Datenbank migrieren möchten, können Sie den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner) nutzen, um die ungefähre Anzahl von benötigten DTUs zu berechnen. Für eine vorhandene Azure SQL-Datenbank-Workload können Sie [Query Performance Insight für SQL-Datenbank](sql-database-query-performance.md) verwenden, um den Ressourcenverbrauch (DTUs) der Datenbank zu verstehen und einen tieferen Einblick zur Optimierung Ihrer Workload zu gewinnen. Sie können auch die DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) verwenden, um den Ressourcenverbrauch der letzten Stunde anzuzeigen. Alternativ dazu wird in der Katalogansicht [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) der Ressourcenverbrauch der letzten 14 Tage angezeigt. Da hierfür Durchschnittswerte für jeweils fünf Minuten genutzt werden, ist jedoch die Genauigkeit geringer.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Workloads, die von einem elastischen Ressourcenpool profitieren

Pools eignen sich gut für eine große Anzahl von Datenbanken mit spezifischen Nutzungsmustern. Im Hinblick auf eine bestimmte Datenbank ist dieses Muster durch eine geringe durchschnittliche Auslastung mit relativ wenigen Nutzungslastspitzen gekennzeichnet. SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbank-Server und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Weitere Informationen finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum V-Kern-basierten Kaufmodell finden Sie unter [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md).
- Das auf DTUs basierende Kaufmodell finden Sie unter [Auf DTUs basiertes Kaufmodell](sql-database-service-tiers-dtu.md).

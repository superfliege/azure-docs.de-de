---
title: Kaufmodelle für Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erhalten Sie Informationen zu den Kaufmodellen, die für Datenbanken im Azure SQL-Datenbank-Dienst verfügbar sind.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: c0e7f941f9845ed7531f3adf03fbca9fbeb2787d
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56456689"
---
# <a name="azure-sql-database-purchasing-models"></a>Kaufmodelle für Azure SQL-Datenbank

Mit Azure SQL-Datenbank können Sie problemlos eine vollständig verwaltete PaaS-Datenbank-Engine erwerben, die an Ihre Anforderungen an Leistung und Kosten angepasst ist. Je nach Bereitstellungsmodell von Azure SQL-Datenbank können Sie das Kaufmodell auswählen, das Ihren Anforderungen entspricht:

- Das [V-Kern-basierte Kaufmodell](sql-database-service-tiers-vcore.md) (empfohlen), mit dem Sie die genaue Menge an Speicherkapazität und Computeressourcen auswählen können, die Sie für Ihre Workload benötigen.
- Das [DTU-basierte Kaufmodell](sql-database-service-tiers-dtu.md), mit dem Sie Compute- und Speicherressourcen in Paketen auswählen können, die für gängige Workloads zusammengestellt wurden.

In Azure SQL-Datenbank-Bereitstellungsmodellen sind verschiedene Kaufmodelle verfügbar:

- Die Bereitstellungsoptionen [Einzeldatenbank](sql-database-single-databases-manage.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) in [Azure SQL-Datenbank](sql-database-technical-overview.md) bieten sowohl das [DTU-basierte Kaufmodell](sql-database-service-tiers-dtu.md) als auch das [V-Kern-basierte Kaufmodell](sql-database-service-tiers-vcore.md) an.
- Die Bereitstellungsoption [Verwaltete Instanz](sql-database-managed-instance.md) in Azure SQL-Datenbank bietet nur das auf [virtuellen Kernen basierende Kaufmodell](sql-database-service-tiers-vcore.md).

> [!IMPORTANT]
> Der [Diensttarif „Hyperscale“ (Vorschau)](sql-database-service-tier-hyperscale.md) befindet sich in der öffentlichen Vorschau und gilt nur für Einzeldatenbanken, die das V-Kern-Kaufmodell verwenden.

In der folgenden Tabelle und im Diagramm werden diese Kaufmodelle verglichen und gegenübergestellt.

|**Kaufmodell**|**Beschreibung**|**Am besten geeignet für**|
|---|---|---|
|DTU-basiertes Modell|Dieses Modell basiert auf einem Paket mit Compute-, Speicher- und E/A-Ressourcen. Computegrößen werden für Einzeldatenbanken als Datenbanktransaktionseinheiten (DTUs) und für Pools für elastische Datenbanken als elastische Datenbanktransaktionseinheiten (eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-purchase-models.md#dtu-based-purchasing-model).|Eignet sich am besten für Kunden, die einfache, vorkonfigurierte Ressourcenoptionen benötigen.|
|V-Kern-basiertes Modell|Mit diesem Modell können Sie Compute- und Speicherressourcen einzeln auswählen. Mit dem V-Kern-basierten Kaufmodell können Sie auch den [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) nutzen, um Kosten einzusparen.|Eignet sich am besten für Kunden, für die Flexibilität, Kontrolle und Transparenz im Vordergrund stehen.|
||||  

![Preismodell](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>Computekosten

Die Computekosten spiegeln die gesamte Computekapazität wider, die für die Anwendung bereitgestellt wird. Für den Diensttarif „Unternehmenskritisch“ werden mindestens drei Replikate automatisch zugeordnet. Um diese zusätzliche Zuordnung von Computeressourcen widerzuspiegeln, ist der Preis im V-Kern-basierten Kaufmodell für den Diensttarif „Unternehmenskritisch“ ungefähr 2,7x höher als für den Diensttarif „Universell“. Aus demselben Grund spiegelt der höhere Speicherpreis pro GB für den Diensttarif „Unternehmenskritisch“ den höheren E/A-Wert und die geringere Wartezeit des SSD-Speichers wider. Bei den Kosten für den Sicherungsspeicher gibt es keinen Unterschied zwischen diesen beiden Diensttarifen, weil wir in beiden Fällen eine Klasse mit Standardspeicher verwenden.

## <a name="storage-costs"></a>Speicherkosten

Unterschiedliche Arten von Speicher werden auch unterschiedlich berechnet. Für Datenspeicher wird Ihnen der bereitgestellte Speicher basierend auf der von Ihnen gewählten maximalen Datenbank- oder Poolgröße berechnet. Die Kosten ändern sich nicht, sofern Sie dieses Maximum nicht verringern oder erhöhen. Der Sicherungsspeicher ist mit den automatischen Sicherungen Ihrer Instanz verbunden und wird dynamisch zugeordnet. Durch eine Verlängerung der Beibehaltungsdauer Ihrer Sicherungen erhöht sich auch der von Ihrer Instanz verbrauchte Sicherungsspeicher. 

7 Tage von automatischen Sicherungen Ihrer Datenbanken werden standardmäßig in den RA-GRS Standard-Blobspeicher kopiert. Der Speicher wird für wöchentliche vollständige Sicherungen, tägliche differenzielle Sicherungen und im 5-Minuten-Takt kopierte Sicherungen von Transaktionsprotokollen verwendet. Die Größe des Transaktionsprotokolls hängt von der Änderungsrate der Datenbank ab. Eine Mindestspeichermenge, die der Gesamtgröße der Datenbank entspricht, wird kostenlos zur Verfügung gestellt. Zusätzlich verbrauchter Sicherungsspeicher wird in GB/Monat abgerechnet.

Weitere Informationen zu den Preisen für Storage finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/sql-database/single/). 

## <a name="vcore-based-purchasing-model"></a>Auf virtuellen Kernen basierendes Erwerbsmodell

Ein virtueller Kern repräsentiert die logische CPU. Virtuelle Kerne werden für verschiedene Hardwaregenerationen und physische Hardwaremerkmale (z.B. Anzahl der Kerne, Arbeitsspeicher, Speichergröße) angeboten. Beim auf dem virtuellen Kern basierenden Kaufmodell erhalten Sie Flexibilität, Kontrolle und Transparenz in Bezug auf den individuellen Ressourcenverbrauch. Außerdem können Sie die lokalen Workloadanforderungen leicht auf die Cloud übertragen. Mit diesem Modell können Sie Computeressourcen, Arbeitsspeicher und Speicher entsprechend den jeweiligen Workloadanforderungen auswählen. Beim V-Kern-basierten Kaufmodell können Sie für [Einzeldatenbanken](sql-database-single-database-scale.md), [Pools für elastische Datenbanken](sql-database-elastic-pool.md) und [verwaltete Instanzen](sql-database-managed-instance.md) zwischen den Diensttarifen [Universell](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) und [Unternehmenskritisch](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) wählen. Für Einzeldatenbanken können Sie auch den [Diensttarif Hyperscale (Vorschau)](sql-database-service-tier-hyperscale.md) auswählen.

Mit dem V-Kern-basierten Kaufmodell können Sie Compute- und Speicherressourcen einzeln auswählen, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis optimieren. Beim V-Kern-basierten Kaufmodell zahlen Kunden für Folgendes:

- Compute (Diensttarif + Anzahl von V-Kernen und Arbeitsspeichermenge + Hardwaregeneration)
- Typ und Menge von Daten und Protokollspeicher
- Sicherungsspeicher (RA-GRS)

> [!IMPORTANT]
> Compute, E/A-Vorgänge, Daten und Protokollspeicher werden pro Datenbank oder Pool für elastische Datenbanken berechnet. Sicherungsspeicher wird pro Datenbank berechnet. Weitere Informationen zu den Kosten für verwaltete Instanzen finden Sie unter [Verwaltete Instanzen](sql-database-managed-instance.md).
> **Regionseinschränkungen:** Die aktuelle Liste der unterstützten Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Wenn Sie eine verwaltete Instanz in einer Region erstellen möchten, die derzeit nicht unterstützt wird, können Sie eine [Supportanfrage über das Azure-Portal senden](sql-database-managed-instance-resource-limits.md#obtaining-a-larger-quota-for-sql-managed-instance).
.

Wenn Ihre Einzeldatenbank oder Ihr Pool für elastische Datenbanken mehr als 300 DTUs verbraucht, können Sie Ihre Kosten durch eine Umstellung auf das V-Kern-basierte Kaufmodell möglicherweise senken. Wenn Sie sich für eine Umstellung entscheiden, können Sie dazu die API Ihrer Wahl oder das Azure-Portal verwenden, ohne dass es zu Ausfallzeiten kommt. Allerdings ist eine Umstellung nicht erforderlich und erfolgt nicht automatisch. Wenn das DTU-basierte Kaufmodell Ihre Leistungs- und Geschäftsanforderungen erfüllt, sollten Sie es weiter nutzen. Wenn Sie sich für eine Umstellung vom DTU-basierten Kaufmodell auf das V-Kern-basierte Kaufmodell entscheiden, wählen Sie die Computegröße mithilfe der folgenden Faustregeln aus: 

- Für jede 100 DTU im Tarif „Standard“ ist mindestens 1 V-Kern im Tarif „Universell“ erforderlich
- Für jede 125 DTU im Tarif „Premium“ ist mindestens 1 V-Kern im Tarif „Unternehmenskritisch“ erforderlich

## <a name="dtu-based-purchasing-model"></a>DTU-basiertes Kaufmodell

Mit einer Datenbanktransaktionseinheit (Database Transaction Unit, DTU) wird eine Mischung aus den Werten von CPU, Arbeitsspeicher, Lesevorgängen und Schreibvorgängen dargestellt. Das DTU-basierte Kaufmodell verfügt über einen Satz mit vorkonfigurierten Paketen mit Computeressourcen und integriertem Speicher, um die Anwendungsleistung auf verschiedenen Ebenen zu erhöhen. Wenn Kunden die Einfachheit eines vorkonfigurierten Pakets und festgelegter Monatszahlungen bevorzugen, ist das DTU-basierte Modell unter Umständen besser für ihre Anforderungen geeignet. Beim DTU-basierten Kaufmodell können Kunden zwischen den Diensttarifen **Basic**, **Standard** und **Premium** für [Einzeldatenbanken](sql-database-single-database-scale.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) wählen. Dieses Kaufmodell ist in [verwalteten Instanzen](sql-database-managed-instance.md) nicht verfügbar.

### <a name="database-transaction-units-dtus"></a>Datenbanktransaktionseinheiten (DTUs)

Für eine einzelne Datenbank in einer bestimmten Computegröße innerhalb eines [Diensttarifs](sql-database-single-database-scale.md) garantiert Microsoft einen bestimmten Ressourcenumfang für diese Datenbank (unabhängig von jeder anderen Datenbank in der Azure-Cloud), wobei eine vorhersagbare Leistungsebene geboten wird. Der Ressourcenumfang wird als Anzahl von Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) berechnet. Es handelt sich dabei um ein Paket aus Compute-, Speicher- und E/A-Ressourcen. Das Verhältnis zwischen diesen Ressourcen wurde ursprünglich anhand einer [OLTP-Benchmark-Workload](sql-database-benchmark-overview.md) ermittelt, die für realistische OLTP-Workloads typisch ist. Wenn Ihre Workload den Umfang einer dieser Ressourcen überschreitet, wird der Durchsatz gedrosselt, wodurch eine niedrigere Leistung und Timeouts verursacht werden. Die Ressourcen, die von Ihrer Workload verwendet werden, wirken sich nicht auf die Ressourcen aus, die für andere SQL-Datenbanken in der Azure-Cloud verfügbar sind, und die von anderen Workloads verwendeten Ressourcen wirken sich nicht auf die Ressourcen aus, die für Ihre SQL-Datenbank verfügbar sind.

![Begrenzungsrahmen](./media/sql-database-what-is-a-dtu/bounding-box.png)

DTUs sind besonders nützlich, um den relativen Umfang von Ressourcen zu verstehen, der zwischen Azure SQL-Datenbank-Instanzen in unterschiedlichen Computegrößen und Diensttarifen vorliegt. Beispielsweise entspricht eine Verdoppelung der DTUs durch Erhöhen der Computegröße einer Datenbank der Verdoppelung der Ressourcen, die dieser Datenbank zur Verfügung stehen. Beispiel: Eine Premium P11-Datenbank mit 1.750 DTUs bietet eine 350 Mal höhere DTU-Computeleistung als eine Basic-Datenbank mit fünf DTUs.  

Um einen tieferen Einblick in den Ressourcenverbrauch (DTU) Ihrer Workload zu erhalten, verwenden Sie die [Statistik zur Abfrageleistung](sql-database-query-performance.md) für die folgenden Aufgaben:

- Ermitteln der häufigsten Abfragen nach den Werten von CPU-Nutzung/Dauer/Ausführungshäufigkeit, bei denen unter Umständen eine Leistungssteigerung erzielt werden kann. Beispielsweise könnte eine E/A-intensive Abfrage davon profitieren, dass [In-Memory-Optimierungstechniken](sql-database-in-memory.md) verwendet werden, um den verfügbaren Arbeitsspeicher in einem bestimmten Diensttarif und einer bestimmten Computegröße besser zu nutzen.
- Analysieren der Details einer Abfrage sowie Anzeigen von deren Text und deren Verlauf der Ressourcenverwendung.
- Zugreifen auf Empfehlungen zur Leistungsoptimierung, die Aktionen erläutern, die vom [SQL Database Advisor](sql-database-advisor.md) ausgeführt werden.

### <a name="elastic-database-transaction-units-edtus"></a>Transaktionseinheiten in elastischer Datenbank (eDTUs)

Statt einen dedizierten Satz von Ressourcen (DTUs) bereitzustellen, die möglicherweise nicht immer für eine immer verfügbare SQL-Datenbank benötigt werden, können Sie Datenbanken in einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md) auf einem SQL-Datenbankserver platzieren, der für diese Datenbanken einen gemeinsam Pool von Ressourcen freigibt. Die freigegebenen Ressourcen in einem Pool für elastische Datenbanken werden als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) gemessen. Pools für elastische Datenbanken stellen eine einfache und kostengünstige Lösung zum Verwalten der Leistungsziele für mehrere Datenbanken mit unterschiedlichsten und unvorhersehbaren Nutzungsmustern dar. Durch einen Pool für elastische Datenbanken wird sichergestellt, dass Ressourcen nicht von einer einzigen Datenbank im Pool genutzt werden können, wobei jeder Datenbank im Pool immer eine Mindestmenge an erforderlichen Ressourcen zur Verfügung steht.

Ein Pool erhält eine festgelegte Anzahl von eDTUs zu einem festen Preis. Im elastischen Pool können einzelne Datenbanken die automatische Skalierung innerhalb der konfigurierten Grenzen flexibel ausführen. Eine Datenbank mit hoher Auslastung nutzt mehr eDTUs, um die Anforderungen zu erfüllen. Datenbanken mit geringerer Auslastung verwenden weniger eDTUs. Datenbanken ohne Auslastung verwenden keine eDTUs. Durch die Bereitstellung von Ressourcen für den gesamten Pool statt pro Datenbank werden Verwaltungsaufgaben vereinfacht, und Sie verfügen über ein vorhersagbares Budget für den Pool.

Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen und Auswirkungen auf die Datenbanken im Pool kommt. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden. Sie können Datenbanken zum Pool hinzufügen oder daraus entfernen oder die Anzahl von eDTUs beschränken, die eine Datenbank mit hoher Auslastung verwenden kann, um eDTUs für andere Datenbanken zu reservieren. Wenn eine Datenbank die Ressourcen voraussichtlich nicht hinreichend nutzt, können Sie sie aus dem Pool entfernen und als einzelne Datenbank mit der voraussichtlichen Menge an erforderlichen Ressourcen konfigurieren.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Ermitteln der für eine Workload erforderlichen Anzahl von DTUs

Wenn Sie die vorhandene Workload eines lokalen oder virtuellen SQL Server-Computers zu Azure SQL-Datenbank migrieren möchten, können Sie den [DTU-Rechner](http://dtucalculator.azurewebsites.net/) nutzen, um die ungefähre Anzahl von benötigten DTUs zu berechnen. Für eine vorhandene Azure SQL-Datenbank-Workload können Sie die [Statistik zur Abfrageleistung](sql-database-query-performance.md) verwenden, um den Ressourcenverbrauch (DTUs) der Datenbank zu verstehen und einen tieferen Einblick zur Optimierung Ihrer Workload zu gewinnen. Sie können auch die DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) verwenden, um den Ressourcenverbrauch der letzten Stunde anzuzeigen. Alternativ dazu wird in der Katalogansicht [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) der Ressourcenverbrauch der letzten 14 Tage angezeigt. Da hierfür Durchschnittswerte für jeweils fünf Minuten genutzt werden, ist jedoch die Genauigkeit geringer.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Workloads, die von einem elastischen Ressourcenpool profitieren

Pools eignen sich gut für eine große Anzahl von Datenbanken mit spezifischen Nutzungsmustern. Im Hinblick auf eine bestimmte Datenbank ist dieses Muster durch eine geringe durchschnittliche Auslastung mit relativ wenigen Nutzungslastspitzen gekennzeichnet. SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbank-Server und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Weitere Informationen finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool.md).

## <a name="purchase-model-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zum Kaufmodell

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>Muss ich meine Anwendung offline schalten, um von einer auf DTUs basierenden Datenbank auf einen V-Kern-basierten Diensttarif umzustellen?

Die neuen Diensttarife bieten eine einfache Onlinekonvertierungsmethode, die dem bestehenden Prozess zur Durchführung eines Upgrades für Datenbanken vom Diensttarif „Standard“ auf „Premium“ und umgekehrt ähnelt. Diese Konvertierung kann mit dem Azure-Portal, mit PowerShell, der Azure CLI, T-SQL oder der REST-API initiiert werden. Weitere Informationen finden Sie unter [Verwalten von Ressourcen für eine einzelne Datenbank in Azure SQL-Datenbank](sql-database-single-database-scale.md) und [Pools für elastische Datenbanken als Hilfe beim Verwalten und Skalieren mehrerer Azure SQL-Datenbank-Instanzen](sql-database-elastic-pool.md).

### <a name="can-i-convert-a-database-from-a-service-tier-using-the-vcore-based-purchase-to-a-service-tier-using-the-dtu-based-purchasing-model"></a>Kann ich eine Datenbank von einem Diensttarif, der das V-Kern-basierte Kaufmodell verwendet, auf einen Diensttarif umstellen, der das DTU-basierte Kaufmodell verwendet?

Ja. Sie können Ihre Datenbank leicht auf ein beliebiges unterstütztes Leistungsziel umstellen, indem Sie das Azure-Portal, PowerShell, die Azure CLI, T-SQL oder die REST-API verwenden. Weitere Informationen finden Sie unter [Verwalten einzelner Datenbanken](sql-database-single-database-scale.md) und [Verwalten von Pools für elastische Datenbanken](sql-database-elastic-pool.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum V-Kern-basierten Kaufmodell finden Sie unter [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md).
- Das auf DTUs basierende Kaufmodell finden Sie unter [Auf DTUs basiertes Kaufmodell](sql-database-service-tiers-dtu.md).

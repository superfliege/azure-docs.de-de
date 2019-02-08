---
title: Häufig gestellte Fragen zu Azure SQL-Datenbank | Microsoft-Dokumentation
description: Antworten auf häufig gestellte Fragen von Kunden zu Cloud-Datenbanken, zur Azure SQL-Datenbank, das Microsoft Managementsystem für relationale Datenbanken (RDBMS) und Database as a Service in der Cloud.
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
ms.date: 01/25/2019
ms.openlocfilehash: 3ede7bdf0837c88340f13a0f921ca1752a3d3c6b
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55511648"
---
# <a name="sql-database-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zu SQL-Datenbank

## <a name="what-is-the-current-version-of-sql-database"></a>Was ist die aktuelle Version von SQL-Datenbank?

Die aktuelle Version von SQL-Datenbank ist V12. Version V11 wurde eingestellt.

## <a name="what-is-the-sla-for-sql-database"></a>Welche Vereinbarung zum Servicelevel (SLA) gilt für SQL-Datenbank?

Wir garantieren für die Konnektivität zwischen Ihrer Microsoft Azure SQL-Datenbank und unserem Internetgateway unabhängig von Ihrem Diensttarif eine Betriebszeit von mindestens 99,99%. 0,01 % ist für Patches, Upgrades und Failover reserviert. Weitere Informationen finden Sie unter [SLA](https://azure.microsoft.com/support/legal/sla/). Informationen zur Verfügbarkeitsarchitektur von Azure SQL-Datenbank finden Sie unter [Hochverfügbarkeit und Azure SQL-Datenbank](sql-database-high-availability.md).

## <a name="can-i-control-when-patching-downtime-occurs"></a>Kann ich den Zeitpunkt der Ausfallzeiten beim Patchen steuern?

 Nein. Die Auswirkungen vom Patchen sind im Allgemeinen nicht merklich, wenn Sie in Ihrer App [Wiederholungslogik einsetzen](sql-database-develop-overview.md#resiliency).

## <a name="what-is-the-new-vcore-based-purchasing-model-for-azure-sql-database"></a>Was ist das neue V-Kern-basierte Kaufmodell für Azure SQL-Datenbank?

Das neue Kaufmodell gilt zusätzlich zum vorhandenen DTU-basierten Modell. Das V-Kern-basierte Modell soll Kunden Flexibilität, Kontrolle, Transparenz und eine einfache Möglichkeit zum Übertragen der lokalen Workloadanforderungen in die Cloud bieten. Außerdem können Kunden ihre Compute- und Speicherressourcen basierend auf ihren jeweiligen Workloadanforderungen skalieren. Bei den Optionen des V-Kern-Modells für Einzeldatenbanken und Pools für elastische Datenbanken können mit dem [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/#services) Einsparungen von bis zu 30 Prozent erzielt werden. Weitere Informationen finden Sie unter [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) und [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md).

## <a name="what-is-a-vcore"></a>Was ist ein V-Kern (virtueller Kern)?

Ein virtueller Kern repräsentiert die logische CPU. Virtuelle Kerne werden für verschiedene Hardwaregenerationen angeboten. Logische CPUs der 4. Generation basieren auf Intel E5-2673 v3-Prozessoren (Haswell) mit 2,4 GHz, und logische CPUs der 5. Generation basieren auf Intel E5-2673 v4-Prozessoren (Broadwell) mit 2,3 GHz.

## <a name="is-moving-to-the-vcore-based-model-required"></a>Ist eine Migration zum V-Kern-basierten Modell erforderlich?

Nein. Mit der Einführung des V-Kern-basierten Modells bei den Bereitstellungsoptionen von Pools für elastische Datenbanken und Singletons möchten wir unseren Kunden eine größere Auswahl und Flexibilität bieten. Wenn Kunden weiterhin das DTU-basierte Modell nutzen möchten, müssen sie aufgrund dieser Ankündigung keine Schritte ausführen, und die Umgebung und Abrechnung ändert sich nicht.

In vielen Fällen können Anwendungen von der Einfachheit eines vorkonfigurierten Ressourcenpakets profitieren. Aus diesem Grund bieten wir diese DTU-basierten Optionen weiterhin für unsere Kunden an. Falls Sie diese Optionen nutzen und Ihre Geschäftsanforderungen damit erfüllt werden, sollten Sie sie weiter verwenden.

Das DTU-basierte Modell und das V-Kern-basierte Modell werden weiterhin parallel angeboten. Wir führen das V-Kern-basierte Modell als Reaktion auf Kundenanfragen nach mehr Transparenz in Bezug auf die Datenbankressourcen und die Möglichkeit zur separaten Skalierung ihrer Compute- und Speicherressourcen ein. Das V-Kern-basierte Modell ermöglicht dank des Azure-Hybridvorteils für SQL Server darüber hinaus zusätzliche Einsparungen für Kunden mit aktiver Software Assurance.

## <a name="how-should-i-choose-between-the-dtu-based-purchasing-model-vs-the-vcore-based-purchasing-model"></a>Was sollte ich bei der Entscheidung zwischen dem DTU-basierten Kaufmodell und dem V-Kern-basierten Kaufmodell beachten?

Die DTU (Database Transaction Unit) basiert auf einer Mischung aus den Werten für CPU, Speicher, Lesevorgänge und Schreibvorgänge. Die auf DTUs basierenden Computegrößen stellen vorkonfigurierte Ressourcenpakete gemäß den unterschiedlichen Stufen der Anwendungsleistung dar. Wenn Kunden die zugrunde liegenden Ressourcen außen vor lassen möchten und den Komfort eines vorkonfigurierten Pakets mit der Möglichkeit zur Zahlung eines monatlichen Festbetrags bevorzugen, ist das DTU-basierte Modell für ihre Anforderungen eventuell besser geeignet. Falls Kunden aber einen umfassenderen Einblick in die zugrunde liegenden Ressourcen benötigen oder diese aus Gründen der Leistungsoptimierung unabhängig voneinander skalieren möchten, ist das V-Kern-basierte Modell die ideale Wahl.  Wenn ein Kunde darüber hinaus über eine aktive Software Assurance (SA) für SQL Server verfügt, kann er seine getätigten Investitionen nutzen und mit dem [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) bis zu 30 Prozent sparen.  Die Optionen beider Kaufmodelle bieten die Vorteile eines vollständig verwalteten Diensts, z.B. automatisierte Sicherungen, Softwareupdates und Patches.

## <a name="what-is-the-azure-hybrid-benefit-for-sql-server"></a>Was ist der Azure-Hybridvorteil für SQL Server?

Der [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) ermöglicht eine maximale Ausnutzung Ihrer aktuellen Lizenzinvestitionen sowie eine schnellere Migration in die Cloud. Mit dem Azure-Hybridvorteil für SQL Server können Sie Ihre SQL Server-Lizenzen mit Software Assurance verwenden und zahlen so einen geringeren Preis („Basistarif“) für SQL-Datenbank. Der Azure-Hybridvorteil für SQL Server ist als öffentliche Vorschauversion des V-Kern-basierten Kaufmodells für Einzeldatenbanken und Pools für elastische Datenbanken von SQL-Datenbank verfügbar. Sie können diesen Vorteil auch bei aktiver SKU in Anspruch nehmen. Beachten Sie aber, dass der Basistarif in Kraft tritt, sobald Sie den Vorteil im Azure-Portal auswählen. Guthaben werden nicht rückwirkend gewährt.

## <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>Gibt es für den Azure-Hybridvorteil für SQL Server Dualnutzungsrechte?

Sie verfügen für die Lizenz 180 Tage lang über Dualnutzungsrechte, um sicherzustellen, dass die Migrationen reibungslos durchgeführt werden können. Nach Ablauf der 180 Tage kann die SQL Server-Lizenz nur in der Cloud in SQL-Datenbank verwendet werden, und lokal und in der Cloud gelten keine Dualnutzungsrechte mehr.

## <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>Wie unterscheidet sich der Azure-Hybridvorteil für SQL Server von License Mobility?

Derzeit bieten wir License Mobility-Vorteile für SQL Server-Kunden mit Software Assurance an, in deren Rahmen die Neuzuweisung von Lizenzen zu freigegebenen Drittanbieterservern möglich ist. Dieser Vorteil kann für Azure IaaS und AWS EC2 verwendet werden.
Der Azure-Hybridvorteil für SQL Server unterscheidet sich in zwei wichtigen Bereichen von License Mobility:

- Er ist mit wirtschaftlichen Vorteilen in Bezug auf das Verlagern von hochgradig virtualisierten Workloads nach Azure verbunden. SQL EE-Kunden können in Azure für die SKU „Universell“ für jeden Kern, der lokal vorhanden ist, vier Kerne für hochgradig virtualisierte Anwendungen erhalten. Bei License Mobility ergeben sich bei der Verlagerung von virtualisierten Workloads in die Cloud keine besonderen Kostenvorteile.
- Hierbei ist ein PaaS-Ziel in Azure (eine verwaltete SQL-Datenbank-Instanz) vorhanden, das mit der lokalen SQL Server-Instanz hochgradig kompatibel ist.

## <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>Welche spezifischen Rechte gelten für den Azure-Hybridvorteil für SQL Server?

SQL-Datenbank-Kunden sind beim Azure-Hybridvorteil für SQL Server die folgenden Rechte zugeordnet:

|Lizenzspeicherbedarf|Was erhalten Sie beim Azure-Hybridvorteil für SQL Server?|
|---|---|
|SQL Server Enterprise Edition – Hauptkunden mit SA|<li>Zahlung des Basistarifs für SKU „Universell“ oder „Unternehmenskritisch“</li><br><li>1 Kern lokal = 4 Kerne in SKU „Universell“</li><br><li>1 Kern lokal = 1 Kern in SKU „Unternehmenskritisch“</li>|
|SQL Server Standard Edition – Hauptkunden mit SA|<li>Zahlung des Basistarifs nur für SKU „Universell“</li><br><li>1 Kern lokal = 1 Kern in SKU „Universell“</li>|
|||

## <a name="is-the-vcore-based-model-available-to-sql-database-managed-instance"></a>Ist das V-Kern-basierte Modell für die verwaltete SQL-Datenbank-Instanz verfügbar?

Die [verwaltete Instanz](sql-database-managed-instance.md) ist nur für das V-Kern-basierte Modell verfügbar. Weitere Informationen finden Sie auch unter [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/managed/).

## <a name="does-the-cost-of-compute-and-storage-depend-on-the-service-tier-that-i-choose"></a>Richten sich die Kosten für Compute und Speicher nach dem von mir gewählten Diensttarif?

Die Computekosten spiegeln die gesamte Computekapazität wider, die für die Anwendung bereitgestellt wird. Für den Diensttarif „Unternehmenskritisch“ werden automatisch mindestens drei Replikate zugeordnet. Um diese zusätzliche Zuordnung von Computeressourcen widerzuspiegeln, ist der V-Kern-Preis für „Unternehmenskritisch“ ca. um das 2,7-fache höher. Aus dem gleichen Grund spiegelt der höhere Speicherpreis pro GB für die Ebene „Unternehmenskritisch“ den höheren E/A-Wert und die geringere Wartezeit des SSD-Speichers wider. Die Kosten für den Sicherungsspeicher unterscheiden sich nicht, da wir in beiden Fällen eine Klasse mit Standardspeicher verwenden.

## <a name="how-am-i-charged-for-storage---based-on-what-i-configure-upfront-or-on-what-the-database-uses"></a>Wie wird der Speicher berechnet: basierend auf meiner vorherigen Konfiguration oder basierend auf der Nutzung durch die Datenbank?

Unterschiedliche Arten von Speicher werden auch unterschiedlich berechnet. Für Datenspeicher wird Ihnen der bereitgestellte Speicher basierend auf der von Ihnen gewählten maximalen Datenbank- oder Poolgröße berechnet. Die Kosten ändern sich nicht, sofern Sie dieses Maximum nicht verringern oder erhöhen. Der Sicherungsspeicher ist mit den automatischen Sicherungen Ihrer Instanz verbunden und wird dynamisch zugeordnet. Durch eine Verlängerung der Beibehaltungsdauer Ihrer Sicherungen erhöht sich auch der von Ihrer Instanz verbrauchte Sicherungsspeicher. Für Ihren gesamten bereitgestellten Serverspeicher fallen keine zusätzlichen Gebühren für den Sicherungsspeicher an. Zusätzlich verbrauchter Sicherungsspeicher wird pro GB und Monat abgerechnet. Beispiel: Wenn Sie über 100 GB Datenbankspeicher verfügen, erhalten Sie ohne Aufpreis einen Sicherungsspeicher von 100 GB. Wenn Sie aber 110 GB sichern, werden Ihnen die zusätzlichen 10 GB berechnet.

Für einen Sicherungsspeicher einer Einzeldatenbank wird Ihnen der Speicher anteilig berechnet, der für die Datenbanksicherungen zugeordnet wurde, abzüglich der Größe der Datenbank. Für den Sicherungsspeicher eines Pools für elastische Datenbanken wird Ihnen der Speicher anteilig berechnet, der für die Datenbanksicherungen aller Datenbanken des Pools zugeordnet wurde, abzüglich der maximalen Datengröße des Pools für elastische Datenbanken. Jede Erhöhung der Datenbankgröße oder des Pools für elastische Datenbanken oder eine Erhöhung der Transaktionsrate führt zu höherem Speicherbedarf und somit auch zu einer höheren Rechnung für Ihren Sicherungsspeicher.  Wenn Sie die maximale Datengröße erhöhen, wird dieser neue Betrag von der berechneten Größe des Sicherungsspeichers abgezogen.

## <a name="how-do-i-select-the-right-sku-when-converting-an-existing-database-to-the-new-service-tiers"></a>Wie wähle ich die richtige SKU aus, wenn ich eine vorhandene Datenbank auf die neuen Diensttarife umstelle?

Für vorhandene SQL-Datenbank-Anwendungen mit dem DTU-basierten Modell ist der Diensttarif „Universell“ mit dem Standard-Tarif vergleichbar. Der Diensttarif „Unternehmenskritisch“ ist mit dem Premium-Tarif vergleichbar. In beiden Fällen sollten Sie mindestens jeweils einen V-Kern für 100 DTUs zuordnen, die von Ihrer Anwendung im DTU-basierten Modell verwendet werden.

## <a name="do-the-new-vcore-based-service-tiers-offer-the-compute-sizes-compatible-with-all-existing-compute-sizes"></a>Sind die Computegrößen, die im Rahmen der neuen Diensttarife auf V-Kern-Basis angeboten werden, mit allen vorhandenen Computegrößen kompatibel?

Die neuen V-Kern-basierten Diensttarife verfügen über vergleichbare Leistungsoptionen für alle Pools für elastische Datenbanken und Datenbanken, für die mindestens 100 DTUs verwendet werden.  Wir werden im Laufe der Zeit weitere Computegrößen hinzufügen, um Workloads mit weniger als 100 DTUs abzudecken.

## <a name="are-there-any-database-feature-differences-between-the-existing-dtu-based-and-new-vcore-based-service-tiers"></a>Bestehen bei den Datenbankfeatures Unterschiede zwischen den vorhandenen DTU-basierten und den neuen V-Kern-basierten Diensttarifen?

Die neuen Diensttarife unterstützen eine Obermenge der Features, die für die aktuellen DTU-basierten Angebote verfügbar sind. Zu den zusätzlichen Features zählen eine Gruppe mit zusätzlichen dynamischen Verwaltungssichten (Dynamic Management Views, DMVs) und zusätzlichen Optionen für die Ressourcenkonfiguration.

## <a name="if-my-database-is-cpu-bound-and-does-not-use-much-storage-can-i-increase-the-compute-without-paying-for-extra-storage"></a>Wenn meine Datenbank CPU-gebunden ist und nicht viel Speicher benötigt, kann ich dann die Computeebene erhöhen, ohne für zusätzlichen Speicher zu zahlen?

Ja. Sie können die Computeebene, die für Ihre Anwendung benötigt wird, separat auswählen und den Speicher unverändert lassen. Der Speicher kann bis auf 32 GB verringert werden.

## <a name="will-the-new-vcore-based-tiers-support-point-in-time-restore-pitr-for-35-days-as-today"></a>Unterstützen die neuen V-Kern-basierten Diensttarife die Point-in-Time-Wiederherstellung (PITR) für einen Zeitraum von 35 Tagen, wie derzeit auch?

Sie können die Sicherungsaufbewahrung für PITR auf einen Wert zwischen 7 und 35 Tagen festlegen. Der Sicherungsspeicher wird basierend auf dem tatsächlichen Speicherverbrauch separat berechnet, wenn eine Speichermenge überschritten wird, die der maximalen Datengröße entspricht. In der Vorschauversion ist die PITR-Aufbewahrungsdauer standardmäßig auf sieben Tage festgelegt. In vielen Fällen reicht die maximale Datengröße zum Speichern von Sicherungen für einen Zeitraum von sieben Tagen aus.

## <a name="why-do-you-allow-selection-of-the-hardware-generation-for-compute"></a>Warum kann die Hardwaregeneration für Compute ausgewählt werden?

Unser Ziel ist maximale Flexibilität, damit Sie eine Leistungskonfiguration wählen können, die möglichst gut zu den Anforderungen der Anwendung passt. Gen4-Hardware verfügt über deutlich mehr Arbeitsspeicher pro V-Kern. Bei Gen5-Hardware können die Computeressourcen aber viel stärker zentral hochskaliert werden. Weitere Informationen finden Sie unter [Auswählen eines V-Kern-Diensttarifs und von Compute-, Arbeitsspeicher-, Speicher- und E/A-Ressourcen](sql-database-service-tiers-vcore.md).

## <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>Muss ich meine Anwendung offline schalten, um von einer auf DTUs basierenden Datenbank auf einen V-Kern-basierten Diensttarif umzustellen?

Die neuen Diensttarife bieten eine einfache Onlinekonvertierungsmethode, die dem bestehenden Prozess zur Durchführung eines Upgrades für Datenbanken vom Diensttarif „Standard“ auf „Premium“ und umgekehrt ähnelt. Diese Konvertierung kann mit dem Azure-Portal, mit PowerShell, der Azure CLI, T-SQL oder der REST-API initiiert werden. Weitere Informationen finden Sie unter [Verwalten von Ressourcen für eine einzelne Datenbank in Azure SQL-Datenbank](sql-database-single-database-scale.md) und [Pools für elastische Datenbanken als Hilfe beim Verwalten und Skalieren mehrerer Azure SQL-Datenbank-Instanzen](sql-database-elastic-pool.md).

## <a name="can-i-convert-a-database-from-a-vcore-based-service-tier-to-a-dtu-based-one"></a>Kann ich eine Datenbank von einem V-Kern-basierten Diensttarif auf einen DTU-basierten Diensttarif umstellen?

Ja. Sie können Ihre Datenbank leicht auf ein beliebiges unterstütztes Leistungsziel umstellen, indem Sie das Azure-Portal, PowerShell, die Azure CLI, T-SQL oder die REST-API verwenden. Weitere Informationen finden Sie unter [Verwalten von Ressourcen für eine einzelne Datenbank in Azure SQL-Datenbank](sql-database-single-database-scale.md) und [Pools für elastische Datenbanken als Hilfe beim Verwalten und Skalieren mehrerer Azure SQL-Datenbank-Instanzen](sql-database-elastic-pool.md).

## <a name="can-i-upgrade-or-downgrade-between-the-general-purpose-and-business-critical-service-tiers"></a>Kann ich zwischen den Diensttarifen „Universell“ und „Unternehmenskritisch“ wechseln?

Ja, mit einigen Einschränkungen. Ihre Ziel-SKU muss die maximale Größe für die Datenbank oder den Pool für elastische Datenbanken einhalten, die Sie für Ihre vorhandene Bereitstellung konfiguriert haben. Bei Verwendung des [Azure-Hybridvorteils für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) ist die SKU „Unternehmenskritisch“ nur für Kunden mit Enterprise Edition-Lizenzen verfügbar. Nur Kunden, die eine Migration von der lokalen Umgebung zu „Universell“ durchgeführt haben, indem sie den Azure-Hybridvorteil für SQL Server mit Enterprise Edition-Lizenzen verwendet haben, können ein Upgrade auf „Unternehmenskritisch“ durchführen. Ausführliche Informationen finden Sie unter [Azure-Hybridvorteil für Windows Server](https://azure.microsoft.com/pricing/hybrid-benefit/).

Diese Umstellung führt nicht zu Ausfallzeit und kann per Azure-Portal, PowerShell, Azure CLI, T-SQL oder REST-API initiiert werden. Weitere Informationen finden Sie unter [Verwalten von Ressourcen für eine einzelne Datenbank in Azure SQL-Datenbank](sql-database-single-database-scale.md) und [Pools für elastische Datenbanken als Hilfe beim Verwalten und Skalieren mehrerer Azure SQL-Datenbank-Instanzen](sql-database-elastic-pool.md).

## <a name="i-am-using-a-premium-rs-database-that-will-not-be-generally-available---can-i-upgrade-it-to-a-new-tier-and-achieve-a-similar-priceperformance-benefit"></a>Ich verwende eine Premium RS-Datenbank, die nicht allgemein verfügbar ist. Kann ich ein Upgrade auf einen neuen Tarif durchführen und einen ähnlichen Vorteil in Bezug auf das Preis-Leistungs-Verhältnis erzielen?

Da beim V-Kern-Modell eine unabhängige Steuerung der bereitgestellten Compute- und Speichermenge möglich ist, können Sie die sich ergebenden Kosten effektiv verwalten. Dies ist somit ein attraktives Ziel für Premium RS-Datenbanken. Zusätzlich ist der [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) mit einem beträchtlichem Rabatt verbunden, wenn das V-Kern-basierte Modell verwendet wird.

## <a name="how-often-can-i-adjust-the-resources-per-pool"></a>Wie oft kann ich die Ressourcen pro Pool anpassen?

Beliebig oft. Weitere Informationen finden Sie unter [Verwalten von Pools für elastische Datenbanken](sql-database-elastic-pool.md).

## <a name="how-long-does-it-take-to-change-the-service-tier-or-compute-size-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Wie lange dauert es, den Diensttarif oder die Computegröße einer Einzeldatenbank zu ändern oder eine Datenbank in einen oder aus einem Pool für elastische Datenbanken zu verschieben?

Um den Diensttarif einer Datenbank zu ändern und ihn in einen Pool und aus einem Pool heraus zu verschieben, muss die Datenbank als Hintergrundvorgang auf die Plattform kopiert werden. Je nach Größe der Datenbanken kann das Ändern des Diensttarifs wenige Minuten oder mehrere Stunden dauern. In beiden Fällen bleiben die Datenbanken während des Verschiebens online und verfügbar. Weitere Informationen zum Ändern von Einzeldatenbanken finden Sie unter [Ändern der Dienstebene für eine Datenbank](sql-database-single-database-scale.md).

## <a name="when-should-i-use-a-single-database-vs-elastic-pools"></a>Wann sollten Singletons Pools für elastische Datenbanken vorgezogen werden?

Pools für elastische Datenbanken sind generell auf herkömmliche [SaaS-Anwendungsmuster (Software-as-a-Service)](sql-database-design-patterns-multi-tenancy-saas-applications.md) ausgelegt, in denen eine Datenbank pro Kunde oder Mandant vorhanden ist. Der Erwerb von Einzeldatenbanken und die Bereitstellung einer für den Normalfall zu großen Menge an Datenbankressourcen, um für jede Einzeldatenbank variierende Anforderungen oder Anforderungen zu Spitzenzeiten zu erfüllen, ist häufig keine kosteneffiziente Lösung. Mit Pools verwalten Sie die gesamte Leistung des Pools, und die Datenbanken werden automatisch nach oben und unten skaliert. Die intelligente Engine von Azure empfiehlt einen Pool für Datenbanken, wenn es ein entsprechendes Nutzungsmuster erkennt. Details finden Sie in der [Anleitung zu Pools für elastische Datenbanken](sql-database-elastic-pool.md).

## <a name="how-does-the-usage-of-sql-database-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Wie wird die Nutzung von SQL-Datenbank beim DTU-basierten Kaufmodell auf meiner Rechnung ausgewiesen?

SQL-Datenbank wird anhand eines vorhersagbaren Stundensatzes basierend auf dem [Kaufmodell](sql-database-service-tiers-dtu.md) berechnet. Der tatsächliche Verbrauch wird stündlich berechnet und anteilig aufgeführt, sodass auf Ihrer Rechnung auch Bruchteile einer Stunde auftauchen können. Wenn eine Datenbank beispielsweise innerhalb eines Monats 12 Stunden existiert, wird auf Ihrer Rechnung eine Nutzung von 0,5 Tagen aufgeführt.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Was geschieht, wenn eine Einzeldatenbank weniger als eine Stunde aktiv ist oder für weniger als eine Stunde einen höheren Diensttarif verwendet?

Die Abrechnung erfolgt für jede Stunde, in der eine Datenbank den höchsten in dieser Stunde angewandten Diensttarif und die höchste Computegröße nutzt – unabhängig von der Verwendung der Datenbank und ob sie weniger als eine Stunde aktiv war. Wenn Sie beispielsweise eine Einzeldatenbank erstellen und diese fünf Minuten später löschen, wird Ihnen eine volle Datenbankstunde in Rechnung gestellt.

Beispiele:

- Wenn Sie eine Basic-Datenbank erstellen und unmittelbar danach auf Standard S1 aktualisieren, wird Ihnen für die erste Stunde der Standard S1-Satz berechnet.
- Wenn Sie für eine Datenbank um 22:00 Uhr ein Upgrade von Basic auf Premium durchführen und das Upgrade um 01:35 Uhr am folgenden Tag abgeschlossen ist, wird Ihnen die Premium-Rate ab 01:00 Uhr berechnet.
- Wenn Sie für eine Datenbank um 11:00 Uhr ein Downgrade von Premium auf Basic durchführen und der Vorgang um 14:15 Uhr abgeschlossen ist, wird für die Datenbank bis 15:00 Uhr die Premium-Rate berechnet. Danach werden die Basic-Raten berechnet.

## <a name="how-does-elastic-pool-usage-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Wie wird die Nutzung des Pools für elastische Datenbanken beim DTU-basierten Kaufmodell auf meiner Rechnung ausgewiesen?

Die Abrechnung der Gebühren für den Pool für elastische Datenbanken erfolgt auf Ihrer Rechnung als elastische DTUs (eDTUs) oder V-Kerne plus Speicher in den Abstufungen, die auf der [Preisseite](https://azure.microsoft.com/pricing/details/sql-database/) ausgewiesen sind. Für Pools für elastische Datenbanken erfolgt keine Abrechnung pro Datenbank. Die Abrechnung erfolgt für jede Stunde, in der ein Pool auf der höchsten eDTU- oder V-Kern-Ebene existiert. Dies gilt unabhängig davon, ob der Pool genutzt wurde oder ob er weniger als eine Stunde aktiv war.

Beispiele für das DTU-basierte Kaufmodell:

- Wenn Sie um 11:18 Uhr einen Standard-Pool für elastische Datenbanken mit 200 eDTUs erstellen und dem Pool fünf Datenbanken hinzufügen, werden Ihnen ab 11:00 Uhr für den Rest des Tages die 200 eDTUs für die ganze Stunde berechnet.
- An Tag 2 nutzt Datenbank 1 ab 5:05 Uhr den ganzen Tag über 50 eDTUs. Die Nutzung der Datenbanken 2-5 schwankt zwischen 0 und 80 eDTUs. Im Laufe des Tages fügen Sie fünf weitere Datenbanken hinzu, die im Laufe des Tages verschiedene eDTUs nutzen. An Tag 2 werden den ganzen Tag über 200 eDTU berechnet.
- An Tag 3 um 05:00 Uhr fügen Sie weitere 15 Datenbanken hinzu. Die Nutzung der Datenbank steigt im Laufe des Tages, bis Sie um 20:05 Uhr entscheiden, die eDTUs für den Pool von 200 auf 400 aufzustocken. Bis 20.00 Uhr werden Ihnen die Gebühren für 200 eDTUs berechnet. Für die verbleibenden vier Stunden des Tages erfolgt die Abrechnung für 400 eDTUs.

## <a name="how-are-elastic-pool-billed-for-the-dtu-based-purchasing-model"></a>Wie wird der Pool für elastische Datenbanken beim DTU-basierten Kaufmodell abgerechnet?

Pools für elastische Datenbanken werden anhand der folgenden Merkmale abgerechnet:

- Ein Pool für elastische Datenbanken wird bei seiner Erstellung abgerechnet, auch wenn keine Datenbanken im Pool vorhanden sind.
- Ein Pool für elastische Datenbanken wird stündlich abgerechnet. Dies ist die gleiche Häufigkeit wie für Computegrößen von Einzeldatenbanken.
- Wenn die Größe eines Pools für elastische Datenbanken geändert wird, wird der Pool erst dann gemäß der neuen Ressourcenmenge berechnet, wenn die Größenänderung abgeschlossen ist. Dies erfolgt nach dem gleichen Muster wie beim Ändern der Computegröße von Einzeldatenbanken.
- Der Preis für einen Pool für elastische Datenbanken basiert auf den Ressourcen des Pools. Der Preis eines Pools für elastische Datenbanken hängt nicht von der Anzahl und Auslastung der darin enthaltenen elastischen Datenbanken ab.

Ausführliche Informationen finden Sie unter [SQL-Datenbank – Preisübersicht](https://azure.microsoft.com/pricing/details/sql-database/), [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) und [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md).

## <a name="how-does-the-vcore-based-usage-show-up-in-my-bill"></a>Wie wird die V-Kern-basierte Nutzung auf meiner Rechnung ausgewiesen?

In dem auf virtuellen Kernen basierenden Modell wird der Dienst gemäß einem vorhersagbaren Stundensatz abgerechnet. Dieser basiert auf dem Diensttarif, den bereitgestellten Computekapazitäten (in virtuellen Kernen), der monatlich bereitgestellten Speicherkapazität in GB und dem verbrauchten Sicherungsspeicher. Wenn der Speicher für Sicherungen die gesamte Datenbankgröße überschreitet (also 100% der Datenbankgröße), fallen zusätzliche Gebühren an. V-Kern-Stunden, konfigurierter Datenbankspeicher, E/A-Verbrauch und Sicherungsspeicher sind auf der Rechnung einzeln angegeben, damit Sie die Details zu den genutzten Ressourcen leichter verfolgen können. Der Sicherungsspeicher deckt maximal 100% der maximalen Datenbankgröße ab. Bei einer Überschreitung werden die Kosten in GB/Monat gemäß Ihrem monatlichen Verbrauch abgerechnet.

Beispiel: 

- Wenn SQL-Datenbank 12 Stunden aktiv war, wird eine Nutzung von 12 Stunden pro virtuellem Kern auf der Rechnung aufgeführt. Falls für SQL-Datenbank zusätzlich 100 GB Speicher bereitgestellt wurden, werden auf der Rechnung die Speichernutzung in GB/Monat, die anteilsmäßig auf Stundenbasis abgerechnet wird, und die Anzahl der im jeweiligen Monat verbrauchten E/A-Vorgänge aufgeführt.
- Wenn SQL-Datenbank weniger als eine Stunde aktiv war, wird Ihnen jede Stunde in Rechnung gestellt, in der SQL-Datenbank ausgeführt wurde. Dabei wird der höchste ausgewählte Diensttarif und die Höchstmenge an während dieser Stunde bereitgestellten Speicherkapazitäten und E/As berechnet – unabhängig von der Nutzung und unabhängig davon, ob die Datenbank weniger als eine Stunde aktiv war.
- Falls Sie eine verwaltete Instanz erstellen und diese fünf Minuten später wieder löschen, wird eine Datenbankstunde berechnet.
- Wenn Sie eine verwaltete Instanz im Tarif „Universell“ mit acht virtuellen Kernen erstellen und umgehend ein Upgrade auf 16 virtuelle Kerne durchführen, werden Ihnen für die erste Stunde 16 virtuelle Kerne in Rechnung gestellt.

> [!NOTE]
> Für einen begrenzten Zeitraum werden keine Gebühren für Sicherungen und E/A-Vorgänge berechnet.

## <a name="how-does-the-use-of-active-geo-replication-or-auto-failover-groups-in-an-elastic-pool-show-up-on-my-bill"></a>Wie wird die Nutzung der aktiven Georeplikation oder von Autofailover-Gruppen in einem Pool für elastische Datenbanken auf meiner Rechnung ausgewiesen?

Im Gegensatz zu Einzeldatenbanken hat die Nutzung der [aktiven Georeplikation](sql-database-active-geo-replication.md) oder von [Autofailover-Gruppen](sql-database-auto-failover-group.md) mit elastischen Datenbanken keine direkte Auswirkung auf die Abrechnung. Ihnen werden nur die für die einzelnen Pools (primärer Pool und sekundärer Pool) bereitgestellten Ressourcen in Rechnung gestellt.

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Wie wirkt sich die Nutzung der Überwachungsfunktion auf meine Rechnung aus?

Die Überwachung ist ohne weitere Kosten in den SQL-Datenbank-Dienst integriert und für alle Diensttarife verfügbar. Um die Überwachungsprotokolle zu speichern, nutzt die Überwachungsfunktion ein Azure-Speicherkonto, und die Sätze für Tabellen und Warteschlangen im Azure-Speicher werden abhängig von der Größe Ihres Überwachungsprotokolls angewandt.

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Wie setze ich das Kennwort für den Serveradministrator zurück?

Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Computer mit SQL Server**, wählen Sie in der Liste den Server aus, und klicken Sie auf **Kennwort zurücksetzen**.

## <a name="how-do-i-manage-databases-and-logins"></a>Wie verwalte ich Datenbanken und Anmeldungen?

Informationen hierzu finden Sie unter [Steuern und Gewähren von Datenbankzugriff](sql-database-manage-logins.md).

> [!NOTE]
> Sie können den Namen des Serveradministratorkontos nach dem Erstellen nicht mehr ändern.

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Wie stelle ich sicher, dass nur autorisierte IP-Adressen auf einen Server zugreifen können?

Informationen finden Sie unter [Gewusst wie: Konfigurieren von Firewalleinstellungen für SQL-Datenbank](sql-database-configure-firewall-settings.md).

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Wie groß ist die erwartete Replikationsverzögerung bei der geografischen Replikation einer Datenbank zwischen zwei Regionen innerhalb derselben Azure-Geografie?

Derzeit wird eine RPO von fünf Sekunden unterstützt. Die Replikationsverzögerung ist geringer, solange die geografisch sekundäre Datenbank in der von Azure empfohlenen zugeordneten Region gehostet wird und den gleichen Diensttarif hat.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Wie groß ist die erwartete Replikationsverzögerung, wenn die geografisch sekundäre Datenbank in der gleichen Region erstellt wird wie die primäre Datenbank?

Auf Grundlage von empirischen Daten besteht nicht zu viel Unterschied zwischen der Replikationsverzögerung innerhalb von und zwischen Regionen, wenn die von Azure empfohlene zugeordnete Region verwendet wird.

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Wie funktioniert die Wiederholungslogik bei eingerichteter Georeplikation, wenn zwischen zwei Regionen ein Netzwerkfehler auftritt?

Bei einer Trennung der Verbindung, versuchen wir alle 10 Sekunden die Verbindung erneut herzustellen.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Was kann ich tun, um sicherzustellen, dass eine wichtige Änderung in der primären Datenbank repliziert wird?

Die geografisch sekundäre Datenbank ist ein asynchrones Replikat, und wir versuchen nicht, sie vollständig mit der primären Datenbank zu synchronisieren. Wir stellen aber eine Methode zum Erzwingen der Synchronisierung bereit, um die Replikation kritischer Änderungen sicherzustellen (z.B. Kennwortaktualisierungen). Die erzwungene Synchronisierung beeinträchtigt die Leistung, da der aufrufende Thread blockiert wird, bis alle durchgeführten Transaktionen repliziert wurden. Weitere Informationen finden Sie unter [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx).

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Welche Tools stehen zur Überwachung der Replikationsverzögerung zwischen der primären Datenbank und der geografisch sekundären Datenbank zur Verfügung?

Die Replikationsverzögerung zwischen der primären Datenbank und der geografisch sekundären wird über eine DMV verfügbar gemacht. Weitere Informationen finden Sie unter [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>So verschieben Sie eine Datenbank auf einen anderen Server im selben Abonnement

Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **SQL-Datenbanken**, wählen Sie in der Liste eine Datenbank aus, und klicken Sie dann auf **Kopieren**. Weitere Details finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md) .

## <a name="to-move-a-database-between-subscriptions"></a>So verschieben Sie eine Datenbank zwischen Abonnements

Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **SQL-Server**, und wählen Sie in der Liste den Server aus, der die Datenbank hostet. Klicken Sie auf **Verschieben**, und wählen Sie die zu verschiebenden Ressourcen und das Abonnement aus, in das dieser Vorgang erfolgen soll.

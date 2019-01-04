---
title: Hochverfügbarkeit – Azure SQL-Datenbankdienst | Microsoft Docs
description: Erfahren Sie mehr über die Hochverfügbarkeitsfunktionen des Azure SQL-Datenbankdiensts.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 9d80f4e7422d881393c8e626ddfc75c4067ef1e2
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53250347"
---
# <a name="high-availability-and-azure-sql-database"></a>Hochverfügbarkeit und Azure SQL-Datenbank

Azure SQL-Datenbank ist eine hochverfügbare PaaS-Datenbank (Platform as a Service), die sicherstellt, dass Ihre Datenbank ohne Beeinträchtigung durch Wartungs- und Ausfallzeiten zu 99,99 % der Zeit ausgeführt wird. Es handelt sich um einen vollständig verwalteten, in der Azure-Cloud gehosteten SQL Server-Datenbank-Engine-Prozess, der sicherstellt, dass Ihre SQL Server-Datenbank immer aktualisiert und gepatcht wird, ohne Ihre Workload zu beeinträchtigen. Wenn eine Instanz gepatcht oder ein Failover dafür ausgeführt wird, ist die Ausfallzeit im Allgemeinen nicht merklich, wenn Sie in Ihrer App [Wiederholungslogik einsetzen](sql-database-develop-overview.md#resiliency). Wenn die Ausführung eines Failovers länger als 60 Sekunden dauert, sollten Sie eine Supportanfrage öffnen. Azure SQL-Datenbank kann auch unter den kritischsten Umständen schnell wiederhergestellt werden. So wird sichergestellt, dass Ihre Daten immer verfügbar sind.

Die Azure Platform führt eine vollständige Verwaltung jeder Azure SQL-Datenbank durch und garantiert, dass keine Datenverluste auftreten und die Datenverfügbarkeit einen hohen Prozentsatz aufweist. In Azure werden Bereiche wie Patchen, Sicherungen, Replikation, Fehlererkennung, zugrunde liegende potenzielle Hardware-, Software- oder Netzwerkfehler, Bereitstellung von Fehlerbehebungen, Failover, Datenbankupgrades und andere Wartungsaufgaben automatisch durchgeführt. SQL Server-Techniker haben die am besten bekannten Methoden implementiert, sodass sichergestellt ist, dass alle Wartungsvorgänge in weniger als 0,01 % der Zeit des Datenbanklebenszyklus abgeschlossen werden. Mit dieser Architektur wird sichergestellt, dass Daten, die bereits committet wurden, niemals verloren gehen und dass Wartungsvorgänge ohne Beeinträchtigung der Workload ausgeführt werden. Es gibt keine Wartungsfenster oder Ausfallzeiten, aufgrund derer Sie die Workload während der Aktualisierung oder Wartung der Datenbank beenden müssen. Die in Azure SQL-Datenbank integrierte Hochverfügbarkeit garantiert, dass die Datenbank niemals zu einem Single Point of Failure in der Softwarearchitektur wird.

Azure SQL-Datenbank basiert auf der an die Cloudumgebung angepasste Architektur der SQL Server-Datenbank-Engine, um die Verfügbarkeit von 99,99 % selbst bei Infrastrukturausfällen sicherzustellen. In Azure SQL-Datenbank werden zwei Hochverfügbarkeits-Architekturmodelle verwendet (beide mit einer Verfügbarkeit von 99,99 %):

- Diensttarifmodell des Typs „Standard/Universell“, das auf der Trennung von Compute- und Speicherebene basiert. Dieses Architekturmodell beruht auf der Hochverfügbarkeit und Zuverlässigkeit der Speicherebene, kann jedoch eine mögliche Leistungsbeeinträchtigung während Wartungsaktivitäten aufweisen.
- Diensttarifmodell des Typs „Premium/Unternehmenskritisch“, das auf einem Cluster von Datenbank-Engine-Prozessen basiert. Dieses Architekturmodell beruht darauf, dass immer ein Quorum von verfügbaren Datenbank-Engine-Knoten vorhanden ist, und weist selbst während Wartungsaktivitäten eine minimale Leistungsbeeinträchtigung der Workload auf.

In Azure werden das zugrunde liegende Betriebssystem, Treiber und die SQL Server-Datenbank-Engine transparent mit minimaler Ausfallzeit für Endbenutzer aktualisiert und gepatcht. Azure SQL-Datenbank wird auf der neuesten stabilen Version der SQL Server-Datenbank-Engine und des Windows-Betriebssystems ausgeführt. Die meisten Benutzer bemerken nicht, dass die Upgrades kontinuierlich ausgeführt werden.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Verfügbarkeit der Diensttarife „Basic“, „Standard“ und „Universell“

Die Standard-Verfügbarkeit bezieht sich auf eine SLA von 99,99 %, die in den Diensttarifen „Basic“, „Standard“ und „Universell“ angewandt wird. Die Hochverfügbarkeit in diesem Architekturmodell wird durch die Trennung der Compute- und Speicherebene sowie die Replikation der Daten in der Speicherebene erreicht.

In der folgenden Abbildung sind vier Knoten im Architekturmodell des Typs „Standard“ mit getrennter Compute- und Speicherebene dargestellt.

![Trennung der Compute- und Speicherebene](media/sql-database-managed-instance/general-purpose-service-tier.png)

Das Standardverfügbarkeitsmodell umfasst zwei Ebenen:

- Eine zustandslose Computeebene, auf der der Prozess `sqlserver.exe` ausgeführt wird und die nur temporäre und zwischengespeicherte Daten (z.B. Plancache, Pufferpool, Spaltenspeicherpool) enthält. Dieser zustandslose SQL Server-Knoten wird von der Azure Service Fabric-Plattform gesteuert, die Prozesse initialisiert, die Integrität des Knotens steuert und bei Bedarf ein Failover zu einer anderen Stelle durchführt.
- Eine zustandsbehaftete Datenebene mit Datenbankdateien (MDF- und LDF-Dateien), die in Azure Storage Premium gespeichert werden. Azure Storage garantiert die Vermeidung von Datenverlusten jeglicher Datensätze, die in Datensatzdateien platziert werden. Azure Storage verfügt über integrierte Datenverfügbarkeit und -redundanz, die sicherstellen, dass jeder Datensatz in einer Protokolldatei und jede Seite in einer Datendatei beibehalten wird, auch wenn der SQL Server-Prozess abstürzt.

Bei jeder Aktualisierung der Datenbank-Engine oder des Betriebssystems, bei Fehlern in Teilen der zugrunde liegenden Infrastruktur oder wenn im SQL Server-Prozess ein schwerwiegendes Problem erkannt wird, wird der zustandslose SQL Server-Prozess in Azure Service Fabric auf einen anderen zustandslosen Serverknoten verschoben. Es sind mehrere Reserveknoten vorhanden, auf denen im Fall eines Failovers ein neuer Computedienst ausgeführt werden kann, um die Failoverzeit zu minimieren. Daten in der Azure Storage-Ebene sind nicht betroffen, und Daten- und Protokolldateien werden an den neu initialisierten SQL Server-Prozess angefügt. Dieser Prozess garantiert eine Verfügbarkeit von 99,99 %, kann jedoch aufgrund der Übergangszeit und der Tatsache, dass der neue SQL Server-Knoten mit „kaltem“ Cache gestartet wird, Auswirkungen auf die Leistung großer Workloads haben.

## <a name="premium-and-business-critical-service-tier-availability"></a>Verfügbarkeit der Diensttarife „Premium“ und „Unternehmenskritisch“

Die Premium-Verfügbarkeit wird in den Diensttarifen „Premium“ und „Unternehmenskritisch“ von Azure SQL-Datenbank aktiviert und ist für hohe Workloads vorgesehen, bei denen keine Leistungsbeeinträchtigung aufgrund laufender Wartungsvorgänge toleriert werden kann.

Im Premium-Modell werden in Azure SQL-Datenbank der Compute- und Speicherbereich auf dem einzelnen Knoten integriert. Die Hochverfügbarkeit in diesem Architekturmodell wird durch die Replikation auf Compute- (SQL Server-Datenbank-Engine-Prozess) und Speicherebene (lokal angefügte SSD) erreicht, die in einem Cluster mit 4 Knoten anhand einer ähnlichen Technologie wie [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) in SQL Server bereitgestellt werden.

![Cluster von Datenbank-Engine-Knoten](media/sql-database-managed-instance/business-critical-service-tier.png)

Der SQL-Datenbank-Engine-Prozess sowie die zugrunde liegenden MDF- und LDF-Dateien werden mit lokal angefügtem SSD-Speicher auf demselben Knoten platziert und bieten eine geringe Latenz für die Workload. Hochverfügbarkeit wird anhand einer ähnlichen Technologie wie [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) in SQL Server implementiert. Jede Datenbank ist ein Cluster von Datenbankknoten mit einer primären Datenbank, die für die Kundenworkload zugänglich ist, und drei sekundären Prozessen, die Kopien der Daten enthalten. Der primäre Knoten überträgt die Änderungen kontinuierlich zu sekundären Knoten, um sicherzustellen, dass die Daten auf sekundären Replikaten verfügbar sind, wenn der primäre Knoten aus bestimmten Gründen ausfällt. Ein Failover wird von Azure Service Fabric verarbeitet: Ein sekundäres Replikat wird der primäre Knoten, und ein neues sekundäres Replikat wird erstellt, um sicherzustellen, dass ausreichend Knoten im Cluster vorhanden sind. Die Workload wird automatisch zum neuen primären Knoten umgeleitet.

Darüber hinaus umfasst der Cluster des Typs „Unternehmenskritisch“ eine integrierte Funktion [Horizontale Leseskalierung](sql-database-read-scale-out.md), die einen gebührenfreien, integrierten und schreibgeschützten Knoten bereitstellt, der zum Ausführen von schreibgeschützten Abfragen (z.B. Berichten) verwendet werden kann, die die Leistung der primären Workload nicht beeinträchtigen sollen.

## <a name="zone-redundant-configuration"></a>Zonenredundante Konfiguration

Standardmäßig werden die Quorumssatzreplikate für die Konfigurationen mit lokalem Speicher im gleichen Rechenzentrum erstellt. Durch die Einführung von [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) haben Sie die Möglichkeit, die verschiedenen Replikate in den Quorumssätzen in unterschiedlichen Verfügbarkeitszonen in derselben Region zu platzieren. Um einen Single Point of Failure auszuschließen, wird der Steuerring zudem in mehreren Zonen als drei Gatewayringe (GW) kopiert. Die Weiterleitung an einen bestimmten Gatewayring wird durch [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM) gesteuert. Da bei der zonenredundanten Konfiguration keine zusätzliche Datenbankredundanz erzeugt wird, ist die Verwendung von Verfügbarkeitszonen in den Diensttarifen „Premium“ oder „Unternehmenskritisch“ ohne Zusatzkosten verfügbar. Durch die Auswahl einer zonenredundanten Datenbank können Sie Ihre Datenbanken der Dienstebenen „Premium“ oder „Unternehmenskritisch“ für deutlich mehr Ausfallszenarien resistent machen – z.B. für schwerwiegende Rechenzentrumsausfälle –, ohne Änderungen an der Anwendungslogik vornehmen zu müssen. Sie können zudem alle vorhandenen Datenbanken oder Pools der Dienstebenen „Premium“ oder „Unternehmenskritisch“ in die zonenredundante Konfiguration konvertieren.

Da der zonenredundante Quorumssatz über Replikate in verschiedenen Rechenzentren mit einiger Entfernung dazwischen verfügt, kann sich durch die erhöhte Netzwerklatenz die Commitzeit erhöhen und dadurch die Leistung einiger OLTP-Workloads beeinträchtigt werden. Sie können jederzeit zur Einzelzonenkonfiguration zurückkehren, indem Sie die zonenredundante Einstellung deaktivieren. Dieser Prozess ist datenintensiv und ähnelt dem regulären Diensttarifupdate. Am Ende des Prozesses wird die Datenbank oder der Pool aus einem zonenredundanten Ring zum Ring einer einzelnen Zone migriert (oder umgekehrt).

> [!IMPORTANT]
> Zonenredundante Datenbanken und Pools für elastische Datenbanken werden derzeit nur für den Diensttarif „Premium“ unterstützt. Standardmäßig werden Sicherungen und Überwachungsdatensätze im RA-GRS-Speicher gespeichert und sind daher möglicherweise bei einem Ausfall der gesamten Zone nicht automatisch verfügbar. 

Die zonenredundante Version der Hochverfügbarkeitsarchitektur wird im folgenden Diagramm veranschaulicht:

![Hochverfügbarkeitsarchitektur, zonenredundant](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="accelerated-database-recovery-adr"></a>Schnellere Datenbankwiederherstellung

Die [schnellere Datenbankwiederherstellung (Accelerated Database Recovery, ADR)](sql-database-accelerated-database-recovery.md) ist eine neue Funktion der SQL-Datenbank-Engine, mit der die Datenbankverfügbarkeit durch einen Neuentwurf des Wiederherstellungsprozesses der SQL-Datenbank-Engine erheblich verbessert wird, insbesondere bei Transaktionen mit langer Ausführungszeit. ADR ist derzeit für Einzeldatenbanken, Pools für elastische Datenbanken und Azure SQL Data Warehouse verfügbar.

## <a name="conclusion"></a>Zusammenfassung

Azure SQL-Datenbank ist nahtlos in die Azure-Plattform integriert und bei der Fehlererkennung und Wiederherstellung in hohem Maße von Service Fabric, in Verbindung mit dem Datenschutz von Azure Storage Blobs und für höhere Fehlertoleranz von Verfügbarkeitszonen abhängig. Gleichzeitig nutzt Azure SQL-Datenbank in vollem Umfang die Technologie der AlwaysOn-Verfügbarkeitsgruppen des SQL Server-Standardprodukts für Replikation und Failover. Dank der Kombination dieser Technologien können Anwendungen die Vorteile eines gemischten Speichermodells voll ausschöpfen und sehr anspruchsvolle SLAs unterstützen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md)
- Weitere Informationen zu [Service Fabric](../service-fabric/service-fabric-overview.md)
- Weitere Informationen zu [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
- Weitere Optionen für Hochverfügbarkeit und Notfallwiederherstellung finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md).

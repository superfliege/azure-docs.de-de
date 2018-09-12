---
title: Hochverfügbarkeit – Azure SQL-Datenbankdienst | Microsoft Docs
description: Erfahren Sie mehr über die Hochverfügbarkeitsfunktionen des Azure SQL-Datenbankdiensts.
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: jovanpop
ms.reviewer: carlrab, sashan
ms.openlocfilehash: 7a60d800ce76f8ff9a903cc068fa7bc87cd33f3f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700634"
---
# <a name="high-availability-and-azure-sql-database"></a>Hochverfügbarkeit und Azure SQL-Datenbank

Azure SQL-Datenbank ist eine hochverfügbare PaaS-Datenbank (Platform as a Service), die sicherstellt, dass Ihre Datenbank ohne Beeinträchtigung durch Wartungs- und Ausfallzeiten zu 99,99 % der Zeit ausgeführt wird. Es handelt sich um einen vollständig verwalteten, in der Azure-Cloud gehosteten SQL Server-Datenbank-Engine-Prozess, der sicherstellt, dass Ihre SQL Server-Datenbank immer aktualisiert und gepatcht wird, ohne Ihre Workload zu beeinträchtigen. Wenn eine Instanz gepatcht oder ein Failover dafür ausgeführt wird, ist die Ausfallzeit im Allgemeinen nicht merklich, wenn Sie in Ihrer App [Wiederholungslogik einsetzen](sql-database-develop-overview.md#resiliency). Wenn die Ausführung eines Failovers länger als 60 Sekunden dauert, sollten Sie eine Supportanfrage öffnen. Azure SQL-Datenbank kann auch unter den kritischsten Umständen schnell wiederhergestellt werden. So wird sichergestellt, dass Ihre Daten immer verfügbar sind.

Die Azure Platform führt eine vollständige Verwaltung jeder Azure SQL-Datenbank durch und garantiert, dass keine Datenverluste auftreten und die Datenverfügbarkeit einen hohen Prozentsatz aufweist. In Azure werden Bereiche wie Patchen, Sicherungen, Replikation, Fehlererkennung, zugrunde liegende potenzielle Hardware-, Software- oder Netzwerkfehler, Bereitstellung von Fehlerbehebungen, Failover, Datenbankupgrades und andere Wartungsaufgaben automatisch durchgeführt. SQL Server-Techniker haben die am besten bekannten Methoden implementiert, sodass sichergestellt ist, dass alle Wartungsvorgänge in weniger als 0,01 % der Zeit des Datenbanklebenszyklus abgeschlossen werden. Mit dieser Architektur wird sichergestellt, dass Daten, die bereits committet wurden, niemals verloren gehen und dass Wartungsvorgänge ohne Beeinträchtigung der Workload ausgeführt werden. Es gibt keine Wartungsfenster oder Ausfallzeiten, aufgrund derer Sie die Workload während der Aktualisierung oder Wartung der Datenbank beenden müssen. Die in Azure SQL-Datenbank integrierte Hochverfügbarkeit garantiert, dass die Datenbank niemals zu einem Single Point of Failure in der Softwarearchitektur wird.

Azure SQL-Datenbank basiert auf der an die Cloudumgebung angepasste Architektur der SQL Server-Datenbank-Engine, um die Verfügbarkeit von 99,99 % selbst bei Infrastrukturausfällen sicherzustellen. In Azure SQL-Datenbank werden zwei Hochverfügbarkeits-Architekturmodelle verwendet (beide mit einer Verfügbarkeit von 99,99 %):
- Modell des Typs „Standard/Universell“, das auf der Trennung der Compute- und Speicherebene basiert. Dieses Architekturmodell beruht auf der Hochverfügbarkeit und Zuverlässigkeit der Speicherebene, kann jedoch eine mögliche Leistungsbeeinträchtigung während Wartungsaktivitäten aufweisen.
- Modell des Typs „Premium/Unternehmenskritisch“, das in einem Cluster von Datenbank-Engine-Prozessen basiert. Dieses Architekturmodell beruht darauf, dass immer ein Quorum von verfügbaren Datenbank-Engine-Knoten vorhanden ist, und weist selbst während Wartungsaktivitäten eine minimale Leistungsbeeinträchtigung der Workload auf.

In Azure werden das zugrunde liegende Betriebssystem, Treiber und die SQL Server-Datenbank-Engine transparent mit minimaler Ausfallzeit für Endbenutzer aktualisiert und gepatcht. Azure SQL-Datenbank wird auf der neuesten stabilen Version der SQL Server-Datenbank-Engine und des Windows-Betriebssystems ausgeführt. Die meisten Benutzer bemerken nicht, dass die Upgrades kontinuierlich ausgeführt werden.

## <a name="standardgeneral-purpose-availability"></a>Verfügbarkeit des Typs „Standard/Universell“

Die Standard-Verfügbarkeit bezieht sich auf eine SLA von 99,99 %, die in den Tarifen „Standard“, „Basic“ und „Universell“ angewandt wird. Die Hochverfügbarkeit in diesem Architekturmodell wird durch die Trennung der Compute- und Speicherebene sowie die Replikation der Daten in der Speicherebene erreicht.

In der folgenden Abbildung sind vier Knoten im Architekturmodell des Typs „Standard“ mit getrennter Compute- und Speicherebene dargestellt.

![Trennung der Compute- und Speicherebene](media/sql-database-managed-instance/general-purpose-service-tier.png)

Das Standardverfügbarkeitsmodell umfasst zwei Ebenen:

- Eine zustandslose Computeebene, auf der der Prozess „sqlserver.exe“ ausgeführt wird und die nur temporäre und zwischengespeicherte Daten (z.B. Plancache Pufferpool, Spaltenspeicherpool) enthält. Dieser zustandslose SQL Server-Knoten wird von der Azure Service Fabric-Plattform gesteuert, die Prozesse initialisiert, die Integrität des Knotens steuert und bei Bedarf ein Failover zu einer anderen Stelle durchführt.
- Eine zustandsbehaftete Datenebene mit Datenbankdateien (MDF- und LDF-Dateien), die in Azure Storage Premium gespeichert werden. Azure Storage garantiert die Vermeidung von Datenverlusten jeglicher Datensätze, die in Datensatzdateien platziert werden. Azure Storage verfügt über integrierte Datenverfügbarkeit und -redundanz, die sicherstellen, dass jeder Datensatz in einer Protokolldatei und jede Seite in einer Datendatei beibehalten wird, auch wenn der SQL Server-Prozess abstürzt.

Bei jeder Aktualisierung der Datenbank-Engine oder des Betriebssystems, bei Fehlern in Teilen der zugrunde liegenden Infrastruktur oder wenn im SQL Server-Prozess ein schwerwiegendes Problem erkannt wird, wird der zustandslose SQL Server-Prozess in Azure Service Fabric auf einen anderen zustandslosen Serverknoten verschoben. Es sind mehrere Reserveknoten vorhanden, auf denen im Fall eines Failovers ein neuer Computedienst ausgeführt werden kann, um die Failoverzeit zu minimieren. Daten in der Azure Storage-Ebene sind nicht betroffen, und Daten- und Protokolldateien werden an den neu initialisierten SQL Server-Prozess angefügt. Dieser Prozess garantiert eine Verfügbarkeit von 99,99 %, kann jedoch aufgrund der Übergangszeit und der Tatsache, dass der neue SQL Server-Knoten mit „kaltem“ Cache gestartet wird, Auswirkungen auf die Leistung großer Workloads haben.

## <a name="premiumbusiness-critical-availability"></a>Verfügbarkeit des Typs „Premium/Unternehmenskritisch“

Die Premium-Verfügbarkeit wird im Premium-Tarif von Azure SQL-Datenbank aktiviert und ist für hohe Workloads vorgesehen, bei denen keine Leistungsbeeinträchtigung aufgrund laufender Wartungsvorgänge toleriert werden kann.

Im Premium-Modell werden in Azure SQL-Datenbank der Compute- und Speicherbereich auf dem einzelnen Knoten integriert. Die Hochverfügbarkeit in diesem Architekturmodell wird durch die Replikation auf Compute- (SQL Server-Datenbank-Engine-Prozess) und Speicherebene (lokal angefügte SSD) erreicht, die in einem Cluster von [Always On-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) auf 4 Knoten bereitgestellt werden.

![Cluster von Datenbank-Engine-Knoten](media/sql-database-managed-instance/business-critical-service-tier.png)

Der SQL Server-Datenbank-Engine-Prozess sowie die zugrunde liegenden MDF- und LDF-Dateien werden mit lokal angefügtem SSD-Speicher auf demselben Knoten platziert und bieten eine geringe Latenz für die Workload. Hochverfügbarkeit wird mithilfe von standardmäßigen [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) implementiert. Jede Datenbank ist ein Cluster von Datenbankknoten mit einer primären Datenbank, die für die Kundenworkload zugänglich ist, und drei sekundären Prozessen, die Kopien der Daten enthalten. Der primäre Knoten überträgt die Änderungen kontinuierlich zu sekundären Knoten, um sicherzustellen, dass die Daten auf sekundären Replikaten verfügbar sind, wenn der primäre Knoten aus bestimmten Gründen ausfällt. Ein Failover wird von der SQL Server-Datenbank-Engine verarbeitet: Ein sekundäres Replikat wird zum primären Knoten, und ein neues sekundäres Replikat wird erstellt, um sicherzustellen, dass ausreichend Knoten im Cluster vorhanden sind. Die Workload wird automatisch zum neuen primären Knoten umgeleitet.

Darüber hinaus umfasst der Cluster des Typs „Unternehmenskritisch“ einen integrierten schreibgeschützten Knoten, der zum Ausführen von schreibgeschützten Abfragen (z.B. Berichten) verwendet werden kann, die die Leistung der primären Workload nicht beeinträchtigen sollen. 

## <a name="zone-redundant-configuration-preview"></a>Zonenredundante Konfiguration (Vorschau)

Standardmäßig werden die Quorumssatzreplikate für die Konfigurationen mit lokalem Speicher im gleichen Rechenzentrum erstellt. Durch die Einführung von [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) haben Sie die Möglichkeit, die verschiedenen Replikate in den Quorumssätzen in unterschiedlichen Verfügbarkeitszonen in derselben Region zu platzieren. Um einen Single Point of Failure auszuschließen, wird der Steuerring zudem in mehreren Zonen als drei Gatewayringe (GW) kopiert. Die Weiterleitung an einen bestimmten Gatewayring wird durch [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM) gesteuert. Da bei der zonenredundanten Konfiguration keine zusätzliche Datenbankredundanz erzeugt wird, ist die Verwendung von Verfügbarkeitszonen auf den Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) ohne Zusatzkosten verfügbar. Durch die Auswahl einer zonenredundanten Datenbank können Sie Ihre Datenbanken der Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) für deutlich mehr Ausfallszenarien resistent machen – z.B. für schwerwiegende Rechenzentrumsausfälle –, ohne Änderungen an der Anwendungslogik vornehmen zu müssen. Sie können zudem alle vorhandenen Datenbanken oder Pools der Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) in die zonenredundante Konfiguration konvertieren.

Da der zonenredundante Quorumssatz über Replikate in verschiedenen Rechenzentren mit einiger Entfernung dazwischen verfügt, kann sich durch die erhöhte Netzwerklatenz die Commitzeit erhöhen und dadurch die Leistung einiger OLTP-Workloads beeinträchtigt werden. Sie können jederzeit zur Einzelzonenkonfiguration zurückkehren, indem Sie die zonenredundante Einstellung deaktivieren. Dieser Prozess ist datenintensiv und ähnelt dem SLO-Update (Service Level Objective). Am Ende des Prozesses wird die Datenbank oder der Pool aus einem zonenredundanten Ring zum Ring einer einzelnen Zone migriert (oder umgekehrt).

> [!IMPORTANT]
> Zonenredundante Datenbanken und Pools für elastische Datenbanken werden derzeit nur für den Diensttarif „Premium“ unterstützt. In der Public Preview-Phase werden Sicherungen und Überwachungsdatensätze im RA-GRS-Speicher gespeichert und sind daher möglicherweise bei einem Ausfall der gesamten Zone nicht automatisch verfügbar. 

Die zonenredundante Version der Hochverfügbarkeitsarchitektur wird im folgenden Diagramm veranschaulicht:
 
![Hochverfügbarkeitsarchitektur, zonenredundant](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Horizontale Leseskalierung
Wie bereits beschrieben, verwenden die Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) Quorumssätze und Always On-Technologie für Hochverfügbarkeit sowohl in Einzelzonenkonfigurationen als auch in zonenredundanten Konfigurationen. Einer der Vorteile der Always On-Technologie besteht darin, dass sich die Replikate jederzeit in einem transaktionssicheren Zustand befinden. Da die Replikate die gleiche Leistungsstufe aufweisen wie das primäre Replikat, kann die Anwendung kostenlos von dieser zusätzlichen Kapazität für die schreibgeschützten Workloads profitieren (horizontale Leseskalierung). Auf diese Weise werden die schreibgeschützten Abfragen von der Hauptworkload für Lesen und Schreiben isoliert und beeinträchtigen deren Leistung nicht. Das Feature der horizontalen Leseskalierung ist für Anwendungen konzipiert, die logisch getrennte schreibgeschützte Workloads z.B. zur Analyse umfassen und daher von dieser zusätzlichen Kapazität profitieren können, ohne eine Verbindung mit dem primären Replikat herstellen zu müssen. 

Um die horizontale Leseskalierung mit einer bestimmten Datenbank zu verwenden, müssen Sie das Feature explizit beim Erstellen der Datenbank aktivieren. Sie können es auch später aktivieren, indem Sie die Konfiguration ändern. Hierzu rufen Sie in PowerShell die Cmdlets [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) oder [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) auf oder verwenden die Methode [Datenbanken – Erstellen oder Aktualisieren](/rest/api/sql/databases/createorupdate) in der Azure Resource Manager-REST-API.

Nachdem die horizontale Leseskalierung für eine Datenbank aktiviert wurde, werden Anwendungen, die eine Verbindung mit dieser Datenbank herstellen, entweder an das Replikat mit Lese-/Schreibzugriff oder an ein schreibgeschütztes Replikat dieser Datenbank weitergeleitet. Die Weiterleitung richtet sich nach der `ApplicationIntent`-Eigenschaft, die in der Verbindungszeichenfolge der Anwendung konfiguriert ist. Informationen zur `ApplicationIntent`-Eigenschaft finden Sie unter [Angeben der Anwendungsabsicht](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Wenn horizontale Leseskalierung deaktiviert ist, oder Sie die ReadScale-Eigenschaft in einer nicht unterstützten Dienstebene festlegen, werden alle Verbindungen unabhängig von der `ApplicationIntent`-Eigenschaft an das Replikat mit Lese-/Schreibzugriff weitergeleitet.  

## <a name="conclusion"></a>Zusammenfassung
Azure SQL-Datenbank ist nahtlos in die Azure-Plattform integriert und bei der Fehlererkennung und Wiederherstellung in hohem Maße von Service Fabric, in Verbindung mit dem Datenschutz von Azure Storage Blobs und für höhere Fehlertoleranz von Verfügbarkeitszonen abhängig. Gleichzeitig nutzt Azure SQL-Datenbank in vollem Umfang die Technologie der AlwaysOn-Verfügbarkeitsgruppen des SQL Server-Standardprodukts für Replikation und Failover. Dank der Kombination dieser Technologien können Anwendungen die Vorteile eines gemischten Speichermodells voll ausschöpfen und sehr anspruchsvolle SLAs unterstützen. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md)
- Weitere Informationen zu [Service Fabric](../service-fabric/service-fabric-overview.md)
- Weitere Informationen zu [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 

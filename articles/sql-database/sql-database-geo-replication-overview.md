---
title: Failovergruppen und aktive Georeplikation – Azure SQL-Datenbank | Microsoft-Dokumentation
description: Verwenden von automatischen Failovergruppen mit aktiver Georeplikation und Aktivieren von automatischem Failover bei Ausfällen
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 07c17d248d78313f1c5f6f1025ae06a623b75944
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259346"
---
# <a name="overview-active-geo-replication-and-auto-failover-groups"></a>Übersicht: Aktive Georeplikation und Gruppen für automatisches Failover
Aktive Georeplikation ist als Geschäftskontinuitätslösung konzipiert, die der Anwendung im Falle eines größeren Rechenzentrumsausfalls eine schnelle Notfallwiederherstellung ermöglicht. Wenn Georeplikation aktiviert ist, kann die Anwendung ein Failover auf eine sekundäre Datenbank in einer anderen Azure-Region initiieren. Bis zu vier sekundäre Datenbanken werden in derselben oder verschiedenen Regionen unterstützt, und die sekundären Datenbanken können auch für schreibgeschützten Abfragezugriff verwendet werden. Das Failover muss durch die Anwendung oder den Benutzer manuell eingeleitet werden. Nach einem Failover hat die neue primäre Datenbank einen anderen Verbindungsendpunkt. 

> [!NOTE]
> Die aktive Georeplikation ist jetzt für alle Datenbanken in allen Diensttarifen und allen Regionen verfügbar.
>  

Gruppen für automatisches Failover sind eine Erweiterung der aktiven Georeplikation. Sie dienen zum Verwalten des simultanen Failovers mehrerer georeplizierter Datenbanken mittels eines durch die Anwendung initiierten Failovers bzw. durch Delegieren des Failovers an den SQL-Datenbank-Dienst auf Basis benutzerdefinierter Kriterien. Letzteres gibt Ihnen die Möglichkeit, nach schwerwiegenden Ausfällen oder anderen ungeplanten Ereignissen, die zum vollständigen oder teilweisen Verlust der Verfügbarkeit der Dienste von SQL-Datenbank in der primären Region führen, automatisch mehrere verwandte Datenbanken in einer sekundären Region wiederherzustellen. Außerdem können sie die lesbaren sekundären Datenbanken zur Auslagerung schreibgeschützter Abfrageworkloads verwenden. Da Gruppen für automatisches Failover mehrere Datenbanken beinhalten, müssen diese Datenbanken auf dem primären Server konfiguriert werden. Primäre und sekundäre Server für die Datenbanken in der Failovergruppe müssen sich im selben Abonnement befinden. Gruppen für automatisches Failover unterstützen die Replikation aller Datenbanken in der Gruppe auf nur einen sekundären Server in einer anderen Region.

> [!NOTE]
> Verwenden Sie aktive Georeplikation, wenn mehrere sekundäre Datenbanken erforderlich sind.
>  

Falls Sie die aktive Georeplikation verwenden und Ihre primäre Datenbank aus beliebigem Grund ausfällt oder einfach offline geschaltet werden muss, können Sie ein Failover auf eine der sekundären Datenbanken initiieren. Wenn das Failover auf eine sekundäre Datenbank aktiviert ist, werden alle anderen sekundären Datenbanken automatisch mit der neuen primären Datenbank verknüpft. Wenn Sie Gruppen für automatisches Failover zum Verwalten der Datenbankwiederherstellung verwenden, führt jeder Ausfall, der eine oder mehrere der Datenbanken in der Gruppe betrifft, zu einem automatischen Failover. Sie können eine Richtlinie für automatisches Failover konfigurieren, die Ihren Anwendungsanforderungen am besten entspricht, oder Sie können stattdessen die manuelle Aktivierung verwenden. Darüber hinaus bieten Gruppen für automatisches Failover Lese-/Schreib-Listenerendpunkte, die während eines Failovers unverändert bleiben. Sowohl bei manueller als auch automatischer Failoveraktivierung schaltet das Failover alle sekundären Datenbanken in der Gruppe zu primären um. Nach Abschluss des Datenbankfailovers wird der DNS-Eintrag automatisch aktualisiert, um die Endpunkte in die neue Region umzuleiten.

Sie können Replikation und Failover für eine einzelne Datenbank oder eine Gruppe von Datenbanken auf einem Server oder in einem Pool für elastische Datenbanken mithilfe der aktiven Georeplikation verwalten. Verwenden Sie dazu 

- Das [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell: einzelne Datenbank](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Pool für elastische Datenbanken](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [PowerShell: Failovergruppe](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [Transact-SQL: einzelne Datenbank oder Pool für elastische Datenbanken](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST-API: einzelne Datenbank](/rest/api/sql/replicationlinks/failover)
- [REST-API: Failovergruppe](/rest/api/sql/failovergroups/failover). 
 
Stellen Sie nach dem Failover sicher, dass die Authentifizierungsanforderungen für Ihren Server und Ihre Datenbank auf der neuen primären konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md). Dies gilt für aktive Georeplikation und Gruppen für automatisches Failover gleichermaßen.

Aktive Georeplikation nutzt die [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)-Technologie von SQL Server, um Transaktionen mit ausgeführtem Commit in der primären Datenbank asynchron mit Momentaufnahmeisolation in eine sekundäre Datenbank zu replizieren. Gruppen für automatisches Failover stellen die Gruppensemantik über der aktiven Georeplikation bereit, es wird aber der gleiche asynchrone Replikationsmechanismus verwendet. Wenngleich die sekundäre Datenbank stets ein wenig hinter der primären Datenbank zurückliegt, sind unvollständige Transaktionen bei sekundären Daten garantiert ausgeschlossen. Regionsübergreifende Redundanz ermöglicht Anwendungen die schnelle Wiederherstellung nach einem dauerhaften Ausfall eines gesamten Rechenzentrums oder von Teilen eines Rechenzentrums aufgrund von Naturkatastrophen, schwerwiegendem menschlichen Versagen oder böswilligen Handlungen. Die spezifischen RPO-Daten finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).

Die folgende Abbildung zeigt ein Beispiel für die Konfiguration der aktiven Georeplikation, wobei sich die primäre Datenbank in der Region „USA, Norden-Mitte“ und die sekundäre Datenbank in der Region „USA, Süden-Mitte“ befindet.

![Georeplikationsbeziehung](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Weil die sekundären Datenbanken lesbar sind, können sie zum Auslagern schreibgeschützter Workloads wie Berichtsaufträge verwendet werden. Bei Verwendung der aktiven Georeplikation können Sie die sekundäre Datenbank in der gleichen Region wie die primäre erstellen, aber dies steigert nicht die Stabilität der Anwendung gegenüber schwerwiegenden Ausfällen. Wenn Sie Gruppen für automatisches Failover verwenden, werden die sekundären Datenbanken immer in einer anderen Region erstellt.

Zusätzlich zur Wiederherstellung im Notfall kann aktive Georeplikation in den folgenden Szenarien verwendet werden:

* **Datenbankmigration:** Sie können die aktive Georeplikation zur Onlinemigration einer Datenbank von einem Server auf einen anderen mit minimalen Ausfallzeiten nutzen.
* **Anwendungsupgrades**: Sie können bei Anwendungsupgrades eine zusätzliche sekundäre Datenbank als Failbackkopie erstellen.

Wenn Sie echte Geschäftskontinuität erreichen möchten, ist das Bereitstellen von Datenbankredundanz zwischen Rechenzentren jedoch nur ein Teil der Lösung. Für die komplette Wiederherstellung einer Anwendung bzw. eines Diensts nach einem schwerwiegenden Fehler ist das Wiederherstellen aller Komponenten erforderlich, aus denen sich der Dienst und alle abhängigen Dienste zusammensetzen. Beispiele dieser Komponenten sind die Clientsoftware (z. B. ein Browser mit benutzerdefiniertem JavaScript), Web-Front-Ends, Speicher und DNS. Es ist wichtig, dass alle Komponenten hinsichtlich derselben Fehler gegen Ausfälle geschützt und innerhalb des RTO (Recovery Time Objective) der Anwendung wieder verfügbar sind. Daher müssen Sie alle abhängigen Dienste bestimmen und mit dem Leistungsumfang und den Funktionen vertraut sein, die sie bieten. Dann müssen Sie entsprechende Maßnahmen ergreifen, um sicherzustellen, dass Ihr Dienst während des Failovers der Dienste funktioniert, von denen er abhängig ist. Weitere Informationen zum Entwerfen von Lösungen für die Notfallwiederherstellung finden Sie unter [Entwerfen von Cloudlösungen für die Notfallwiederherstellung mithilfe der aktiven Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Möglichkeiten der aktiven Georeplikation
Das Feature der aktiven Georeplikation bietet die folgenden wichtigen Möglichkeiten:
* **Automatische asynchrone Replikation**: Sie können eine sekundäre Datenbank nur durch Hinzufügen zu einer vorhandenen Datenbank erstellen. Die sekundäre Datenbank kann auf einem anderen Azure SQL-Datenbank-Server erstellt werden. Nach der Erstellung wird die sekundäre Datenbank mit den Daten aufgefüllt, die aus der primären Datenbank kopiert wurden. Dieser Prozess wird als Seeding bezeichnet. Nachdem eine sekundäre Datenbank erstellt wurde und das Seeding erfolgt ist, werden Aktualisierungen der primären Datenbank automatisch asynchron in die sekundäre Datenbank repliziert. Asynchrone Replikation bedeutet, dass für Transaktionen in der primären Datenbank ein Commit erfolgt, ehe sie in die sekundäre Datenbank repliziert werden. 
* **Lesbare sekundäre Datenbanken:** Eine Anwendung kann für schreibgeschützte Vorgänge auf eine sekundäre Datenbank zugreifen, indem sie die Sicherheitsprinzipale für den Zugriff auf die primäre Datenbank oder andere Sicherheitsprinzipale verwendet. Die sekundären Datenbanken werden im Momentaufnahme-Isolationsmodus betrieben. Auf diese Weise wird sichergestellt, dass die Replikation von Aktualisierungen der primären Datenbank (Protokollwiedergabe) nicht durch Abfragen verzögert wird, die für die sekundäre Datenbank ausgeführt werden.

> [!NOTE]
> Die Protokollwiedergabe wird in der sekundären Datenbank verzögert, wenn Schemaupdates für die primäre Datenbank vorhanden sind. In diesem Fall ist eine Schemasperre für die sekundäre Datenbank erforderlich. 
> 

* **Mehrere lesbare sekundäre Datenbanken**: Mehrere sekundäre Datenbanken sorgen für mehr Redundanz und einen besseren Schutz der primären Datenbank und Anwendung. Wenn mehrere sekundäre Datenbanken vorhanden sind, bleibt die Anwendung auch bei Ausfall einer der sekundären Datenbanken geschützt. Wenn es nur eine sekundäre Datenbank gibt und diese ausfällt, ist die Anwendung bis zum Erstellen einer neuen sekundären Datenbank einem höheren Risiko ausgesetzt.

> [!NOTE]
> Wenn Sie mit der aktiven Georeplikation eine global verteilte Anwendung erstellen und schreibgeschützten Zugriff auf Daten in mehr als vier Regionen bereitstellen müssen, können Sie eine sekundäre Datenbank einer sekundären Datenbank erstellen (dieser Prozess wird als Verkettung bezeichnet). Auf diese Weise können Sie die Datenbankreplikation praktisch unbegrenzt skalieren. Darüber hinaus verkürzt die Verkettung den Mehraufwand der Replikation von der primären Datenbank. Der Nachteil besteht in der erhöhten Replikationsverzögerung in den äußersten sekundären Datenbanken. 
>

* **Unterstützung von Datenbanken in einem Pool für elastische Datenbanken:** Jedes Replikat kann einzeln an einem Pool für elastische Datenbanken teilnehmen oder sich in keinem Pool befinden. Die Auswahl des Pools für jedes Replikat erfolgt einzeln und ist nicht von der Konfiguration eines anderen Replikats abhängig (ob primär oder sekundär). Jeder Pool für elastische Datenbanken befindet sich innerhalb einer einzelnen Region, daher können mehrere Replikate in derselben Topologie nie einen Pool für elastische Datenbanken gemeinsam verwenden.
* **Konfigurierbare Leistungsstufe der sekundären Datenbank**: Sowohl primäre als auch sekundäre Datenbanken sind erforderlich, um die gleiche Dienstebene zu haben. Eine sekundäre Datenbank kann mit einer niedrigeren Leistungsstufe (DTUs) als die primäre erstellt werden. Diese Option wird nicht für Anwendungen mit Datenbanken empfohlen, die eine hohe Schreibaktivität aufweisen, da die größere Replikationsverzögerung das Risiko erheblicher Datenverluste nach einem Failover steigert. Darüber hinaus wird nach einem Failover die Leistung der Anwendung beeinträchtigt, bis die neue primäre Datenbank auf eine höhere Leistungsstufe aktualisiert ist. Das Diagramm mit dem Protokoll-E/A-Prozentsatz im Azure-Portal bietet eine gute Möglichkeit, einzuschätzen, welche Leistungsstufe für die sekundäre Datenbank mindestens erforderlich ist, um die Replikationslast zu bewältigen. Wenn die Leistungsstufe der primären Datenbank beispielsweise P6 (1.000 DTU) ist und ihr Protokoll-E/A-Prozentsatz 50 % beträgt, muss die Leistungsstufe der sekundären Datenbank mindestens P4 (500 DTU) sein. Sie können die Protokoll-E/A-Daten auch mithilfe der Datenbanksicht [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) oder [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) abrufen.  Weitere Informationen zu den SQL-Datenbankleistungsstufen finden Sie unter [Wie heißen die Diensttarife von SQL-Datenbank?](sql-database-service-tiers.md). 
* **Benutzergesteuertes Failover und Failback:** Eine sekundäre Datenbank kann jederzeit durch die Anwendung oder den Benutzer explizit die primäre Rolle erhalten. Bei einem echten Ausfall sollte die Option „Nicht geplant“ verwendet werden, die eine sekundäre Datenbank umgehend zu einer primären Datenbank heraufstuft. Wenn die ausgefallene primäre Datenbank wiederhergestellt ist und wieder zur Verfügung steht, kennzeichnet das System diese wiederhergestellte primäre Datenbank automatisch als sekundäre Datenbank und bringt sie auf den aktuellen Stand der neuen primären Datenbank. Aufgrund der asynchronen Natur der Replikation kann eine kleine Menge von Daten bei einem nicht geplanten Failover verloren gehen, wenn eine primäre Datenbank ausfällt, bevor die aktuellen Änderungen in die sekundäre Datenbank repliziert wurden. Wenn für eine primäre Datenbank mit mehreren sekundären Datenbanken ein Failover durchgeführt wird, konfiguriert das System automatisch die Replikationsbeziehungen neu und verknüpft die verbleibenden sekundären Datenbanken mit der soeben heraufgestuften primären Datenbank, ohne dass ein Benutzereingriff erforderlich ist. Wenn der Ausfall behoben ist, durch den das Failover verursacht wurde, kann es wünschenswert sein, die Anwendung wieder in die primäre Region zurückzuführen. Zu diesem Zweck sollte der Failoverbefehl mit der Option „Geplant“ aufgerufen werden. 
* **Synchronisieren von Anmeldeinformationen und Firewallregeln:** Wir empfehlen die Verwendung von [Datenbank-Firewallregeln](sql-database-firewall-configure.md) für georeplizierte Datenbanken, damit diese Regeln mit der Datenbank repliziert werden können. So wird sichergestellt, dass alle sekundären Datenbanken die gleichen Firewallregeln besitzen wie die primäre Datenbank. Mit diesem Ansatz müssen Kunden auf Servern, auf denen sowohl die primäre als auch die sekundäre Datenbank gehostet wird, keine Firewallregeln mehr manuell konfigurieren und verwalten. Analog dazu wird durch die Verwendung von [eigenständigen Datenbankbenutzern](sql-database-manage-logins.md) für den Datenzugriff sichergestellt, dass für primäre und sekundäre Datenbanken immer die gleichen Benutzeranmeldeinformationen gelten, damit bei einem Failover keine Unterbrechungen durch Benutzernamen- und Kennwortkonflikte auftreten. Durch Hinzufügen von [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) können Kunden den Benutzerzugriff sowohl für primäre als auch für sekundäre Datenbanken verwalten, sodass die Notwendigkeit der Verwaltung von Anmeldeinformationen in Datenbanken vollständig entfällt.

## <a name="auto-failover-group-capabilities"></a>Funktionen von Gruppen für automatisches Failover

Gruppen für automatisches Failover bieten eine leistungsfähige Abstraktion der aktiven Georeplikation durch Unterstützung der Replikation auf Gruppenebene und des automatischen Failovers. Darüber hinaus entfällt damit die Notwendigkeit, die SQL-Verbindungszeichenfolge nach dem Failover zu ändern, indem die zusätzlichen Listenerendpunkte bereitgestellt werden. 

* **Failovergruppe**: Eine oder mehrere Failovergruppen können zwischen zwei Servern in verschiedenen Regionen erstellt werden (primäre und sekundäre Server). Jede Gruppe kann eine oder mehrere Datenbanken enthalten, die als Einheit wiederhergestellt werden, falls alle oder einige primäre Datenbanken aufgrund eines Ausfalls in der primären Region nicht mehr verfügbar sind.  
* **Primärserver:** ein Server, auf dem die primären Datenbanken in der Failovergruppe gehostet werden.
* **Sekundärserver:** ein Server, auf dem die sekundären Datenbanken in der Failovergruppe gehostet werden. Der Sekundärserver kann sich nicht in der gleichen Region wie der primäre Server befinden.
* **Hinzufügen von Datenbanken zu einer Failovergruppe**: Sie können mehrere Datenbanken in einem Server oder in einem Pool für elastische Datenbanken in die gleiche Failovergruppe einfügen. Wenn Sie der Gruppe eine eigenständige Datenbank hinzufügen, wird automatisch eine sekundäre Datenbank mit der gleichen Edition und Leistungsstufe erstellt. Wenn sich die primäre Datenbank in einem Pool für elastische Datenbanken befindet, wird die sekundäre Datenbank automatisch im Pool für elastische Datenbanken gleichen Namens erstellt. Wenn Sie eine Datenbank hinzufügen, die bereits auf dem sekundären Server eine sekundäre Datenbank hat, erbt die Gruppe diese Georeplikation.

> [!NOTE]
> Wenn Sie eine Datenbank hinzufügen, die bereits eine sekundäre Datenbank auf einem Server hat, der nicht Teil der Failovergruppe ist, wird eine neue sekundäre Datenbank auf dem sekundären Server erstellt. 
>

* **Lese-/Schreib-Failovergruppenlistener:** ein DNS CNAME-Eintrag in der Form **&lt;name-der-failovergruppe&gt;.database.windows.net**, der auf die URL des aktuellen primären Servers verweist. Er ermöglicht Lese-/Schreib-SQL-Anwendungen das transparente erneute Herstellen einer Verbindung mit der primären Datenbank, wenn die primäre Datenbank nach einem Failover geändert wird. 
* **Nur-Lese-Failovergruppenlistener:** ein DNS CNAME-Eintrag in der Form **&lt;name-der-failovergruppe&gt;.secondary.database.windows.net**, der auf die URL des sekundären Servers verweist. Er ermöglicht den schreibgeschützten SQL-Anwendungen das transparente Herstellen einer Verbindung mit der sekundären Datenbank unter Verwendung der angegebenen Regeln für den Lastenausgleich. 
* **Richtlinie für automatisches Failover**: Standardmäßig wird die Failovergruppe mit einer Richtlinie für automatisches Failover konfiguriert. Das System löst das Failover aus, nachdem der Fehler erkannt und die Toleranzperiode abgelaufen ist. Das System muss sicherstellen, dass der Ausfall aufgrund des Ausmaßes der Auswirkungen nicht durch die integrierte Hochverfügbarkeitsinfrastruktur des SQL-Datenbank-Diensts gemildert werden kann. Wenn Sie den Failoverworkflow aus der Anwendung steuern möchten, können Sie automatisches Failover deaktivieren. 
* **Schreibgeschützte Failoverrichtlinie**: Standardmäßig ist das Failover des schreibgeschützten Listeners deaktiviert. Dadurch wird sichergestellt, dass die Leistung der primären Datenbank nicht beeinträchtigt wird, wenn die sekundäre Datenbank offline ist. Es bedeutet jedoch auch, dass die schreibgeschützten Sitzungen erst dann eine Verbindung herstellen können, nachdem die sekundäre Datenbank wiederhergestellt wurde. Wenn Sie keine Ausfallzeiten für die schreibgeschützten Sitzungen tolerieren und die primäre Datenbank vorübergehend sowohl für den schreibgeschützten als auch den Lese-/Schreibdatenverkehr auf Kosten der möglichen Leistungseinbußen der primären Datenbank verwenden möchten, können Sie das Failover für den schreibgeschützten Listener aktivieren. In diesem Fall wird der schreibgeschützte Datenverkehr automatisch zum primären Server umgeleitet, wenn der sekundäre Server nicht verfügbar ist.  
* **Manuelles Failover**: Sie können ein Failover zu einem beliebigen Zeitpunkt unabhängig von der Konfiguration für automatisches Failover manuell initiieren. Wenn die Richtlinie für automatisches Failover nicht konfiguriert ist, müssen die Datenbanken in der Failovergruppe mit manuellem Failover wiederhergestellt werden. Sie können ein erzwungenes oder freundliches Failover (mit vollständiger Datensynchronisierung) initiieren. Mit letzterem könnten aktive Server in die primäre Region verschoben werden. Nach Abschluss des Failovers werden die DNS-Einträge automatisch aktualisiert, um die Konnektivität mit dem richtigen Server sicherzustellen.
* **Toleranzperiode mit Datenverlust**: Da die primären und sekundären Datenbanken mithilfe der asynchronen Replikation synchronisiert werden, kann das Failover zu Datenverlust führen. Sie können die Richtlinie für automatisches Failover entsprechend der Toleranz Ihrer Anwendung gegenüber Datenverlust anpassen. Durch Konfigurieren von **GracePeriodWithDataLossHours** können Sie steuern, wie lange das System wartet, bevor das Failover initiiert wird, das wahrscheinlich zu Datenverlust führt. 

> [!NOTE]
> Wenn das System erkennt, dass die Datenbanken in der Gruppe noch immer online sind (etwa wenn der Ausfall nur die Dienststeuerungsebene betroffen hat), aktiviert es sofort unabhängig von dem durch **GracePeriodWithDataLossHours** festgelegten Wert das Failover mit vollständiger Datensynchronisierung (freundliches Failover). Dadurch wird sichergestellt, dass es während der Wiederherstellung nicht zu Datenverlusten kommt. Die Toleranzperiode ist nur wirksam, wenn kein freundliches Failover möglich ist. Wenn der Ausfall vor Ablauf der Toleranzperiode behoben wird, wird das Failover nicht aktiviert.
>

* **Mehrere Failovergruppen**: Sie können mehrere Failovergruppen für das gleiche Serverpaar konfigurieren, um die Skalierung der Failover zu steuern. Jede Gruppe führt ein unabhängiges Failover durch. Wenn Ihre mehrinstanzenfähige Anwendung Pools für elastische Datenbanken verwendet, können Sie diese Funktion zum Mischen primärer und sekundärer Datenbanken in den einzelnen Pools verwenden. Auf diese Weise können Sie die Auswirkungen eines Ausfalls auf nur die Hälfte der Mandanten reduzieren.

## <a name="best-practices-of-using-failover-groups-for-business-continuity"></a>Bewährte Methoden der Verwendung von Failovergruppen für die Geschäftskontinuität
Beim Entwerfen eines Diensts, der die Geschäftskontinuität aufrechterhalten soll, sollten Sie die folgenden Richtlinien beachten:

- **Verwenden Sie mindestens eine Failovergruppe, um das Failover mehrerer Datenbanken zu verwalten**: Failovergruppen können zwischen zwei Servern in verschiedenen Regionen erstellt werden (primäre und sekundäre Server). Jede Gruppe kann eine oder mehrere Datenbanken enthalten, die als Einheit wiederhergestellt werden, falls alle oder einige primäre Datenbanken aufgrund eines Ausfalls in der primären Region nicht mehr verfügbar sind. Die Failovergruppe erstellt eine geosekundäre Datenbank mit dem gleichen Dienstziel wie die primäre Datenbank. Wenn Sie der Failovergruppe eine vorhandene Georeplikationsbeziehung hinzufügen, stellen Sie sicher, dass die geosekundäre Datenbank mit dem gleichen Servicelevelziel wie die primäre Datenbank konfiguriert ist.
- **Lese-/Schreib-Listener für OLTP-Workload verwenden:** Verwenden Sie beim Durchführen von OLTP-Vorgängen **&lt;name-der-failovergruppe&gt;.database.windows.net** als Server-URL, damit die Verbindungen automatisch an den primären Server weitergeleitet werden. Diese URL wird nach dem Failover nicht geändert. Beachten Sie, dass das Failover die Aktualisierung von DNS-Einträgen einschließt, damit die Clientverbindungen erst dann an die neue primäre Datenbank weitergeleitet werden, wenn der Client-DNS-Cache aktualisiert wurde.
- **Nur-Lese-Listener für schreibgeschützte Workload verwenden:** Wenn Sie über eine logisch isolierte schreibgeschützte Workload verfügen, die gegenüber einer bestimmten Veraltung tolerant ist, können Sie die sekundäre Datenbank in der Anwendung verwenden. Verwenden Sie für schreibgeschützte Sitzungen **&lt;name-der-failovergruppe&gt;.secondary.database.windows.net** als Server-URL, damit die Verbindung automatisch an die sekundäre Datenbank weitergeleitet wird. Es wird außerdem empfohlen, in der Verbindungszeichenfolge die Leseabsicht anzugeben. Verwenden Sie dazu **ApplicationIntent=ReadOnly**. 
- **Vorbereitung auf die Beeinträchtigung der Leistung:** Die SQL-Failoverentscheidung ist unabhängig vom Rest der Anwendung oder der anderen verwendeten Dienste. Die Anwendung kann mit einigen Komponenten in einer Region und anderen in einer anderen „vermischt“ werden. Um die Beeinträchtigung zu vermeiden, stellen Sie redundante Anwendungsbereitstellung in der DR-Region sicher, und befolgen Sie die Richtlinien in diesem Artikel. Beachten Sie, dass die Anwendung in der DR-Region nicht unbedingt in eine andere Verbindungszeichenfolge verwenden muss.  
- **Vorbereiten auf Datenverluste:** Wenn ein Ausfall erkannt wird, löst SQL automatisch ein Lese-/Schreib-Failover aus, sofern nach unseren Erkenntnissen keine Daten verloren gegangen sind. Andernfalls wartet es für den mit **GracePeriodWithDataLossHours** festgelegten Zeitraum. Wenn Sie **GracePeriodWithDataLossHours** angegeben haben, müssen Sie auf Datenverluste vorbereitet sein. Bei Ausfällen priorisiert Azure grundsätzlich die Verfügbarkeit. Wenn keine Datenverluste auftreten dürfen, legen Sie **GracePeriodWithDataLossHours** auf eine ausreichend große Zahl fest, z.B. 24 Stunden. 

> [!IMPORTANT]
> Bei Pools für elastische Datenbanken mit 800 oder weniger DTUs und mehr als 250 Datenbanken, die die Georeplikation verwenden, können Probleme wie längere geplante Failover und Leistungseinbußen auftreten.  Diese Probleme treten häufiger bei schreibintensiven Workloads auf, wenn Georeplikations-Endpunkte geographisch weit auseinander liegen, oder wenn mehrere sekundäre Endpunkte für jede Datenbank verwendet werden.  Ein Anzeichen für derartige Probleme ist eine steigende Verzögerung bei der Georeplikation.  Diese Verzögerung können Sie mit [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) überwachen.  Wenn diese Probleme auftreten, können Sie sie entschärfen, indem Sie z.B. die Anzahl von Pool-DTUs erhöhen oder die Anzahl von Datenbanken für die Georeplikation im gleichen Pool verringern.

## <a name="failover-groups-and-network-security"></a>Failovergruppen und Netzwerksicherheit 

Für einige Anwendungen erfordern die Sicherheitsregeln, dass der Netzwerkzugriff auf die Datenebene auf eine bestimmte Komponente oder Komponenten wie virtueller Computer, Webdienst usw. beschränkt ist. Diese Anforderung stellt für den Entwurf der Geschäftskontinuität und die Verwendung der Failovergruppen eine Herausforderung dar. Sie sollten bei der Implementierung eines solchen eingeschränkten Zugriffs die folgenden Optionen berücksichtigen.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Verwendung von Regeln für Failovergruppen und virtuelle Netzwerke

Wenn Sie [Dienstendpunkte des virtuellen Netzwerks und Regeln](sql-database-vnet-service-endpoint-rule-overview.md) verwenden, um den Zugriff auf Ihre SQL-Datenbank-Instanz einzuschränken, denken Sie daran, dass jeder Dienstendpunkt des virtuellen Netzwerks nur für eine einzige Azure-Region gilt. Der Endpunkt ermöglicht anderen Regionen nicht das Akzeptieren von Nachrichten aus dem Subnetz. Da das Failover dazu führt, dass die SQL-Clientsitzungen zu dem Server in der anderen (sekundären) Region umgeleitet werden, treten bei diesen Sitzungen Fehler auf, wenn sie von Clients ausgehen, die sich außerhalb dieser Region befinden. Aus diesem Grund kann die Richtlinie für automatisches Failover nicht aktiviert werden, wenn die beteiligten Server in den Regeln für virtuelle Netzwerke enthalten sind. Um das manuelle Failover zu unterstützen, gehen Sie folgendermaßen vor:

1.  Stellen Sie die redundanten Kopien der Front-End-Komponenten der Anwendung (Webdienst, VMs usw.) in der sekundären Region bereit.
2.  Konfigurieren Sie die [Regeln für virtuelle Netzwerke](sql-database-vnet-service-endpoint-rule-overview.md) individuell für den primären und sekundären Server.
3.  Aktivieren Sie das [Front-End-Failover durch eine Traffic Manager-Konfiguration](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime).
4.  Initiieren Sie ein manuelles Failover, wenn der Ausfall erkannt wird.

Diese Option ist für die Anwendungen optimiert, die konsistente Latenz zwischen Front-End und Datenebene erfordern, und unterstützt die Wiederherstellung, wenn Front-End, Datenebene oder beide vom Ausfall betroffen sind. 

> [!NOTE]
> Stellen Sie bei Verwendung des **schreibgeschützten Listeners** für den Lastenausgleich einer schreibgeschützten Workload sicher, dass diese Workload auf einem virtuellen Computer oder einer anderen Ressource in der sekundären Region ausgeführt wird, damit sie eine Verbindung mit der sekundären Datenbank herstellen kann.
>

 ### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Verwenden von Failovergruppen und SQL-Datenbank-Firewallregeln

Wenn Ihr Geschäftskontinuitätsplan das Durchführen eines Failovers mithilfe von Gruppen mit automatischem Failover erfordert, können Sie den Zugriff auf Ihre SQL-Datenbank mithilfe der herkömmlichen Firewallregeln einschränken.  Um das automatische Failover zu unterstützen, gehen Sie folgendermaßen vor:

1.  [Erstellen Sie eine öffentliche IP-Adresse.](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address) 
2.  [Erstellen Sie einen öffentlichen Lastenausgleich](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer), und weisen Sie ihm die öffentliche IP-Adresse zu. 
3.  [Erstellen Sie ein virtuelles Netzwerk und die VMs](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) für Ihre Front-End-Komponenten. 
4.  [Erstellen Sie eine Netzwerksicherheitsgruppe](../virtual-network/security-overview.md), und konfigurieren Sie eingehende Verbindungen. 
5. Stellen Sie mithilfe des [Diensttags](../virtual-network/security-overview.md#service-tags) „Sql“ sicher, dass die ausgehenden Verbindungen für Azure SQL-Datenbank geöffnet sind. 
5.  Erstellen Sie eine [SQL-Datenbank-Firewallregel](sql-database-firewall-configure.md), um eingehenden Datenverkehr von der öffentlichen IP-Adresse, die Sie in Schritt 1 erstellt haben, zuzulassen. 

Weitere Informationen zur Konfiguration des ausgehenden Zugriffs und zu der IP-Adresse, die in den Firewallregeln verwendet werden soll, finden Sie unter [Ausgehende Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md).

Die oben genannte Konfiguration sorgt dafür, dass das automatische Failover keine Verbindungen von den Front-End-Komponenten blockiert, und setzt voraus, dass die Anwendung die längeren Wartezeiten zwischen dem Front-End und der Datenebene tolerieren kann.

> [!IMPORTANT]
> Um die Geschäftskontinuität bei regionalen Ausfällen zu gewährleisten, müssen Sie die geografische Redundanz sowohl für die Front-End-Komponenten als auch die Datenbanken sicherstellen. 
>

## <a name="upgrading-or-downgrading-a-primary-database"></a>Upgrade oder Downgrade einer primären Datenbank
Sie können für eine primäre Datenbank ein Upgrade oder Downgrade auf eine andere Leistungsstufe (im gleichen Diensttarif) ausführen, ohne die Verbindung mit sekundären Datenbanken zu trennen. Bei einem Upgrade wird empfohlen, zuerst das Upgrade für die sekundäre Datenbank und anschließend das Upgrade für die primäre Datenbank auszuführen. Drehen Sie bei einem Downgrade die Reihenfolge um: Führen Sie zuerst das Downgrade für die primäre und anschließend das Downgrade für die sekundäre Datenbank aus. Wenn Sie ein Upgrade oder Downgrade der Datenbank auf eine andere Dienstebene durchführen, wird diese Empfehlung erzwungen. 

> [!NOTE]
> Wenn Sie eine sekundäre Datenbank als Teil der Konfiguration der Failovergruppe erstellt haben, sollten Sie kein Downgrade der sekundären Datenbank durchführen. So wird sichergestellt, dass Ihre Datenebene nach dem Aktivieren des Failovers ausreichende Kapazität zum Verarbeiten des normalen Workloads hat. 
>

## <a name="preventing-the-loss-of-critical-data"></a>Verhindern des Verlusts wichtiger Daten
Aufgrund der hohen Latenz von WANs wird für die fortlaufende Kopie ein asynchroner Replikationsmechanismus verwendet. Asynchrone Replikation macht Datenverlust unvermeidlich, sobald ein Ausfall auftritt. Bei einigen Anwendung dürfen jedoch ggf. keine Daten verloren gehen. Um diese kritischen Aktualisierungen zu schützen, kann ein Anwendungsentwickler die Systemprozedur [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) aufrufen, unmittelbar nachdem der Commit für die Transaktion erfolgt ist. Das Aufrufen von **sp_wait_for_database_copy_sync** blockiert den aufrufenden Thread so lange, bis die letzte Transaktion mit erfolgtem Commit in die sekundäre Datenbank übertragen wurde. Es wird jedoch nicht abgewartet, bis die übertragenen Transaktionen wiedergegeben und in der sekundären Datenbank committet werden. **sp_wait_for_database_copy_sync** ist auf eine bestimmte fortlaufende Kopierverknüpfung begrenzt. Jeder Benutzer mit den Rechten zum Herstellen der Verbindung mit der primären Datenbank kann diese Prozedur aufrufen.

> [!NOTE]
> **sp_wait_for_database_copy_sync** verhindert Datenverlust nach einem Failover, garantiert aber nicht die vollständige Synchronisierung für den Lesezugriff. Die vom Aufruf der Prozedur **sp_wait_for_database_copy_sync** verursachte Verzögerung kann signifikant sein und hängt von der Größe des Transaktionsprotokolls zum Zeitpunkt des Aufrufs ab. 
> 

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Programmgesteuertes Verwalten von Failovergruppen und aktive Georeplikation
Wie bereits zuvor erwähnt, können Gruppen für automatisches Failover und aktive Georeplikation auch programmgesteuert mit Azure PowerShell und der REST-API verwaltet werden. Die folgenden Tabellen beschreiben den verfügbaren Satz von Befehlen.

**Azure Resource Manager-API und rollenbasierte Sicherheit:** Die aktive Georeplikation umfasst eine Reihe von Azure Resource Manager-APIs für die Verwaltung. Hierzu zählen unter anderem die [Azure SQL-Datenbank-REST-API](https://docs.microsoft.com/rest/api/sql/) und [Azure PowerShell-Cmdlets](https://docs.microsoft.com/powershell/azure/overview). Diese APIs erfordern die Verwendung von Ressourcengruppen und unterstützen rollenbasierte Sicherheit (RBAC). Weitere Informationen zur Implementierung von Zugriffsrollen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/overview.md).

## <a name="manage-sql-database-failover-using-transact-sql"></a>Verwalten eines Failovers von SQL-Datenbanken mit Transact-SQL

| Get-Help | BESCHREIBUNG |
| --- | --- |
| [ALTER DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/alter-database-azure-sql-database) |Verwenden Sie das Argument ADD SECONDARY ON SERVER, um eine sekundäre Datenbank für eine vorhandene Datenbank zu erstellen und die Datenreplikation zu starten. |
| [ALTER DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/alter-database-azure-sql-database) |Verwenden Sie FAILOVER oder FORCE_FAILOVER_ALLOW_DATA_LOSS, um die sekundäre Datenbank zur primären zu erklären und zu ihr zu wechseln – damit starten Sie das Failover. |
| [ALTER DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/alter-database-azure-sql-database) |Verwenden Sie REMOVE SECONDARY ON SERVER, um die Datenreplikation zwischen einer SQL-Datenbank und der angegebenen sekundären Datenbank zu beenden. |
| [sys.geo_replication_links (Azure SQL-Datenbank)](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Gibt Informationen über alle vorhandenen Replikationsverknüpfungen für alle Datenbanken auf dem logischen Azure SQL-Datenbankserver zurück. |
| [sys.dm_geo_replication_link_status (Azure SQL-Datenbank)](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Ruft den Zeitpunkt der letzten Replikation, die Verzögerung der letzten Replikation und andere Informationen über die Replikationsverknüpfung für eine angegebene SQL-Datenbank ab. |
| [sys.dm_operation_status (Azure SQL-Datenbank)](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Zeigt den Status für alle Datenbankvorgänge an, einschließlich des Status der Replikationsverknüpfungen. |
| [sp_wait_for_database_copy_sync (Azure SQL-Datenbank)](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |Bewirkt, dass die Anwendung wartet, bis alle Transaktionen mit erfolgtem Commit repliziert und von der aktiven sekundären Datenbank bestätigt wurden. |
|  | |

## <a name="manage-sql-database-failover-using-powershell"></a>Verwalten eines Failovers von SQL-Datenbanken mit PowerShell

| Cmdlet | BESCHREIBUNG |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Ruft mindestens eine Datenbank ab. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Erstellt eine sekundäre Datenbank für eine vorhandene Datenbank und startet die Datenreplikation. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Erklärt die sekundäre Datenbank zur primären und wechselt zu ihr – dadurch wird das Failover gestartet. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Beendet die Datenreplikation zwischen einer SQL-Datenbank und der angegebenen sekundären Datenbank. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Ruft die Georeplikationsverknüpfungen zwischen einer Azure SQL-Datenbank und einer Ressourcengruppe oder einer SQL Server-Instanz ab. |
| [New-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Dieser Befehl erstellt eine Failovergruppe und registriert sie auf primären und sekundären Servern.|
| [Remove-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Entfernt die Failovergruppe vom Server und löscht alle in der Gruppe enthaltenen sekundäre Datenbanken. |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Ruft die Konfiguration der Failovergruppe ab. |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Ändert die Konfiguration der Failovergruppe. |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Löst das Failover der Failovergruppe auf den sekundären Server aus. |
|  | |

> [!IMPORTANT]
> Beispielskripts finden Sie unter [Verwenden von PowerShell zum Konfigurieren der aktiven Georeplikation für eine einzelne Azure SQL-Datenbank](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [Verwenden von PowerShell zum Konfigurieren der aktiven Georeplikation für eine in einem Pool enthaltene Azure SQL-Datenbank](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md) sowie unter [Verwenden von PowerShell zum Konfigurieren einer Failovergruppe bei der aktiven Georeplikation für eine einzelne Azure SQL-Datenbank](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

## <a name="manage-sql-database-failover-using-the-rest-api"></a>Verwalten eines Failovers von SQL-Datenbanken mit der REST-API
| API | BESCHREIBUNG |
| --- | --- |
| [Create or Update Database (createMode=Restore)](/rest/api/sql/Databases/CreateOrUpdate) |Erstellt oder aktualisiert eine primäre oder sekundäre Datenbank oder stellt diese wieder her. |
| [Get Create or Update Database Status](/rest/api/sql/Databases/CreateOrUpdate) |Ruft den Status während eines Erstellungsvorgangs ab. |
| [Set Secondary Database as Primary (Planned Failover)](/rest/api/sql/replicationlinks/failover) |Legt fest, welche Replikatdatenbank als primäre Replikatdatenbank verwendet wird, indem ein Failover von der aktuellen primären Replikatdatenbank durchgeführt wird. |
| [Set Secondary Database as Primary (Unplanned Failover)](/rest/api/sql/replicationlinks/failoverallowdataloss) |Legt fest, welche Replikatdatenbank als primäre Replikatdatenbank verwendet wird, indem ein Failover von der aktuellen primären Replikatdatenbank durchgeführt wird. Bei diesem Vorgang können Daten verloren gehen. |
| [Get Replication Link](/rest/api/sql/replicationlinks/get) |Ruft eine spezifische Replikationsverknüpfung für eine angegebene SQL-Datenbank in einer Georeplikationspartnerschaft ab. Es werden die Informationen abgerufen, die in der Katalogsicht „sys.geo_replication_links“ sichtbar sind. |
| [Replication Links - List By Database](/rest/api/sql/replicationlinks/listbydatabase) | Ruft alle Replikationsverknüpfungen für eine angegebene SQL-Datenbank in einer Georeplikationspartnerschaft ab. Es werden die Informationen abgerufen, die in der Katalogsicht „sys.geo_replication_links“ sichtbar sind. |
| [Delete Replication Link](/rest/api/sql/databases/delete) | Löscht einen Datenbankreplikationslink. Kann nicht während eines Failovers verwendet werden. |
| [Create or Update Failover Group](/rest/api/sql/failovergroups/createorupdate) | Erstellt oder aktualisiert eine Failovergruppe. |
| [Delete Failover Group](/rest/api/sql/failovergroups/delete) | Entfernt die Failovergruppe vom Server |
| [Failover (Planned)](/rest/api/sql/failovergroups/failover) | Führt ein Failover vom aktuellen primären Server auf diesen Server durch. |
| [Force Failover Allow Data Loss](/rest/api/sql/failovergroups/forcefailoverallowdataloss) |Führt ein Failover vom aktuellen primären Server auf diesen Server durch. Bei diesem Vorgang können Daten verloren gehen. |
| [Get Failover Group](/rest/api/sql/failovergroups/get) | Ruft eine Failovergruppe ab. |
| [List Failover Groups By Server](/rest/api/sql/failovergroups/listbyserver) | Listet die Failovergruppen eines Servers auf. |
| [Update Failover Group](/rest/api/sql/failovergroups/update) | Aktualisiert eine Failovergruppe. |
|  | |

## <a name="next-steps"></a>Nächste Schritte
* Beispielskripts:
   - [Configure and failover a single database using active geo-replication](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) (Konfiguration und Failover einer einzelnen Datenbank mithilfe von aktiver Georeplikation)
   - [Configure and failover a pooled database using active geo-replication](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md) (Konfiguration und Failover einer gepoolten Datenbank mithilfe von aktiver Georeplikation)
   - [Konfiguration und Failover einer Failovergruppe für eine einzelne Datenbank](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
* Weitere Informationen zu Authentifizierungsanforderungen für einen neuen primären Server und die Datenbank finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).


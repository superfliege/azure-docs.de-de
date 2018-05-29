---
title: Hochverfügbarkeit – Azure SQL-Datenbankdienst | Microsoft Docs
description: Erfahren Sie mehr über die Hochverfügbarkeitsfunktionen des Azure SQL-Datenbankdiensts.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 04/24/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: e541513890d357587e5c1e792165123c2beb5d96
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32777014"
---
# <a name="high-availability-and-azure-sql-database"></a>Hochverfügbarkeit und Azure SQL-Datenbank
Seit der Einführung des PaaS-Angebots von Azure SQL-Datenbank hat Microsoft seinen Kunden zugesichert, Hochverfügbarkeit (High Availability, HA) direkt in den Dienst zu integrieren. So sind auf Kundenseite weder Maßnahmen oder Entscheidungen rund um HA erforderlich, noch muss spezielle Logik implementiert werden. Microsoft bietet Kunden eine Vereinbarung zum Servicelevel (SLA) und behält die vollständige Kontrolle über die Konfiguration und den Betrieb des Hochverfügbarkeitssystems. Die SLA zur Hochverfügbarkeit gilt für eine SQL-Datenbank in einer Region und bietet keinen Schutz vor Totalausfällen einer Region aufgrund von Faktoren, die außerhalb der Kontrolle von Microsoft liegen (z.B. Naturkatastrophen, Kriege, Terroranschläge, Aufstände, staatliche Maßnahmen, Netzwerk- oder Geräteausfälle außerhalb der Rechenzentren von Microsoft, u.a. am Kundenstandort oder zwischen dem Kundenstandort und dem Microsoft-Rechenzentrum).

Zur Vereinfachung des Problemraums von HA geht Microsoft von folgenden Annahmen aus:
1.  Hardware- und Softwarefehler sind unvermeidlich.
2.  Operative Mitarbeiter begehen Fehler, die zu Ausfällen führen.
3.  Planmäßige Wartungsvorgänge verursachen Ausfälle. 

Während Einzelvorfälle selten sind, können sie in einem größeren Maßstab – z. B. auf Cloudebene – wöchentlich und sogar täglich auftreten. 

## <a name="fault-tolerant-sql-databases"></a>Fehlertolerante SQL-Datenbanken
Kunden kommt es mehr auf die Resilienz ihrer eigenen Datenbanken an und nicht auf die Resilienz des SQL-Datenbankdienst als Ganzes. Eine Betriebszeit von 99,99 % für einen Dienst ist bedeutungslos, wenn meine Datenbank zu den 0,01 % gehört, die ausgefallen sind. Jede Datenbank muss fehlertolerant sein, und die Fehlerminderung darf niemals zum Verlust von Transaktionen führen, die bereits committed wurden. 

Für Daten verwendet SQL-Datenbank sowohl lokalen Speicher (LS), der auf direkt zugeordneten Datenträgern/VHDs basiert, als auch Remotespeicher (RS), der auf Azure Storage Premium-Seitenblobs basiert. 
- Lokaler Speicher wird in Datenbanken und Pools für elastische Datenbanken der Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) eingesetzt, die für geschäftskritische OLTP-Anwendungen mit hohen IOPS-Anforderungen ausgelegt sind. 
- Remotespeicher wird für die Dienstebenen „Basic“, „Standard“ und „Universell“ verwendet, die für budgetorientierte Unternehmensworkloads ausgelegt sind, die Speicher- und Computeleistung für die unabhängige Skalierung benötigen. Sie verwenden ein einzelnes Seitenblob für Datenbank- und Protokolldateien sowie die integrierte Speicherreplikation und Failovermechanismen.

Die Replikations-, Fehlererkennungs- und Failovermechanismen von SQL-Datenbank sind in beiden Fällen vollständig automatisiert und funktionieren ohne Benutzereingriffe. Diese Architektur stellt sicher, dass Daten, die bereits committed wurden, niemals verloren gehen und dass die Dauerhaftigkeit von Daten absoluten Vorrang hat.

Hauptvorteile:
- Kunden profitieren von den Vorteilen replizierter Datenbanken, ohne dass sie komplizierte Hardware-, Software-, Betriebssystem- oder Virtualisierungsumgebungen konfigurieren oder verwalten müssen.
- Die ACID-Eigenschaften relationaler Datenbanken werden vollständig vom System verwaltet.
- Failover sind voll automatisiert und verhindern den Verlust von Daten, für die ein Commit ausgeführt wurde.
- Das Verbindungsrouting zum primären Replikat wird dynamisch durch den Dienst verwaltet und kommt ohne Anwendungslogik aus.
- Die hohe Redundanz aufgrund von Automatisierung verursacht keine zusätzlichen Kosten.

> [!NOTE]
> Die beschriebenen Architektur für Hochverfügbarkeit kann ohne vorherige Ankündigung geändert werden. 

## <a name="data-redundancy"></a>Datenredundanz

Die Hochverfügbarkeitslösung in SQL-Datenbank basiert auf [Always On-Verfügbarkeitsgruppen](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)-Technologie von SQL Server und sorgt dafür, dass diese mit minimalen Unterschieden sowohl für LS- als auch für RS-Datenbanken funktioniert. In LS-Konfigurationen soll die Always On-Verfügbarkeitsgruppen-Technologie Persistenz gewährleisten und in RS-Konfigurationen für Verfügbarkeit sorgen (niedrige RTO durch aktive Georeplikation). 

## <a name="local-storage-configuration"></a>Konfiguration mit lokalem Speicher

Bei dieser Konfiguration wird jede Datenbank vom Verwaltungsdienst innerhalb des Steuerrings online geschaltet. Ein primäres Replikat und mindestens zwei sekundäre Replikate (Quorumssatz) befinden sich in einem Mandantenring, der drei unabhängige physische Subsysteme innerhalb desselben Rechenzentrums umfasst. Alle Lese- und Schreibvorgänge werden vom Gateway (GW) an das primäre Replikat gesendet. Die Schreibvorgänge werden asynchron an die sekundären Replikate repliziert. SQL-Datenbank verwendet ein quorumbasiertes Commitschema, bei dem Daten auf das primäre sowie mindestens auf ein sekundäres Replikat geschrieben werden, bevor für die Transaktion ein Commit ausgeführt wird.

Das Failoversystem der [Service Fabric](../service-fabric/service-fabric-overview.md) erstellt Replikate bei Knotenausfällen automatisch neu und sorgt beim Ausfall und beim Beitritt von Knoten zum System dafür, dass der Quorumssatz ausgeglichen bleibt. Planmäßige Wartungen werden sorgfältig koordiniert, um zu verhindern, dass der Quorumssatz unter die minimale Replikatanzahl fällt (in der Regel zwei Replikate). Dieses Modell eignet sich besonders für Datenbanken der Dienstebenen „Premium“ und „Unternehmenskritisch“ (Vorschau), erfordert jedoch Redundanz sowohl der Compute- als auch der Speicherkomponenten und verursacht höhere Kosten.

## <a name="remote-storage-configuration"></a>Remotespeicherkonfiguration

Für Remotespeicherkonfigurationen (Ebenen „Basic“, „Standard“ und „Universell“) wird genau eine Kopie im Remote Blob Storage vorgehalten. Dadurch werden die Funktionen des Speichersystems genutzt, um Dauerhaftigkeit, Redundanz und Bit-Rot-Erkennung zu gewährleisten. 

Die Hochverfügbarkeitsarchitektur wird im folgenden Diagramm veranschaulicht:
 
![Hochverfügbarkeitsarchitektur](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection-and-recovery"></a>Fehlererkennung und Wiederherstellung 
Ein weit verteiltes System benötigt ein sehr zuverlässiges Fehlererkennungssystem, das Fehler zuverlässig, schnell und so kundennah wie möglich ermitteln kann. Bei SQL-Datenbank basiert dieses System auf Azure Service Fabric. 

Das primäre Replikat lässt sofort erkennen, ob und wann das primäre Replikat ausgefallen ist und der Betrieb nicht fortgesetzt werden kann, da alle Lese- und Schreibvorgänge zuerst auf dem primären Replikat stattfinden. Der Prozess, bei dem ein sekundäres Replikat auf den Status eines primären Replikats höher gestuft wird, hat ein RTO (Recovery Time Objective) von 30 Sekunden und ein RPO (Recovery Point Objective) von 0 Sekunden. Um die Auswirkungen des RTOs von 30 Sekunden zu minimieren, empfiehlt es sich, mehrmals erneute Verbindungsversuche mit kürzeren Wartezeiten für fehlgeschlagene Verbindungsversuche auszuführen.

Wenn ein sekundäres Replikat ausfällt, fällt die Datenbank auf einen minimalen Quorumssatz ohne Ersatzreplikate zurück. Service Fabric initiiert die Neukonfiguration auf ähnliche Weise wie den Prozess, der auf einen Ausfall des primären Replikats folgt: Nachdem kurz gewartet und entschieden wurde, ob es sich um einen dauerhaften Fehler handelt, wird ein weiteres sekundäres Replikat erstellt. Bei vorübergehenden Dienstausfällen, z. B. bei einem Betriebssystemfehler oder einem Upgrade, wird nicht sofort ein neues Replikat erstellt, um dem fehlerhaften Knoten stattdessen einen Neustart zu ermöglichen. 

Für Remotespeicherkonfigurationen verwendet SQL-Datenbank Always On-Funktionen, um für Datenbanken während Upgrades ein Failover auszuführen. Dazu wird im Rahmen des planmäßigen Upgrades eine neue SQL-Instanz im Voraus bereitgestellt, um die Datenbankdatei anzufügen und vom Remotespeicher wiederherzustellen. Bei Prozessabstürzen oder anderen ungeplanten Ereignissen wird die Instanzverfügbarkeit von Windows Fabric verwaltet. Dabei wird in einem letzten Wiederherstellungsschritt die Remotedatenbankdatei angefügt.

## <a name="zone-redundant-configuration-preview"></a>Zonenredundante Konfiguration (Vorschau)

Standardmäßig werden die Quorumssatzreplikate für die Konfigurationen mit lokalem Speicher im gleichen Rechenzentrum erstellt. Durch die Einführung von [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) haben Sie die Möglichkeit, die verschiedenen Replikate in den Quorumssätzen in unterschiedlichen Verfügbarkeitszonen in derselben Region zu platzieren. Um einen Single Point of Failure auszuschließen, wird der Steuerring zudem in mehreren Zonen als drei Gatewayringe (GW) kopiert. Die Weiterleitung an einen bestimmten Gatewayring wird durch [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM) gesteuert. Da bei der zonenredundanten Konfiguration keine zusätzliche Datenbankredundanz erzeugt wird, ist die Verwendung von Verfügbarkeitszonen auf den Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) ohne Zusatzkosten verfügbar. Durch die Auswahl einer zonenredundanten Datenbank können Sie Ihre Datenbanken der Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) für deutlich mehr Ausfallszenarien resistent machen – z.B. für schwerwiegende Rechenzentrumsausfälle –, ohne Änderungen an der Anwendungslogik vornehmen zu müssen. Sie können zudem alle vorhandenen Datenbanken oder Pools der Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) in die zonenredundante Konfiguration konvertieren.

Da der zonenredundante Quorumssatz über Replikate in verschiedenen Rechenzentren mit einiger Entfernung dazwischen verfügt, kann sich durch die erhöhte Netzwerklatenz die Commitzeit erhöhen und dadurch die Leistung einiger OLTP-Workloads beeinträchtigt werden. Sie können jederzeit zur Einzelzonenkonfiguration zurückkehren, indem Sie die zonenredundante Einstellung deaktivieren. Dieser Prozess ist datenintensiv und ähnelt dem SLO-Update (Service Level Objective). Am Ende des Prozesses wird die Datenbank oder der Pool aus einem zonenredundanten Ring zum Ring einer einzelnen Zone migriert (oder umgekehrt).

> [!IMPORTANT]
> Zonenredundante Datenbanken und Pools für elastische Datenbanken werden nur auf den Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) unterstützt. In der Public Preview-Phase werden Sicherungen und Überwachungsdatensätze im RA-GRS-Speicher gespeichert und sind daher möglicherweise bei einem Ausfall der gesamten Zone nicht automatisch verfügbar. 

Die zonenredundante Version der Hochverfügbarkeitsarchitektur wird im folgenden Diagramm veranschaulicht:
 
![Hochverfügbarkeitsarchitektur, zonenredundant](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Horizontale Leseskalierung
Wie bereits beschrieben, verwenden die Dienstebenen „Premium“ oder „Unternehmenskritisch“ (Vorschau) Quorumssätze und Always On-Technologie für Hochverfügbarkeit sowohl in Einzelzonenkonfigurationen als auch in zonenredundanten Konfigurationen. Einer der Vorteile der Always On-Technologie besteht darin, dass sich die Replikate jederzeit in einem transaktionssicheren Zustand befinden. Da die Replikate die gleiche Leistungsstufe aufweisen wie das primäre Replikat, kann die Anwendung kostenlos von dieser zusätzlichen Kapazität für die schreibgeschützten Workloads profitieren (horizontale Leseskalierung). Auf diese Weise werden die schreibgeschützten Abfragen von der Hauptworkload für Lesen und Schreiben isoliert und beeinträchtigen deren Leistung nicht. Das Feature der horizontalen Leseskalierung ist für Anwendungen konzipiert, die logisch getrennte schreibgeschützte Workloads z.B. zur Analyse umfassen und daher von dieser zusätzlichen Kapazität profitieren können, ohne eine Verbindung mit dem primären Replikat herstellen zu müssen. 

Um die horizontale Leseskalierung mit einer bestimmten Datenbank zu verwenden, müssen Sie das Feature explizit beim Erstellen der Datenbank aktivieren. Sie können es auch später aktivieren, indem Sie die Konfiguration ändern. Hierzu rufen Sie in PowerShell die Cmdlets [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) oder [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) auf oder verwenden die Methode [Datenbanken – Erstellen oder Aktualisieren](/rest/api/sql/databases/createorupdate) in der Azure Resource Manager-REST-API.

Nachdem die horizontale Leseskalierung für eine Datenbank aktiviert wurde, werden Anwendungen, die eine Verbindung mit dieser Datenbank herstellen, entweder an das Replikat mit Lese-/Schreibzugriff oder an ein schreibgeschütztes Replikat dieser Datenbank weitergeleitet. Die Weiterleitung richtet sich nach der `ApplicationIntent`-Eigenschaft, die in der Verbindungszeichenfolge der Anwendung konfiguriert ist. Informationen zur `ApplicationIntent`-Eigenschaft finden Sie unter [Angeben der Anwendungsabsicht](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Wenn horizontale Leseskalierung deaktiviert ist, oder Sie die ReadScale-Eigenschaft in einer nicht unterstützten Dienstebene festlegen, werden alle Verbindungen unabhängig von der `ApplicationIntent`-Eigenschaft an das Replikat mit Lese-/Schreibzugriff weitergeleitet.  

> [!NOTE]
> Es ist möglich, horizontale Leseskalierung auf einer „Standard“- oder „Universell“-Datenbank zu aktivieren, obwohl dadurch nicht die schreibgeschützte beabsichtigte Sitzung zu einem separaten Replikat geleitet wird. Dies soll vorhandene Anwendungen unterstützen, die zwischen den Ebenen Standard/Universell und Premium/Unternehmenskritisch nach oben oder unten skaliert werden.  

Das Feature der horizontalen Leseskalierung unterstützt Konsistenz auf Sitzungsebene. Wenn die schreibgeschützte Sitzung nach einem Verbindungsfehler, der durch die Nichtverfügbarkeit eines Replikats verursacht wurde, die Verbindung wiederherstellt, wird sie möglicherweise an ein anderes Replikat weitergeleitet. Obwohl dieser Fall unwahrscheinlich ist, kann dies dazu führen, dass ein veraltetes Dataset verarbeitet wird. Gleiches gilt in diesem Fall: Wenn eine Anwendung Daten mithilfe einer Sitzung mit Lese-/Schreibzugriff schreibt und sie mithilfe einer schreibgeschützten Sitzung sofort liest, ist es möglich, dass die neuen Daten nicht sofort sichtbar sind.

## <a name="conclusion"></a>Zusammenfassung
Azure SQL-Datenbank ist nahtlos in die Azure-Plattform integriert und bei der Fehlererkennung und Wiederherstellung in hohem Maße von Service Fabric, in Verbindung mit dem Datenschutz von Azure Storage Blobs und für höhere Fehlertoleranz von Verfügbarkeitszonen abhängig. Gleichzeitig nutzt Azure SQL-Datenbank in vollem Umfang die Always On-Technologie des SQL Server-Standardprodukts für Replikation und Failover. Dank der Kombination dieser Technologien können Anwendungen die Vorteile eines gemischten Speichermodells voll ausschöpfen und sehr anspruchsvolle SLAs unterstützen. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md)
- Weitere Informationen zu [Service Fabric](../service-fabric/service-fabric-overview.md)
- Weitere Informationen zu [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 

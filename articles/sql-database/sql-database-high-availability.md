---
title: Hochverfügbarkeit – Azure SQL-Datenbankdienst | Microsoft Docs
description: Erfahren Sie mehr über die Hochverfügbarkeitsfunktionen des Azure SQL-Datenbankdiensts.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 03/16/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 8deb78ba108aafc3297e6b96d6d88d0c56c60afd
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
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
- Lokaler Speicher wird in Premium-Datenbanken und -Pools eingesetzt, die für geschäftskritische OLTP-Anwendungen mit hohen IOPS-Anforderungen ausgelegt sind. 
- Remotespeicher wird für die Dienstebenen „Basic“ und „Standard“ verwendet, die für budgetorientierte Unternehmensworkloads ausgelegt sind, die Speicher- und Computeleistung für die unabhängige Skalierung benötigen. Sie verwenden ein einzelnes Seitenblob für Datenbank- und Protokolldateien sowie die integrierte Speicherreplikation und Failovermechanismen.

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

Die Hochverfügbarkeitslösung in SQL-Datenbank basiert auf [Always On](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server)-Technologie von SQL Server und sorgt dafür, dass diese mit minimalen Unterschieden sowohl für LS- als auch für RS-Datenbanken funktioniert. In LS-Konfigurationen soll Always On Persistenz gewährleisten und in RS-Konfigurationen für Verfügbarkeit sorgen (niedrige RTO). 

## <a name="local-storage-configuration"></a>Konfiguration mit lokalem Speicher

Bei dieser Konfiguration wird jede Datenbank vom Verwaltungsdienst innerhalb des Steuerrings online geschaltet. Ein primäres Replikat und mindestens zwei sekundäre Replikate (Quorumssatz) befinden sich in einem Mandantenring, der drei unabhängige physische Subsysteme innerhalb desselben Rechenzentrums umfasst. Alle Lese- und Schreibvorgänge werden vom Gateway (GW) an das primäre Replikat gesendet. Die Schreibvorgänge werden asynchron an die sekundären Replikate repliziert. SQL-Datenbank verwendet ein quorumbasiertes Commitschema, bei dem Daten auf das primäre sowie mindestens auf ein sekundäres Replikat geschrieben werden, bevor für die Transaktion ein Commit ausgeführt wird.

Das Failoversystem der [Service Fabric](../service-fabric/service-fabric-overview.md) erstellt Replikate bei Knotenausfällen automatisch neu und sorgt beim Ausfall und beim Beitritt von Knoten zum System dafür, dass der Quorumssatz ausgeglichen bleibt. Planmäßige Wartungen werden sorgfältig koordiniert, um zu verhindern, dass der Quorumssatz unter die minimale Replikatanzahl fällt (in der Regel zwei Replikate). Dieses Modell eignet sich besonders für Premium-Datenbanken, erfordert jedoch, dass die Compute- und Speicherkomponenten redundant sind, und verursacht höhere Kosten.

## <a name="remote-storage-configuration"></a>Remotespeicherkonfiguration

Für Remotespeicherkonfigurationen (Ebenen „Basic“ und „Standard“) wird genau eine Kopie im Remote Blob Storage vorgehalten. Dadurch werden die Funktionen des Speichersystems genutzt, um Dauerhaftigkeit, Redundanz und Bit-Rot-Erkennung zu gewährleisten. 

Die Hochverfügbarkeitsarchitektur wird im folgenden Diagramm veranschaulicht:
 
![Hochverfügbarkeitsarchitektur](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection-and-recovery"></a>Fehlererkennung und Wiederherstellung 
Ein weit verteiltes System benötigt ein sehr zuverlässiges Fehlererkennungssystem, das Fehler zuverlässig, schnell und so kundennah wie möglich ermitteln kann. Bei SQL-Datenbank basiert dieses System auf Azure Service Fabric. 

Das primäre Replikat lässt sofort erkennen, ob und wann das primäre Replikat ausgefallen ist und der Betrieb nicht fortgesetzt werden kann, da alle Lese- und Schreibvorgänge zuerst auf dem primären Replikat stattfinden. Der Prozess, bei dem ein sekundäres Replikat auf den Status eines primären Replikats höher gestuft wird, hat ein RTO (Recovery Time Objective) von 30 Sekunden und ein RPO (Recovery Point Objective) von 0 Sekunden. Um die Auswirkungen des RTOs von 30 Sekunden zu minimieren, empfiehlt es sich, mehrmals erneute Verbindungsversuche mit kürzeren Wartezeiten für fehlgeschlagene Verbindungsversuche auszuführen.

Wenn ein sekundäres Replikat ausfällt, fällt die Datenbank auf einen minimalen Quorumssatz ohne Ersatzreplikate zurück. Service Fabric initiiert die Neukonfiguration auf ähnliche Weise wie den Prozess, der auf einen Ausfall des primären Replikats folgt: Nachdem kurz gewartet und entschieden wurde, ob es sich um einen dauerhaften Fehler handelt, wird ein weiteres sekundäres Replikat erstellt. Bei vorübergehenden Dienstausfällen, z. B. bei einem Betriebssystemfehler oder einem Upgrade, wird nicht sofort ein neues Replikat erstellt, um dem fehlerhaften Knoten stattdessen einen Neustart zu ermöglichen. 

Für Remotespeicherkonfigurationen verwendet SQL-Datenbank Always On-Funktionen, um für Datenbanken während Upgrades ein Failover auszuführen. Dazu wird im Rahmen des planmäßigen Upgrades eine neue SQL-Instanz im Voraus bereitgestellt, um die Datenbankdatei anzufügen und vom Remotespeicher wiederherzustellen. Bei Prozessabstürzen oder anderen ungeplanten Ereignissen wird die Instanzverfügbarkeit von Windows Fabric verwaltet. Dabei wird in einem letzten Wiederherstellungsschritt die Remotedatenbankdatei angefügt.

## <a name="zone-redundant-configuration-preview"></a>Zonenredundante Konfiguration (Vorschau)

Standardmäßig werden die Quorumssatzreplikate für die Konfigurationen mit lokalem Speicher im gleichen Rechenzentrum erstellt. Durch die Einführung von [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) haben Sie die Möglichkeit, die verschiedenen Replikate in den Quorumssätzen in unterschiedlichen Verfügbarkeitszonen in derselben Region zu platzieren. Um einen Single Point of Failure auszuschließen, wird der Steuerring zudem in mehreren Zonen als drei Gatewayringe (GW) kopiert. Die Weiterleitung an einen bestimmten Gatewayring wird durch [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM) gesteuert. Da bei der zonenredundanten Konfiguration keine zusätzliche Datenbankredundanz generiert wird, ist die Verwendung von Verfügbarkeitszonen auf der Dienstebene „Premium“ ohne Zusatzkosten verfügbar. Durch die Auswahl einer zonenredundanten Datenbank können Sie Ihre Premium-Datenbanken für deutlich mehr Ausfallszenarien resistent machen, z.B. für schwerwiegende Rechenzentrumsausfälle, ohne Änderungen an der Anwendungslogik vorzunehmen. Sie können zudem alle vorhandenen Premium-Datenbanken oder -Pools in die zonenredundante Konfiguration konvertieren.

Da der zonenredundante Quorumssatz über Replikate in verschiedenen Rechenzentren mit einiger Entfernung dazwischen verfügt, kann sich durch die erhöhte Netzwerklatenz die Commitzeit erhöhen und dadurch die Leistung einiger OLTP-Workloads beeinträchtigt werden. Sie können jederzeit zur Einzelzonenkonfiguration zurückkehren, indem Sie die zonenredundante Einstellung deaktivieren. Dieser Prozess ist datenintensiv und ähnelt dem SLO-Update (Service Level Objective). Am Ende des Prozesses wird die Datenbank oder der Pool aus einem zonenredundanten Ring zum Ring einer einzelnen Zone migriert (oder umgekehrt).

> [!IMPORTANT]
> Zonenredundante Datenbanken und Pools für elastische Datenbanken werden nur auf der Dienstebene „Premium“ unterstützt. In der Public Preview-Phase werden Sicherungen und Überwachungsdatensätze im RA-GRS-Speicher gespeichert und sind daher möglicherweise bei einem Ausfall der gesamten Zone nicht automatisch verfügbar. 

Die zonenredundante Version der Hochverfügbarkeitsarchitektur wird im folgenden Diagramm veranschaulicht:
 
![Hochverfügbarkeitsarchitektur, zonenredundant](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="conclusion"></a>Zusammenfassung
Azure SQL-Datenbank ist nahtlos in die Azure-Plattform integriert und bei der Fehlererkennung und Wiederherstellung in hohem Maße von Service Fabric, in Verbindung mit dem Datenschutz von Azure Storage Blobs und für höhere Fehlertoleranz von Verfügbarkeitszonen abhängig. Gleichzeitig nutzt Azure SQL-Datenbank in vollem Umfang die Always On-Technologie des SQL Server-Standardprodukts für Replikation und Failover. Dank der Kombination dieser Technologien können Anwendungen die Vorteile eines gemischten Speichermodells voll ausschöpfen und sehr anspruchsvolle SLAs unterstützen. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md)
- Weitere Informationen zu [Service Fabric](../service-fabric/service-fabric-overview.md)
- Weitere Informationen zu [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 

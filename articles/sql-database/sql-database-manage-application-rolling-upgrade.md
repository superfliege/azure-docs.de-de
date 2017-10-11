---
title: Parallelen Anwendungsupgrades - Azure SQL-Datenbank | Microsoft Docs
description: "Erfahren Sie, wie Azure SQL Database Geo-Replikation verwenden, um online-Upgrades Ihrer Cloud-Anwendung zu unterstützen."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 58f42859-1e37-463c-a3d8-a3ca2e867148
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/16/2016
ms.author: sashan
ms.openlocfilehash: 4b59c8aa3dea3e8fba692ab66420295a09210d3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Verwalten des parallelen Upgrades von Cloudanwendungen mit SQL-Datenbank aktive georeplikation
> [!NOTE]
> [Aktive geografische Replikation](sql-database-geo-replication-overview.md) ist jetzt für alle Datenbanken in allen Ebenen verfügbar.
> 

Informationen zur Verwendung [georeplikation](sql-database-geo-replication-overview.md) in SQL-Datenbank, um parallele Upgrades Ihrer Cloud-Anwendung zu aktivieren. Da eine unterbrechungsfreie Upgrades ist, muss es Teil der Planung der Geschäftskontinuität und Entwurf. In diesem Artikel stellen wir sehen uns zwei unterschiedliche Methoden im Rahmen des Upgradeprozesses orchestriert, und erläutern die vor- und Nachteile der einzelnen Optionen. Im Rahmen dieses Artikels verwenden wir eine einfache Anwendung, die von einer Website mit einer einzelnen Datenbank als ihre Datenebene verbunden besteht. Unser Ziel ist es zum Aktualisieren von Version 1 der Anwendung auf Version 2 ohne erhebliche Auswirkungen auf die endbenutzererfahrung. 

Wenn die Upgradeoptionen für die Auswertung sollten Sie die folgenden Faktoren:

* Auswirkungen auf die Verfügbarkeit der Anwendung während der Upgrades. Wie lange die Anwendungsfunktion eingeschränkt oder beeinträchtigt werden kann.
* Möglichkeit zum Rollback bei einem Upgrade ein Fehler auftritt.
* Eine Schwachstelle der Anwendung, wenn ein schwerwiegender Fehler während des Upgrades auftritt.
* Insgesamt Kosten.  Dies schließt zusätzliche Redundanz und Mehrkosten temporäre Komponenten, die durch den Aktualisierungsvorgang verwendet. 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Aktualisieren von Anwendungen, die auf datenbanksicherungen für die Wiederherstellung im Notfall basieren
Wenn die Anwendung auf automatische datenbanksicherungen basiert und geowiederherstellung Wiederherstellung im Notfall verwendet, wird es in der Regel auf einer einzelnen Azure-Region bereitgestellt. In diesem Fall werden im Rahmen des Upgradeprozesses eine Sicherung Bereitstellung aller Komponenten der Anwendung der Aktualisierung beteiligt. Sie werden mit einem Profil für das Failover Azure Traffic Manager (WATM) nutzen, um die Unterbrechung durch Endbenutzer zu minimieren.  Das folgende Diagramm veranschaulicht die betriebssystemumgebung, die vor dem Upgrade zu. Der Endpunkt <i>Contoso 1.azurewebsites.net</i> stellt eine produktionsslot an die Anwendung, die aktualisiert werden muss. Damit die Möglichkeit, ein Rollback müssen das Upgrade Sie einen Stufe-Slot durch eine vollständig synchronisierte Kopie der Anwendung erstellen. Die folgenden Schritte sind erforderlich, um die Anwendung für das Upgrade vorzubereiten:

1. Erstellen Sie einen Stufe Slot für das Upgrade an. Um führen, die eine sekundäre Datenbank (1) erstellen und Bereitstellen einer identischen Website in derselben Azure-Region. Überwachen Sie die sekundäre Datenbank, um festzustellen, ob das seeding abgeschlossen ist.
2. Erstellen Sie ein Failover-Profil in WATM mit <i>Contoso 1.azurewebsites.net</i> als onlineendpunkt und <i>Contoso 2.azurewebsites.net</i> als offline. 

> [!NOTE]
> Beachten Sie die Schritte zur Vorbereitung wirkt sich nicht auf die Anwendung im produktionsslot und ordnungsgemäß im vollständigen Zugriffsmodus.
>  

![Konfiguration des SQL-Go-Datenbankreplikation. Cloud-Wiederherstellung im Notfall.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

Werden bei Abschluss der Schritte zur Vorbereitung ist die Anwendung für die tatsächlichen Aktualisierung bereit. Das folgende Diagramm veranschaulicht die Schritte im Rahmen Upgradeprozesses. 

1. Legen Sie die primäre Datenbank in den produktionsslot zu nur-Lese-Modus (3). Dadurch wird sichergestellt, dass die Produktionsinstanz von der Anwendung (V1) schreibgeschützt sind und während des Upgrades daher verhindert, dass die datenabweichungen zwischen V1 und V2-Datenbankinstanzen bleibt.  
2. Trennen Sie die sekundäre Datenbank mit dem Modus für geplante Beendigung (4). Es wird eine vollständige Synchronisierung unabhängige Kopie der primären Datenbank erstellt. Diese Datenbank wird aktualisiert.
3. Aktivieren der primären Datenbank mit Lese-/ Schreibmodus, und führen Sie das Aktualisierungsskript im Slot Phase (5).     

![Konfiguration des SQL-Datenbank Geo-Replikation. Cloud-Wiederherstellung im Notfall.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Wenn das Upgrade erfolgreich abgeschlossen, können Sie jetzt die Endbenutzer die bereitgestellten Kopie der Anwendung zu wechseln. Es werden jetzt produktionsslot an die Anwendung.  Führen Sie dazu einige zusätzliche Schritte wie im folgenden Diagramm dargestellt.

1. Wechseln Sie den onlineendpunkt in der WATM-Profil zu <i>Contoso 2.azurewebsites.net</i>, welche Punkte die V2-Version der Website (6). Es wird nun im produktionsslot mit der V2-Anwendung und der Endbenutzer-Datenverkehr an sie gesendet wird.  
2. Wenn Sie die V1-Anwendungskomponenten nicht mehr benötigen, damit Sie ohne Bedenken können entfernen Sie (7).   

![Konfiguration des SQL-Datenbank Geo-Replikation. Cloud-Wiederherstellung im Notfall.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Wenn im Rahmen des Upgradeprozesses nicht erfolgreich ist, z. B. aufgrund eines Fehlers in das Aktualisierungsskript die Phase Slot angesehen wird, gefährdet. Um ein Rollback für die Anwendung in den Zustand vor dem upgrade stellen Sie einfach die Anwendung in den produktionsslot zu übergeben, die uneingeschränkten Zugriff. Die Schritte werden in die nächste Abbildung angezeigt.    

1. Legen Sie die Datenbankkopie auf Lese-/ Schreibmodus (8). Dadurch wird der vollständige V1 funktional im produktionsslot wiederhergestellt.
2. Führen Sie die Fehlerursachenanalyse aus, und entfernen Sie die betroffenen Komponenten in der Phase-Slot (9). 

An diesem Punkt wird die Anwendung voll funktionsfähig ist, und die Upgradeschritte können wiederholt werden.

> [!NOTE]
> Das Rollback erfordert keine Änderungen in WATM-Profil aus, da es bereits verweist <i>Contoso 1.azurewebsites.net</i> als aktiven Endpunkt.
> 
> 

![Konfiguration des SQL-Datenbank Geo-Replikation. Cloud-Wiederherstellung im Notfall.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

Der Schlüssel **Vorteil** dieser Option wird die Aktualisierung einer Anwendung in einer einzelnen Region mit einer Reihe von einfachen Schritten. Die Kosten des Upgrades ist relativ niedrig. Die Main **Kompromiss** darin, dass wenn ein schwerwiegender Fehler, während des Upgrades auftritt die Wiederherstellung in den Zustand vor dem upgrade erneute Bereitstellung der Anwendung in einer anderen Region und Wiederherstellung der Datenbank aus einer Sicherung mithilfe der geowiederherstellung umfassen soll. Dieser Vorgang führt zu erheblichen Ausfallzeiten.   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Aktualisieren von Anwendungen, die geografische Replikation der Datenbank für die Wiederherstellung im Notfall benötigen
Wenn Ihre Anwendung die geografische Replikation für Geschäftskontinuität nutzt, wird sie für mindestens zwei verschiedenen Regionen mit einer aktiven Bereitstellung in der primären Region und ein standby-Bereitstellung in Sicherung Region bereitgestellt. Zusätzlich zu den Faktoren, die weiter oben erwähnt muss im Rahmen des Upgradeprozesses garantiert, dass:

* Die Anwendung bleibt jederzeit während des Upgrades von schwerwiegenden Fehlern geschützt
* Die geografisch redundante Komponenten der Anwendung werden parallel mit den aktiven Komponenten aktualisiert.

Azure Traffic Manager (WATM) mithilfe des Failovercluster-Profils mit ein aktives und drei sicherungsendpunkte wird nutzen, um diese Ziele zu erreichen.  Das folgende Diagramm veranschaulicht die betriebssystemumgebung, die vor dem Upgrade zu. Die Websites <i>Contoso 1.azurewebsites.net</i> und <i>Contoso dr.azurewebsites.net</i> eine produktionsslot an die Anwendung mit vollständige geografische Redundanz darstellen. Damit die Möglichkeit, ein Rollback müssen das Upgrade Sie einen Stufe-Slot durch eine vollständig synchronisierte Kopie der Anwendung erstellen. Müssen, stellen Sie sicher, dass die Anwendung schnell wiederhergestellt werden kann, für den Fall, dass ein schwerwiegender Fehler während des Upgrades auftritt, muss das Einschubfach Phase auch Geo-redundant sein. Die folgenden Schritte sind erforderlich, um die Anwendung für das Upgrade vorzubereiten:

1. Erstellen Sie einen Stufe Slot für das Upgrade an. Um führen, die eine sekundäre Datenbank (1) erstellen und eine identische Kopie der Website in derselben Azure-Region bereitstellen. Überwachen Sie die sekundäre Datenbank, um festzustellen, ob das seeding abgeschlossen ist.
2. Erstellen Sie eine geografisch redundante sekundäre Datenbank im Slot Phase durch Geo-Replikation die sekundäre Datenbank für die Sicherung Region (Dies wird "verkettet geografische Replikation" bezeichnet). Überwachen Sie die sekundäre Sicherung, um festzustellen, ob der Seedingvorgang abgeschlossenen (3) ist.
3. Erstellen Sie eine standby-Kopie der Website in der backup-Region aus, und verknüpfen Sie es mit der geografisch redundante sekundäre (4).  
4. Fügen Sie zusätzliche Endpunkte <i>Contoso 2.azurewebsites.net</i> und <i>Contoso 3.azurewebsites.net</i> der Failover-Profil in WATM als offline-Endpunkte (5). 

> [!NOTE]
> Beachten Sie die Schritte zur Vorbereitung wirkt sich nicht auf die Anwendung im produktionsslot und ordnungsgemäß im vollständigen Zugriffsmodus.
> 
> 

![Konfiguration des SQL-Datenbank Geo-Replikation. Cloud-Wiederherstellung im Notfall.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Sobald der Schritte zur Vorbereitung abgeschlossen wurden, ist die Phase für die Aktualisierung bereit. Das folgende Diagramm veranschaulicht die Upgradeschritte.

1. Legen Sie die primäre Datenbank in den produktionsslot zu nur-Lese-Modus (6). Dadurch wird sichergestellt, dass die Produktionsinstanz von der Anwendung (V1) schreibgeschützt sind und während des Upgrades daher verhindert, dass die datenabweichungen zwischen V1 und V2-Datenbankinstanzen bleibt.  
2. Trennen Sie die sekundäre Datenbank in der gleichen Region mit dem Modus für geplante Beendigung (7). Es wird eine vollständige Synchronisierung unabhängige Kopie der primären Datenbank erstellt, der automatisch einen primären nach dem Beenden werden soll. Diese Datenbank wird aktualisiert.
3. Aktivieren Sie in der Phase Slot Lese-/ Schreibmodus der primären Datenbank, und führen Sie das Upgrade-Skript (8).    

![Konfiguration des SQL-Datenbank Geo-Replikation. Cloud-Wiederherstellung im Notfall.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Wenn das Upgrade wurde erfolgreich abgeschlossen. Sie sind jetzt bereit für die um die Endbenutzer auf die V2-Version der Anwendung zu wechseln. Im folgende Diagramm werden die Schritte veranschaulicht.

1. Wechseln Sie den aktiven Endpunkt in der WATM-Profil zu <i>Contoso 2.azurewebsites.net</i>, die nun verweist auf die V2-Version von der Website (9). Es wird nun eine produktionsslot mit der V2-Anwendung, und Endbenutzer-Datenverkehr an sie gesendet wird. 
2. Wenn Sie die V1-Anwendung nicht mehr benötigen, damit Sie ohne Bedenken können entfernen Sie (10 und 11).  

![Konfiguration des SQL-Datenbank Geo-Replikation. Cloud-Wiederherstellung im Notfall.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Wenn im Rahmen des Upgradeprozesses nicht erfolgreich ist, z. B. aufgrund eines Fehlers in das Aktualisierungsskript die Phase Slot angesehen wird, gefährdet. Um ein Rollback für die Anwendung in den Zustand vor dem upgrade stellen Sie einfach mithilfe der Anwendung im produktionsslot mit Vollzugriff. Die Schritte werden in die nächste Abbildung angezeigt.    

1. Legen Sie die Kopie der primären Datenbank im produktionsslot auf Lese-/ Schreibmodus (12). Dadurch wird der vollständige V1 funktional im produktionsslot wiederhergestellt.
2. Führen Sie die Fehlerursachenanalyse aus, und entfernen Sie die betroffenen Komponenten in der Phase-Slot (13 und 14). 

An diesem Punkt wird die Anwendung voll funktionsfähig ist, und die Upgradeschritte können wiederholt werden.

> [!NOTE]
> Das Rollback erfordert keine Änderungen in WATM-Profil aus, da es bereits verweist <i>Contoso 1.azurewebsites.net</i> als aktiven Endpunkt.
> 
> 

![Konfiguration des SQL-Datenbank Geo-Replikation. Cloud-Wiederherstellung im Notfall.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

Der Schlüssel **Vorteil** für diese Option ist, dass Sie die Anwendung und geografisch redundante Clientkopie parallel durchführen können, ohne Beeinträchtigung der Geschäftskontinuität während des Upgrades. Die Main **Kompromiss** ist, dass doppelte Redundanz der einzelnen Komponenten der Anwendung und daher höhere Dollar Kosten fallen. Es beinhaltet auch einen etwas komplizierteren Workflow. 

## <a name="summary"></a>Zusammenfassung
Upgrade im Artikel beschriebenen Methoden unterscheiden sich hinsichtlich der Komplexität und die Kosten Dollar aber beide darauf konzentrieren, minimiert die Zeit, wenn der Benutzer auf schreibgeschützte Vorgänge beschränkt ist. Diese Zeit wird direkt durch die Dauer des Upgradeskript definiert. Es ist nicht abhängig, auf die Größe der Datenbank, die Dienstebene, die Sie ausgewählt haben, die Website-Konfiguration und andere Faktoren, die Sie nicht einfach steuern können. Dies ist, da die Vorbereitungsschritte über die Upgradeschritte entkoppelt sind und ohne Beeinträchtigung der produktionsanwendung erfolgen können. Die Effizienz des Upgradeskript ist der wichtigste Faktor, der für den Endbenutzer während der Upgrades bestimmt. Daher ist die beste Möglichkeit verbessert werden kann durch die Konzentration Ihrer Arbeit darauf, auf das Aktualisierungsskript so effizient wie möglich zu machen.  

## <a name="next-steps"></a>Nächste Schritte
* Eine Business Continuity – Überblick und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md).
* Finden Sie Informationen zu Azure SQL-Datenbank automatisierte Sicherungen [SQL-Datenbank automatisierte Sicherungen](sql-database-automated-backups.md).
* Zur Verwendung der automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus automatisierten Sicherungen](sql-database-recovery-using-backups.md).
* Weitere Informationen zu schneller Wiederherstellungsoptionen finden Sie unter [aktive geografische Replikation](sql-database-geo-replication-overview.md).



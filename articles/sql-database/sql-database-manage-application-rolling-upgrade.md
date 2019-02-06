---
title: Parallele Anwendungsupgrades – Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Georeplikation von Azure SQL-Datenbank verwenden, um Onlineupgrades Ihrer Cloudanwendung zu unterstützen.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 01/29/2019
ms.openlocfilehash: 50f6f114a4d90f48218f751e1649e8694e664491
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55295745"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Verwalten von parallelen Upgrades von Cloudanwendungen mithilfe der aktiven Georeplikation von SQL-Datenbank

Erfahren Sie, wie Sie die [aktive Georeplikation](sql-database-auto-failover-group.md) in SQL-Datenbank verwenden, um parallele Upgrades Ihrer Cloudanwendung zu aktivieren. Da ein Upgrade einen unterbrechenden Vorgang darstellt, sollte es Teil der Planung und des Designs Ihrer Geschäftskontinuität sein. In diesem Artikel betrachten wir zwei unterschiedliche Methoden zum Orchestrieren des Upgradevorgangs und erörtern die Vor- und Nachteile jeder Option. Für die Zwecke dieses Artikels verwenden wir eine Anwendung, die aus einer mit einer Einzeldatenbank als Datenebene verbundenen Website besteht. Unser Ziel ist es, die Version 1 der Anwendung auf die Version 2 zu aktualisieren, ohne dass sich dies deutlich auf die Endbenutzererfahrung auswirkt.

Beim Evaluieren der Upgradeoptionen sollten Sie die folgenden Faktoren berücksichtigen:

* Die Auswirkung auf die Verfügbarkeit der Anwendung während Upgrades. Wie lange die Anwendungsfunktion möglicherweise eingeschränkt oder beeinträchtigt ist.
* Möglichkeit zum Rollback, falls das Upgrade ein Fehler auftritt.
* Das Sicherheitsrisiko für die Anwendung, wenn während des Upgrades ein schwerwiegender Fehler auftritt, der nicht in Zusammenhang mit dem Upgrade steht.
* Die insgesamt anfallenden Kosten.  Dies schließt zusätzliche Redundanz und Mehrkosten der vom Upgradevorgang verwendeten temporären Komponenten ein.

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Upgraden von Anwendungen, die sich auf Datenbanksicherungen für die Notfallwiederherstellung verlassen

Wenn sich Ihre Anwendung auf automatische Datenbanksicherungen verlässt und die Geowiederherstellung zur Notfallwiederherstellung verwendet, wird sie in einer einzelnen Azure-Region bereitgestellt. Um die Unterbrechung für den Endbenutzers zu minimieren, erstellen Sie in dieser Region eine Stagingumgebung mit allen am Upgrade beteiligten Anwendungskomponenten. Das folgende Diagramm veranschaulicht die Betriebsumgebung vor dem Upgradevorgang. Der Endpunkt `contoso.azurewebsites.net` stellt einen Produktionsslot der Webanwendung dar. Sie müssen einen Stagingslot mit einer vollständig synchronisierten Kopie der Datenbank erstellen, um ein Rollback des Upgrades zu ermöglichen. Anhand der folgenden Schritte erstellen Sie einen Stagingumgebung für das Upgrade:

1. Erstellen Sie eine sekundäre Datenbank in derselben Azure-Region. Überwachen Sie die sekundäre Datenbank, um festzustellen, ob der Seedingprozess abgeschlossen ist (1).
2. Erstellen Sie einen neuen Bereitstellungsslot für Ihre Webanwendung mit der Bezeichnung „Staging“. Dieser wird im DNS mit der URL `contoso-staging.azurewebsites.net` registriert (2).

> [!NOTE]
> Beachten Sie, dass sich die Vorbereitungsschritte nicht auf den Produktionsslot auswirken und dieser im Vollzugriffsmodus funktionieren kann.
>  

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

Wenn die Vorbereitungsschritte abgeschlossen sind, ist die Anwendung bereit für das tatsächliche Upgrade. Das folgende Diagramm veranschaulicht die Schritte, die der Upgradevorgang umfasst.

1. Setzen Sie die primäre Datenbank in den schreibgeschützten Modus (3). Durch diesen Modus wird sichergestellt, dass der Produktionsslot der Webanwendung (V1) während des Upgrades schreibgeschützt bleibt, sodass Datenabweichungen zwischen V1- und V2-Datenbankinstanzen vermieden werden.  
2. Trennen Sie die sekundäre Datenbank mithilfe des Modus für die geplante Beendigung (4). Dieser erstellt eine vollständig synchronisierte, unabhängige Kopie der primären Datenbank. Diese Datenbank wird upgegradet.
3. Ändern Sie den Modus der primären Datenbank in den Lese-/Schreibmodus, und führen Sie das Upgradeskript aus (5).

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Wenn das Upgrade erfolgreich abgeschlossen ist, können die Endbenutzer jetzt zu der aktualisierten Kopie der Anwendung wechseln. Diese wird jetzt zu einem Produktionsslot.  Der Wechsel umfasst ein paar mehr Schritte, wie im folgenden Diagramm dargestellt.

1. Aktivieren Sie einen Swapvorgang zwischen Produktions- und Stagingslots der Webanwendung (6). Damit werden die URLs der beiden Slots umgestellt. Jetzt verweist `contoso.azurewebsites.net` auf die V2-Version der Website und der Datenbank (Produktionsumgebung).  
2. Wenn Sie die V1-Version, die nach dem Austausch eine Stagingkopie wird, nicht mehr benötigen, können Sie die Stagingumgebung (7) außer Betrieb setzen.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Wenn der Upgradevorgang nicht erfolgreich ist, z.B. aufgrund eines Fehlers im Upgradeskript, sollte der Stagingslot als gefährdet betrachtet werden. Legen Sie für die Anwendung im Produktionsslot wieder „Vollzugriff“ fest, um sie auf den Status vor dem Upgrade zurückzusetzen. Die Schritte hierzu werden im nächsten Diagramm angezeigt.

1. Legen Sie für die Datenbankkopie den Lese-/Schreibmodus fest (8). Dadurch wird die vollständige V1-Funktionalität in der Produktionskopie wiederhergestellt.
2. Führen Sie die Analyse der Grundursache, und setzen Sie die Stagingumgebung (9) außer Betrieb.

An dieser Stelle ist die Anwendung voll funktionsfähig, und die Upgradeschritte können wiederholt werden.

> [!NOTE]
> Das Rollback erfordert keine DNS-Änderungen, da Sie den Swap-Vorgang noch nicht ausgeführt haben.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

Der wichtigste **Vorteil** dieser Option ist, dass Sie eine Anwendung in einer einzelnen Region mithilfe einer Reihe von einfachen Schritten upgraden können. Die anfallenden Kosten für das Upgrade sind relativ gering. Der wichtigste **Nachteil** ist, dass im Fall eines schwerwiegenden Fehlers während des Upgrades die Wiederherstellung auf den Status vor dem Upgrade beinhaltet, dass die Anwendung in einer anderen Region erneut bereitgestellt und die Datenbank mithilfe der Geowiederherstellung aus einer Sicherung wiederhergestellt wird. Dieser Prozess führt zu erheblicher Downtime.

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Upgrade von Anwendungen, die für die Notfallwiederherstellung von der Datenbank-Georeplikation abhängen

Wenn Ihre Anwendung für die Geschäftskontinuität die aktive Georeplikation oder Failovergruppen nutzt, wird sie in mindestens zwei verschiedenen Regionen bereitgestellt. In der primären Region wird eine primäre Datenbank und in der Sicherungsregion eine schreibgeschützte sekundäre Datenbank bereitgestellt. Zusätzlich zu den weiter oben genannten Faktoren muss der Upgradevorgang Folgendes garantieren:

* Die Anwendung bleibt jederzeit während des Upgrades vor schwerwiegenden Fehlern geschützt
* Die georedundanten Komponenten der Anwendung werden parallel mit den aktiven Komponenten upgegradet

Zum Erreichen dieser Ziele nutzen Sie neben den Bereitstellungsslots für die Webanwendung den Azure Traffic Manager (ATM), indem Sie das Failoverprofil mit einem aktiven Endpunkt und einem Sicherungsendpunkt verwenden.  Das folgende Diagramm veranschaulicht die Betriebsumgebung vor dem Upgradevorgang. Die Websites `contoso-1.azurewebsites.net` und `contoso-dr.azurewebsites.net` stellen eine Produktionsumgebung der Anwendung mit vollständiger geografischer Redundanz dar. Die Produktionsumgebung umfasst die folgenden Komponenten:

1. Produktionsslot der Webanwendung `contoso-1.azurewebsites.net` in der primären Region (1)
2. Primäre Datenbank in der primären Region (2) 
3. Ein Standby-Instanz der Webanwendung in der Sicherungsregion (3)
4. Georeplizierte sekundäre Datenbank in der Sicherungsregion (4)
5. Azure Traffic Manager-Leistungsprofil mit Onlineendpunkt `contoso-1.azurewebsites.net` und Offlineendpunkt `contoso-dr.azurewebsites.net`

Sie müssen eine Stagingumgebung mit einer vollständig synchronisierten Kopie der Anwendung erstellen, um ein Rollback des Upgrades zu ermöglichen. Die Stagingumgebung muss ebenfalls georedundant sein, da Sie sicherstellen müssen, dass die Anwendung im Fall eines schwerwiegenden Fehlers während des Upgradevorgangs schnell wiederhergestellt werden kann. Anhand der folgenden Schritte erstellen Sie einen Stagingumgebung für das Upgrade:

1. Stellen Sie einen Stagingslot der Webanwendung in der primären Region (6) bereit.
2. Erstellen Sie eine sekundäre Datenbank in der primären Azure-Region (7). Konfigurieren Sie den Stagingslot der Webanwendung, um eine Verbindung damit herzustellen. 
3. Erstellen Sie eine weitere georedundante sekundär Datenbank in der Sicherungsregion, indem Sie die sekundäre Datenbank in der primären Region replizieren (dies wird „verkettete Georeplikation“ genannt) (8).
3. Stellen Sie einen Stagingslot der Webanwendungsinstanz in der Sicherungsregion (9) bereit, und konfigurieren Sie ihn so, dass er eine Verbindung mit der in Schritt (9) erstellten georedundaten sekundären Datenbank herstellt.


> [!NOTE]
> Beachten Sie, dass sich die Vorbereitungsschritte nicht auf die Anwendung im Produktionsslot auswirken und diese im Lese-/Schreibmodus voll funktionsfähig bleibt.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

Wenn die Vorbereitungsschritte abgeschlossen sind, ist die Stagingumgebung bereit für das Upgrade. Das folgende Diagramm veranschaulicht die Upgradeschritte.

1. Legen Sie für die primäre Datenbank im Produktionsslot den schreibgeschützten Modus fest (10). Durch diesen Modus wird sichergestellt, dass die Produktionsdatenbank (V1) während des Upgrades unverändert bleibt, sodass Datenabweichungen zwischen V1- und V2-Datenbankinstanzen vermieden werden.  
2. Trennen Sie die sekundäre Datenbank in derselben Region mithilfe des Modus für die geplante Beendigung (11). Dieser erstellt eine unabhängige, aber vollständig synchronisierte Kopie der Produktionsdatenbank. Diese Datenbank wird upgegradet.
3. Führen Sie das Upgradeskript für `contoso-1-staging.azurewebsites.net`, `contoso-dr-staging.azurewebsites.net` und die primäre Stagingdatenbank aus (12). Die Änderungen in der Datenbank werden automatisch in die sekundäre Stagingdatenbank repliziert. 

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

Wenn das Upgrade erfolgreich abgeschlossen ist, können die Endbenutzer jetzt zu der V2-Version der Anwendung wechseln. Das folgende Diagramm veranschaulicht die dafür notwendigen Schritte.

1. Aktivieren Sie einen Swapvorgang zwischen Produktions- und Stagingslots der Webanwendung in der primären Region (13) und in der Sicherungsregion (14). V2 der Anwendung wird jetzt ein Produktionsslot mit einer redundanten Kopie in der Sicherungsregion.
2. Sie können die Stagingumgebung außer Betrieb setzen, wenn Sie die V1-Anwendung (15 und 16) nicht mehr benötigen.  

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Wenn der Upgradevorgang nicht erfolgreich ist, z.B. aufgrund eines Fehlers im Upgradeskript, sollte die Stagingumgebung als inkonsistent betrachtet werden. Um die Anwendung wieder auf den Status vor dem Upgrade zurückzusetzen, verwenden Sie wieder V1 der Anwendung in der Produktionsumgebung. Die Schritte hierzu werden im nächsten Diagramm angezeigt.

1. Legen Sie für die Kopie der primären Datenbank im Produktionsslot den Lese-/Schreibmodus fest (17). Dadurch wird die vollständige V1 im Produktionsslot wiederhergestellt.
2. Führen Sie die Analyse der Grundursache, und reparieren oder entfernen Sie die Stagingumgebung (18 und 19).

An dieser Stelle ist die Anwendung voll funktionsfähig, und die Upgradeschritte können wiederholt werden.

> [!NOTE]
> Das Rollback erfordert keine DNS-Änderungen, da Sie den Swap-Vorgang nicht ausgeführt haben.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

Der wichtigste **Vorteil** dieser Option ist, dass Sie die Anwendung und ihre georedundante Kopie parallel upgraden können, ohne Ihre Geschäftskontinuität während des Upgrades zu beeinträchtigen. Der wichtigste **Nachteil** ist, dass die Option von jeder Anwendungskomponente die doppelte Redundanz erfordert und daher höhere Kosten anfallen. Sie umfasst auch einen komplexeren Workflow.

## <a name="summary"></a>Zusammenfassung

Die beiden in diesem Artikel beschriebenen Upgrademethoden unterscheiden sich hinsichtlich der Komplexität und der anfallenden Kosten. Beide legen jedoch den Fokus auf die Minimierung der Zeit, in der die Endbenutzer auf schreibgeschützte Vorgänge eingeschränkt sind. Diese Zeit wird direkt durch die Dauer des Upgradeskripts definiert. Sie hängt nicht von der Datenbankgröße, der gewählten Dienstebene, der Websitekonfiguration oder anderen Faktoren ab, die Sie nicht einfach steuern können. Alle Vorbereitungsschritte werden von den Upgradeschritten entkoppelt und haben keine Auswirkungen auf die Produktionsanwendung. Die Effizienz des Upgradeskripts ist der wichtigste Faktor, der die Endbenutzerfreundlichkeit während Upgrades bestimmt. Die beste Möglichkeit diese zu verbessern ist daher, sich darauf zu konzentrieren, das Upgradeskript so effizient wie möglich zu gestalten.  

## <a name="next-steps"></a>Nächste Schritte

* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
* Weitere Informationen zur aktive Georeplikation der Azure SQL-Datenbank finden Sie unter [Erstellen lesbarer sekundärer Datenbanken mithilfe aktiver Georeplikation](sql-database-active-geo-replication.md).
* Weitere Informationen zur Failovergruppen für einen Azure SQL-Datenbank finden Sie unter [Verwenden von Autofailover-Gruppen für ein transparentes und koordiniertes Failover mehrerer Datenbanken](sql-database-auto-failover-group.md).
* Weitere Informationen zu Bereitstellungsslots und Stagingumgebungen in Azure App Service finden Sie unter [Einrichten von Stagingumgebungen in Azure App Service](../app-service/deploy-staging-slots.md).  
* Weitere Informationen zu Azure Traffic Manager-Profilen finden Sie unter [Verwalten von Azure Traffic Manager-Profilen](../traffic-manager/traffic-manager-manage-profiles.md).  



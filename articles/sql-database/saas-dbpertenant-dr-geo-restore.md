---
title: Notfallwiederherstellung für SaaS-Anwendungen über georedundante Azure SQL-Datenbank-Sicherungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie georedundante Azure SQL-Datenbank-Sicherungen verwenden können, um eine mehrinstanzenfähige SaaS-Anwendung nach einem Ausfall wiederherzustellen.
keywords: Tutorial zur SQL-Datenbank
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/16/2018
ms.author: ayolubek
ms.openlocfilehash: a677e6eb583e293f83df824804aa4cd6f8f5d778
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="recover-a-multi-tenant-saas-application-using-geo-restore-from-database-backups"></a>Wiederherstellen einer mehrinstanzenfähigen SaaS-Anwendung über Geowiederherstellung aus Datenbanksicherungen

In diesem Tutorial erkunden Sie ein vollständiges Szenario zur Notfallwiederherstellung für eine mehrinstanzenfähige SaaS-Anwendung, die mit dem Modell implementiert wurde, bei dem eine Datenbank pro Mandant verwendet wird. Sie verwenden [_Geowiederherstellung_](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-recovery-using-backups), um den Katalog und die Mandantendatenbanken aus automatisch verwalteten georedundanten Sicherungen in einer anderen Wiederherstellungsregion wiederherzustellen. Nach Behebung des Ausfalls verwenden Sie [_Georeplikation_](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview), um geänderte Datenbanken in ihre ursprüngliche Region zurückzuführen.

![Architektur für Geowiederherstellung](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Geowiederherstellung ist die kostengünstigste Notfallwiederherstellungslösung für SQL-Datenbank.  Allerdings kann eine Wiederherstellung aus georedundanten Sicherungen zu einem Datenverlust von bis zu einer Stunde führen und kann, je nach Größe jeder Datenbank, erhebliche Zeit beanspruchen. **Um Anwendungen mit dem geringsten möglichen RPO und RTO wiederherzustellen, verwenden Sie Georeplikation anstelle von Geowiederherstellung**.

In diesem Tutorial werden sowohl der Wiederherstellungs- als auch der Rückführungsworkflow vorgestellt. Folgendes wird vermittelt:
> [!div class="checklist"]

>* Synchronisieren der Datenbankinformationen sowie der Konfigurationsinformationen für den Pool für elastische Datenbanken im Mandantenkatalog
>* Einrichten einer Spiegelimageumgebung in einer Wiederherstellungsregion, wobei die Umgebung die Anwendung, Server und Pools umfasst    
>* Wiederherstellen des Katalogs und der Mandantendatenbanken über _Geowiederherstellung_
>* Rückführen des Mandantenkatalogs und der geänderten Mandantendatenbanken über _Georeplikation_, nachdem der Ausfall behoben ist
>* Aktualisieren des Katalogs, nachdem jede Datenbank wiederhergestellt (oder zurückgeführt) wurde, um den aktuellen Speicherort der aktiven Kopie der Datenbank jedes Mandanten zu nachzuverfolgen
>* Sicherstellen, dass sich die Anwendung und die Mandantendatenbank immer zusammen in derselben Azure-Region befinden, um die Wartezeit zu verringern  
 

Bevor Sie mit diesem Tutorial beginnen, sollten Sie sich vergewissern, dass die folgenden Voraussetzungen erfüllt sind:
* Die Wingtip Tickets-SaaS-App mit einer Datenbank pro Mandant ist bereitgestellt. Unter [Bereitstellen und Kennenlernen der Wingtip Tickets SaaS-App mit einer Datenbank pro Mandant](saas-dbpertenant-get-started-deploy.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.  
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Einführung in die Vorgehensweise einer Geowiederherstellung

Notfallwiederherstellung (NW) ist ein wichtiger Aspekt für viele Anwendungen, sei es aus Konformitätsgründen oder für Geschäftskontinuität. Sollte es zu einem längeren Dienstausfall kommen, lässt sich die Geschäftsunterbrechung mit einem gut vorbereiteten NW-Plan minimieren. Ein NW-Plan, der auf Geowiederherstellung basiert, muss mehrere Ziele erfüllen:
 * Jegliche erforderliche Kapazität in der ausgewählten Wiederherstellungsregion muss reserviert werden, damit sichergestellt ist, dass sie zum Wiederherstellen der Mandantendatenbanken verfügbar ist.
 * Es muss eine Umgebung für Spiegelimagewiederherstellung eingerichtet werden, die der ursprünglichen Pool- und Datenbankkonfiguration entspricht. 
 * Es muss möglich sein, den Wiederherstellungsprozess während der Ausführung abzubrechen, wenn die ursprüngliche Region wieder online geschaltet wurde.
 * Schnelle Mandantenbereitstellung, damit das Onboarding neuer Mandanten so schnell wie möglich wieder gestartet werden kann.  
 * Der Plan muss für ein Wiederherstellen der Mandanten in der Reihenfolge ihrer Priorität optimiert sein.
 * Der Plan muss dafür optimiert sein, Mandanten schnellstmöglich online schalten zu können, indem Schritte nach Möglichkeit parallel ausgeführt werden.
 * Der Plan muss ausfallsicher, neu startbar und idempotent sein.
 * Rückführen von Datenbanken in ihre ursprüngliche Region mit minimaler Auswirkung auf Mandanten, wenn der Ausfall behoben ist.  

> [!Note]
> Die Anwendung wird in dem _Regionspaar_ der Region wiederhergestellt, in dem die Anwendung bereitgestellt wird. Weitere Informationen finden Sie unter [Azure-Regionspaare](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).   



In diesem Tutorial werden diese Herausforderungen mit Funktionen von Azure SQL-Datenbank und der Azure-Plattform gemeistert:

* [Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), um jegliche erforderliche Kapazität so schnell wie möglich zu reservieren. Azure Resource Manager-Vorlagen werden verwendet, um ein Spiegelimage der ursprünglichen Server und der Pools für elastische Datenbanken in der Wiederherstellungsregion bereitzustellen. Außerdem werden ein separater Server und Pool für die Bereitstellung neuer Mandanten erstellt. 
* [Clientbibliothek für elastische Datenbanken](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-database-client-library) (CBED), um einen Mandantendatenbankkatalog zu erstellen und zu verwalten.  Der Katalog wird erweitert, sodass er die regelmäßig aktualisierten Pool- und Datenbankkonfigurationsinformationen enthält.
* [Shardverwaltungs-Wiederherstellungsfunktionen](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) der CBED, um Datenbankspeicherorteinträge im Katalog während einer Wiederherstellung und Rückführung zu verwalten.  
* [Geowiederherstellung](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery), um den Katalog und die Mandantendatenbanken aus automatisch verwalteten georedundanten Sicherungen wiederherzustellen. 
* [Asynchrone Wiederherstellungsvorgänge](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations), die in der Mandantenprioritätsreihenfolge gesendet, vom System für jeden Pool in die Warteschlange gesetzt und in Batches verarbeitet werden, sodass der Pool nicht überlastet wird. Diese Vorgänge können bei Bedarf vor oder während der Ausführung abgebrochen werden.    
* [Georeplikation](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview), um Datenbanken nach einem Ausfall in die ursprüngliche Region zurückzuführen. Durch Verwendung von Georeplikation ist sichergestellt, dass keine Daten verloren gehen und der Mandant nur minimal betroffen ist.
* [DNS-Aliase für SQL-Server](https://docs.microsoft.com/azure/sql-database/dns-alias-overview), damit der Katalogsynchronisierungsprozess eine Verbindung mit dem aktiven Katalog herstellen kann, egal, wo sich dieser befindet.  

## <a name="get-the-disaster-recovery--scripts"></a>Abrufen der Notfallwiederherstellungsskripts 

Die NW-Skripts, die in diesem Tutorial verwendet werden, sind im [GitHub-Repository für Wingtip Tickets SaaS-Datenbank pro Mandant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) verfügbar. Lesen Sie die Schritte zum Herunterladen und Entsperren der Wingtip Tickets-Verwaltungsskripts unter [Allgemeine Hinweise](saas-tenancy-wingtip-app-guidance-tips.md).
> [!IMPORTANT]
> Wie alle Wingtip Tickets-Verwaltungsskripts haben die NW-Skripts die Qualität von Beispielskripts und sollten daher nicht in einer Produktionsumgebung verwendet werden.   

## <a name="review-the-healthy-state-of-the-application"></a>Überprüfen des ordnungsgemäßen Zustands der Anwendung
Bevor Sie den Wiederherstellungsprozess starten, sollten Sie den normalen ordnungsgemäßen Zustand der Anwendung überprüfen.
1. Öffnen Sie in Ihrem Webbrowser den Wingtip Tickets-Veranstaltungshub (Events Hub, http://events.wingtip-dpt.&lt;Benutzer&gt;.trafficmanager.net – ersetzen Sie &lt;Benutzer&gt; durch den Benutzerwert Ihrer Bereitstellung).
    * Scrollen Sie zum unteren Rand der Seite, und beachten Sie den Katalogservernamen und den Standort in der Fußzeile.  Der Standort entspricht die Region, in der Sie die App bereitgestellt haben.    
        Tipp: Zeigen Sie mit der Maus auf den Standort, um die Anzeige zu vergrößern.

    ![Ordnungsgemäßer Zustand des Veranstaltungshubs in der ursprünglichen Region](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

1. Klicken Sie auf den Mandanten „Contoso Concert Hall“, um dessen Veranstaltungsseite zu öffnen.
    * Beachten Sie in der Fußzeile den Servernamen für den Mandanten. Der Standort ist mit dem Standort des Katalogservers identisch.

    ![Ursprüngliche Region von „Contoso Concert Hall“](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png)    
1. Überprüfen und öffnen Sie im [Azure-Portal](https://portal.azure.com) die Ressourcengruppe, in der Sie die App bereitgestellt haben.
    * Beachten Sie die Ressourcen und die Region, in der die Dienstkomponenten der App und die SQL-Datenbankserver bereitgestellt werden.

## <a name="sync-tenant-configuration-into-catalog"></a>Synchronisieren der Mandantenkonfiguration in den Katalog

In dieser Aufgabe starten Sie einen Prozess, in dem die Konfiguration der Server, der Pools für elastische Datenbanken und der Datenbanken in den Mandantenkatalog synchronisiert wird.  Diese Informationen werden später dazu verwendet, eine Spiegelimageumgebung in der Wiederherstellungsregion zu konfigurieren.

> [!IMPORTANT]
> Der Einfachheit halber werden der Synchronisierungsprozess und andere lange dauernde Wiederherstellungs- und Rückführungsprozesse in diesen Beispielen als lokale Powershell-Aufträge oder -Sitzungen implementiert, die unter Ihrer Clientbenutzeranmeldung ausgeführt werden. Die Authentifizierungstoken, die ausgegeben werden, wenn Sie sich anmelden, laufen nach einigen Stunden ab, und die Aufträge schlagen dann fehl. In einem Produktionsszenario sollten lange dauernde Prozesse als zuverlässige Azure-Dienste beliebiger Art implementiert werden, die unter einem Dienstprinzipal ausgeführt werden. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell zum Erstellen eines Dienstprinzipals mit einem Zertifikat](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. Öffnen Sie in _PowerShell ISE_ die Datei „...\Learning Modules\UserConfig.psm1“. Ersetzen Sie `<resourcegroup>` und `<user>` in den Zeilen 10 und 11 durch die jeweiligen Werte, die Sie beim Bereitstellen der App verwendet haben.  Speichern Sie die Datei.

2. Öffnen Sie in *PowerShell ISE* das Skript „...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1“.
    *  Im Verlauf dieses Tutorials führen Sie jedes der Szenarien in diesem PowerShell-Skript aus, also belassen Sie diese Datei geöffnet.

3. Legen Sie Folgendes fest:
    * **$DemoScenario = 1**: Starten eines Hintergrundauftrags, in dem Mandantenserver- und Poolkonfigurationsinformationen in den Katalog synchronisiert werden

3. Drücken Sie **F5**, um das Synchronisierungsskript auszuführen. 
    *  Diese Informationen werden später verwendet, um sicherzustellen, dass bei der Wiederherstellung ein Spiegelimage der Server, Pools und Datenbanken in der Wiederherstellungsregion erstellt wird.  
![Synchronisierungsprozess](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Lassen Sie das PowerShell-Fenster im Hintergrund weiter laufen, und führen Sie die weiteren Schritte dieses Tutorials aus.

> [!Note]
> Im Synchronisierungsprozess wird die Verbindung mit dem Katalog über einen DNS-Alias hergestellt. Der Alias wird während der Wiederherstellung und Rückführung so geändert, dass er auf den aktiven Katalog verweist. Der Synchronisierungsprozess hält den Katalog mit jeder Datenbank- oder Poolkonfigurationsänderung, die in der Wiederherstellungsregion vorgenommen wurde, auf dem neuesten Stand.  Während der Rückführung werden diese Änderungen auf die entsprechenden Ressourcen in der ursprünglichen Region angewendet.

## <a name="geo-restore-recovery-process-overview"></a>Übersicht über den Wiederherstellungsprozess mit Geowiederherstellung

Bei dem Wiederherstellungsprozess mit Geowiederherstellung erfolgen die Bereitstellung der Anwendung und die Wiederherstellung der Datenbanken aus Sicherungen in die Wiederherstellungsregion.

Im Wiederherstellungsprozess wird wie folgt vorgegangen:

1. Der Traffic Manager-Endpunkt für die Web-App in der ursprünglichen Region wird deaktiviert. Durch Deaktivieren des Endpunkts wird verhindert, dass Benutzer eine Verbindung mit der App in einem ungültigen Zustand herstellen, sollte die ursprüngliche Region während der Wiederherstellung wieder online geschaltet werden.

1. Es wird ein Wiederherstellungskatalogserver in der Wiederherstellungsregion bereitgestellt, die Katalogdatenbank wird über Geowiederherstellung wiederhergestellt, und der _activecatalog_-Alias wird aktualisiert, sodass er auf den wiederhergestellten Katalogserver verweist.  
    * Durch das Ändern des Katalogalias wird sichergestellt, dass der Katalogsynchronisierungsprozess immer mit dem aktiven-Katalog synchronisiert.

1. Alle vorhandenen Mandanten im Wiederherstellungskatalog werden als offline markiert, damit Zugriff auf Mandantendatenbanken verhindert wird, bevor diese wiederhergestellt sind.

1. Es wird eine Instanz der App in der Wiederherstellungsregion bereitgestellt, und die App wird so konfiguriert, dass für sie der wiederhergestellte Katalog in dieser Region verwendet wird.
    * Um die Wartezeit möglichst klein zu halten, ist die Beispiel-App so konzipiert, dass für sie immer eine Verbindung mit einer Mandantendatenbank in derselben Region hergestellt wird.

1. Es werden ein Server und ein Pool für elastische Datenbanken bereitgestellt, in dem neue Mandanten bereitgestellt werden. Durch Erstellen dieser Ressourcen wird sichergestellt, dass sich die Bereitstellung neuer Mandanten und die Wiederherstellung vorhandener Mandanten nicht wechselseitig stören.

1. Der neue Mandantenalias wird aktualisiert, sodass er auf den Server für neue Mandantendatenbanken in der Wiederherstellungsregion verweist. Durch Ändern dieses Alias wird sichergestellt, dass Datenbanken für jeden neuen Mandanten in der Wiederherstellungsregion bereitgestellt werden.
        
1. Es werden Server und Pools für elastische Datenbanken in der Wiederherstellungsregion bereitgestellt, damit Mandantendatenbanken wiederhergestellt werden können. Diese Server und Pools sind ein Spiegelimage der Konfiguration in der ursprünglichen Region.  Durch Bereitstellung von Pools im Voraus wird die Kapazität reserviert, die zum Wiederherstellen aller Datenbanken erforderlich ist.
    * Ein Ausfall in einer Region bedingt möglicherweise großen Druck auf die Ressourcen, die im Regionspaar (gekoppelte Region) verfügbar sind.  Wenn Sie für eine Notfallwiederherstellung mit Geowiederherstellung arbeiten, ist schnelles Reservieren von Ressourcen empfehlenswert. Sie sollten das Verwenden von Georeplikation in Betracht ziehen, wenn es unverzichtbar ist, dass eine Anwendung in einer bestimmten Region wiederhergestellt werden muss. 

1. Der Traffic Manager-Endpunkt für die Web-App in der Wiederherstellungsregion wird aktiviert. Durch das Aktivieren dieses Endpunkts wird es der Anwendung ermöglicht, neue Mandanten bereitzustellen. Zu diesem Zeitpunkt sind die vorhandenen Mandanten immer noch offline.

1. Es werden Batches mit Anforderungen gesendet, um die Datenbanken in der Reihenfolge ihrer Priorität wiederherzustellen. 
    * Die Batches sind so konzipiert, dass die Datenbanken parallel über alle Pools wiederhergestellt werden.  
    * Wiederherstellungsanforderungen werden asynchron gesendet, sodass sie schnell gesendet und zur Ausführung in jedem Pool in die Warteschlange gesetzt werden.
    * Da Wiederherstellungsanforderungen durch alle Pools hindurch parallel verarbeitet werden, empfiehlt es sich, wichtige Mandanten über viele Pools zu verteilen. 

1. Der SQL-Datenbankdienst wird überwacht, um zu bestimmen, wann Datenbanken wiederhergestellt sind. Nachdem eine Mandantendatenbank wiederhergestellt ist, wird sie im Katalog als online markiert, und es wird eine „rowversion“-Summe für die Mandantendatenbank aufgezeichnet. 
    * Die Anwendung kann auf die Mandantendatenbanken zugreifen, sobald diese im Katalog als online markiert sind.
    * Eine Summe der „rowversion“-Werte in der Mandantendatenbank wird im Katalog gespeichert. Diese Summe fungiert als ein Fingerabdruck, über den der Rückführungsprozess bestimmen kann, ob die Datenbank in der Wiederherstellungsregion aktualisiert wurde.      

## <a name="run-the-recovery-script"></a>Ausführen des Wiederherstellungsskripts

> [!IMPORTANT]
> In diesem Tutorial werden Datenbanken aus georedundanten Sicherungen wiederhergestellt. Obwohl diese Sicherungen in der Regel innerhalb von 10 Minuten verfügbar sind, kann es bis zu einer Stunde dauern, bevor sie verfügbar sind. Das Skript wird angehalten, bis sie verfügbar sind.  Jetzt ist der richtige Zeitpunkt für einen Kaffee.

Stellen Sie sich nun vor, dass es einen Ausfall in der Region gibt, in der die Anwendung bereitgestellt wird, und führen Sie das Wiederherstellungsskript aus:

1. Legen Sie in *PowerShell ISE* im Skript „...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1“ die folgenden Werte fest:
    * **$DemoScenario = 2**: Wiederherstellen der App in einer Wiederherstellungsregion, indem sie aus georedundanten Sicherungen wiederhergestellt wird

1. Drücken Sie **F5** , um das Skript auszuführen.  
    * Das Skript wird in einem neuen PowerShell-Fenster geöffnet, und in ihm werden einige PowerShell-Aufträge gestartet, die parallel ausgeführt werden.  In diesen Aufträgen werden Server, Pools und Datenbanken in der Wiederherstellungsregion wiederhergestellt. 
    * Die Wiederherstellungsregion ist die _gekoppelte Region_, die der Azure-Region zugeordnet ist, in der Sie die Anwendung bereitgestellt haben. Weitere Informationen finden Sie unter [Azure-Regionspaare](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions). 

1. Überwachen Sie den Status des Wiederherstellungsprozesses im PowerShell-Fenster.

    ![Wiederherstellungsprozess](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

>Um den Code für die Wiederherstellungsaufträge zu untersuchen, sehen Sie sich die PowerShell-Skripts an, die im Ordner „...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\RecoveryJobs“ enthalten sind.

## <a name="review-the-application-state-during-recovery"></a>Überprüfen des Anwendungszustands während der Wiederherstellung
Solange der Anwendungsendpunkt in Traffic Manager deaktiviert ist, ist die Anwendung nicht verfügbar. Der Katalog wird wiederhergestellt, und alle Mandanten werden als offline markiert.  Der Anwendungsendpunkt in der Wiederherstellungsregion wird dann aktiviert, und die Anwendung ist wieder online. Obwohl die Anwendung verfügbar ist, werden Mandanten im Veranstaltungshub solange als offline angezeigt, bis ihre Datenbanken wiederhergestellt wurden. Sie müssen Ihre Anwendung so konzipieren, dass sie mit Mandantendatenbanken umgehen kann, die offline sind.

1. Aktualisieren Sie den Wingtip Tickets-Veranstaltungshub (Events Hub) in Ihrem Webbrowser, nachdem die Datenbank wiederhergestellt wurde, aber bevor die Mandanten wieder online sind.
    * Sie können in der Fußzeile sehen, dass der Name des Katalogservers jetzt das Suffix _-recovery_ hat und der Server sich in der Wiederherstellungsregion befindet.
    * Mandanten, die noch nicht wiederhergestellt wurden, sind als offline markiert und können nicht ausgewählt werden.   
 
    ![Wiederherstellungsprozess](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * Wenn Sie die „Veranstaltungen“-Seite eines Mandanten direkt öffnen, während der Mandant offline ist, wird auf der Seite eine „Mandant ist offline“-Benachrichtigung angezeigt. Versuchen Sie beispielsweise, wenn Contoso Concert Hall offline ist, „http://events.wingtip-dpt.&lt;Benutzer&gt;.trafficmanager.net/contosoconcerthall“ zu öffnen. ![Wiederherstellungsprozess](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Bereitstellen eines neuen Mandanten in der Wiederherstellungsregion
Schon bevor Mandantendatenbanken wiederhergestellt sind, können Sie neue Mandanten in der Wiederherstellungsregion bereitstellen. Neue Mandantendatenbanken, die in der Wiederherstellungsregion bereitgestellt wurden, werden später mit den wiederhergestellten Datenbanken zurückgeführt.   

1. Legen Sie in *PowerShell ISE* im Skript „...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1“ die folgende Eigenschaft fest:
    * **$DemoScenario = 3**: Bereitstellen eines neuen Mandanten in der Wiederherstellungsregion

1. Drücken Sie **F5** , um das Skript auszuführen. 

1. Die Seite für Hawthorn Hall-Veranstaltungen wird im Browser geöffnet, wenn die Bereitstellung abgeschlossen ist. 
    * Beachten Sie, dass sich die Hawthorn Hall-Datenbank in der Wiederherstellungsregion befindet.
    ![Hawthorn Hall, bereitgestellt in der Wiederherstellungsregion](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

1. Aktualisieren Sie im Browser die Wingtip Tickets-Seite für den Veranstaltungshub (Events Hub), damit Hawthorn Hall auf der Seite enthalten ist. 
    * Wenn Sie Hawthorn Hall bereitgestellt haben, ohne zu warten, bis die anderen Mandanten wiederherstellt waren, sind andere Mandanten möglicherweise noch offline.

## <a name="review-the-recovered-state-of-the-application"></a>Überprüfen des wiederhergestellten Status der Anwendung

Wenn der Wiederherstellungsprozess abgeschlossen ist, sind die Anwendung und alle Mandanten in der Wiederherstellungsregion voll funktionsfähig. 

1. Sobald im PowerShell-Konsolenfenster angezeigt wird, dass alle Mandanten wiederhergestellt sind, aktualisieren Sie den Veranstaltungshub.  Die Mandanten werden als online angezeigt, so auch der neue Mandant Hawthorn Hall.

    ![Wiederhergestellte und neue Mandanten im Veranstaltungshub](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

1. Klicken Sie auf „Contoso Concert Hall“, und öffnen Sie die zugehörige Seite für Veranstaltungen.
    * In der Fußzeile sehen Sie, dass sich die Datenbank auf dem Wiederherstellungsserver befindet, der sich in der Wiederherstellungsregion befindet.

    ![Contoso in der Wiederherstellungsregion](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Liste der Ressourcengruppen.  
    * Beachten Sie die Ressourcengruppe, die Sie bereitgestellt haben, und die Wiederherstellungsressourcengruppe mit dem Suffix _-recovery_.  Die Wiederherstellungsressourcengruppe enthält alle Ressourcen, die während des Wiederherstellungsprozesses erstellt wurden, sowie neue Ressourcen, die während des Ausfalls erstellt wurden.  

1. Öffnen Sie die Wiederherstellungsressourcengruppe, und beachten Sie die folgenden Elemente:
    * Die Wiederherstellungsversionen des Katalog- und des tenants1-Servers mit dem Suffix _-recovery_.  Die wiederhergestellten Katalog- und Mandantendatenbanken auf diesen Servern haben alle die Namen, die in der ursprünglichen Region verwendet wurden.

    * Der SQL-Server _tenants2-dpt-&lt;Benutzer&gt;-recovery_.  Dieser Server wird dazu verwendet, neue Mandanten während des Ausfalls bereitzustellen.
    *   Der App Service namens _events-wingtip-dpt-&lt;recoveryregion&gt;-&lt;Benutzer&gt_. Dies ist die Wiederherstellungsinstanz der Veranstaltungen-App. 

    ![Contoso in der Wiederherstellungsregion](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png)   
    
1. Öffnen Sie den SQL-Server _tenants2-dpt-&lt;Benutzer&gt;-recovery_.  Dieser enthält die Datenbank _hawthornhall_ und den Pool für elastische Datenbanken _Pool1_.  Die _hawthornhall_-Datenbank ist im Pool für elastische Datenbanken _Pool1_ als eine elastische Datenbank konfiguriert.

## <a name="change-tenant-data"></a>Ändern von Mandantendaten 
In dieser Aufgabe aktualisieren Sie eine der wiederhergestellten Mandantendatenbanken. Im Rückführungsprozess werden wiederhergestellte Datenbanken, die geändert wurden, in die ursprüngliche Region kopiert. 

1. Suchen Sie in Ihrem Browser nach der Veranstaltungsliste für die Contoso Concert Hall, scrollen Sie durch die Veranstaltungen, und notieren Sie sich die letzte Veranstaltung, _Seriously Strauss_.

1. Legen Sie in *PowerShell ISE* im Skript „...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1“ den folgenden Wert fest:
    * **$DemoScenario = 4**: Löschen einer Veranstaltung aus einem Mandanten in der Wiederherstellungsregion
1. Drücken Sie **F5**, um das Skript auszuführen.
1. Aktualisieren Sie die Contoso Concert Hall-Seite für Veranstaltungen (http://events.wingtip-dpt.&lt; Benutzer&gt;.trafficmanager.net/contosoconcerthall). Sie können sehen, dass die Veranstaltung „Seriously Strauss“ fehlt.

An dieser Stelle des Tutorials haben Sie die Anwendung wiederhergestellt, die jetzt in der Wiederherstellungsregion ausgeführt wird.  Sie haben einen neuen Mandanten in der Wiederherstellungsregion bereitgestellt und Daten von einem der wiederhergestellten Mandanten geändert.  

> [!Note]
> Die anderen Tutorials im Beispiel sind nicht so konzipiert, dass sie mit der App im Wiederherstellungsstatus ausgeführt werden können. Wenn Sie die anderen Tutorials untersuchen möchten, müssen Sie die Anwendung zuerst zurückführen.

## <a name="repatriation-process-overview"></a>Übersicht über den Rückführungsprozess

Im Rückführungsprozess werden die Anwendung und deren Datenbanken in ihrer ursprünglichen Region wiederhergestellt, nachdem ein Ausfall behoben wurde.

![Rückführung über Geowiederherstellung](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 


Im Prozess wird wie folgt vorgegangen:

1. Alle derzeit laufenden Wiederherstellungsaktivitäten werden gestoppt, und alle ausstehenden oder wirksamen Anforderungen zur Datenbankwiederherstellung werden abgebrochen.

1. In der ursprünglichen Region werden die Mandantendatenbanken reaktiviert, die seit dem Ausfall nicht geändert wurden.  Hierzu gehören Datenbanken, die noch nicht wiederhergestellt wurden, und Datenbanken, die wiederhergestellt, danach aber nicht geändert wurden. Die reaktivierten Datenbanken haben exakt den Zustand wie beim letzten Zugriff von ihren Mandanten.

1. In der ursprünglichen Region wird für den neuen Mandanten ein Spiegelimage seines Servers und seines Pools für elastische Datenbanken bereitgestellt. Nachdem dieser Vorgang abgeschlossen ist, wird der neue Mandantenalias so aktualisiert, dass er auf diesen Server verweist. Das Aktualisieren des Alias bewirkt, dass das Onboarding neuer Mandanten in der ursprünglichen Region statt in der Wiederherstellungsregion erfolgt.

1. Bei Verwendung von Georeplikation wird der Katalog aus der Wiederherstellungsregion in die ursprüngliche Region verschoben.

1. Die Poolkonfiguration in der ursprünglichen Region wird aktualisiert, damit sie mit den Änderungen konsistent ist, die während des Ausfalls in der Wiederherstellungsregion vorgenommen wurden.

1. Es werden die Server und Pools erstellt, die zum Hosten aller neuen Datenbanken erforderlich sind, die während des Ausfalls erstellt wurden.

1. Bei Verwendung von Georeplikation werden Mandantendatenbanken, die nach der Wiederherstellung aktualisiert wurden, und alle neuen Mandantendatenbanken zurückgeführt, die während des Ausfalls bereitgestellt wurden. 

1. Ressourcen, die während des Wiederherstellungsprozesses in der Wiederherstellungsregion erstellt wurden, werden bereinigt.

Um die Anzahl der Mandantendatenbanken zu beschränken, die zurückgeführt werden müssen, werden die Schritte 1 bis 3 sofort ausgeführt.  

Schritt 4 wird nur ausgeführt, wenn der Katalog in der Wiederherstellungsregion während des Ausfalls geändert wurde. Der Katalog wird aktualisiert, wenn in der Wiederherstellungsregion neue Mandanten erstellt wurden oder irgendeine Datenbank- oder Poolkonfiguration geändert wurde.

Es ist entscheidend, dass der Schritt 7 nur minimale Unterbrechung verursacht und keine Daten verloren gehen. Um dieses Ziel zu erreichen, wird im Prozess _Georeplikation_ verwndet.

Bevor eine Datenbank georepliziert wird, wird die entsprechende Datenbank in der ursprünglichen Region gelöscht. Die Datenbank in der Wiederherstellungsregion wird dann georepliziert, wodurch ein sekundäres Replikat in der ursprünglichen Region erstellt wird. Nach Abschluss der Replikation wird der Mandant im Katalog als offline markiert, wodurch alle Verbindungen mit der Datenbank in der Wiederherstellungsregion getrennt werden. Für die Datenbank wird dann ein Failover ausgeführt, sodass alle ausstehenden Transaktionen in der sekundären Datenbank verarbeitet werden, weshalb keine Daten verloren gehen. Bei dem Failover werden die Datenbankrollen vertauscht.  Die sekundäre Datenbank in der ursprünglichen Region wird die primäre Datenbank mit Lese-/Schreibzugriff, und die Datenbank in der Wiederherstellungsregion wird eine schreibgeschützte sekundäre Datenbank. Der Mandanteneintrag im Katalog wird aktualisiert, damit er auf die Datenbank in der ursprünglichen Region verweist, und der Mandant wird als online markiert.  An diesem Punkt ist die Rückführung der Datenbank abgeschlossen. 

Anwendungen sollten mit Wiederholungslogik geschrieben werden, um sicherzustellen, dass sie Verbindungen automatisch wieder herstellen, wenn diese unterbrochen wurden.  Erfolgt die Verbindungswiederherstellung mithilfe des Katalogs, wird die jeweilige Verbindung mit der zurückgeführten Datenbank in der ursprünglichen Region hergestellt. Obwohl die kurze Verbindungstrennung häufig nicht bemerkt wird, ist es vorteilhaft, Datenbanken außerhalb der Geschäftszeiten zurückzuführen. 

Sobald eine Datenbank zurückgeführt ist, kann die sekundäre Datenbank in der Wiederherstellungsregion gelöscht werden. Für die Datenbank in der ursprünglichen Region wird dann wieder Geowiederherstellung als Notfallwiederherstellungsschutz verwendet.

In Schritt 8 werden Ressourcen in der Wiederherstellungsregion, einschließlich der Wiederherstellungsserver und -pools, gelöscht.

## <a name="run-the-repatriation-script"></a>Ausführen des Rückführungsskripts
Nehmen Sie nun an, dass der Ausfall behoben ist, und führen Sie das Rückführungsskript aus.

Wenn Sie dem Tutorial gefolgt sind, werden Fabrikam Jazz Club und Dogwood Dojo im Skript sofort in der ursprünglichen Region reaktiviert, da sie nicht geändert wurden. Danach werden der neue Mandant, Hawthorn Hall, und der Mandant Contoso Concert Hall, da er geändert wurde, zurückgeführt. Im Skript wird auch der Katalog zurückgeführt, der aktualisiert wurde, als Hawthorn Hall bereitgestellt wurde.
  
1. Vergewissern Sie sich in *PowerShell ISE* im Skript „...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1“,

1. dass der Katalogsynchronisierungsprozess weiterhin in seiner PowerShell-Instanz ausgeführt wird.  Starten Sie ihn bei Bedarf neu, indem Sie Folgendes festlegen:
    * **$DemoScenario = 1**: Starten der Synchronisierung der Mandantenserver-, Pool- und Datenbankkonfigurationsinformationen in den Katalog
    * Drücken Sie **F5** , um das Skript auszuführen.
1.  Legen Sie dann Folgendes fest, um den Rückführungsprozess zu starten:
    * **$DemoScenario = 5**: Rückführen der App in ihre ursprüngliche Region
    * Drücken Sie **F5**, um das Wiederherstellungsskript in einem neuen PowerShell-Fenster auszuführen.  Die Rückführung dauert einige Minuten und kann im PowerShell-Fenster überwacht werden.
1. Während das Skript ausgeführt wird, aktualisieren Sie die Seite für den Veranstaltungshub (Events Hub, http://events.wingtip-dpt.&lt; Benutzer&gt;.trafficmanager.net).
    * Sie können sehen, dass alle Mandanten im gesamten Verlauf dieses Prozesses online und für Zugriffe verfügbar sind.
1. Klicken Sie auf den Mandanten „Fabrikam Jazz Club“, um ihn zu öffnen. Wenn Sie diesen Mandanten nicht geändert haben, sehen Sie anhand der Fußzeile, dass der Server bereits als der ursprüngliche Server wiederhergestellt wurde.
1. Öffnen oder aktualisieren Sie die Seite für Contoso Concert Hall-Veranstaltungen. Anhand der Fußzeile können Sie sehen, dass sich die Datenbank immer noch auf dem Wiederherstellungsserver (_-recovery_-Server) befindet.  
1. Aktualisieren Sie die Seite für Contoso Concert Hall-Veranstaltungen, nachdem der Rückführungsprozess abgeschlossen ist, und stellen Sie fest, dass sich die Datenbank nun in Ihrer ursprünglichen Region befindet.
1. Aktualisieren Sie den Veranstaltungshub erneut, und öffnen Sie Hawthorn Hall. Sie können sehen, dass sich die Datenbank dieses Mandanten ebenfalls in der ursprünglichen Region befindet. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Bereinigen der Ressourcen in der Wiederherstellungsregion nach der Rückführung
Sobald die Rückführung abgeschlossen ist, können die Ressourcen in der Wiederherstellungsregion gefahrlos gelöscht werden. 

> [!IMPORTANT]
> Sie sollten diese Ressourcen umgehend löschen, damit sie nicht weiter abgerechnet werden.

Im Wiederherstellungsprozess werden alle Wiederherstellungsressourcen in einer Wiederherstellungsressourcengruppe erstellt. Im Bereinigungsprozess wird diese Ressourcengruppe gelöscht und werden alle Verweise auf die Ressourcen aus dem Katalog entfernt. 

1. Legen Sie in *PowerShell ISE* im Skript „...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1“ Folgendes fest:
    * **$DemoScenario = 6**: Löschen von veralteten Ressourcen aus der Wiederherstellungsregion

1. Drücken Sie **F5** , um das Skript auszuführen.

Nach dem Bereinigen der Ressourcen hat die Anwendung wieder den Zustand, den sie zu Beginn hatte.  An diesem Punkt können Sie das Skript erneut ausführen oder andere Tutorials durcharbeiten.

## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Entwerfen der Anwendung, sodass Anwendung und Datenbank in derselben Region zusammengestellt werden 
Die Anwendung ist so entworfen, dass Verbindungen in ihr immer aus einer Instanz in der Region hergestellt werden, in der sich auch die Mandantendatenbank befindet. Dieser Entwurf verringert die Wartezeit zwischen der Anwendung und der Datenbank. Für diese Optimierung wird davon ausgegangen, dass Interaktionen zwischen der App und der Datenbank wesentlich häufiger sind als Interaktionen zwischen Benutzern und der App.  

Mandantendatenbanken können während einer Rückführung für einige Zeit über die Wiederherstellungs- und die ursprüngliche Region verteilt sein.  Für jede Datenbank sucht die App über eine DNS-Suche zu dem Namen des Mandantenservers nach der Region, in der sich die Datenbank befindet. In SQL-Datenbank ist der Servername ein Alias. Der als Alias vorliegende Servername enthält der Namen der Region. Wenn sich die Anwendung nicht in derselben Region wie die Datenbank befindet, erfolgt eine Umleitung zu der Instanz, die sich in derselben Region wie der Datenbankserver befindet.  Das Umleiten zu der Instanz in derselben Region wie die Datenbank minimiert die Wartezeit zwischen der App und der Datenbank.  

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]

>* Verwenden des Mandantenkatalogs, um in regelmäßigen Abständen aktualisierte Konfigurationsinformationen zu speichern, sodass eine Spiegelimage-Wiederherstellungsumgebung in einer anderen Region erstellt werden kann
>* Verwenden von Geowiederherstellung, um Azure SQL-Datenbanken in der Wiederherstellungsregion wiederherzustellen
>* Aktualisieren des Mandantenkatalogs entsprechend den Speicherorten der wiederhergestellten Mandantendatenbanken  
>* Verwenden eines DNS-Alias, damit eine Anwendung durchgängig Verbindung mit dem Mandantenkatalog hat, ohne dass eine Neukonfiguration erforderlich ist
>* Verwenden von Georeplikation, um wiederhergestellte Datenbanken in ihre ursprüngliche Region zurückzuführen, nachdem ein Ausfall behoben wurde

Führen Sie jetzt die [Notfallwiederherstellung für eine mehrinstanzenfähige SaaS-Anwendung über Datenbankgeoreplikation](saas-dbpertenant-dr-geo-replication.md) aus, um zu erfahren, wie sich durch Verwenden von Georeplikation die Zeit deutlich verkürzen lässt, die zum Wiederherstellen einer umfangreichen mehrinstanzenfähigen Anwendung erforderlich ist.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Zusätzliche Tutorials, die auf der Wingtip-SaaS-Anwendung aufbauen](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)

---
title: "Verwalten eines Schemas für Azure SQL-Datenbank in einer mehrinstanzenfähigen App | Microsoft-Dokumentation"
description: "Verwalten des Schemas für mehrere Mandanten in einer mehrinstanzenfähigen Anwendung, die Azure SQL-Datenbank verwendet"
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: 135764a7d89dcf711ff7fe9416850f1af9329479
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2018
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Verwalten des Schemas für mehrere Mandanten in einer mehrinstanzenfähigen Anwendung, die Azure SQL-Datenbank verwendet

In diesem Tutorial werden die Herausforderungen bei der Verwaltung einer potenziell sehr großen Anzahl von Datenbanken in einer SaaS-Anwendung (Software-as-a-Service) in der Cloud betrachtet. In Demos werden Lösungen zur Verwaltung der Schemaerweiterungen vorgestellt, die während der Lebensdauer einer Anwendung entwickelt und implementiert werden.

Bei der Weiterentwicklung einer Anwendung werden möglicherweise Änderungen an den Tabellenspalten oder an einem anderen Schema, an den zugehörigen Verweisdaten oder an leistungsbezogenen Elementen vorgenommen. Mit einer SaaS-Anwendung müssen diese Änderungen auf koordinierte Weise über zahlreiche vorhandene Mandantendatenbanken hinweg bereitgestellt werden. Zusätzlich müssen diese Änderungen in zukünftigen Mandantendatenbanken enthalten sein, die der Anwendung hinzugefügt werden. Aus diesem Grund müssen die Änderungen auch in den Prozess integriert werden, mit dem neue Datenbanken bereitgestellt werden.

#### <a name="two-scenarios"></a>Zwei Szenarien

In diesem Tutorial werden die folgenden zwei Szenarien untersucht:
- Bereitstellen der Aktualisierungen von Verweisdaten für alle Mandanten
- Retuning eines Index für die Tabelle mit den Verweisdaten

Für die mandantendatenbankübergreifende Ausführung dieser Vorgänge wird das Feature [Elastische Aufträge](sql-database-elastic-jobs-overview.md) von Azure SQL-Datenbank verwendet. Die Aufträge gelten auch für die als Vorlage verwendete Gold-Version der Mandantendatenbanken. Diese Vorlage wird verwendet, wenn neue Datenbanken bereitgestellt werden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Auftragskontos
> * Ausführen von Abfragen über mehrere Mandanten hinweg
> * Aktualisieren von Daten in allen Mandantendatenbanken
> * Erstellen eines Index für eine Tabelle in allen Mandantendatenbanken

## <a name="prerequisites"></a>Voraussetzungen

- Die Wingtip Tickets-App muss bereits bereitgestellt worden sein:
    - Anleitungen hierzu finden Sie im ersten Tutorial, in dem die auf SaaS basierende *Wingtip Tickets*-App für mehrinstanzenfähige Datenbanken vorgestellt wird:<br />[Bereitstellen und Kennenlernen einer mehrinstanzenfähigen Anwendung mit Sharding, die Azure SQL-Datenbank verwendet](saas-multitenantdb-get-started-deploy.md)
        - Der Bereitstellungsprozess dauert weniger als fünf Minuten.
    - Sie müssen die *mehrinstanzenfähige Wingtip-Version mit Sharding* installiert haben. Die Versionen für *Eigenständig* und *Datenbank pro Mandant* unterstützen das aktuelle Tutorial nicht.

- Die aktuelle Version von SQL Server Management Studio (SSMS) muss installiert sein. [Laden Sie SSMS herunter, und installieren Sie es](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Azure PowerShell muss installiert sein. Ausführliche Informationen finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> In diesem Tutorial werden Funktionen des Azure SQL-Datenbank-Diensts verwendet, die als eingeschränkte Vorschauversion vorliegen ([Aufträge für die elastische Datenbank](sql-database-elastic-database-client-library.md)). Wenn Sie dieses Tutorial durcharbeiten möchten, geben Sie Ihre Abonnement-ID für *SaaSFeedback@microsoft.com* mit dem Betreff „subject=Elastic Jobs Preview“ an. Wenn Sie die Bestätigung erhalten haben, dass die Aktivierung für Ihr Abonnement ausgeführt wurde, [laden Sie die aktuellen Vorabversion-Cmdlets für Aufträge herunter und installieren Sie sie](https://github.com/jaredmoo/azure-powershell/releases). Die Vorschauversion ist eingeschränkt, wenden Sie sich daher an *SaaSFeedback@microsoft.com*, wenn Sie Fragen haben oder Support benötigen.

## <a name="introduction-to-saas-schema-management-patterns"></a>Einführung in SaaS-Schemaverwaltungsmuster

Das in diesem Beispiel verwendete mehrinstanzenfähige Datenbankmodell mit Sharding ermöglicht, dass eine Mandantendatenbank einen oder mehrere Mandanten enthält. In diesem Beispiel wird untersucht, wie Sie eine Mischung aus einer Datenbank mit mehreren Mandanten und einer Datenbank mit nur einem Mandanten und damit ein *hybrides* Mandantenverwaltungsmodell verwenden können. Die Verwaltung dieser Datenbanken ist kompliziert. [Elastische Aufträge](sql-database-elastic-jobs-overview.md) erleichtert die Verwaltung der SQL-Datenebene. Aufträge ermöglichen es Ihnen, auf sichere und zuverlässige Weise Transact-SQL-Skripts als Aufgaben für eine Gruppe von Mandantendatenbanken auszuführen. Die Aufgaben sind unabhängig von Benutzerinteraktionen oder Eingaben. Mithilfe dieser Methode können Änderungen an Schemadaten oder an gemeinsamen Verweisdaten für alle Mandanten in einer Anwendung bereitgestellt werden. Elastische Aufträge können auch verwendet werden, um eine Gold-Version der Datenbank als Vorlage zu verwalten. Diese Vorlage dient zum Erstellen neuer Mandanten, wodurch immer sichergestellt wird, das die neuesten Schema- und Verweisdaten verwendet werden.

![Bildschirm](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Eingeschränkte Vorschau – Elastische Aufträge

Es gibt eine neue Version von „Elastische Aufträge“, die nun eine integrierte Funktion von Azure SQL-Datenbank darstellt. „Integriert“ bedeutet, dass keine weiteren Dienste oder Komponenten erforderlich sind. Diese neue Version von Elastische Aufträge liegt derzeit als eingeschränkte Vorschauversion vor. Die eingeschränkte Vorschauversion unterstützt derzeit PowerShell zum Erstellen von Auftragskonten sowie T-SQL zum Erstellen und Verwalten von Aufträgen.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Abrufen von Quellcode und Skripts zur Anwendung Wingtip Tickets SaaS Multi-tenant Database

Die Skripts und der Quellcode der mehrinstanzenfähigen Wingtip Tickets-SaaS-Datenbank stehen im Repository [WingtipTicketsSaaS-MultiTenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) auf GitHub zur Verfügung. Schritte zum Herunterladen und Entsperren der Wingtip Tickets-SaaS-Skripts finden Sie unter [General guidance for working with Wingtip Tickets sample SaaS apps](saas-tenancy-wingtip-app-guidance-tips.md) (Allgemeine Hinweise zur Verwendung von Wingtip Tickets-Beispiel-SaaS-Apps). 

## <a name="create-a-job-account-database-and-new-job-account"></a>Erstellen einer Auftragskonto-Datenbank und eines neuen Auftragskontos

Für dieses Tutorial müssen Sie mit PowerShell die Auftragskonto-Datenbank und das Auftragskonto erstellen. Wie auch die von SQL Agent verwendete MSDB-Datenbank verwendet „Elastische Aufträge“ eine Azure SQL-Datenbank, um Auftragsdefinitionen, den Auftragsstatus und den Auftragsverlauf zu speichern. Sobald das Auftragskonto erstellt wurde, können Sie sofort Aufträge erstellen und überwachen.

1. Öffnen Sie *…\\Learning Modules\\Schema Management\\Demo-SchemaManagement.ps1* in der **PowerShell ISE**.
2. Drücken Sie **F5** , um das Skript auszuführen.

Das Skript *Demo-SchemaManagement.ps1* ruft das Skript *Deploy-SchemaManagement.ps1* auf, um eine Tier-*S2*-Datenbank mit dem Namen **jobaccount** auf dem Katalogserver zu erstellen. Anschließend erstellt das Skript das Auftragskonto, und die Datenbank „jobaccount“ wird als Parameter an den Aufruf zur Auftragskontoerstellung übergeben.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Erstellen eines Auftrags, um neue Verweisdaten für alle Mandanten bereitzustellen

#### <a name="prepare"></a>Vorbereiten

Jede Mandantendatenbank enthält eine Gruppe von Veranstaltungsorttypen in der Tabelle **VenueTypes**. Diese definieren die Art von Ereignissen, die an einem Veranstaltungsort präsentiert werden. In dieser Übung stellen Sie eine Aktualisierung für alle Datenbanken bereit, wobei zwei weitere Veranstaltungsorttypen hinzugefügt werden: *Motorcycle Racing* und *Swimming Club*. Diese Veranstaltungsorttypen entsprechen dem Hintergrundbild, das in der Mandantenereignis-App angezeigt wird.

Bevor Sie die neuen Verweisdaten bereitstellen, ermitteln Sie die Anzahl der Veranstaltungsorttypen, die bereits vorhanden sind. Es sollten 10 sein. Klicken Sie dazu auf den folgenden Link zur Wingtip-Webbenutzeroberfläche, und klicken Sie dann auf das Dropdownmenü **Venue Type** (Veranstaltungsorttyp):
- http://events.wingtip-mt.<USER>.trafficmanager.net

Nun können Sie zählen, wie viele Veranstaltungsorttypen es ursprünglich gibt. Beachten Sie besonders, dass bisher weder *Motorcycle Racing* noch *Swimming Club* vorhanden ist.

#### <a name="steps"></a>Schritte

Nun erstellen Sie einen Auftrag zum Aktualisieren der Tabelle **VenueTypes** (Veranstaltungsorttypen) in jeder Mandantendatenbank durch Hinzufügen der beiden neuen Veranstaltungsorttypen.

Zum Erstellen eines neuen Auftrags verwenden Sie die Gruppe von gespeicherten Systemprozeduren für Aufträge, die in der Datenbank *jobaccount* erstellt wurden. Die Prozeduren wurden beim Erstellen des Auftragskontos erstellt.

1. Stellen Sie in SSMS eine Verbindung mit dem Mandantenserver her: tenants1-mt-\<Benutzer\>.database.windows.net

2. Wechseln Sie zur Datenbank *tenants1* auf dem Server *tenants1-mt-\<Benutzer\>.database.windows.net*.

3. Fragen Sie die Tabelle *VenueTypes* ab, um sich zu überzeugen, dass *Motorcycle Racing* und *Swimming Club* noch nicht in der Ergebnisliste erscheinen.

4. Stellen Sie eine Verbindung mit dem Katalogserver her. Dieser lautet *catalog-mt-\<Benutzer\>.database.windows.net*.

5. Stellen Sie eine Verbindung mit der Datenbank *jobaccount* im Katalogserver her.

6. Öffnen Sie in SSMS die Datei *…\\Learning Modules\\Schema Management\\DeployReferenceData.sql*.

7. Ändern Sie die Anweisung „set @User = &lt;Benutzer&gt;“, und ersetzen Sie den Wert „Benutzer“ durch den Benutzer, der beim Bereitstellen der mehrinstanzenfähigen Wingtip Tickets-SaaS-Datenbankanwendung verwendet wurde.

8. Drücken Sie **F5** , um das Skript auszuführen.

#### <a name="observe"></a>Beobachen

Beachten Sie die folgenden Elemente im Skript *DeployReferenceData.sql*:

- **sp\_add\_target\_group** erstellt den Zielgruppennamen *DemoServerGroup* und fügt Zielmember zur Gruppe hinzu.

- **sp\_add\_target\_group\_member** fügt die folgenden Elemente hinzu:
    - Einen Zielmembertyp *server*
        - Dies ist der Server *tenants1-mt-&lt;Benutzer&gt;*, der die Mandantendatenbanken enthält.
        - Somit werden bei der Ausführung des Auftrags alle Datenbanken auf dem Server einbezogen.
    - Einen Zielmembertyp *database* für die Gold-Version der Datenbank (*basetenantdb*), die sich auf dem Server *catalog-mt-&lt;Benutzer&gt;* befindet
    - Einen Zielmembertyp *database*, der die in einem späteren Tutorial verwendete Datenbank *adhocreporting* enthält

- **sp\_add\_job** erstellt einen Auftrag mit dem Namen *Reference Data Deployment* (Verweisdatenbereitstellung).

- **sp\_add\_jobstep** erstellt den Auftragsschritt mit dem T-SQL-Befehlstext zum Aktualisieren der Verweistabelle „VenueTypes“.

- Die übrigen Ansichten im Skript zeigen das Vorhandensein der Objekte an und überwachen die Auftragsausführung. Verwenden Sie diese Abfragen, um den Statuswert in der **lifecycle**-Spalte zu überprüfen und zu ermitteln, wann der Auftrag abgeschlossen wurde. Der Auftrag aktualisiert die Mandantendatenbank und die zwei weiteren Datenbanken mit der Verweistabelle.

Wechseln Sie in SSMS zur Mandantendatenbank auf dem Server *tenants1-mt-&lt;Benutzer&gt;*. Fragen Sie die Tabelle *VenueTypes* ab, um sich zu überzeugen, dass *Motorcycle Racing* und *Swimming Club* jetzt der Tabelle hinzugefügt wurden. Die Gesamtzahl der Veranstaltungsorttypen sollte um zwei gestiegen sein.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Erstellen eines Auftrags zum Verwalten des Index der Verweistabelle

Diese Übung ähnelt der vorherigen Übung. In dieser Übung wird ein Auftrag erstellt, um den Index für den Primärschlüssel der Verweistabelle neu zu erstellen. Eine Indexneuerstellung ist ein typischer Datenbank-Verwaltungsvorgang, den Administratoren nach dem Laden großer Datenmengen in eine Tabelle ausführen, um die Leistung zu verbessern.

1. Stellen Sie in SSMS eine Verbindung mit der Datenbank *jobaccount* auf dem Server *catalog-mt-&lt;Benutzer&gt;.database.windows.net* her.

2. Öffnen Sie die Datei *…\\Learning Modules\\Schema Management\\OnlineReindex.sql* in SSMS.

3. Drücken Sie **F5** , um das Skript auszuführen.

#### <a name="observe"></a>Beobachen

Beachten Sie folgende Elemente im Skript *OnlineReindex.sql*:

* **sp\_add\_job** erstellt einen neuen Auftrag mit dem Namen *Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885*.

* **sp\_add\_jobstep** erstellt den Auftragsschritt mit dem T-SQL-Befehlstext zum Aktualisieren des Index.

* Die verbleibenden Ansichten im Skript überwachen die Auftragsausführung. Verwenden Sie diese Abfragen, um den Statuswert in der **lifecycle**-Spalte zu überprüfen und zu ermitteln, wann der Auftrag für alle Zielgruppenelemente erfolgreich abgeschlossen wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Verwalten horizontal hochskalierter Clouddatenbanken](sql-database-elastic-jobs-overview.md)
* [Erstellen und Verwalten von horizontal hochskalierten Clouddatenbanken](sql-database-elastic-jobs-create-and-manage.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> - Erstellen eines Auftragskontos zum Abfragen mehrerer Mandanten
> - Aktualisieren von Daten in allen Mandantendatenbanken
> - Erstellen eines Index für eine Tabelle in allen Mandantendatenbanken

Probieren Sie danach das Tutorial [Ausführen von Ad-hoc-Analyseabfragen für mehrere Azure SQL-Datenbanken](saas-multitenantdb-adhoc-reporting.md) aus.


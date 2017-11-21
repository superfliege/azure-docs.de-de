---
title: "Verwalten eines Schemas für Azure SQL-Datenbank in einer mehrinstanzenfähigen App | Microsoft-Dokumentation"
description: "Verwalten des Schemas für mehrere Mandanten in einer mehrinstanzenfähigen Anwendung, die Azure SQL-Datenbank verwendet"
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: billgib
ms.openlocfilehash: 346177be29ec196464f4f441858222ac5d5eb8c3
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Verwalten des Schemas für mehrere Mandanten in einer mehrinstanzenfähigen Anwendung, die Azure SQL-Datenbank verwendet

Im [ersten Tutorial zur mehrinstanzenfähigen Wingtip Tickets-SaaS-Datenbank](saas-multitenantdb-get-started-deploy.md) wird veranschaulicht, wie die App eine mehrinstanzenfähige Datenbank mit Sharding bereitstellen und im Katalog registrieren kann. Wie jede andere Anwendung wird die Wingtip Tickets-SaaS-App laufend weiterentwickelt, und gelegentlich sind Änderungen an der Datenbank erforderlich. Derartige Änderungen können ein neues oder geändertes Schema, neue oder geänderte Verweisdaten und routinemäßige Wartungsaufgaben sein, die eine optimale Leistung der App sicherstellen sollen. Bei einer SaaS-Anwendung müssen diese Änderungen u.U. koordiniert für eine große Anzahl von Mandantendatenbanken bereitgestellt werden. Damit diese Änderungen in künftigen Mandantendatenbanken angewendet werden, müssen sie in den Bereitstellungsprozess eingeschlossen werden.

In diesem Tutorial werden zwei Szenarien erläutert: das Bereitstellen der Aktualisierungen von Verweisdaten für alle Mandanten und das Retuning eines Index für die Tabelle mit den Referenzdaten. Mit der Funktion [Elastische Aufträge](sql-database-elastic-jobs-overview.md) werden diese Vorgänge für Mandantendatenbanken durchgeführt, sowie auch für die *goldene* Mandantendatenbank, die als Vorlage für neue Datenbanken verwendet wird.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]

> * Erstellen eines Auftragskontos
> * Ausführen von Abfragen über mehrere Mandanten hinweg
> * Aktualisieren von Daten in allen Mandantendatenbanken
> * Erstellen eines Index für eine Tabelle in allen Mandantendatenbanken


Stellen Sie vor dem Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die mehrinstanzenfähige Wingtip Tickets-SaaS-Datenbankanwendung wurde bereitgestellt. Unter [Bereitstellen und Kennenlernen der App Wingtip Tickets SaaS Multi-tenant Database](saas-multitenantdb-get-started-deploy.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Die aktuelle Version von SQL Server Management Studio (SSMS) wurde installiert. [Herunterladen und Installieren von SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> *In diesem Tutorial werden Funktionen des SQL-Datenbank-Diensts verwendet, die als eingeschränkte Vorschauversion vorliegen (Elastische Datenbankaufträge). Wenn Sie dieses Tutorial durcharbeiten möchten, geben Sie Ihre Abonnement-ID für SaaSFeedback@microsoft.com mit dem Betreff „subject=Elastic Jobs Preview“ an. Wenn Sie die Bestätigung erhalten haben, dass die Aktivierung für Ihr Abonnement ausgeführt wurde, [laden Sie die aktuellen Vorabversion-Cmdlets für Aufträge herunter und installieren Sie sie](https://github.com/jaredmoo/azure-powershell/releases). Die Vorschauversion ist eingeschränkt, wenden Sie sich daher an SaaSFeedback@microsoft.com, wenn Sie Fragen haben oder Support benötigen.*


## <a name="introduction-to-saas-schema-management-patterns"></a>Einführung in SaaS-Schemaverwaltungsmuster

Das in diesem Beispiel verwendete mehrinstanzenfähige Datenbankmodell mit Sharding ermöglicht, dass eine Mandantendatenbank eine beliebige Anzahl von Mandanten enthält. In diesem Beispiel wird untersucht, wie Sie eine Mischung aus einer Datenbank mit mehreren Mandanten und einer Datenbank mit nur einem Mandanten und damit ein „hybrides“ Mandantenverwaltungsmodell verwenden können. Die Wartung und Verwaltung dieser Datenbanken ist kompliziert. [Elastische Aufträge](sql-database-elastic-jobs-overview.md) erleichtert die Verwaltung der SQL-Datenebene. Mit Aufträgen können Sie Aufgaben (T-SQL-Skripts) auf sichere und zuverlässige Weise unabhängig von Benutzeraktionen oder -eingaben für eine Gruppe von Datenbanken ausführen. Mithilfe dieser Methode können Änderungen an Schemadaten und gemeinsamen Verweisdaten für alle Mandanten in einer Anwendung bereitgestellt werden. Mit Elastische Aufträge kann auch eine *goldene* Kopie der Datenbank gepflegt werden, anhand derer neue Mandanten erstellt werden. Dabei wird sichergestellt, dass sie stets die aktuellen Schema- und Verweisdaten aufweist.

![Bildschirm](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Eingeschränkte Vorschau – Elastische Aufträge

Es gibt eine neue Version von Elastische Aufträge, die nun eine integrierte Funktion von Azure SQL-Datenbank darstellt (und keine weiteren Dienste und Komponenten erfordert). Diese neue Version von Elastische Aufträge liegt derzeit als eingeschränkte Vorschauversion vor. Die eingeschränkte Vorschauversion unterstützt derzeit PowerShell zum Erstellen von Auftragskonten sowie T-SQL zum Erstellen und Verwalten von Aufträgen.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-scripts"></a>Abrufen der Skripts für die mehrinstanzenfähige Wingtip Tickets-SaaS-Datenbankanwendung

Die Skripts und der Quellcode der mehrinstanzenfähigen Wingtip Tickets-SaaS-Datenbank stehen im GitHub-Repository [WingtipTicketsSaaS-MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) zur Verfügung. <!-- [Steps to download the Wingtip Tickets SaaS Multi-tenant Database scripts](saas-multitenantdb-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts)-->

## <a name="create-a-job-account-database-and-new-job-account"></a>Erstellen einer Auftragskonto-Datenbank und eines neuen Auftragskontos

Für dieses Tutorial müssen Sie mit PowerShell die Auftragskonto-Datenbank und das Auftragskonto erstellen. Wie auch MSDB und SQL-Agent verwendet Elastische Aufträge eine Azure SQL-Datenbank, um Auftragsdefinitionen, den Auftragsstatus und den Auftragsverlauf zu speichern. Wenn das Auftragskonto erstellt wurde, können Sie sofort Aufträge erstellen und überwachen.

1. Öffnen Sie „…*Learning Modules\\Schema Management\\\\Demo-SchemaManagement.ps1*“ in der **PowerShell ISE**.
1. Drücken Sie **F5** , um das Skript auszuführen.

Das Skript *Demo-SchemaManagement.ps1* ruft das Skript *Deploy-SchemaManagement.ps1* auf, um eine *S2*-Datenbank mit dem Namen **jobaccount** auf dem Katalogserver zu erstellen. Anschließend erstellt es das Auftragskonto, und die Datenbank „jobaccount“ wird als Parameter an den Aufruf zur Auftragskontoerstellung übergeben.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Erstellen eines Auftrags, um neue Verweisdaten für alle Mandanten bereitzustellen

Jede Mandantendatenbank enthält eine Gruppe von Veranstaltungsorttypen in der Tabelle **VenueTypes** (Veranstaltungsorttypen), die die Art von Ereignissen definieren, die an einem Veranstaltungsort präsentiert werden. In dieser Übung stellen Sie eine Aktualisierung für alle Datenbanken bereit, wobei zwei weitere Veranstaltungsorttypen hinzugefügt werden: *Motorcycle Racing* und *Swimming Club*. Diese Veranstaltungsorttypen entsprechen dem Hintergrundbild, das in der Mandantenereignis-App angezeigt wird.

Klicken Sie auf das Dropdownmenü „Venue Type“ (Veranstaltungsorttyp), und vergewissern Sie sich, dass nur zehn Einträge für Veranstaltungsorttypen verfügbar sind. Achten Sie insbesondere darauf, dass „Motorcycle Racing“ und „Swimming Club“ nicht in der Liste enthalten sind.

Nun erstellen wir einen Auftrag zum Aktualisieren der Tabelle **VenueTypes** (Veranstaltungsorttypen) in allen Mandantendatenbanken und fügen die neuen Veranstaltungsorttypen hinzu.

Zum Erstellen eines neuen Auftrags verwenden wir eine Gruppe von gespeicherten Systemprozeduren für Aufträge, die beim Erstellen des Auftragskontos in der Datenbank „jobaccount“ erstellt wurden.

1. Stellen Sie in SSMS eine Verbindung mit dem Mandantenserver her: tenants1-mt-\<Benutzer\>.database.windows.net
2. Navigieren Sie zur Datenbank *tenants1* auf dem Server *tenants1-mt-\<Benutzer\>.database.windows.net*, und fragen Sie die Tabelle *VenueTypes* ab, um sich zu vergewissern, dass *Motorcycle Racing* und *Swimming Club* **nicht** in der Ergebnisliste enthalten sind.
3. Stellen Sie eine Verbindung mit dem Katalogserver her: catalog-mt-\<Benutzer\>.database.windows.net
4. Stellen Sie eine Verbindung mit der Datenbank „jobaccount“ im Katalogserver her.
5. Öffnen Sie in SSMS die Datei „…\\Learning Modules\\Schema Management\\DeployReferenceData.sql“.
6. Ändern Sie die Anweisung „set @User = &lt;Benutzer&gt;“, und ersetzen Sie den Wert „Benutzer“ durch den Benutzer, der beim Bereitstellen der mehrinstanzenfähigen Wingtip Tickets-SaaS-Datenbankanwendung verwendet wurde.
7. Drücken Sie **F5** , um das Skript auszuführen.

    * **sp\_add\_target\_group** erstellt den Zielgruppennamen „DemoServerGroup“, und nun müssen Zielelemente zur Gruppe hinzugefügt werden.
    * **sp\_add\_target\_group\_member** fügt einen *server*-Zielelementtyp hinzu, der vorsieht, dass alle Datenbanken auf diesem Server (beachten Sie, dass dies der Server „tenants1-mt-&lt;Benutzer&gt;“ mit den Mandantendatenbanken ist) bei der Auftragsausführung in den Auftrag eingeschlossen werden müssen, ein *database*-Zielelementtyp für die „goldene“ Datenbank („basetenantdb“), die sich auf dem Server „catalog-mt-&lt;Benutzer&gt;“ befindet, und schließlich ein *database*-Zielelementtyp, der die Datenbank „adhocreporting“ enthält, welche in einem späteren Tutorial verwendet wird.
    * **sp\_add\_job** erstellt einen Auftrag mit dem Namen „Reference Data Deployment“ (Verweisdatenbereitstellung).
    * **sp\_add\_jobstep** erstellt den Auftragsschritt mit dem T-SQL-Befehlstext zum Aktualisieren der Verweistabelle „VenueTypes“.
    * Die übrigen Ansichten im Skript zeigen das Vorhandensein der Objekte an und überwachen die Auftragsausführung. Verwenden Sie diese Abfragen, um den Statuswert in der **lifecycle**-Spalte zu überprüfen und zu ermitteln, wann der Auftrag in der Mandantendatenbank und den beiden zusätzlichen Datenbanken mit der Verweistabelle erfolgreich abgeschlossen wurde.

1. Navigieren Sie in SSMS zur Mandantendatenbank auf dem Server *tenants1-mt-&lt;Benutzer&gt;*, und fragen Sie die Tabelle *VenueTypes* ab, um sich zu vergewissern, dass *Motorcycle Racing* und *Swimming Club* jetzt zur Tabelle **hinzugefügt* wurden.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Erstellen eines Auftrags zum Verwalten des Index der Verweistabelle

Ähnlich wie in der vorherigen Übung wird in dieser ein Auftrag erstellt, um den Index für den Verweistabellen-Primärschlüssel neu zu erstellen. Dies ist ein typischer Datenbank-Verwaltungsvorgang, den Administratoren nach dem Laden großer Datenmengen in eine Tabelle ausführen.


1. Stellen Sie in SSMS eine Verbindung mit der Datenbank „jobaccount“ auf dem Server „catalog-mt-&lt;Benutzer&gt;.database.windows.net“ her.
2. Öffnen Sie die Datei „…\\Learning Modules\\Schema Management\\OnlineReindex.sql“ in SSMS.
3. Drücken Sie **F5**, um das Skript auszuführen.

    * **sp\_add\_job** erstellt einen neuen Auftrag mit dem Namen „Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885“.
    * **sp\_add\_jobstep** erstellt den Auftragsschritt mit dem T-SQL-Befehlstext zum Aktualisieren des Index.
    * Die verbleibenden Ansichten im Skript überwachen die Auftragsausführung. Verwenden Sie diese Abfragen, um den Statuswert in der **lifecycle**-Spalte zu überprüfen und zu ermitteln, wann der Auftrag für alle Zielgruppenelemente erfolgreich abgeschlossen wurde.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Erstellen eines Auftragskontos zum Abfragen mehrerer Mandanten
> * Aktualisieren von Daten in allen Mandantendatenbanken
> * Erstellen eines Index für eine Tabelle in allen Mandantendatenbanken

[Tutorial zu Ad-hoc-Analysen](saas-multitenantdb-adhoc-reporting.md)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

<!--* Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Verwalten horizontal hochskalierter Clouddatenbanken](sql-database-elastic-jobs-overview.md)
* [Erstellen und Verwalten von horizontal hochskalierten Clouddatenbanken](sql-database-elastic-jobs-create-and-manage.md)

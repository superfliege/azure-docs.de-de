---
title: "Ausführen von Ad-hoc-Berichterstellungsabfragen für mehrere Azure SQL-Datenbanken | Microsoft-Dokumentation"
description: "Ausführen von Ad-hoc-Berichterstellungsabfragen für mehrere SQL-Datenbanken in einem mehrinstanzenfähigen App-Beispiel."
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
ms.date: 11/13/2017
ms.author: AyoOlubeko
ms.openlocfilehash: c85dec1023e4d4f0a14dfbc249850b6dc6e78edf
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Ausführen von Ad-hoc-Analyseabfragen für mehrere Azure SQL-Datenbanken

In diesem Tutorial führen Sie auf mehrere Mandantendatenbanken verteilte Abfragen aus, um eine interaktive Ad-hoc-Berichterstellung zu ermöglichen. Diese Abfragen können Einblicke geben, die tief in den Regelbetriebsdaten der Wingtip Tickets-SaaS-App verborgen sind. Zur Durchführung dieser Datenextraktionen stellen Sie eine weitere Analysedatenbank auf dem Katalogserver bereit und erlauben verteilte Abfragen mithilfe von Elastic Query.


In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]

> * Bereitstellen einer Ad-hoc-Berichterstellungsdatenbank
> * Ausführen verteilter Abfragen für alle Mandantendatenbanken


Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die App Wingtip Tickets SaaS Multi-tenant Database wird bereitgestellt. Unter [Bereitstellen und Kennenlernen der App Wingtip Tickets SaaS Multi-tenant Database](saas-multitenantdb-get-started-deploy.md) finden Sie Informationen dazu, wie Sie diese App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* SQL Server Management Studio (SSMS) muss installiert sein. Sie können SSMS unter [Herunterladen von SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) herunterladen und installieren.


## <a name="ad-hoc-reporting-pattern"></a>Ad-hoc-Berichterstellungsmuster

![Ad-hoc-Berichterstellungsmuster](media/saas-multitenantdb-adhoc-reporting/adhocreportingpattern_shardedmultitenantDB.png)

SaaS-Anwendungen können die gewaltige Menge von Mandantendaten analysieren, die zentral in der Cloud gespeichert sind. Diese Daten vermitteln Einblicke in Betrieb und Nutzung Ihrer Anwendung. An diesen gewonnenen Erkenntnissen können Sie sich bei der Entwicklung von Funktionen, Verbesserungen der Benutzerfreundlichkeit und anderen Investitionen in Ihre Apps und Dienste orientieren.

Es ist einfach, auf diese Daten in einer Datenbank mit mehreren Mandanten zuzugreifen, aber weniger einfach bei einer Verteilung über potentiell Tausende von Datenbanken. Ein Ansatz besteht in der Verwendung von [Elastic Query](sql-database-elastic-query-overview.md), wodurch Abfragen für einen verteilten Satz von Datenbanken mit gemeinsamem Schema ermöglicht werden. Diese Datenbanken können auf verschiedene Ressourcengruppen und Abonnements verteilt werden. Allerdings muss ein Anmeldename vorhanden sein, der allgemeinen Zugriff hat, um Daten aus allen Datenbanken zu extrahieren. Flexible Abfragen verwenden eine einzige *Hauptdatenbank*, in der externe Tabellen definiert werden, die Tabellen oder Sichten in den verteilten Datenbanken (Mandantendatenbanken) spiegeln. Abfragen, die an diese Hauptdatenbank übermittelt werden, werden kompiliert, um einen Plan der verteilten Abfrage zu erzeugen, bei dem Teile der Abfrage nach Bedarf per Push an die Mandantendatenbanken übertragen werden. Elastic Query verwendet die Shardzuordnung in der Katalogdatenbank, um den Speicherort aller Mandantendatenbanken zu bestimmen. Für Einrichtung und Abfragen wird einfach gewöhnlicher [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)-Code verwendet, und Ad-hoc-Abfragen von Tools wie Power BI und Excel werden unterstützt.

Durch das Verteilen von Abfragen über Mandantendatenbanken bietet Elastic Query schnellen Einblick in Liveproduktionsdaten. Dadurch dass es möglich ist, das Elastic Query Daten aus vielen Datenbanken abruft, kann die Abfragewartezeit manchmal länger sein als für äquivalente Abfragen, die an eine einzelne Datenbank mit mehreren Mandanten ausgestellt werden. Achten Sie darauf, Ihre Abfragen so zu entwerfen, dass ein Minimum an Daten zurückgegeben wird. Elastic Query ist häufig optimal für das Abfragen kleiner Mengen von Echtzeitdaten und weniger für das Erstellen von häufig verwendeten oder komplexen Analyseabfragen oder -berichten. Wenn Abfragen nicht optimal ausgeführt werden, können Sie dem [Ausführungsplan](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) entnehmen, welcher Teil der Abfrage per Push in die Remotedatenbank übertragen wurde. Bewerten Sie außerdem, wie viele Daten zurückgegeben werden. Für Abfragen, die eine komplexe analytische Verarbeitung erfordern, ist es möglicherweise besser, wenn die extrahierten Mandantendaten in einer Datenbank gespeichert werden, die für Analyseabfragen optimiert ist. SQL-Datenbank und SQL Data Warehouse könnten eine solche Analysedatenbank hosten.

<!-- ?? This pattern for analytics is explained in the [tenant analytics tutorial](saas-multitenantdb-tenant-analytics.md).
-->

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-scripts"></a>Abrufen der Skripts für die Anwendung Wingtip Tickets SaaS Multi-tenant Database

Die Skripts und der Quellcode der Anwendung Wingtip Tickets SaaS Multi-tenant Database stehen im GitHub-Repository [WingtipTicketsSaaS-MultitenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) zur Verfügung. Befolgen Sie dabei in jedem Fall die Schritte zur Entsperrung, wie Sie in der Infodatei beschrieben sind.

## <a name="create-ticket-sales-data"></a>Erstellen von Ticketverkaufsdaten

Um Abfragen mit einem interessanteren Dataset auszuführen, erstellen Sie Ticketverkaufsdaten mit dem Ticketgenerator.

1. Öffnen Sie nun \\Learning Modules\\Operational Analytics\\Adhoc Reporting\\*Demo-AdhocReporting.ps1* in der *PowerShell ISE*, und legen Sie folgende Werte fest:
   * **$DemoScenario** = 1, **Tickets für Events an allen Veranstaltungsorten kaufen**.
2. Drücken Sie **F5**, um das Skript auszuführen und Ticketverkaufsdaten zu generieren. Während das Skript ausgeführt wird, können Sie die nächsten Schritte des Tutorials ausführen. Sie können die Ticketdaten im Abschnitt *Ausführen von verteilten Ad-hoc-Abfragen* abfragen. Warten Sie zunächst, bis der Ticketgenerator abgeschlossen wurde.

## <a name="explore-the-tenant-tables"></a>Untersuchen der Mandantentabellen 

In der Anwendung Wingtip Tickets SaaS Multi-tenant Database sind Mandanten in einem hybriden Mandantenverwaltungsmodell gespeichert. Die Mandantendaten werden dabei entweder in einer Datenbank mit mehreren Mandanten oder aber in einer Einzelmandanten-Datenbank gespeichert und können zwischen diesen beiden verschoben werden. Wenn Sie alle Mandantendatenbanken abfragen, ist darauf zu achten, dass Elastic Query die Daten so behandeln kann, als seien sie Teil einer einzigen logischen Datenbank, die vom Mandanten freigegeben wurde. 

Um dieses Muster umzusetzen, enthalten alle Mandantentabellen eine *VenueId*-Spalte, die angibt, zu welchem Mandanten die Daten gehören. *VenueId* wird als Hash des Namens des Veranstaltungsorts berechnet. Für die Einführung eines eindeutigen Werts für diese Spalte kann jedoch ein beliebiger Ansatz verwendet werden. Dieser Ansatz ähnelt der Berechnung des Mandantenschlüssels zur Verwendung im Katalog. Tabellen mit *VenueId* werden von Elastic Query zum Parallelisieren von Abfragen und zu deren Übertragung an die jeweilige Remote-Mandantendatenbank verwendet. Dadurch wird die Menge der zurückgegebenen Daten erheblich reduziert, was insbesondere dann zu einem Leistungsanstieg führt, wenn mehrere Mandanten vorhanden sind, deren Daten in einzelnen Mandantendatenbanken gespeichert werden.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Bereitstellen der für verteilte Ad-hoc-Abfragen verwendeten Datenbank

In dieser Übung wird die Datenbank *adhocreporting* bereitgestellt. Dies ist die Hauptdatenbank, die das Schema enthält, das zum Abfragen aller Mandantendatenbanken verwendet wird. Die Datenbank wird auf dem vorhandenen Katalogserver bereitgestellt. Dies ist der Server, der für alle verwaltungsbezogenen Datenbanken in der Beispielanwendung verwendet wird.

1. Öffnen Sie ...\\Learning Modules\\Operational Analytics\\Adhoc Reporting\\*Demo-AdhocReporting.ps1* in der *PowerShell ISE*, und legen Sie folgende Werte fest:
   * **$DemoScenario** = 2, **Ad-hoc-Analysedatenbank bereitstellen**.

2. Drücken Sie **F5**, um das Skript auszuführen und die Datenbank *adhocreporting* zu erstellen.

Im nächsten Abschnitt fügen Sie das Schema in der Datenbank hinzu, damit es zum Ausführen von verteilten Abfragen verwendet werden kann.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>Konfigurieren der Datenbank „head“ für verteilte Abfragen

In dieser Übung wird ein Schema (die externe Datenquelle und die externen Tabellendefinitionen) in die Ad-hoc-Berichterstellungs-Datenbank eingefügt, wodurch die Abfrage aller Mandantendatenbanken ermöglicht wird.

1. Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit der Ad-hoc-Berichterstellungs-Datenbank her, die Sie im vorherigen Schritt erstellt haben. Der Name der Datenbank lautet *adhocreporting*.
2. Öffnen Sie ...\Learning Modules\Operational Analytics\Adhoc Reporting\ *Initialize-AdhocReportingDB.sql* in SSMS.
3. Prüfen Sie das SQL-Skript, und achten Sie auf Folgendes:

   Für die elastische Abfrage werden datenbankbezogene Anmeldeinformationen verwendet, um auf die einzelnen Mandantendatenbanken zuzugreifen. Diese Anmeldeinformationen müssen in allen Datenbanken verfügbar sein, und im Normalfall sollten die Rechte gewährt sein, die zum Aktivieren dieser Ad-hoc-Abfragen mindestens erforderlich sind.

    ![erstellen von anmeldeinformationen](media/saas-multitenantdb-adhoc-reporting/create-credential.png)

   Die externe Datenquelle, die für die Verwendung der Mandanten-Shardzuordnung in der Katalogdatenbank definiert ist. Indem sie als externe Datenquelle verwendet wird, werden Abfragen auf alle Datenbanken verteilt, die bei Ausgabe der Abfrage im Katalog registriert sind. Da sich Servernamen bei jeder Bereitstellung unterscheiden, ruft dieses Initialisierungsskript den Speicherort der Katalogdatenbank ab, indem es den aktuellen Server (@@servername) abruft, auf dem das Skript ausgeführt wird.

    ![erstellen einer externen datenquelle](media/saas-multitenantdb-adhoc-reporting/create-external-data-source.png)

   Die externen Tabellen, die auf die Mandantentabellen verweisen, werden mit **DISTRIBUTION = SHARDED(VenueId)** definiert. Hierdurch wird eine Abfrage für eine bestimmte *VenueId* an die entsprechende Datenbank übermittelt. Wie wir im nächsten Abschnitt sehen werden, führt dies in vielen Szenarien zu einer Leistungssteigerung.

    ![erstellen externer tabellen](media/saas-multitenantdb-adhoc-reporting/external-tables.png)

   Die lokale Tabelle *VenueTypes*, die erstellt und mit Daten gefüllt wird. Diese Verweisdatentabelle kommt in allen Mandantendatenbanken vor. Daher kann sie hier als lokale Tabelle dargestellt werden, die mit den gemeinsamen Daten aufgefüllt ist. Für einige Abfragen kann dies die Menge der zwischen den Mandantendatenbanken und der Datenbank *adhocreporting* bewegten Daten verringern.

    ![tabelle erstellen](media/saas-multitenantdb-adhoc-reporting/create-table.png)

   Wenn Sie so Verweistabellen einbeziehen, achten Sie darauf, dass Sie das Tabellenschema und die Tabellendaten aktualisieren, immer wenn Sie die Mandantendatenbank aktualisieren.

4. Drücken Sie **F5**, um das Skript auszuführen und die Datenbank *adhocreporting* zu initialisieren. 

Jetzt können Sie verteilte Abfragen ausführen und Einblicke in alle Mandanten erhalten.

## <a name="run-ad-hoc-distributed-queries"></a>Ausführen von verteilten Ad-hoc-Abfragen

Nach Abschluss der Erstellung der Datenbank *adhocreporting* können Sie testweise einige verteilte Abfragen ausführen. Ziehen Sie den Ausführungsplan mit ein, um einen besseren Überblick zu haben, wo die Verarbeitung von Abfragen stattfindet. 

Um ausführlichere Informationen zu erhalten, können Sie im Ausführungsplan auf die Plansymbole zeigen. 

1. Öffnen Sie ...\\Learning Modules\\Operational Analytics\\Adhoc Reporting\\*Demo-AdhocReportingQueries.sql* in SSMS.
2. Stellen Sie sicher, dass Sie mit der Datenbank **adhocreporting** verbunden sind.
3. Klicken Sie auf das Menü **Abfrage** und dann auf **Include Actual Execution Plan** (Tatsächlichen Ausführungsplan einschließen).
4. Markieren Sie die Abfrage *Which venues are currently registered?* (Welche Veranstaltungsorte sind aktuell registriert?), und drücken Sie **F5**.

   Die Abfrage gibt die gesamte Liste an Veranstaltungsorten zurück. Dies zeigt, wie schnell und unkompliziert Sie alle Mandanten abfragen und Daten jedes Mandanten zurückgeben können.

   Schauen Sie sich den Plan an, und beachten Sie, dass die Gesamtkosten die remote Abfrage ist, weil wir einfach zu jeder Mandantendatenbank gehen und die Informationen zum Veranstaltungsort auswählen.

   ![SELECT * FROM dbo.Venues](media/saas-multitenantdb-adhoc-reporting/query1-plan.png)

5. Wählen Sie die nächste Abfrage aus, und drücken Sie **F5**.

   Diese Abfrage verknüpft Daten der Mandantendatenbank mit Daten der lokalen *VenueTypes*-Tabelle (diese ist lokal, da es sich dabei um eine Tabelle in der Datenbank *adhocreporting* handelt).

   Schauen Sie sich den Plan an, und beachten Sie, dass der Hauptteil der Kosten die remote Abfrage ist, weil wir die Informationen zum Veranstaltungsort jedes Mandanten abfragen und anschließend eine schnelle lokale Verknüpfung mit der lokalen *VenueTypes*-Tabelle durchführen, um den Anzeigenamen anzuzeigen.

   ![Verknüpfung von remoten und lokalen Daten](media/saas-multitenantdb-adhoc-reporting/query2-plan.png)

6. Wählen Sie nun die Abfrage *On which day were the most tickets sold?* (An welchem Tag wurden die meisten Tickets verkauft?) aus, und drücken Sie **F5**.

   Diese Abfrage führt etwas komplexere Verknüpfungen und Aggregationen durch. Wesentlich ist zu beachten, dass der Hauptteil der Verarbeitung remote erfolgt. Erneut erhalten wir nur die Zeilen, die wir benötigen. Für die aggregierten Ticketverkaufszahlen jedes Veranstaltungsorts pro Tag wird jeweils nur eine Zeile zurückgegeben.

   ![query](media/saas-multitenantdb-adhoc-reporting/query3-plan.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Ausführen verteilter Abfragen für alle Mandantendatenbanken
> * Stellen Sie eine Ad-hoc-Berichterstellungs-Datenbank bereit, und fügen Sie Schemas hinzu, um verteilte Abfragen auszuführen.

<!-- ??
Now try the [Tenant Analytics tutorial](saas-multitenantdb-tenant-analytics.md) to explore extracting data to a separate analytics database for more complex analytics processing...
-->

## <a name="additional-resources"></a>Zusätzliche Ressourcen

<!-- ??
* Additional [tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->

* [Elastische Abfrage](sql-database-elastic-query-overview.md)

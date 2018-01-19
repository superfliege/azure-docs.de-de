---
title: Spickzettel zu Azure SQL Data Warehouse | Microsoft Dokumentation
description: "Es wird beschrieben, wo Sie Links und bewährte Methoden finden, um schnell Azure SQL Data Warehouse-Lösungen zu erstellen."
services: sql-data-warehouse
documentationcenter: NA
author: acomet
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 12/14/2017
ms.author: acomet
ms.openlocfilehash: 2d17385ff255ddf7b85baa81600a2af60d015540
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Spickzettel für Azure SQL Data Warehouse
Diese Seite enthält Informationen zum Entwurf für die wichtigsten Anwendungsfälle Ihrer Data Warehouse-Lösung. Dieser Spickzettel bietet umfassende Unterstützung beim Aufbau eines erstklassigen Data Warehouse. Es wird jedoch ausdrücklich empfohlen, die Informationen zu den einzelnen Schritten sorgfältig zu lesen. Zunächst wird empfohlen, diesen informativen Artikel über **[Fakten und Irrtümer]** bezüglich SQL Data Warehouse lesen.

Im Anschluss folgt eine Skizzierung des Prozesses, den Sie bei Ihren ersten Schritten mit dem Entwurf von SQL Data Warehouse befolgen sollten.

![Skizzierung]

## <a name="queries-and-operations-across-tables"></a>Tabellenübergreifende Abfragen und Vorgänge

Es ist wichtig, sich im Vorfeld die wichtigsten Vorgänge und Abfragen, die in Ihrem Data Warehouse stattfinden, in Erfahrung zu bringen. Bei der Architektur Ihres Data Warehouse sollte Ihr Hauptaugenmerk dementsprechend auf ebenjenen Vorgänge liegen. Häufige Vorgänge könnten beispielsweise Folgende sein:
* Verknüpfen von einer oder zwei Faktentabellen mit Dimensionstabellen, Filtern dieser Tabelle für einen bestimmten Zeitraum und Anfügen der Ergebnisse an einen Data Mart
* Durchführen von umfangreichen oder geringfügigen Updates in der Faktenumsatztabelle
* Anfügen von ausschließlich Daten an Tabellen

Nur, wenn Sie wissen, welche Arten von Vorgängen vorhanden sind, können Sie Ihre Tabellen optimal entwerfen.

## <a name="data-migration"></a>Datenmigration

Es wird empfohlen, zuerst Ihre Daten **[in ADLS]** oder den Azure Blob Storage zu laden. Anschließend sollten Sie mithilfe von PolyBase die Daten in SQL Data Warehouse in einer Stagingtabelle laden. Folgende Konfigurationen werden empfohlen:

| Entwurf | Empfehlung |
|:--- |:--- |
| Distribution | Round-Robin |
| Indizierung | Heap |
| Partitionierung | Keine |
| Ressourcenklasse | largerc oder xlargerc |

Erfahren Sie mehr über die **[Datenmigration], das [Laden von Daten]** mit **[ausführlicheren Anweisungen] zum Laden**. 

## <a name="distributed-or-replicated-tables"></a>Verteilte oder replizierte Tabellen

Abhängig von den Tabelleneigenschaften werden folgende Strategien empfohlen:

| Typ | Für folgende Fälle empfohlen | In folgenden Fälle mit Vorsicht zu behandeln|
|:--- |:--- |:--- |
| Repliziert | • Kleine Dimensionstabellen in einem Sternschema mit max. 2 GB Speicher nach Komprimierung (ca. 5-facher Komprimierung) |• Viele Schreibtransaktionen in Tabellen (z.B. „insert“, „upsert“, „delete“, „update“)<br></br>• Häufige Änderungen an der Bereitstellung von DWUs (Data Warehouse Einheiten)<br></br>• Nutzung von lediglich 2 bis 3 Spalten bei einer Tabelle mit einer Vielzahl von Spalten<br></br>• Indizierung einer replizierten Tabelle |
| Roundrobin (Standard) | • Temporäre/Stagingtabellen<br></br> • Fehlen eines eindeutigen Verknüpfungsschlüssels oder einer geeigneten Spalte |• Leistungseinbußen aufgrund von Datenverschiebungen |
| Hash | • Faktentabellen<br></br>• Umfangreiche Dimensionstabellen |• Fehlende Möglichkeit zur Aktualisierung des Verteilungsschlüssels |

**Tipps:**
* Beginnen Sie mit einer Roundrobintabelle. Versuchen Sie jedoch auf eine Hashverteilungsstrategie hinzuarbeiten, um von den Vorteilen einer hochgradig parallelen Architektur profitieren zu können.
* Stellen Sie sicher, dass gemeinsame Hashschlüssel dasselbe Datenformat aufweisen.
* Führen Sie Verteilungen nicht im varchar-Format durch.
* Dimensionstabellen mit gemeinsamem Hashschlüsseln für eine Faktentabelle mit häufigen Verknüpfungsvorgängen könnten mit einer Hashverteilung versehen sein.
* Verwenden Sie *[sys.dm_pdw_nodes_db_partition_stats]*, um eine etwaige Schiefe in den Daten zu analysieren.
* Verwenden Sie *[sys.dm_pdw_request_steps]*, um Datenverschiebungen nach Abfragen zu analysieren, die Zeitübertragung zu überwachen und Vorgänge zu mischen. Dies ist für die Überprüfung Ihre Verteilungsstrategie hilfreich.

Weitere Informationen finden Sie in den Artikeln zu **[replizierten Tabellen] und [verteilten Tabellen]**.

## <a name="indexing-your-table"></a>Indizieren einer Tabelle

Die Indizierung ist **hervorragend** zum schnellen Lesen von Tabellen geeignet. Abhängig von Ihren Anforderungen steht Ihnen eine einzigartige Gruppe von Technologien zur Verfügung:

| Typ | Für folgende Fälle empfohlen | In folgenden Fälle mit Vorsicht zu behandeln|
|:--- |:--- |:--- |
| Heap | • Staging-/temporäre Tabelle<br></br>• Kleine Tabellen mit geringfügigen Suchvorgängen |• Beliebige Suchvorgänge zum Durchsuchen einer gesamten Tabelle |
| Gruppierter Index | • Tabelle mit bis zu 100 Mio. Zeilen<br></br>• Große Tabellen (mehr als 100 Mio. Zeilen) mit häufiger Nutzung von lediglich 1-2 Spalten |• Verwendung bei einer replizierten Tabelle<br></br>• Komplexe Abfragen mit mehreren Join- und Group By-Vorgängen<br></br>• Durchführung von Updates für indizierte Spalten, die Speicher belegen |
| Gruppierter Columnstore-Index (CCI) (Standard) | • Große Tabellen (mehr als 100 Mio. Zeilen) | • Verwendung bei einer replizierten Tabelle<br></br>• Umfangreiche Aktualisierungsvorgänge für Ihre Tabelle<br></br>• Übermäßige Tabellenpartitionierung: Zeilengruppen erstrecken sich nicht auf andere Verteilungsknoten und -partitionen |

**Tipps:**
* Neben einem gruppierten Index empfiehlt es sich, einer häufig zum Filtern verwendeten Spalte einen nicht gruppierten Index hinzuzufügen. 
* Achten Sie darauf, wie Sie den Speicher für eine Tabelle mit einem CCI verwalten. Wenn Sie Daten laden, sollte der Benutzer (oder die Abfrage) eine umfangreiche Ressourcenklasse nutzen können. Achten Sie daher darauf, Zeilengruppen nicht zu kürzen und stattdessen viele kleine komprimierte Zeilengruppen zu erstellen.
* Für Computeleistung optimierte Stufe – ideal für CCI
* Bei einem CCI können aufgrund von unzureichender Komprimierung Ihrer Zeilengruppen möglicherweise Leistungseinbußen auftreten. Daher empfiehlt es sich, den CCI neu zu erstellen oder neu zu organisieren. Sie sollten mindestens 100.000 Zeilen pro komprimierter Zeilengruppe verwalten. Ideal sind 1 Mio. Zeilen in einer Zeilengruppe.
* Basierend auf der Häufigkeit und der Größe inkrementeller Ladevorgänge empfiehlt es sich, die Neuorganisation oder Neuerstellung von Indizes zu automatisieren. Hier empfiehlt sich immer eine gründliche Bereinigung.
* Beim Kürzen einer Zeilengruppe sollten Sie strategisch vorgehen: Wie groß sind die offenen Zeilengruppen? Wie viele Daten werden Sie in den kommenden Tagen voraussichtlich laden?

Weitere Informationen finden Sie im Artikel zu **[Indizes]**.

## <a name="partitioning"></a>Partitionierung
Wenn Sie über umfangreiche Faktentabellen (Tabellen mit über 1 Mrd. Zeilen) verfügen, empfiehlt sich eventuell eine Partitionierung Ihrer Tabelle. Der Partitionsschlüssel sollte in den meisten Fällen auf dem Datum basieren. Achten Sie darauf, die Partitionierung im Rahmen zu halten, insbesondere bei einem gruppierten Columnstore-Index.
Bei Stagingtabellen, die ETL erfordern, kann eine Partitionierung von Vorteil sein. Sie vereinfacht die Verwaltung des Datenlebenszyklus.
Achten Sie darauf, die Partitionierung im Rahmen zu halten, insbesondere bei einem gruppierten Columnstore-Index.

Weitere Informationen finden Sie im Artikel zu **[Partitionen]**.

## <a name="incremental-load"></a>Inkrementell laden

Zunächst sollten Sie größere Ressourcenklassen zum Laden Ihrer Daten zuordnen. Es wird empfohlen, PolyBase und ADF V2 für die Automatisierung der ETL-Pipelines in SQL DW zu verwenden.

Bei einer großen Anzahl von Updates in Ihren Verlaufsdaten wird empfohlen, zuerst die betreffenden Daten zu löschen. Anschließend können Sie eine Masseneinfügung der neuen Daten vornehmen. Eine solche 2-stufige Vorgehensweise ist effizienter.

## <a name="maintain-statistics"></a>Verwalten von Statistiken
Automatische Statistiken werden demnächst als allgemein verfügbare Version veröffentlicht. Bis dahin müssen Statistiken in SQL Data Warehouse manuell verwaltet werden. Es ist wichtig, Statistiken zu aktualisieren, wenn **wesentliche** Änderungen an Ihren Daten vorgenommen werden. Hierdurch werden Ihre Abfragepläne optimiert. Wenn die Verwaltung aller Statistiken zu viel Zeit in Anspruch nimmt, sollten Sie die Auswahl der Spalten, die mit Statistiken zu versehen sind, stärker eingrenzen. Sie können auch die Häufigkeit der Updates definieren. Beispielsweise können Sie Datumsspalten aktualisieren, wenn täglich neue Werte hinzugefügt werden. Die größten Vorteile ergeben sich, wenn Sie Statistiken für Spalten in Verknüpfungen, in der WHERE-Klausel verwendete Spalten und Spalten in GROUP BY nutzen.

Weitere Informationen finden Sie im Artikel zu **[Statistiken]**.

## <a name="resource-class"></a>Ressourcenklasse
SQL Data Warehouse nutzt Ressourcengruppen zum Zuordnen von Arbeitsspeicher zu Abfragen. Wenn Sie mehr Speicher zur Beschleunigung von Abfrage- oder Ladevorgängen benötigen, sollten Sie umfangreichere Ressourcenklassen zuordnen. Die Verwendung umfangreicherer Ressourcenklassen hat jedoch auch Nachteile im Hinblick auf die Parallelität zur Folge. Dies sollten Sie berücksichtigen, bevor Sie sämtliche Benutzer einer umfangreichen Ressourcenklasse zuordnen.

Wenn Sie feststellen, dass Abfragen zu lange dauern, stellen Sie sicher, dass Ihre Benutzer keine Vorgänge in umfangreichen Ressourcenklassen ausführen. Umfangreiche Ressourcenklassen beanspruchen viele Parallelitätsslots. Dies kann dazu führen, dass sich andere Abfragen in der Warteschlange ansammeln.

Wenn Sie die computeoptimierte Stufe verwenden, erhält jede Ressourcenklasse 2,5-mal mehr Speicher als in der für Elastizität optimierten Stufe.

Weitere Informationen finden Sie im Artikel zum Arbeiten mit **[Ressourcenklassen und Parallelität]**.

## <a name="lower-your-cost"></a>Senkung Ihrer Kosten
Ein wichtiges Feature von SQL Data Warehouse ist die Möglichkeit, die Anwendung zu pausieren, wenn Sie sie nicht verwenden. Es werden dann keine Computeressourcen berechnet. Ein weiteres wichtiges Feature ist die Möglichkeit zum Skalieren der Ressourcen. Das Pausieren und Skalieren kann über das Azure-Portal oder mit PowerShell-Befehlen durchgeführt werden.

Mit Azure Functions können Sie nun jederzeit eine automatische Skalierung veranlassen:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-you-architecture-for-performance"></a>Optimieren der Architektur für hohe Leistung

Wir empfehlen, SQL-Datenbank und Azure Analysis Services in einer Hub-and-Spoke-Architektur in Erwägung zu ziehen. Als solch eine Lösung ermöglicht sie nicht nur die Workloadisolation zwischen unterschiedlichen Benutzergruppen, sondern stellt gleichzeitig auch erweiterte Sicherheitsfeatures von SQL-Datenbank sowie Azure Analysis Services bereit. Darüber hinaus bietet eine derartige Lösung grenzenlose Parallelität für Ihre Benutzer.

Weitere Informationen finden Sie im Artikel zu **[typischen Architekturen mit SQL Data Warehouse]**.

Stellen Sie über SQL Data Warehouse mit einem Klick die Spokes in SQL-Datenbanken bereit:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Skizzierung]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[Laden von Daten]:./design-elt-data-loading.md
[ausführlicheren Anweisungen]: ./guidance-for-loading-data.md
[Indizes]:./sql-data-warehouse-tables-index.md
[Partitionen]:./sql-data-warehouse-tables-partition.md
[Statistiken]:./sql-data-warehouse-tables-statistics.md
[Ressourcenklassen und Parallelität]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[typischen Architekturen mit SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[Fakten und Irrtümer]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[Datenmigration]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[replizierten Tabellen]:https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[verteilten Tabellen]:https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[in ADLS]: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql

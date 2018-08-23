---
title: Elastische Abfrage – Zugreifen auf Daten in Azure SQL Data Warehouse über Azure SQL-Datenbank | Microsoft-Dokumentation
description: Hier werden die bewährten Methoden zur Verwendung von elastischen Abfragen zum Zugreifen auf Daten in Azure SQL Data Warehouse über Azure SQL-Datenbank beschrieben.
services: sql-data-warehouse
author: hirokib
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/11/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: 344cb1bed56b0b6af7bd3704f8674ae30695f885
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "42140140"
---
# <a name="best-practices-for-using-elastic-query-in-azure-sql-database-to-access-data-in-azure-sql-data-warehouse"></a>Bewährte Methoden zur Verwendung von elastischen Abfragen in Azure SQL-Datenbank zum Zugreifen auf Daten in Azure SQL Data Warehouse
Hier werden die bewährten Methoden zur Verwendung von elastischen Abfragen zum Zugreifen auf Daten in Azure SQL Data Warehouse über Azure SQL-Datenbank beschrieben. 

## <a name="what-is-an-elastic-query"></a>Was ist eine elastische Abfrage?
Mit einer elastischen Abfrage können Sie T-SQL und externe Tabellen verwenden, um eine Abfrage in eine Azure SQL-Datenbank zu schreiben, die per Remotezugriff an ein Azure SQL Data Warehouse gesendet wird. Diese Funktion ermöglicht je nach Szenario Kosteneinsparungen und leistungsstärkere Architekturen.

Bei dieser Funktion sind zwei primäre Szenarien möglich:

1. Domänenisolation
2. Ausführung von Remoteabfragen

### <a name="domain-isolation"></a>Domänenisolation

Eine Domänenisolation bezieht sich auf das klassische Data Mart-Szenario. In bestimmten Szenarien sollte möglicherweise eine logische Datendomäne für nachgelagerte Benutzer zur Verfügung gestellt werden, die vom Rest der Data Warehouse-Instanz isoliert sind. Hierfür kann es verschiedenste Gründe geben, wie u.a. Folgende:

1. Ressourcenisolation – Die SQL-Datenbank wurde dahingehend optimiert, dass eine große Basis gleichzeitiger Benutzer Workloads bereitstellen, die sich geringfügig von den großen Analyseabfragen unterscheiden können, für die das Data Warehouse reserviert ist. Durch die Isolation wird sichergestellt, dass die entsprechenden Workloads durch die jeweiligen Tools verarbeitet werden.
2. Sicherheitsisolation – Dient zur selektiven Trennung einer autorisierten Datenteilmenge über bestimmte Schemas.
3. Sandkasten – Bieten eine Reihe von Beispieldaten als Testmaterial, um Produktionsabfragen etc. zu untersuchen.

Elastische Abfragen bieten die Möglichkeit, mühelos Teilmengen von SQL Data Warehouse-Daten auszuwählen und in eine SQL-Datenbankinstanz zu verschieben. Zudem können bei dieser Isolierung auch Remoteabfragen ausgeführt werden, die interessantere Cacheszenarien ermöglichen.

### <a name="remote-query-execution"></a>Ausführung von Remoteabfragen

Elastische Abfragen ermöglichen die Ausführung von Remoteabfragen in einer SQL Data Warehouse-Instanz. Indem Sie Ihre heißen und kalten Daten durch diese beiden Datenbanken trennen, können Sie von den Vorteilen der SQL-Datenbank und denen von SQL Data Warehouse profitieren. Benutzer können neuere Daten in einer SQL-Datenbank speichern, die zur Berichterstellung und von einer großen Anzahl gewöhnlicher Geschäftskunden genutzt werden kann. Wenn jedoch weitere Daten oder Berechnungen erforderlich sind, kann ein Benutzer einen Teil der Abfrage an eine SQL Data Warehouse-Instanz auslagern, in der umfangreiche Aggregate weitaus schneller und effizienter verarbeitet werden können.

## <a name="elastic-query-process"></a>Prozess für elastische Abfragen
Eine elastische Abfrage kann verwendet werden, um Daten in einer SQL Data Warehouse-Instanz in SQL-Datenbankinstanzen zur Verfügung zu stellen. Durch elastische Abfragen können Abfragen aus einer SQL-Datenbank auf Tabellen in einer SQL Data Warehouse-Remoteinstanz verweisen. 

Der erste Schritt besteht darin, eine Definition für eine externe Datenquelle zu erstellen, die auf die SQL Data Warehouse-Instanz verweist. Diese verwendet die bestehenden Benutzeranmeldeinformationen in der SQL Data Warehouse-Instanz. Es sind keine Änderungen an der SQL Data Warehouse-Remoteinstanz erforderlich. 

> [!IMPORTANT] 
> 
> Sie müssen über die Berechtigung ALTER ANY EXTERNAL DATA SOURCE verfügen. Diese Berechtigung ist in der Berechtigung ALTER DATABASE enthalten. ALTER ANY EXTERNAL DATA SOURCE-Berechtigungen sind erforderlich, um auf Remotedatenquellen zu verweisen.

Erstellen Sie als Nächstes eine Definition für eine externe Remotetabelle in einer SQL-Datenbankinstanz, die auf eine Remotetabelle in der SQL Data Warehouse-Instanz verweist. Wenn für eine Abfrage eine externe Tabelle verwendet wird, wird der auf die externe Tabelle verweisende Teil der Abfrage an die zu verarbeitende SQL Data Warehouse-Instanz gesendet. Wenn die Abfrage abgeschlossen ist, wird das Resultset an die aufrufende SQL-Datenbankinstanz zurückgesendet. Ein kurzes Tutorial zum Einrichten einer elastischen Abfrage zwischen der SQL-Datenbank und SQL Data Warehouse finden Sie unter [Konfigurieren einer elastischen Abfrage mit SQL Data Warehouse][Configure Elastic Query with SQL Data Warehouse].

Weitere Informationen zu elastischen Abfragen mit der SQL-Datenbank finden Sie unter [Übersicht über elastische Abfragen in Azure SQL-Datenbank][Azure SQL Database elastic query overview].

## <a name="best-practices"></a>Bewährte Methoden
Verwenden Sie diese bewährten Methoden zur effektiven Nutzung von elastischen Abfragen.

### <a name="general"></a>Allgemein

- Wählen Sie bei der Ausführung von Remoteabfragen nur die erforderlichen Spalten aus, und wenden Sie die richtigen Filter an. Anderenfalls ist nicht nur eine höhere Computeleistung erforderlich, sondern auch die Größe des Resultsets und damit die Menge der Daten steigen, die zwischen den beiden Instanzen verschoben werden müssen.
- Verwalten Sie Daten für Analysezwecke in SQL Data Warehouse und in der SQL-Datenbank in gruppierten Columnstore-Indizes, um die Analyseleistung zu erhöhen.
- Stellen Sie sicher, dass Quellentabellen zum Verschieben von Abfragen und Daten partitioniert werden.
- Stellen Sie sicher, dass die als Cache verwendeten SQL-Datenbankinstanzen partitioniert werden, um präzisere Updates und eine einfachere Verwaltung zu ermöglichen. 
- Verwenden Sie idealerweise PremiumRS-Datenbanken, da diese die Analysevorteile der gruppierten Columnstore-Indizierung speziell für E/A-intensive Workloads bieten – und das zu einem günstigeren Preis als Premium-Datenbanken.
- Nutzen Sie nach Ladevorgängen Identifikationsspalten für Lasten oder Gültigkeitsdaten für Upserts in den SQL-Datenbankinstanzen, um die Integrität der Cachequelle zu wahren. 
- Erstellen Sie separat eine Anmeldung und einen Benutzer in Ihrer SQL Data Warehouse-Instanz für die Anmeldeinformationen zur Remoteanmeldung bei Ihrer SQL-Datenbank, die in der externen Datenquelle definiert sind. 

### <a name="elastic-querying"></a>Ausführen von elastischen Abfragen

- In vielen Fällen könnte man eine Art von gestreckter Tabelle verwalten, bei der sich ein Teil Ihrer Tabelle zu Leistungszwecken innerhalb der SQL-Datenbank als zwischengespeicherte Daten befinden, wobei die restlichen Daten in SQL Data Warehouse gespeichert werden. Sie benötigen zwei Objekte in der SQL-Datenbank: eine externe Tabelle in der SQL-Datenbank, die auf die Basistabelle in SQL Data Warehouse verweist, und den „zwischengespeicherten“ Teil der Tabelle in der SQL-Datenbank. Erwägen Sie die Erstellung einer Ansicht über den oberen Teil des zwischengespeicherten Teils der Tabelle und der externen Tabelle, die sowohl Tabellen vereint als auch Filter anwendet, die in der SQL-Datenbank und in SQL Data Warehouse materialisierte Daten trennt, die durch externe Tabellen offengelegt wurden.

  Stellen Sie sich vor, Sie möchten die Daten des letzten Jahres in einer SQL-Datenbankinstanz speichern. In der Tabelle **ext.Orders** wird auf die Tabellen mit den Data Warehouse-Bestellungen verwiesen. Die Tabelle **dbo.Orders** enthält die Daten des letzten Jahres für die SQL-Datenbankinstanz. Anstatt die Benutzer um die Angabe zu bitten, welche Tabelle abgefragt werden soll, erstellen wir oberhalb der beiden Tabellen eine Ansicht am Partitionspunkt des letzten Jahres.

  ```sql
  CREATE VIEW dbo.Orders_Elastic AS
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]
  FROM
    [dbo].[Orders]
  WHERE 
    YEAR([o_orderdate]) >= '<Most Recent Year>'
  UNION
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]         
  FROM
    [ext].[Orders]
  WHERE
    YEAR([o_orderdate]) < '<Most Recent Year>'
  ```

  Durch eine auf solche Weise erzeugte Ansicht kann der Abfragecompiler feststellen, ob Ihre Benutzerabfrage mithilfe der Data Warehouse-Instanz beantwortet werden muss. 

  Beim Übermitteln, Kompilieren, Ausführen und Verschieben von Daten im Zusammenhang mit den einzelnen elastischen Abfragen an die Data Warehouse-Instanz entsteht Overhead. Sie sollten sich darüber im Klaren sein, dass sich jede elastische Abfrage nachteilig auf Parallelitätsslots auswirkt und Ressourcen beansprucht.  


- Wenn Sie vorhaben, ein ausführlicheres Drilldown auf das Resultset in der Data Warehouse-Instanz auszuführen, sollten Sie es eventuell in einer temporären Tabelle in der SQL-Datenbank materialisieren, um die Leistung zu steigern und einen unnötigen Ressourcenverbrauch zu vermeiden.

### <a name="moving-data"></a>Verschieben von Daten 

- Vereinfachen Sie die Datenverwaltung möglichst durch Quellentabellen, die nur angefügt werden können, damit mühelos Updates zwischen der Data Warehouse- und der Datenbank-Instanz verwaltet werden können.
- Verschieben Sie Daten auf Partitionsebene mithilfe der Semantik der „flush“- und „fill“-Methode, um die Abfragekosten auf der Data Warehouse-Ebene und die Menge der zum Aktualisieren der Datenbankinstanz verschobenen Daten zu minimieren. 

### <a name="when-to-choose-azure-analysis-services-vs-sql-database"></a>Gründe für die Auswahl von Azure Analysis Services oder der SQL-Datenbank

Verwenden Sie Azure Analysis Services in folgenden Fällen:

- Sie haben vor, Ihren Cache mit einem BI-Tool zu verwenden, das eine Vielzahl kleinerer Abfragen übermittelt.
- Sie sind auf Abfragewartezeiten von unter 1 Sekunde angewiesen.
- Sie besitzen Erfahrungen in der Verwaltung bzw. Entwicklung von Modellen für Analysis Services. 

Verwenden Sie Azure SQL-Datenbank in folgenden Fällen:

- Sie möchten Ihre Cachedaten mithilfe von SQL abfragen.
- Für bestimmte Abfragen ist eine Remoteausführung erforderlich.
- Sie verfügen über umfangreichere Cacheanforderungen.

## <a name="faq"></a>Häufig gestellte Fragen

F: Kann ich Datenbanken in einem Pool für elastische Datenbanken mit elastischen Abfragen verwenden?

A: Ja. SQL-Datenbanken in einem Pool für elastische Datenbanken können elastische Abfragen verwenden. 

F: Gibt es eine Obergrenze für die Anzahl der Datenbanken, die ich für elastische Abfragen verwenden kann?

A: Es gibt keine feste Obergrenze für die Anzahl der Datenbanken, die für elastische Abfragen verwendet werden können? Allerdings zählt jede elastische Abfrage (Abfragen, die SQL Data Warehouse treffen) zu den normalen Parallelitätsgrenzwerten.

F: Gibt es DTU-Grenzwerte für elastische Abfragen?

A: DTU-Grenzwerte werden für elastische Abfragen nicht anders vorgegeben. Die Standardrichtlinie ist, dass logische Server mit DTU-Grenzwerten versehen sind, um zu verhindern, dass Kunden versehentlich Mehrkosten entstehen. Wenn Sie neben einer SQL Data Warehouse-Instanz zusätzlich mehrere Datenbanken für elastische Abfragen aktivieren, kann es vorkommen, dass Sie unerwartet die Obergrenze erreichen. Senden Sie in diesem Fall eine Anfrage zur Erhöhung des DTU-Grenzwerts für Ihren logischen Server. Sie können Ihr Kontingent erhöhen, indem Sie ein [Supportticket erstellen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) und als Anfragetyp *Kontingent* auswählen.

F: Kann ich bei elastischen Abfragen die Sicherheit auf Zeilenebene bzw. dynamische Datenmaskierung (DDM) anwenden?

A: Ja, Kunden, die erweiterte Sicherheitsfunktionen bei der SQL-Datenbank verwenden möchten, müssen hierfür die Daten lediglich zuerst in die SQL-Datenbank verschieben und in dieser speichern. Die Sicherheit auf Zeilenebene und DDM können derzeit nicht auf über externe Tabellen abgefragte Daten angewendet werden. 

F: Kann ich Daten von meiner SQL-Datenbankinstanz in die Data Warehouse-Instanz schreiben?

A: Diese Funktion wird derzeit nicht unterstützt. Wenn Sie möchten, dass diese Funktion in Zukunft eingeführt werden soll, besuchen Sie unsere [Feedbackseite][Feedback page], um dort für die Erstellung dieser Funktionalität abzustimmen. 

F: Kann ich räumliche Typen wie „geometry“ oder „geography“ verwenden?

A: Räumliche Typen können als varbinary(max)-Werte in SQL Data Warehouse gespeichert werden. Wenn Sie diese Spalten anhand von elastischen Abfragen abfragen, können Sie diese zur Laufzeit in die entsprechenden Typen konvertieren.

![Räumliche Typen](./media/sql-data-warehouse-elastic-query-with-sql-database/geometry-types.png)

<!--Article references-->

[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Configure Elastic Query with SQL Data Warehouse]: tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md
[Feedback Page]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Database elastic query overview]: ../sql-database/sql-database-elastic-query-overview.md



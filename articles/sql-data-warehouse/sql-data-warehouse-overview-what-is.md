---
title: Was ist Azure SQL Data Warehouse? | Microsoft Docs
description: "Verteilte Datenbank der Enterprise-Klasse, die mehrere Petabyte an relationalen und nicht relationalen Daten verarbeiten kann Sie ist das branchenweit erste Cloud-Data Warehouse mit Funktionen zum Vergrößern, Verkleinern und Anhalten innerhalb weniger Sekunden."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 10/23/2017
ms.author: jrj;barbkess
ms.openlocfilehash: d5ad5b566bd8d40ab6d7a9151af54890fd47cc88
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="what-is-azure-sql-data-warehouse"></a>Was ist Azure SQL Data Warehouse?

SQL Data Warehouse ist ein cloudbasiertes Enterprise Data Warehouse (EDW) mit Massively Parallel Processing (MPP) zur schnellen Ausführung komplexer Abfragen für mehrere Petabytes an Daten. Verwenden Sie SQL Data Warehouse als Schlüsselkomponente einer Big Data-Lösung. Importieren Sie große Datenmengen mit einfachen PolyBase-T-SQL-Abfragen in SQL Data Warehouse, und nutzen Sie dann die Vorteile von MPP für aufwendige Analysen. Im Zuge von Integrationen und Analysen wird das Data Warehouse zur Single Version of Truth, auf die sich Ihr Unternehmen bei der Gewinnung von Erkenntnissen verlassen kann.  


## <a name="key-component-of-big-data-solution"></a>Schlüsselkomponente für Big Data-Lösungen
SQL Data Warehouse ist eine Schlüsselkomponente einer umfassenden Big Data-Lösung in der Cloud.

![Data Warehouse-Lösung](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

In einer Clouddatenlösung werden Daten aus verschiedensten Quellen in Big Data-Speichern erfasst. Nach der Speicherung in einem Big Data-Speicher werden die Daten durch Hadoop, Spark und Machine Learning-Algorithmen vorbereitet und trainiert. Wenn die Daten für komplexe Analysen bereit sind, fragt SQL Data Warehouse die Big Data-Speicher mithilfe von PolyBase ab. PolyBase verwendet T-SQL-Standardabfragen, um die Daten an SQL Data Warehouse zu übermitteln.
 
SQL Data Warehouse speichert Daten in relationalen Tabellen mit spaltenbasiertem Speicher. Dieses Format zeichnet sich durch erheblich geringere Datenspeicherkosten und eine bessere Abfrageleistung aus. Für die in SQL Data Warehouse gespeicherten Daten können Sie umfangreiche Analysen durchführen. Im Vergleich zu herkömmlichen Datenbanksystemen dauern Analyseabfragen nur noch Sekunden statt Minuten (oder Stunden statt Tage). 

Die Analyseergebnisse können an Berichtsdatenbanken oder Anwendungen auf der ganzen Welt übermittelt werden. Business Analysts können so Erkenntnisse gewinnen und fundierte Geschäftsentscheidungen treffen.

## <a name="optimization-choices"></a>Optimierungsoptionen

Die auf Flexibilität ausgelegten [Leistungsstufen](performance-tiers.md) von SQL Data Warehouse ermöglichen die Erfüllung von Datenanforderungen jeglicher Größe. Sie können sich für ein Data Warehouse mit optimierter Elastizität oder für ein Data Warehouse mit Computeoptimierung entscheiden. 

- Die **Leistungsstufe „Optimiert für Elastizität“** trennt die Compute- und die Speicherebene in der Architektur. Diese Option eignet sich besonders für Workloads, die durch häufige Skalierung optimal von der Trennung zwischen Compute und Speicher profitieren können, um vorübergehende Aktivitätsspitzen zu unterstützen. Diese Computestufe hat den niedrigsten Einstiegspreis und deckt dank Skalierung den größten Teil der Kundenworkloads ab.

- Die **Leistungsstufe „Optimiert für Compute“** nutzt die neueste Azure-Hardware, um einen neuen NVMe-SSD-Zwischenspeicher einzuführen, der dafür sorgt, dass besonders häufig verwendete Daten in der Nähe der CPUs bleiben – also genau dort, wo Sie sie haben möchten. Dank automatischem Tiering des Speichers eignet sich diese Leistungsstufe hervorragend für komplexe Abfragen, da sämtliche Eingaben/Ausgaben lokal auf der Computeebene abgewickelt werden. Darüber hinaus wird der Columnstore erweitert, um eine unbegrenzte Menge an Daten in Ihrem SQL Data Warehouse speichern zu können. Die Leistungsstufe „Optimiert für Compute“ bietet die größtmögliche Skalierbarkeit und ermöglicht eine Skalierung auf bis zu 30.000 cDWU (compute Data Warehouse Units). Diese Stufe empfiehlt sich für Workloads mit kontinuierlich hohem Leistungsbedarf.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen von SQL Data Warehouse vertraut gemacht haben, können Sie sich nun darüber informieren, wie Sie schnell [eine SQL Data Warehouse-Instanz erstellen][create a SQL Data Warehouse] und [Beispieldaten][load sample data] laden. Falls Sie mit Azure noch nicht vertraut sind und auf neue Terminologie stoßen, ist das [Azure-Glossar][Azure glossary] sehr nützlich. Weitere SQL Data Warehouse-Ressourcen:  

* [Kundenerfolgsgeschichten]
* [Blogs]
* [Funktionsanfragen]
* [Videos]
* [Customer Advisory Team-Blogs]
* [Erstellen eines Supporttickets]
* [MSDN-Forum]
* [Stack Overflow-Forum]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Erstellen eines Supporttickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Kundenerfolgsgeschichten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Customer Advisory Team-Blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funktionsanfragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-Forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-Forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/en-us/support/legal/sla/

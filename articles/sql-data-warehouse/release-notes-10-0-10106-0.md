---
title: 'Azure SQL Data Warehouse: Versionshinweise Dezember 2018 | Microsoft-Dokumentation'
description: Versionshinweise zu Azure SQL Data Warehouse
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 12/12/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 8e82e352ebea4634b1b99864245adcf606352657
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469334"
---
# <a name="whats-new-in-azure-sql-data-warehouse-version---100101060"></a>Neuerungen in Azure SQL Data Warehouse Version 10.0.10106.0
Azure SQL Data Warehouse (SQL DW) wird ständig verbessert. In diesem Artikel informieren wir Sie über neue Features und Änderungen, die SQL DW Version 10.0.10106.0 eingeführt wurden.

## <a name="query-restartability---ctas-and-insertselect"></a>Neustartfähigkeit von Abfragen – CTAS und INSERT/SELECT
In seltenen Fällen (d. h. bei zeitweiligen Netzwerk-Verbindungsproblemen, Knotenausfällen) kann die Ausführung von Abfragen in Azure SQL DW fehlschlagen. Anweisungen mit längerer Ausführungszeit wie [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) und INSERT-SELECT-Vorgänge sind für dieses potenzielle Problem stärker anfällig. Mit diesem Release wurde Wiederholungslogik für CTAS- und INSERT-SELECT-Anweisungen (zusätzlich zu den zuvor angekündigten SELECT-Anweisungen) in Azure SQL DW implementiert, sodass das System diese vorübergehenden Probleme behandeln und das Fehlschlagen von Abfragen verhindern kann. Die Anzahl der Wiederholungsversuche und die Liste der behandelten vorübergehenden Fehler sind vom konfiguriert.

## <a name="return-order-by-optimization"></a>Rückgabereihenfolge nach Optimierung
Abfragen vom Typ „SELECT…ORDER BY“ profitieren in diesem Release von einer Leistungssteigerung.  Zuvor hat das Abfrageausführungsmodul Ergebnisse auf jedem Computeknoten geordnet und an den Steuerknoten gestreamt, auf dem sie dann zusammengeführt wurden. Mit dieser Verbesserung senden stattdessen alle Computeknoten ihre Ergebnisse an einen zentralen Computeknoten, der sie dann zusammenführt und die sortierten Ergebnisse über den Computeknoten an den Benutzer zurückgibt.  Wenn die Abfrageergebnisse eine große Anzahl von Zeilen enthalten, bedeutet dies eine erhebliche Leistungssteigerung.

## <a name="data-movement-enhancements-for-partitionmove-and-broadcastmove"></a>Verbesserung der Datenverschiebung für PartitionMove und BroadcastMove
In Azure SQL Data Warehouse Gen2 verwenden die Datenverschiebungsschritte von Typ „ShuffleMove“ Techniken zur Datensofortverschiebung, die [hier im Blog zu Leistungsverbesserungen](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/) beschrieben sind.  Ab diesem Release werden auch für die Datenverschiebungstypen „PartitionMove“ und „BroadcastMove“ auch diese Techniken zur Datensofortverschiebung verwendet.  Benutzerabfragen, die diese Typen von Datenverschiebungsschritten verwenden, erfahren eine Leistungssteigerung.  Um diese Leistungsverbesserungen zu nutzen, ist keine Codeänderung erforderlich.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen von SQL Data Warehouse vertraut gemacht haben, können Sie sich nun darüber informieren, wie Sie schnell [eine SQL Data Warehouse-Instanz erstellen][create a SQL Data Warehouse]. Falls Sie mit Azure noch nicht vertraut sind und neue Terminologie erlernen müssen, ist das [Azure-Glossar][Azure glossary] sehr nützlich. Weitere SQL Data Warehouse-Ressourcen:  

* [Kundenerfolgsgeschichten]
* [Blogs]
* [Funktionsanfragen]
* [Videos]
* [Customer Advisory Team-Blogs]
* [Stack Overflow-Forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Customer Advisory Team-Blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Kundenerfolgsgeschichten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Funktionsanfragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-Forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

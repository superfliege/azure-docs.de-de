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
ms.openlocfilehash: 0fa7004b9b0dd1ecf087566e7ce8a7d6ebf9be93
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57842106"
---
# <a name="whats-new-in-azure-sql-data-warehouse-december-2018"></a>Neuerungen in Azure SQL Data Warehouse Dezember 2018
Azure SQL Data Warehouse wird fortlaufend verbessert. In diesem Artikel informieren wir Sie über neue Features und Änderungen, die im Dezember 2018 eingeführt wurden.

## <a name="virtual-network-service-endpoints-generally-available"></a>Allgemeine Verfügbarkeit der Dienstendpunkte virtueller Netzwerke
In diesem Release sind Dienstendpunkte in virtuellen Netzwerken (VNETs) für Azure SQL Data Warehouse in allen Azure-Regionen allgemein verfügbar. Mit Dienstendpunkten im virtuellen Netzwerk können Sie Verbindungen aus einem bestimmten Subnetz oder einer Gruppe von Subnetzen in Ihrem virtuellen Netzwerk mit Ihrem logischen Server isolieren. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und Azure SQL Data Warehouse befindet sich jederzeit innerhalb des Azure-Backbonenetzwerks. Diese direkte Route wird speziellen Routen, bei denen Internetdatenverkehr über virtuelle Geräte oder lokal geleitet wird, vorgezogen. Es erfolgt keine zusätzliche Abrechnung für den virtuellen Netzwerkzugriff über Dienstendpunkte. Das aktuelle Preismodell für [Azure SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/) ist weiterhin gültig.

Ab diesem Release steht auch die PolyBase-Konnektivität mit [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) (ADLS) über den Treiber des [Azure-Blobdateisystems](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-abfs-driver) zur Verfügung. Azure Data Lake Storage Gen2 stattet Azure Storage mit allen Eigenschaften aus, die im gesamten Lebenszyklus von Analysedaten benötigt werden. Die Features der beiden vorhandenen Azure-Speicherdienste „Azure Blob Storage“ und „Azure Data Lake Storage Gen1“ wurden zusammengeführt. Features von [Azure Data Lake Storage Gen1](https://docs.microsoft.com/azure/data-lake-store/index), z.B. Dateisystemsemantik, Sicherheit und Skalierbarkeit auf Dateiebene, werden mit den kostengünstigen, mehrstufigen Speicherlösungen und Hochverfügbarkeits- und Notfallwiederherstellungsfunktionen von [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) kombiniert. 

Mit PolyBase können Sie Daten auch aus Azure Storage (im VNET gesichert) in Azure SQL Data Warehouse importieren. Außerdem wird auch das Exportieren von Daten aus Azure SQL Data Warehouse nach Azure Storage (im VNET gesichert) per PolyBase unterstützt. 

Weitere Informationen zu VNET-Dienstendpunkten in Azure SQL Data Warehouse finden Sie im [Blogbeitrag](https://azure.microsoft.com/blog/general-availability-of-vnet-service-endpoints-for-azure-sql-data-warehouse/) oder in der [Dokumentation](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json).

## <a name="automatic-performance-monitoring-preview"></a>Automatische Leistungsüberwachung (Vorschauversion)
Der [Abfragespeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-2017) ist jetzt als Vorschauversion für Azure SQL Data Warehouse verfügbar. Der Abfragespeicher ist als Hilfe für die Problembehandlung in Bezug auf die Leistung konzipiert, und es werden Abfragen, Abfragepläne, Laufzeitstatistiken und der Abfrageverlauf nachverfolgt, damit Sie die Aktivität und Leistung Ihrer Data Warehouse-Instanz überwachen können. Der Abfragespeicher umfasst eine Reihe von internen Speichern sowie dynamische Verwaltungssichten (DMVs), die Folgendes ermöglichen:

- Ermitteln und Optimieren von Abfragen mit dem höchsten Ressourcenverbrauch
- Erkennen und Verbessern von Ad-hoc-Workloads
- Auswerten der Abfrageleistung und der Auswirkungen auf den Plan anhand von Änderungen in Bezug auf Statistiken, Indizes oder die Systemgröße (DWU-Einstellung)
- Anzeigen des vollständigen Abfragetexts für alle ausgeführten Abfragen

Der Abfragespeicher enthält drei separate Speicher:  
- Einen Planspeicher für die Speicherung von Informationen zum Ausführungsplan 
- Einen Speicher für Laufzeitstatistiken zum Aufbewahren der Informationen zu den Ausführungsstatistiken
- Einen Speicher für Wartestatistiken zum Aufbewahren der Informationen aus den Wartestatistiken 

SQL Data Warehouse verwaltet diese Speicher automatisch und stellt ohne weitere Kosten eine unbegrenzte Anzahl von Abfragen bereit, die in den letzten sieben Tagen gespeichert wurden. Das Aktivieren des Abfragespeichers ist so einfach wie das Ausführen einer ALTER DATABASE-T-SQL-Anweisung:

```sql
ALTER DATABASE [Database Name] SET QUERY_STORE = ON;
```
Weitere Informationen zum Abfragespeicher in Azure SQL Data Warehouse finden Sie im Artikel [Leistungsüberwachung mit dem Abfragespeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-2017), und Informationen zu den Abfragespeicher-DMVs, z.B. sys.query_store_query, finden Sie in [diesem Artikel](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-query-transact-sql?view=sql-server-2017). In [diesem Blogbeitrag](https://azure.microsoft.com/blog/automatic-performance-monitoring-in-azure-sql-data-warehouse-with-query-store/) wird das Release angekündigt.

## <a name="lower-compute-tiers-for-azure-sql-data-warehouse-gen2"></a>Niedrigere Computetarife für Azure SQL Data Warehouse Gen2
Azure SQL Data Warehouse Gen2 unterstützt jetzt niedrigere Computetarife. Kunden profitieren von den führenden Azure SQL Data Warehouse-Features für Leistung, Flexibilität und Sicherheit, die bei 100 cDWU ([Compute Data Warehouse-Einheiten](https://docs.microsoft.com/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu)) beginnen und in Minutenschnelle auf 30.000 cDWU skaliert werden können. Ab Mitte Dezember 2018 können Kunden in [bestimmten Regionen](https://docs.microsoft.com/azure/sql-data-warehouse/gen2-lower-tier-regions) von der Gen2-Leistung und -Flexibilität bei niedrigeren Computetarifen profitieren. Für die restlichen Regionen beginnt die Verfügbarkeit im Laufe des Jahrs 2019.

Durch die Beseitigung des Einstiegspunkts für das Data Warehousing der nächsten Generation öffnet Microsoft die Türen für wertorientierte Kunden, die alle Vorteile eines sicheren, leistungsstarken Data Warehouse nutzen möchten, ohne erraten zu müssen, welche Testumgebung für sie am besten geeignet ist. Anstelle des aktuellen Einstiegspunkts von 500 cDWU können Kunden bereits mit nur 100 cDWU beginnen. SQL Data Warehouse Gen2 verfügt weiterhin über Unterstützung für Vorgänge zum Anhalten und Fortsetzen und bietet im Hinblick auf Computevorgänge weit mehr als nur die übliche Flexibilität. Gen2 unterstützt auch unbegrenzte Columnstore-Speicherkapazität sowie das 2,5-fache an Arbeitsspeicher pro Abfrage, bis zu 128 gleichzeitige Abfragen und das [adaptive Zwischenspeichern](https://azure.microsoft.com/blog/adaptive-caching-powers-azure-sql-data-warehouse-performance-gains/). Diese Features führen im Vergleich zur gleichen Data Warehouse-Einheit unter Gen1 im Durchschnitt zur fünffachen Leistung – und das zum gleichen Preis. Georedundante Sicherungen mit integrierter Datenschutzgarantie gehören in Gen2 zum Standard. Azure SQL Data Warehouse Gen2 kann bei Bedarf sofort skaliert werden.

## <a name="columnstore-background-merge"></a>Columnstore-Zusammenführung im Hintergrund
Standardmäßig speichert Azure SQL Data Warehouse (Azure SQL DW) Daten in einem Spaltenformat mit Mikropartitionen, die als [Zeilengruppen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-memory-optimizations-for-columnstore-compression) bezeichnet werden. In Situationen mit eingeschränkter Speicherkapazität bei der Indexerstellung oder dem Laden von Daten werden die Zeilengruppen mit weniger als der optimalen Größe von einer Million Zeilen komprimiert. Zeilengruppen können auch aufgrund von Löschungen fragmentiert werden. Kleine oder fragmentierte Zeilengruppen führen zu einer höheren Arbeitsspeichernutzung sowie zu ineffizienten Abfrageausführungen. In diesem Release von Azure SQL DW führt der Columnstore-Hintergrundwartungstask kleine komprimierte Zeilengruppen zusammen und erstellt damit größere Zeilengruppen, um den Speicher besser zu nutzen und die Abfrageausführung zu beschleunigen.

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
[Stack Overflow-Forum]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

---
title: Azure SQL Data Warehouse – Versionshinweise Oktober 2018 | Microsoft-Dokumentation
description: Versionshinweise zu Azure SQL Data Warehouse
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 12/04/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 9160a5f4e3a452682787ff500199e43e7fad0c77
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2019
ms.locfileid: "54213688"
---
# <a name="whats-new-in-azure-sql-data-warehouse-october-2018"></a>Neuerungen in Azure SQL Data Warehouse Oktober 2018
Azure SQL Data Warehouse wird fortlaufend verbessert. In diesem Artikel informieren wir Sie über neue Features und Änderungen, die im Oktober 2018 eingeführt wurden.

## <a name="devops-for-data-warehousing"></a>DevOps für Data Warehousing
Das oft geforderte Feature für SQL Data Warehouse (SQL DW) ist ab sofort in der Vorschau verfügbar und bietet Unterstützung für SQL Server Data Tools (SSDT) in Visual Studio! Entwicklerteams können nun über eine einzelne Codebasis mit Versionskontrolle zusammenarbeiten und rasch Änderungen in einer beliebigen Instanz auf der ganzen Welt bereitstellen. Möchten Sie daran teilhaben? Dieses Feature ist ab sofort als Vorschau verfügbar. Registrieren können Sie sich über das Formular [SQL Data Warehouse Visual Studio SQL Server Data Tools (SSDT) – Registrierung für die Vorschau](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR4-brmKy3TZOjoktwuHd7S1UODkwQ1lVMEw1NDBGRjNLRDNWOFlQRUpIRi4u). Angesichts der hohen Nachfrage prüfen wir die Zulassung zur Vorschau, um ein optimales Benutzererlebnis für unsere Kunden sicherzustellen. Sobald Sie sich registriert haben, bestätigen wir innerhalb von sieben Werktagen Ihren Status.

## <a name="row-level-security-generally-available"></a>Sicherheit auf Zeilenebene allgemein verfügbar
Azure SQL Data Warehouse (SQL DW) unterstützt ab sofort Sicherheit auf Zeilenebene (Row Level Security, RLS), sodass Sie eine leistungsstarke Funktion zum Schutz Ihrer sensiblen Daten erhalten. Mit der RLS können Sie Sicherheitsrichtlinien implementieren, um den Zugriff auf Zeilen in Ihren Tabellen zu regeln (um also zu bestimmen, wer auf welche Zeilen zugreifen darf). Die RLS ermöglicht diese engmaschige Zugriffssteuerung, ohne dass Sie Ihr Data Warehouse umgestalten müssen. Sie vereinfacht die allgemeine Sicherheitsimplementierung, da sich die Zugriffsbeschränkungslogik auf der Datenbankebene und nicht auf einer anderen, von den Daten getrennten Anwendungsebene befindet. Durch die RLS ist es zudem nicht mehr erforderlich, Ansichten zu erstellen, um Zeilen für die Zugriffssteuerung auszublenden. Dieses Sicherheitsfeature der Unternehmensklasse ist für all unsere Kunden kostenfrei.

## <a name="advanced-advisors"></a>Erweiterte Ratgeber
Die erweiterte Optimierung für Azure SQL Data Warehouse (SQL DW) wurde durch zusätzliche Data Warehouse-Empfehlungen und Metriken noch stärker vereinfacht. Ihnen stehen zusätzliche erweiterte Leistungsempfehlungen durch den Azure Advisor zur Verfügung. Hierzu zählen Folgende:
1.  Adaptiver Cache – Erhalten Sie Empfehlungen dafür, wann Sie eine Skalierung durchführen müssen, um die Cacheauslastung zu optimieren.
2.  Tabellenverteilung – Erfahren Sie, wann Tabellen repliziert werden müssen, um Datenverschiebungen zu reduzieren und die Workloadleistung zu steigern. 
3.  Tempdb – Finden Sie heraus, wann Ressourcenklassen skaliert und konfiguriert werden müssen, um tempdb-Konflikte zu reduzieren.

Data Warehouse-Metriken wurden stärker in [Azure Monitor](https://azure.microsoft.com/blog/enhanced-capabilities-to-monitor-manage-and-integrate-sql-data-warehouse-in-the-azure-portal/) integriert, was beispielsweise durch ein erweitertes, anpassbares Überwachungsdiagramm auf dem Übersichtsblatt für Metriken nahezu in Echtzeit realisiert wurde. Sie müssen nicht mehr das Data Warehouse-Übersichtsblatt verlassen, um im Rahmen der Nutzungsüberwachung oder der Validierung und Anwendung von Data Warehouse-Empfehlungen auf Azure Monitor-Metriken zugreifen zu können. Darüber hinaus stehen neue Metriken wie tempdb und die adaptive Cacheauslastung zur Verfügung, die Ihre Leistungsempfehlungen abrunden.

## <a name="advanced-tuning-with-integrated-advisors"></a>Erweiterte Optimierung durch integrierte Ratgeber
Noch stärker vereinfacht wurde die erweiterte Optimierung für Azure SQL Data Warehouse (SQL DW) durch zusätzliche Data Warehouse-Empfehlungen, Metriken und eine Neugestaltung des Portalübersichtsblatts, das eine integrierte Umgebung mit dem Azure Advisor und mit Azure Monitor bietet.

## <a name="accelerated-database-recovery-adr"></a>Schnellere Datenbankwiederherstellung
Accelerated Database Recovery (ADR) für Azure SQL Data Warehouse ist ab sofort als öffentliche Vorschau verfügbar. ADR ist eine neue SQL Server-Engine, mit der die Datenbankverfügbarkeit aufgrund eines Neuentwurfs des aktuellen Wiederherstellungsprozesses rundum verbessert wird, insbesondere bei Transaktionen mit langer Ausführungsdauer. Die wichtigsten Vorteile von ADR sind die schnelle und konsistente Datenbankwiederherstellung und der sofortige Transaktionsrollback.

## <a name="azure-monitor-diagnostics-logs"></a>Azure Monitor-Diagnoseprotokolle
SQL Data Warehouse (SQL DW) bietet dank der direkten Integration in Azure Monitor-Diagnoseprotokolle ab sofort bessere Einblicke in Analyseworkloads. Dank dieser neuen Funktion können Entwickler das Workloadverhalten über einen längeren Zeitraum analysieren und fundierte Entscheidungen zur Abfrageoptimierung oder Kapazitätsverwaltung treffen. Wir haben nun einen externen Protokollierungsprozess über [Azure Monitor-Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json#logs) eingeführt, die zusätzliche Einblicke in Ihre Data Warehouse-Workload bereitstellen. Mit einem einzigen Mausklick können Sie so jetzt Diagnoseprotokolle mithilfe von [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-queries) konfigurieren, um Funktionen zur Problembehandlung bezüglich der Leistung von Verlaufsabfragen nutzen zu können. Azure Monitor-Diagnoseprotokolle unterstützen anpassbare Aufbewahrungszeiträume, indem die Protokolle für Überwachungszwecke in einem Speicherkonto gespeichert werden, die Möglichkeit, Protokolle zur Bereitstellung von Telemetriedaten nahezu in Echtzeit an Event Hubs zu streamen, und die Option, Protokolle mithilfe von Log Analytics mit Protokollabfragen zu analysieren. Diagnoseprotokolle umfassen Telemetrieansichten zu Ihrem Data Warehouse, die den am häufigsten verwendeten DMVs zur Behandlung von Leistungsproblemen für SQL Data Warehouse entsprechen. Für dieses erste Release haben wir Ansichten für folgende dynamische Systemverwaltungssichten aktiviert:

- [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql)
- [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql)
- [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql)
- [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql)
- [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql)

## <a name="columnstore-memory-management"></a>Columnstore-Arbeitsspeicherverwaltung
Mit zunehmender Anzahl von komprimierten Columnstore-Zeilengruppen nimmt auch der Arbeitsspeicherbedarf für die Verwaltung der internen Spaltensegmentmetadaten für diese Zeilengruppen zu.  Dies kann die Abfrageleistung sowie Abfragen für einige der Columnstore-DMVs (Dynamic Management Views, dynamische Verwaltungssichten) beeinträchtigen.  In diesem Release wurden Verbesserungen implementiert, um die Größe der internen Metadaten für diese Fälle zu optimieren und so die Benutzerfreundlichkeit und Leistung solcher Abfragen zu verbessern. 

## <a name="azure-data-lake-storage-gen2-integration-ga"></a>Azure Data Lake Storage Gen2-Integration (allgemein verfügbar)
Azure SQL Data Warehouse (SQL DW) verfügt jetzt über native Integration in Azure Data Lake Storage Gen2. Kunden können nun Daten mithilfe von externen Tabellen von ABFS in SQL DW laden. Durch diese Funktion können Kunden Integrationen in ihre Data Lakes in Data Lake Storage Gen2 durchführen. 

## <a name="bug-fixes"></a>Fehlerbehebungen

| Titel | BESCHREIBUNG |
|:---|:---|
| **Fehler bei CETAS zu PARQUET in kleinen Ressourcenklassen in Data Warehouses vom Typ DW2000 und vieles mehr** | Durch diese Fehlerbehebung wird ein Nullverweis im Codepfad von CREATE EXTERNAL TABLE AS zu PARQUET korrekt identifiziert. |
|**Wert der Identitätsspalte kann in einigen CTAS-Vorgängen verloren gehen.** | Der Wert einer Identitätsspalte kann nicht beibehalten werden, wenn CTAS auf eine andere Tabelle angewendet wird. In einem Blog gemeldet: [https://blog.westmonroepartners.com/azure-sql-dw-identity-column-bugs/](https://blog.westmonroepartners.com/azure-sql-dw-identity-column-bugs/). |
| **Interner Fehler in einigen Fällen, wenn während der Ausführung einer Abfrage eine Sitzung beendet wird** | Dieser Fehlerbehebung löst eine InvalidOperationException aus, wenn eine Sitzung beendet wird, während die Abfrage weiterhin ausgeführt wird. |
| **(Bereitgestellt im November 2018) Kunden meldeten eine beeinträchtige Leistung beim Versuch, mithilfe von Polybase mehrere kleine Dateien aus ADLS (Gen1) zu laden.** | Die Validierung von AAD-Sicherheitstoken führte zu einem Engpass bei der Systemleistung. Die Leistungsprobleme konnten durch eine Aktivierung der Zwischenspeicherung für Sicherheitstoken abgemildert werden. |


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen von SQL Data Warehouse vertraut gemacht haben, können Sie sich nun darüber informieren, wie Sie schnell [eine SQL Data Warehouse-Instanz erstellen][create a SQL Data Warehouse]. Falls Sie mit Azure noch nicht vertraut sind und auf neue Terminologie stoßen, ist das [Azure-Glossar][Azure glossary] sehr nützlich. Weitere SQL Data Warehouse-Ressourcen:  

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

---
title: Azure SQL Data Warehouse – Versionshinweise | Microsoft-Dokumentation
description: Versionshinweise zu Azure SQL Data Warehouse
services: sql-data-warehouse
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
manager: craigg
ms.openlocfilehash: 51932ebf7d5bdc6830098ce7136a3eee7255ffe1
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245506"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Azure SQL Data Warehouse – Versionshinweise
In diesem Artikel sind die neuen Funktionen und Verbesserungen in den neuesten Versionen von [SQL Server auf virtuellen Azure-Computern](sql-data-warehouse-overview-what-is.md) zusammengefasst. Der Artikel enthält zudem wichtige Inhaltsaktualisierungen, die nicht in direktem Zusammenhang mit einer Version stehen, aber im gleichen Zeitraum veröffentlicht werden. Informationen zu Verbesserungen bei anderen Azure-Diensten finden Sie unter [Dienstupdates](https://azure.microsoft.com/updates).

## <a name="sql-data-warehouse-version-100101060-january"></a>SQL Data Warehouse-Version 10.0.10106.0 (Januar)

### <a name="service-improvements"></a>Verbesserungen beim Dienst

| Verbesserungen beim Dienst | Details |
| --- | --- |
| **Neustartfähigkeit von Abfragen – CTAS und INSERT/SELECT** | In seltenen Fällen (d.h. bei zeitweiligen Netzwerk-Verbindungsproblemen, Knotenausfällen) kann die Ausführung von Abfragen in Azure SQL DW zu Fehlern führen. Anweisungen mit längerer Ausführungszeit wie [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) und INSERT-SELECT-Vorgänge sind für dieses potenzielle Problem stärker anfällig. In diesem Release implementiert Azure SQL Data Warehouse zusätzlich zu den bereits angekündigten SELECT-Anweisungen eine Wiederholungslogik für CTAS und INSERT-SELECT-Anweisungen. Die Änderungen erlauben dem Dienst das transparente Behandeln vorübergehender Probleme und das Verhindern von Fehlern bei Abfragen. Die Anzahl der Wiederholungsversuche und die Liste der behandelten vorübergehenden Fehler sind vom konfiguriert.|
|**Rückgabereihenfolge nach Optimierung**|Abfragen vom Typ „SELECT…ORDER BY“ profitieren in diesem Release von einer Leistungssteigerung.   Jetzt senden alle Serverknoten ihre Ergebnisse an einen einzigen Serverknoten, der die Ergebnisse zusammenführt und sortiert und dann über den Serverknoten an den Benutzer zurückgibt.  Wenn die Abfrageergebnisse eine große Anzahl von Zeilen enthalten, bedeutet das Zusammenführen der Ergebnisse auf einem Serverknoten eine erhebliche Leistungssteigerung. Zuvor hat das Abfrageausführungsmodul Ergebnisse auf jedem Computeknoten geordnet und an den Steuerknoten gestreamt, auf dem sie dann zusammengeführt wurden.|
|**Verbesserung der Datenverschiebung für PartitionMove und BroadcastMove**|In Azure SQL Data Warehouse Gen2 verwenden die Datenverschiebungsschritte vom Typ ShuffleMove Techniken zur Datensofortverschiebung, die [im Blog zu Leistungsverbesserungen](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/) beschrieben sind. Ab diesem Release werden auch für die Datenverschiebungstypen „PartitionMove“ und „BroadcastMove“ auch diese Techniken zur Datensofortverschiebung verwendet. Benutzerabfragen, die diese Typen von Datenverschiebungsschritten verwenden, erfahren eine Leistungssteigerung. Um diese Leistungsverbesserungen zu nutzen, ist keine Codeänderung erforderlich.|

### <a name="documentation-improvements"></a>Verbesserungen bei der Dokumentation

| Verbesserungen bei der Dokumentation | Details |
| --- | --- |
|none | |
| | |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen eines SQL Data Warehouse](./create-data-warehouse-portal.md)

## <a name="more-information"></a>Weitere Informationen
- [Blog: Azure SQL Data Warehouse](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Customer Advisory Team-Blogs](https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/)
- [Kundenerfolgsgeschichten](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Stack Overflow-Forum](http://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
- [Videos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Azure-Glossar](../azure-glossary-cloud-terminology.md)
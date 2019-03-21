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
ms.openlocfilehash: 0b0d302dcd86f18eef6bf7600dd93878d0d9bd99
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57902687"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Azure SQL Data Warehouse – Versionshinweise
In diesem Artikel sind die neuen Funktionen und Verbesserungen in den neuesten Versionen von [Azure SQL Data Warehouse](sql-data-warehouse-overview-what-is.md) zusammengefasst. Der Artikel enthält zudem wichtige Inhaltsaktualisierungen, die nicht in direktem Zusammenhang mit einer Version stehen, aber im gleichen Zeitraum veröffentlicht werden. Informationen zu Verbesserungen bei anderen Azure-Diensten finden Sie unter [Dienstupdates](https://azure.microsoft.com/updates).

## <a name="sql-data-warehouse-version-100101060-january"></a>SQL Data Warehouse-Version 10.0.10106.0 (Januar)

### <a name="service-improvements"></a>Verbesserungen beim Dienst

| Verbesserungen beim Dienst | Details |
| --- | --- |
|**Rückgabereihenfolge nach Optimierung**|Abfragen vom Typ „SELECT…ORDER BY“ profitieren in diesem Release von einer Leistungssteigerung.   Jetzt senden alle Serverknoten ihre Ergebnisse an einen einzigen Serverknoten, der die Ergebnisse zusammenführt und sortiert und dann über den Serverknoten an den Benutzer zurückgibt.  Wenn die Abfrageergebnisse eine große Anzahl von Zeilen enthalten, bedeutet das Zusammenführen der Ergebnisse auf einem Serverknoten eine erhebliche Leistungssteigerung. Zuvor hat das Abfrageausführungsmodul Ergebnisse auf jedem Computeknoten geordnet und an den Steuerknoten gestreamt, auf dem sie dann zusammengeführt wurden.|
|**Verbesserung der Datenverschiebung für PartitionMove und BroadcastMove**|In Azure SQL Data Warehouse Gen2 verwenden die Datenverschiebungsschritte vom Typ ShuffleMove Techniken zur Datensofortverschiebung, die [im Blog zu Leistungsverbesserungen](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/) beschrieben sind. Ab diesem Release werden auch für die Datenverschiebungstypen „PartitionMove“ und „BroadcastMove“ auch diese Techniken zur Datensofortverschiebung verwendet. Benutzerabfragen, die diese Typen von Datenverschiebungsschritten verwenden, erfahren eine Leistungssteigerung. Um diese Leistungsverbesserungen zu nutzen, ist keine Codeänderung erforderlich.|
|**Relevante Fehler**|Falsche Version von Azure SQL Data Warehouse version: „SELECT @@VERSION“ gibt unter Umständen die falsche Version 10.0.9999.0 zurück. Die richtige Version für das aktuelle Release lautet 10.0.10106.0. Dieser Fehler wurde gemeldet und wird derzeit geprüft.

### <a name="documentation-improvements"></a>Verbesserungen bei der Dokumentation

| Verbesserungen bei der Dokumentation | Details |
| --- | --- |
|none | |
| | |

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen eines SQL Data Warehouse](./create-data-warehouse-portal.md)

## <a name="more-information"></a>Weitere Informationen
- [Blog: Azure SQL Data Warehouse](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Customer Advisory Team-Blogs](https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/)
- [Kundenerfolgsgeschichten](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
- [Videos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Azure-Glossar](../azure-glossary-cloud-terminology.md)

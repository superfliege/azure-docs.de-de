---
title: Azure SQL Data Warehouse – Versionshinweise | Microsoft-Dokumentation
description: Versionshinweise zu Azure SQL Data Warehouse
services: sql-data-warehouse
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
ms.openlocfilehash: e77556ac0d6f64797906c0f3b4181f147b1668e2
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448419"
---
# <a name="azure-sql-data-warehouse-release-notes-and-documentation-updates"></a>Updates an den Versionshinweisen und der Dokumentation zu Azure SQL Data Warehouse

Azure SQL Data Warehouse (SQL DW) ist ein cloudbasiertes Enterprise Data Warehouse mit MPP (Massively Parallel Processing) zur schnellen Ausführung komplexer Abfragen für mehrere Petabytes an Daten. Verwenden Sie SQL Data Warehouse als Schlüsselkomponente einer Big Data-Lösung. Importieren Sie große Datenmengen mit einfachen PolyBase-T-SQL-Abfragen in SQL Data Warehouse, und nutzen Sie dann die Vorteile von MPP für aufwendige Analysen. Im Zuge von Integrationen und Analysen wird das Data Warehouse zur Single Version of Truth, auf die sich Ihr Unternehmen bei der Gewinnung von Erkenntnissen verlassen kann.

Klicken Sie unten auf die Links, um weitere Informationen zu den neuen Features und Verbesserungen zu erhalten, die Sie in der aktuellen Version von Azure SQL Data Warehouse erwarten können. Sie können davon ausgehen, dass Sie diese Dienstupdates während Ihres identifizierten Wartungszeitplans erhalten.

- [März 2019](./release-notes-10-0-10106-0.md#march-2019)
- [Januar 2019](./release-notes-10-0-10106-0.md#january-2019)
- [Dezember 2018](./release-notes-10-0-10106-0.md#december-2018)
- [Oktober 2018](./release-notes-10-0-10106-0.md#october-2018)
- [September 2018](./release-notes-september-2018.md)
- [August 2018](./release-notes-august-2018.md)
- [Juli 2018](./release-notes-july-2018.md)
- [Juni 2018](./release-notes-june-2018.md)
- [Mai 2018](./release-notes-may-2018.md)

## <a name="checking-the-code-version-that-has-been-applied-to-your-data-warehouse"></a>Überprüfen der Codeversion, die auf Ihre Data Warehouse-Instanz angewendet wurde

Hiermit wird bestätigt, welches Release auf Ihre Data Warehouse-Instanz angewendet wurde. Stellen Sie per SSMS eine Verbindung mit Ihrem Data Warehouse her, und führen Sie die folgende Syntax aus, um die aktuelle Version von SQL Data Warehouse zurückzugeben.

```sql
SELECT @@VERSION AS 'SQL Data Warehouse';
```

Beispielausgabe: ![SQL Data Warehouse-Version](./media/release-notes/sql_data_warehouse_version.png)

Verwenden Sie das identifizierte Datum, um zu bestätigen, welches Release auf Ihre Azure SQL Data Warehouse-Instanz angewendet wurde. 


## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule) zum Anzeigen eines Wartungszeitplans 
- [Weitere Informationen](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule) zum Ändern eines Wartungszeitplans
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric) zum Erstellen, Anzeigen und Verwalten von Warnungen mit Azure Monitor
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) zu Webhookaktionen für Protokollwarnungsregeln
- [Weitere Informationen](https://docs.microsoft.com/azure/service-health/service-health-overview) zu Azure Service Health
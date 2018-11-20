---
title: Azure SQL Data Warehouse – Versionshinweise | Microsoft-Dokumentation
description: Versionshinweise zu Azure SQL Data Warehouse
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 11/11/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: b702d375f7a66843918a960ca3783c078eac541e
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51579315"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Azure SQL Data Warehouse – Versionshinweise

Azure SQL Data Warehouse ist ein cloudbasiertes Enterprise Data Warehouse (EDW) mit Massively Parallel Processing (MPP) zur schnellen Ausführung komplexer Abfragen für mehrere Petabyte an Daten. Verwenden Sie SQL Data Warehouse als Schlüsselkomponente einer Big Data-Lösung. Importieren Sie große Datenmengen mit einfachen PolyBase-T-SQL-Abfragen in SQL Data Warehouse, und nutzen Sie dann die Vorteile von MPP für aufwendige Analysen. Im Zuge von Integrationen und Analysen wird das Data Warehouse zur Single Version of Truth, auf die sich Ihr Unternehmen bei der Gewinnung von Erkenntnissen verlassen kann.

Klicken Sie unten auf die Links, um weitere Informationen zu den neuen Features und Verbesserungen zu erhalten, die Sie in der aktuellen Version von Azure SQL Data Warehouse erwarten können. Sie können davon ausgehen, dass Sie diese Dienstupdates während Ihres identifizierten Wartungszeitplans erhalten.

- [Oktober 2018](./release-notes-october-2018.md)
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

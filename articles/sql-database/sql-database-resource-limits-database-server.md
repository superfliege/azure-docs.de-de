---
title: Ressourceneinschränkungen für Azure SQL-Datenbank-Server | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die Ressourcenlimits von Azure SQL-Datenbank-Servern für Einzeldatenbanken und Pools für elastische Datenbanken. Darüber hinaus bietet er Informationen darüber, was geschieht, wenn diese Ressourcenlimits erreicht oder überschritten werden.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan,moslake
manager: craigg
ms.date: 11/13/2018
ms.openlocfilehash: 57ff6025cd60d786931177f8bc0cc22e9baa60e7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55479123"
---
# <a name="sql-database-resource-limits-for-azure-sql-database-server"></a>SQL-Datenbank-Ressourcenlimits für Azure SQL-Datenbank-Server

Dieser Artikel bietet eine Übersicht über die SQL-Datenbank-Ressourcenlimits für einen SQL-Datenbank-Server, der Einzeldatenbanken und Pools für elastische Datenbanken verwaltet. Darüber hinaus bietet er Informationen darüber, was geschieht, wenn diese Ressourcenlimits erreicht oder überschritten werden.

> [!NOTE]
> Informationen zu den Grenzwerten für verwaltete Instanzen finden Sie unter [SQL-Datenbank-Ressourcenlimits für verwaltete Instanzen](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>Maximale Ressourcenlimits

| Ressource | Begrenzung |
| :--- | :--- |
| Datenbanken pro Server | 5.000 |
| Standardanzahl von Servern pro Abonnement in beliebiger Region | 20 |
| Maximale Anzahl von Servern pro Abonnement in beliebiger Region | 200 |  
| DTU/eDTU-Kontingent pro Server | 54.000 |  
| Virtuelle Kerne pro Server/Instanz | 540 |
| Maximale Anzahl von Pools pro Server | Begrenzt durch die Anzahl von DTUs oder virtuellen Kernen. Beispiel: Wenn jeder Pool 1.000 DTUs umfasst, kann ein Server 54 Pools unterstützen.|
||||

> [!NOTE]
> Um ein höheres DTU/eDTU-Kontingent oder eine höhere Anzahl von virtuellen Kernen oder Servern als die Standardanzahl zu erhalten, können Sie im Azure-Portal eine neue Supportanfrage für das Abonnement mit dem Problemtyp „Kontingent“ übermitteln. Durch das DTU/eDTU-Kontingent und das Limit für Datenbanken pro Server wird die Anzahl der Pools für elastische Datenbanken pro Server eingeschränkt.
> [!IMPORTANT]
> Wenn sich die Anzahl von Datenbanken dem Grenzwert pro SQL-Datenbank-Server nähert, kann Folgendes geschehen:
> - Höhere Latenz bei der Ausführung von Abfragen, die die Masterdatenbank betreffen.  Dies bezieht sich auch auf die Ansichten der Ressourcennutzungsstatistiken (z. B. sys.resource_stats) ein.
> - Höhere Latenz bei Verwaltungsvorgängen und dem Rendern von Portalblickpunkten. Dazu gehört auch das Aufzählen von Datenbanken auf dem Server.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Was geschieht, wenn die Datenbankressourcen erreicht werden?

### <a name="compute-dtus-and-edtus--vcores"></a>Compute (DTUs und eDTUs / vCores)

Bei zunehmender Datenbank-Computenutzung (gemessen an DTUs und eDTUs oder vCores) steigt die Abfragewartezeit und kann auch durch einen Timeout beendet werden. Unter diesen Bedingungen können Abfragen durch den Dienst in eine Warteschlange gestellt werden, und ihnen werden Ressourcen für die Ausführung bereitgestellt, sobald diese frei werden.
Wenn eine hohe Computenutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:

- Erhöhen der Computegröße der Datenbank oder des Pools für elastische Datenbanken, um mehr Computeressourcen für die Datenbank bereitzustellen. Siehe [Skalieren der Ressourcen für einzelne Datenbanken](sql-database-single-database-scale.md) und [Skalieren der Ressourcen für elastische Pools in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md).
- Optimieren der Abfragen, um die Ressourcenverwendung für jede Abfrage zu reduzieren. Weitere Informationen finden Sie unter [Abfrageoptimierung/Abfragehinweise](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Wenn der verwendete Datenbankspeicherplatz die maximale Größe erreicht, treten bei Einfügungen in die Datenbank und Updates, die die Datenmenge erhöhen, Fehler auf, sodass Clients eine [Fehlermeldung](sql-database-develop-error-messages.md) erhalten. Die Datenbankanweisungen SELECT und DELETE werden weiterhin erfolgreich ausgeführt.

Wenn eine hohe Speicherplatznutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:

- Erhöhen der maximalen Größe von Datenbank oder Pool für elastische Datenbanken bzw. Hinzufügen von weiterem Speicher. Siehe [Skalieren der Ressourcen für einzelne Datenbanken](sql-database-single-database-scale.md) und [Skalieren der Ressourcen für elastische Pools in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md).
- Wenn sich die Datenbank in einem Pool für elastische Datenbanken befindet, kann sie auch aus dem Pool heraus verschoben werden, damit ihr Speicherplatz nicht mit anderen Datenbanken gemeinsam genutzt wird.
- Verkleinern Sie eine Datenbank, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

### <a name="sessions-and-workers-requests"></a>Sitzungen und Worker (Anforderungen)

Die maximale Anzahl von Sitzungen und Workern wird durch Diensttarif und Computegröße (DTUs und eDTUs) bestimmt. Neue Anforderungen werden abgelehnt, wenn Sitzung oder Worker Grenzwerte erreicht haben, und Clients erhalten eine Fehlermeldung. Während die Anzahl der verfügbaren Verbindungen von der Anwendung gesteuert werden kann, ist die Anzahl paralleler Worker oft schwieriger zu schätzen und zu steuern. Dies gilt insbesondere während der Spitzenlastzeiten, wenn Datenbankressourcen-Grenzen erreicht werden und Worker aufgrund länger laufender Abfragen Schlange stehen.

Wenn eine hohe Sitzungs- oder Workernutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:

- Erhöhen des Diensttarifs oder der Computegröße der Datenbank oder des Pools für elastische Datenbanken. Siehe [Skalieren der Ressourcen für einzelne Datenbanken](sql-database-single-database-scale.md) und [Skalieren der Ressourcen für elastische Pools in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md).
- Optimieren von Abfragen, um die Ressourcenverwendung durch die einzelnen Abfragen zu verringern, wenn die Ursache der zunehmenden Auslastung durch Worker Konflikte bezüglich der Computeressourcen sind. Weitere Informationen finden Sie unter [Abfrageoptimierung/Abfragehinweise](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Nächste Schritte

- Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md).
- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
- Informationen zu DTUs und eDTUs finden Sie unter [Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs)](sql-database-service-tiers.md#dtu-based-purchasing-model).
- Informationen zu Größenbeschränkungen für tempdb finden Sie unter [Tempdb-Datenbank in SQL-Datenbank](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).

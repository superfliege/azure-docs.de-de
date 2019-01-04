---
title: Konfigurieren von Azure SQL-Datenbank | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure SQL-Datenbank konfigurieren und verwalten.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: b4dd21324591075d7625a82fbbb661c4a8e84b1d
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53439537"
---
# <a name="how-to-use-azure-sql-database"></a>Verwenden von Azure SQL-Datenbank

In diesem Abschnitt sind verschiedene Anleitungen, Skripts und Erläuterungen aufgeführt, die beim Verwalten und Konfigurieren von Azure SQL-Datenbank hilfreich sein können. Spezifische Anleitungen für [Einzeldatenbank](sql-database-howto-single-database.md) und [Verwaltete Instanz](sql-database-howto-managed-instance.md) stehen ebenfalls zur Verfügung.

## <a name="load-data"></a>Laden von Daten

- [Kopieren einer Einzeldatenbank innerhalb von Azure](https://docs.microsoft.com/azure/sql-database/sql-database-copy)
- [Importieren einer Datenbank über eine BACPAC-Datei](https://docs.microsoft.com/azure/sql-database/sql-database-import)
- [Exportieren einer Datenbank in eine BACPAC-Datei](https://docs.microsoft.com/azure/sql-database/sql-database-export)
- [Laden von Daten mit BCP](https://docs.microsoft.com/azure/sql-database/sql-database-load-from-csv-with-bcp)
- [Laden von Daten mit ADF](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Datensynchronisierung

- [SQL-Datensynchronisierung](https://docs.microsoft.com/azure/sql-database/sql-database-sync-data)
- [Datensynchronisierungs-Agent](https://docs.microsoft.com/azure/sql-database/sql-database-data-sync-agent)
- [Replizieren von Schemaänderungen](https://docs.microsoft.com/azure/sql-database/sql-database-update-sync-schema)
- [Überwachen mit OMS](https://docs.microsoft.com/azure/sql-database/sql-database-sync-monitor-oms)
- [Bewährte Methoden für die Datensynchronisierung](https://docs.microsoft.com/azure/sql-database/sql-database-best-practices-data-sync)
- [Problembehandlung für die Datensynchronisierung](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-data-sync)

## <a name="monitoring-and-tuning"></a>Überwachung und Optimierung

-  [Manuelles Optimieren](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
- [Überwachen der Leistung mithilfe von DMVs](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
- [Überwachen der Leistung mit dem Abfragespeicher](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)
- [Behandeln von Leistungsproblemen mithilfe von Intelligent Insights](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-troubleshoot-performance)
- [Verwenden des Intelligent Insights-Diagnoseprotokolls](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-use-diagnostics-log)
- [Überwachen des In-Memory-OLTP-Speicherplatzes](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitoring)

### <a name="extended-events"></a>Erweiterte Ereignisse

- [Erweiterte Ereignisse](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-db-diff-from-svr)
- [Speichern erweiterter Ereignisse in der Ereignisdatei](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-event-file)
- [Speichern erweiterter Ereignisse im Ringpuffer](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-ring-buffer)

## <a name="configure-features"></a>Konfigurieren von Features

- [Konfigurieren der Azure AD-Authentifizierung](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)
- [Konfigurieren des bedingten Zugriffs](https://docs.microsoft.com/azure/sql-database/sql-database-conditional-access)
- [Mehrstufige AAD-Authentifizierung](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Konfigurieren der mehrstufigen Authentifizierung](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication-configure)
- [Konfigurieren einer temporalen Aufbewahrungsrichtlinie](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables-retention-policy)
- [Konfigurieren von TDE mit BYOK](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure)
- [Rotieren von TDE-BYOK-Schlüsseln](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-key-rotation)
- [Entfernen einer TDE-Schutzvorrichtung](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-remove-tde-protector)
- [Konfigurieren von In-Memory-OLTP](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-migration)
- [Konfigurieren von Azure Automation](https://docs.microsoft.com/azure/sql-database/sql-database-manage-automation)

## <a name="develop-applications"></a>Entwickeln von Anwendungen

- [Konnektivität](https://docs.microsoft.com/azure/sql-database/sql-database-libraries)
- [Verwenden des Spark-Connectors](https://docs.microsoft.com/azure/sql-database/sql-database-spark-connector)
- [Authentifizieren der App](https://docs.microsoft.com/azure/sql-database/sql-database-client-id-keys)
- [Fehlermeldungen](https://docs.microsoft.com/azure/sql-database/sql-database-develop-error-messages)
- [Verwenden der Batchverarbeitung für bessere Leistung](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance)
- [Konnektivitätsleitfaden](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues)
- [DNS-Aliase](https://docs.microsoft.com/azure/sql-database/dns-alias-overview)
- [Einrichten eines DNS-Alias über PowerShell](https://docs.microsoft.com/azure/sql-database/dns-alias-powershell)
- [Ports – ADO.NET](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12)
- [C und C ++](https://docs.microsoft.com/azure/sql-database/sql-database-develop-cplusplus-simple)
- [Excel](https://docs.microsoft.com/azure/sql-database/sql-database-connect-excel)

## <a name="design-applications"></a>Entwerfen von Anwendungen

- [Entwurf für die Notfallwiederherstellung](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery)
- [Entwurf für Pools für elastische Datenbanken](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool)
- [Entwurf für App-Upgrades](https://docs.microsoft.com/azure/sql-database/sql-database-manage-application-rolling-upgrade)

### <a name="design-multi-tenant-saas-applications"></a>Entwurf von SaaS-Anwendungen mit mehreren Mandanten

- [SaaS-Entwurfsmuster](https://docs.microsoft.com/azure/sql-database/saas-tenancy-app-design-patterns)
- [SaaS-Video Indexer](https://docs.microsoft.com/azure/sql-database/saas-tenancy-video-index-wingtip-brk3120-20171011)
- [SaaS-App-Sicherheit](https://docs.microsoft.com/azure/sql-database/saas-tenancy-elastic-tools-multi-tenant-row-level-security)

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Anleitungen zum Verwenden einer verwalteten Instanz](sql-database-howto-managed-instance.md).
- Erfahren Sie mehr über [Anleitungen zum Verwenden einer Einzeldatenbank](sql-database-howto-single-database.md).

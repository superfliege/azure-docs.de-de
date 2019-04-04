---
title: Azure SQL-Datenbank – Versionshinweise | Microsoft-Dokumentation
description: Weitere Informationen zu den neuen Features und Verbesserungen in Azure SQL-Datenbank und in der zugehörigen Dokumentation
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: carlrab
ms.openlocfilehash: 6600a578ba9c73c8a2c71466fd0b008f19058b80
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57861287"
---
# <a name="sql-database-release-notes"></a>Versionshinweise zu SQL-Datenbank

In diesem Artikel erhalten Sie weitere Informationen zu den neuen Features und Verbesserungen in Azure SQL-Datenbank und in der zugehörigen Dokumentation. Die Verbesserungen an SQL-Datenbank finden Sie außerdem unter [Azure-Updates](https://azure.microsoft.com/updates/?product=sql-database). Informationen zu Verbesserungen bei anderen Azure-Diensten finden Sie unter [Dienstupdates](https://azure.microsoft.com/updates).

## <a name="march-2019"></a>März 2019

### <a name="service-improvements"></a>Verbesserungen beim Dienst

| Verbesserungen beim Dienst | Details |
| --- | --- |
| In Kürze verfügbar ||
| &nbsp; |

### <a name="documentation-improvements"></a>Verbesserungen bei der Dokumentation

| Verbesserungen bei der Dokumentation | Details |
| --- | --- |
| Hinzugefügt: Protokollbeschränkungen für Singletons|Weitere Informationen finden Sie unter [Limits des auf virtuellen Kernen basierenden Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank](sql-database-vcore-resource-limits-single-databases.md).|
| Hinzugefügt: Protokollbeschränkungen für Pools für elastische Datenbanken und Pooldatenbanken|Weitere Informationen finden Sie unter [Ressourcenlimits für Pools für elastische Datenbanken, die das V-Kern-basierte Kaufmodell verwenden](sql-database-vcore-resource-limits-elastic-pools.md).|
| Hinzugefügt: Transaktionsprotokollratengovernance| Neuer Inhalt für die [Transaktionsprotokollratengovernance](sql-database-resource-limits-database-server.md#transaction-log-rate-governance)|
| Aktualisiert: PowerShell-Beispiele für Singletons und Pools für elastische Datenbanken für die Verwendung des az.sql-Moduls | Weitere Informationen finden Sie in den [PowerShell-Beispielen für Singletons und Pools für elastische Datenbanken](sql-database-powershell-samples.md#single-database-and-elastic-pools).|
| &nbsp; |

## <a name="february-2019"></a>Februar 2019

### <a name="service-improvements"></a>Verbesserungen beim Dienst

| Verbesserungen beim Dienst | Details |
| --- | --- |
|Allgemein verfügbar: Erstellen eines fortsetzbaren Onlineindizes| Weitere Informationen finden Sie unter [CREATE INDEX (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql).|
|Verbessert: Unterstützung für verwaltete Instanzen für Routingtabellen| Weitere Informationen finden Sie unter [Netzwerkanforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements).|
|Hinzugefügt: Unterstützung für das Umbenennen von Datenbank in verwalteten Instanzen | Weitere Informationen finden Sie in der Syntax zu [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) und [sp_rename](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-rename-transact-sql).|
|SQL-Datenbank als Quelle von Verweisdaten für Stream Analytics | Weitere Informationen finden Sie unter [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).|
|Hinzugefügt: Unterstützung für verwaltete Instanzen im Datenmigrations-Assistenten |Weitere Informationen finden Sie unter [What's new in DMA (Neuerungen im Datenmigrations-Assistenten)](https://docs.microsoft.com/sql/dma/dma-whatsnew).|
|Hinzugefügt: Unterstützung für Zielbereitschaftsüberprüfung für verwaltete Instanzen in SQL Server Migration Assistant | Weitere Informationen finden Sie unter [SQL Server Migration Assistant](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant).
|Hinzugefügt: Unterstützung der Migration von Amazon RDS zu verwalteten Instanzen im Datenmigrationsdienst | Weitere Informationen finden Sie unter [Tutorial: Migrieren von RDS SQL Server zu Azure SQL-Datenbank oder zu einer verwalteten Azure SQL-Datenbank-Instanz mithilfe von DMS (online)](../dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online.md).|
| &nbsp; |

### <a name="documentation-improvements"></a>Verbesserungen bei der Dokumentation

| Verbesserungen bei der Dokumentation | Details |
| --- | --- |
|Hinzugefügt: Erläuterungen zu den Bereitstellungsoptionen für verwaltete Instanzen|Viele Artikel wurden aktualisiert, um die Anwendbarkeit der Bereitstellungsoptionen auf Singletons, Pools für elastische Datenbanken und verwaltete Instanzen näher zu erläutern. |
|Aktualisiert: tempdb-Größen für das DTU-basierte Kaufmodell | Weitere Informationen finden Sie unter [Tempdb-Datenbank in SQL-Datenbank](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
|Aktualisiert: Unterstützung für das Importieren und Exportieren mit BACPAC-Datei für verwaltete Instanzen| Weitere Informationen finden Sie unter [Importieren einer BACPAC-Datei](sql-database-import.md) und [Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei](sql-database-export.md). |
| &nbsp; |


## <a name="january-2019"></a>Januar 2019

### <a name="service-improvements"></a>Verbesserungen beim Dienst

| Verbesserungen beim Dienst | Details |
| --- | --- |
| Zusätzliche Optionen für die Granularität von Computeressourcen | Die Computeoptionen für die Dienstebenen „Universell“ und „Unternehmenskritisch“ für [Singletons](sql-database-vcore-resource-limits-single-databases.md) und [Pools für elastische Datenbanken](sql-database-vcore-resource-limits-elastic-pools.md) präziser konfiguriert werden.|
| Anzeige von Überwachungsdatensätzen für verwaltete Instanzen im Azure-Portal | Die Anzeige von [Überwachungsdatensätzen für verwaltete Instanzen](sql-database-managed-instance-auditing.md) im Azure-Portal wird nun unterstützt.|
| Das Feature „Advanced Thread Detection“ wurde in „Advanced Data Security“ umbenannt. | Das Feature „Advanced Thread Detection“ wurde für Singletons, Pools für elastische Datenbanken und verwaltete Instanzen in [Advanced Data Security](sql-advanced-threat-protection.md) umbenannt. |
| &nbsp; |

### <a name="documentation-improvements"></a>Verbesserungen bei der Dokumentation

| Verbesserungen bei der Dokumentation | Details |
| --- | --- |
| Verwaltete Instanzen und Transaktionsreplikation | Ein Artikel über die Verwendung der [Transaktionsreplikation mit verwalteten Instanzen](replication-with-sql-database-managed-instance.md) wurde hinzugefügt. |
| Hinzugefügt: Tutorial zu Azure AD mit verwalteten Instanzen | In diesem Tutorial zu [Azure AD mit verwalteten Instanzen](sql-database-managed-instance-aad-security-tutorial.md) wird veranschaulicht, wie Sie die Sicherheit von verwalteten Instanzen mithilfe von Azure AD-Anmeldungen konfigurieren und testen können. |
| Aktualisiert: Auftragsautomatisierung mit Transact-SQL-Skripts | Die Erläuterungen zur Verwendung der [Auftragsautomatisierung mithilfe von Transact-SQL-Skripts](sql-database-job-automation-overview.md) für Singletons, Pools für elastische Datenbanken und verwaltete Instanzen wurden aktualisiert und verbessert. |
| Aktualisiert: Sicherheit für verwaltete Instanzen | Die Erläuterungen zum [Sicherheitsmodell für verwaltete Instanzen](sql-database-security-overview.md) wurden aktualisiert und verbessert, und es wurden Vergleiche mit den Sicherheitsmodellen für Singletons und Pools für elastische Datenbanken hinzugefügt. |
| Aktualisiert: sämtliche Schnellstartanleitungen und Tutorials | Alle Schnellstarts und Tutorials in der [Dokumentation](https://docs.microsoft.com/azure/sql-database) wurden aktualisiert, damit sie den Änderungen im Azure-Portal entsprechen. |
| Hinzugefügt: Schnellstartanleitungen | Schnellstartanleitungen für [Singletons](sql-database-quickstart-guide.md) und [verwaltete Instanzen](sql-database-managed-instance-quickstart-guide.md) wurden hinzugefügt. |
| Hinzugefügt: SQL-Datenbank-Glossar | In diesem [Glossar](sql-database-glossary-terms.md) finden Sie eine Liste der festgelegten Begriffe für SQL-Datenbank und Links zu den Seiten, auf denen die Begriffe im entsprechenden Kontext erläutert werden. |
| &nbsp; |

## <a name="contribute-to-content-improvement"></a>Mitwirken an der Verbesserung des Inhalts

Bei der Dokumentation zu Azure SQL-Datenbank handelt es sich um eine Open Source-Dokumentation. Dadurch entstehen mehrere Vorteile:

- Durch die öffentliche Planung in Open Source-Repositorys erhalten wir Feedback zu benötigten Dokumentationen.
- Durch die öffentliche Prüfung in Open Source-Repositorys können wir mit dem ersten Release den bestmöglichen Inhalt veröffentlichen.
- Durch öffentliche Updates in Open Source-Repositorys können wir Inhalt kontinuierlich verbessern.

Weitere Informationen zum Mitwirken an der Dokumentation zu Azure SQL-Datenbank finden Sie unter [Leitfaden für Mitwirkende an der Microsoft-Dokumentation: Übersicht](https://docs.microsoft.com/contribute/). Die Benutzeroberfläche auf [docs.microsoft.com](https://docs.microsoft.com/) bezieht [GitHub-Workflows](https://github.com/) direkt ein, um den Prozess noch einfacher zu gestalten. Beginnen Sie mit dem [Bearbeiten des angezeigten Dokuments](https://docs.microsoft.com/contribute/#quick-edits-to-existing-documents). Alternativ können Sie [neue Artikel prüfen](https://docs.microsoft.com/contribute/#review-open-prs) oder [nützliche Tickets erstellen](https://docs.microsoft.com/contribute/#create-quality-issues).

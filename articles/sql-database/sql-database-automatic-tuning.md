---
title: Azure SQL-Datenbank – Automatische Optimierung | Microsoft-Dokumentation
description: Azure SQL-Datenbank analysiert SQL-Abfragen und führt eine automatische Anpassung an die Benutzerworkload durch.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: dd6e8f5f46e9fdf6887cc2a0b0c7b15bbd00fabd
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626198"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatische Optimierung in Azure SQL-Datenbank

Die automatische Optimierung von Azure SQL-Datenbank sorgt dank kontinuierlicher, KI-basierter Leistungsoptimierung für optimale Leistung und stabile Workloads.

Bei der automatischen Optimierung handelt es sich um einen vollständig verwalteten Dienst mit integrierter Intelligenz, der kontinuierlich Abfragen für eine Datenbank überwacht und automatisch deren Leistung verbessert. Dies wird durch eine dynamische Anpassung der Datenbank an die unterschiedlichen Workloads sowie durch die Anwendung von Optimierungsempfehlungen erreicht. Dank künstlicher Intelligenz lernt die automatische Optimierung horizontal von allen Datenbanken in Azure und verbessert dynamisch ihre Optimierungsmaßnahmen. Je länger die automatische Optimierung für eine Azure SQL-Datenbank verwendet wird, desto besser ist ihre Leistung.

Die automatische Optimierung von Azure SQL-Datenbank ist vielleicht eines der wichtigsten Features, die Sie aktivieren können, um stabile Workloads mit optimaler Leistung zu erhalten.

## <a name="what-can-automatic-tuning-do-for-you"></a>Vorteile der automatischen Optimierung

- Automatisierte Optimierung der Leistung von Azure SQL-Datenbanken
- Automatisierte Überprüfung von Leistungszuwächsen
- Automatisches Rollback und automatische Korrektur
- Protokollierung des Optimierungsverlaufs
- Optimierung von T-SQL-Aktionsskripts für manuelle Bereitstellungen
- Proaktive Überwachung der Workloadleistung
- Funktion für das horizontale Hochskalieren von einigen Hunderttausend Datenbanken
- Positive Auswirkung auf DevOps-Ressourcen und die Gesamtkosten

## <a name="safe-reliable-and-proven"></a>Sicher, zuverlässig und bewährt

Die Leistung anspruchsvoller Workloads wird durch auf Azure SQL-Datenbanken angewendete Optimierungsmaßnahmen in keinem Fall beeinträchtigt. Bei der Entwicklung des Systems wurde sehr genau darauf geachtet, dass es zu keinerlei Störung der Benutzerworkloads kommt. Empfehlungen der automatisierten Optimierung werden nur bei geringer Auslastung angewendet. Zur Wahrung der Workloadleistung kann das System automatische Optimierungsmaßnahmen auch vorübergehend deaktivieren. In diesem Fall wird im Azure-Portal eine entsprechende Meldung angezeigt. Die automatische Optimierung betrachtet Workloads mit der höchsten Ressourcenpriorität.

Die Mechanismen für die automatische Optimierung sind ausgereift und wurden auf der Grundlage hunderttausender, in Azure ausgeführter Datenbanken perfektioniert. Angewendete automatisierte Optimierungsmaßnahmen werden automatisch überprüft, um sicherzustellen, dass sie sich positiv auf die Workloadleistung auswirken. Zurückgestellte Leistungsempfehlungen werden dynamisch erkannt und unverzüglich zurückgesetzt. Die Optimierungsverbesserungen, die für die einzelnen Azure SQL-Datenbanken vorgenommen wurden, werden transparent im Optimierungsverlaufsprotokoll dokumentiert. 

![Wie funktioniert die automatische Optimierung?](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Die automatische Optimierung von Azure SQL-Datenbank teilt sich seine Kernlogik mit der SQL Server-Engine für die automatische Optimierung. Weitere technische Informationen zum integrierten Intelligenzmechanismus finden Sie unter [Automatic tuning](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) (Automatische Optimierung).

## <a name="use-automatic-tuning"></a>Verwenden der automatischen Optimierung

Die automatische Optimierung muss für Ihr Abonnement manuell aktiviert werden. Informationen zum Aktivieren der automatischen Optimierung über das Azure-Portal finden Sie unter [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).

Die automatische Optimierung kann autonom arbeiten, automatisch Optimierungsempfehlungen anwenden und Leistungszuwächse automatisch überprüfen. 

Zur präziseren Steuerung können Sie die automatische Anwendung von Optimierungsempfehlungen deaktivieren und Optimierungsempfehlungen manuell über das Azure-Portal anwenden. Sie können die Lösung auch zum reinen Anzeigen von Empfehlungen der automatischen Optimierung verwenden und sie über Skripts und Tools Ihrer Wahl manuell anwenden. 

Im folgenden Video können Sie sich einen Überblick über die Funktionsweise der automatischen Optimierung in typischen Verwendungsszenarien verschaffen:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

## <a name="automatic-tuning-options"></a>Optionen für die automatische Optimierung

In Azure SQL-Datenbank stehen folgende Optionen für die automatische Optimierung zur Verfügung:
 1. **CREATE INDEX:** identifiziert Indizes, die die Leistung Ihrer Workload verbessern können, erstellt Indizes und überprüft automatisch, ob die Leistung der Abfragen verbessert wurde.
 2. **DROP INDEX:** identifiziert redundante und doppelte Indizes sowie Indizes, die über einen langen Zeitraum hinweg nicht verwendet wurden. Beachten Sie, dass diese Option nicht kompatibel mit Anwendungen ist, die Partitionswechsel und Indexhinweise verwenden.
 3. **FORCE LAST GOOD PLAN:** identifiziert SQL-Abfragen mit einem Ausführungsplan, der langsamer als der vorherige gute Plan ist, und Abfragen, die den letzten bekannten guten Plan anstelle des zurückgestellten Plans verwenden.

Azure SQL-Datenbank identifiziert die Empfehlungen von **CREATE INDEX**, **DROP INDEX** und **FORCE LAST GOOD PLAN**, die Ihre Datenbank optimieren können, und zeigt sie im Azure-Portal an. Weitere Informationen zur Identifikation von Indizes, die geändert werden sollten, finden Sie unter [Azure SQL Database Advisor im Azure-Portal](sql-database-advisor-portal.md). Sie können Empfehlungen entweder manuell anwenden, indem Sie das Portal verwenden, oder Sie können einstellen, dass Azure SQL-Datenbank Empfehlungen automatisch anwendet, die Workload nach den Änderungen überwacht und überprüft, dass die Empfehlung die Leistung Ihrer Workload verbessert. 

Die Optionen für die automatische Optimierung können unabhängig pro Datenbank aktiviert oder deaktiviert werden. Sie können auch auf einem logischen Server konfiguriert und auf jede Datenbank angewandt werden, die Einstellungen von diesem Server erbt. Logische Server können Azure-Standardwerte für die Einstellungen für automatische Optimierung erben. In den Azure-Standardwerten sind derzeit FORCE_LAST_GOOD_PLAN und CREATE_INDEX aktiviert und DROP_INDEX deaktiviert.

Das Konfigurieren der Optionen für die automatische Optimierung auf dem Server und das Erben von Einstellungen durch die Datenbanken auf dem übergeordneten Server wird für die Konfiguration der automatischen Optimierung empfohlen, da es die Verwaltung der Optionen für die automatische Optimierung bei einer großen Anzahl von Datenbanken vereinfacht.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Aktivieren der automatischen Optimierung in Azure SQL-Datenbank und zum Verwalten Ihrer Workload finden Sie unter [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).
- Informationen zum manuellen Überprüfen und Anwenden von Empfehlungen der automatischen Optimierung finden Sie unter [Suchen und Anwenden von Empfehlungen zur Leistung](sql-database-advisor-portal.md).
- Informationen zum Erstellen von E-Mail-Benachrichtigungen für automatische Optimierungsempfehlungen finden Sie unter [E-Mail-Benachrichtigungen zur automatischen Optimierung](sql-database-automatic-tuning-email-notifications.md).
- Informationen zur integrierten Intelligenz, die bei der automatischen Optimierung zum Einsatz kommt, finden Sie unter [Artificial Intelligence tunes Azure SQL Databases](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/) (Optimierung von Azure SQL-Datenbanken durch künstliche Intelligenz).
- Informationen zur Funktionsweise der automatischen Optimierung in Azure SQL-Datenbank und SQL Server 2017 finden Sie unter [Automatische Optimierung](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) für SQL Server.

---
title: "Azure SQL-Datenbank – Automatische Optimierung | Microsoft-Dokumentation"
description: "Azure SQL-Datenbank analysiert SQL-Abfragen und führt eine automatische Anpassung an die Benutzerworkload durch."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/19/2017
ms.author: jovanpop
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 64abcf680199938c54be5005ee96670a8e580270
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatische Optimierung in Azure SQL-Datenbank

Azure SQL-Datenbank ist ein vollständig verwalteter Datendienst zum Überwachen der Abfragen, die in der Datenbank ausgeführt werden und verbessert die Leistung bei der Bewältigung der Datenbankworkload automatisch. Azure SQL-Datenbank verfügt über eine integrierte [automatische Optimierung](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning), mit der die Leistung Ihrer Abfragen automatisch optimiert und verbessert werden kann, indem die Datenbank dynamisch an Ihre Workload angepasst wird. Die automatische Optimierung in Azure SQL-Datenbank ist unter Umständen eines der wichtigsten Features, das Sie in Azure SQL-Datenbank aktivieren können, um die Leistung Ihrer Abfragen zu optimieren.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="automatic-tuning-options"></a>Optionen für die automatische Optimierung

Folgende Optionen für die [automatische Optimierung](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) sind in Azure SQL-Datenbank verfügbar:
 1. **CREATE INDEX**: Identifiziert die Indizes, die die Leistung Ihrer Workload verbessern können, erstellt die Indizes und überprüft, dass diese die Leistung von Abfragen verbessern.
 2. **DROP INDEX**: Identifiziert redundante und doppelte Indizes sowie Indizes, die über einen langen Zeitraum hinweg nicht verwendet wurden.
 3. **PLAN REGRESSION CORRECTION**: Identifiziert die SQL-Abfragen, die einen Ausführungsplan verwenden, der langsamer als der vorherige gute Plan ist und verwendet den letzten bekannten guten Plan statt des zurückgestellten Plans.

Azure SQL-Datenbank identifiziert die Empfehlungen von **CREATE INDEX**, **DROP INDEX** und **PLAN REGRESSION CORRECTION**, die Ihre Datenbank optimieren können und zeigt diese im Azure-Portal an. Weitere Informationen zur Identifikation von Indizes, die geändert werden sollten, finden Sie unter [Azure SQL Database Advisor im Azure-Portal](sql-database-advisor-portal.md). Sie können Empfehlungen entweder manuell anwenden, indem Sie das Portal verwenden, oder Sie können einstellen, dass Azure SQL-Datenbank Empfehlungen automatisch anwendet, die Workload nach den Änderungen überwacht und überprüft, dass die Empfehlung die Leistung Ihrer Workload verbessert.

Die Optionen für die automatische Optimierung können unabhängig pro Datenbank aktiviert oder deaktiviert werden. Sie können auch auf einem logischen Server konfiguriert und auf jede Datenbank angewendet werden, die Einstellungen von diesem Server erbt. Das Konfigurieren der Optionen für die [automatische Optimierung](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) auf dem Server und das Erben von Einstellungen durch die Datenbanken auf dem Server wird für die Konfiguration der automatischen Optimierung empfohlen, da es die Verwaltung der Optionen für die automatische Optimierung bei einer großen Anzahl von Datenbanken vereinfacht.

Die erforderlichen Schritte für das Azure-Portal finden Sie im Artikel [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Aktivierung der automatischen Optimierung in Azure SQL-Datenbank und zur Verwendung für die vollständige Verwaltung Ihrer Workload finden Sie unter [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).
- Bei der Verwendung der manuellen Optimierung helfen Ihnen die Informationen unter [Azure SQL Database Advisor im Azure-Portal](sql-database-advisor-portal.md) weiter. Außerdem können Sie manuell die Empfehlungen anwenden, die zu einer Verbesserung der Leistung Ihrer Abfragen führen.
- Hier finden Sie weitere Informationen zu der integrierten Logik, die [Azure SQL-Datenbank](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/) optimiert.
- Hier finden Sie weitere Informationen zur [automatischen Optimierung](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) in Azure SQL-Datenbank und SQL Server 2017.


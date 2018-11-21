---
title: Optimieren von Masseneinfügungen für Azure Database for PostgreSQL-Server
description: In diesem Artikel wird beschrieben, wie Sie Vorgänge für Masseneinfügungen auf einem Azure Database for PostgreSQL-Server optimieren können.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 2fe3c3cc71823880d71223334b89816199561ca9
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628693"
---
# <a name="optimizing-bulk-inserts-and-use-of-transient-data-on-azure-database-for-postgresql-server"></a>Optimieren von Masseneinfügungen und der Verwendung von kurzlebigen Daten für Azure Database for PostgreSQL-Server 
In diesem Artikel wird beschrieben, wie Sie Vorgänge für Masseneinfügungen und die Verwendung von kurzlebigen Daten auf einem Azure Database for PostgreSQL-Server optimieren können.

## <a name="using-unlogged-tables"></a>Verwenden von nicht protokollierten Tabellen
Erwägen Sie die Verwendung von nicht protokollierten Tabellen für Kunden mit Workloadvorgängen, die kurzlebige Daten oder Masseneinfügungen von großen Datasets nutzen.

Nicht protokollierte Tabellen sind ein PostgreSQL-Feature, das auf effektive Weise zum Optimieren von Masseneinfügungen eingesetzt werden kann. Für PostgreSQL werden Write-Ahead-Protokolle (Write-Ahead Logging, WAL) genutzt, um standardmäßig für Atomarität und Dauerhaftigkeit – zwei der ACID-Eigenschaften – zu sorgen. Das Einfügen in eine nicht protokollierte Tabelle bedeutet, dass PostgreSQL Einfügungen ohne Schreibvorgänge in das Transaktionsprotokoll durchführt, wobei es sich bereits um einen E/A-Vorgang handelt. Aus diesem Grund kann für diese Tabellen eine deutlich höhere Geschwindigkeit als für gewöhnliche Tabellen erzielt werden.

Hier sind die Optionen für die Erstellung einer nicht protokollierten Tabelle angegeben:
- Erstellen einer neuen nicht protokollierten Tabelle mit dieser Syntax: `CREATE UNLOGGED TABLE <tableName>`.
- Konvertieren einer vorhandenen protokollierten Tabelle in eine nicht protokollierte Tabelle mit dieser Syntax: `ALTER <tableName> SET UNLOGGED`.  Dies kann mit der folgenden Syntax rückgängig gemacht werden: `ALTER <tableName> SET LOGGED`.

## <a name="unlogged-table-tradeoff"></a>Nachteile von nicht protokollierten Tabellen
Nicht protokollierte Tabellen sind nicht vor Abstürzen geschützt. Eine nicht protokollierte Tabelle wird nach einem Absturz automatisch gekürzt, oder es erfolgt das nicht korrekte Herunterfahren. Der Inhalt einer nicht protokollierten Tabelle wird zudem nicht auf Standbyservern repliziert. Alle Indizes, die für eine nicht protokollierte Tabelle erstellt werden, werden ebenfalls automatisch nicht protokolliert.  Nachdem der Einfügevorgang abgeschlossen wurde, können Sie die Tabelle in den protokollierten Zustand konvertieren, damit der Einfügevorgang dauerhafter Art ist.

Für einige Kundenworkloads konnte aber eine Leistungsverbesserung von 15 bis 20 Prozent erzielt werden, indem nicht protokollierte Tabellen verwendet wurden.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie Ihre Workload auf die Nutzung von kurzlebigen Daten und umfangreichen Masseneinfügungen.  

Sehen Sie sich die folgende PostgreSQL-Dokumentation an: [Create Table SQL Commands](https://www.postgresql.org/docs/current/static/sql-createtable.html) (Erstellen von SQL-Befehlen für Tabellen)
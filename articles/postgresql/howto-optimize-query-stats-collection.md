---
title: Optimieren der Sammlung mit Abfragestatistiken für Azure Database for PostgreSQL-Server
description: In diesem Artikel wird beschrieben, wie Sie die Sammlung mit Abfragestatistiken auf einem Azure Database for PostgreSQL-Server optimieren können.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 076442d85d7f628504cca95c36f3e99f4d0c5117
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966686"
---
# <a name="optimize-query-statistics-collection-in-azure-database-for-postgresql-server"></a>Optimieren der Sammlung mit Abfragestatistiken für Azure Database for PostgreSQL-Server 
In diesem Artikel wird beschrieben, wie Sie die Sammlung mit Abfragestatistiken auf einem Azure Database for PostgreSQL-Server optimieren können.

## <a name="using-pgstatsstatements"></a>Verwenden von „pg_stats_statements“
**pg_stat_statements** ist eine PostgreSQL-Erweiterung, die für Azure Database for PostgreSQL standardmäßig aktiviert ist. Die Erweiterung ermöglicht das Nachverfolgen von Ausführungsstatistiken zu allen SQL-Anweisungen, die auf einem Server ausgeführt werden. Dieses Modul greift aber in jede Abfrageausführung ein und ist mit nicht unerheblichen Leistungseinbußen verbunden. Wenn Sie **pg_stat_statements** aktivieren, wird Abfragetext in Dateien auf dem Datenträger geschrieben.

Für Kunden, die eindeutige Abfragen mit langem Abfragetext nutzen oder **pg_stat_statements** nicht aktiv überwachen, empfehlen wir, **pg_stat_statements** aus Leistungsgründen zu deaktivieren. Verwenden Sie hierfür `pg_stat_statements.track = NONE`.

Bei einigen Kundenworkloads hat sich eine Leistungsverbesserung in Höhe von 50 Prozent ergeben, nachdem **pg_stat_statements** deaktiviert wurde. Der Nachteil der Deaktivierung von „pg_stat_statements“ besteht darin, das keine Problembehandlung für Leistungsprobleme durchgeführt werden kann.

Gehen Sie wie folgt vor, um `pg_stat_statements.track = NONE` festzulegen:

- Navigieren Sie im Azure-Portal zur [Seite für die PostgreSQL-Ressourcenverwaltung, und wählen Sie das Blatt mit den Serverparametern aus](howto-configure-server-parameters-using-portal.md).

![Blatt mit PostgreSQL-Serverparametern](./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png)

- Gehen Sie mit der [Azure CLI](howto-configure-server-parameters-using-cli.md) wie folgt vor: az postgres server configuration set `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`.

## <a name="using-query-store"></a>Verwenden des Abfragespeichers 
Das Feature [Abfragespeicher](concepts-query-store.md) in Azure Database for PostgreSQL ermöglicht ein Verfahren zum Nachverfolgen von Abfragestatistiken mit besserer Leistung und wird als Alternative zu *pg_stats_statements* empfohlen. 

## <a name="next-steps"></a>Nächste Schritte
Überlegen Sie sich, ob Sie `pg_stat_statements.track = NONE` im [Azure-Portal](howto-configure-server-parameters-using-portal.md) festlegen oder die [Azure CLI](howto-configure-server-parameters-using-cli.md) verwenden sollen.

Weitere Informationen finden Sie unter [Verwendungsszenarios für den Abfragespeicher](concepts-query-store-scenarios.md) und [Bewährte Methoden für den Abfragespeicher](concepts-query-store-best-practices.md). 

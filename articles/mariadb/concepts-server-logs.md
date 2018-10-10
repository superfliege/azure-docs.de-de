---
title: Serverprotokolle für Azure Database for MariaDB
description: Beschreibt die Protokolle, die in Azure Database for MariaDB verfügbar sind, sowie die verfügbaren Parameter zum Aktivieren verschiedener Protokolliergrade.
author: rachel-msft
ms.author: raagyema
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: d982af64517ed452d907f62a39e975d472951392
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992982"
---
# <a name="server-logs-in-azure-database-for-mariadb"></a>Serverprotokolle in Azure Database for MariaDB
In Azure Database for MariaDB ist das Protokoll für langsame Abfragen für Benutzer verfügbar. Der Zugriff auf das Transaktionsprotokoll wird jedoch nicht unterstützt. Das Protokoll für langsame Abfragen kann verwendet werden, um Leistungsengpässe für die Problembehandlung zu erkennen.

Weitere Informationen zum Protokoll für langsame Abfragen finden Sie in der MariaDB-Dokumentation zum [Protokoll für langsame Abfragen](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-server-logs"></a>Zugreifen auf Serverprotokolle
Sie können die Serverprotokolle von Azure Database for MariaDB mit dem Azure-Portal und der Azure CLI auflisten und herunterladen.

Wählen Sie im Azure-Portal Ihren Azure Database for MariaDB-Server aus. Wählen Sie unter der Überschrift **Überwachung** die Seite **Serverprotokolle** aus.

<!-- For more information on Azure CLI, see [Configure and access server logs using Azure CLI](howto-configure-server-logs-in-cli.md).-->

## <a name="log-retention"></a>Protokollaufbewahrung
Protokolle sind für bis zu sieben Tage nach ihrer Erstellung verfügbar. Wenn die Gesamtgröße der verfügbaren Protokolle 7 GB überschreitet, werden die ältesten Dateien gelöscht, bis Speicherplatz verfügbar ist.

Die Protokolle werden alle 24 Stunden oder bei Erreichen einer Größe von 7 GB rotiert, je nachdem, welches Ereignis früher eintritt.

## <a name="configure-logging"></a>Konfigurieren der Protokollierung
Das Protokoll für langsame Abfragen ist standardmäßig deaktiviert. Legen Sie „slow_query_log“ auf „ON“ fest, um dieses Feature zu aktivieren.

Weitere Parameter, die Sie anpassen können:

- **long_query_time**: Die Abfrage wird protokolliert, wenn sie länger als „long_query_time“ (in Sekunden) dauert. Der Standardwert ist 10 Sekunden.
- **log_slow_admin_statements**: Wenn „ON“, sind administrative Anweisungen wie ALTER_TABLE und ANALYZE_TABLE in den Anweisungen enthalten, die in „slow_query_log“ geschrieben werden.
- **log_queries_not_using_indexes**: Bestimmt, ob Abfragen, die keine Indizes verwenden, in „slow_query_log“ protokolliert werden.
- **log_throttle_queries_not_using_indexes**: Dieser Parameter schränkt die Anzahl der Nichtindexabfragen ein, die in das Protokoll für langsame Abfragen geschrieben werden können. Dieser Parameter wird wirksam, wenn „log_queries_not_using_indexes“ auf „ON“ festgelegt ist.

Vollständige Beschreibungen der Parameter des Protokolls für langsame Abfragen finden Sie in der [MariaDB-Dokumentation zum Protokoll für langsame Abfragen](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="next-steps"></a>Nächste Schritte
- [Gewusst wie: Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe des Azure-Portals](howto-configure-server-logs-portal.md).

---
title: Serverprotokolle für Azure Database for MySQL
description: Beschreibt die Protokolle, die in Azure Database for MySQL verfügbar sind, sowie die verfügbaren Parameter zum Aktivieren verschiedener Protokolliergrade.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 09/17/2018
ms.openlocfilehash: ac5be20815b552c08e5cd1054bf24d7a10b56498
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124268"
---
# <a name="server-logs-in-azure-database-for-mysql"></a>Serverprotokolle in Azure Database for MySQL
In Azure Database for MySQL ist das Protokoll für langsame Abfragen für Benutzer verfügbar. Der Zugriff auf das Transaktionsprotokoll wird jedoch nicht unterstützt. Das Protokoll für langsame Abfragen kann verwendet werden, um Leistungsengpässe für die Problembehandlung zu erkennen. 

Weitere Informationen zum MySQL-Protokoll für langsame Abfragen finden Sie im [Abschnitt zu langsamen Abfragen](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) im MySQL-Referenzhandbuch.

## <a name="access-server-logs"></a>Zugreifen auf Serverprotokolle
Sie können die Serverprotokolle von Azure Database for MySQL mit dem Azure Portal und der Azure CLI auflisten und herunterladen.

Wählen Sie im Azure-Portal Ihren Azure Database for MySQL-Server aus. Wählen Sie unter der Überschrift **Überwachung** die Seite **Serverprotokolle** aus.

Weitere Informationen zur Azure CLI finden Sie unter [Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI](howto-configure-server-logs-in-cli.md).

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

Vollständige Beschreibungen der Parameter des Protokolls für langsame Abfragen finden Sie in der [MySQL-Dokumentation zum Protokoll für langsame Abfragen](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI](howto-configure-server-logs-in-cli.md).

---
title: Überwachung in Azure Database for MariaDB
description: In diesem Artikel werden die Überwachungs- und Warnmetriken (CPU, Speicher, Verbindungsstatistiken und Ähnliches) für Azure Database for MariaDB beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/10/2018
ms.openlocfilehash: f34e32b9bda83ac1185d0c7b7dcaaaa7b47161a1
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58082482"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>Überwachung in Azure Database for MariaDB
Die Überwachung der Daten zu Ihren Servern unterstützt Sie bei der Problembehandlung und der Optimierung Ihrer Workloads. Azure Database for MariaDB bietet verschiedene Metriken, die Einblicke in das Verhalten Ihres Servers ermöglichen.

## <a name="metrics"></a>Metriken
Alle Azure-Metriken werden im Minutentakt erfasst, und für jede Metrik steht ein Verlauf von 30 Tagen zur Verfügung. Sie können Warnungen für die Metriken konfigurieren. Darüber hinaus können weitere Aufgaben wie das Einrichten automatisierter Aktionen, das Durchführen erweiterter Analysen und das Archivieren des Verlaufs ausgeführt werden. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL).

### <a name="list-of-metrics"></a>Liste der Metriken
Für Azure Database for MariaDB sind folgende Metriken verfügbar:

|Metrik|Metrikanzeigename|Unit|BESCHREIBUNG|
|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Die CPU-Auslastung in Prozent|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Die Arbeitsspeicherauslastung in Prozent|
|io_consumption_percent|E/A in Prozent|Prozent|Die E/A-Auslastung in Prozent|
|storage_percent|Speicher in Prozent|Prozent|Der verwendete Speicher relativ zum Maximalwert des Servers (in Prozent)|
|storage_used|Verwendeter Speicher|Byte|Die Menge des verwendeten Speichers. Der vom Dienst verwendete Speicher kann die Datenbankdateien, Transaktionsprotokolle und Serverprotokolle umfassen.|
|serverlog_storage_percent|Serverprotokollspeicher in Prozent|Prozent|Der Prozentsatz des Serverprotokollspeichers, der aus dem maximalen Serverprotokollspeicher des Servers verwendet wird.|
|serverlog_storage_usage|Verwendeter Serverprotokollspeicher|Byte|Die Menge des verwendeten Serverprotokollspeichers.|
|serverlog_storage_limit|Begrenzung des Serverprotokollspeichers|Byte|Der maximale Serverprotokollspeicher für diesen Server.|
|storage_limit|Speicherbegrenzung|Byte|Der maximale Speicher für diesen Server|
|active_connections|Die aktiven Verbindungen.|Count|Die Anzahl aktiver Verbindungen mit dem Server|
|connections_failed|Verbindungsfehler|Count|Die Anzahl von Verbindungsfehlern für den Server|
|network_bytes_egress|Netzwerk ausgehend|Byte|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen.|
|network_bytes_ingress|Netzwerk eingehend|Byte|Eingehender Netzwerkdatenverkehr über aktive Verbindungen.|

## <a name="server-logs"></a>Serverprotokolle
Sie können die Protokollierung von langsamen Abfragen auf Ihrem Server aktivieren. Weitere Informationen zur Protokollierung finden Sie auf der Seite [Serverprotokolle](concepts-server-logs.md).

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen dazu, wie Sie mit dem Azure-Portal, der REST-API oder der CLI auf Metriken zugreifen bzw. diese exportieren, finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
  - Anleitungen zum Erstellen einer Warnung zu einer Metrik finden Sie unter [Einrichten von Warnungen](howto-alert-metric.md).

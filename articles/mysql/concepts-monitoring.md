---
title: "Überwachen in Azure Database for MySQL"
description: "In diesem Artikel werden die Überwachungs- und Warnmetriken (CPU, Grenzwerte, Speicher, Verbindungsstatistiken und Ähnliches) für Azure Database for MySQL beschrieben."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 7ecfb8151cd81fb588f964fdfa3a74aacab24874
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="monitoring-in-azure-database-for-mysql"></a>Überwachen in Azure Database for MySQL
Die Überwachung von Daten zu Ihren Servern unterstützt Sie bei der Problembehandlung und der Optimierung für Workloads. Azure Database for MySQL bietet verschiedene Metriken, die Einblicke in das Verhalten der Ressourcen ermöglichen, die dem MySQL-Server zugrunde liegen. 

## <a name="metrics"></a>Metriken
Alle Azure-Metriken werden im Minutentakt erfasst, und für jede Metrik steht ein Verlauf von 30 Tagen zur Verfügung. 

Sie können Warnungen für die Metriken konfigurieren. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-on-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL). 

Darüber hinaus können weitere Aufgaben wie das Einrichten automatisierter Aktionen, das Durchführen erweiterter Analysen und das Archivieren des Verlaufs ausgeführt werden. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Liste der Metriken
Für Azure Database for MySQL sind folgende Metriken verfügbar:

|Metrik|Metrikanzeigename|Unit|BESCHREIBUNG|
|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Die CPU-Auslastung in Prozent|
|compute_limit|Grenzwert für Computeeinheit|Count|Die maximale Anzahl von Computeeinheiten dieses Servers|
|compute_consumption_percent|Prozentsatz von Computeeinheit|Prozent|Die verwendeten Computeeinheiten relativ zum Maximalwert des Servers (in Prozent)|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Die Arbeitsspeicherauslastung in Prozent|
|io_consumption_percent|E/A in Prozent|Prozent|Die E/A-Auslastung in Prozent|
|storage_percent|Speicher in Prozent|Prozent|Der verwendete Speicher relativ zum Maximalwert des Servers (in Prozent)|
|storage_used|Verwendeter Speicher|Byte|Die Menge des verwendeten Speichers. Der vom Dienst verwendete Speicher umfasst die Datenbankdateien, Transaktionsprotokolle und Serverprotokolle.|
|storage_limit|Speicherbegrenzung|Byte|Der maximale Speicher für diesen Server|
|active_connections|Aktive Verbindungen gesamt|Count|Die Anzahl aktiver Verbindungen mit dem Server|
|connections_failed|Fehlerhafte Verbindungen gesamt|Count|Die Anzahl von Verbindungsfehlern für den Server|


## <a name="next-steps"></a>Nächste Schritte
- Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-on-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL). 
- Weitere Informationen dazu, wie Sie mit dem Azure-Portal, der REST-API oder der CLI auf Metriken zugreifen bzw. diese exportieren, finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

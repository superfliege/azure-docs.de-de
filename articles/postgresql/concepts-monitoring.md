---
title: "Überwachung in Azure Database for PostgreSQL | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Metriken (u.a. CPU, Grenzwerte, Speicher und Verbindungsstatistiken) für Überwachung und Warnungen für Azure Database for PostgreSQL beschrieben."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/24/2017
ms.openlocfilehash: d48159fbc5e52bf1916a744e3912ca7ceb0ab60f
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="monitoring-in-azure-database-for-postgresql"></a>Überwachung in Azure Database for PostgreSQL
Die Überwachung der Daten zu Ihren Servern unterstützt Sie bei der Problembehandlung und der Optimierung Ihrer Workloads. Azure Database for PostgreSQL bietet verschiedene Metriken, die Einblicke in das Verhalten der Ressourcen gewähren, die dem PostgreSQL-Server zugrunde liegen. 

## <a name="metrics"></a>Metriken
Alle Azure-Metriken werden im Minutentakt erfasst, und für jede Metrik steht ein Verlauf von 30 Tagen zur Verfügung. 

Sie können Warnungen zu den Metriken konfigurieren. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-on-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL). 

Darüber hinaus können weitere Aufgaben wie das Einrichten automatisierter Aktionen, das Durchführen erweiterter Analysen und das Archivieren des Verlaufs ausgeführt werden. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Liste der Metriken
Die folgenden Metriken sind für Azure Database for PostgreSQL verfügbar:

|Metrik|Metrikanzeigename|Einheit|Beschreibung|
|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Der Prozentsatz der verwendeten CPU|
|compute_limit|Grenzwert für Computeeinheit|Anzahl|Die maximale Anzahl von Computeeinheiten dieses Servers|
|compute_consumption_percent|Prozentsatz von Computeeinheit|Prozent|Die verwendeten Computeeinheiten relativ zum Maximalwert des Servers (in Prozent)|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Der Prozentsatz des verwendeten Arbeitsspeichers|
|io_consumption_percent|E/A in Prozent|Prozent|Der Prozentsatz der verwendeten Ein-/Ausgaben|
|storage_percent|Speicher in Prozent|Prozent|Der verwendete Speicher relativ zum Maximalwert des Servers (in Prozent)|
|storage_used|Verwendeter Speicher|Byte|Die Menge des verwendeten Speichers. Der vom Dienst verwendete Speicher umfasst die Datenbankdateien, Transaktionsprotokolle und die Serverprotokolle.|
|storage_limit|Speicherbegrenzung|Byte|Der maximale Speicher für diesen Server|
|active_connections|Aktive Verbindungen gesamt|Anzahl|Die Anzahl der aktiven Verbindungen mit dem Server|
|connections_failed|Fehlerhafte Verbindungen gesamt|Anzahl|Die Anzahl der Verbindungsfehler für den Server|


> [!NOTE]
> Die Computeeinheit setzt sich aus Arbeitsspeicher und CPU zusammen. Der Prozentsatz der Computeeinheit lautet „max(memory%, cpu%)“. Überprüfen Sie die Diagramme für Arbeitsspeicher und CPU, um zu ermitteln, was zu Änderungen des Prozentsatzes der Computeeinheiten beiträgt. Weitere Informationen finden Sie unter [Erläuterungen zu Computeeinheiten in Azure Database for PostgreSQL](concepts-compute-unit-and-storage.md).

## <a name="next-steps"></a>Nächste Schritte
- Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-on-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL). 
- Weitere Informationen dazu, wie Sie mit dem Azure-Portal, der REST-API oder der CLI auf Metriken zugreifen bzw. diese exportieren, finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

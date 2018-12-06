---
title: Azure Security-Datenexport nach SIEM – Pipelinekonfiguration | Microsoft-Dokumentation
description: Dieser Artikel beschreibt das Exportieren von Azure Security Center-Protokollen nach SIEM
services: security-center
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/26/2018
ms.author: barclayn
ms.openlocfilehash: 4cc9478197b39198a94ace1cbefab81de3cdb32b
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52306725"
---
# <a name="azure-security-data-export-to-siem-pipeline-configuration"></a>Azure Security-Datenexport nach SIEM – Pipelinekonfiguration

Dieses Dokument erläutert im Detail das Verfahren zum Exportieren von Azure Security Center-Sicherheitsdaten nach SIEM.

Von Azure Security Center erzeugte verarbeitete Ereignisse werden im [Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) von Azure veröffentlicht, einem der über Azure Monitor verfügbaren Protokolltypen. Azure Monitor bietet eine konsolidierte Pipeline zum Routing beliebiger Überwachungsdaten zu einem SIEM-Tool. Dies erfolgt durch Streaming der Daten an einen Event Hub, von wo aus sie dann in ein Partnertool hereingezogen werden können.

Diese Pipe verwendet die [einzelne Azure-Überwachungspipeline](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) für den Zugriff auf die Überwachungsdaten aus Ihrer Azure-Umgebung. So können Sie mühelos SIEMs und Überwachungstools zum Verarbeiten der Daten einrichten.

In den nächsten Abschnitten wird beschrieben, wie Sie Daten zum Streamen an einen Event Hub konfigurieren können. Die Schritte setzen voraus, dass Sie bereits Azure Security Center in Ihrem Azure-Abonnement konfiguriert haben.

Allgemeine Übersicht

![Allgemeine Übersicht](media/security-center-export-data-to-siem/overview.png)

## <a name="what-is-the-azure-security-data-exposed-to-siem"></a>Was sind die für SIEM verfügbar gemachten Azure-Sicherheitsdaten?

In dieser Version stellen wir die [Sicherheitswarnungen](../security-center/security-center-managing-and-responding-alerts.md) zur Verfügung. In zukünftigen Versionen wird das Dataset mit Empfehlungen zur Sicherheit erweitert.

## <a name="how-to-setup-the-pipeline"></a>Einrichten der Pipeline

### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Bevor Sie beginnen, müssen Sie [einen Event Hubs-Namespace erstellen](../event-hubs/event-hubs-create.md). Dieser Namespace und Event Hub sind das Ziel für sämtliche Ihrer Überwachungsdaten.

### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Streamen des Azure-Aktivitätsprotokolls an Event Hubs

Bitte lesen Sie den folgenden Artikel: [Streamen des Azure-Aktivitätsprotokolls an Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md).

### <a name="install-a-partner-siem-connector"></a>Installieren eines Partner-SIEM-Connectors 

Die Weiterleitung Ihrer Überwachungsdaten an einen Event Hub mit Azure Monitor ermöglicht eine einfache Integration in SIEM- und Überwachungstools von Partnern.

Unter dem folgenden Link finden Sie die Liste [unterstützter SIEMs](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub).

## <a name="example-for-querying-data"></a>Beispiel zum Abfragen von Daten 

Hier sind einige Splunk-Abfragen, mit denen Sie Warnungsdaten erhalten können:

| **Beschreibung der Abfrage**                                | **Abfrage**                                                                                                                              |
|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Alle Warnungen                                              | index=main Microsoft.Security/locations/alerts                                                                                         |
| Zusammenfassen der Anzahl der Vorgänge nach deren Namen             | index=main sourcetype="amal:security" \| table operationName \| stats count by operationName                                |
| Abrufen von Informationen von Warnungen: Zeit, Name, Status, ID und Abonnement | index=main Microsoft.Security/locations/alerts \| table \_time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Nächste Schritte

- [Unterstützte SIEMs](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)
- [Streamen von Aktivitätsprotokollen an Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)
- [Sicherheitswarnungen.](../security-center/security-center-managing-and-responding-alerts.md)
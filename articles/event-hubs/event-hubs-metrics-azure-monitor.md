---
title: Azure Event Hubs-Metriken in Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Überwachen von Event Hubs mithilfe von Azure Monitor
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2018
ms.author: shvija
ms.openlocfilehash: 134374a128264eb6b48076ef240436951f8d6a39
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40004985"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor-preview"></a>Azure Event Hubs-Metriken in Azure Monitor (Vorschauversion)

Event Hubs-Metriken geben Aufschluss über den Zustand von Event Hubs-Ressourcen in Ihrem Azure-Abonnement. Mithilfe eines umfangreichen Satzes von Metrikdaten können Sie die allgemeine Integrität Ihrer Event Hubs nicht nur auf Namespaceebene, sondern auch auf Entitätsebene bewerten. Diese Statistiken können wichtig sein, da sie Sie beim Überwachen des Zustands Ihrer Event Hubs unterstützen. Metriken können auch hilfreich sein, um Problemursachen ohne die Unterstützung des Azure-Supports zu beheben.

Azure Monitor bietet einheitliche Benutzeroberflächen für die übergreifende Überwachung verschiedener Azure-Dienste. Weitere Informationen finden Sie unter [Überwachung in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) und im Beispiel zum [Abrufen von Azure Monitor-Metriken mit .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) auf GitHub.

## <a name="access-metrics"></a>Zugreifen auf Metriken

Azure Monitor bietet mehrere Möglichkeiten für den Metrikzugriff. Sie können über das [Azure-Portal](https://portal.azure.com), über die Azure Monitor-APIs (REST und .NET) sowie über Analyselösungen wie die Operation Management Suite und Event Hubs auf Metriken zugreifen. Weitere Informationen finden Sie unter [Azure Monitor-Metriken](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

Metriken sind standardmäßig aktiviert, und es stehen Daten für die letzten 30 Tage zur Verfügung. Zur längeren Aufbewahrung können Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Dies wird in den [Diagnoseeinstellungen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) in Azure Monitor konfiguriert.

## <a name="access-metrics-in-the-portal"></a>Zugreifen auf Metriken über das Portal

Im [Azure-Portal](https://portal.azure.com) können Sie Metriken im Zeitverlauf überwachen. Das folgende Beispiel zeigt, wie Sie die erfolgreichen und die eingehenden Anforderungen auf Kontoebene anzeigen:

![][1]

Sie können auch direkt über den Namespace auf die Metriken zugreifen. Wählen Sie dazu den Namespace aus, und klicken Sie anschließend auf **Metriken (Vorschau)**. Wenn Sie Metriken anzeigen möchten, die für den Bereich des Event Hubs gefiltert wurden, wählen Sie den Event Hub aus, und klicken Sie anschließend auf **Metriken (Vorschau)**.

Bei Metriken mit Dimensionsunterstützung müssen Sie den gewünschten Dimensionswert als Filter verwenden, wie im folgenden Beispiel zu sehen:

![][2]

## <a name="billing"></a>Abrechnung

Die Verwendung von Metriken in Azure Monitor ist in der Vorschauversion kostenlos. Wenn Sie allerdings zusätzliche Lösungen zur Metrikdatenerfassung verwenden, fallen unter Umständen Kosten für diese Lösungen an. So werden etwa Gebühren für Azure Storage berechnet, wenn Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Kosten fallen auch an, wenn Sie Metrikdaten für eine ausführlichere Analyse an Log Analytics streamen.

Die folgenden Metriken bieten Ihnen eine Übersicht über die Integrität eines Diensts. 

> [!NOTE]
> Einige Metriken werden eingestellt, da sie unter einem anderen Namen verwendet werden. Deshalb müssen Sie möglicherweise Ihre Verweise aktualisieren. Metriken, die mit dem Schlüsselwort „veraltet“ markiert sind, werden zukünftig nicht mehr unterstützt.

Alle Metrikwerte werden minütlich an Azure Monitor gesendet. Die Zeitgranularität definiert das Zeitintervall, für das Metrikwerte dargestellt werden. Das unterstützte Zeitintervall für alle Event Hubs-Metriken beträgt eine Minute.

## <a name="request-metrics"></a>Anforderungsmetriken

Zählt die Anzahl der Anforderungen von Daten und Verwaltungsvorgängen

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
| Eingehende Anforderungen (Vorschau) | Die Anzahl von Anforderungen an den Azure Event Hubs-Dienst in einem bestimmten Zeitraum <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName |
| Erfolgreiche Anforderungen (Vorschau)   | Die Anzahl erfolgreicher Anforderungen an den Azure Event Hubs-Dienst in einem bestimmten Zeitraum <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName |
| Serverfehler (Vorschau) | Die Anzahl der aufgrund eines Fehlers nicht verarbeiteten Anforderungen an den Azure Event Hubs-Dienst in einem bestimmten Zeitraum <br/><br/>Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName |
|Benutzerfehler (Vorschau)|Die Anzahl der aufgrund von Benutzerfehlern nicht verarbeiteten Anforderungen in einem bestimmten Zeitraum<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Gedrosselte Anforderungen (Vorschau)|Die Anzahl von Anforderungen, die gedrosselt wurden, da der Grenzwert für die Nutzung der Durchsatzeinheit überschritten wurde.<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Fehler zur Kontingentüberschreitung (Vorschau)|Die Anzahl von Anforderungen hat das verfügbare Kontingent überschritten. Weitere Informationen zu Event Hubs-Kontingenten finden Sie in [diesem Artikel](event-hubs-quotas.md).<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

## <a name="throughput-metrics"></a>Durchsatzmetriken

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
|Gedrosselte Anforderungen (Vorschau)|Die Anzahl von Anforderungen, die gedrosselt wurden, da der Grenzwert für die Nutzung der Durchsatzeinheit überschritten wurde.<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

## <a name="message-metrics"></a>Nachrichtenmetriken

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
|Eingehende Nachrichten (Vorschau)|Die Anzahl von Ereignissen oder Nachrichten, die in einem bestimmten Zeitraum an Event Hubs gesendet wurden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Ausgehende Nachrichten (Vorschau)|Die Anzahl von Ereignissen oder Nachrichten, die in einem bestimmten Zeitraum aus Event Hubs abgerufen wurden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Eingehende Bytes (Vorschau)|Die Anzahl von Bytes, die in einem bestimmten Zeitraum an den Azure Event Hubs-Dienst gesendet wurden<br/><br/> Einheit: Bytes <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Ausgehende Bytes (Vorschau)|Die Anzahl von Bytes, die in einem bestimmten Zeitraum vom Azure Event Hubs-Dienst empfangen wurden<br/><br/> Einheit: Bytes <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

## <a name="connection-metrics"></a>Verbindungsmetriken

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
|ActiveConnections (Vorschau)|Die Anzahl der aktiven Verbindungen in einem Namespace und bei einer Entität<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Geöffnete Verbindungen (Vorschau)|Die Anzahl der geöffneten Verbindungen<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Geschlossene Verbindungen (Vorschau)|Die Anzahl der geschlossenen Verbindungen<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

## <a name="event-hubs-capture-metrics"></a>Event Hubs-Erfassungsmetriken

Sie können Event Hubs-Erfassungsmetriken überwachen, wenn Sie das Erfassungsfeature für Ihre Event Hubs aktivieren. Die folgenden Metriken beschreiben, was Sie bei aktiviertem Erfassungsfeature überwachen können.

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
|Backlog erfassen (Vorschau)|Die Anzahl von Bytes, die noch für das gewählte Ziel erfasst werden müssen<br/><br/> Einheit: Bytes <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Erfasste Nachrichten (Vorschau)|Die Anzahl von Nachrichten oder Ereignissen, die in einem bestimmten Zeitraum für das gewählte Ziel erfasst werden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Erfasste Bytes (Vorschau)|Die Anzahl von Bytes, die in einem bestimmten Zeitraum für das gewählte Ziel erfasst werden<br/><br/> Einheit: Bytes <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

## <a name="metrics-dimensions"></a>Metrikdimensionen

Azure Event Hubs unterstützt folgende Dimensionen für Metriken in Azure Monitor. Das Hinzufügen von Dimensionen zu Ihren Metriken ist optional. Wenn Sie keine Dimensionen hinzufügen, werden Metriken auf Namespaceebene angegeben. 

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
|EntityName| Event Hubs unterstützt die Event Hub-Entitäten unter dem Namespace.|

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Übersicht über die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-overview.md).
* Rufen Sie das [Azure Monitor-Metrikbeispiel mit .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) auf GitHub ab. 

Weitere Informationen zu Event Hubs erhalten Sie unter den folgenden Links:

* Erste Schritte mit einem [Event Hubs-Tutorial](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
* [Beispielanwendungen mit Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png




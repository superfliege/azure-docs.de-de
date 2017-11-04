---
title: Azure Service Bus-Metriken in Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: "Überwachen von Service Bus-Entitäten mit Azure Monitor"
services: service-bus-messaging
documentationcenter: .NET
author: christianwolf42
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: sethm
ms.openlocfilehash: fcc7e1cbacc7889c9525207b238162e6caa6b00b
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2017
---
# <a name="azure-service-bus-metrics-in-azure-monitor-preview"></a>Azure Service Bus-Metriken in Azure Monitor (Vorschauversion)

Service Bus-Metriken informieren Sie über den Status der Ressourcen in Ihrem Azure-Abonnement. Mithilfe eines umfangreichen Satzes von Metrikdaten können Sie die allgemeine Integrität Ihrer Service Bus-Ressourcen nicht nur auf Namespaceebene, sondern auch auf Entitätsebene bewerten. Diese Statistiken können wichtig sein, da sie Sie beim Überwachen des Zustands von Service Bus unterstützen. Metriken können auch beim Beheben der Ursachen von Problemen helfen, ohne dass Sie sich an den Azure-Support wenden müssen.

Azure Monitor bietet einheitliche Benutzeroberflächen für die übergreifende Überwachung verschiedener Azure-Dienste. Weitere Informationen finden Sie unter [Überwachung in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) und im Beispiel zum [Abrufen von Azure Monitor-Metriken mit .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) auf GitHub.

## <a name="access-metrics"></a>Zugreifen auf Metriken

Azure Monitor bietet mehrere Möglichkeiten für den Metrikzugriff. Sie können über das [Azure-Portal](https://portal.azure.com), über die Azure Monitor-APIs (REST und .NET) sowie über Analyselösungen wie die Operation Management Suite und Event Hubs auf Metriken zugreifen. Weitere Informationen finden Sie unter [Azure Monitor-Metriken](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

Metriken sind standardmäßig aktiviert, und es stehen Daten für die letzten 30 Tage zur Verfügung. Zur längeren Aufbewahrung können Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Dies wird in den [Diagnoseeinstellungen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) in Azure Monitor konfiguriert.

## <a name="access-metrics-in-the-portal"></a>Zugreifen auf Metriken über das Portal

Im [Azure-Portal](https://portal.azure.com) können Sie Metriken im Zeitverlauf überwachen. Das folgende Beispiel zeigt, wie Sie die erfolgreichen und die eingehenden Anforderungen auf Kontoebene anzeigen:

![][1]

Sie können auch direkt über den Namespace auf die Metriken zugreifen. Dazu wählen Sie den Namespace aus und klicken dann auf **Metriken (Vorschau)**. Um Metriken, die auf Entitätsebene gefiltert werden, anzuzeigen, wählen Sie die Entität aus und klicken dann auf **Metriken (Vorschau)**.

![][2]

Bei Metriken mit Dimensionsunterstützung müssen Sie den gewünschten Dimensionswert als Filter verwenden.

## <a name="billing"></a>Abrechnung

Die Verwendung von Metriken in Azure Monitor ist in der Vorschauversion kostenlos. Wenn Sie allerdings zusätzliche Lösungen zur Metrikdatenerfassung verwenden, fallen unter Umständen Kosten für diese Lösungen an. So werden etwa Gebühren für Azure Storage berechnet, wenn Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Kosten fallen auch an, wenn Sie Metrikdaten für eine ausführlichere Analyse an die Operation Management Suite (OMS) streamen.

Die folgenden Metriken bieten Ihnen eine Übersicht über die Integrität eines Diensts. 

> [!NOTE]
> Einige Metriken werden eingestellt, da sie unter einem anderen Namen verwendet werden. Deshalb müssen Sie möglicherweise Ihre Verweise aktualisieren. Metriken, die mit dem Schlüsselwort „veraltet“ markiert sind, werden zukünftig nicht mehr unterstützt.

Alle Metrikwerte werden minütlich an Azure Monitor gesendet. Das Aggregationsintervall definiert das Zeitintervall, für das Metrikwerte dargestellt werden. Das unterstützte Zeitintervall für alle Service Bus-Metriken beträgt 1 Minute.

## <a name="request-metrics"></a>Anforderungsmetriken

Zählt die Anzahl der Anforderungen von Daten und Verwaltungsvorgängen

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
| Eingehende Anforderungen (Vorschau) | Die Anzahl der Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Erfolgreiche Anforderungen (Vorschau)|Die Anzahl der erfolgreichen Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Serverfehler (Vorschau)|Die Anzahl der aufgrund eines Fehlers nicht verarbeiteten Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Benutzerfehler (Vorschau)|Die Anzahl der aufgrund von Benutzerfehlern nicht verarbeiteten Anforderungen in einem bestimmten Zeitraum<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Gedrosselte Anforderungen (Vorschau)|Die Anzahl der Anforderungen, die aufgrund eines überschrittenen Verbrauchs gedrosselt wurden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

## <a name="message-metrics"></a>Nachrichtenmetriken

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
|Eingehende Nachrichten (Vorschau)|Die Anzahl von Ereignissen oder Nachrichten, die in einem bestimmten Zeitraum an Service Bus gesendet wurden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Ausgehende Nachrichten (Vorschau)|Die Anzahl von Ereignissen oder Nachrichten, die in einem bestimmten Zeitraum von Service Bus empfangen wurden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

## <a name="connection-metrics"></a>Verbindungsmetriken

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
|ActiveConnections (Vorschau)|Die Anzahl der aktiven Verbindungen in einem Namespace und bei einer Entität<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Geöffnete Verbindungen (Vorschau)|Die Anzahl der geöffneten Verbindungen<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Geschlossene Verbindungen (Vorschau)|Die Anzahl der geschlossenen Verbindungen<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName |

## <a name="resource-usage-metrics"></a>Metriken zur Ressourcennutzung

| Metrikname | Beschreibung |
| ------------------- | ----------------- |
|CPU-Auslastung pro Namespace (Vorschau)|Der Prozentsatz der CPU-Auslastung des Namespace<br/><br/> Einheit: Prozent <br/> Aggregationstyp: Maximum <br/> Dimension: EntityName|
|Arbeitsspeichernutzung pro Namespace (Vorschau)|Der Prozentsatz der Arbeitsspeichernutzung des Namespace<br/><br/> Einheit: Prozent <br/> Aggregationstyp: Maximum <br/> Dimension: EntityName|

## <a name="metrics-dimensions"></a>Metrikdimensionen

Azure Service Bus unterstützt folgende Dimensionen für Metriken in Azure Monitor. Das Hinzufügen von Dimensionen zu Ihren Metriken ist optional. Wenn Sie keine Dimensionen hinzufügen, werden Metriken auf Namespaceebene angegeben. 

|Dimensionsname|Beschreibung|
| ------------------- | ----------------- |
|EntityName| Service Bus unterstützt Messagingentitäten unter dem Namespace.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Übersicht über die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png



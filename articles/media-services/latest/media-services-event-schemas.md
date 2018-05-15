---
title: Azure Event Grid-Schemas für Media Services-Ereignisse
description: Beschreibt die Eigenschaften, die mit Azure Event Grid für Media Services-Ereignisse bereitgestellt werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: c443ea755c9e7e88b4685682d5e43f675d42efa4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Azure Event Grid-Schemas für Media Services-Ereignisse

Dieser Artikel beschreibt die Schemas und Eigenschaften für Media Services-Ereignisse.

## <a name="media-services-job-state-change-event"></a>Statusänderungsereignis des Media Services-Auftrags

Dieser Abschnitt enthält die Eigenschaften und das Schema für das Statusänderungsereignis des Media Services-Auftrags.  

### <a name="available-event-types"></a>Verfügbare Ereignistypen

Ein Media Services-**Auftrag** gibt die folgenden Ereignistypen aus:

| Ereignistypen | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| Wird ausgelöst, wenn sich der Status des Auftrags ändert. |

### <a name="example-event"></a>Beispielereignis

Das folgende Beispiel zeigt das Schema eines Statusereignisses eines abgeschlossenen Auftrags: 

```json
[
  {
    "topic": "/subscriptions/{subscription id}/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
    "subject": "transforms/VideoAnalyzerTransform/jobs/{job id}",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "<id>",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

### <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| Thema | Zeichenfolge | Dieser Wert wird von Event Grid bereitgestellt. |
| subject | Zeichenfolge | Der Ressourcenpfad unter der Media Services-Kontoressource. |
| eventType | Zeichenfolge | Einer der registrierten Ereignistypen für die Ereignisquelle. Beispiel: “Microsoft.Media.JobStateChange“. |
| eventTime | Zeichenfolge | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | Zeichenfolge | Eindeutiger Bezeichner für das Ereignis. |
| data | object | Media Services-Ereignisdaten. |
| dataVersion | Zeichenfolge | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | Zeichenfolge | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| previousState | Zeichenfolge | Der Status des Auftrags vor dem Ereignis. |
| state | Zeichenfolge | Der neue Status des Auftrags, über den in diesem Ereignis eine Benachrichtigung erfolgt. Beispiele: „Queued: The Job is awaiting resources“ (In der Warteschlange: Der Auftrag wartet auf Ressourcen) oder „Scheduled: The job is ready to start“ (Geplant: Der Auftrag ist bereit für den Start).|

Dabei kann der Auftragsstatus einer der folgenden Werte sein: *Queued* (In der Warteschlange), *Scheduled* (Geplant), *Processing* (Verarbeitung), *Finished* (Abgeschlossen), *Error* (Fehler), *Canceled* (Abgebrochen), *Canceling* (Wird abgebrochen).

## <a name="next-steps"></a>Nächste Schritte

[Registrieren für Statusänderungsereignisse von Aufträgen](job-state-events-cli-how-to.md)

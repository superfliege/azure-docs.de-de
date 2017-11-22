---
title: "Azure Event Grid-Ereignisschema für Event Hubs"
description: "Beschreibt die Eigenschaften, die mit Azure Event Grid für Event Hubs-Ereignisse bereitgestellt werden"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/07/2017
ms.author: tomfitz
ms.openlocfilehash: 80959ee589a1cfcf317a98c3bafd7f92c796fc2d
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Azure Event Grid-Ereignisschema für Event Hubs

In diesem Artikel werden die Eigenschaften und das Schema für Event Hubs-Ereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

### <a name="available-event-types"></a>Verfügbare Ereignistypen

Event Hubs gibt den Ereignistyp **Microsoft.EventHub.CaptureFileCreated** aus, wenn eine Erfassungsdatei erstellt wird.

## <a name="example-event"></a>Beispielereignis

In diesem Beispielereignis ist das Schema eines Event Hubs-Ereignisses dargestellt, das ausgelöst wird, wenn eine Datei über das Feature „Erfassen“ gespeichert wird: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```

## <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | Typ | Beschreibung |
| -------- | ---- | ----------- |
| Thema | string | Vollständiger Ressourcenpfad zu der Ereignisquelle. Dieses Feld ist nicht beschreibbar. |
| subject | string | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | string | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | string | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | string | Eindeutiger Bezeichner für das Ereignis. |
| data | objekt | Event Hub-Ereignisdaten. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | Beschreibung |
| -------- | ---- | ----------- |
| fileUrl | string | Der Pfad zur Erfassungsdatei. |
| fileType | string | Der Dateityp der Erfassungsdatei. |
| partitionId | string | Die Shard-ID. |
| sizeInBytes | Ganze Zahl | Die Dateigröße. |
| eventCount | Ganze Zahl | Die Anzahl der Ereignisse in der Datei. |
| firstSequenceNumber | Ganze Zahl | Die kleinste Sequenznummer aus der Warteschlange. |
| lastSequenceNumber | Ganze Zahl | Die größte Sequenznummer aus der Warteschlange. |
| firstEnqueueTime | string | Die erste Zeit aus der Warteschlange. |
| lastEnqueueTime | string | Die letzte Zeit aus der Warteschlange. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
* Informationen zur Behandlung von Event Hubs-Ereignissen finden Sie unter [Streamen von Big Data in einem Data Warehouse](event-grid-event-hubs-integration.md).
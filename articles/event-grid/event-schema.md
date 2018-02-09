---
title: Azure Event Grid-Ereignisschema
description: "Beschreibt die Eigenschaften, die mit Azure Event Grid für Ereignisse bereitgestellt werden."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: babanisa
ms.openlocfilehash: 2b0039c7b90ef6f003641e096521f84885171c26
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="azure-event-grid-event-schema"></a>Azure Event Grid-Ereignisschema

Dieser Artikel beschreibt die Eigenschaften und das Schema für alle Ereignisse. Ereignisse bestehen aus einer Gruppe von fünf erforderlichen Zeichenfolgeneigenschaften und einem erforderlichen Datenobjekt. Die Eigenschaften gelten für alle Ereignisse von jedem Herausgeber. Das Datenobjekt enthält Eigenschaften, die für die einzelnen Herausgeber spezifisch sind. Bei Systemthemen sind diese Eigenschaften spezifisch für den Ressourcenanbieter, z.B. Azure Storage oder Azure Event Hubs.

Ereignisse werden an Azure Event Grid in einem Array gesendet, das mehrere Ereignisobjekte enthalten kann. Bei nur einem einzelnen Ereignis hat das Array die Länge 1. Das Array kann eine Gesamtgröße von bis zu 1 MB haben. Jedes Ereignis im Array ist auf 64 KB beschränkt.

Sie finden das JSON-Schema für das Event Grid-Ereignis und die Datennutzlast für jeden Azure-Herausgeber im [Event Schema-Speicher](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane).

## <a name="event-schema"></a>Ereignisschema

Das folgende Beispiel zeigt die Eigenschaften, die von allen Ereignisherausgebern verwendet werden:

```json
[
  {
    "topic": string,
    "subject": string,    
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

Das für ein Azure Blob Storage-Ereignis veröffentlichte Schema sieht beispielsweise folgendermaßen aus:

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```
 
## <a name="event-properties"></a>Ereigniseigenschaften

Alle Ereignisse enthalten die gleichen Daten der obersten Ebene:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| Thema | Zeichenfolge | Vollständiger Ressourcenpfad zu der Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt. |
| subject | Zeichenfolge | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | Zeichenfolge | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | Zeichenfolge | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | Zeichenfolge | Eindeutiger Bezeichner für das Ereignis. |
| data | object | Die für den Ressourcenanbieter spezifischen Ereignisdaten. |
| dataVersion | Zeichenfolge | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | Zeichenfolge | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Weitere Informationen zu den Eigenschaften im Datenobjekt finden Sie in der Ereignisquelle:

* [Azure-Abonnements (Verwaltungsvorgänge)](event-schema-subscriptions.md)
* [Blob Storage](event-schema-blob-storage.md)
* [Event Hubs](event-schema-event-hubs.md)
* [Ressourcengruppen (Verwaltungsvorgänge)](event-schema-resource-groups.md)

Für benutzerdefinierte Themen bestimmt der Ereignisherausgeber das Datenobjekt. Die Daten der obersten Ebene müssen die gleichen Felder wie über Standardressourcen definierte Ereignisse enthalten. Beim Veröffentlichen von Ereignissen in benutzerdefinierten Themen sollten Sie erwägen, den Betreff zur Unterstützung der Weiterleitung und Filterung zu modellieren.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).

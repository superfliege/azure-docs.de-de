---
title: Verwenden von Azure Event Grid mit Ereignissen im CloudEvents-Schema
description: Beschreibt das Festlegen des CloudEvents-Schemas für Ereignisse in Azure Event Grid.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 05/09/2018
ms.author: babanisa
ms.openlocfilehash: 31af59fd7057bef6e427f08cef695688dc2111d1
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Verwenden des CloudEvents-Schemas mit Event Grid

Zusätzlich zu seinem [Standardereignisschema](event-schema.md) unterstützt Azure Event Grid nativ Ereignisse im [CloudEvents-JSON-Schema](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](http://cloudevents.io/) ist eine [Spezifikation eines offenen Standards](https://github.com/cloudevents/spec/blob/master/spec.md) zum allgemeinen Beschreiben von Ereignisdaten.

CloudEvents vereinfacht die Interoperabilität, indem ein allgemeines Ereignisschema für die Veröffentlichung bereitgestellt wird und cloudbasierte Ereignisse genutzt werden. Dieses Schema ermöglicht einheitliche Tools und Standardmöglichkeiten für Routing und Behandlung von Ereignissen sowie universelle Möglichkeiten, das äußere Ereignisschema zu deserialisieren. Mit einem allgemeinen Schema können Sie Arbeit leichter plattformübergreifend integrieren.

CloudEvents wird von mehreren [Partnern](https://github.com/cloudevents/spec/blob/master/community/contributors.md), einschließlich Microsoft, über die [Cloud Native Compute Foundation](https://www.cncf.io/) erstellt. Es ist derzeit als Version 1.0 verfügbar.

Dieser Artikel beschreibt den Einsatz des CloudEvents-Schemas mit Event Grid.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>CloudEvents-Schema

Hier ist ein Beispiel für ein Azure Blob Storage-Ereignis im CloudEvents-Format:

``` JSON
{
    "cloudEventsVersion" : "0.1",
    "eventType" : "Microsoft.Storage.BlobCreated",
    "eventTypeVersion" : "",
    "source" : "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}#blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "eventID" : "173d9985-401e-0075-2497-de268c06ff25",
    "eventTime" : "2018-04-28T02:18:47.1281675Z",
    "data" : {
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
    }
}
```

CloudEvents v0.1 hat folgende Eigenschaften:

| CloudEvents        | Typ     | JSON-Beispielwert             | BESCHREIBUNG                                                        | Event Grid-Zuordnung
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| eventType          | Zeichenfolge   | "com.example.someevent"          | Typ des aufgetretenen Vorkommens                                   | eventType
| eventTypeVersion   | Zeichenfolge   | "1.0"                            | Die Version des eventType (optional)                            | dataVersion
| cloudEventsVersion | Zeichenfolge   | "0.1"                            | Die Version der CloudEvents-Spezifikation, die das Ereignis verwendet        | *übergeben*
| Quelle             | URI      | "/mycontext"                     | Beschreibt den Ereignisproduzenten                                       | topic#subject
| eventID            | Zeichenfolge   | "1234-1234-1234"                 | ID des Ereignisses                                                    | id
| eventTime          | Zeitstempel| "2018-04-05T17:31:00Z"           | Zeitstempel des Ereignisses (optional)                    | eventTime
| schemaURL          | URI      | "https://myschema.com"           | Eine Verknüpfung mit dem Schema, das dem Attribut entspricht (optional) | *nicht verwendet*
| contentType        | Zeichenfolge   | "application/json"               | Beschreibt das Datencodierungsformat (optional)                       | *nicht verwendet*
| Erweiterungen         | Map      | { "extA": "vA", "extB", "vB" }  | Zusätzliche Metadaten (optional)                                 | *nicht verwendet*
| data               | Objekt   | { "objA": "vA", "objB", "vB" }  | Die Ereignisnutzlast (optional)                                       | data

Weitere Informationen finden Sie in der [CloudEvents-Spezifikation](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes).

## <a name="configure-event-grid-for-cloudevents"></a>Konfigurieren von Event Grid für CloudEvents

Gegenwärtig bietet Azure Event Grid Vorschauunterstützung für die CloudEvents-JSON-Formateingabe und -ausgabe in **USA, Westen-Mitte**, **USA, Mitte** und **Europa, Norden**.

Sie können Event Grid für die Eingabe und Ausgabe von Ereignissen im CloudEvents-Schema verwenden. Sie können CloudEvents für Systemereignisse wie Blob Storage-Ereignisse und IoT Hub-Ereignisse sowie benutzerdefinierte Ereignisse verwenden. Diese Ereignisse können auch bei der Übertragung hin und her transformiert werden.


| Eingabeschema       | Ausgabeschema
|--------------------|---------------------
| CloudEvents-Format | CloudEvents-Format
| Event Grid-Format  | CloudEvents-Format
| CloudEvents-Format | Event Grid-Format
| Event Grid-Format  | Event Grid-Format

Für alle Ereignisschemas setzt Event Grid beim Veröffentlichen in einem Event Grid-Thema und Erstellen eines Ereignisabonnements eine Überprüfung voraus. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).

### <a name="input-schema"></a>Eingabeschema

Um das Eingabeschema auf ein benutzerdefiniertes Thema in CloudEvents festzulegen, verwenden Sie beim Erstellen Ihres Themas `--input-schema cloudeventv01schema` den folgenden Parameter in Azure CLI. Das benutzerdefinierte Thema erwartet nun eingehende Ereignisse im CloudEvents v0.1-Format.

Verwenden Sie zum Erstellen eines benutzerdefinierten Event Grid-Themas:

```azurecli
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventv01schema
```

Die aktuelle Version von CloudEvents unterstützt nicht die Batchverarbeitung von Ereignissen. Um Ereignisse mit einem CloudEvent-Schema in einem Thema zu veröffentlichen, veröffentlichen Sie jedes Ereignis einzeln.

### <a name="output-schema"></a>Ausgabeschema

Um das Ausgabeschema auf ein Ereignisabonnement in CloudEvents festzulegen, verwenden Sie beim Erstellen Ihres Ereignisabonnements `--event-delivery-schema cloudeventv01schema` den folgenden Parameter in Azure CLI. Ereignisse für dieses Ereignisabonnement werden jetzt im CloudEvents v0.1-Format übermittelt.

Verwenden Sie zum Erstellen eines Ereignisabonnements:

```azurecli
az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

Die aktuelle Version von CloudEvents unterstützt nicht die Batchverarbeitung von Ereignissen. Ein Ereignisabonnement, das für ein CloudEvent-Schema konfiguriert ist, empfängt jedes Ereignis einzeln.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Überwachung von Ereignisübermittlungen finden Sie unter [Überwachen der Event Grid-Nachrichtenübermittlung](monitor-event-delivery.md).
* Wir ermutigen Sie, CloudEvents zu testen, zu kommentieren und dazu [beizutragen](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).

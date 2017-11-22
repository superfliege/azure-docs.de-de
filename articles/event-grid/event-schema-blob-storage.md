---
title: "Azure Event Grid-Ereignisschema für Blob Storage"
description: "Beschreibt die Eigenschaften, die mit Azure Event Grid für Blob Storage-Ereignisse bereitgestellt werden."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: bdc64733b75fd809cf0245986aa96370343c1a34
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Azure Event Grid-Ereignisschema für Blob Storage

In diesem Artikel werden die Eigenschaften und das Schema für Blob Storage-Ereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

## <a name="available-event-types"></a>Verfügbare Ereignistypen

Blob Storage gibt die folgenden Ereignistypen aus:

| Ereignistypen | Beschreibung |
| ---------- | ----------- |
| Microsoft.Storage.BlobCreated | Wird ausgelöst, wenn ein Blob erstellt wird. |
| Microsoft.Storage.BlobDeleted | Wird ausgelöst, wenn ein Blob gelöscht wird. |

## <a name="example-event"></a>Beispielereignis

Das folgende Beispiel zeigt das Schema eines Blob-Erstellungsereignisses: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  }
}]
```

Das Schema für ein Blob-Löschungsereignis sieht ähnlich aus: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  }
}]
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
| data | objekt | Ereignisdaten für Blob Storage. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | Beschreibung |
| -------- | ---- | ----------- |
| api | string | Der Vorgang, durch den das Ereignis ausgelöst wurde. |
| clientRequestId | string | Ein vom Client generierter, nicht transparenter Wert mit einer Zeichenbeschränkung von 1 KB. Wenn Sie die Speicheranalyseprotokollierung aktiviert haben, wird er in den Analyseprotokollen aufgezeichnet. |
| requestId | string | Der eindeutige Bezeichner für die Anforderung. Verwenden Sie diesen für die Problembehandlung bei der Anforderung. |
| eTag | string | Der Wert, den Sie verwenden können, um Vorgänge bedingt auszuführen. |
| contentType | string | Der für das Blob angegebene Inhaltstyp. |
| contentLength | Ganze Zahl | Die Größe des Blobs in Byte. |
| blobType | string | Der Blobtyp. |
| url | string | Der Pfad des Blobs. |
| sequencer | string | Ein vom Benutzer festgelegter Wert, mit dem Sie Anforderungen nachverfolgen können. |
| storageDiagnostics | objekt | Informationen zur Speicherdiagnose. |
 
## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
* Eine Einführung zum Arbeiten mit Blob Storage-Ereignissen finden Sie unter [Routen von Blob Storage-Ereignissen -– Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 

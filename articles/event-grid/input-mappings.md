---
title: Zuordnen benutzerdefinierter Felder zum Azure Event Grid-Schema
description: Dieser Artikel beschreibt, wie Sie ein benutzerdefiniertes Schema in das Azure Event Grid-Schema konvertieren.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: tomfitz
ms.openlocfilehash: 32f93f383ec4044afb0696fcef1705c9ed65d673
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34301966"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Zuordnen benutzerdefinierter Felder zum Event Grid-Schema

Auch wenn Ihre Daten nicht dem erwarteten [Event Grid-Schema](event-schema.md) entsprechen, können Sie dennoch Event Grid verwenden, um Ereignisse an Abonnenten weiterzuleiten. Dieser Artikel beschreibt, wie Sie Ihr Schema dem Event Grid-Schema zuordnen.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Ursprüngliches Ereignisschema

Angenommen, Sie verfügen über eine Anwendung, die Ereignisse in folgendem Format sendet:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Obwohl dieses Format nicht dem erforderlichen Schema entspricht, ermöglicht Event Grid es Ihnen, Ihre Felder dem Schema zuzuordnen. Alternativ dazu können Sie die Werte auch im ursprünglichen Schema empfangen.

## <a name="create-custom-topic-with-mapped-fields"></a>Erstellen eines benutzerdefinierten Themas mit zugeordneten Feldern

Wenn Sie ein benutzerdefiniertes Thema erstellen, geben Sie an, wie Felder aus Ihrem ursprünglichen Ereignis dem Event Grid-Schema zugeordnet werden sollen. Zum Anpassen der Zuordnung werden drei Eigenschaften verwendet:

* Der `--input-schema`-Parameter gibt den Typ des Schemas an. Die verfügbaren Optionen lauten *cloudeventv01schema*, *customeventschema* und *eventgridschema*. Der Standardwert ist „eventgridschema“. Wenn Sie eine benutzerdefinierte Zuordnung zwischen Ihrem Schema und dem Event Grid-Schema erstellen, verwenden Sie „customeventschema“. Wenn Ereignisse im CloudEvents-Schema vorhanden sind, verwenden Sie „cloudeventv01schema“.

* Der `--input-mapping-default-values`-Parameter gibt Standardwerte für Felder im Event Grid-Schema an. Sie können Standardwerte für *subject*, *eventtype* und *dataversion* festlegen. In der Regel verwenden Sie diesen Parameter, wenn Ihr benutzerdefiniertes Schema kein Feld enthält, das einem dieser drei Felder entspricht. Sie können z.B. angeben, dass „dataversion“ immer auf **1.0** festgelegt wird.

* Der `--input-mapping-fields`-Parameter ordnet Felder aus Ihrem Schema dem Event Grid-Schema zu. Werte werden in mit Leerzeichen getrennten Schlüssel-Wert-Paaren angegeben. Verwenden Sie als Schlüsselnamen den Namen des Event Grid-Felds. Als Wert verwenden Sie den Namen Ihres Felds. Sie können Schlüsselnamen für *id*, *topic*, *eventtime*, *subject*, *eventtype* und *dataversion* verwenden.

Das folgende Beispiel erstellt ein benutzerdefiniertes Thema mit einigen zugeordneten und einigen Standardfeldern:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

## <a name="subscribe-to-event-grid-topic"></a>Abonnieren eines Event Grid-Themas

Wenn Sie das benutzerdefinierte Thema abonnieren, geben Sie das Schema an, das Sie für den Empfang der Ereignisse verwenden möchten. Sie verwenden den `--event-delivery-schema`-Parameter und legen ihn auf *cloudeventv01schema*, *eventgridschema* oder *inputeventschema* fest. Der Standardwert ist „eventgridschema“.

Die Beispiele in diesem Abschnitt verwenden einen Queue Storage-Instanz für den Ereignishandler. Weitere Informationen finden Sie unter [Weiterleiten von benutzerdefinierten Ereignissen an Azure Queue Storage](custom-event-to-queue-storage.md).

Das folgende Beispiel abonniert ein Event Grid-Thema und verwendet das Event Grid-Standardschema:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub1 \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

Das nächste Beispiel verwendet das Eingabeschema des Ereignisses:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub2 \
  --event-delivery-schema inputeventschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

## <a name="publish-event-to-topic"></a>Veröffentlichen des Ereignisses im Thema

Sie können jetzt ein Ereignis an das benutzerdefinierte Thema senden und die Ergebnisse der Zuordnung anzeigen. Das folgende Skript postet ein Ereignis im [Beispielschema](#original-event-schema):

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/mapeventfields.json)'")

curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Sehen Sie sich jetzt Ihre Queue Storage-Instanz an. Die beiden Abonnements haben Ereignisse in verschiedenen Schemas übermittelt.

Das erste Abonnement hat das Event Grid-Schema verwendet. Das übermittelte Ereignis liegt in folgendem Format vor:

```json
{
  "Id": "016b3d68-881f-4ea3-8a9c-ed9246582abe",
  "EventTime": "2018-05-01T20:00:25.2606434Z",
  "EventType": "Created",
  "DataVersion": "1.0",
  "MetadataVersion": "1",
  "Topic": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.EventGrid/topics/demotopic",
  "Subject": "DefaultSubject",
  "Data": {
    "myEventTypeField": "Created",
    "resource": "Users/example/Messages/1000",
    "resourceData": { "someDataField1": "SomeDataFieldValue" } 
  }
}
```

Diese Felder enthalten die Zuordnungen aus dem benutzerdefinierten Thema. **myEventTypeField** ist **EventType** zugeordnet. Es werden die Standardwerte für **DataVersion** und **Subject** verwendet. Das **Data**-Objekt enthält die ursprünglichen Ereignisschemafelder.

Das zweite Abonnement verwendet das Eingabeereignisschema. Das übermittelte Ereignis liegt in folgendem Format vor:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": { "someDataField1": "SomeDataFieldValue" }
}
```

Beachten Sie, dass die ursprünglichen Felder übermittelt wurden.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Ereignisübermittlungen und Wiederholungen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).
* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).

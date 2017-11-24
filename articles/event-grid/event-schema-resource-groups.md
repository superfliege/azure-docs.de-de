---
title: Ereignisschema der Azure Event Grid-Ressourcengruppe
description: "Beschreibt die Eigenschaften, die mit Azure Event Grid für Ressourcengruppenereignisse bereitgestellt werden."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: c7435d87f9aaa906c3f6758186b64f3458cb9716
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Azure Event Grid-Ereignisschema für Ressourcengruppen

In diesem Artikel werden die Eigenschaften und das Schema für Ressourcengruppenereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

Azure-Abonnements und Ressourcengruppen geben die gleichen Ereignistypen aus. Die Ereignistypen beziehen sich auf Änderungen in den Ressourcen. Der Hauptunterschied besteht darin, dass Ressourcengruppen Ereignisse für Ressourcen innerhalb der Ressourcengruppe und Azure-Abonnements Ereignisse für Ressourcen im gesamten Abonnement ausgeben. 

## <a name="available-event-types"></a>Verfügbare Ereignistypen

Ressourcengruppen können Verwaltungsereignisse von Azure Resource Manager ausgeben, beispielsweise wenn eine VM erstellt oder ein Speicherkonto gelöscht wird.

| Ereignistypen | Beschreibung |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Wird ausgelöst, wenn ein Vorgang zum Erstellen oder Aktualisieren einer Ressource erfolgreich ist. |
| Microsoft.Resources.ResourceWriteFailure | Wird ausgelöst, wenn ein Vorgang zum Erstellen oder Aktualisieren einer Ressource fehlschlägt. |
| Microsoft.Resources.ResourceWriteCancel | Wird ausgelöst, wenn ein Vorgang zum Erstellen oder Aktualisieren einer Ressource abgebrochen wird. |
| Microsoft.Resources.ResourceDeleteSuccess | Wird ausgelöst, wenn ein Vorgang zum Löschen einer Ressource erfolgreich ist. |
| Microsoft.Resources.ResourceDeleteFailure | Wird ausgelöst, wenn ein Vorgang zum Löschen einer Ressource fehlschlägt. |
| Microsoft.Resources.ResourceDeleteCancel | Wird ausgelöst, wenn ein Vorgang zum Löschen einer Ressource abgebrochen wird. Dieses Ereignis tritt ein, wenn eine Vorlagenbereitstellung abgebrochen wird. |

## <a name="example-event"></a>Beispielereignis

Das folgende Beispiel zeigt das Schema eines Ressourcenerstellungsereignisses: 

```json
[
    {
    "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"{request-operation}",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```

Das Schema für ein Ressourcenlöschungsereignis sieht ähnlich aus:

```json
[{
  "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicApp0ecd6c02-2296-4d7c-9865-01532dc99c93",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2017-11-07T21:24:19.6959483Z",
  "id": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
  "data": {
    "authorization": "{azure_resource_manager_authorizations}",
    "claims": "{azure_resource_manager_claims}",
    "correlationId": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
    "httpRequest": "{request-operation}",
    "resourceProvider": "Microsoft.EventGrid",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "operationName": "Microsoft.EventGrid/eventSubscriptions/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47"
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
| data | objekt | Ereignisdaten der Ressourcengruppe. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | Beschreibung |
| -------- | ---- | ----------- |
| authorization | string | Die angeforderte Autorisierung für den Vorgang. |
| claims | string | Die Eigenschaften der Ansprüche. |
| correlationId | string | Eine Vorgangs-ID für die Problembehandlung. |
| httpRequest | string | Die Details des Vorgangs. |
| resourceProvider | string | Der Ressourcenanbieter, der den Vorgang ausführt. |
| resourceUri | string | Der URI der Ressource im Vorgang. |
| operationName | string | Der Vorgang, der ausgeführt wurde. |
| status | string | Der Status des Vorgangs. |
| subscriptionId | string | Die Abonnement-ID der Ressource. |
| tenantId | string | Die Mandanten-ID der Ressource. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).

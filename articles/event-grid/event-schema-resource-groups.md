---
title: Ereignisschema der Azure Event Grid-Ressourcengruppe
description: Beschreibt die Eigenschaften, die mit Azure Event Grid für Ressourcengruppenereignisse bereitgestellt werden.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 08/02/2018
ms.author: tomfitz
ms.openlocfilehash: 407d9fd5b6f4d554af37b60edf12422f8816ac00
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495321"
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Azure Event Grid-Ereignisschema für Ressourcengruppen

In diesem Artikel werden die Eigenschaften und das Schema für Ressourcengruppenereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

Azure-Abonnements und Ressourcengruppen geben die gleichen Ereignistypen aus. Die Ereignistypen beziehen sich auf Änderungen in den Ressourcen. Der Hauptunterschied besteht darin, dass Ressourcengruppen Ereignisse für Ressourcen innerhalb der Ressourcengruppe und Azure-Abonnements Ereignisse für Ressourcen im gesamten Abonnement ausgeben.

Ressourcenereignisse werden für PUT-, PATCH- und DELETE-Vorgänge erstellt, die an `management.azure.com` gesendet werden. Bei POST- und GET-Vorgängen werden keine Ereignisse erstellt. Bei Vorgängen, die an die Datenebene gesendet werden (z.B. `myaccount.blob.core.windows.net`), werden keine Ereignisse erstellt.

Wenn Sie Ereignisse für eine Ressourcengruppe abonnieren, erhält Ihr Endpunkt alle Ereignisse für diese Ressourcengruppe. Hierzu können Ereignisse gehören, die für Ihre Zwecke angezeigt werden sollen, z.B. zur Aktualisierung eines virtuellen Computers. Es können aber auch Ereignisse vorhanden sein, die für Sie ggf. weniger wichtig sind, z.B. das Schreiben eines neuen Eintrags im Bereitstellungsverlauf. Sie können alle Ereignisse an Ihrem Endpunkt empfangen und Code schreiben, mit dem die gewünschten Ereignisse verarbeitet werden. Beim Erstellen des Ereignisabonnements können Sie aber auch einen Filter festlegen.

Zur programmgesteuerten Verarbeitung von Ereignissen können Sie Ereignisse sortieren, indem Sie sich den Wert für `operationName` ansehen. Beispielsweise werden für Ihren Endpunkt ggf. nur Ereignisse für Vorgänge verarbeitet, die `Microsoft.Compute/virtualMachines/write` oder `Microsoft.Storage/storageAccounts/write` entsprechen.

Der Betreff des Ereignisses ist die Ressourcen-ID der Ressource, die das Ziel des Vorgangs ist. Geben Sie zum Filtern von Ereignissen für eine Ressource beim Erstellen des Ereignisabonnements die Ressourcen-ID an. Informationen zu Beispielskripts finden Sie unter [Abonnieren von Ereignissen für eine Ressourcengruppe und Filtern nach einer Ressource mit PowerShell](scripts/event-grid-powershell-resource-group-filter.md) oder [Abonnieren von Ereignissen für eine Ressourcengruppe und Filtern nach einer Ressource mit der Azure CLI](scripts/event-grid-cli-resource-group-filter.md). Verwenden Sie zum Filtern nach einem Ressourcentyp einen Wert im folgenden Format: `/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`

## <a name="available-event-types"></a>Verfügbare Ereignistypen

Ressourcengruppen können Verwaltungsereignisse von Azure Resource Manager ausgeben, beispielsweise wenn eine VM erstellt oder ein Speicherkonto gelöscht wird.

| Ereignistypen | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Wird ausgelöst, wenn ein Vorgang zum Erstellen oder Aktualisieren einer Ressource erfolgreich ist. |
| Microsoft.Resources.ResourceWriteFailure | Wird ausgelöst, wenn ein Vorgang zum Erstellen oder Aktualisieren einer Ressource fehlschlägt. |
| Microsoft.Resources.ResourceWriteCancel | Wird ausgelöst, wenn ein Vorgang zum Erstellen oder Aktualisieren einer Ressource abgebrochen wird. |
| Microsoft.Resources.ResourceDeleteSuccess | Wird ausgelöst, wenn ein Vorgang zum Löschen einer Ressource erfolgreich ist. |
| Microsoft.Resources.ResourceDeleteFailure | Wird ausgelöst, wenn ein Vorgang zum Löschen einer Ressource fehlschlägt. |
| Microsoft.Resources.ResourceDeleteCancel | Wird ausgelöst, wenn ein Vorgang zum Löschen einer Ressource abgebrochen wird. Dieses Ereignis tritt ein, wenn eine Vorlagenbereitstellung abgebrochen wird. |

## <a name="example-event"></a>Beispielereignis

Im folgenden Beispiel wird das Schema für das Ereignis **ResourceWriteSuccess** veranschaulicht. Das gleiche Schema wird für die Ereignisse **ResourceWriteFailure** und **ResourceWriteCancel** verwendet, nur mit anderen Werten für `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

Im folgenden Beispiel wird das Schema für das Ereignis **ResourceDeleteSuccess** veranschaulicht. Das gleiche Schema wird für die Ereignisse **ResourceDeleteFailure** und **ResourceDeleteCancel** verwendet, nur mit anderen Werten für `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

## <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| Thema | Zeichenfolge | Vollständiger Ressourcenpfad zu der Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt. |
| subject | Zeichenfolge | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | Zeichenfolge | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | Zeichenfolge | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | Zeichenfolge | Eindeutiger Bezeichner für das Ereignis. |
| data | object | Ereignisdaten der Ressourcengruppe. |
| dataVersion | Zeichenfolge | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | Zeichenfolge | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| authorization | object | Die angeforderte Autorisierung für den Vorgang. |
| claims | object | Die Eigenschaften der Ansprüche. Weitere Informationen finden Sie auf der Seite zur [JWT-Spezifikation](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | Zeichenfolge | Eine Vorgangs-ID für die Problembehandlung. |
| httpRequest | object | Die Details des Vorgangs. Dieses Objekt ist nur enthalten, wenn eine vorhandene Ressource aktualisiert oder eine Ressource gelöscht wird. |
| resourceProvider | Zeichenfolge | Der Ressourcenanbieter, der den Vorgang ausführt. |
| resourceUri | Zeichenfolge | Der URI der Ressource im Vorgang. |
| operationName | Zeichenfolge | Der Vorgang, der ausgeführt wurde. |
| status | Zeichenfolge | Der Status des Vorgangs. |
| subscriptionId | Zeichenfolge | Die Abonnement-ID der Ressource. |
| tenantId | Zeichenfolge | Die Mandanten-ID der Ressource. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).

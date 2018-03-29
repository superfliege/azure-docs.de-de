---
title: Azure Event Grid-Abonnement mit Vorlage
description: Erstellen Sie ein Azure Event Grid-Abonnement mithilfe einer Resource Manager-Vorlage
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: ee0b2c228ae4ea53c0ee9794529aa190334ceed9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="use-resource-manager-template-for-event-grid-subscription"></a>Verwenden einer Resource Manager-Vorlage zum Erstellen eines Azure Event Grid-Abonnements

In diesem Artikel erfahren Sie, wie Sie mit einer Azure Resource Manager-Vorlage Azure Event Grid-Abonnements erstellen können. Das Format, das Sie verwenden, hängt davon ab, ob Sie Ressourcengruppenereignisse oder Ereignisse für einen bestimmten Ressourcentyp abonnieren. In diesem Artikel werden beide Formate vorgestellt.

## <a name="subscribe-to-resource-group-events"></a>Abonnieren von Ressourcengruppenereignissen

Verwenden Sie beim Abonnieren von Ressourcengruppenereignissen für den Ressourcentyp `Microsoft.EventGrid/eventSubscriptions`. Für den Ereignispunkttyp geben Sie entweder `WebHook` oder `EventHub` an.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.EventGrid/eventSubscriptions",
            "name": "mySubscription",
            "apiVersion": "2018-01-01",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false,
                    "includedEventTypes": ["All"]
                }
            }
        }
    ]
}
```

Wenn Sie diese Vorlage für eine Ressourcengruppe bereitstellen, abonnieren Sie Ereignisse für diese Ressourcengruppe.

## <a name="subscribe-to-resource-events"></a>Abonnieren von Ressourcenereignissen

Beim Abonnieren von Ressourcenereignissen ordnen Sie das Abonnement der gewünschten Ressource zu, indem Sie den Ressourcentyp und -namen in die Abonnementdefinition aufnehmen. Verwenden Sie für den Ressourcentyp `<provider-namespace>/<resource-type>/providers/eventSubscriptions`. Verwenden Sie `<resource-name>/Microsoft.EventGrid/<subscription-name>` für den Namen. Für den Ereignispunkttyp geben Sie entweder `WebHook` oder `EventHub` an.

Das folgende Beispiel zeigt, wie Blob Storage-Ereignisse abonniert werden.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
            "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/myStorageSubscription')]",
            "apiVersion": "2018-01-01",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Eine Einführung in den Ressourcen-Manager finden Sie unter [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
* Informationen zu den ersten Schritten mit Event Grid finden Sie unter [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure CLI und Event Grid](custom-event-quickstart.md).
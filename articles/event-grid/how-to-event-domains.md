---
title: Verwalten großer Themenmengen in Azure Event Grid und Veröffentlichen von Ereignissen für diese mithilfe von Ereignisdomänen
description: Zeit, wie große Themenmengen in Azure Event Grid erstellt und verwaltet und Ereignisse für diese mithilfe von Ereignisdomänen veröffentlicht werden.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 48a5356b03e38e864ba76f048febdb0b040893f5
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669297"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Verwalten von Themen und Veröffentlichen von Ereignissen mithilfe von Ereignisdomänen

In diesem Artikel werden die folgenden Aufgaben erläutert:

* Erstellen einer Event Grid-Domäne
* Abonnieren von Themen
* Auflisten von Schlüsseln
* Veröffentlichen von Ereignissen für eine Domäne

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-event-domain"></a>Erstellen einer Ereignisdomäne

Das Erstellen einer Ereignisdomäne kann über die `eventgrid`-Erweiterung für die [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) erfolgen. Nachdem Sie eine Domäne erstellt haben, können Sie sie verwenden, um große Mengen von Themen zu verwalten.

```azurecli-interactive
# if you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name>
  -l <location>
```

Bei einer erfolgreichen Erstellung wird Folgendes zurückgegeben:

```json
{
  "endpoint": "https://<my-domain-name>.westus2-1.eventgrid.azure.net/api/events",
  "id": "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>",
  "inputSchema": "EventGridSchema",
  "inputSchemaMapping": null,
  "location": "westus2",
  "name": "<my-domain-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<my-resource-group>",
  "tags": null,
  "type": "Microsoft.EventGrid/domains"
}
```

Beachten Sie die `endpoint`- und `id`-Objekte, da diese zum Verwalten der Domäne und Veröffentlichen von Ereignissen erforderlich sind.

## <a name="create-topics-and-subscriptions"></a>Erstellen von Themen und Abonnements

Der Event Grid-Dienst erstellt und verwaltet automatisch das entsprechende Thema in einer Domäne basierend auf dem Aufruf zum Erstellen eines Ereignisabonnements für ein Domänenthema. Es gibt keinen separaten Schritt zum Erstellen eines Themas in einer Domäne. Wenn das letzte Ereignisabonnement für ein Thema gelöscht wird, wird analog dazu das Thema ebenfalls gelöscht.

Das Abonnieren eines Themas in einer Domäne ist identisch mit dem Abonnieren beliebiger anderer Azure-Ressourcen:

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/<my-topic>" \
  --endpoint https://contoso.azurewebsites.net/api/f1?code=code
```

Die angegebene Ressourcen-ID ist die gleiche ID, die beim Erstellen der Domäne zuvor zurückgegeben wurde. Um das Thema anzugeben, das Sie abonnieren möchten, fügen Sie `/topics/<my-topic>` am Ende der Ressourcen-ID hinzu.

Um ein Domänenbereichereignis-Abonnement zu erstellen, das alle Ereignisse in der Domäne empfängt, geben Sie die Domäne als `resource-id` an, ohne Themen anzugeben, z.B. `/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>`.

Wenn Sie einen Testendpunkt benötigen, um Ihre Ereignisse zu abonnieren, können Sie jederzeit eine [vordefinierte Webanwendung](https://github.com/Azure-Samples/azure-event-grid-viewer) bereitstellen, die die eingehenden Ereignisse anzeigt. Sie können die Ereignisse an Ihre Testwebsite unter `https://<your-site-name>.azurewebsites.net/api/updates` senden.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Berechtigungen, die für ein Thema festgelegt sind, werden in Azure Active Directory gespeichert und müssen explizit gelöscht werden. Das Löschen eines Ereignisabonnements widerruft nicht den Zugriff eines Benutzers zum Erstellen von Ereignisabonnements, wenn er Schreibzugriff für ein Thema besitzt.

## <a name="manage-access-to-topics"></a>Verwalten des Zugriffs auf Themen

Das Verwalten des Zugriffs auf Themen erfolgt über eine [Rollenzuweisung](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-cli). Die Rollenzuweisung verwendet rollenbasierten Zugriff (RBAC), um die Vorgänge mit Azure-Ressourcen auf autorisierte Benutzer in einem bestimmten Bereich einzuschränken.

Event Grid verfügt über zwei integrierte Rollen, mit denen Sie bestimmten Benutzern Zugriff auf verschiedene Themen innerhalb einer Domäne zuweisen können. Diese Rollen sind `EventGrid EventSubscription Contributor (Preview)` (ermöglicht das Erstellen und Löschen von Abonnements) und `EventGrid EventSubscription Reader (Preview)` (ermöglicht nur das Auflisten von Ereignisabonnements).

Der folgende Befehl würde `alice@contoso.com` auf das Erstellen und Löschen von Ereignisabonnements ausschließlich für das Thema `foo` beschränken:

```azurecli-interactive
az role assignment create --assignee alice@contoso.com --role "EventGrid EventSubscription Contributor (Preview)" --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/foo
```

Weitere Informationen zu den folgenden Themen finden Sie unter [Event Grid: Sicherheit und Authentifizierung](./security-authentication.md):

* Verwaltungszugriffssteuerung
* Vorgangstypen
* Erstellen benutzerdefinierter Rollendefinitionen

## <a name="publish-events-to-an-event-grid-domain"></a>Veröffentlichen von Ereignissen in einer Event Grid-Domäne

Das Veröffentlichen von Ereignissen für eine Domäne ist identisch mit dem [Veröffentlichen für ein benutzerdefiniertes Thema](./post-to-custom-topic.md). Der einzige Unterschied besteht darin, dass Sie das Thema angeben müssen, an das jedes Ereignis gesendet werden soll. Das folgende Array von Ereignissen würde zu einem Ereignis mit `"id": "1111"` für Thema `foo` führen, während ein Ereignis mit `"id": "2222"` an Thema `bar` gesendet würde:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Verwenden Sie Folgendes zum Abrufen der Schlüssel für eine Domäne:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

Verwenden Sie dann Ihre bevorzugte Methode zum Erstellen eines HTTP-POST-Aufrufs, um Ihre Ereignisse für Ihre Event Grid-Domäne zu veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu grundlegenden Konzepten in Ereignisdomänen und zum Grund ihrer Nützlichkeit finden Sie unter [Konzeptionelle Übersicht über Ereignisdomänen](./event-domains.md).
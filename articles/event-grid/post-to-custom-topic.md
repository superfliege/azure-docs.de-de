---
title: Posten eines Ereignisses in einem benutzerdefinierten Azure Event Grid-Thema
description: Erfahren Sie, wie Sie ein Ereignis in einem benutzerdefinierten Thema für Azure Event Grid posten.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/05/2018
ms.author: tomfitz
ms.openlocfilehash: 1c23aef0773ffddbc26e4090ecf137b632394ee3
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Posten eines Ereignisses in einem benutzerdefinierten Azure Event Grid-Thema

In diesem Artikel wird beschrieben, wie Sie ein Ereignis in einem benutzerdefinierten Thema posten. Es wird außerdem das Format von Postings und Ereignisdaten gezeigt. Die [Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) gilt nur für Postings, die dem erwarteten Format entsprechen.

## <a name="endpoint"></a>Endpunkt

Verwenden Sie beim Senden des HTTP-POST-Befehls an ein benutzerdefiniertes Thema das URI-Format: `https://<topic-endpoint>?api-version=2018-01-01`.

`https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01` ist beispielsweise ein gültiger URI.

Um den Endpunkt für ein benutzerdefiniertes Thema mit der Azure CLI abzurufen, verwenden Sie diesen Befehl:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Um den Endpunkt für ein benutzerdefiniertes Thema mit Azure PowerShell abzurufen, verwenden Sie diesen Befehl:

```powershell
(Get-AzureRmEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Header

Schließen Sie einen Headerwert namens `aeg-sas-key` in die Anforderung ein, der einen Schlüssel für die Authentifizierung enthält.

Ein gültiger Headerwert ist beispielsweise `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`.

Um den Schlüssel für ein benutzerdefiniertes Thema mit der Azure CLI abzurufen, verwenden Sie diesen Befehl:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

Um den Schlüssel für ein benutzerdefiniertes Thema mit PowerShell abzurufen, verwenden Sie diesen Befehl:

```powershell
(Get-AzureRmEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Ereignisdaten

Für benutzerdefinierte Themen müssen die Daten der obersten Ebene müssen die gleichen Felder enthalten wie die über Standardressourcen definierten Ereignisse. Eine dieser Eigenschaften ist eine Dateneigenschaft, die eindeutige Eigenschaften für das benutzerdefinierte Thema enthält. Als Ereignisherausgeber legen Sie die Eigenschaften für dieses Datenobjekt fest. Verwenden Sie das folgende Schema:

```json
[
  {
    "id": string,    
    "eventType": string,
    "subject": string,
    "eventTime": string-in-date-time-format,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string
  }
]
```

Eine Beschreibung dieser Eigenschaften finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

Ein gültiges Ereignisdatenschema lautet beispielsweise wie folgt:

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0"
}]
```

## <a name="response"></a>response

Nachdem Sie das Posten an den Themenendpunkt durchgeführt haben, erhalten Sie eine Antwort. Die Antwort ist ein HTTP-Standardantwortcode. Einige häufige Antworten lauten:

|Ergebnis  |Antwort  |
|---------|---------|
|Erfolgreich  | 200 – OK  |
|Falscher Endpunkt | 404 – Nicht gefunden |
|Ungültiger Zugriffsschlüssel | 401 – Nicht autorisiert |
|Fehlerhaftes Format der Ereignisdaten | 400 – Ungültige Anforderung |

Für Fehler hat der Nachrichtentext das folgende Format:

```json
{
    "error": {
        "code": "<HTTP status code>",
        "message": "<description>",
        "details": [{
            "code": "<HTTP status code>",
            "message": "<description>"
    }]
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Überwachung von Ereignisübermittlungen finden Sie unter [Überwachen der Event Grid-Nachrichtenübermittlung](monitor-event-delivery.md).
* Weitere Informationen zum Authentifizierungsschlüssel finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).

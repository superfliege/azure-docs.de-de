---
title: Weiterleiten von Azure Media Services-Ereignissen an einen benutzerdefinierten Webendpunkt | Microsoft-Dokumentation
description: Verwenden Sie Azure Event Grid zum Abonnieren des Ereignisses einer Media Services-Auftragsstatusänderung.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/20/2018
ms.author: juliako
ms.openlocfilehash: e7268a066acf41c454de0c66aa21603199d85a60
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034840"
---
# <a name="route-azure-media-services-events-to-a-custom-web-endpoint-using-cli"></a>Weiterleiten von Azure Media Services-Ereignissen an einen benutzerdefinierten Webendpunkt mithilfe der CLI

Azure Event Grid ist ein Ereignisdienst für die Cloud. In diesem Artikel abonnieren Sie Azure Media Services-Ereignisse zur Auftragsstatusänderung mithilfe der Azure CLI und lösen das Ereignis zum Anzeigen der Ergebnisse aus. 

Ereignisse werden üblicherweise an einen Endpunkt gesendet, der auf das Ereignis reagiert (beispielsweise ein Webhook oder eine Azure-Funktion). In diesem Tutorial erfahren Sie, wie Sie einen Webhook erstellen und festlegen.

Nach Abschluss der Schritte in diesem Artikel sehen Sie, dass die Ereignisdaten an einen Endpunkt gesendet wurden.

## <a name="prerequisites"></a>Voraussetzungen

- Ein aktives Azure-Abonnement
- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)

    Merken Sie sich die Werte, die Sie für den Namen der Ressourcengruppe und des Media Services-Kontos verwendet haben.

- Installieren Sie die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Für diesen Artikel ist die Azure CLI-Version 2.0 oder höher erforderlich. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Sie können ebenso [Azure Cloud Shell](https://shell.azure.com/bash) verwenden.

## <a name="enable-event-grid-resource-provider"></a>Aktivieren des Event Grid-Ressourcenanbieters

Als Erstes müssen Sie sicherstellen, dass Sie den Event Grid-Ressourcenanbieter in Ihrem Abonnement aktiviert haben. 

Führen Sie im **Azure**-Portal folgende Schritte aus:

1. Wechseln Sie zu „Abonnements“.
2. Wählen Sie Ihr Abonnement aus.
3. Klicken Sie unter „Einstellungen“ auf „Ressourcenanbieter“.
4. Suchen Sie nach „Event Grid“.
5. Stellen Sie sicher, dass Event Grid registriert ist. Falls nicht, klicken Sie auf die Schaltfläche **Registrieren**.  

## <a name="create-a-generic-azure-function-webhook"></a>Erstellen eines generischen Azure Function-Webhooks 

### <a name="create-a-message-endpoint"></a>Erstellen eines Nachrichtenendpunkts

Bevor Sie den Even Grid-Artikel abonnieren, erstellen Sie einen Endpunkt, der die Nachrichten sammelt, damit Sie sie anzeigen können.

Erstellen Sie eine Funktion, die durch einen generischen Webhook ausgelöst wird, wie im Artikel [Generischer Webhook](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function) beschrieben. In diesem Tutorial wird **C#**-Code verwendet.

Sobald der Webhook erstellt ist, kopieren Sie die URL, indem Sie oben im **Azure**-Portalfenster auf den Link *Funktions-URL abrufen* klicken. Der letzte Teil der URL ist nicht erforderlich (*&clientID=default*).

![Erstellen eines Webhooks](./media/job-state-events-cli-how-to/generic_webhook_files.png)

### <a name="validate-the-webhook"></a>Validieren des Webhooks

Wenn Sie Ihren eigenen Webhook-Endpunkt bei Event Grid registrieren, wird Ihnen eine POST-Anforderung mit einem einfachen Validierungscode gesendet, damit Sie den Besitz des Endpunkts nachweisen können. Ihre App muss durch Rückgabe desselben Validierungscodes reagieren. Event Grid übermittelt keine Ereignisse an Webhook-Endpunkte, welche die Validierung nicht erfolgreich abgeschlossen haben. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](https://docs.microsoft.com/azure/event-grid/security-authentication). Dieser Abschnitt legt zwei Teile fest, die für die Validierung definiert werden müssen.

#### <a name="update-the-source-code"></a>Aktualisieren des Quellcodes

Nachdem Sie den Webhook erstellt haben, wird im Browser die Datei **run.csx** angezeigt. Ersetzen Sie den Standardcode durch den folgenden Code. 

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"Webhook was triggered!");

    string jsonContent = await req.Content.ReadAsStringAsync();
    string eventGridValidation = 
        req.Headers.FirstOrDefault( x => x.Key == "Aeg-Event-Type" ).Value?.FirstOrDefault();

    dynamic eventData = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"event: {eventData}");

    if (eventGridValidation != String.Empty)
    {
        if (eventData[0].data.validationCode !=String.Empty && eventData[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent")
        {
            return req.CreateResponse(HttpStatusCode.OK, new 
            {
                validationResponse = eventData[0].data.validationCode
            });
        }
    }
    
    log.Info(jsonContent);

    return req.CreateResponse(HttpStatusCode.OK);
}
```

#### <a name="update-test-request-body"></a>Aktualisieren des Testanforderungstexts

Auf der rechten Seite des **Azure**-Portalfensters sehen Sie zwei Registerkarten: **Dateien anzeigen** und **Testen**. Wählen Sie die Registerkarte **Testen** aus. Geben Sie im Feld **Anforderungstext** folgenden JSON-Code ein. Sie können ihn unverändert einfügen, da keine Werte geändert werden müssen.

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2017-08-06T22:09:30.740323Z"
}
]
```

Klicken Sie oben im Fenster auf **Speichern und ausführen**.

![Anforderungstext](./media/job-state-events-cli-how-to/generic_webhook_test.png)

## <a name="register-for-the-event-grid-subscription"></a>Registrieren für das Event Grid-Abonnement 

Sie abonnieren einen Artikel, um Event Grid mitzuteilen, welche Ereignisse Sie nachverfolgen möchten. Im folgenden Beispiel wird das von Ihnen erstellte Media Services-Konto abonniert. Außerdem wird die URL des von Ihnen erstellten Azure Function-Webhooks als Endpunkt für Ereignisbenachrichtigungen übergeben. 

Ersetzen Sie `<event_subscription_name>` durch einen eindeutigen Namen für Ihr Ereignisabonnement. Verwenden Sie für `<resource_group_name>` und `<ams_account_name>` die Werte, die Sie beim Erstellen des Media Services-Kontos verwendet haben. Fügen Sie für `<endpoint_URL>` Ihre Endpunkt-URL ein. Entfernen Sie *&clientID=default* aus der URL. Durch die Angabe eines Endpunkts beim Abonnieren übernimmt Event Grid die Weiterleitung von Ereignissen an diesen Endpunkt. 

```cli
amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $amsResourceId \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

Der ID-Wert der Media Services-Kontoressource sieht etwa wie folgt aus:

```
/subscriptions/81212121-2f4f-4b5d-a3dc-ba0015515f7b/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amstestaccount
```

## <a name="test-the-events"></a>Testen der Ereignisse

Führen Sie einen Codierungsauftrag aus, z.B. wie im Schnellstart [Streamen von Videodateien](stream-files-dotnet-quickstart.md) beschrieben.

Sie haben das Ereignis ausgelöst, und Event Grid hat die Nachricht an den Endpunkt gesendet, den Sie beim Abonnieren konfiguriert haben. Navigieren Sie zum zuvor erstellten Webhook. Klicken Sie auf **Überwachen** und **Aktualisieren**. Die Statusänderungsereignisse des Auftrags werden angezeigt: „Queued“ (In Warteschlange), „Scheduled“ (Geplant), „Processing“ (Wird verarbeitet), „Finished“ (Abgeschlossen), „Error“ (Fehler), „Canceled“ (Abgebrochen), „Canceling“ (Wird abgebrochen).  Weitere Informationen finden Sie unter [Media Services-Ereignisschemas](media-services-event-schemas.md).

Das folgende Beispiel zeigt das Schema des JobStateChange-Ereignisses:

```json
[{
  "topic": "/subscriptions/<subscription id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job id>",
  "eventType": "Microsoft.Media.JobStateChange",
  "eventTime": "2018-04-20T21:17:26.2534881",
  "id": "<id>",
  "data": {
    "previousState": "Scheduled",
    "state": "Processing"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

![Testen von Ereignissen](./media/job-state-events-cli-how-to/test_events.png)

## <a name="next-steps"></a>Nächste Schritte

[Reagieren auf Ereignisse](reacting-to-media-services-events.md)

## <a name="see-also"></a>Weitere Informationen

[Azure-CLI](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)

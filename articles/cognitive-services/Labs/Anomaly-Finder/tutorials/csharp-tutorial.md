---
title: 'Tutorial: Anomalieerkennung, C#'
titlesuffix: Azure Cognitive Services
description: Lernen Sie eine C#-App kennen, die die Anomalieerkennungs-API verwendet. Senden Sie originale Datenpunkte an die API, und rufen Sie den erwarteten Wert und Anomaliepunkte ab.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: f99ce765c1d9417fd5ca88b49214eca8a3b0bf49
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887648"
---
# <a name="tutorial-anomaly-detection-with-c-application"></a>Tutorial: Anomalieerkennung mit einer C#-Anwendung

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Lernen Sie eine einfache Windows-Anwendung kennen, die die API zur Anomalieerkennung verwendet, um Anomalien aus der Eingabe zu erkennen. Im Beispiel werden die Zeitreihendaten mit Ihrem Abonnementschlüssel an die Anomalieerkennungs-API übermittelt, anschließend werden alle Anomaliepunkte und der erwartete Wert für jeden Datenpunkt bei der API abgerufen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="platform-requirements"></a>Plattformanforderungen

Das Beispiel wurde mit [Visual Studio 2017, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs) für .NET Framework entwickelt. 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonnieren der Anomalieerkennung und Abrufen eines Abonnementschlüssels 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Abrufen und Verwenden des Beispiels

Sie können die Beispielanwendung zur Anomalieerkennung von [Github](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git) auf Ihren Computer klonen. 
<a name="Step1"></a>
### <a name="install-the-example"></a>Installieren des Beispiels

Öffnen Sie in Ihrem GitHub-Desktop „Sample\AnomalyDetectionSample.sln“.

<a name="Step2"></a>
### <a name="build-the-example"></a>Erstellen des Beispiels

Drücken Sie STRG+UMSCHALT+B, oder klicken Sie auf dem Menüband auf „Erstellen“ und anschließend auf „Projektmappe erstellen“.

<a name="Step3"></a>
### <a name="run-the-example"></a>Ausführen des Beispiels

1. Drücken Sie nach Abschluss des Buildvorgangs **F5**, oder klicken Sie auf dem Menüband auf **Starten**, um das Beispiel auszuführen.
2. Finden Sie auf der Benutzeroberfläche der Anomalieerkennung das Fenster mit dem Texteditierfeld „{your_subscription_key}“ (Ihr Abonnementschlüssel).
3. Ersetzen Sie die request.json-Datei, die die Beispieldaten enthält, durch Ihre eigenen Daten, und klicken Sie dann auf die Schaltfläche „Senden“. Microsoft empfängt die Daten, die Sie hochladen, und verwendet sie zum Erkennen eventueller enthaltener Anomaliepunkte. Die von Ihnen geladenen Daten bleiben nicht dauerhaft auf Microsoft-Servern erhalten. Um Anomaliepunkte erneut zu erkennen, müssen Sie die Daten noch einmal hochladen.
4. Wenn die Daten geeignet sind, finden Sie das Ergebnis der Anomalieerkennung im Feld „Response“ (Antwort). Sollten Fehler auftreten, werden die Fehlerinformationen ebenfalls im Feld „Response“ (Antwort) angezeigt.

<a name="Review"></a>
### <a name="read-the-result"></a>Lesen des Ergebnisses

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Überprüfen und Lernen

Nachdem Sie nun über eine aktive Anwendung verfügen, erfahren Sie als Nächstes, wie sich die Integration zwischen dieser Beispiel-App und der Cognitive Services-Technologie gestaltet. Dieser Schritt macht es Ihnen leichter, entweder mit der Weiterentwicklung dieser App fortzufahren oder eine eigene App mithilfe der Microsoft-Anomalieerkennung zu entwickeln.

Diese Beispiel-App nutzt den Endpunkt der Restful-API zur Anomalieerkennung.

Sehen wir uns zur Überprüfung, wie die Restful-API in der Beispielanwendung verwendet wird, einen Codeausschnitt aus **AnomalyDetectionClient.cs** an. Die Datei enthält die Codekommentare „KEY SAMPLE CODE STARTS HERE“ und „KEY SAMPLE CODE ENDS HERE“, die den Beginn und das Ende des betreffenden Beispielcodes angeben, damit Sie die weiter unten reproduzierten Codeausschnitte leichter finden.

```csharp
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

```
### <a name="request"></a>**Anforderung**
Der Codeausschnitt unten zeigt, wie der HttpClient zum Senden Ihres Abonnementschlüssels und der Datenpunkte an den Endpunkt der Anomalieerkennungs-API verwendet wird.

```csharp
    public async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData)
    {
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) })
        {
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Build the request content
            // ---------------------------------------------------------------------- 
            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Send the request content with POST method.
            // ---------------------------------------------------------------------- 
            var res = await client.PostAsync(endpoint, content);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------
            if (res.IsSuccessStatusCode)
            {
                return await res.Content.ReadAsStringAsync();
            }
            else
            {
                return $"ErrorCode: {res.StatusCode}";
            }
        }
    }
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)

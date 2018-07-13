---
title: Javascript-App zur Erkennung von Anomalien – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Lernen Sie eine Javascript-Web-App kennen, die die API zur Anomalieerkennung in Microsoft Cognitive Services verwendet. Senden Sie originale Datenpunkte an die API, und rufen Sie den erwarteten Wert und Anomaliepunkte ab.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 42c3941a05efe8b74f818cd99f3606b3073892a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35375346"
---
# <a name="anomaly-detection-javascript-application"></a>Javascript-Anwendung zur Anomalieerkennung

Lernen Sie eine Webanwendung kennen, die die REST-API zur Anomalieerkennung zum Erkennen einer Anomalie verwendet. Im Beispiel werden die Zeitreihendaten mit Ihrem Abonnementschlüssel an die Anomalieerkennungs-API übermittelt, anschließend werden alle Anomaliepunkte und der erwartete Wert für jeden Datenpunkt bei der API abgerufen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="platform-requirements"></a>Plattformanforderungen

Dieses Tutorial wurde mit einem einfachen Text-Editor entwickelt.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonnieren der Anomalieerkennung und Abrufen eines Abonnementschlüssels 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Abrufen und Verwenden des Beispiels

Dieses Tutorial stellt zwei Szenarien für die Erkennung von Anomalien in Zeitreihendaten zur Verfügung. Lassen Sie uns anfangen.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Herunterladen des Tutorial-Projekts

Klonen Sie das [JavaScript-Tutorial zur Erkennung von Anomalien in Cognitive Services](https://github.com/MicrosoftAnomalyDetection/javascript-sample), oder laden Sie die ZIP-Datei herunter, und extrahieren Sie sie in einem leeren Verzeichnis.

<a name="Step2"></a>
### <a name="run-the-example"></a>Ausführen des Beispiels

Es gibt zwei Szenarien, in denen Sie das Beispiel ausprobieren können.
1. Setzen Sie Ihren **Abonnementschlüssel** in das Feld „Subscription Key“ (Abonnementschlüssel) der Erkennungsfunktion auf „anomalydetection.html“ ein.
2. Legen Sie den Endpunkt für die API zur Anomalieerkennung fest, und überprüfen Sie in „Subscription Region“ (Abonnementregion), ob Sie die richtige Region verwenden.
3. Öffnen Sie die **anomalydetection.html**-Datei in einem Webbrowser.

**Szenario 1: Erkennen von Daten einer wöchentlichen Zeitreihe**
1. Geben Sie im Feld „Period“ (Zeitraum) den Zeitraum **7** ein. 
2. Ersetzen Sie im Feld „Points“ (Punkte) die Beispieldaten durch die Datenpunkte Ihrer wöchentlichen Zeitreihe (JSON), oder verwenden Sie die Beispieldaten direkt.
3. Klicken Sie auf die Schaltfläche „Anomaly Detection“ (Anomalieerkennung), und überprüfen Sie das Ergebnis im rechten Textfeld „Response“ (Antwort).

**Szenario 2: Erkennen der Daten der Zeitreihe ohne Zeitraum**
1. Lassen Sie den Zeitraum im Feld „Period“ (Zeitraum) leer, und tun Sie so, als ob Sie den Zeitraum der Zeitreihe nicht kennen.
2. Verwenden Sie die gleichen Zeitreihendaten wie in Szenario 1.
3. Klicken Sie auf die Schaltfläche „Anomaly Detection“ (Anomalieerkennung), und überprüfen Sie das Feld „Period“ (Zeitraum) im rechten Textfeld „Response“ (Antwort).

<a name="Step3"></a>
### <a name="read-the-result"></a>Lesen des Ergebnisses

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Überprüfen und Lernen

Jetzt erhalten Sie eine laufende Anwendung. Sehen wir uns an, wie sich die Beispiel-App in die Cognitive Services-Technologie integriert. Dieser Schritt macht es Ihnen leichter, entweder mit der Weiterentwicklung dieser App fortzufahren oder eine eigene App mithilfe der Microsoft-Anomalieerkennung zu entwickeln.
Diese Beispiel-App nutzt den Endpunkt der Restful-API zur Anomalieerkennung.
Sehen wir uns zu Überprüfung, wie die Restful-API in der Beispielanwendung verwendet wird, einen Codeausschnitt aus „anomalydetection.html“ an.
```JavaScript
function anomalyDetection(url, subscriptionKey, points, period) {
    var obj = new Object();
    obj.Points = JSON.parse(points); // this points are read from text box.
    obj.Period = parseInt(period);//period=7 weekly period
    var tsData = JSON.stringify(obj);
    // Perform the REST API call.
    $.ajax({
        url: url, //Anomaly Detection API endpoint
        // Request headers.
        beforeSend: function (xhrObj) {
            xhrObj.setRequestHeader("Content-Type", "application/json");
            xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey); // Replace your subscription key
        },
        type: "POST",
        // Request body.
        data: tsData, // json format
        })
        .done(function (data) {
            // Show formatted JSON on webpage.
            $("#responseTextArea").val(JSON.stringify(data, null, 2));
        })
        .fail(function (jqXHR, textStatus, errorThrown) {
            // Display error message.
            var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
            $("#responseTextArea").val(errorString);           
        });
}

```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)

---
title: 'Schnellstart: Erkennen von Emotionen auf Gesichtern in einem Bild – Emotionen-API, Java'
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der Emotionen-API mit Java für Android.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: a3250771c51495890c54060dc46b8bd88a8f54af
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228677"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Schnellstart: Erstellen einer App zur Erkennung von Emotionen auf Gesichtern in einem Bild

> [!IMPORTANT]
> Die Emotionen-API wird am 15. Februar 2019 eingestellt. Die Funktion zur Erkennung von Emotionen ist jetzt als Teil der [Gesichtserkennungs-API](https://docs.microsoft.com/azure/cognitive-services/face/) allgemein verfügbar. 

Dieser Artikel enthält Informationen und ein Codebeispiel für den schnellen Einstieg in die Verwendung der [Emotion Recognize-Methode](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) in der Android-Clientbibliothek der Emotionen-API. Das Beispiel veranschaulicht, wie Sie Java zum Erkennen der von Personen ausgedrückten Emotionen verwenden können.

## <a name="prerequisites"></a>Voraussetzungen
* Rufen Sie das Emotionen-API Java for Android-SDK [hier](https://github.com/Microsoft/Cognitive-emotion-android) ab.
* Rufen Sie [hier](https://azure.microsoft.com/try/cognitive-services/) Ihren kostenlosen Abonnementschlüssel ab.

## <a name="recognize-emotions-java-for-android-example-request"></a>Java for Android-Beispielanforderung zur Emotionserkennung

```java
// // This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;

public class Main
{
    public static void main(String[] args)
    {
        HttpClient httpClient = new DefaultHttpClient();

        try
        {
            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
            //   URL below with "westcentralus".
            URIBuilder uriBuilder = new URIBuilder("https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize");

            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers. Replace the example key below with your valid subscription key.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

            // Request body. Replace the example URL below with the URL of the image you want to analyze.
            StringEntity reqEntity = new StringEntity("{ \"url\": \"http://example.com/images/test.jpg\" }");
            request.setEntity(reqEntity);

            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                System.out.println(EntityUtils.toString(entity));
            }
        }
        catch (Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Beispielantwort der Emotionserkennung
Ein erfolgreicher Aufruf gibt ein Array von Gesichtseinträgen zusammen mit den ihnen zugeordneten Emotionsbewertungen in der Rangfolge der Rechteckgröße der Gesichter in absteigender Reihenfolge zurück. Eine leere Antwort zeigt an, dass keine Gesichter erkannt wurden. Ein Emotionseintrag enthält die folgenden Felder:
* faceRectangle: Rechteckposition des Gesichts auf dem Bild
* scores: Emotionsbewertungen für jedes Gesicht auf dem Bild

```json
application/json
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]

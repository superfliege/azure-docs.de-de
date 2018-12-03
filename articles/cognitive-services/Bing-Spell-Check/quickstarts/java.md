---
title: 'Schnellstart: Bing-Rechtschreibprüfungs-API, Java'
titlesuffix: Azure Cognitive Services
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der Bing-Rechtschreibprüfungs-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: 90963f90d4526a6495ec9de73e10f2cd792841e0
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52306929"
---
# <a name="quickstart-for-bing-spell-check-api-with-java"></a>Schnellstart für die Bing-Rechtschreibprüfungs-API mit Java 

In diesem Artikel erfahren Sie, wie Sie die [Bing-Rechtschreibprüfungs-API](https://azure.microsoft.com/services/cognitive-services/spell-check/)  mit Java verwenden. Die Rechtschreibprüfungs-API gibt eine Liste mit nicht erkannten Wörtern und Ersetzungsvorschlägen zurück. Normalerweise übergeben Sie Text an die API, und anschließend nehmen Sie entweder die vorgeschlagenen Ersetzungen im Text vor, oder Sie zeigen sie für den Benutzer Ihrer Anwendung an, damit er selbst entscheiden kann, ob er die Ersetzungen vornehmen möchte. In diesem Artikel wird erläutert, wie Sie eine Anforderung mit dem Text „Hollo, wrld!“ senden. Die vorgeschlagenen Ersetzungen sind „Hello“ und „world“.

## <a name="prerequisites"></a>Voraussetzungen

Zum Kompilieren und Ausführen des Codes benötigen Sie [JDK 7 oder 8](https://aka.ms/azure-jdks). Sie können auch eine Java-Entwicklungsumgebung verwenden, ein Text-Editor ist jedoch ausreichend.

Sie benötigen ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit der **Bing-Rechtschreibprüfungs-API v7**. Die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/#lang) ist für diesen Schnellstart ausreichend. Sie benötigen den Zugriffsschlüssel, den Sie beim Aktivieren Ihrer kostenlosen Testversion erhalten, oder Sie können den Schlüssel eines kostenpflichtigen Abonnements von Ihrem Azure-Dashboard verwenden.  Siehe auch [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="get-spell-check-results"></a>Abrufen der Ergebnisse der Rechtschreibprüfung

1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Java-Projekt.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `subscriptionKey`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```java
import java.io.*;
import java.net.*;
import javax.net.ssl.HttpsURLConnection;

public class HelloWorld {

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    // NOTE: Replace this example key with a valid subscription key.
    static String key = "ENTER KEY HERE";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";

    public static void check () throws Exception {
        String params = "?mkt=" + mkt + "&mode=" + mode;
        URL url = new URL(host + path + params);
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        connection.setRequestProperty("Content-Length", "" + text.length() + 5);
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();

        BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
        String line;
        while ((line = in.readLine()) != null) {
            System.out.println(line);
        }
        in.close();
    }

    public static void main(String[] args) {
        try {
            check ();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

**Antwort**

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial zur Bing-Rechtschreibprüfung](../tutorials/spellcheck.md)

## <a name="see-also"></a>Weitere Informationen

- [Übersicht über die Bing-Rechtschreibprüfung](../proof-text.md)
- [Referenz zur Bing-Rechtschreibprüfungs-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)

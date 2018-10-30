---
title: 'Schnellstart: Veröffentlichen einer Wissensdatenbank – REST, Java – QnA Maker'
titleSuffix: Azure Cognitive Services
description: In dieser REST-basierten Schnellstartanleitung wird das Veröffentlichen Ihrer Wissensdatenbank beschrieben, mit der die neueste Version der getesteten Wissensdatenbank per Pushvorgang in einen dedizierten Azure Search-Index übertragen wird, der die veröffentlichte Wissensdatenbank darstellt. Außerdem wird ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: 83390ace8d9747d218fdd5c6b50ba9bdc6d11957
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648818"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-java"></a>Schnellstart: Veröffentlichen einer Wissensdatenbank in QnA Maker mit Java

In dieser Schnellstartanleitung wird das programmgesteuerte Veröffentlichen Ihrer Wissensdatenbank (Knowledge Base, KB) Schritt für Schritt beschrieben. Bei der Veröffentlichung wird die aktuelle Version der Wissensdatenbank per Pushvorgang an einen dedizierten Azure Search-Index übertragen und ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.

In dieser Schnellstartanleitung werden QnA Maker-APIs aufgerufen:
* [Veröffentlichen](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): Für diese API sind keine Informationen im Text der Anforderung erforderlich.

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-java-repo-note.md)]

1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Java-Projekt.
2. Fügen Sie den unten stehenden Code hinzu.
3. Ersetzen Sie den `key`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
4. Führen Sie das Programm aus.

```java
import java.io.*;
import java.lang.reflect.Type;
import java.net.*;
import java.util.*;
import javax.net.ssl.HttpsURLConnection;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *    <dependency>
 *      <groupId>com.google.code.gson</groupId>
 *      <artifactId>gson</artifactId>
 *      <version>2.8.1</version>
 *    </dependency>
 */
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.google.gson.reflect.TypeToken;

/* NOTE: To compile and run this code:
1. Save this file as PublishKB.java.
2. Run:
    javac PublishKB.java -cp .;gson-2.8.1.jar -encoding UTF-8
3. Run:
    java -cp .;gson-2.8.1.jar PublishKB
*/

public class PublishKB {

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
    static String subscriptionKey = "ENTER KEY HERE";

// Replace this with a valid knowledge base ID.
    static String kb = "ENTER ID HERE";

    static String host = "https://westus.api.cognitive.microsoft.com";
    static String service = "/qnamaker/v4.0";
    static String method = "/knowledgebases/";

    public static String PrettyPrint (String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

// Send an HTTP POST request.
    public static String Post (URL url, String content) throws Exception {
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/json");
        connection.setRequestProperty("Content-Length", content.length() + "");
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        byte[] encoded_content = content.getBytes("UTF-8");
        wr.write(encoded_content, 0, encoded_content.length);
        wr.flush();
        wr.close();

        if (connection.getResponseCode() == 204)
        {
            return "{'result' : 'Success.'}";
        }
        else {
            StringBuilder response = new StringBuilder ();
            BufferedReader in = new BufferedReader(new InputStreamReader(connection.getErrorStream(), "UTF-8"));

            String line;
            while ((line = in.readLine()) != null) {
                response.append(line);
            }
            in.close();

            return response.toString();
        }
    }

// Sends the request to publish the knowledge base.
    public static String PublishKB (String kb) throws Exception {
        URL url = new URL(host + service + method + kb);
        System.out.println ("Calling " + url.toString() + ".");
        return Post(url, "");
    }

    public static void main(String[] args) {
        try {
            String response = PublishKB (kb);
            System.out.println (PrettyPrint (response));
        }
        catch (Exception e) {
            System.out.println (e.getCause().getMessage());
        }
    }
}
```

## <a name="the-publish-a-knowledge-base-response"></a>Antwort auf das Veröffentlichen einer Wissensdatenbank

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
{
  "result": "Success."
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
---
title: 'Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und Java'
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie ein Bild in die API für die visuelle Bing-Suche hochladen und Erkenntnisse dazu gewinnen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 79b118c0a4fd28eacf24dc63f04f36314807b41a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57531087"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und Java

Verwenden Sie diese Schnellstartanleitung, um die API für die visuelle Bing-Suche zum ersten Mal aufzurufen und die Suchergebnisse anzuzeigen. Mit dieser einfachen C#-Anwendung wird ein Bild in die API hochgeladen, und es werden die zurückgegebenen Informationen angezeigt. Diese Anwendung ist zwar in Java geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

Beim Hochladen eines lokalen Bilds müssen die Formulardaten den Content-Disposition-Header enthalten. Der `name`-Parameter muss auf „image“ festgelegt werden. Der `filename`-Parameter kann auf eine beliebige Zeichenfolge festgelegt werden. Der Inhalt des Formulars sind die Binärdaten des Bilds. Sie können eine maximale Bildgröße von 1 MB hochladen.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Voraussetzungen

* Das [Java Development Kit (JDK) 7 oder 8](https://aka.ms/azure-jdks)
* Die [Gson-Bibliothek](https://github.com/google/gson)
* [Apache HttpComponents](https://hc.apache.org/downloads.cgi)


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Erstellen und Initialisieren eines Projekts

1. Erstellen Sie in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Editor ein neues Java-Projekt, und importieren Sie die folgenden Bibliotheken.

    ```java
    import java.util.*;
    import java.io.*;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    // HttpClient libraries
    
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.ContentType;
    import org.apache.http.entity.mime.MultipartEntityBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    ```

2. Erstellen Sie Variablen für Ihren API-Endpunkt, den Abonnementschlüssel und den Pfad zu Ihrem Bild. 

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

## <a name="create-the-json-parser"></a>Erstellen des JSON-Parsers

Erstellen Sie eine Methode, um die JSON-Antwort von der API mithilfe von `JsonParser` besser lesbar zu machen.

    ```java
    public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## <a name="construct-the-search-request-and-query"></a>Erstellen der Suchanforderung und -abfrage

1. Erstellen Sie in der Main-Methode Ihrer Anwendung einen HTTP-Client mit `HttpClientBuilder.create().build();`.

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Erstellen Sie eine `HttpEntity` zum Hochladen des Bilds in die API.

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Erstellen Sie ein `httpPost`-Objekt mit Ihrem Endpunkt, und legen Sie den Header auf die Verwendung Ihres Abonnementschlüssels fest.

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>Empfangen und Verarbeiten der JSON-Antwort

1. Verwenden Sie `HttpClient.execute()` zum Senden einer Anforderung an die API, und speichern Sie die Antwort in einem `InputStream`-Objekt.
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. Speichern Sie die JSON-Zeichenfolge, und geben Sie die Antwort aus.

```java
String json = new Scanner(stream).useDelimiter("\\A").next();
System.out.println("\nJSON Response:\n");
System.out.println(prettify(json));
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Web-App für die benutzerdefinierte Suche](../tutorial-bing-visual-search-single-page-app.md)

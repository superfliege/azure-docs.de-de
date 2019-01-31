---
title: 'Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und C#'
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie ein Bild in die API für die visuelle Bing-Suche hochladen und Erkenntnisse dazu gewinnen.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 2ceef9391e6476ac184989b0411428203abefe14
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173342"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-c"></a>Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und C#

Verwenden Sie diese Schnellstartanleitung, um die API für die visuelle Bing-Suche zum ersten Mal aufzurufen und die Suchergebnisse anzuzeigen. Mit dieser einfachen C#-Anwendung wird ein Bild in die API hochgeladen, und es werden die zurückgegebenen Informationen angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

* Eine beliebige [Visual Studio 2017](https://www.visualstudio.com/downloads/)-Edition.
* Das [Json.NET](https://www.newtonsoft.com/json)-Framework, das als NuGet-Paket verfügbar ist
* Unter Linux/macOS kann diese Anwendung mit [Mono](http://www.mono-project.com/) ausgeführt werden

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Erstellen und Initialisieren eines Projekts

1. Erstellen Sie eine neue Konsolenlösung in Visual Studio mit der Bezeichnung `BingSearchApisQuickStart`. Fügen Sie dann die folgenden Namespaces in die Hauptcodedatei ein.

    ```csharp
    using System;
    using System.Text;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    ```

2. Fügen Sie Variablen für Ihren Abonnementschlüssel, den Endpunkt und den Pfad für das hochzuladende Bild hinzu.

    ```csharp
        const string accessKey = "<yoursubscriptionkeygoeshere>";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
        static string imagePath = @"<pathtoimagegoeshere>";
    ```


1. Erstellen Sie eine Methode namens `GetImageFileName()`, um den Pfad für das Bild abzurufen.
    
    ```csharp
    static string GetImageFileName(string path)
            {
                return new FileInfo(path).Name;
            }
    ```

2. Erstellen Sie eine Methode, um die Binärzeichen des Bilds abzurufen.

    ```csharp
    static byte[] GetImageBinary(string path)
    {
        return File.ReadAllBytes(path);
    }
    ```

## <a name="build-the-form-data"></a>Erstellen der Formulardaten

Beim Hochladen eines lokalen Bilds müssen die Formulardaten, die an die API gesendet werden, ordnungsgemäß formatiert sein. Sie müssen den Header „Content-Disposition“ enthalten, und der `name`-Parameter muss auf „image“ festgelegt werden. Der `filename`-Parameter kann auf eine beliebige Zeichenfolge festgelegt werden. Der Inhalt des Formulars umfasst die Binärdaten des Bilds. Sie können eine maximale Bildgröße von 1 MB hochladen.

    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

1. Fügen Sie zum richtigen Formatieren der POST-Formulardaten Begrenzungszeichenfolgen hinzu, mit denen Zeichen für Anfang, Ende und Zeilenumbruch für die Daten bestimmt werden.

    ```csharp
    // Boundary strings for form data in body of POST.
    const string CRLF = "\r\n";
    static string BoundaryTemplate = "batch_{0}";
    static string StartBoundaryTemplate = "--{0}";
    static string EndBoundaryTemplate = "--{0}--";
    ```

2. Mit den folgenden Variablen werden Parameter zu den Formulardaten hinzugefügt. 

    ```csharp
    const string CONTENT_TYPE_HEADER_PARAMS = "multipart/form-data; boundary={0}";
    const string POST_BODY_DISPOSITION_HEADER = "Content-Disposition: form-data; name=\"image\"; filename=\"{0}\"" + CRLF +CRLF;
    ```

3. Erstellen Sie eine Funktion namens `BuildFormDataStart()`, um den Anfangsteil der erforderlichen Formulardaten zu erstellen. Verwenden Sie dazu die Begrenzungszeichenfolgen und den Bildpfad.
    
    ```csharp
        static string BuildFormDataStart(string boundary, string filename)
        {
            var startBoundary = string.Format(StartBoundaryTemplate, boundary);

            var requestBody = startBoundary + CRLF;
            requestBody += string.Format(POST_BODY_DISPOSITION_HEADER, filename);

            return requestBody;
        }
    ```

4. Erstellen Sie eine Funktion namens `BuildFormDataEnd()`, um den Endteil der erforderlichen Formulardaten zu erstellen. Verwenden Sie dazu die Begrenzungszeichenfolgen.
    
    ```csharp
        static string BuildFormDataEnd(string boundary)
        {
            return CRLF + CRLF + string.Format(EndBoundaryTemplate, boundary) + CRLF;
        }
    ```

## <a name="call-the-bing-visual-search-api"></a>Aufrufen der API für die visuelle Bing-Suche

1. Erstellen Sie eine Funktion zum Aufrufen des Endpunkts für die visuelle Bing-Suche und Zurückgeben der JSON-Antwort. Die Funktion sollte den Anfangs- und Endteil der Formulardaten, ein Bytearray mit den Bilddaten und einen ContentType-Wert umfassen.

2. Verwenden Sie ein `WebRequest`-Objekt zum Speichern von URI, ContentType-Wert und Headern.  

3. Verwenden Sie `request.GetRequestStream()` zum Schreiben der Formular- und Bilddaten. Rufen Sie dann die Antwort ab. Diese Funktion sollte dem folgenden Code ähneln:
        
    ```csharp
        static string BingImageSearch(string startFormData, string endFormData, byte[] image, string contentTypeValue)
        {
            WebRequest request = HttpWebRequest.Create(uriBase);
            request.ContentType = contentTypeValue;
            request.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            request.Method = "POST";

            // Writes the boundary and Content-Disposition header, then writes
            // the image binary, and finishes by writing the closing boundary.
            using (Stream requestStream = request.GetRequestStream())
            {
                StreamWriter writer = new StreamWriter(requestStream);
                writer.Write(startFormData);
                writer.Flush();
                requestStream.Write(image, 0, image.Length);
                writer.Write(endFormData);
                writer.Flush();
                writer.Close();
            }

            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            return json;
        }
    ```

## <a name="create-the-main-method"></a>Erstellen der Main-Methode

1. Rufen Sie in der Main-Methode Ihrer Anwendung den Dateinamen und die Bildbinärdatei für das Bild ab. 

    ```csharp
    var filename = GetImageFileName(imagePath);
    var imageBinary = GetImageBinary(imagePath);
    ```

2. Richten Sie den POST-Text ein, indem Sie die entsprechende Begrenzung formatieren. Rufen Sie dann `startFormData()` und `endFormData` auf, um die Formulardaten zu erstellen. 

    ```csharp
    // Set up POST body.
    var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
    var startFormData = BuildFormDataStart(boundary, filename);
    var endFormData = BuildFormDataEnd(boundary);
    ```

3. Erstellen Sie den ContentType-Wert, indem Sie `CONTENT_TYPE_HEADER_PARAMS` und die Begrenzung für Formulardaten formatieren.

    ```csharp
    var contentTypeHdrValue = string.Format(CONTENT_TYPE_HEADER_PARAMS, boundary);
    ```

4. Rufen Sie `BingImageSearch()` auf, um die API-Antwort abzurufen. Dann geben Sie die Antwort aus.

    ```csharp
    var json = BingImageSearch(startFormData, endFormData, imageBinary, contentTypeHdrValue);
    Console.WriteLine(json);
    Console.WriteLine("enter any key to continue");
    Console.readKey();
    ```

## <a name="using-httpclient"></a>Verwenden von HTTPClient

Wenn Sie HttpClient verwenden, können Sie mit MultipartFormDataContent die Formulardaten erstellen. Verwenden Sie die folgenden Codeausschnitte, um die gleichen benannten Methoden wie im vorherigen Beispiel zu ersetzen.

Ersetzen Sie die Main-Methode durch den folgenden Code:

```csharp
        static void Main()
        {
            try
            {
                Console.OutputEncoding = System.Text.Encoding.UTF8;

                if (accessKey.Length == 32)
                {
                    if (IsImagePathSet(imagePath))
                    {
                        var filename = GetImageFileName(imagePath);
                        Console.WriteLine("Getting image insights for image: " + filename);
                        var imageBinary = GetImageBinary(imagePath);

                        var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
                        var json = BingImageSearch(imageBinary, boundary, uriBase, accessKey);

                        Console.WriteLine("\nJSON Response:\n");
                        Console.WriteLine(JsonPrettyPrint(json));
                    }
                }
                else
                {
                    Console.WriteLine("Invalid Bing Visual Search API subscription key!");
                    Console.WriteLine("Please paste yours into the source code.");
                }

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }
        }
```

Ersetzen Sie die BingImageSearch-Methode durch den folgenden Code:

```csharp
        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response.
        /// </summary>
        static string BingImageSearch(byte[] image, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new ByteArrayContent(image), "image", "myimage");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }

            return json;
        }
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Web-App für die benutzerdefinierte Suche](../tutorial-bing-visual-search-single-page-app.md)

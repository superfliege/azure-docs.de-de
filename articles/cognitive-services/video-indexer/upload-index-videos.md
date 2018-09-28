---
title: 'Beispiel: Hochladen und Indizieren von Videos mit Video Indexer'
titlesuffix: Azure Cognitive Services
description: In diesem Thema wird veranschaulicht, wie Sie APIs zum Hochladen und Indizieren Ihrer Videos mit Video Indexer verwenden.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: sample
ms.date: 09/15/2018
ms.author: juliako
ms.openlocfilehash: e84411535b82b3e4861b529f490bdde0eb25fd42
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983884"
---
# <a name="example-upload-and-index-your-videos"></a>Beispiel: Hochladen und Indizieren Ihrer Videos  

In diesem Artikel wird gezeigt, wie Sie ein Video mit Azure Video Indexer hochladen. Die Video Indexer-API bietet zwei Optionen für das Hochladen: 

* Hochladen des Videos von einer URL (bevorzugt)
* Senden der Videodatei als Bytearray im Hauptteil der Anforderung

In diesem Artikel wird veranschaulicht, wie Sie die API zum [Hochladen eines Videos ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) verwenden, um Ihre Videos über eine URL hochzuladen und zu indizieren. Das Codebeispiel in diesem Artikel schließt den auskommentierten Code ein, der zeigt, wie Sie das Bytearray hochladen.  

Außerdem werden in diesem Artikel einige Parameter beschrieben, die Sie für die API festlegen können, um den Prozess und die Ausgabe der API zu ändern.

> [!Note]
> Beim Erstellen eines Video Indexer-Kontos können Sie ein kostenloses Testkonto (mit einer bestimmten Anzahl von kostenlosen Indizierungsminuten) oder eine kostenpflichtige Option wählen (ohne Einschränkung durch eine Kontingentvorgabe). <br/>Bei der kostenlosen Testversion stellt Video Indexer bis zu 600 Minuten an kostenloser Indizierungszeit für Websitebenutzer und bis zu 2.400 Minuten an kostenloser Indizierungszeit für API-Benutzer bereit. Bei der kostenpflichtigen Option erstellen Sie ein Video Indexer-Konto, das [mit Ihrem Azure-Abonnement und einem Azure Media Services-Konto verbunden ist](connect-to-azure.md). Sie bezahlen für die Minuten der Indizierungszeit und die Gebühren für das Media Services-Konto. 

## <a name="uploading-considerations"></a>Überlegungen zum Hochladen
    
- Wenn Sie das Video über eine URL hochladen (bevorzugt), muss der Endpunkt mit TLS 1.2 (oder höher) gesichert werden.
- Die Option für das Bytearray ist auf 2 GB bei einem Zeitlimit von 30 Minuten beschränkt.
- Die im Parameter `videoURL` angegebene URL muss codiert sein.

## <a name="configurations-and-params"></a>Konfigurationen und Parameter

In diesem Abschnitt werden einige optionale Parameter und ihre Anwendung beschrieben.

### <a name="externalid"></a>externalID 

Dieser Parameter ermöglicht Ihnen das Angeben einer ID, die dem Video zugeordnet wird. Die ID kann auf die externe Systemintegration für die Verwaltung der Videoinhalte (Video Content Management, VCM) angewendet werden. Die Videos, die im Video Indexer-Portal enthalten sind, können anhand der angegebenen externen ID durchsucht werden.

### <a name="indexingpreset"></a>indexingPreset

Verwenden Sie diesen Parameter, wenn unformatierte oder externe Aufzeichnungen Hintergrundgeräusche enthalten. Dieser Parameter wird verwendet, um den Indizierungsprozess zu konfigurieren. Sie können die folgenden Werte angeben:

- `Default`: Indizieren und Extrahieren von Erkenntnissen für Audio- und Videodaten
- `AudioOnly`: Indizieren und Extrahieren von Erkenntnissen ausschließlich für Audiodaten (Videodaten werden ignoriert)
- `DefaultWithNoiseReduction`: Indizieren und Extrahieren von Erkenntnissen aus Audio- und Videodaten mit Anwendung von Algorithmen für die Rauschunterdrückung auf den Audiodatenstrom

Der Preis richtet sich nach der gewählten Indizierungsoption.  

### <a name="callbackurl"></a>callbackUrl

Eine POST-URL für die Benachrichtigung, wenn die Indizierung abgeschlossen ist. Video Indexer fügt zwei Abfragezeichenfolgenparameter hinzu: „id“ und „state“. Wenn die Rückruf-URL beispielsweise „https://test.com/notifyme?projectName=MyProject“ lautet, wird die Benachrichtigung mit zusätzlichen Parametern an „https://test.com/notifyme?projectName=MyProject&id=1234abcd&state=Processed“ gesendet.

Sie können der URL auch weitere Parameter hinzufügen, bevor Sie den Aufruf per POST-Vorgang an Video Indexer senden. Diese Parameter werden in den Rückruf einbezogen. Später im Code können Sie die Abfragezeichenfolge analysieren und alle angegebenen Parameter in der Abfragezeichenfolge zurückerhalten (Daten, die Sie ursprünglich an die URL angefügt haben, sowie die von Video Indexer bereitgestellten Informationen). Die URL muss codiert sein.

### <a name="streamingpreset"></a>streamingPreset

Nachdem Ihr Video hochgeladen wurde, kann das Video von Video Indexer optional codiert werden. Anschließend wird der Vorgang mit dem Indizieren und Analysieren des Videos fortgesetzt. Nachdem Video Indexer die Analyse abgeschlossen hat, erhalten Sie eine Benachrichtigung mit der Video-ID.  

Bei Verwendung der [Upload video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?)- oder [Re-Index Video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?)-API lautet einer der optionalen Parameter `streamingPreset`. Wenn Sie `streamingPreset` auf `Default`, `SingleBitrate` oder `AdaptiveBitrate` festlegen, wird der Codierungsprozess ausgelöst. Wenn die Indizierung und Codierung von Aufträgen abgeschlossen ist, wird das Video veröffentlicht, damit Sie Ihr Video auch streamen können. Der Streamingendpunkt, von dem aus Sie das Video streamen möchten, muss sich im Status **Wird ausgeführt** befinden.

Zum Ausführen der Indizierung und Codierung von Aufträgen sind für das [Azure Media Services-Konto, das mit Ihrem Video Indexer-Konto verbunden ist](connect-to-azure.md), reservierte Einheiten (Reserved Units, RUs) erforderlich. Weitere Informationen finden Sie unter [Übersicht über das Skalieren der Medienverarbeitung](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview). Da es sich hierbei um rechenintensive Aufträge handelt, wird dringend die Verwendung des Einheitentyps S3 empfohlen. Die Anzahl von RUs definiert die maximale Anzahl von Aufträgen, die parallel ausgeführt werden können. Die Baseline-Empfehlung lautet: zehn RUs vom Typ S3. 

Wenn Sie Ihr Video nicht codieren, sondern nur indizieren möchten, können Sie `streamingPreset`auf `NoStreaming` festlegen.

### <a name="videourl"></a>videoUrl

Eine URL der zu indizierenden Video-/Audiodatei. Die URL muss auf eine Mediendatei zeigen (HTML-Seiten werden nicht unterstützt). Die Datei kann durch ein Zugriffstoken als Teil des URI geschützt werden, und der Endpunkt für die Datei muss mit TLS 1.2 oder höher gesichert werden. Die URL muss codiert sein. 

Wenn `videoUrl` nicht angegeben ist, erwartet Video Indexer, dass Sie die Datei als „multipart/form“ im Hauptteil übergeben.

## <a name="code-sample"></a>Codebeispiel

Mit dem folgenden C#-Codeausschnitt wird die Nutzung aller Video Indexer-APIs zusammen veranschaulicht.

```csharp
public async Task Sample()
{
    var apiUrl = "https://api.videoindexer.ai";
    var location = "westus2";
    var apiKey = "...";

    System.Net.ServicePointManager.SecurityProtocol =
        System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

    // create the http client
    var handler = new HttpClientHandler();
    handler.AllowAutoRedirect = false;
    var client = new HttpClient(handler);
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);

    // obtain account information and access token
    string queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"generateAccessTokens", "true"},
            {"allowEdit", "true"},
        });
    HttpResponseMessage result = await client.GetAsync($"{apiUrl}/auth/trial/Accounts?{queryParams}");
    var json = await result.Content.ReadAsStringAsync();
    var accounts = JsonConvert.DeserializeObject<AccountContractSlim[]>(json);
    // take the relevant account, here we simply take the first
    var accountInfo = accounts.First();

    // we will use the access token from here on, no need for the apim key
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // upload a video
    var content = new MultipartFormDataContent();
    Debug.WriteLine("Uploading...");
    // get the video from URL
    var videoUrl = "VIDEO_URL"; // replace with the video URL

    // as an alternative to specifying video URL, you can upload a file.
    // remove the videoUrl parameter from the query params below and add the following lines:
    //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
    //byte[] buffer =newbyte[video.Length];
    //video.Read(buffer, 0, buffer.Length);
    //content.Add(newByteArrayContent(buffer));

    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"name", "video_name"},
            {"description", "video_description"},
            {"privacy", "private"},
            {"partition", "partition"},
            {"videoUrl", videoUrl},
        });
    var uploadRequestResult = await client.PostAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos?{queryParams}", content);
    var uploadResult = await uploadRequestResult.Content.ReadAsStringAsync();

    // get the video id from the upload result
    string videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
    Debug.WriteLine("Uploaded");
    Debug.WriteLine("Video ID:");
    Debug.WriteLine(videoId);

    // wait for the video index to finish
    while (true)
    {
        await Task.Delay(10000);

        queryParams = CreateQueryString(
            new Dictionary<string, string>()
            {
                {"accessToken", accountInfo.AccessToken},
                {"language", "English"},
            });

        var videoGetIndexRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/Index?{queryParams}");
        var videoGetIndexResult = await videoGetIndexRequestResult.Content.ReadAsStringAsync();

        string processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

        Debug.WriteLine("");
        Debug.WriteLine("State:");
        Debug.WriteLine(processingState);

        // job is finished
        if (processingState != "Uploaded" && processingState != "Processing")
        {
            Debug.WriteLine("");
            Debug.WriteLine("Full JSON:");
            Debug.WriteLine(videoGetIndexResult);
            break;
        }
    }

    // search for the video
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"id", videoId},
        });

    var searchRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/Search?{queryParams}");
    var searchResult = await searchRequestResult.Content.ReadAsStringAsync();
    Debug.WriteLine("");
    Debug.WriteLine("Search:");
    Debug.WriteLine(searchResult);

    // Generate video access token (used for get widget calls)
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var videoTokenRequestResult = await client.GetAsync($"{apiUrl}/auth/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/AccessToken?allowEdit=true");
    var videoAccessToken = (await videoTokenRequestResult.Content.ReadAsStringAsync()).Replace("\"", "");
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // get insights widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
            {"widgetType", "Keywords"},
            {"allowEdit", "true"},
        });
    var insightsWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/InsightsWidget?{queryParams}");
    var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
    Debug.WriteLine("Insights Widget url:");
    Debug.WriteLine(insightsWidgetLink);

    // get player widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
        });
    var playerWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/PlayerWidget?{queryParams}");
    var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
    Debug.WriteLine("");
    Debug.WriteLine("Player Widget url:");
    Debug.WriteLine(playerWidgetLink);
}

private string CreateQueryString(IDictionary<string, string> parameters)
{
    var queryParameters = HttpUtility.ParseQueryString(string.Empty);
    foreach (var parameter in parameters)
    {
        queryParameters[parameter.Key] = parameter.Value;
    }

    return queryParameters.ToString();
}

public class AccountContractSlim
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Location { get; set; }
    public string AccountType { get; set; }
    public string Url { get; set; }
    public string AccessToken { get; set; }
}
```



## <a name="next-steps"></a>Nächste Schritte

[Untersuchen der von der v2-API erstellten Azure Video Indexer-Ausgabe](video-indexer-output-json-v2.md)

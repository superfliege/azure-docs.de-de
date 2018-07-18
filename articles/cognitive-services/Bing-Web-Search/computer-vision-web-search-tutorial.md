---
title: 'Tutorial: Mobile App für die visuelle Suche'
description: Open Source-C#-Anwendung, die mithilfe der Maschinelles Sehen-API von Cognitive Services, der Bing-Websuche-API und dem plattformübergreifenden Framework Xamarin.Forms die visuelle Suche implementiert
services: bing-web-search, computer-vision
author: Aristoddle
manager: bking
ms.service: cognitive-services
ms.devlang: csharp
ms.topic: article
ms.date: 06/22/2017
ms.author: t-jolan
ms.openlocfilehash: 54dabaeaad2e49ba7cc138636054bc3dfa4b8379
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374250"
---
# <a name="visual-search-mobile-app-tutorial"></a>Tutorial: Mobile App für die visuelle Suche

## <a name="introduction"></a>Einführung  
Dieses Tutorial behandelt die Endpunkte der [Maschinelles Sehen-API](https://azure.microsoft.com/services/cognitive-services/computer-vision/) und der [Bing-Websuche-API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) und veranschaulicht, wie mit diesen eine Basisanwendung für die visuelle Suche mit [Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/) erstellt werden kann.  Dieses Tutorial deckt die folgenden Themen ab: 

> [!div class="checklist"]
> * Einrichten von Visual Studio, um Xamarin.Forms-Anwendungen zu entwickeln
> * Erfassen und Importieren von Bilddaten mithilfe des [Media-Plug-Ins für Xamarin](https://github.com/jamesmontemagno/MediaPlugin)
> * Analysieren von Text aus einem Bild mithilfe der Maschinelles Sehen-API
> * Senden von Suchanforderungen an die Bing-Websuche-API
> * Analysieren von JSON-Antworten aus beiden APIs mit [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) (mit LINQ und Modellobjektdeserialisierung)
> * Erstellen einer plattformübergreifenden Xamarin.Forms-Benutzeroberfläche für die visuelle Suche 

## <a name="prerequisites"></a>Voraussetzungen

Dieses Beispiel wurde mit Xamarin.Forms in [Visual Studio 2017](https://www.visualstudio.com/downloads/) entwickelt. Sie können die Community Edition von Visual Studio 2017 verwenden. Weitere Informationen dazu, wie Sie Xamarin mit Visual Studio verwenden, finden Sie in der [Xamarin-Dokumentation](https://developer.xamarin.com/guides/cross-platform/getting_started/).

In diesem Beispiel werden die folgenden NuGet-Pakete verwendet:

> [!div class="checklist"]
> * [Media-Plug-In für Xamarin](https://github.com/jamesmontemagno/MediaPlugin)
> * [Json.NET](https://github.com/JamesNK/Newtonsoft.Json)

Die Anwendung verwendet die folgenden Cognitive Services-APIs:

> [!div class="checklist"]
> * [Bing-Websuche-API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) 
> * [Maschinelles Sehen-API](https://azure.microsoft.com/services/cognitive-services/computer-vision/)

Unter [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/) erfahren Sie, wie Sie den Schlüssel für die 30-tägige Testversion für diese APIs erhalten. Weitere Informationen zu Schlüsseln für die kommerzielle Nutzung finden Sie unter [Preise](https://azure.microsoft.com/pricing/calculator/).

## <a name="setup-for-development"></a>Setup für die Entwicklung  

### <a name="installing-xamarin-on-windows"></a>Installieren von Xamarin unter Windows
Öffnen Sie den Visual Studio-Installer, wählen Sie das Hamburger-Menü aus, das mit der Visual Studio-Installation verknüpft ist, und wählen Sie **Ändern** aus. Dies gilt für jede installierte Version von Visual Studio 2017.  

![Visual Studio-Installer](./media/computer-vision-web-search-tutorial/visual-studio-installer.png) 

Scrollen Sie zu „Mobil und Gaming“ und aktivieren Sie **Mobile-Entwicklung mit .NET**, falls diese Option noch nicht aktiviert ist.

![Auswahl von Visual Studio-Installer mit Xamarin.Forms](./media/computer-vision-web-search-tutorial/xamarin-forms-is-enabled.png)

Klicken Sie abschließend unten rechts im Fenster auf **Ändern**, und warten Sie, bis Xamarin installiert wurde.

### <a name="installing-xamarin-on-macos"></a>Installieren von Xamarin unter macOS
Xamarin ist bereits mit Visual Studio für Mac vorkonfiguriert. So sollte keine Installation mehr erforderlich sein.

## <a name="building-and-running-the-app"></a>Erstellen und Ausführen der App

Eine Visual Studio-Lösungsdatei *(.sln)* für die Anwendung für die visuelle Suche können Sie unter [Anwendung für die visuelle Suche mit Cognitive Services](https://azure.microsoft.com/resources/samples/cognitive-services-xamarin-forms-computer-vision-search/) herunterladen. Sie können das ZIP-Archiv über den Webbrowser herunterladen, und dann auf Ihre Arbeitsstation aus GitHub klonen, oder mithilfe von Visual Studio herunterladen.

Öffnen Sie `VisualSearchApp.sln` in Visual Studio, um das Beispiel zu verwenden.  Möglicherweise dauert es einen Augenblick, bis die notwendigen Komponenten initialisiert wurden.

Für diese Anwendung sind zwei Drittanbieterbibliotheken erforderlich: **Json.NET** und **Media-Plug-Ins für Xamarin**. Sie können diese Bibliotheken rechts in Visual Studio mit dem NuGet-Paket-Manager installieren. Wählen Sie **Extras > NuGet-Paket-Manager > NuGet-Pakete für Projektmappe verwalten** aus, oder klicken Sie mit der rechten Maustaste im Projektmappen-Explorer in der Projektmappe auf die Lösung, und wählen Sie im Kontextmenü **NuGet-Pakete verwalten** aus.

Suchen Sie im NuGet-Fenster nach **Media-Plug-In für Xamarin** (Xam.Plugin.Media) Version 2.6.2 und **Json.NET** (Newtonsoft.Json) 10.0.3 und installieren Sie diese. Achten Sie darauf, dass Sie bei der Installation alle Projekte auswählen.

Um die Anwendung für alle verfügbaren Plattformen zu erstellen, drücken Sie **STRG + UMSCHALT + B**, oder klicken Sie im Menüband auf **Erstellen**, und wählen Sie **Projektmappe erstellen** aus.  Wie Sie einen Code für iOS unter Windows kompilieren und testen, erfahren Sie in [dieser Anleitung](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/).

Sie müssen Sie eine Zielkonfiguration, eine Zielplattform und ein Zielprojekt auswählen, bevor Sie die Anwendung ausführen.  Xamarin.Forms-Anwendungen kompilieren in nativen Code für Windows, Android und iOS. Die Screenshots in diesem Tutorial stammen aus der Windows-Version des Beispiels, doch die Funktionsweise ist in allen Versionen gleich. Die in dieser Anleitung verwendeten Bereitstellungseinstellungen werden hier angezeigt.  

![Visual Studio-Konfiguration für die Kompilation für ein Android-Smartphone](./media/computer-vision-web-search-tutorial/configuration-selection.png)

Nachdem der Buildprozess abgeschlossen wurde, und Sie eine Zielplattform ausgewählt haben, klicken Sie in der Symbolleiste auf die Schaltfläche **Starten**, oder drücken Sie **F5**. Visual Studio stellt Ihre Lösung für die Zielplattform bereit und startet diese.

Die Seite „Schlüssel hinzufügen“ wird angezeigt. (Diese Seite ist in `AddKeysPage.xaml` definiert.)  

![Bild von der Seite „Schlüssel hinzufügen“](./media/computer-vision-web-search-tutorial/add-keys-page.png)  

Fügen Sie hier Ihren Schlüssel für die Maschinelles Sehen-API und die Bing-Websuche-API ein. Für die Maschinelles Sehen-API müssen Sie außerdem den Server auswählen, auf dem der Endpunkt gehostet wird.

> [!TIP]
> Um diese Seite zu überspringen, geben Sie diese Informationen an den entsprechenden Stellen in `App.xaml.cs` ein. 

Die Anwendung überprüft Ihre Schlüssel mithilfe einer Testabfrage, und leitet Sie dann auf die Seite zur OCR-Auswahl weiter (definiert in `OcrSelectPage.xaml`).
   
![Bild von der Seite zur OCR-Auswahl](./media/computer-vision-web-search-tutorial/ocr-select-page.png)  

Oben im Bildschirm können Sie auswählen, ob der zu erkennende Text, gedruckt oder handgeschrieben ist.

> [!TIP]
> Halten Sie das Element, aus dem Text extrahiert werden soll, so flach wie möglich, und achten Sie darauf, dass es gleichmäßig und ohne Reflektionen beleuchtet wird. Die OCR für handschriftlichen Text eignet sich oft besser für Handschriften oder anderen „ausgefallene“ Schriftarten.

Klicken Sie dann auf **Foto aufnehmen** oder auf **Foto importieren**, um ein Foto mit der Kamera Ihres Geräts aufzunehmen, oder um ein auf Ihrem Gerät gespeichertes Foto auszuwählen.

Nachdem ein Foto gemacht oder ausgewählt wurde, wird das Bild an die Maschinelles Sehen-API übermittelt. Die Seite mit den gefundenen Wörtern (definiert in `OcrResultsPage.xaml`) zeigt alle Wörter an, die im Bild erkannt wurden.

![Bild von der Seite mit den OCR-Ergebnissen](./media/computer-vision-web-search-tutorial/ocr-results-page.png)  

> [!NOTE]
> Das Bild, das wir für diese Ergebnisse verwendet haben, befindet sich im Quellrepository als `SamplePhotos\TestImage.jpg`.

Wenn Sie ein Element auf der Seite mit den gefundenen Wörtern anklicken, erscheint die Seite „Webergebnisse“ (`WebResultsPage.xaml`), die die besten Bing-Ergebnisse für diese Suche anzeigt.

![Bild von der Seite „Webergebnisse“](./media/computer-vision-web-search-tutorial/web-results-page.png)  
Wählen Sie abschließend auf der Seite „Webergebnisse“ ein Element aus, um den Link in einer eingebetteten Webansicht zu öffnen. Sie können auch zurück navigieren, um ein anderes Ergebnis auszuwählen.

![Bild von der Webansicht](./media/computer-vision-web-search-tutorial/web-view-page.png)  

Sie können auf der Seite wie in einem beliebigen Webbrowser navigieren oder zurück zur Seite „Webergebnisse“ gehen. 

## <a name="review-and-learn"></a>Wiederholen und Anwenden

Nach dieser Einführung in die Anwendung für die visuelle Suche wollen wir uns genauer ansehen, wie die beiden Microsoft Cognitive Services-APIs verwendet werden.  Egal, ob Sie dieses Beispiel als Startpunkt für Ihre eigene Anwendung oder als API-Anleitung verwenden: Es ist sinnvoll, jeden Bildschirm der Anwendung zu betrachten, um die Funktionsweise genau zu verstehen.

### <a name="add-keys-page"></a>Seite „Schlüssel hinzufügen“
Die Benutzeroberfläche der Seite „Schlüssel hinzufügen“ ist in `AddKeysPage.xaml` und die primäre Logik in `AddKeysPage.xaml.cs` definiert. Da die Schlüssel mithilfe einer Testanforderung überprüft werden, sehen wir uns die Verwendung der Cognitive Services-Endpunkte in C# an.  

Die Basisstruktur dieser Interaktion ist: 

1. Instanziieren Sie *HttpResponseMessage*- und *HttpClient*-Objekte aus *System.Net.Http*.
2. Fügen Sie die gewünschten Header (definiert in der jeweiligen Endpunkt-API-Referenz) dem *HttpClient*-Objekt hinzu.
3. Senden Sie eine GET- oder POST-Anforderung mit Ihren Daten, und fügen Sie der Endpunkt-URI alle erforderlichen Parameter hinzu.
4. Überprüfen Sie, ob die Antwort erfolgreich war.
5. Übermitteln Sie die Antwort, damit sie weiter verarbeitet wird.

Die Funktion `CheckBingSearchKey()`, die die Gültigkeit des Bing-Suche-API-Schlüssels überprüft, wird hier angezeigt.

```csharp
async Task CheckBingSearchKey(object sender = null, EventArgs e = null)
{
    HttpResponseMessage response;
    HttpClient SearchApiClient = new HttpClient();

    SearchApiClient.DefaultRequestHeaders.Add(AppConstants.OcpApimSubscriptionKey, BingSearchKeyEntry.Text);

    try
    {
        response = await SearchApiClient.GetAsync(AppConstants.BingWebSearchApiUrl + "q=test");

        if (response.StatusCode != System.Net.HttpStatusCode.Unauthorized)
        {
            BingSearchKeyEntry.BackgroundColor = Color.Green;
            AppConstants.BingWebSearchApiKey = BingSearchKeyEntry.Text;
            bingSearchKeyWorks = true;
        }
        else
        {
            BingSearchKeyEntry.BackgroundColor = Color.Red;
            bingSearchKeyWorks = false;
        }
    }
    catch( Exception exception )
    {
        BingSearchKeyEntry.BackgroundColor = Color.Red;
        Console.WriteLine($"ERROR: {exception.Message}");
    }
}
```

Mit der ähnlichen Funktion `CheckComputerVisionKey()` werden Maschinelles Sehen-Schlüssel überprüft.

### <a name="ocr-select-page"></a>Seite für die OCR-Auswahl

Die Seite für die OCR-Auswahl (`OcrSelectPage.xaml`) erfüllt zwei wichtige Funktionen. Erstens legt sie fest, welche Art von OCR für das Zielfoto ausgeführt wird. Zweitens können Sie darüber das Bild aufnehmen oder öffnen, das Sie verarbeiten möchten.

Der zweite Aspekt ist meist relativ aufwändig in einer plattformübergreifenden Anwendung, weil für jede Plattform ein anderer Code erforderlich ist. Das Media-Plug-In für Xamarin benötigt dafür nur wenige Codezeilen.

Die folgende Funktion enthält ein Beispiel für die Verwendung des Media-Plug-Ins für Xamarin zur Bilderfassung.  Gehen Sie folgendermaßen vor:

1. Überprüfen Sie die Verfügbarkeit der Kamera auf dem aktuellen Gerät.
2. Instanziieren Sie ein `StoreCameraMediaOptions`-Objekt, um den Speicherort des aufgenommenen Bilds anzugeben.
3. Nehmen Sie ein Bild auf. Sie erhalten ein `MediaFile`-Objekt mit den Bilddaten.
4. Entpacken Sie `MediaFile` in ein Bytearray.
5. Geben Sie das Bytearray zur weiteren Verarbeitung zurück.

Das ist die Funktion `TakePhoto()`, die das Media-Plug-In für Xamarin für die Aufnahme von Fotos verwendet.  

```csharp
async Task<byte[]> TakePhoto()
{
    MediaFile photoMediaFile = null;
    byte[] photoByteArray = null;

    if (CrossMedia.Current.IsCameraAvailable)
    {
        var mediaOptions = new StoreCameraMediaOptions
        {
            PhotoSize = PhotoSize.Medium,
            AllowCropping = true,
            SaveToAlbum = true,
            Name = $"{DateTime.UtcNow}.jpg"
        };
        photoMediaFile = await CrossMedia.Current.TakePhotoAsync(mediaOptions);
        photoByteArray = MediaFileToByteArray(photoMediaFile);
    }
    else
    {
        await DisplayAlert("Error", "No camera found", "OK");
        Console.WriteLine($"ERROR: No camera found");
    }
    return photoByteArray;
}
```
Das Hilfsprogramm für den Fotoimport funktioniert ähnlich. Beide Funktionen finden Sie hier: `OcrSelectPage.xaml.cs`. 
 
> [!NOTE]
> Der Endpunkt für die OCR von handschriftlichem Text kann nur Fotos verarbeiten, die kleiner als 4 MB sind. Um Probleme mit der Dateigröße zu vermeiden, reduzieren wir das Foto auf 50 % der ursprünglichen Größe, indem wir für das Objekt `StoreCameraMediaOptions` die Option `PhotoSize = PhotoSize.Medium` festlegen.  Wenn Ihr Gerät Fotos in sehr hoher Qualität aufnimmt und Sie Fehlermeldungen erhalten, versuchen Sie stattdessen die Option `PhotoSize = PhotoSize.Small`.

Das ist die Hilfsprogrammfunktion für das Konvertieren von *MediaFile* in ein Bytearray.

```csharp
byte[] MediaFileToByteArray(MediaFile photoMediaFile)
{
    using (var memStream = new MemoryStream())
    {
        photoMediaFile.GetStream().CopyTo(memStream);
        return memStream.ToArray();
    }
}
```

### <a name="ocr-results-page"></a>Seite mit OCR-Ergebnissen

Die Seite mit den OCR-Ergebnissen zeigt Text an, der aus dem Bild mit **Json.NET** mithilfe der [Methode SelectToken](http://www.newtonsoft.com/json/help/html/SelectToken.htm) extrahiert wurde.  Die Funktionsweise der beiden OCR-Endpunkte unterscheidet sich ein wenig, sodass wir beide gesondert betrachten.  

Da die Maschinelles Sehen-API nur an wenigen Serverspeicherorten gehostet wird, muss der Endpunkt dynamisch zur Laufzeit erstellt werden. Die Funktion `SetOcrLocation` (Teil der statischen *AppConstants*-Klasse in `AppConstants.cs`) bestimmt den Speicherort des URI-Endpunkts. Er entspricht der Benutzerauswahl auf der Seite „Schlüssel hinzufügen“ oder dem in `App.xaml.cs` festgelegten Wert. 

```csharp
public static void SetOcrLocation(string location)
{
    ComputerVisionApiOcrUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/ocr?language=en&detectOrientation=true";
    ComputerVisionApiHandwritingUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true";
}
```

Sehen wir uns diese API-Anforderungen genauer an. Die Maschinelles Sehen-API für die OCR kann zwar Text in einer unbestimmten Sprache analysieren, doch wir legen fest, dass nach englischem Text gesucht werden soll, um die Ergebnisse zu verbessern. Außerdem erkennt die API die Textausrichtung. Das Verwenden einer fixierten Ausrichtung kann die Analyseergebnisse verbessern. Allerdings kann die Erkennung der Ausrichtung in einer mobilen Anwendung nützlich sein.

Weitere Informationen zu den mit diesem Endpunkt verfügbaren Parametern finden Sie in der [OCR-API-Referenz für gedruckten Text](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc).  

Die OCR-API für handschriftlichen Text befindet sich noch in der Vorschau, und ist zurzeit nur für die englische Sprache verfügbar.  Deswegen ist der einzige Parameter derzeit ein Flag, das angibt, ob handschriftlicher Text analysiert werden soll. Die API für die OCR handgeschriebener Texte kann sowohl computergedruckten als auch handschriftlichen Text analysieren. Allerdings erzielt `handwriting=false` bessere Ergebnisse für gedruckten Text. 

Da diese Anwendung auf Englisch verfügbar ist, hätten wir für dieses Beispiel nur die OCR für handschriftlichen Text verwenden und das `handwriting` Flag entsprechend der Angaben des Benutzers setzen können.  Zur Veranschaulichung haben wir beide Endpunkte verwendet.  

Weitere Informationen zu den mit diesem Endpunkt verfügbaren Parametern finden Sie in der [OCR-API-Referenz für handschriftlichen Text](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).

Betrachten wir nun die Funktionen, die diese APIs aufrufen.

`FetchPrintedWordList()` verwendet den Endpunkt für die OCR von handschriftlichem Text, um gedruckten Text in Bildern zu analysieren. Die HTTP-Anforderung folgt einer ähnlichen Struktur wie der Aufruf zur Schlüsselüberprüfung auf der Seite „Schlüssel hinzufügen“, allerdings müssen wir ein Foto senden. Ein Foto ist zu groß für eine Übermittlung in einer Abfragezeichenfolge. Deswegen muss eine HTTP-POST-Anforderung anstelle einer GET-Anforderung verwendet werden. Das Foto (im Arbeitsspeicher als Bytearray vorhanden) muss als `ByteArrayContent`-Objekt codiert werden. Fügen Sie dem Objekt einen Header hinzu, der den gesendeten Datentyp definiert. 

Die zulässigen Inhaltstypen finden Sie in der [Maschinelles Sehen-API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).  

```csharp
async Task<ObservableCollection<string>> FetchPrintedWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiOcrUrl, content);
        }

        string ResponseString = await response.Content.ReadAsStringAsync();
        JObject json = JObject.Parse(ResponseString);

        // Here, we pull down each "line" of text and then join it to 
        // make a string representing the entirety of each line.  
        // In the Handwritten endpoint, you are able to extract the 
        // "line" without any further processing.  If you would like 
        // to simply get a list of all extracted words,* you can do 
        // this with:
        // json.SelectTokens("$.regions[*].lines[*].words[*].text) 
        IEnumerable<JToken> lines = json.SelectTokens("$.regions[*].lines[*]");
        if (lines != null)
        {
            foreach (JToken line in lines)
            {
                IEnumerable<JToken> words = line.SelectTokens("$.words[*].text");
                if (words != null)
                {
                    wordList.Add(string.Join(" ", words.Select(x => x.ToString())));
                }
            }
        }
    }
    return wordList;
}
```
> [!TIP]
> Mit der **Json.NET**-Methode [SelectToken](http://www.newtonsoft.com/json/help/html/SelectToken.htm) extrahieren wir Text aus dem Antwortobjekt. `SelectToken` eignet sich ideal, wenn nur ein bestimmter Teil der JSON-Antwort benötigt wird, der dann an die nächste Funktion übergeben werden kann. In anderen Teilen des Codes deserialisieren wir JSON-Antworten in Modellobjekte, die in `ModelObjects.cs` definiert sind.

Der Hauptunterschied zwischen der OCR-Anforderung für gedruckten und handschriftlichen Text ist: Der OCR-Dienst für gedruckten Text gibt den erkannten Text als Teil der HTTP-Antwort zurück – der OCR-Dienst für handschriftlichen Text erfordert eine zusätzliche Anforderung zum Abrufen der Informationen. Die ursprüngliche OCR-Anforderung für handschriftlichen Text gibt einen HTTP-Status 202 zurück, der nur kennzeichnet, dass die Verarbeitung begonnen wurde. Diese Antwort enthält einen Endpunkt, den der Client überprüfen muss, um die vollständige Antwort zu erhalten, sobald sie verfügbar ist. `FetchHandwrittenWordList()` zeigt die gesamte Funktionsweise.

```csharp
async Task<ObservableCollection<string>> FetchHandwrittenWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        // Make the POST request to the handwriting recognition URL
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiHandwritingUrl, content);
        }

        // Fetch results
        IEnumerable<string> operationLocationValues;
        string statusUri = string.Empty;
        if (response.Headers.TryGetValues("Operation-Location", out operationLocationValues))
        {
            statusUri = operationLocationValues.FirstOrDefault();

            // Ping status URL, wait for processing to finish 
            JObject obj = await FetchResultFromStatusUri(statusUri);
            IEnumerable<JToken> strings = obj.SelectTokens("$.recognitionResult.lines[*].text");
            foreach (string s in strings)
            {
                wordList.Add((string)s);
            }
        }
    }
    return wordList;
}
```

`FetchResultFromStatusUri()` ist der zweite Teil des OCR-Prozesses für handschriftlichen Text. Die Funktion pingt den URI, der aus den Metadaten der ursprünglichen Antwort extrahiert wurde, bis ein Ergebnis ermittelt wird oder ein Timeout eintritt.  Es ist wichtig, dass diese Funktion asynchron in einem eigenen Thread aufgerufen wird. Andernfalls wird die Benutzeroberfläche gesperrt, bis die Verarbeitung abgeschlossen ist.

```csharp
// Takes in the url to check for handwritten text parsing results, and pings it per second until processing is finished
// Returns the JObject holding data for a successful parse
async Task<JObject> FetchResultFromStatusUri(string statusUri)
{
    JObject obj = null;
    int timeoutcounter = 0;
    HttpResponseMessage response = await visionApiClient.GetAsync(statusUri);
    string responseString = await response.Content.ReadAsStringAsync();
    obj = JObject.Parse(responseString);
    while ((!((string)obj.SelectToken("status")).Equals("Succeeded")) && (timeoutcounter++ < 60))
    {
        await Task.Delay(1000);
        response = await visionApiClient.GetAsync(statusUri);
        responseString = await response.Content.ReadAsStringAsync();
        obj = JObject.Parse(responseString);
    } 
    return obj;
}
```

### <a name="web-results-page"></a>Seite „Webergebnisse“
Wählen Sie einen Suchbegriff aus, der auf der Seite mit den OCR-Ergebnissen angezeigt wird, und gehen Sie weiter auf die Seite „Webergebnisse“.  Hier erstellen Sie eine [Bing-Websuche-API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)-Anforderung, senden diese an den Dienstendpunkt, und deserialisieren die JSON-Antwort mit **Json.NET** mit der [DeserializeObject](http://www.newtonsoft.com/json/help/html/DeserializeObject.htm)-Methode.  

```csharp
async Task<WebResultsList> GetQueryResults()
{
    // URL-encode the query term
    var queryString = System.Net.WebUtility.UrlEncode(queryTerm);

    // Here we encode the URL that will be used for the GET request to 
    // find query results.  Its arguments are as follows:
    // 
    // - [count=20] This sets the number of webpage objects returned at 
    //   "$.webpages" in the JSON response.  Currently, the API asks for 
    //   20 webpages in the response
    //
    // - [mkt=en-US] This sets the market where the results come from.
    //   Currently, the API looks for english results based in the 
    //   United States.
    //
    // - [q=queryString] This sets the string queried using the Search 
    //   API.   
    //
    // - [responseFilter=Webpages] This filters the response to only 
    //   include Webpage results.  This tag can take a comma seperated 
    //   list of response types that you are looking for.  If left 
    //   blank, all responses (webPages, News, Videos, etc) are 
    //   returned.
    //
    // - [setLang=en] This sets the languge for user interface strings. 
    //   To learn more about UI strings, check the Web Search API 
    //   reference.
    //
    // - [API Reference] https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference
    string uri = AppConstants.BingWebSearchApiUrl + $"count=20&mkt=en-US&q={queryString}&responseFilter=Webpages&setLang=en";

    // Make the HTTP Request
    WebResultsList webResults = null;
    HttpResponseMessage httpResponseMessage = await searchApiClient.GetAsync(uri);
    var responseContentString = await httpResponseMessage.Content.ReadAsStringAsync();
    JObject json = JObject.Parse(responseContentString);
    JToken resultBlock = json.SelectToken("$.webPages");
    if (resultBlock != null)
    {
        webResults = JsonConvert.DeserializeObject<WebResultsList>(resultBlock.ToString());
    }
    return webResults;
}
```

Die Bing-Websuche-API funktioniert am besten, wenn Sie so viele Informationen wie möglich zu den Wünschen des Benutzers bereitstellen. Vor allem sollten Sie in der Regel die Parameter `mkt` und `setLang` (hier für Englisch (USA) festgelegt) verwenden, um den Standort und die bevorzugte Sprache des Benutzers zu identifizieren.

> [!NOTE]
> Wir halten die Bing-Websuche-Abfrage einfach, damit der Quellcode leicht verständlichen ist.  In einer realen Anwendung sollten Sie Ihrer HTTP-Anforderung die folgenden Header hinzufügen, um bessere Ergebnisse zu erhalten: 
> * Benutzer-Agent  
> * X-MSEdge-ClientID  
> * X-Search-ClientIP  
> * X-Search-Location  
>
> Weitere Informationen zu diesen Headerwerten finden Sie in der [Bing-Websuche-API-Referenz](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers).

## <a name="next-steps"></a>Nächste Schritte
Microsoft Cognitive Services bietet viele zusätzliche Dienste, die Sie problemlos in diese Anwendung integrieren können.  Beispielsweise ist Folgendes möglich:

* Hinzufügen der [Bing-Entitätssuche](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/), um Websuchergebnisse zu verbessern
* Auswahl von [Benutzerdefinierte Bing-Suche](https://azure.microsoft.com/services/cognitive-services/bing-custom-search/) anstelle von „Bing-Websuche“
* Verwenden der Bildauswertungsfunktion [Bing-Bildersuche](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/), um weitere Informationen zu aufgenommenen Bildern zu erhalten, und ähnliche Bilder im Web zu suchen
* Verwenden der [Bing-Rechtschreibprüfung](https://azure.microsoft.com/services/cognitive-services/spell-check/), um die Qualität analysierter Texte zu verbessern
* Integrieren von [Microsoft Translator](https://azure.microsoft.com/services/cognitive-services/translator-text-api/), um extrahierten Text in verschiedenen Sprachen anzuzeigen
* Kombinieren und Hinzufügen anderer Dienste aus dem [Cognitive Services-Portal](https://azure.microsoft.com/services/cognitive-services/) für die optimale Benutzerfreundlichkeit

---
title: Cognitive Dienste auf Azure Government | Microsoft Docs
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: yujhongmicrosoft
manager: zakramer
ms.assetid: cba97199-851d-43ae-a75a-c601f3f81601
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 10/09/2017
ms.author: yujhong
ms.openlocfilehash: 7a11ba977d7b5e557981b1a65284022905488485
ms.sourcegitcommit: 09a2485ce249c3ec8204615ab759e3b58c81d8cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="cognitive-services-on-azure-government--computer-vision-face-translator-text-apis"></a>Cognitive Dienste auf Azure Government – Computer-Vision Smiley, Konvertierer Text-APIs

Um einen Überblick über die Cognitive Dienste auf Azure Government, finden Sie unter [klicken Sie hier](documentation-government-services-aiandcognitiveservices.md).

## <a name="prerequisites"></a>Voraussetzungen
* Installieren und Konfigurieren von [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0)
* Verbinden [PowerShell mit Azure Government](documentation-government-get-started-connect-with-ps.md)

## <a name="part-1-provision-cognitive-services-accounts"></a>Teil 1: Bereitstellung Cognitive Services-Konten

Zugriff auf eine der Cognitive Dienste-APIs, müssen Sie zuerst ein Cognitive Services-Konto für jede der APIs bereitstellen, die Sie zugreifen möchten. **Cognitive Services noch nicht im Azure Government-Portal unterstützt**, aber Sie können Azure PowerShell verwenden, um die APIs und -Dienste zugreifen. 

> [!NOTE]
> Sie müssen durch das Erstellen eines Kontos und das Abrufen eines Schlüssels (siehe unten) wechseln **für jede** der APIs, die Sie zugreifen möchten.
> 
> 

1. Stellen Sie sicher, dass die **Cognitive Services Ressourcenanbieter auf Ihr Konto registriert**. Sie können dazu **den folgenden Powershell-Befehl ausführen:**

   ```PowerShell
   Get-AzureRmResourceProvider
   ```
   Wenn Sie dies tun **nicht finden Sie unter `Microsoft.CognitiveServices`** , müssen Sie die Ressourcenanbieter von registrieren **Ausführen des folgenden Befehls**:
   ```PowerShell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.CognitiveServices
   ```
2. Ersetzen Sie in den folgenden Powershell-Befehl "Rg-Name", "Name-des-your-api" und "Speicherort der Ressourcengruppe" durch die entsprechenden Kontoinformationen ein. 

   Ersetzen Sie das Tag "API-Typ" durch eine der drei folgenden APIs, die Sie zugreifen möchten:
       * ComputerVision
       * Gesicht
       * TextTranslation

   ```PowerShell
   New-AzureRmCognitiveServicesAccount -ResourceGroupName 'rg-name' -name 'name-of-your-api' -Type <type of API> -SkuName S0 -Location 'location-of-resourcegroup'
   ```
   Beispiel: 

   ```PowerShell
   New-AzureRmCognitiveServicesAccount -ResourceGroupName 'resourcegrouptest' -name 'myFaceAPI' -Type Face -SkuName S0 -Location 'usgovvirginia'
   ```

   Nachdem Sie den Befehl ausführen, sollte etwa wie folgt angezeigt werden: 

   ![cog1](./media/documentation-government-cognitiveservices-img1.png)

3. Kopieren und speichern Sie die "Endpoint"-Attribut an einer Stelle wie Sie es benötigen, wenn Aufrufe an die API. 

### <a name="retrieve-account-key"></a>Kontoschlüssel abrufen

Sie müssen einen kontoschlüssel für den Zugriff auf die jeweilige API abgerufen werden. 

Ersetzen Sie das Tag "Youraccountname" in den folgenden Powershell-Befehl mit dem Namen, den Sie das Konto gegeben, die Sie haben soeben erstellt haben. Ersetzen Sie das Tag "Rg-Name" mit dem Namen der Ressourcengruppe an.

```PowerShell
Get-AzureRmCognitiveServicesAccountKey -Name <youraccountname> -ResourceGroupName 'rg-name'
```

Beispiel:
```PowerShell
Get-AzureRmCognitiveServicesAccountKey -Name myFaceAPI -ResourceGroupName 'resourcegrouptest'
```
Kopieren und speichern Sie das erste an einer beliebigen Stelle Schlüssel als Sie benötigen die Angaben an die API aufrufen.

![cog2](./media/documentation-government-cognitiveservices-img2.png)

Jetzt sind Sie Aufrufe an die APIs bereit. 

## <a name="part-2-api-quickstarts"></a>Teil 2: API-Schnellstarts 
Die folgenden Schnellstarts unterstützt Sie beim Einstieg in die APIs, die über Cognitive in Azure Government-Dienste zur Verfügung.

## <a name="computer-vision-api"></a>Maschinelles Sehen-API
### <a name="prerequisites"></a>Voraussetzungen

* Das Windows SDK für Microsoft Computer Vision-API abrufen [hier](https://github.com/Microsoft/Cognitive-vision-windows).

* Stellen Sie sicher, dass Visual Studio installiert wurde:
    -   [Visual Studio 2017, Version 15.3](https://www.visualstudio.com/vs/preview/), einschließlich der Workload **Azure-Entwicklung**
    
    >[!NOTE] 
    > Nach der Installation oder upgrade auf Visual Studio 2017 Version 15.3, müssen Sie auch die 2017 von Visual Studio-Tools für Azure-Funktionen manuell aktualisieren. Sie können die Tools im Menü **Extras** aktualisieren. Navigieren Sie zu **Erweiterungen und Updates...** >          **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**. 
    >
    >
    
### <a name="variations"></a>Abweichungen
* Der URI für den Zugriff auf die Oberfläche-API in Azure Government lautet:
   - `https://(resource-group-location).api.cognitive.microsoft.us/face/v1.0`
   - Der Hauptunterschied zwischen diesem URI und der in der kommerziellen Azure verwendete URI ist das Ende des **US** und der Ort, an den Anfang des URIs


### Analysieren Sie ein Image mit Computer Vision API mithilfe von c# <a name="AnalyzeImage"> </a>

Mit der [analysieren Image Methode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa), können Sie visuelle Funktionen auf der Grundlage von Bildinhalt extrahieren. Sie können ein Image hochladen oder geben Sie eine Bild-URL und wählen Sie die Funktionen, die zurückgegeben wird, einschließlich:
* Eine detaillierte Liste der Transponder, die im Zusammenhang mit dem Inhalt des Betriebssystemabbilds.
* Eine Beschreibung des bildinhalttyps in ein vollständiger Satz.
* Die Koordinaten, Geschlecht und Alter der alle Flächen in der Abbildung enthalten.
* ImageType (ClipArt oder eine Linie zeichnen).
* Die vorherrschende Farbe, Akzentfarbe, oder ob ein Bild schwarz ist & Weiß.
* Die definierten in dieser Kategorie [Taxonomie](../cognitive-services/computer-vision/category-taxonomy.md).
* Enthält das Image sexuell unzulässige Inhalt?

### <a name="analyze-an-image-c-example-request"></a>Analysieren Sie eine Image C#-Beispiel für eine Anforderung

1. Erstellen Sie eine neue Konsole-Projektmappe in Visual Studio.
2. Ersetzen Sie "Program.cs" durch den folgenden Code ein.
3. Ändern der `uriBase` "Endpoint"-Attribut, das Sie aus Teil 1 gespeichert haben, und behalten Sie die "/ analyze" nach dem Endpunkt.
4. Ersetzen Sie die `subscriptionKey` Wert mit Ihrem gültiger Abonnementschlüssel.
5. Führen Sie das Programm aus.

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;

namespace VisionApp1
{
        static class Program
        {
            // **********************************************
            // *** Update or verify the following values. ***
            // **********************************************

            // Replace the subscriptionKey string value with your valid subscription key.
            const string subscriptionKey = "<subscription key>";

            //Copy and paste the "Endpoint" attribute that you saved before into the uriBase string "/analyze" at the end. 
            //Example: https://virginia.api.cognitive.microsoft.us/vision/v1.0/analyze
  
            const string uriBase = "<endpoint>/analyze";
            
            static void Main()
            {
                // Get the path and filename to process from the user.
                Console.WriteLine("Analyze an image:");
                Console.Write("Enter the path to an image you wish to analyze: ");
                string imageFilePath = Console.ReadLine();

                // Execute the REST API call.
                MakeAnalysisRequest(imageFilePath);

                Console.WriteLine("\nPlease wait a moment for the results to appear. Then, press Enter to exit...\n");
                Console.ReadLine();
            }


            /// <summary>
            /// Gets the analysis of the specified image file by using the Computer Vision REST API.
            /// </summary>
            /// <param name="imageFilePath">The image file.</param>
            static async void MakeAnalysisRequest(string imageFilePath)
            {
                HttpClient client = new HttpClient();

                // Request headers.
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

                // Request parameters. A third optional parameter is "details".
                string requestParameters = "visualFeatures=Categories,Description,Color&language=en";

                // Assemble the URI for the REST API Call.
                string uri = uriBase + "?" + requestParameters;

                HttpResponseMessage response;

                // Request body. Posts a locally stored JPEG image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (ByteArrayContent content = new ByteArrayContent(byteData))
                {
                    // This example uses content type "application/octet-stream".
                    // The other content types you can use are "application/json" and "multipart/form-data".
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

                    // Execute the REST API call.
                    response = await client.PostAsync(uri, content);

                    // Get the JSON response.
                    string contentString = await response.Content.ReadAsStringAsync();

                    // Display the JSON response.
                    Console.WriteLine("\nResponse:\n");
                    Console.WriteLine(JsonPrettyPrint(contentString));
                }
            }


            /// <summary>
            /// Returns the contents of the specified file as a byte array.
            /// </summary>
            /// <param name="imageFilePath">The image file to read.</param>
            /// <returns>The byte array of the image data.</returns>
            static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }


            /// <summary>
            /// Formats the given JSON string by adding line breaks and indents.
            /// </summary>
            /// <param name="json">The raw JSON string to format.</param>
            /// <returns>The formatted JSON string.</returns>
            static string JsonPrettyPrint(string json)
            {
                if (string.IsNullOrEmpty(json))
                    return string.Empty;

                json = json.Replace(Environment.NewLine, "").Replace("\t", "");

                StringBuilder sb = new StringBuilder();
                bool quote = false;
                bool ignore = false;
                int offset = 0;
                int indentLength = 3;

                foreach (char ch in json)
                {
                    switch (ch)
                    {
                        case '"':
                            if (!ignore) quote = !quote;
                            break;
                        case '\'':
                            if (quote) ignore = !ignore;
                            break;
                    }

                    if (quote)
                        sb.Append(ch);
                    else
                    {
                        switch (ch)
                        {
                            case '{':
                            case '[':
                                sb.Append(ch);
                                sb.Append(Environment.NewLine);
                                sb.Append(new string(' ', ++offset * indentLength));
                                break;
                            case '}':
                            case ']':
                                sb.Append(Environment.NewLine);
                                sb.Append(new string(' ', --offset * indentLength));
                                sb.Append(ch);
                                break;
                            case ',':
                                sb.Append(ch);
                                sb.Append(Environment.NewLine);
                                sb.Append(new string(' ', offset * indentLength));
                                break;
                            case ':':
                                sb.Append(ch);
                                sb.Append(' ');
                                break;
                            default:
                                if (ch != ' ') sb.Append(ch);
                                break;
                        }
                    }
                }

                return sb.ToString().Trim();
            }
        }
    }
```
### <a name="analyze-an-image-response"></a>Analysieren Sie eine Image-Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Es folgt ein Beispiel für eine erfolgreiche Antwort:

```json

{
   "categories": [
      {
         "name": "people_baby",
         "score": 0.52734375
      },
      {
         "name": "people_young",
         "score": 0.4375
      }
   ],
   "description": {
      "tags": [
         "person",
         "indoor",
         "clothing",
         "woman",
         "white",
         "table",
         "food",
         "girl",
         "smiling",
         "posing",
         "holding",
         "black",
         "sitting",
         "young",
         "plate",
         "hair",
         "wearing",
         "cake",
         "large",
         "shirt",
         "dress",
         "eating",
         "standing",
         "blue"
      ],
      "captions": [
         {
            "text": "a woman posing for a picture",
            "confidence": 0.460196158842535
         }
      ]
   },
   "requestId": "7c20cc50-f5eb-453b-abb5-98378917431c",
   "metadata": {
      "width": 721,
      "height": 960,
      "format": "Jpeg"
   },
   "color": {
      "dominantColorForeground": "Black",
      "dominantColorBackground": "White",
      "dominantColors": [
         "White"
      ],
      "accentColor": "7C4F57",
      "isBWImg": false
   }
}
```
Weitere Informationen finden Sie unter [öffentliche Dokumentation](../cognitive-services/computer-vision/index.md) und [öffentliche API-Dokumentation](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa) für Computer Vision-API.

## <a name="face-api"></a>Gesichtserkennungs-API
### <a name="prerequisites"></a>Voraussetzungen
* Abrufen von Microsoft Gesicht API Windows SDK [hier](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)

* Stellen Sie sicher, dass Visual Studio installiert wurde:
    -   [Visual Studio 2017, Version 15.3](https://www.visualstudio.com/vs/preview/), einschließlich der Workload **Azure-Entwicklung**
    
    >[!NOTE] 
    > Nach der Installation oder upgrade auf Visual Studio 2017 Version 15.3, müssen Sie auch die 2017 von Visual Studio-Tools für Azure-Funktionen manuell aktualisieren. Sie können die Tools im Menü **Extras** aktualisieren. Navigieren Sie zu **Erweiterungen und Updates...** >          **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**. 
    >
    >
    
### <a name="variations"></a>Abweichungen 
* Der URI für den Zugriff auf die Oberfläche-API in Azure Government lautet:
   - `https://(resource-group-location).api.cognitive.microsoft.us/face/v1.0`
   - Der Hauptunterschied zwischen diesem URI und der in der kommerziellen Azure verwendete URI ist das Ende des **US** und der Ort, an den Anfang des URIs

### Erkennen von Flächen in Bildern mit mithilfe von c#-API-Oberfläche <a name="Detect"> </a>
Verwenden der [werdender - Methode erkennen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) Flächen in einem Bild zu erkennen und zurückgeben Gesicht Attribute einschließlich:
* Vordere-ID: Eindeutige ID in verschiedenen Gesicht-API-Szenarien verwendet. 
* Vordere Rechteck: Links, oben, Breite und Höhe, die den Speicherort der Schriftart in der Abbildung.
* Informationen: Ein Array von 27 Point-Face-Informationen zu den wichtigen Positionen Gesicht-Komponenten verweisen.
* Gesichtsausdruck Alter, Geschlecht, einschließlich Attribute Smiley Gesichtsausdruck Haar, Intensität und Head darstellen. 

### <a name="face-detect-c-example-request"></a>C#-Beispiel für eine Anforderung zu Gesicht erkennen

Im Beispiel wird in c# geschrieben mithilfe der Clientbibliothek Gesicht-API. 

1. Erstellen Sie eine neue Konsole-Projektmappe in Visual Studio.
2. Ersetzen Sie "Program.cs" durch den folgenden Code ein.
3. Ersetzen Sie die `subscriptionKey` Wert mit dem Schlüsselwert, die Sie über abgerufen.
4. Ändern der `uriBase` Wert für das "Endpoint"-Attribut, das Sie oben abgerufen.
5. Führen Sie das Programm aus.
6. Geben Sie den Pfad zu einem Bild auf der Festplatte freigeben.

```csharp

using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;

namespace FaceApp1
{

    static class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************
         
        // Replace the subscriptionKey string value with your valid subscription key.
        const string subscriptionKey = "<subscription key>";

        //Copy and paste the "Endpoint" attribute that you saved before into the uriBase string "/detect" at the end. 
        //Example: https://virginia.api.cognitive.microsoft.us/face/v1.0/detect
        const string uriBase ="<endpoint>/detect";

        static void Main()
        {
            // Get the path and filename to process from the user.
            Console.WriteLine("Detect faces:");
            Console.Write("Enter the path to an image with faces that you wish to analzye: ");
            string imageFilePath = Console.ReadLine();

            // Execute the REST API call.
            MakeAnalysisRequest(imageFilePath);

            Console.WriteLine("\nPlease wait a moment for the results to appear. Then, press Enter to exit...\n");
            Console.ReadLine();
        }


        /// <summary>
        /// Gets the analysis of the specified image file by using the Computer Vision REST API.
        /// </summary>
        /// <param name="imageFilePath">The image file.</param>
        static async void MakeAnalysisRequest(string imageFilePath)
        {
            HttpClient client = new HttpClient();

            // Request headers.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request parameters. A third optional parameter is "details".
            string requestParameters = "returnfaceId=true&returnfaceLandmarks=false&returnfaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion";

            // Assemble the URI for the REST API Call.
            string uri = uriBase + "?" + requestParameters;

            HttpResponseMessage response;

            // Request body. Posts a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (ByteArrayContent content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

                // Execute the REST API call.
                response = await client.PostAsync(uri, content);

                // Get the JSON response.
                string contentString = await response.Content.ReadAsStringAsync();

                // Display the JSON response.
                Console.WriteLine("\nResponse:\n");
                Console.WriteLine(JsonPrettyPrint(contentString));
            }
        }


        /// <summary>
        /// Returns the contents of the specified file as a byte array.
        /// </summary>
        /// <param name="imageFilePath">The image file to read.</param>
        /// <returns>The byte array of the image data.</returns>
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }


        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            int offset = 0;
            int indentLength = 3;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\'':
                        if (quote) ignore = !ignore;
                        break;
                }

                if (quote)
                    sb.Append(ch);
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (ch != ' ') sb.Append(ch);
                            break;
                    }
                }
            }

            return sb.ToString().Trim();
        }
    }
}
```
### <a name="face-detect-response"></a>Vordere erkennen Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Es folgt ein Beispiel für eine erfolgreiche Antwort: 

```json
Response:
[
   {
      "faceId": "0ed7f4db-1207-40d4-be2e-84694e42d682",
      "faceRectangle": {
         "top": 60,
         "left": 83,
         "width": 361,
         "height": 361
      },
      "faceAttributes": {
         "smile": 0.284,
         "headPose": {
            "pitch": 0.0,
            "roll": -12.2,
            "yaw": -16.7
         },
         "gender": "female",
         "age": 16.5,
         "facialHair": {
            "moustache": 0.0,
            "beard": 0.0,
            "sideburns": 0.0
         },
         "glasses": "NoGlasses",
         "emotion": {
            "anger": 0.003,
            "contempt": 0.001,
            "disgust": 0.001,
            "fear": 0.002,
            "happiness": 0.284,
            "neutral": 0.694,
            "sadness": 0.012,
            "surprise": 0.004
         }
      }
   }
]
```
Weitere Informationen finden Sie unter [öffentliche Dokumentation](../cognitive-services/Face/index.md), und [öffentliche API-Dokumentation](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) für Gesicht-API.

## <a name="text-translation-api"></a>Textübersetzung API 
### <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass Visual Studio installiert wurde:
    -   [Visual Studio 2017, Version 15.3](https://www.visualstudio.com/vs/preview/), einschließlich der Workload **Azure-Entwicklung**
    
    >[!NOTE] 
    > Nach der Installation oder upgrade auf Visual Studio 2017 Version 15.3, müssen Sie auch die 2017 von Visual Studio-Tools für Azure-Funktionen manuell aktualisieren. Sie können die Tools im Menü **Extras** aktualisieren. Navigieren Sie zu **Erweiterungen und Updates...** >          **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**. 
    >
    >
    
### <a name="variations"></a>Abweichungen
* Der URI für den Zugriff auf den Text Ausdrucksübersetzungs-API in Azure Government lautet: 
   - `https://api.microsofttranslator.us/v2/http.svc`
   - Der einzige Unterschied aus dem kommerziellen URI ist der Endpunkt des **US**
* Der URI zum Abrufen des Zugriffstokens, das Text-Ausdrucksübersetzungs-API lautet der `Endpoint` -Attribut, das in Teil 1 von diesem Schnellstart mit gespeichert wurde `/IssueToken` am Ende

### <a name="text-translation-method"></a>Text Übersetzung-Methode
Verwenden Sie dieses Beispiel wird die [Textübersetzung - Translate-Methode](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) übersetzen Sie eine Textzeichenfolge in einer anderen Sprache in eine andere Sprache angegeben. Es gibt mehrere [Sprachcodes](https://msdn.microsoft.com/library/hh456380.aspx) , mit dem Text Ausdrucksübersetzungs-API verwendet werden kann. 

### <a name="text-translation-c-example-request"></a>Text Übersetzung C#-Beispiel für eine Anforderung

Im Beispiel wird in c# geschrieben. 

1. Erstellen Sie eine neue Konsole-Projektmappe in Visual Studio.
2. Ersetzen Sie "Program.cs" mit den entsprechenden folgenden Code ein.
3. Ersetzen Sie die `subscriptionKey` Wert mit dem Schlüsselwert, die Sie über abgerufen.
4. Erstellen Sie eine Token.cs-Klasse und kopieren und fügen Sie den entsprechenden Code unten. 
5. Führen Sie das Programm aus.

Sie können auch andere Sprachen und Texte testen durch Ersetzen des "Texts", "from" und "to" in "Program.cs" Variablen. 

```csharp
using System;
using System.IO;
using System.Net;
using System.Runtime.Serialization;
using System.Threading.Tasks;

namespace TextTranslationApp1
{
        static class Program
        {
            // **********************************************
            // *** Update or verify the following value. ***
            // **********************************************

            // Replace the subscriptionKey string value with your valid subscription key.
            const string subscriptionKey = "<subscription key>";

            // This is the base uri for accessing the Text Translation API. We will be testing the /translate method.
            const string uriBase = "https://api.microsofttranslator.us/v2/http.svc/translate?";

            static void Main()
            {
                MainAsync().Wait();
            }

            private static async Task MainAsync()
            {
                // Replace the "text" variable with whatever text you want to translate
                // Replace the "from" variable with the language that the "text" string is written in
                // Replace the "to" variable with the language that you want the text to be translated into

                string text = "elephants are cute";
                string from = "en";
                string to = "es";

                //Formulating the uri to make a GET call to the API 
                string uri = uriBase + "text=" + text + "&from=" + from + "&to=" + to;

                //Declare an instance of the Token class
                Token token = new Token();
                //Retrieve the token
                string accessToken = await token.GetToken(subscriptionKey);
                // Make the GET call
                HttpWebRequest httpWebRequest = (HttpWebRequest)WebRequest.Create(uri);
                httpWebRequest.Headers.Add("Authorization", accessToken);
                //Getting the JSON response
                using (WebResponse response = httpWebRequest.GetResponse())
                using (Stream stream = response.GetResponseStream())
                {
                    DataContractSerializer dcs = new DataContractSerializer(Type.GetType("System.String"));
                    string translation = (string)dcs.ReadObject(stream);
                    System.Console.WriteLine("Translation for source text '{0}' from {1} to {2} is", text, from, to);
                    Console.WriteLine(translation);
                    Console.ReadLine();
                }

            }

        }
    }
```
Wir werden jetzt eine Token.cs-Klasse erstellen, die unsere Methode zum Abrufen des Zugriffstokens, das Text-Ausdrucksübersetzungs-API enthält.
Klicken Sie mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und fügen Sie "Neue Klasse". Kopieren Sie den folgenden Code in die Token.cs-Klasse.

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

namespace TextTranslationApp1
{
    public class Token
    {
        /// URL of the token service
        /// Replace this url with your endpoint (the location should be the location of your resource group) and add the "IssueToken" at the end
        private static readonly Uri ServiceUrl = new Uri("https://virginia.api.cognitive.microsoft.us/sts/v1.0/IssueToken");

        /// Name of header used to pass the subscription key to the token service
        private const string OcpApimSubscriptionKeyHeader = "Ocp-Apim-Subscription-Key";

        /// After obtaining a valid token, this class will cache it for this duration.
        /// Use a duration of 5 minutes, which is less than the actual token lifetime of 10 minutes.
        private static readonly TimeSpan TokenCacheDuration = new TimeSpan(0, 5, 0);

        /// Cache the value of the last valid token obtained from the token service.
        public string _storedTokenValue = string.Empty;

        /// When the last valid token was obtained.
        private DateTime _storedTokenTime = DateTime.MinValue;


        /// Gets the HTTP status code for the most recent request to the token service.
        public HttpStatusCode RequestStatusCode
        {
            get; private set;
        }

        // This method retrieves and caches an access token and generates a new one after 10 minutes, when the token expires. 
        public async Task<string> GetToken(string SubscriptionKey)
        {
            if (string.IsNullOrWhiteSpace(SubscriptionKey))
            {
                return string.Empty;
            }

            // Re-use the cached token if there is one.
            if ((DateTime.Now - _storedTokenTime) < TokenCacheDuration)
            {
                return _storedTokenValue;
            }

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = ServiceUrl;
                request.Content = new StringContent(string.Empty);
                request.Headers.TryAddWithoutValidation(OcpApimSubscriptionKeyHeader, SubscriptionKey);
                client.Timeout = TimeSpan.FromMinutes(10);
                var response = await client.SendAsync(request);
                this.RequestStatusCode = response.StatusCode;
                response.EnsureSuccessStatusCode();
                var token = await response.Content.ReadAsStringAsync();
                _storedTokenTime = DateTime.Now;
                _storedTokenValue = "Bearer " + token;
                return _storedTokenValue;
            }
        }       
    }
}

```
### <a name="text-translation-response"></a>Übersetzung Textantwort

Ein Beispiel für eine erfolgreiche antwortausgabe über die Konsole ist unten dargestellt. 

![Erfolg](./media/documentation-government-cognitiveservices-img6.png)

Weitere Informationen finden Sie unter [öffentliche Dokumentation](../cognitive-services/translator/translator-info-overview.md) und [öffentliche API-Dokumentation](http://docs.microsofttranslator.com/text-translate.html) für Konvertierer Text-API.

### <a name="next-steps"></a>Nächste Schritte
* Abonnieren Sie die [Azure Government-Blog](https://blogs.msdn.microsoft.com/azuregov/)
* Abrufen von Hilfe Stapelüberlauf mithilfe der "[Azure Gov](https://stackoverflow.com/questions/tagged/azure-gov)" Tag
* Senden Sie uns Feedback, oder bitten Sie die neue Funktionen über die [Azure Government-Feedback-Forum](https://feedback.azure.com/forums/558487-azure-government)

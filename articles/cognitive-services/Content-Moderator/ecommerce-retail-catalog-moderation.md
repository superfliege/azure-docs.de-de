---
title: Moderation von E-Commerce-Katalogen mit Machine Learning und KI mit Azure Content Moderator | Microsoft-Dokumentation
description: Automatisches Moderieren von E-Commerce-Katalogen mit Machine Learning und KI
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/25/2017
ms.author: sajagtap
ms.openlocfilehash: 5cb93941751c8744a19a5473d13f1e135a589eaa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373235"
---
# <a name="ecommerce-catalog-moderation-with-machine-learning"></a>Moderation von E-Commerce-Katalog mit Machine Learning

In diesem Tutorial wird erläutert, wie die auf Machine Learning basierende intelligente Moderation von E-Commerce-Katalogen durch ein intelligentes Katalogsystem implementiert wird, das computergestützte KI-Technologien mit der von Menschen durchgeführten Moderation kombiniert.

![Klassifizierte Produktbilder](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="business-scenario"></a>Geschäftsszenario

Mit computergestützten Technologien können Produktbilder in folgende Kategorien klassifiziert und moderiert werden:

1. Nicht jugendfreie Inhalte (Nacktdarstellungen)
2. Freizügige Inhalte (Anzüglichkeiten)
3. Prominente
4. US-amerikanische Flagge
5. Spielzeuge
6. Stifte

## <a name="tutorial-steps"></a>Schritte im Tutorial

Im Tutorial werden folgende Schritte erläutert:

1. Registrieren Sie sich, und stellen Sie ein Content Moderator-Team zusammen.
2. Konfigurieren Sie Moderationstags (Bezeichnungen) für potenzielle Prominente und gekennzeichnete Inhalte.
3. Verwenden Sie die Bildersuche-API von Content Moderator, um Bilder auf potenziell nicht jugendfreie Inhalte zu prüfen.
4. Verwenden Sie die Maschinelles Sehen-API, um Bilder auf potenzielle Prominente zu prüfen.
5. Verwenden Sie den Maschinelles Sehen-Dienst, um Bilder auf das potenzielle Vorhandensein von Flags zu prüfen.
6. Lassen Sie die differenzierten Überprüfungsergebnisse durch Menschen überprüfen und zum Gegenstand für die abschließende Entscheidungsfindung machen.

## <a name="create-a-team"></a>Zusammenstellen eines Teams

Informationen zur Registrierung für Content Moderator und Zusammenstellen eines Teams finden Sie auf der [Schnellstartseite](quick-start.md). Notieren Sie sich die **Team-ID** auf der Seite **Anmeldeinformationen**.


## <a name="define-custom-tags"></a>Definieren von benutzerdefinierten Tags

Informationen zum Hinzufügen benutzerdefinierter Tags finden Sie im Artikel [Überblick über Tags](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags). Neben integrierten Tags **adult** und **racy** ermöglichen die neuen Tags die Anzeige der beschreibenden Namen für die Tags im Überprüfungstool.

In unserem Fall werden folgende benutzerdefinierte Tags (**celebrity**, **flag**, **us**, **toy**, **pen**) definiert:

![Konfigurieren von benutzerdefinierten Tags](images/tutorial-ecommerce-tags2.PNG)

## <a name="list-your-api-keys-and-endpoints"></a>Auflisten von API-Schlüsseln und -Endpunkten

1. Im Tutorial werden drei APIs und die entsprechenden Schlüssel sowie API-Endpunkte verwendet.
2. Ihre API-Endpunkte variieren je nach Abonnementregion und der Überprüfungsteam-ID von Content Moderator.

> [!NOTE]
> Das Tutorial ist für die Verwendung von Abonnementschlüsseln in den Regionen konzipiert, die in den folgenden Endpunkten angezeigt werden. Achten Sie darauf, dass Ihre API-Schlüssel mit den Regions-URIs übereinstimmen, da Ihre Schlüssel sonst möglicherweise nicht mit den folgenden Endpunkten funktionieren:

         // Your API keys
        public const string ContentModeratorKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string ComputerVisionKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string CustomVisionKey = "XXXXXXXXXXXXXXXXXXXX";

        // Your end points URLs will look different based on your region and Content Moderator Team ID.
        public const string ImageUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/Evaluate";
        public const string ReviewUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0/teams/YOURTEAMID/reviews";
        public const string ComputerVisionUri = "https://westcentralus.api.cognitive.microsoft.com/vision/v1.0";
        public const string CustomVisionUri = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/Prediction/XXXXXXXXXXXXXXXXXXXX/url";

## <a name="scan-for-adult-and-racy-content"></a>Prüfen von Bildern auf nicht jugendfreie und freizügige Inhalte

1. Die Funktion akzeptiert eine Bild-URL und ein Array von Schlüssel-Wert-Paaren als Parameter.
2. Sie ruft die Bildersuche-API von Content Moderator auf, um die Ergebnisse für nicht jugendfreie und freizügige Inhalte zu erhalten.
3. Wenn das Ergebnis größer als 0,4 ist (Bereich liegt zwischen 0 und 1), wird der Wert im Array **ReviewTags** auf **true** festgelegt.
4. Mit dem Array **ReviewTags** wird das entsprechende Tag im Überprüfungstool markiert.

        public static bool EvaluateAdultRacy(string ImageUrl, ref KeyValuePair[] ReviewTags)
        {
            float AdultScore = 0;
            float RacyScore = 0;

            var File = ImageUrl;
            string Body = $"{{\"DataRepresentation\":\"URL\",\"Value\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(ImageUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // {“answers”:[{“answer”:“Hello”,“questions”:[“Hi”],“score”:100.0}]}
                // Parse the response body. Blocking!
                GetAdultRacyScores(response.Content.ReadAsStringAsync().Result, out AdultScore, out RacyScore);
            }

            ReviewTags[0] = new KeyValuePair();
            ReviewTags[0].Key = "a";
            ReviewTags[0].Value = "false";
            if (AdultScore > 0.4)
            {
                ReviewTags[0].Value = "true";
            }

            ReviewTags[1] = new KeyValuePair();
            ReviewTags[1].Key = "r";
            ReviewTags[1].Value = "false";
            if (RacyScore > 0.3)
            {
                ReviewTags[1].Value = "true";
            }
            return response.IsSuccessStatusCode;
        }

## <a name="scan-for-celebrities"></a>Prüfen von Bildern auf Prominente

1. Registrieren Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) der [Maschinelles Sehen-API](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
2. Klicken Sie auf die Schaltfläche **API-Schlüssel abrufen**.
3. Akzeptieren Sie die Bedingungen.
4. Wählen Sie zur Anmeldung eine Option aus der Liste der verfügbaren Internetkonten aus.
5. Beachten Sie die API-Schlüssel, die auf Ihrer Dienstseite angezeigt werden.
    
   ![Schlüssel für die Maschinelles Sehen-API](images/tutorial-computer-vision-keys.PNG)
    
6. Informationen zur Funktion, um das Bild mit der Maschinelles Sehen-API zu prüfen, finden Sie im Projektquellcode.

         public static bool EvaluateComputerVisionTags(string ImageUrl, string ComputerVisionUri, string ComputerVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage Response = CallAPI(ComputerVisionUri, ComputerVisionKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (Response.IsSuccessStatusCode)
            {
                ReviewTags[2] = new KeyValuePair();
                ReviewTags[2].Key = "cb";
                ReviewTags[2].Value = "false";

                ComputerVisionPrediction CVObject = JsonConvert.DeserializeObject<ComputerVisionPrediction>(Response.Content.ReadAsStringAsync().Result);

                if ((CVObject.categories[0].detail != null) && (CVObject.categories[0].detail.celebrities.Count() > 0))
                {                 
                    ReviewTags[2].Value = "true";
                }
            }

            return Response.IsSuccessStatusCode;
        }

## <a name="classify-into-flags-toys-and-pens"></a>Klassifizieren von Flags, Spielzeugen und Stiften

1. [Melden](https://www.customvision.ai/account/signin) Sie sich bei der [Custom Vision-API (Vorschauversion)](https://www.customvision.ai/) an.
2. Erstellen Sie mit dem [Schnellstart](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) Ihre benutzerdefinierte Klassifizierung, um die Erkennung des potenziellen Vorhandenseins von Flags, Spielzeugen und Stiften zu ermöglichen.
   ![Schulungsbilder zur Custom Vision-API](images/tutorial-ecommerce-custom-vision.PNG)
3. [Rufen Sie die Vorhersageendpunkt-URL](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api) für Ihre benutzerdefinierte Klassifizierung ab.
4. Verweisen Sie auf den Projektquellcode, um die Funktion anzuzeigen, die zur Überprüfung Ihres Bilds den benutzerdefinierten Klassifizierungsvorhersageendpunkt aufruft.

        public static bool EvaluateCustomVisionTags(string ImageUrl, string CustomVisionUri, string CustomVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(CustomVisionUri, CustomVisionKey, CallType.POST,
                                                   "Prediction-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // Parse the response body. Blocking!
                SaveCustomVisionTags(response.Content.ReadAsStringAsync().Result, ref ReviewTags);
            }
            return response.IsSuccessStatusCode;
        }       
 
## <a name="reviews-for-human-in-the-loop"></a>Überprüfung auf Human-in-the-Loop-Simulationen

1. In den vorherigen Abschnitten haben Sie eingehende Bilder auf nicht jugendfreie und freizügige Inhalte (Content Moderator), Prominente (Maschinelles Sehen) und Flags (Custom Vision) überprüft.
2. Stellen Sie basierend auf unseren Übereinstimmungsschwellenwerten für die einzelnen Überprüfungen differenzierte Fälle für die durch Menschen durchgeführte Überprüfungen im Überprüfungstool zur Verfügung.
        public static bool CreateReview(string ImageUrl, KeyValuePair[] Metadata) {

            ReviewCreationRequest Review = new ReviewCreationRequest();
            Review.Item[0] = new ReviewItem();
            Review.Item[0].Content = ImageUrl;
            Review.Item[0].Metadata = new KeyValuePair[MAXTAGSCOUNT];
            Metadata.CopyTo(Review.Item[0].Metadata, 0);

            //SortReviewItems(ref Review);

            string Body = JsonConvert.SerializeObject(Review.Item);

            HttpResponseMessage response = CallAPI(ReviewUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            return response.IsSuccessStatusCode;
        }

## <a name="submit-batch-of-images"></a>Senden von Bildern in Batches

1. Bei diesem Tutorial wird davon ausgegangen, dass ein Verzeichnis namens „C:Test“ mit einer Textdatei bestehend aus einer Liste von Bild-URLs vorhanden ist.
2. Der folgende Code überprüft das Vorhandensein der Datei und liest alle URLs im Arbeitsspeicher.
            // Suchen Sie nach einem Testverzeichnis mit einer Textdatei bestehend aus der Liste der Bild-URLs, um Folgendes zu überprüfen: var topdir = @"C:\test\"; var Urlsfile = topdir +"Urls.txt";

            if (!Directory.Exists(topdir))
                return;

            if (!File.Exists(Urlsfile))
            {
                return;
            }

            // Read all image URLs in the file
            var Urls = File.ReadLines(Urlsfile);

## <a name="initiate-all-scans"></a>Initiieren aller Überprüfungen

1. Diese Funktion der obersten Ebene führt Schleifen durch alle Bild-URLs in der bereits erwähnten Textdatei aus.
2. Diese werden mit den einzelnen APIs überprüft und wenn die Zuverlässigkeitsbewertung unseren Kriterien entspricht, wird eine Überprüfung durch menschliche Moderatoren durchgeführt.
             // Für jede Bild-URL in der Datei: foreach (var Url in Urls) { // Initiieren Sie ein neues Array mit Überprüfungstags: ReviewTags = new KeyValuePair[MAXTAGSCOUNT];

                // Evaluate for potential adult and racy content with Content Moderator API
                EvaluateAdultRacy(Url, ref ReviewTags);

                // Evaluate for potential presence of celebrity (ies) in images with Computer Vision API
                EvaluateComputerVisionTags(Url, ComputerVisionUri, ComputerVisionKey, ref ReviewTags);

                // Evaluate for potential presence of custom categories other than Marijuana
                EvaluateCustomVisionTags(Url, CustomVisionUri, CustomVisionKey, ref ReviewTags);

                // Create review in the Content Moderator review tool
                CreateReview(Url, ReviewTags);
            }

## <a name="license"></a>Lizenz

Alle SDKs und Beispiele zu Microsoft Cognitive Services sind mit der MIT-Lizenz lizenziert. Weitere Informationen finden Sie unter [The MIT License (MIT)](https://microsoft.mit-license.org/) (Die MIT-Lizenz (MIT)).

## <a name="developer-code-of-conduct"></a>Verhaltensregeln für Entwickler

Von Entwicklern, die Cognitive Services (einschließlich dieser Clientbibliothek und der Beispiele) verwenden, wird erwartet, dass sie die Verhaltensregeln zu Microsoft Cognitive Services für Entwickler („Developer Code of Conduct for Microsoft Cognitive Services“) unter http://go.microsoft.com/fwlink/?LinkId=698895 befolgen.

## <a name="next-steps"></a>Nächste Schritte

Vertiefen Sie Ihre Kenntnisse im Rahmen des Tutorials mithilfe der [Projektquelldateien](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) auf GitHub.

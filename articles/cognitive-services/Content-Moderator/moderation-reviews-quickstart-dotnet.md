---
title: Azure Content Moderator – Erstellen von Überprüfungen per .NET | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Überprüfungen mit dem Azure Content Moderator SDK für .NET erstellen.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 6a0ff48f4ea17f9c800f3e6c096df2492699f87a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373347"
---
# <a name="create-reviews-using-net"></a>Erstellen von Berichten mit .NET

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des Content Moderator SDK für .NET erleichtern, um Folgendes durchzuführen:
 
- Erstellen einer Gruppe von Überprüfungen für menschliche Moderatoren
- Abrufen des Status von vorhandenen Überprüfungen für menschliche Moderatoren

Im Allgemeinen durchläuft der Inhalt in irgendeiner Form eine automatisierte Moderation, bevor dieser für die manuelle Prüfung geplant ist. In diesem Artikel wird lediglich beschrieben, wie die Überprüfung für die manuelle Moderation erstellt wird. Eine umfassenderes Szenario wird in den Tutorials [Moderation von Facebook-Inhalten](facebook-post-moderation.md) und [Moderation von E-Commerce-Katalogen](ecommerce-retail-catalog-moderation.md) beschrieben.

In diesem Artikel wird davon ausgegangen, dass Sie bereits mit Visual Studio und C# vertraut sind.

## <a name="sign-up-for-content-moderator-services"></a>Registrieren für Content Moderator-Dienste

Um Content Moderator-Dienste über die REST-API oder über das SDK verwenden zu können, benötigen Sie einen Abonnementschlüssel.
Informationen zum Abrufen des Schlüssels finden Sie in [diesem Schnellstart](quick-start.md).

## <a name="create-your-visual-studio-project"></a>Erstellen Ihres Visual Studio-Projekts

1. Fügen Sie Ihrer Projektmappe ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** hinzu.

   Versehen Sie das Projekt im Beispielcode mit dem Namen **CreateReviews**.

1. Wählen Sie dieses Projekt als einzelnes Startprojekt für die Projektmappe aus.

1. Fügen Sie einen Verweis auf die Projektassembly **ModeratorHelper** hinzu, die Sie im Schnellstart [Hilfscode zum Zurückgeben eines Content Moderator-Clients](content-moderator-helper-quickstart-dotnet.md) erstellt haben.

### <a name="install-required-packages"></a>Installieren erforderlicher Pakete

Installieren Sie die folgenden NuGet-Pakete:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Aktualisieren der using-Anweisungen des Programms

Ändern Sie die using-Anweisungen des Programms.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>Erstellen einer Klasse zum Zuordnen interner Inhaltsinformationen mit der Überprüfungs-ID

Fügen Sie der **Program**-Klasse die folgende Klasse hinzu:
Verwenden Sie diese Klasse, um die Überprüfungs-ID Ihrer internen Inhalts-ID für das Element zuzuordnen.

    /// <summary>
    /// Associates the review ID (assigned by the service) to the internal
    /// content ID of the item.
    /// </summary>
    public class ReviewItem
    {
        /// <summary>
        /// The media type for the item to review.
        /// </summary>
        public string Type;

        /// <summary>
        /// The URL of the item to review.
        /// </summary>
        public string Url;

        /// <summary>
        /// The internal content ID for the item to review.
        /// </summary>
        public string ContentId;

        /// <summary>
        /// The ID that the service assigned to the review.
        /// </summary>
        public string ReviewId;
    }

### <a name="initialize-application-specific-settings"></a>Initialisieren anwendungsspezifischer Einstellungen

> [!NOTE]
> Die RPS-Rate (Requests Per Second, Anforderungen pro Sekunde) Ihres Content Moderator-Dienstschlüssels ist begrenzt. Bei Überschreitung des Limits löst das SDK eine Ausnahme mit dem Fehlercode 429 aus. 
>
> Bei einem Schlüssel des Free-Tarifs ist die Anforderungsrate auf eine einzelne Anforderung pro Sekunde beschränkt.

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Fügen Sie der Klasse **Program** in „Program.cs“ die folgenden Konstanten hinzu.
    
    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const double latencyDelay = 45;

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Fügen Sie der Klasse **Program** in „Program.cs“ die folgenden Konstanten und statischen Felder hinzu.

Aktualisieren Sie diese Werte, indem Sie Informationen speziell für Ihr Abonnement und Ihr Team eingeben.

> [!NOTE]
> Legen Sie für die TeamName-Konstante den Namen fest, den Sie bei der Erstellung Ihres Abonnements zum [Content Moderator-Prüfungstool](https://contentmoderator.cognitive.microsoft.com/) verwendet haben. Die TeamName-Konstante kann über das Menü **Einstellungen** (Zahnradsymbol) im Abschnitt **Anmeldeinformationen** abgerufen werden.
>
> Ihr Teamname ist der Wert des Felds **ID** im Abschnitt **API**.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "{teamname}";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "{callbackUrl}";

    /// <summary>
    /// The media type for the item to review.
    /// </summary>
    /// <remarks>Valid values are "image", "text", and "video".</remarks>
    private const string MediaType = "image";

    /// <summary>
    /// The URLs of the images to create review jobs for.
    /// </summary>
    private static readonly string[] ImageUrls = new string[] {
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg"
        // add more if you want
    };

    /// <summary>
    /// The metadata key to initially add to each review item.
    /// </summary>
    private const string MetadataKey = "sc";

    /// <summary>
    /// The metadata value to initially add to each review item.
    /// </summary>
    private const string MetadataValue = "true";

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Fügen Sie der Klasse **Program** in „Program.cs“ die folgenden statischen Felder hinzu.

Verwenden Sie diese Felder, um den Anwendungszustand nachzuverfolgen.

    /// <summary>
    /// A static reference to the text writer to use for logging.
    /// </summary>
    private static TextWriter writer;

    /// <summary>
    /// The cached review information, associating a local content ID
    /// to the created review ID for each item.
    /// </summary>
    private static List<ReviewItem> reviewItems =
        new List<ReviewItem>();

## <a name="create-a-method-to-write-messages-to-the-log-file"></a>Erstellen einer Methode zum Schreiben von Nachrichten in die Protokolldatei

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. 

    /// <summary>
    /// Writes a message to the log file, and optionally to the console.
    /// </summary>
    /// <param name="message">The message.</param>
    /// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
    private static void WriteLine(string message = null, bool echo = false)
    {
        writer.WriteLine(message ?? String.Empty);

        if (echo)
        {
            Console.WriteLine(message ?? String.Empty);
        }
    }

## <a name="create-a-method-to-create-a-set-of-reviews"></a>Erstellen einer Methode zum Erstellen einer Gruppe von Überprüfungen

In der Regel muss ein Teil Ihrer Geschäftslogik für die Identifizierung von eingehenden Bildern, Texten oder Videos überprüft werden. Verwenden Sie in diesem Fall allerdings nur eine feste Liste von Bildern.

Fügen Sie der **Program**-Klasse die folgende Methode hinzu.

    /// <summary>
    /// Create the reviews using the fixed list of images.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void CreateReviews(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Creating reviews for the following images:", true);

        // Create the structure to hold the request body information.
        List<CreateReviewBodyItem> requestInfo =
            new List<CreateReviewBodyItem>();

        // Create some standard metadata to add to each item.
        List<CreateReviewBodyItemMetadataItem> metadata =
            new List<CreateReviewBodyItemMetadataItem>(
            new CreateReviewBodyItemMetadataItem[] {
                new CreateReviewBodyItemMetadataItem(
                    MetadataKey, MetadataValue)
            });

        // Populate the request body information and the initial cached review information.
        for (int i = 0; i < ImageUrls.Length; i++)
        {
            // Cache the local information with which to create the review.
            var itemInfo = new ReviewItem()
            {
                Type = MediaType,
                ContentId = i.ToString(),
                Url = ImageUrls[i],
                ReviewId = null
            };

            WriteLine($" - {itemInfo.Url}; with id = {itemInfo.ContentId}.", true);

            // Add the item informaton to the request information.
            requestInfo.Add(new CreateReviewBodyItem(
                itemInfo.Type, itemInfo.Url, itemInfo.ContentId,
                CallbackEndpoint, metadata));

            // Cache the review creation information.
            reviewItems.Add(itemInfo);
        }

        var reviewResponse = client.Reviews.CreateReviewsWithHttpMessagesAsync(
            "application/json", TeamName, requestInfo);

        // Update the local cache to associate the created review IDs with
        // the associated content.
        var reviewIds = reviewResponse.Result.Body;
        for (int i = 0; i < reviewIds.Count; i++)
        {
            Program.reviewItems[i].ReviewId = reviewIds[i];
        }

        WriteLine(JsonConvert.SerializeObject(
        reviewIds, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Erstellen einer Methode zum Abrufen des Status von vorhandenen Überprüfungen

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. 

> [!Note]
> In der Praxis würden Sie die Rückruf-URL `CallbackEndpoint` auf die URL festlegen, die die Ergebnisse der manuelle Überprüfung (über eine HTTP-POST-Anforderung) empfangen würde.
> Sie können diese Methode ändern, um den Status von ausstehenden Überprüfungen zu überprüfen.

    /// <summary>
    /// Gets the review details from the server.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void GetReviewDetails(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Getting review details:", true);
        foreach (var item in reviewItems)
        {
            var reviewDetail = client.Reviews.GetReviewWithHttpMessagesAsync(
                TeamName, item.ReviewId);

            WriteLine(
                $"Review {item.ReviewId} for item ID {item.ContentId} is " +
                $"{reviewDetail.Result.Body.Status}.", true);
            WriteLine(JsonConvert.SerializeObject(
                reviewDetail.Result.Body, Formatting.Indented));

            Thread.Sleep(throttleRate);
        }
    }

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>Hinzufügen von Code zum Erstellen von Überprüfungen und Überprüfen des Status

Fügen Sie der Methode **Main** den folgenden Code hinzu.

Dieser Code simuliert viele Vorgänge, die Sie zur Definition und Verwaltung der Liste ausführen, sowie die Verwendung der Liste zum Überprüfen von Bildern. Mit den Protokollierungsfunktionen können Sie die Antwortobjekte anzeigen, die von den SDK-Aufrufen des Content Moderator-Diensts generiert werden.

    using (TextWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        writer = outputWriter;
        using (var client = Clients.NewClient())
        {
            CreateReviews(client);
            GetReviewDetails(client);

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propigate.");
            Thread.Sleep(latencyDelay * 1000);

            GetReviewDetails(client);
        }

        writer = null;
        outputWriter.Flush();
        outputWriter.Close();
    }

    Console.WriteLine();
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();

## <a name="run-the-program-and-review-the-output"></a>Ausführen des Programms und Überprüfen der Ausgabe

Im Folgenden wird eine Beispielausgabe angezeigt:

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Melden Sie sich beim Content Moderator-Prüfungstool an, um die ausstehende Bildüberprüfung anzuzeigen. Die Bezeichnung **sc** muss hierbei auf **true** festgelegt sein. Darüber hinaus sehen Sie die Standardtags **a** und **r** und benutzerdefinierte Tags, die Sie ggf. innerhalb des Prüfungstools definiert haben. 

Klicken Sie zum Senden auf die Schaltfläche **Weiter**.

![Bildüberprüfung für menschliche Moderatoren](images/moderation-reviews-quickstart-dotnet.PNG)

Drücken Sie eine beliebige Taste, um fortzufahren.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Sehen Sie sich die folgende Ausgabe in der Protokolldatei an.

> [!NOTE]
> In Ihrer Ausgabedatei enthalten die Zeichenfolgen „\{teamname}“ und „\{callbackUrl}“ die Werte für die Felder `TeamName` bzw. `CallbackEndpoint`.

Die Überprüfungs-IDs und die Bildinhalts-URLs ändern sich nach jeder Anwendungsausführung, und nach einer Überprüfung zeigt das Feld `reviewerResultTags`, wie der Prüfer das Element gekennzeichnet hat.

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.
    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Pending",
        "reviewerResultTags": [],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="your-callback-url-if-provided-receives-this-response"></a>Die von Ihrer Rückruf-URL (sofern angegeben) empfangene Antwort

Es wird eine Antwort ähnlich wie im folgenden Beispiel angezeigt:

    {
        "ReviewId": "201801i48a2937e679a41c7966e838c92f5e649",
        "ModifiedOn": "2018-01-06T05:04:40.5525865Z",
        "ModifiedBy": "yourusername",
        "CallBackType": "Review",
        "ContentId": "0",
        "ContentType": "Image",
        "Metadata": {
            "sc": "true"
            },
        "ReviewerResultTags": {
            "a": "False",
            "r": "False",
        }
    }


## <a name="next-steps"></a>Nächste Schritte

[Laden Sie die entsprechende Visual Studio-Projektmappe sowie andere Content Moderator-Schnellstarts für .NET herunter](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator), und beginnen Sie mit der Integration.

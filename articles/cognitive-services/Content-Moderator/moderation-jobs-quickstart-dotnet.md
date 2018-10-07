---
title: 'Schnellstart: Starten von Moderationsaufträgen mit .NET – Content Moderator'
titlesuffix: Azure Cognitive Services
description: In diesem Artikel wird beschrieben, wie Moderationsaufträge mithilfe des Azure Content Moderator SDK für .NET initiiert werden.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: 6045d6daf2abace6e2b38bd6fd6e22516e3a60a0
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227430"
---
# <a name="quickstart-start-moderation-jobs-using-net"></a>Schnellstart: Starten von Moderationsaufträgen mit .NET

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des [Content Moderator SDK für .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) erleichtern, um Folgendes durchzuführen:
 
- Starten eines Moderationsauftrags zum Prüfen und Erstellen von Überprüfungen durch menschliche Moderatoren
- Abrufen des Status einer ausstehenden Überprüfung
- Nachverfolgen und Abrufen des endgültigen Status der Überprüfung
- Senden des Ergebnisses an die Rückruf-URL

In diesem Artikel wird davon ausgegangen, dass Sie bereits mit Visual Studio und C# vertraut sind.

## <a name="sign-up-for-content-moderator"></a>Registrieren für Content Moderator

Um Content Moderator-Dienste über die REST-API oder über das SDK verwenden zu können, benötigen Sie einen Abonnementschlüssel.
Informationen zum Abrufen des Schlüssels finden Sie in [diesem Schnellstart](quick-start.md).

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Registrieren für ein Überprüfungstoolkonto, falls dies nicht bereits im vorhergehenden Schritt geschehen ist

Wenn Sie Ihren Content Moderator über das Azure-Portal erhalten haben, müssen Sie sich auch [für das Überprüfungstoolkonto registrieren](https://contentmoderator.cognitive.microsoft.com/) und ein Überprüfungsteam erstellen. Sie benötigen die Team-ID und das Überprüfungstool, um die Überprüfungs-API aufzurufen, einen Auftrag zu starten sowie die Überprüfungen im Tool anzuzeigen.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>Sicherstellen, dass Ihr API-Schlüssel die Überprüfungs-API zur Erstellung der Überprüfung aufrufen kann

Nach Abschluss der vorhergehenden Schritte verfügen Sie möglicherweise über zwei Content Moderator-Schlüssel, wenn Sie über das Azure-Portal begonnen haben. 

Wenn Sie den von Azure bereitgestellten API-Schlüssel in Ihrem SDK-Beispiel verwenden möchten, führen Sie die Schritte im Abschnitt [Verwenden des Azure-Schlüssels bei der Überprüfungs-API](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api) aus, damit Ihre Anwendung die Überprüfungs API aufrufen und Überprüfungen erstellen kann.

Wenn Sie den vom Überprüfungstool generierten Schlüssel für eine kostenlose Testversion verwenden, ist der Schlüssel dem Überprüfungstoolkonto bereits bekannt. Deshalb sind keine weiteren Schritte erforderlich.

## <a name="define-a-custom-moderation-workflow"></a>Definieren eines benutzerdefinierten Moderationsworkflows

Ein Moderationsauftrag überprüft Ihren Inhalt mithilfe der APIs und ermittelt im Rahmen eines **Workflows**, ob Überprüfungen erstellt werden müssen.
Das Tool beinhaltet zwar einen Standardworkflow, für diesen Schnellstart wird jedoch [ein benutzerdefinierter Workflow definiert](Review-Tool-User-Guide/Workflows.md).

Verwenden Sie den Namen des Workflows in Ihrem Code, der den Moderationsauftrag startet.

## <a name="create-your-visual-studio-project"></a>Erstellen Ihres Visual Studio-Projekts

1. Fügen Sie Ihrer Projektmappe ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** hinzu.

   Versehen Sie das Projekt im Beispielcode mit dem Namen **CreateReviews**.

1. Wählen Sie dieses Projekt als einzelnes Startprojekt für die Projektmappe aus.

### <a name="install-required-packages"></a>Installieren erforderlicher Pakete

Installieren Sie die folgenden NuGet-Pakete:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Aktualisieren der using-Anweisungen des Programms

Ändern Sie die using-Anweisungen des Programms.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>Erstellen des Content Moderator-Clients

Fügen Sie den folgenden Code hinzu, um einen Content Moderator-Client für Ihr Abonnement zu erstellen.

> [!IMPORTANT]
> Aktualisieren Sie die Felder **AzureRegion** und **CMSubscriptionKey** mit den Werten Ihres Regionsbezeichners und des Abonnementschlüssels.


    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "YOUR API REGION";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"https://{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR API KEY";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.Endpoint = AzureBaseURL;
            return client;
        }
    }

### <a name="initialize-application-specific-settings"></a>Initialisieren anwendungsspezifischer Einstellungen

Fügen Sie der Klasse **Program** in „Program.cs“ die folgenden Konstanten und statischen Felder hinzu.

> [!NOTE]
> Legen Sie für die TeamName-Konstante den Namen fest, den Sie bei der Erstellung Ihres Content Moderator-Abonnements verwendet haben. Sie können die TeamName-Konstante von der [Website von Content Moderator](https://westus.contentmoderator.cognitive.microsoft.com/) beziehen.
> Nachdem Sie sich angemeldet haben, klicken Sie im Menü **Einstellungen** (Zahnradsymbol) auf **Anmeldeinformationen**.
>
> Ihr Teamname ist der Wert des Felds **ID** im Abschnitt **API**.


    /// <summary>
    /// The moderation job will use this workflow that you defined earlier.
    /// See the quickstart article to learn how to setup custom workflows.
    /// </summary>
    private const string WorkflowName = "OCR";
    
    /// <summary>
    /// The name of the team to assign the job to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Content Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "***";

    /// <summary>
    /// The URL of the image to create a review job for.
    /// </summary>
    private const string ImageUrl =
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png";

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Reviews show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "";

## <a name="add-code-to-auto-moderate-create-a-review-and-get-the-job-details"></a>Hinzufügen von Code zum automatischen Moderieren, Erstellen einer Überprüfung und Abrufen der Auftragsdetails

> [!Note]
> In der Praxis legen Sie die Rückruf-URL **CallbackEndpoint** auf die URL fest, die die Ergebnisse der manuelle Überprüfung (über eine HTTP-POST-Anforderung) empfängt.

Fügen Sie der Methode **Main** zunächst den folgenden Code hinzu.

    using (TextWriter writer = new StreamWriter(OutputFile, false))
    {
        using (var client = Clients.NewClient())
        {
            writer.WriteLine("Create review job for an image.");
            var content = new Content(ImageUrl);
        
            // The WorkflowName contains the name of the workflow defined in the online review tool.
            // See the quickstart article to learn more.
            var jobResult = client.Reviews.CreateJobWithHttpMessagesAsync(
                    TeamName, "image", "contentID", WorkflowName, "application/json", content, CallbackEndpoint);

            // Record the job ID.
            var jobId = jobResult.Result.Body.JobIdProperty;

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                jobResult.Result.Body, Formatting.Indented));

            Thread.Sleep(2000);
            writer.WriteLine();

            writer.WriteLine("Get review job status.");
            var jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
                    TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                    jobDetails.Result.Body, Formatting.Indented));

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propagate.");
            Thread.Sleep(latencyDelay * 1000);

            writer.WriteLine("Get review details.");
            jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
            TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
            jobDetails.Result.Body, Formatting.Indented));
        }
        writer.Flush();
        writer.Close();
    }

> [!NOTE]
> Für Ihren Content Moderator-Dienstschlüssel gilt ein Ratenlimit vom Typ „Anforderungen pro Sekunde“ (Requests Per Second, RPS). Wenn Sie das Limit überschreiten, löst das SDK eine Ausnahme mit dem Fehlercode 429 aus. 
>
> Bei einem Schlüssel des Free-Tarifs ist die Anforderungsrate auf eine einzelne Anforderung pro Sekunde beschränkt.

## <a name="run-the-program-and-review-the-output"></a>Ausführen des Programms und Überprüfen der Ausgabe

Im Folgenden wird eine Beispielausgabe in der Konsole angezeigt:

    Perform manual reviews on the Content Moderator site.
    Then, press any key to continue.

Melden Sie sich beim Content Moderator-Prüfungstool an, um die ausstehende Bildüberprüfung anzuzeigen.

Klicken Sie zum Senden auf die Schaltfläche **Weiter**.

![Bildüberprüfung für menschliche Moderatoren](images/ocr-sample-image.PNG)

## <a name="see-the-sample-output-in-the-log-file"></a>Anzeigen der Beispielausgabe in der Protokolldatei

> [!NOTE]
> In Ihrer Ausgabedatei enthalten die Zeichenfolgen **Teamname**, **ContentId**, **CallBackEndpoint** und **WorkflowId** die Werte, die Sie weiter oben verwendet haben.

    Create moderation job for an image.
    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

    Get review details.
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": [
        {
            "Ts": "2018-01-07T00:38:26.7714671",
            "Msg": "Successfully got hasText response from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:26.4181346",
            "Msg": "Getting hasText from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:25.5122828",
            "Msg": "Starting Execution - Try 1"
        }
        ]
    }


## <a name="your-callback-url-if-provided-receives-this-response"></a>Die von Ihrer Rückruf-URL (sofern angegeben) empfangene Antwort

Es wird eine Antwort ähnlich wie im folgenden Beispiel angezeigt:

> [!NOTE]
> In der Antwort auf Ihren Rückruf enthalten die Zeichenfolgen **ContentId** und **WorkflowId** die Werte, die Sie zuvor verwendet haben.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe",
        "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
        "WorkFlowId": "OCR",
        "Status": "Complete",
        "ContentType": "Image",
        "CallBackType": "Job",
        "ContentId": "contentID",
        "Metadata": {
            "hastext": "True",
            "ocrtext": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
            "imagename": "contentID"
        }
    }


## <a name="next-steps"></a>Nächste Schritte

Rufen Sie das [Content Moderator .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) und die [Visual Studio-Projektmappe](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) dafür sowie andere Content Moderator-Schnellstarts für .NET ab, und beginnen Sie mit der Integration.

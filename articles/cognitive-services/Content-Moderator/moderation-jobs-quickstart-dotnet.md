---
title: Azure Content Moderator – Starten von Moderationsaufträgen mit .NET | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, Moderationsaufträge mithilfe des Azure Content Moderator SDK für .NET initiiert werden.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/06/2018
ms.author: sajagtap
ms.openlocfilehash: a103875607355993e216ce1ddea02009fc8fa1c4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373154"
---
# <a name="start-moderation-jobs-using-net"></a>Starten von Moderationsaufträgen mit .NET

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des Content Moderator SDK für .NET erleichtern, um Folgendes durchzuführen:
 
- Starten eines Moderationsauftrags zum Prüfen und Erstellen von Überprüfungen durch menschliche Moderatoren
- Abrufen des Status einer ausstehenden Überprüfung
- Nachverfolgen und Abrufen des endgültigen Status der Überprüfung
- Senden des Ergebnisses an die Rückruf-URL

In diesem Artikel wird davon ausgegangen, dass Sie bereits mit Visual Studio und C# vertraut sind.

## <a name="sign-up-for-content-moderator-services"></a>Registrieren für Content Moderator-Dienste

Um Content Moderator-Dienste über die REST-API oder über das SDK verwenden zu können, benötigen Sie einen Abonnementschlüssel.
Informationen zum Abrufen des Schlüssels finden Sie in [diesem Schnellstart](quick-start.md).

## <a name="define-a-custom-moderation-workflow"></a>Definieren eines benutzerdefinierten Moderationsworkflows

Ein Moderationsauftrag überprüft Ihren Inhalt mithilfe der APIs und ermittelt im Rahmen eines **Workflows**, ob Überprüfungen erstellt werden müssen.
Das Tool beinhaltet zwar einen Standardworkflow, für diesen Schnellstart wird jedoch [ein benutzerdefinierter Workflow definiert](Review-Tool-User-Guide/Workflows.md).

Verwenden Sie den Namen des Workflows in Ihrem Code, der den Moderationsauftrag startet.

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
    /// the Conent Moderator web site. Your team name is the Id associated 
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
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
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
        
            // The WorkflowName contains the nameof the workflow defined in the online review tool.
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

[Laden Sie die entsprechende Visual Studio-Projektmappe sowie andere Content Moderator-Schnellstarts für .NET herunter](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator), und beginnen Sie mit der Integration.

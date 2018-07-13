---
title: Azure Content Moderator – Videomoderation | Microsoft-Dokumentation
description: Verwenden Sie die Videomoderation, um Scans auf mögliche nicht jugendfreie und anzügliche Inhalte durchzuführen.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/02/2018
ms.author: sajagtap
ms.openlocfilehash: ef58f5990d4a0a19ab2b8c61b42ab2a0754dc6fa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377691"
---
# <a name="video-moderation"></a>Videomoderation

Heutzutage generieren Online-Viewer Milliarden von Videoaufrufen auf beliebten und regionalen Websites von sozialen Medien. Durch das Anwenden von Diensten, die auf maschinellem Lernen basieren, um mögliche nicht jugendfreie und anzügliche Inhalte vorherzusagen, können Sie Ihre Moderationskosten senken.

## <a name="sign-up-for-the-content-moderator-media-processor-public-preview"></a>Anmelden für den Content Moderator-Medienprozessor (öffentliche Vorschauversion)

### <a name="create-a-free-azure-account"></a>Erstellen eines kostenlosen Azure-Kontos

[Beginnen Sie hier](https://azure.microsoft.com/free/) damit, ein kostenloses Azure-Konto einzurichten, falls Sie noch keines besitzen.

### <a name="create-an-azure-media-services-account"></a>Erstellen eines Azure Media Services-Kontos

Die Videofunktion von Content Moderator ist als **Medienprozessor** in der öffentlichen Vorschauversion in Azure Media Services (AMS) kostenlos verfügbar.

[Erstellen Sie ein Azure Media Services-Konto](https://docs.microsoft.com/azure/media-services/media-services-portal-create-account) in Ihrem Azure-Abonnement.

### <a name="get-azure-active-directory-credentials"></a>Erhalten der Azure Active Directory-Anmeldeinformationen

   1. Lesen Sie den Artikel [Erste Schritte mit der Azure AD-Authentifizierung mithilfe des Azure-Portals](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad), um zu erfahren, wie Sie das Azure-Portal verwenden können, um Ihre Azure AD-Anmeldeinformationen zu erhalten.
   1. Lesen Sie den Artikel [Verwenden der Azure AD-Authentifizierung zum Zugreifen auf die Azure Media Services-API mit .NET](https://docs.microsoft.com/azure/media-services/media-services-dotnet-get-started-with-aad), um zu erfahren, wie Sie Ihre Azure Active Directory-Anmeldeinformationen mit dem .NET SDK verwenden.

   > [!NOTE]
   > Der Beispielcode in diesem Schnellstart verwendet die Methode der **Dienstprinzipalauthentifizierung**, die in beiden Artikeln beschrieben wird.

Sobald Sie Ihre AMS-Anmeldeinformationen erhalten haben, gibt es zwei Möglichkeiten, um den Content Moderator-Medienprozessor zu testen.

## <a name="use-azure-media-services-explorer"></a>Verwenden des Azure Media Services-Explorers

Verwenden Sie den interaktiven [Azure Media Services-Explorer (AMS)](https://azure.microsoft.com/blog/managing-media-workflows-with-the-new-azure-media-services-explorer-tool/), um Ihr AMS-Konto zu durchsuchen, Videos hochzuladen und Scans mit dem Content Moderator-Medienprozessor durchzuführen. [Laden Sie diesen von GitHub herunter](https://github.com/Azure/Azure-Media-Services-Explorer/releases), installieren Sie ihn, und [durchsuchen Sie den Quellcode](http://github.com/Azure/Azure-Media-Services-Explorer), um in die Verwendung des AMS SDK einzusteigen.

![Azure Media Services-Explorer mit Content Moderator](images/ams-explorer-content-moderator.PNG)

## <a name="net-quickstart-with-visual-studio-and-c"></a>.NET-Schnellstart mit Visual Studio und C#

1. Fügen Sie Ihrer Projektmappe ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** hinzu.

   Versehen Sie das Projekt im Beispielcode mit dem Namen **VideoModeration**.

1. Wählen Sie dieses Projekt als einzelnes Startprojekt für die Projektmappe aus.

### <a name="install-required-packages"></a>Installieren erforderlicher Pakete

Installieren Sie die folgenden NuGet-Pakete, die auf [NuGet](https://www.nuget.org/) verfügbar sind.

- windowsazure.mediaservices
- windowsazure.mediaservices.extensions

### <a name="update-the-programs-using-statements"></a>Aktualisieren der using-Anweisungen des Programms

Ändern Sie die using-Anweisungen des Programms.

    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.IO;
    using System.Threading;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using System.Collections.Generic;


### <a name="initialize-application-specific-settings"></a>Initialisieren anwendungsspezifischer Einstellungen

Fügen Sie der Klasse **Program** in „Program.cs“ die folgenden statischen Felder hinzu.

    // declare constants and globals
    private static CloudMediaContext _context = null;
    private static CloudStorageAccount _StorageAccount = null;

    // Azure Media Services (AMS) associated Storage Account, Key, and the Container that has 
    // a list of Blobs to be processed.
    static string STORAGE_NAME = "YOUR AMS ASSOCIATED BLOB STORAGE NAME";
    static string STORAGE_KEY = "YOUR AMS ASSOCIATED BLOB STORAGE KEY";
    static string STORAGE_CONTAINER_NAME = "YOUR BLOB CONTAINER FOR VIDEO FILES";

    private static StorageCredentials _StorageCredentials = null;

    // Azure Media Services authentication. See the quickstart for how to get these.
    private const string AZURE_AD_TENANT_NAME = "microsoft.onmicrosoft.com";
    private const string CLIENT_ID = "YOUR CLIENT ID"
    private const string CLIENT_SECRET = "YOUR CLIENT SECRET";

    // REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
    private const string REST_API_ENDPOINT = "YOUR API ENDPOINT";

    // Content Moderator Media Processor Nam
    private const string MEDIA_PROCESSOR = "Azure Media Content Moderator";

    // Input and Output files in the current directory of the executable
    private const string INPUT_FILE = "VIDEO FILE NAME";
    private const string OUTPUT_FOLDER = "";

### <a name="create-a-preset-file-json"></a>Erstellen einer Voreinstellungsdatei (JSON)

Erstellen Sie eine JSON-Datei im aktuellen Verzeichnis mit der Versionsnummer.

    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";
    //Example file content:
    //        {
    //             "version": "2.0"
    //        }
    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";

### <a name="add-the-following-code-to-the-main-method"></a>Code zum Hinzufügen zur Main-Methode

Die Main-Methode erstellt zunächst einen Azure-Medienkontext und anschließend einen Azure-Speicherkontext, falls Ihre Videos sich in Blob Storage befinden.
Der restliche Code überprüft ein Video aus einem lokalen Ordner, einem Blob oder mehreren Blogs innerhalb eines Azure-Speichercontainers.
Sie können alle Optionen testen, indem Sie die anderen Codezeilen auskommentieren.

    // Create Azure Media Context
    CreateMediaContext();

    // Create Storage Context
    CreateStorageContext();

    // Use a file as the input.
    // IAsset asset = CreateAssetfromFile();
    
    // -- OR ---
    
    // Or a blob as the input
    // IAsset asset = CreateAssetfromBlob((CloudBlockBlob)GetBlobsList().First());

    // Then submit the asset to Content Moderator
    // RunContentModeratorJob(asset);

    //-- OR ----

    // Just run the content moderator on all blobs in a list (from a Blob Container)
    RunContentModeratorJobOnBlobs();

### <a name="add-the-code-to-create-an-azure-media-context"></a>Code zum Erstellen eines Azure-Medienkontexts

    /// <summary>
    /// Creates a media context from azure credentials
    /// </summary>
    static void CreateMediaContext()
    {
        // Get Azure AD credentials
        var tokenCredentials = new AzureAdTokenCredentials(AZURE_AD_TENANT_NAME,
            new AzureAdClientSymmetricKey(CLIENT_ID, CLIENT_SECRET),
            AzureEnvironments.AzureCloudEnvironment);

        // Initialize an Azure AD token
        var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

        // Create a media context
        _context = new CloudMediaContext(new Uri(REST_API_ENDPOINT), tokenProvider);
    }

### <a name="add-the-code-to-create-an-azure-storage-context"></a>Code zum Erstellen eines Azure-Speicherkontexts
Sie verwenden den Speicherkontext, der mithilfe Ihrer Storage-Anmeldeinformationen erstellt wurden, um auf Blob Storage zuzugreifen.

    /// <summary>
    /// Creates a storage context from the AMS associated storage name and key
    /// </summary>
    static void CreateStorageContext()
    {
        // Get a reference to the storage account associated with a Media Services account. 
        if (_StorageCredentials == null)
        {
            _StorageCredentials = new StorageCredentials(STORAGE_NAME, STORAGE_KEY);
        }
        _StorageAccount = new CloudStorageAccount(_StorageCredentials, false);
    }

### <a name="add-the-code-to-create-azure-media-assets-from-local-file-and-blob"></a>Code zum Erstellen von Azure-Medienobjekten aus lokalen Dateien und Blobs
Der Content Moderator-Medienprozessor führt Aufträge für **Objekte** innerhalb der Azure Media Services-Plattform aus.
Diese Methoden erstellen die Objekte aus einer lokalen Datei oder einem zugeordneten Blob.

    /// <summary>
    /// Creates an Azure Media Services Asset from the video file
    /// </summary>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromFile()
    {
        return _context.Assets.CreateFromFile(INPUT_FILE, AssetCreationOptions.None); ;
    }

    /// <summary>
    /// Creates an Azure Media Services asset from your blog storage
    /// </summary>
    /// <param name="Blob"></param>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromBlob(CloudBlockBlob Blob)
    {
        // Create asset from the FIRST blob in the list and return it
        return _context.Assets.CreateFromBlob(Blob, _StorageCredentials, AssetCreationOptions.None);
    }

### <a name="add-the-code-to-scan-a-collection-of-videos-as-blobs-within-a-container"></a>Code zum Überprüfen mehrerer Videos (als Blobs) innerhalb eines Containers

    /// <summary>
    /// Runs the Content Moderator Job on all Blobs in a given container name
    /// </summary>
    static void RunContentModeratorJobOnBlobs()
    {
        // Get the reference to the list of Blobs. See the following method.
        var blobList = GetBlobsList();

        // Iterate over the Blob list items or work on specific ones as needed
        foreach (var sourceBlob in blobList)
        {
            // Create an Asset
            IAsset asset = _context.Assets.CreateFromBlob((CloudBlockBlob)sourceBlob,
                               _StorageCredentials, AssetCreationOptions.None);
            asset.Update();

            // Submit to Content Moderator
            RunContentModeratorJob(asset);
        }
    }

    /// <summary>
    /// Get all blobs in your container
    /// </summary>
    /// <returns></returns>
    static IEnumerable<IListBlobItem> GetBlobsList()
    {
        // Get a reference to the Container within the Storage Account
        // that has the files (blobs) for moderation
        CloudBlobClient CloudBlobClient = _StorageAccount.CreateCloudBlobClient();
        CloudBlobContainer MediaBlobContainer = CloudBlobClient.GetContainerReference(STORAGE_CONTAINER_NAME);

        // Get the reference to the list of Blobs 
        var blobList = MediaBlobContainer.ListBlobs();
        return blobList;
    }

### <a name="add-the-method-to-run-the-content-moderator-job"></a>Methode zum Hinzufügen des Content Moderator-Auftrags

    /// <summary>
    /// Run the Content Moderator job on the designated Asset from local file or blob storage
    /// </summary>
    /// <param name="asset"></param>
    static void RunContentModeratorJob(IAsset asset)
    {
        // Grab the presets
        string configuration = File.ReadAllText(CONTENT_MODERATOR_PRESET_FILE);

        // grab instance of Azure Media Content Moderator MP
        IMediaProcessor mp = _context.MediaProcessors.GetLatestMediaProcessorByName(MEDIA_PROCESSOR);

        // create Job with Content Moderator task
        IJob job = _context.Jobs.Create(String.Format("Content Moderator {0}",
                asset.AssetFiles.First() + "_" + Guid.NewGuid()));

        ITask contentModeratorTask = job.Tasks.AddNew("Adult and racy classifier task",
                mp, configuration,
                TaskOptions.None);
        contentModeratorTask.InputAssets.Add(asset);
        contentModeratorTask.OutputAssets.AddNew("Adult and racy classifier output",
            AssetCreationOptions.None);

        job.Submit();


        // Create progress printing and querying tasks
        Task progressPrintTask = new Task(() =>
        {
            IJob jobQuery = null;
            do
            {
                var progressContext = _context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                    .First();
                    Console.WriteLine(string.Format("{0}\t{1}",
                    DateTime.Now,
                    jobQuery.State));
                    Thread.Sleep(10000);
             }
             while (jobQuery.State != JobState.Finished &&
             jobQuery.State != JobState.Error &&
             jobQuery.State != JobState.Canceled);
        });
        progressPrintTask.Start();

        Task progressJobTask = job.GetExecutionProgressTask(
        CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling 
        // method for job progress should log errors.  Here we check 
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            ErrorDetail error = job.Tasks.First().ErrorDetails.First();
            Console.WriteLine(string.Format("Error: {0}. {1}",
            error.Code,
            error.Message));
        }

        DownloadAsset(job.OutputMediaAssets.First(), OUTPUT_FOLDER);
    }

### <a name="add-a-couple-of-helper-functions"></a>Hinzufügen von Hilfsfunktionen

Diese Methoden laden die Content Moderator-Ausgabedatei (JSON) von der Azure Media Services-Ressource herunter, und verfolgen den Status des Moderationsauftrags, damit das Programm einen Ausführungsstatus in der Konsole protokollieren kann.

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    // event handler for Job State
    static void StateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine("Job state changed event:");
        Console.WriteLine("  Previous state: " + e.PreviousState);
        Console.WriteLine("  Current state: " + e.CurrentState);
        switch (e.CurrentState)
        {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job finished.");
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
                Console.WriteLine("Job is canceled.\n");
                break;
            case JobState.Error:
                Console.WriteLine("Job failed.\n");
                break;
            default:
                break;
        }
    }

### <a name="run-the-program-and-review-the-output"></a>Ausführen des Programms und Überprüfen der Ausgabe

Nachdem der Content Moderator-Auftrag abgeschlossen ist, analysieren Sie die JSON-Antwort. Diese besteht aus folgenden Elementen:

- Zusammenfassung der Videoinformationen
- **Screenshots** als "**fragments**"
- **Keyframes** als "**events**" mit dem Flag **reviewRecommended" (= true or false)"**, das auf **adultScore** und **racyScore** basiert.
- **start**, **duration**, **totalDuration** und **timestamp** werden in „Takten“ angegeben. Teilen Sie diese Werte durch **timescale**, um die Anzahl in Sekunden zu erhalten.
 
> [!NOTE]

> - `adultScore` gibt das mögliche Vorhandensein und die Vorhersage von Inhalten an, die in bestimmten Situationen als explizit sexuell oder nicht jugendfrei betrachtet werden können.
> - `racyScore` gibt das mögliche Vorhandensein und die Vorhersage von Inhalten an, die in bestimmten Situationen als zweideutig/freizügig oder als für Erwachsene bestimmt betrachtet werden können.
> - `adultScore` und `racyScore` liegen zwischen 0 und 1. Je höher der Wert ist, desto höher ist die vom Modell ermittelte Wahrscheinlichkeit, dass die Kategorie zutreffend ist. Diese Vorschauversion basiert nicht auf manuell programmierten Ergebnissen, sondern auf einem statistischen Modell. Es wird empfohlen, anhand Ihrer eigenen Inhalte zu testen, wie die jeweiligen Kategorien zu Ihren Anforderungen passen.
> - `reviewRecommended` ist entweder TRUE oder FALSE (abhängig von den internen Ergebnisschwellenwerten). Kunden müssen entscheiden, ob sie diesen Wert verwenden oder auf der Grundlage ihrer Inhaltsrichtlinien eigene Schwellenwerte festlegen möchten.
>

    {
    "version": 2,
    "timescale": 90000,
    "offset": 0,
    "framerate": 50,
    "width": 1280,
    "height": 720,
    "totalDuration": 18696321,
    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]
    }
    ]
    }

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Videoüberprüfungen](video-reviews-quickstart-dotnet.md) aus Ihrer Moderationsausgabe generieren.

Fügen Sie die [Transkriptmoderation](video-transcript-moderation-review-tutorial-dotnet.md) zu Ihren Videoüberprüfungen hinzu.

In diesem ausführlichen Tutorial erhalten Sie weitere Informationen zum Erstellen einer [vollständigen Projektmappe zur Video- und Transkriptmoderation](video-transcript-moderation-review-tutorial-dotnet.md).

Laden Sie die [Visual Studio-Projektmappe](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) für diesen und andere Content Moderator-Schnellstarts für .NET herunter.

---
title: 'Tutorial: Moderieren von Videos und Transkripts in .NET – Content Moderator'
titlesuffix: Azure Cognitive Services
description: In diesem ausführlichen Tutorial erfahren Sie, wie Sie eine vollständige Lösung für die Video- und Transkriptmoderation mit computergestützter Moderation und Erstellung von Überprüfungen mit menschlicher Beteiligung entwickeln.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: e3578c08b78894c2f9a858e97c7cbe2d260731c5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878747"
---
# <a name="tutorial-video-and-transcript-moderation"></a>Tutorial: Video- und Transkriptmoderation

Mit den Video-APIs von Content Moderator können Sie Videos moderieren und im Tool für die Prüfung durch Menschen Videoüberprüfungen erstellen. 

In diesem ausführlichen Tutorial erfahren Sie, wie Sie eine vollständige Lösung für die Video- und Transkriptmoderation mit computergestützter Moderation und Erstellung von Überprüfungen mit menschlicher Beteiligung entwickeln.

Laden Sie für dieses Tutorial die [C#-Konsolenanwendung](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) herunter. In der Konsolenanwendung werden das SDK und die zugehörigen Pakete genutzt, um die folgenden Aufgaben durchzuführen:

- Komprimieren der Eingabevideos zur schnelleren Verarbeitung
- Moderieren des Videos, um Shots und Frames mit Erkenntnissen zu erhalten
- Verwenden der Framezeitstempel zum Erstellen von Miniaturansichten (Bilder)
- Übermitteln von Zeitstempeln und Miniaturansichten zum Erstellen von Videoüberprüfungen
- Konvertieren der Videosprache in Text (Transkript) mit der Media Indexer-API
- Moderieren des Transkripts mit dem Dienst für die Textmoderation
- Hinzufügen des moderierten Transkripts zur Videoüberprüfung

## <a name="sample-program-outputs"></a>Beispiele für die Ausgabe des Programms

Bevor wir fortfahren, sehen wir uns die folgenden Beispielausgaben des Programms an:

- [Konsolenausgabe](#program-output)
- [Videoüberprüfung](#video-review-default-view)
- [Transkriptansicht](#video-review-transcript-view)

## <a name="prerequisites"></a>Voraussetzungen

1. Registrieren Sie sich für die Website mit dem [Content Moderator-Überprüfungstool](https://contentmoderator.cognitive.microsoft.com/), und [erstellen Sie benutzerdefinierte Tags](Review-Tool-User-Guide/tags.md), die von der C#-Konsolenanwendung über den Code zugewiesen werden. Im folgenden Screenshot sind die benutzerdefinierten Tags dargestellt.

  ![Benutzerdefinierte Tags für Videomoderation](images/video-tutorial-custom-tags.png)

1. Zum Ausführen der Beispielanwendung benötigen Sie ein Azure-Konto und ein Azure Media Services-Konto. Außerdem benötigen Sie Zugriff auf die private Vorschau von Content Moderator. Abschließend benötigen Sie noch die Anmeldeinformationen für die Azure Active Directory-Authentifizierung. Weitere Informationen zum Beziehen dieser Informationen finden Sie in der [Schnellstartanleitung zur Videomoderations-API](video-moderation-api.md).

1. Bearbeiten Sie die Datei `App.config`, und fügen Sie den Active Directory-Mandantennamen, Dienstendpunkte und Abonnementschlüssel hinzu, die mit `#####` gekennzeichnet sind. Sie benötigen die folgenden Informationen:

|Schlüssel|BESCHREIBUNG|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Endpunkt für die AMS-API (Azure Media Services)|
|`ClientSecret`|Abonnementschlüssel für Azure Media Services|
|`ClientId`|Client-ID für Azure Media Services|
|`AzureAdTenantName`|Active Directory-Mandantenname für Ihre Organisation|
|`ContentModeratorReviewApiSubscriptionKey`|Abonnementschlüssel für die Content Moderator-Überprüfungs-API|
|`ContentModeratorApiEndpoint`|Endpunkt für die Content Moderator-API|
|`ContentModeratorTeamId`|Content Moderator-Team-ID|

## <a name="getting-started"></a>Erste Schritte

Die `Program`-Klasse in `Program.cs` ist der Haupteinstiegspunkt in die Anwendung für die Videomoderation.

### <a name="methods-of-class-program"></a>Methoden der Program-Klasse

|Methode|BESCHREIBUNG|
|-|-|
|`Main`|Dient zum Analysieren die Befehlszeile, Erfassen der Benutzereingabe und Starten der Verarbeitung.|
|`ProcessVideo`|Dient zum Komprimieren, Hochladen, Moderieren und Erstellen von Videoüberprüfungen.|
|`CreateVideoStreamingRequest`|Dient zum Erstellen eines Datenstroms zum Hochladen eines Videos.|
|`GetUserInputs`|Dient zum Erfassen der Benutzereingabe. Wird verwendet, wenn keine Befehlszeilenoptionen vorhanden sind.|
|`Initialize`|Dient zum Initialisieren der Objekte, die für den Moderationsprozess erforderlich sind.|

### <a name="the-main-method"></a>Main-Methode

Mit `Main()` beginnt die Ausführung. Dies ist also der Ausgangspunkt für das Verständnis des Prozesses für die Videomoderation.

    static void Main(string[] args)
    {
        if (args.Length == 0)
        {
            string videoPath = string.Empty;
            GetUserInputs(out videoPath);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(Path.GetDirectoryName(videoPath), "log.txt");
            try
            {
                ProcessVideo(videoPath).Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
        else
        {
            DirectoryInfo directoryInfo = new DirectoryInfo(args[0]);
            if (args.Length == 2)
                bool.TryParse(args[1], out generateVtt);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(args[0], "log.txt");
            var files = directoryInfo.GetFiles("*.mp4", SearchOption.AllDirectories);
            foreach (var file in files)
            {
                try
                {
                    ProcessVideo(file.FullName).Wait();
                }
                catch (Exception ex)
                {
                    Console.WriteLine(ex.Message);
                }
            }
        }
    }

`Main()` verarbeitet die folgenden Befehlszeilenargumente:

- Den Pfad zu einem Verzeichnis mit MPEG-4-Videodateien, die für die Moderation übermittelt werden sollen. Alle `*.mp4`-Dateien in diesem Verzeichnis und den Unterverzeichnissen werden für die Moderation übermittelt.
- Optional ein boolesches Flag (true/false), um anzugeben, ob Texttranskripts zum Moderieren von Audiodaten generiert werden sollten.

Wenn keine Befehlszeilenargumente vorhanden sind, ruft `Main()` die `GetUserInputs()`-Methode auf. Mit dieser Methode wird der Benutzer aufgefordert, den Pfad zu einer einzelnen Videodatei einzugeben und anzugeben, ob ein Texttranskript generiert werden soll.

> [!NOTE]
> Die Konsolenanwendung nutzt die [Azure Media Indexer-API](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2), um aus dem Audiotitel des hochgeladenen Videos Transkripts zu generieren. Die Ergebnisse werden im WebVTT-Format bereitgestellt. Weitere Informationen zu diesem Format finden Sie unter [Web Video Text Tracks Format](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API) (Webvideo-Texttitelformat).

### <a name="initialize-and-processvideo-methods"></a>Initialize- und ProcessVideo-Methode

Unabhängig davon, ob die Optionen des Programms von der Befehlszeile oder aus der interaktiven Benutzereingabe stammen, ruft `Main()` als Nächstes `Initialize()` auf, um die folgenden Instanzen zu erstellen:

|Klasse|BESCHREIBUNG|
|-|-|
|`AMSComponent`|Dient zum Komprimieren von Videodateien, bevor diese zur Moderation übermittelt werden.|
|`AMSconfigurations`|Dient als Schnittstelle zu den Konfigurationsdaten der Anwendung (in `App.config`).|
|`VideoModerator`| Dient zum Hochladen, Codieren, Verschlüsseln und Moderieren per AMS-SDK.|
|`VideoReviewApi`|Dient zum Verwalten von Videoüberprüfungen im Content Moderator-Dienst.|

Auf diese Klassen wird in den folgenden Abschnitten dieses Tutorials näher eingegangen (mit Ausnahme von `AMSConfigurations`, da diese Klasse unkompliziert ist).

Abschließend werden die Videodateien einzeln nacheinander verarbeitet, indem jeweils `ProcessVideo()` aufgerufen wird.

    private static async Task ProcessVideo(string videoPath)
    {
        Stopwatch sw = new Stopwatch();
        sw.Start();
        Console.ForegroundColor = ConsoleColor.White;
        Console.WriteLine("\nVideo compression process started...");

        var compressedVideoPath = amsComponent.CompressVideo(videoPath);
        if (string.IsNullOrWhiteSpace(compressedVideoPath))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Compression failed.");
        }

        Console.WriteLine("\nVideo compression process completed...");

        UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
        UploadAssetResult uploadResult = new UploadAssetResult();

        if (generateVtt)
        {
            uploadResult.GenerateVTT = generateVtt;
        }
        Console.WriteLine("\nVideo moderation process started...");

        if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Review process failed.");
        }

        Console.WriteLine("\nVideo moderation process completed...");
        Console.WriteLine("\nVideo review process started...");
        string reviewId = await videoReviewApi.CreateVideoReviewInContentModerator(uploadResult);
        Console.WriteLine("\nVideo review successfully completed...");
        sw.Stop();
        Console.WriteLine("\nTotal Elapsed Time: {0}", sw.Elapsed);
        using (var stw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            stw.WriteLine("Video File Name: " + Path.GetFileName(videoPath));
            stw.WriteLine($"ReviewId: {reviewId}");
            stw.WriteLine("Total Elapsed Time: {0}", sw.Elapsed);
        }
    }


Die `ProcessVideo()`-Methode ist relativ einfach. Sie führt die folgenden Vorgänge in dieser Reihenfolge durch:

- Komprimieren des Videos
- Hochladen des Videos in ein Azure Media Services-Asset
- Erstellen eines AMS-Auftrags zum Moderieren des Videos
- Erstellen einer Videoüberprüfung in Content Moderator

In den folgenden Abschnitten werden einige individuelle Prozesse, die von `ProcessVideo()` aufgerufen werden, ausführlicher behandelt. 

## <a name="compressing-the-video"></a>Komprimieren des Videos

Zur Reduzierung des Netzwerkdatenverkehrs konvertiert die Anwendung Videodateien in das Format „H.264 (MPEG-4 AVC)“ und skaliert sie auf eine maximale Breite von 640 Pixel. Die Verwendung des H.264-Codecs wird aufgrund seiner hohen Effizienz (Komprimierungsrate) empfohlen. Die Komprimierung wird mit dem kostenlosen Befehlszeilentool `ffmpeg` durchgeführt, das im Ordner `Lib` der Visual Studio-Lösung enthalten ist. Die Eingabedateien können ein beliebiges Format haben, das von `ffmpeg` unterstützt wird, z.B. die gängigsten Videodateiformate und -codecs.

> [!NOTE]
> Wenn Sie das Programm mit Befehlszeilenoptionen starten, geben Sie ein Verzeichnis mit den Videodateien an, die für die Moderation übermittelt werden sollen. Alle Dateien in diesem Verzeichnis, die über die Dateinamenerweiterung `.mp4` verfügen, werden verarbeitet. Aktualisieren Sie zum Verarbeiten von anderen Dateinamenerweiterungen die `Main()`-Methode in der Datei `Program.cs`, damit sie die gewünschten Erweiterungen enthält.

Der Code, mit dem eine einzelne Videodatei komprimiert wird, ist die `AmsComponent`-Klasse in `AMSComponent.cs`. Die für diese Funktionalität zuständige Methode ist `CompressVideo()` (hier dargestellt).

    public string CompressVideo(string videoPath)
    {
        string ffmpegBlobUrl;
        if (!ValidatePreRequisites())
        {
            Console.WriteLine("Configurations check failed. Please cross check the configurations!");
            throw new Exception();
        }

        if (File.Exists(_configObj.FfmpegExecutablePath))
        {
            ffmpegBlobUrl = this._configObj.FfmpegExecutablePath;
        }
        else
        {
            Console.WriteLine("ffmpeg.exe is missing. Please check the Lib folder");
            throw new Exception();
        }

        string videoFilePathCom = videoPath.Split('.')[0] + "_c.mp4";
        ProcessStartInfo processStartInfo = new ProcessStartInfo();
        processStartInfo.WindowStyle = ProcessWindowStyle.Hidden;
        processStartInfo.FileName = ffmpegBlobUrl;
        processStartInfo.Arguments = "-i \"" + videoPath + "\" -vcodec libx264 -n -crf 32 -preset veryfast -vf scale=640:-1 -c:a aac -aq 1 -ac 2 -threads 0 \"" + videoFilePathCom + "\"";
        var process = Process.Start(processStartInfo);
        process.WaitForExit();
        process.Close();
        return videoFilePathCom;
    }

Mit diesem Code werden die folgenden Schritte ausgeführt:

- Es wird überprüft, ob die Konfiguration in `App.config` alle erforderlichen Daten enthält.
- Es wird überprüft, ob die `ffmpeg`-Binärdaten vorhanden sind.
- Der Name der Ausgabedatei wird erstellt, indem `_c.mp4` an den Basisnamen der Datei angefügt wird (z.B. `Example.mp4` -> `Example_c.mp4`).
- Erstellt eine Befehlszeilen-Zeichenfolge zum Durchführen der Konvertierung.
- Startet einen `ffmpeg`-Prozess über die Befehlszeile.
- Es wird gewartet, bis das Video verarbeitet wurde.

> [!NOTE]
> Wenn Sie wissen, dass Ihre Videos bereits per H.264 komprimiert wurden und über die richtigen Dimensionen verfügen, können Sie `CompressVideo()` umschreiben, um die Komprimierung zu überspringen.

Die Methode gibt den Dateinamen der komprimierten Ausgabedatei zurück.

## <a name="uploading-and-moderating-the-video"></a>Hochladen und Moderieren des Videos

Das Video muss in Azure Media Services gespeichert werden, bevor es vom Content Moderation-Dienst verarbeitet werden kann. Die `Program`-Klasse in `Program.cs` verfügt über eine kleine `CreateVideoStreamingRequest()`-Methode, mit der ein Objekt zurückgegeben wird, das für die Streaminganforderung zum Hochladen des Videos steht.

    private static UploadVideoStreamRequest CreateVideoStreamingRequest(string compressedVideoFilePath)
    {
        return
            new UploadVideoStreamRequest
            {
                VideoStream = File.ReadAllBytes(compressedVideoFilePath),
                VideoName = Path.GetFileName(compressedVideoFilePath),
                EncodingRequest = new EncodingRequest()
                {
                    EncodingBitrate = AmsEncoding.AdaptiveStreaming
                },
                VideoFilePath = compressedVideoFilePath
            };
    }

Das sich ergebende `UploadVideoStreamRequest`-Objekt ist unter `UploadVideoStreamRequest.cs` (und dem übergeordneten `UploadVideoRequest`-Element in `UploadVideoRequest.cs`) definiert. Diese Klassen werden hier nicht angezeigt. Sie sind kurz und haben nur den Zweck, die komprimierten Videodaten und die zugehörigen Informationen zu enthalten. Eine andere Nur-Daten-Klasse, nämlich `UploadAssetResult` (`UploadAssetResult.cs`), wird für die Ergebnisse des Uploadprozesses verwendet. Jetzt sind diese Zeilen in `ProcessVideo()` verständlich:

    UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
    UploadAssetResult uploadResult = new UploadAssetResult();

    if (generateVtt)
    {
        uploadResult.GenerateVTT = generateVtt;
    }
    Console.WriteLine("\nVideo moderation process started...");

    if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
    {
        Console.ForegroundColor = ConsoleColor.Red;
        Console.WriteLine("Video Review process failed.");
    }

Mit diesen Zeilen werden die folgenden Aufgaben durchgeführt:

- Erstellen eines `UploadVideoStreamRequest`-Elements zum Hochladen der komprimierten Videodaten
- Festlegen des `GenerateVTT`-Flags der Anforderung, wenn der Benutzer ein Texttranskript angefordert hat
- Aufrufen von `CreateAzureMediaServicesJobToModerateVideo()` zum Durchführen des Uploads und Empfangen des Ergebnisses

## <a name="deep-dive-into-video-moderation"></a>Ausführliche Betrachtung der Videomoderation

Die `CreateAzureMediaServicesJobToModerateVideo()`-Methode befindet sich in der Datei `VideoModerator.cs`, die den Großteil des Codes für die Interaktion mit Azure Media Services enthält. Der Quellcode der Methode ist im folgenden Auszug dargestellt.

    public bool CreateAzureMediaServicesJobToModerateVideo(UploadVideoStreamRequest uploadVideoRequest, UploadAssetResult uploadResult)
    {
        asset = CreateAsset(uploadVideoRequest);
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        // Encoding the asset , Moderating the asset, Generating transcript in parallel
        IAsset encodedAsset = null;
        //Creates the job for the tasks.
        IJob job = this._mediaContext.Jobs.Create("AMS Review Job");

        //Adding encoding task to job.
        ConfigureEncodeAssetTask(uploadVideoRequest.EncodingRequest, job);

        ConfigureContentModerationTask(job);

        //adding transcript task to job.
        if (uploadResult.GenerateVTT)
        {
            ConfigureTranscriptTask(job);
        }

        Stopwatch timer = new Stopwatch();
        timer.Start();
        //submit and execute job.
        job.Submit();
        job.GetExecutionProgressTask(new CancellationTokenSource().Token).Wait();
        timer.Stop();
        using (var sw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            sw.WriteLine("AMS Job Elapsed Time: {0}", timer.Elapsed);
        }

        if (job.State == JobState.Error)
        {
            throw new Exception("Video moderation has failed due to AMS Job error.");
        }

        UploadAssetResult result = uploadResult;
        encodedAsset = job.OutputMediaAssets[0];
        result.ModeratedJson = GetCmDetail(job.OutputMediaAssets[1]);
        // Check for valid Moderated JSON
        var jsonModerateObject = JsonConvert.DeserializeObject<VideoModerationResult>(result.ModeratedJson);

        if (jsonModerateObject == null)
        {
            return false;
        }
        if (uploadResult.GenerateVTT)
        {
            GenerateTranscript(job.OutputMediaAssets.Last());
        }

        uploadResult.StreamingUrlDetails = PublishAsset(encodedAsset);
        string downloadUrl = GenerateDownloadUrl(asset, uploadVideoRequest.VideoName);
        uploadResult.StreamingUrlDetails.DownloadUri = downloadUrl;
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        uploadResult.VideoFilePath = uploadVideoRequest.VideoFilePath;
        return true;
    }

Mit diesem Code werden die folgenden Aufgaben durchgeführt:

- Erstellen eines AMS-Auftrags für die erforderlichen Verarbeitungsschritte
- Hinzufügen von Aufgaben zum Codieren und Moderieren der Videodatei und Generieren eines Texttranskripts
- Übermitteln des Auftrags, Hochladen der Datei und Starten der Verarbeitung
- Abrufen der Moderationsergebnisse, des Texttranskripts (falls angefordert) und anderer Informationen

## <a name="sample-video-moderation-response"></a>Antwort der Beispielvideomoderation

Das Ergebnis des Auftrags für die Videomoderation (siehe [Schnellstart zur Videomoderation](video-moderation-api.md)) ist eine JSON-Datenstruktur mit den Moderationsergebnissen. Diese Ergebnisse umfassen eine Aufschlüsselung der Fragmente (Shots) des Videos, die jeweils Ereignisse (Clips) mit Keyframes enthalten, die zur Überprüfung gekennzeichnet wurden. Jeder Keyframe wird anhand der Wahrscheinlichkeit bewertet, dass er nur für Erwachsene geeignete bzw. anzügliche Inhalte aufweist. Im folgenden Beispiel ist eine JSON-Antwort dargestellt:

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

Eine Transkription der Audiodaten aus dem Video wird auch produziert, wenn das `GenerateVTT`-Flag festgelegt wird.

> [!NOTE]
> Die Konsolenanwendung nutzt die [Azure Media Indexer-API](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2), um aus dem Audiotitel des hochgeladenen Videos Transkripts zu generieren. Die Ergebnisse werden im WebVTT-Format bereitgestellt. Weitere Informationen zu diesem Format finden Sie unter [Web Video Text Tracks Format](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API) (Webvideo-Texttitelformat).


## <a name="creating-the-human-in-the-loop-review"></a>Erstellen der Überprüfung mit menschlicher Beteiligung

Der Moderationsprozess gibt eine Liste mit Keyframes aus dem Video zurück – zusammen mit einem Transkript der Audiotitel. Der nächste Schritt ist das Erstellen einer Überprüfung im Content Moderator-Prüfungstool für menschliche Moderatoren. Wenn Sie zurück zur `ProcessVideo()`-Methode in `Program.cs` navigieren, sehen Sie den Aufruf der `CreateVideoReviewInContentModerator()`-Methode. Diese Methode befindet sich in der `videoReviewApi`-Klasse, die in `VideoReviewAPI.cs` enthalten ist. Sie ist hier dargestellt.

    public async Task<string> CreateVideoReviewInContentModerator(UploadAssetResult uploadAssetResult)
    {
    
        string reviewId = string.Empty;
        List<ProcessedFrameDetails> frameEntityList = framegenerator.CreateVideoFrames(uploadAssetResult);
        string path = uploadAssetResult.GenerateVTT == true ? this._amsConfig.FfmpegFramesOutputPath + Path.GetFileNameWithoutExtension (uploadAssetResult.VideoName) + "_aud_SpReco.vtt" : "";
        TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult();
        
    if (File.Exists(path))
        {
            screenTextResult = await GenerateTextScreenProfanity(reviewId, path, frameEntityList);
            uploadAssetResult.Category1TextScore = screenTextResult.Category1Score;
            uploadAssetResult.Category2TextScore = screenTextResult.Category2Score;
            uploadAssetResult.Category3TextScore = screenTextResult.Category3Score;
            uploadAssetResult.Category1TextTag = screenTextResult.Category1Tag;
            uploadAssetResult.Category2TextTag = screenTextResult.Category2Tag;
            uploadAssetResult.Category3TextTag = screenTextResult.Category3Tag;
        }
        
        var reviewVideoRequestJson = CreateReviewRequestObject(uploadAssetResult, frameEntityList);
        if (string.IsNullOrWhiteSpace(reviewVideoRequestJson))
        {
            throw new Exception("Video review process failed in CreateVideoReviewInContentModerator");
        }
        
        reviewId = JsonConvert.DeserializeObject<List<string>>(ExecuteCreateReviewApi(reviewVideoRequestJson).Result).FirstOrDefault();
        frameEntityList = framegenerator.GenerateFrameImages(frameEntityList, uploadAssetResult, reviewId);
        await CreateAndPublishReviewInContentModerator(uploadAssetResult, frameEntityList, reviewId, path, screenTextResult);
        return reviewId;
    
    }

Mit `CreateVideoReviewInContentModerator()` werden mehrere andere Methoden aufgerufen, um die folgenden Aufgaben durchzuführen:

> [!NOTE]
> Die Konsolenanwendung nutzt die [FFmpeg](https://ffmpeg.org/)-Bibliothek zum Generieren von Miniaturansichten. Die Miniaturansichten (Bilder) entsprechen den Framezeitstempeln in der [Ausgabe der Videomoderation](#sample-video-moderation-response).

|Aufgabe|Methoden|Datei|
|-|-|-|
|Extrahieren der Keyframes aus dem Video und Erstellen von entsprechenden Miniaturansichten|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Scannen des Texttranskripts, falls verfügbar, zum Ermitteln von Audiodaten, die nur für Erwachsene geeignet bzw. anzüglich sind|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|Vorbereiten und Übermitteln einer Videoüberprüfungsanforderung zur Prüfung durch einen Menschen|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>Videoüberprüfung – Standardansicht

Im folgenden Screenshot sind die Ergebnisse der obigen Schritte dargestellt.

![Videoüberprüfung – Standardansicht](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>Transkriptgenerierung

Bisher ging es im Code dieses Tutorials vor allem um den visuellen Inhalt. Die Überprüfung des Sprachinhalts ist ein separater und optionaler Prozess, für den wie erwähnt ein aus den Audiodaten generiertes Transkript verwendet wird. Jetzt ist der Zeitpunkt gekommen, einen Blick darauf zu werfen, wie Texttranskripte erstellt und im Überprüfungsprozess verwendet werden. Die Aufgabe zur Generierung des Transkripts obliegt dem [Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-index-content)-Dienst.

Die Anwendung führt die folgenden Aufgaben durch:

|Aufgabe|Methoden|Datei|
|-|-|-|
|Ermitteln, ob Texttranskripte generiert werden sollen|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Wenn ja: Übermitteln eines Transkriptionsauftrags im Rahmen der Moderation|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Abrufen einer lokalen Kopie des Transkripts|`GenerateTranscript()`|`VideoModerator.cs`|
|Kennzeichnen der Frames des Videos, die ungeeignete Audiodaten enthalten|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|Hinzufügen der Ergebnisse zur Überprüfung|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Aufgabenkonfiguration

Wir beginnen gleich mit der Übermittlung des Transkriptionsauftrags. Mit `CreateAzureMediaServicesJobToModerateVideo()` (bereits beschrieben) wird `ConfigureTranscriptTask()` aufgerufen.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

Die Konfiguration für die Transkriptaufgabe wird aus der Datei `MediaIndexerConfig.json` im Ordner `Lib` der Lösung gelesen. AMS-Medienobjekte werden für die Konfigurationsdatei und für die Ausgabe des Transkriptionsprozesses erstellt. Bei Ausführung des AMS-Auftrags wird mit dieser Aufgabe ein Texttranskript aus dem Audiotitel der Videodatei erstellt.

> [!NOTE]
> Die Beispielanwendung erkennt nur Sprache auf Englisch (USA).

### <a name="transcript-generation"></a>Transkriptgenerierung

Das Transkript wird als AMS-Medienobjekt veröffentlicht. Zum Scannen des Transkripts auf unzulässigen Inhalt lädt die Anwendung das Medienobjekt aus Azure Media Services herunter. `CreateAzureMediaServicesJobToModerateVideo()` ruft `GenerateTranscript()` auf (hier gezeigt), um die Datei abzurufen.

    public bool GenerateTranscript(IAsset asset)
    {
        try
        {
            var outputFolder = this._amsConfigurations.FfmpegFramesOutputPath;
            IAsset outputAsset = asset;
            IAccessPolicy policy = null;
            ILocator locator = null;
            policy = _mediaContext.AccessPolicies.Create("My 30 days readonly policy", TimeSpan.FromDays(360), AccessPermissions.Read);
            locator = _mediaContext.Locators.CreateLocator(LocatorType.Sas, outputAsset, policy, DateTime.UtcNow.AddMinutes(-5));
            DownloadAssetToLocal(outputAsset, outputFolder);
            locator.Delete();
            return true;
        }
        catch
        {   //TODO:  Logging
            Console.WriteLine("Exception occurred while generating index for video.");
            throw;
        }
    }

Nach dem erforderlichen AMS-Setup wird der eigentliche Download durchgeführt, indem die generische Funktion `DownloadAssetToLocal()` aufgerufen wird, mit der ein AMS-Medienobjekt in eine lokale Datei kopiert wird.

## <a name="transcript-moderation"></a>Transkriptmoderation

Das Transkript ist nun verfügbar und wird gescannt und für die Überprüfung verwendet. Für die Erstellung der Überprüfung ist das `CreateVideoReviewInContentModerator()`-Element zuständig, mit dem `GenerateTextScreenProfanity()` für die Durchführung des Auftrags aufgerufen wird. Mit dieser Methode wird wiederum das `TextScreen()`-Element aufgerufen, das den Großteil der Funktionalität enthält. 

`TextScreen()` führt die folgenden Aufgaben durch:

- Analysieren des Transkripts auf Zeitstempel und Untertitel
- Übermitteln der einzelnen Untertitel für die Textmoderation
- Kennzeichnen aller Frames, die ggf. unzulässigen Sprachinhalt aufweisen

Wir sehen uns diese Aufgaben nun genauer an:

### <a name="initialize-the-code"></a>Initialisieren des Codes

Initialisieren Sie zuerst alle Variablen und Sammlungen.

    private async Task<TranscriptScreenTextResult> TextScreen(string filepath, List<ProcessedFrameDetails> frameEntityList)
    {
        List<TranscriptProfanity> profanityList = new List<TranscriptProfanity>();
        string responseContent = string.Empty;
        HttpResponseMessage response;
        bool category1Tag = false;
        bool category2Tag = false;
        bool category3Tag = false;
        double category1Score = 0;
        double category2Score = 0;
        double category3Score = 0;
        List<string> vttLines = File.ReadAllLines(filepath).Where(line => !line.Contains("NOTE Confidence:") && line.Length > 0).ToList();
        StringBuilder sb = new StringBuilder();
        List<CaptionScreentextResult> csrList = new List<CaptionScreentextResult>();
        CaptionScreentextResult captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };

        // Code from the next sections in the tutorial
    

### <a name="parse-the-transcript-for-captions"></a>Analysieren des Transkripts auf Untertitel

Analysieren Sie als Nächstes das Transkript im VTT-Format auf Untertitel und Zeitstempel. Im Prüfungstool werden diese Untertitel im Fenster für die Videoüberprüfung auf der Registerkarte „Transkript“ angezeigt. Die Zeitstempel werden verwendet, um die Untertitel mit den entsprechenden Videoframes zu synchronisieren.

        // Code from the previous section(s) in the tutorial

        //
        // Parse the transcript
        //
        foreach (var line in vttLines.Skip(1))
        {
                if (line.Contains("-->"))
                {
                    if (sb.Length > 0)
                    {
                        captionScreentextResult.Captions.Add(sb.ToString());
                        sb.Clear();
                    }
                    if (captionScreentextResult.Captions.Count > 0)
                    {
                        csrList.Add(captionScreentextResult);
                        captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };
                    }
                    string[] times = line.Split(new string[] { "-->" }, StringSplitOptions.RemoveEmptyEntries);
                    string startTimeString = times[0].Trim();
                    string endTimeString = times[1].Trim();
                    int startTime = (int)TimeSpan.ParseExact(startTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    int endTime = (int)TimeSpan.ParseExact(endTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    captionScreentextResult.StartTime = startTime;
                    captionScreentextResult.EndTime = endTime;
                }
                else
                {
                    sb.Append(line);
                }
                if (sb.Length + line.Length > 1024)
                {
                    captionScreentextResult.Captions.Add(sb.ToString());
                    sb.Clear();
                }
            }
            if (sb.Length > 0)
            {
                captionScreentextResult.Captions.Add(sb.ToString());
            }
            if (captionScreentextResult.Captions.Count > 0)
            {
                csrList.Add(captionScreentextResult);
            }

            // Code from the following section in the quickstart

### <a name="moderate-captions-with-the-text-moderation-service"></a>Moderieren von Untertiteln mit dem Dienst für die Textmoderation

Als Nächstes untersuchen wir die analysierten Textuntertitel mit der Text-API von Content Moderator.

> [!NOTE]
> Für Ihren Content Moderator-Dienstschlüssel gilt ein Ratenlimit vom Typ „Anforderungen pro Sekunde“ (Requests Per Second, RPS). Wenn Sie das Limit überschreiten, löst das SDK eine Ausnahme mit dem Fehlercode 429 aus. 
>
> Ein Schlüssel des Free-Tarifs verfügt über ein RPS-Ratenlimit.

    //
    // Moderate the captions or cues
    //
    int waitTime = 1000;
    foreach (var csr in csrList)
    {
                bool captionAdultTextTag = false;
                bool captionRacyTextTag = false;
                bool captionOffensiveTextTag = false;
                bool retry = true;

                foreach (var caption in csr.Captions)
                {
                    while (retry)
                    {
                        try
                        {
                            System.Threading.Thread.Sleep(waitTime);
                            var lang = await CMClient.TextModeration.DetectLanguageAsync("text/plain", caption);
                            var oRes = await CMClient.TextModeration.ScreenTextWithHttpMessagesAsync(lang.DetectedLanguageProperty, "text/plain", caption, null, null, null, true);
                            response = oRes.Response;
                            responseContent = await response.Content.ReadAsStringAsync();
                            retry = false;
                        }
                        catch (Exception e)
                        {
                            if (e.Message.Contains("429"))
                            {
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                                waitTime = (int)(waitTime * 1.5);
                                Console.WriteLine($"wait time: {waitTime}");
                            }
                            else
                            {
                                retry = false;
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                            }
                        }
                    }
                    var jsonTextScreen = JsonConvert.DeserializeObject<TextScreen>(responseContent);
                    if (jsonTextScreen != null)
                    {
                        TranscriptProfanity transcriptProfanity = new TranscriptProfanity();
                        transcriptProfanity.TimeStamp = "";
                        List<Terms> transcriptTerm = new List<Terms>();
                        if (jsonTextScreen.Terms != null)
                        {
                            foreach (var term in jsonTextScreen.Terms)
                            {
                                var profanityobject = new Terms
                                {
                                    Term = term.Term,
                                    Index = term.Index
                                };
                                transcriptTerm.Add(profanityobject);
                            }
                            transcriptProfanity.Terms = transcriptTerm;
                            profanityList.Add(transcriptProfanity);
                        }
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) captionAdultTextTag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) captionRacyTextTag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) captionOffensiveTextTag = true;
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) category1Tag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) category2Tag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) category3Tag = true;
                        category1Score = jsonTextScreen.Classification.Category1.Score > category1Score ? jsonTextScreen.Classification.Category1.Score : category1Score;
                        category2Score = jsonTextScreen.Classification.Category2.Score > category2Score ? jsonTextScreen.Classification.Category2.Score : category2Score;
                        category3Score = jsonTextScreen.Classification.Category3.Score > category3Score ? jsonTextScreen.Classification.Category3.Score : category3Score;
                    }
                    foreach (var frame in frameEntityList.Where(x => x.TimeStamp >= csr.StartTime && x.TimeStamp <= csr.EndTime))
                    {
                        frame.IsAdultTextContent = captionAdultTextTag;
                        frame.IsRacyTextContent = captionRacyTextTag;
                        frame.IsOffensiveTextContent = captionOffensiveTextTag;
                    }
                }
            }
            TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult()
            {
                TranscriptProfanity = profanityList,
                Category1Tag = category1Tag,
                Category2Tag = category2Tag,
                Category3Tag = category3Tag,
                Category1Score = category1Score,
                Category2Score = category2Score,
                Category3Score = category3Score
            };
            return screenTextResult;
    }

### <a name="breaking-down-the-text-moderation-step"></a>Aufschlüsseln des Textmoderationsschritts

Da `TextScreen()` eine etwas umfangreichere Methode ist, schlüsseln wir sie hier auf.

1. Zuerst liest die Methode die Transkriptdatei Zeile für Zeile. Leere Zeilen und Zeilen mit einer Anmerkung (`NOTE`) und zugehöriger Zuverlässigkeitsbewertung werden ignoriert. Die Zeitstempel und Textelemente werden aus den Hinweisen (*Cues*) in der Datei extrahiert. Ein „Cue“ steht für Text aus dem Audiotitel und enthält die Start- und Endzeit. Der „Cue“ beginnt mit der Zeitstempelzeile mit der Zeichenfolge `-->`. Es folgen eine oder mehrere Textzeilen.

1. Instanzen von `CaptionScreentextResult` (in `TranscriptProfanity.cs` definiert) enthalten die Informationen, die für die einzelnen „Cues“ analysiert werden.  Wenn eine neue Zeitstempelzeile erkannt oder eine maximale Textlänge von 1.024 Zeichen erreicht wird, wird der `csrList` ein neues `CaptionScreentextResult`-Element hinzugefügt. 

1. Mit der nächsten Methode werden die einzelnen „Cues“ an die Textmoderations-API übermittelt. Sie ruft die Elemente `ContentModeratorClient.TextModeration.DetectLanguageAsync()` und `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()` auf, die in der `Microsoft.Azure.CognitiveServices.ContentModerator`-Assembly definiert sind. Um nicht unter das Ratenlimit zu fallen, wartet die Methode eine Sekunde lang, bevor die „Cues“ übermittelt werden.

1. Nachdem die Ergebnisse des Diensts für die Textmoderation empfangen wurden, werden sie von der Methode analysiert, um zu ermitteln, ob sie die Schwellenwerte für die Zuverlässigkeit erfüllen. Diese Werte liegen in `App.config` unter `OffensiveTextThreshold`, `RacyTextThreshold` und `AdultTextThreshold` vor. Abschließend werden auch die unzulässigen Begriffe gespeichert. Alle Frames innerhalb des Zeitbereichs eines „Cues“ werden gekennzeichnet, um anzugeben, dass sie anstößigen, anzüglichen bzw. nur für Erwachsene geeigneten Text enthalten.

1. `TextScreen()` gibt eine `TranscriptScreenTextResult`-Instanz zurück, die das Ergebnis der Textmoderation für das gesamte Video enthält. Dieses Objekt enthält Flags und Bewertungen für die unterschiedlichen Arten von unzulässigem Inhalt sowie eine Liste mit allen unzulässigen Begriffen. Der Aufrufer `CreateVideoReviewInContentModerator()` ruft `UploadScreenTextResult()` auf, um diese Informationen an die Überprüfung anzufügen, damit sie für menschliche Prüfer verfügbar sind.
 
## <a name="video-review-transcript-view"></a>Videoüberprüfung – Transkriptansicht

Der folgende Screenshot enthält das Ergebnis der Schritte für die Transkriptgenerierung und der Moderation.

![Videomoderation – Transkriptansicht](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Programmausgabe

In der folgenden Befehlszeilenausgabe des Programms sind die unterschiedlichen Aufgaben und ihre Durchführung zu sehen. Darüber hinaus sind das Moderationsergebnis (im JSON-Format) und das Sprachtranskript in demselben Verzeichnis wie die ursprünglichen Videodateien verfügbar.

    Microsoft.ContentModerator.AMSComponentClient
    Enter the fully qualified local path for Uploading the video :
    "Your File Name.MP4"
    Generate Video Transcript? [y/n] : y
    
    Video compression process started...
    Video compression process completed...
    
    Video moderation process started...
    Video moderation process completed...
    
    Video review process started...
    Video Frames Creation inprogress...
    Frames(83) created successfully.
    Review Created Successfully and the review Id 201801va8ec2108d6e043229ba7a9e6373edec5
    Video review successfully completed...
    
    Total Elapsed Time: 00:05:56.8420355


## <a name="next-steps"></a>Nächste Schritte

[Laden Sie die Visual Studio-Lösung herunter](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp). Laden Sie außerdem die Beispieldateien und erforderlichen Bibliotheken für dieses Tutorial herunter, und beginnen Sie mit der Integration.

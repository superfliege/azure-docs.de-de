---
title: Analysieren von Videos mit Azure Media Services | Microsoft Docs
description: Führen Sie die Schritte in diesem Tutorial aus, um Videos mit Azure Media Services zu analysieren.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/28/2018
ms.author: juliako
ms.openlocfilehash: 314ffce8a9f8dde62cac670099afbc2223df37e4
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971997"
---
# <a name="tutorial-analyze-videos-with-azure-media-services"></a>Tutorial: Analysieren von Videos mit Azure Media Services 

In diesem Tutorial wird gezeigt, wie Videos mit Azure Media Services analysiert werden. Es gibt viele Szenarien, in denen Sie umfassende Einblicke in aufgezeichnete Videos oder Audioinhalte erlangen möchten. Um beispielsweise eine höhere Kundenzufriedenheit zu erreichen, können Unternehmen eine Spracherkennung ausführen, um Kundensupportaufzeichnungen in einen durchsuchbaren Katalog mit Indizes und Dashboards umzuwandeln. Dann können sie Einblicke in ihr Geschäft erhalten, z.B. eine Liste der häufigsten Beschwerden, Quellen solcher Beschwerden usw.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:    

> [!div class="checklist"]
> * Erstellen eines Media Services-Kontos
> * Zugreifen auf die Media Services-API
> * Konfigurieren der Beispiel-App
> * Untersuchen des Codes für die Analyse des angegebenen Videos
> * Ausführen der App
> * Untersuchen der Ausgabe
> * Bereinigen von Ressourcen

> [!Note]
> Verwenden Sie das Azure-Portal, so wie in der [Übersicht über das Skalieren der Medienverarbeitung](../previous/media-services-scale-media-processing-overview.md) beschrieben, um Ihr Media Services-Konto auf 10 S3 reservierte Einheiten für Medien.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Visual Studio noch nicht installiert haben, können Sie [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15) abrufen.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie ein GitHub-Repository auf Ihren Computer, das das .NET-Beispiel enthält, indem Sie den folgenden Befehl verwenden:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

Das Beispiel befindet sich im Ordner [AnalyzeVideos](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/AnalyzeVideos).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-analyzes-the-specified-video"></a>Untersuchen des Codes für die Analyse des angegebenen Videos

In diesem Abschnitt werden die Funktionen untersucht, die in der Datei „[Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/Program.cs)“ des *AnalyzeVideos*-Projekts definiert werden.

Mit dem Beispiel werden die folgenden Aktionen durchgeführt:

1. Erstellen einer Transformation und eines Auftrags für die Videoanalyse
2. Erstellen eines Eingangsmedienobjekts und Hochladen des Videos in dieses Objekt. Das Medienobjekt wird als Eingabe für den Auftrag verwendet.
3. Erstellen eines Ausgabemedienobjekts zum Speichern der Auftragsausgabe 
4. Übermitteln des Auftrags
5. Überprüfen des Auftragsstatus
6. Herunterladen der durch die Auftragsausführung entstandenen Dateien 

### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten der Verwendung von Media Services-APIs mit dem .NET SDK

Um mit der Verwendung von Media Services-APIs in .NET zu beginnen, müssen Sie ein **AzureMediaServicesClient**-Objekt erstellen. Zum Erstellen des Objekts müssen Sie Anmeldeinformationen bereitstellen, die für den Client zum Herstellen einer Verbindung mit Azure mithilfe von Azure AD erforderlich sind. In dem Code, den Sie zu Beginn des Artikels geklont haben, erstellt die Funktion **GetCredentialsAsync** das ServiceClientCredentials-Objekt basierend auf den in der lokalen Konfigurationsdatei angegebenen Anmeldeinformationen. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Erstellen eines Eingabeobjekts und Hochladen einer lokalen Datei in dieses 

Die **CreateInputAsset**-Funktion erstellt ein neues [Eingabeobjekt](https://docs.microsoft.com/rest/api/media/assets) und lädt die angegebene lokale Videodatei in dieses hoch. Dieses Objekt wird als Eingabe für Ihren Codierungsauftrag verwendet. In Media Services v3 kann die Eingabe in einen Auftrag ein Objekt sein, oder es kann sich um Inhalte handeln, die Sie für Ihr Media Services-Konto über HTTPS-URLs zur Verfügung stellen. Wenn Sie erfahren möchten, wie die Codierung aus einer HTTPS-URL erfolgt, lesen Sie [diesen](job-input-from-http-how-to.md) Artikel.  

In Media Services v3 verwenden Sie Azure Storage-APIs zum Hochladen von Dateien. Im folgenden Codeausschnitt wird gezeigt, wie dies funktioniert.

Die folgende Funktion führt diese Aktionen aus:

* Erstellen eines Objekts 
* Abrufen einer nicht schreibgeschützten [SAS-URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) für den [Container](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) des Objekts im Speicher
* Die Datei in den Container im Speicher mithilfe der SAS-URL hochladen

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-the-job"></a>Erstellen eines Ausgabemedienobjekts zum Speichern des Auftragsergebnisses 

Das [Ausgabeobjekt](https://docs.microsoft.com/rest/api/media/assets) speichert das Ergebnis Ihres Auftrags. Das Projekt definiert die **DownloadResults**-Funktion, die die Ergebnisse aus diesem Ausgabeobjekt in den Ordner „output“ herunterlädt, damit Sie sehen können, was Sie abgerufen haben.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-analyzes-videos"></a>Erstellen einer Transformation und eines Auftrags, der Videos analysiert

Bei der Codierung oder Verarbeitung von Inhalten in Media Services besteht ein allgemeines Muster darin, die Codierungseinstellungen als eine Anleitung einzurichten. Anschließend übermitteln Sie einen **Auftrag**, um diese Anleitung auf ein Video anzuwenden. Durch die Übermittlung von neuen Aufträgen für jedes neue Video wenden Sie diese Anleitung auf alle Videos in Ihrer Bibliothek an. Eine Anleitung wird in Media Services als eine **Transformation** aufgerufen. Weitere Informationen finden Sie unter [Transformationen und Aufträge](transform-concept.md). Das Beispiel in diesem Tutorial definiert eine Anleitung, die das angegebene Video analysiert. 

#### <a name="transform"></a>Transformieren

Beim Erstellen einer neuen [Transformations](https://docs.microsoft.com/rest/api/media/transforms)instanz müssen Sie angeben, was als Ausgabe generiert werden soll. Der erforderliche Parameter ist ein **TransformOutput**-Objekt, wie im Code oben gezeigt. Jedes **TransformOutput**-Objekt enthält eine **Voreinstellung**. Die **Voreinstellung** beschreibt schrittweise Anweisungen von Video- und/oder Audioverarbeitungsvorgängen, die verwendet werden sollen, um das gewünschte **TransformOutput**-Objekt zu generieren. In diesem Beispiel wird die **VideoAnalyzerPreset**-Voreinstellung verwendet und die Sprache („en-US“) an ihren Konstruktor übergeben. Diese Voreinstellung ermöglicht Ihnen, mehrere Audio- und Videoinformationen aus einer Videodatei zu extrahieren. Sie können die **AudioAnalyzerPreset**-Voreinstellung verwenden, wenn Sie mehrere Audioinformationen aus einem Video extrahieren müssen. 

Beim Erstellen einer **Transformation** sollten Sie zunächst mit der **Get**-Methode überprüfen, ob eine solche bereits vorhanden ist. Der folgende Code zeigt dies.  In Media Services v3 geben **Get**-Methoden für Entitäten **null** zurück, wenn die Entität nicht vorhanden ist (eine Überprüfung des Namens ohne Unterscheidung zwischen Groß-/Kleinschreibung ).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>Auftrag

Wie bereits erwähnt, ist das [Transformations](https://docs.microsoft.com/rest/api/media/transforms)objekt die Anleitung und ein [Auftrag](https://docs.microsoft.com/rest/api/media/jobs) die tatsächliche Anforderung an Media Services, diese **Transformation** auf ein bestimmtes Eingabevideo oder einen Audioinhalt anzuwenden. Der **Auftrag** gibt Informationen wie den Speicherort des Eingabevideos und den Speicherort für die Ausgabe an. Sie können den Speicherort des Videos mit HTTPS-URLs, SAS-URLs oder Objekten angeben, die sich in Ihrem Media Services-Konto befinden. 

In diesem Beispiel ist die Auftragseingabe ein lokales Video.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>Warten auf den Abschluss des Auftrags

Der Abschluss des Auftrags nimmt einige Zeit in Anspruch. Wenn er erfolgt ist, möchten Sie benachrichtigt werden. Es bestehen verschiedene Optionen, sich beim Abschluss des [Auftrags](https://docs.microsoft.com/rest/api/media/jobs) benachrichtigen zu lassen. Die einfachste Möglichkeit (die hier gezeigten wird) ist die Verwendung von Abrufvorgängen. 

Der Abruf ist aufgrund potenzieller Latenzzeiten keine empfohlene bewährte Methode für Produktionsanwendungen. Der Abruf kann gedrosselt werden, wenn er für ein Konto im Übermaß verwendet wird. Entwickler sollten stattdessen Event Grid verwenden.

Event Grid ist für hohe Verfügbarkeit, konsistente Leistung und dynamische Skalierung ausgelegt. Mit Event Grid können Ihre Apps Ereignisse aus praktisch allen Azure-Diensten sowie aus benutzerdefinierten Quellen überwachen und darauf reagieren. Eine einfache, HTTP-basierte, reaktive Ereignisverarbeitung hilft Ihnen mit intelligentem Filtern und Routing von Ereignissen dabei, effiziente Lösungen zu erstellen. Weitere Informationen finden Sie unter [Routing von Ereignissen an einen benutzerdefinierten Webendpunkt](job-state-events-cli-how-to.md).

Der **Auftrag** durchläuft in der Regel die folgenden Zustände: **Geplant**, **In der Warteschlange**, **Verarbeitung** und **Abgeschlossen** (der letzte Zustand). Wenn für den Auftrag ein Fehler aufgetreten ist, erhalten Sie den Zustand **Fehler**. Wenn der Auftrag aktuell abgebrochen wird, erhalten Sie **Abbrechen** und **Abgebrochen**, wenn dies geschehen ist.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#WaitForJobToFinish)]

### <a name="download-the-result-of-the-job"></a>Herunterladen des Ergebnisses des Auftrags

Die folgende Funktion lädt die Ergebnisse aus dem [Ausgabeobjekt](https://docs.microsoft.com/rest/api/media/assets) in den Ordner „output“ herunter, damit Sie die Ergebnisse des Auftrags untersuchen können. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#DownloadResults)]

### <a name="clean-up-resource-in-your-media-services-account"></a>Bereinigen von Ressourcen in Ihrem Media Services-Konto

Im Allgemeinen sollten Sie alles mit Ausnahme der Objekte bereinigen, die Sie wiederverwenden möchten. (In der Regel werden Transformationen wiederverwendet und Streaminglocators gespeichert.) Wenn Sie Ihr Konto nach dem Experimentieren bereinigen möchten, sollten Sie die Ressourcen löschen, bei denen Sie nicht beabsichtigen, sie wiederzuverwenden. Der folgende Code löscht beispielsweise Aufträge.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Drücken Sie STRG+F5, um die *AnalyzeVideos*-Anwendung auszuführen.

Wenn das Programm ausgeführt wird, generiert der Auftrag Miniaturansichten für jedes Gesicht, das im Video gefunden wird. Außerdem wird die Datei „insights.json“ generiert.

## <a name="examine-the-output"></a>Untersuchen der Ausgabe

Die Ausgabedatei der Analyse von Videos wird als „insights.json“ bezeichnet. Diese Datei enthält Informationen zu Ihrem Video. Sie finden eine Beschreibung der Elemente in der JSON-Datei im Artikel [Medienintelligenz](intelligence-concept.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie keine Ressourcen in Ihrer Ressourcengruppe mehr benötigen, einschließlich der Media Services und Speicherkonten, die Sie für dieses Tutorial erstellt haben, löschen Sie die zuvor erstellte Ressourcengruppe. Sie können das **CloudShell**-Tool verwenden.

Führen Sie in **CloudShell** den folgenden Befehl aus:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Multithreading

Die Azure Media Services v3 SDKs sind nicht threadsicher. Beim Arbeiten mit einer Multithreadanwendung sollten Sie ein neues AzureMediaServicesClient-Objekt pro Thread generieren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Hochladen, Codieren und Streamen von Dateien](stream-files-tutorial-with-api.md)

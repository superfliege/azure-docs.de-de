---
title: Livestreaming mit Azure Media Services v3 | Microsoft-Dokumentation
description: In diesem Tutorial wird Schritt für Schritt das Livestreaming mit Media Services v3 unter Verwendung von .NET Core erläutert.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 12/28/2018
ms.author: juliako
ms.openlocfilehash: 858c062c2b3d61b38247e323bf70d2768d33b257
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969334"
---
# <a name="tutorial-stream-live-with-media-services-v3-using-apis"></a>Tutorial: Livestreaming mit Media Services v3 unter Verwendung von APIs

In Azure Media Services sind [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) für die Verarbeitung von Livestreaminginhalten zuständig. Ein Liveereignis stellt einen Eingabeendpunkt (Erfassungs-URL) bereit, den Sie dann für einen Liveencoder bereitstellen. Das Liveereignis empfängt Live-Eingabestreams aus dem Liveencoder und stellt diese zum Streamen durch einen oder mehrere [Streamingendpunkte](https://docs.microsoft.com/rest/api/media/streamingendpoints) (StreamingEndpoints) zur Verfügung. Zudem stellen Liveereignisse einen Vorschauendpunkt (Vorschau-URL) bereit, mit dem Sie eine Vorschau des Streams anzeigen und überprüfen können, bevor Sie ihn weiter verarbeiten und übermitteln. In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von .NET Core ein Liveereignis vom Typ **Pass-Through** erstellen. 

> [!NOTE]
> Lesen Sie [Live streaming with Azure Media Services v3](live-streaming-overview.md) (Livestreaming mit Azure Media Services v3), bevor Sie mit diesem Tutorial fortfahren. 

Das Tutorial veranschaulicht folgende Vorgehensweisen:    

> [!div class="checklist"]
> * Zugreifen auf die Media Services-API
> * Konfigurieren der Beispiel-App
> * Untersuchen des Codes für Livestreaming
> * Ansehen des Ereignisses mit [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) unter http://ampdemo.azureedge.net
> * Bereinigen von Ressourcen

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Lernprogramms müssen folgende Voraussetzungen erfüllt sein:

- Installieren Sie Visual Studio Code oder Visual Studio.
- Installieren und verwenden Sie die CLI lokal. Dieser Artikel erfordert mindestens Version 2.0 der Azure CLI. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

    Derzeit funktionieren nicht alle Befehle der [CLI von Media Services v3](https://aka.ms/ams-v3-cli-ref) in Azure Cloud Shell. Es wird empfohlen, die CLI lokal zu verwenden.

- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)

    Merken Sie sich die Werte, die Sie für den Namen der Ressourcengruppe und des Media Services-Kontos verwendet haben.

- Eine Kamera oder ein Gerät (beispielsweise ein Laptop) zum Übertragen einer Veranstaltung
- Ein lokaler Liveencoder, der Signale von der Kamera in Datenströme konvertiert, die an den Media Services-Livestreamingdienst gesendet werden. Der Datenstrom muss das Format **RTMP** oder **Smooth Streaming** haben.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie ein GitHub-Repository auf Ihren Computer, das das .NET-Streamingbeispiel enthält, indem Sie den folgenden Befehl verwenden:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Das Livestreamingbeispiel befindet sich im Ordner [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

> [!IMPORTANT]
> In diesem Beispiel wird für alle Ressourcen jeweils ein eindeutiges Suffix verwendet. Wenn Sie das Debuggen abbrechen oder die App beenden, ohne das Beispiel vollständig zu durchlaufen, werden in Ihrem Konto mehrere Liveereignisse generiert. <br/>
> Die aktiven Liveereignisse müssen unbedingt beendet werden. Andernfalls werden sie Ihnen **in Rechnung gestellt**.

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>Untersuchen des Codes für Livestreaming

In diesem Abschnitt werden die Funktionen untersucht, die in der Datei [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) des Projekts *MediaV3LiveApp* definiert werden.

Das Beispiel erstellt für jede Ressource ein eindeutiges Suffix, damit es zu keinen Namenskonflikten kommt, wenn Sie das Beispiel mehrmals ausführen, ohne die Ressourcen zu bereinigen.

> [!IMPORTANT]
> In diesem Beispiel wird für alle Ressourcen jeweils ein eindeutiges Suffix verwendet. Wenn Sie das Debuggen abbrechen oder die App beenden, ohne das Beispiel vollständig zu durchlaufen, werden in Ihrem Konto mehrere Liveereignisse generiert. <br/>
> Die aktiven Liveereignisse müssen unbedingt beendet werden. Andernfalls werden sie Ihnen **in Rechnung gestellt**.
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten der Verwendung von Media Services-APIs mit dem .NET SDK

Um mit der Verwendung von Media Services-APIs in .NET zu beginnen, müssen Sie ein **AzureMediaServicesClient**-Objekt erstellen. Zum Erstellen des Objekts müssen Sie Anmeldeinformationen bereitstellen, die für den Client zum Herstellen einer Verbindung mit Azure mithilfe von Azure AD erforderlich sind. In dem Code, den Sie zu Beginn des Artikels geklont haben, erstellt die Funktion **GetCredentialsAsync** das ServiceClientCredentials-Objekt basierend auf den in der lokalen Konfigurationsdatei angegebenen Anmeldeinformationen. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Erstellen eines Liveereignisses

In diesem Abschnitt erfahren Sie, wie Sie ein Liveereignis vom Typ **Pass-Through** erstellen. („LiveEventEncodingType“ ist in diesem Fall auf „None“ festgelegt.) Wenn Sie ein Live Encoding-fähiges Liveereignis erstellen möchten, legen Sie „LiveEventEncodingType“ auf „Standard“ fest. 

Beim Erstellen des Liveereignisses können Sie unter anderem auch folgende Punkte angeben:

* Media Services-Speicherort 
* Streamingprotokoll für das Liveereignis (momentan unterstützte Protokolle: RTMP und Smooth Streaming)
       
    Die Protokolloption kann nicht geändert werden, während das Liveereignis oder die dazugehörigen Liveausgaben (LiveOutputs) aktiv sind. Sollten Sie verschiedene Protokolle benötigen, können Sie für jedes Streamingprotokoll ein separates Liveereignis erstellen.  
* IP-Einschränkungen für Erfassung und Vorschau. Sie können die IP-Adressen definieren, die zum Erfassen eines Videos für dieses Liveereignis zulässig sind. Zulässige IP-Adressen können als einzelne IP-Adresse (Beispiel: 10.0.0.1), als IP-Adressbereiche mit einer IP-Adresse und einer CIDR-Subnetzmaske (Beispiel: 10.0.0.1/22) oder als IP-Adressbereiche mit einer IP-Adresse und einer Subnetzmaske in Punkt-Dezimalschreibweise (Beispiel: 10.0.0.1(255.255.252.0)) angegeben werden.
    
    Wenn keine IP-Adressen angegeben sind und es keine Regeldefinition gibt, sind keine IP-Adressen zulässig. Um alle IP-Adressen zuzulassen, erstellen Sie eine Regel und legen 0.0.0.0/0 fest.

Bei der Ereigniserstellung können Sie angeben, dass das Ereignis automatisch gestartet werden soll. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Abrufen von Erfassungs-URLs

Wenn das Liveereignis erstellt wurde, können Sie Erfassungs-URLs abrufen, die Sie dem Liveencoder bereitstellen. Diese URLs werden vom Encoder zur Eingabe eines Livedatenstroms verwendet.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Abrufen der Vorschau-URL

Verwenden Sie den Vorschauendpunkt (previewEndpoint), um eine Vorschau anzuzeigen und sich zu vergewissern, dass die Eingabe des Encoders auch tatsächlich empfangen wird.

> [!IMPORTANT]
> Vergewissern Sie sich, dass das Video an die Vorschau-URL übertragen wird, bevor Sie fortfahren.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-liveevents-and-liveoutputs"></a>Erstellen und Verwalten von Liveereignissen und Liveausgaben

Wenn der Datenstrom an das Liveereignis übertragen wird, können Sie das Streamingereignis starten, indem Sie ein Medienobjekt, eine Liveausgabe und einen Streaminglocator (StreamingLocator) erstellen. Dadurch wird der Datenstrom archiviert und über den Streamingendpunkt für die Zuschauer verfügbar gemacht. 

#### <a name="create-an-asset"></a>Erstellen eines Medienobjekts

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

Erstellen Sie ein Medienobjekt, das von der Liveausgabe verwendet werden kann.

#### <a name="create-a-liveoutput"></a>Erstellen einer Liveausgabe

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaminglocator"></a>Erstellen eines Streaminglocators

> [!NOTE]
> Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein **Standard**-Streamingendpunkt mit dem Status **Beendet** hinzugefügt. Um mit dem Streamen der Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Status **Wird ausgeführt** aufweisen. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Bereinigen der Ressourcen in Ihrem Media Services-Konto

Wenn die Streamingereignisse beendet sind und Sie die zuvor bereitgestellten Ressourcen bereinigen möchten, führen Sie das folgende Verfahren aus:

* Beenden Sie die Datenstromeingabe vom Encoder.
* Beenden Sie das Liveereignis. Nach Beendigung des Liveereignisses fallen dafür keine Kosten mehr an. Wenn Sie den Kanal erneut starten, weist er die gleiche Erfassungs-URL auf, damit Sie den Encoder nicht erneut konfigurieren müssen.
* Sie können Ihren Streamingendpunkt beenden, sofern Sie das Archiv Ihres Liveereignisses nicht als bedarfsgesteuerten Datenstrom bereitstellen möchten. Nach Beendigung des Liveereignisses fallen dafür keine Kosten mehr an.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupAccount)]   

## <a name="watch-the-event"></a>Ansehen des Ereignisses

Kopieren Sie zum Ansehen des Ereignisses die Streaming-URL, die Sie beim Ausführen des Codes in [Erstellen eines Streaminglocators](#create-a-streaminglocator) erhalten haben, und verwenden Sie einen Player Ihrer Wahl. Sie können den [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) verwenden, um Ihren Datenstrom unter http://ampdemo.azureedge.net zu testen. 

Das Liveereignis konvertiert Ereignisse automatisch in On-Demand-Inhalt, wenn es beendet wird. Auch nach dem Beenden und Löschen des Ereignisses können die Benutzer archivierte Inhalte als bedarfsgesteuertes Video streamen, solange das Medienobjekt nicht gelöscht wurde. Medienobjekte können nicht gelöscht werden, wenn sie von Ereignissen verwendet werden. Zuerst muss das betreffende Ereignis gelöscht werden. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie keine Ressourcen in Ihrer Ressourcengruppe mehr benötigen, einschließlich der Media Services und Speicherkonten, die Sie für dieses Tutorial erstellt haben, löschen Sie die zuvor erstellte Ressourcengruppe.

Führen Sie den folgenden CLI-Befehl aus:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Wenn Sie das Liveereignis nicht beenden, fallen weiter Kosten dafür an. Falls das Projekt/Programm abstürzt oder aus einem anderen Grund geschlossen wird, bleibt das Liveereignis unter Umständen aktiv und verursacht weitere Kosten.

## <a name="next-steps"></a>Nächste Schritte

[Streamen von Dateien](stream-files-tutorial-with-api.md)


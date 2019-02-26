---
title: 'Streamen von Videodateien mit Azure Media Services: CLI | Microsoft-Dokumentation'
description: Führen Sie die in diesem Schnellstart beschriebenen Schritte aus, um ein neues Azure Media Services-Konto zu erstellen, eine Datei zu codieren und in Azure Media Player zu streamen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, streamen
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: ''
ms.date: 02/19/2019
ms.author: juliako
ms.openlocfilehash: 8de004b0ca55cb46336a072dabb682f342c7d8dd
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446493"
---
# <a name="quickstart-stream-video-files---cli"></a>Schnellstart: Streamen von Videodateien: CLI

Dieser Schnellstart zeigt, wie einfach es ist, zu Codieren und mit dem Streaming von Videos für eine Vielzahl von Browsern und Geräten mit Azure Media Services zu beginnen. Ein Eingabeinhalt kann über HTTPS-URLs, SAS-URLs oder Pfade zu Dateien in Azure Blob Storage angegeben werden.
Das Beispiel in diesem Thema codiert Inhalte, die Sie über eine HTTPS-URL zugänglich machen. AMS v3 unterstützt derzeit keine segmentierte Transfercodierung über HTTPS-URLs.

Am Ende des Schnellstarts sind Sie in der Lage, ein Video zu streamen.  

![Video abspielen](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

Um mit dem Verschlüsseln, Codieren, Analysieren, Verwalten und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Das Media Services-Konto muss einem oder mehreren Speicherkonten zugeordnet werden.

Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein. Zur Vermeidung zusätzlicher Wartezeit und Kosten für ausgehenden Datenverkehr wird dringend empfohlen, Speicherkonten zu verwenden, die sich an demselben Ort wie das Media Services-Konto befinden.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

```azurecli
az group create -n amsResourceGroup -l westus2
```

### <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

In diesem Beispiel erstellen Sie ein universelles LRS-Standardkonto der Version 2.

Falls Sie mit Speicherkonten experimentieren möchten, verwenden Sie `--sku Standard_LRS`. Wenn Sie eine SKU für die Produktion auswählen, sollten Sie jedoch die Verwendung von `--sku Standard_RAGRS` erwägen, da diese Option geografische Replikation zum Gewährleisten der Geschäftskontinuität bietet. Weitere Informationen finden Sie unter [Storage accounts](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest) (Speicherkonten).
 
```azurecli
az storage account create -n amsstorageaccount --kind StorageV2 --sku Standard_LRS -l westus2 -g amsResourceGroup
```

### <a name="create-an-azure-media-service-account"></a>Erstellen eines Azure Media Services-Kontos

```azurecli
az ams account create --n amsaccount -g amsResourceGroup --storage-account amsstorageaccount -l westus2
```

Sie erhalten eine Antwort, die in etwa wie folgt aussieht:

```
{
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
  "location": "West US 2",
  "mediaServiceId": "8b569c2e-d648-4fcb-9035-c7fcc3aa7ddf",
  "name": "amsaccount",
  "resourceGroup": "amsResourceGroupTest",
  "storageAccounts": [
    {
      "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Storage/storageAccounts/amsstorageaccount",
      "resourceGroup": "amsResourceGroupTest",
      "type": "Primary"
    }
  ],
  "tags": null,
  "type": "Microsoft.Media/mediaservices"
}
```

## <a name="start-streaming-endpoint"></a>Starten des Streamingendpunkts

Der folgende CLI-Befehl startet den standardmäßigen **Streamingendpunkt**.

```azurecli
az ams streaming-endpoint start  -n default -a amsaccount -g amsResourceGroup
```

Nach seinem Start erhalten Sie eine Antwort, die in etwa wie folgt aussieht:

```
az ams streaming-endpoint start  -n default -a amsaccount -g amsResourceGroup
{
  "accessControl": null,
  "availabilitySetName": null,
  "cdnEnabled": true,
  "cdnProfile": "AzureMediaStreamingPlatformCdnProfile-StandardVerizon",
  "cdnProvider": "StandardVerizon",
  "created": "2019-02-06T21:58:03.604954+00:00",
  "crossSiteAccessPolicies": null,
  "customHostNames": [],
  "description": "",
  "freeTrialEndTime": "2019-02-21T22:05:31.277936+00:00",
  "hostName": "amsaccount-usw22.streaming.media.azure.net",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingendpoints/default",
  "lastModified": "2019-02-06T21:58:03.604954+00:00",
  "location": "West US 2",
  "maxCacheAge": null,
  "name": "default",
  "provisioningState": "Succeeded",
  "resourceGroup": "amsResourceGroup",
  "resourceState": "Running",
  "scaleUnits": 0,
  "tags": {},
  "type": "Microsoft.Media/mediaservices/streamingEndpoints"
}
```

Wenn der Streamingendpunkt bereits ausgeführt wird, erhalten Sie folgende Antwort:

```
(InvalidOperation) The server cannot execute the operation in its current state.
```

## <a name="create-a-transform-for-adaptive-bitrate-encoding"></a>Erstellen einer Transformation für die Adaptive Bitrate-Codierung

Erstellen Sie eine **Transformation**, um allgemeine Aufgaben für die Codierung oder Analyse von Videos zu konfigurieren. In diesem Beispiel möchten wir eine Adaptive Bitrate-Codierung durchführen. Anschließend übermitteln Sie einen **Auftrag** unter der von Ihnen erstellten Transformation. Der Auftrag ist die eigentliche Anforderung an Media Services, die erstellte Transformation auf ein bestimmtes Eingabevideo oder auf einen Audioinhalt anzuwenden.

```azurecli
az ams transform create --name testEncodingTransform --preset AdaptiveStreaming --description 'a simple Transform for Adaptive Bitrate Encoding' -g amsResourceGroup -a amsaccount
```

Sie erhalten eine Antwort, die in etwa wie folgt aussieht:

```
{
  "created": "2019-02-15T00:11:18.506019+00:00",
  "description": "a simple Transform for Adaptive Bitrate Encoding",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform",
  "lastModified": "2019-02-15T00:11:18.506019+00:00",
  "name": "testEncodingTransform",
  "outputs": [
    {
      "onError": "StopProcessingJob",
      "preset": {
        "odatatype": "#Microsoft.Media.BuiltInStandardEncoderPreset",
        "presetName": "AdaptiveStreaming"
      },
      "relativePriority": "Normal"
    }
  ],
  "resourceGroup": "amsResourceGroup",
  "type": "Microsoft.Media/mediaservices/transforms"
}
```

## <a name="create-an-output-asset"></a>Erstellen eines Ausgabemedienobjekts

Der folgende Befehl erstellt ein **Ausgabemedienobjekt**, das als Ausgabe des Codierungsauftrags verwendet wird.

```azurecli
az ams asset create -n testOutputAssetName -a amsaccount -g amsResourceGroup
```

Sie erhalten eine Antwort, die in etwa wie folgt aussieht:

```
{
  "alternateId": null,
  "assetId": "96427438-bbce-4a74-ba91-e38179b72f36",
  "container": null,
  "created": "2019-02-14T23:58:19.127000+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/assets/testOutputAssetName",
  "lastModified": "2019-02-14T23:58:19.127000+00:00",
  "name": "testOutputAssetName",
  "resourceGroup": "amsResourceGroup",
  "storageAccountName": "amsstorageaccount",
  "storageEncryptionFormat": "None",
  "type": "Microsoft.Media/mediaservices/assets"
}
```

## <a name="start-job-with-https-input"></a>Starten des Auftrags mit einer HTTPS-Eingabe

Wenn Sie in Media Services v3 Aufträge zur Verarbeitung Ihrer Videos übermitteln, müssen Sie Media Services mitteilen, wo sich das Eingabevideo befindet. Eine der Optionen ist die Angabe einer HTTPS-URL als Auftragseingabe (wie im folgenden Beispiel gezeigt). 

Beim Ausführen von `az ams job start` können Sie eine Bezeichnung für die Ausgabe des Auftrags festlegen. Die Bezeichnung kann später verwendet werden, um den Verwendungszweck des Ausgabemedienobjekts festzustellen. 

- Wenn Sie der Bezeichnung einen Wert zuweisen, legen Sie „--output-assets“ auf „assetname=label“ fest.
- Wenn Sie der Bezeichnung keinen Wert zuweisen, legen Sie „--output-assets“ auf „assetname=“ fest.
  Beachten Sie, dass Sie `output-assets` ein Gleichheitszeichen (=) hinzufügen. 

```azurecli
az ams job start --name testJob001 --transform-name testEncodingTransform --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' --files 'Ignite-short.mp4' --output-assets testOutputAssetName= -a amsaccount -g amsResourceGroup 
```

Sie erhalten eine Antwort, die in etwa wie folgt aussieht:

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

### <a name="check-status"></a>Status überprüfen

Überprüfen Sie nach fünf Minuten den Status des Auftrags. Der Status sollte „Beendet“ lauten. Wenn dies nicht der Fall ist, überprüfen Sie den Status in ein paar Minuten noch einmal. Sobald der Status „Beendet“ angezeigt wird, fahren Sie mit dem nächsten Schritt fort, um einen **Streaminglocator** zu erstellen.

```azurecli
az ams job show -a amsaccount -g amsResourceGroup -t testEncodingTransform -n testJob001
```

## <a name="create-streaming-locator-and-get-path"></a>Erstellen eines Streaminglocators und Abrufen des Pfads

Nachdem die Codierung abgeschlossen ist, besteht der nächste Schritt darin, das Video im Ausgabeobjekt für Clients zur Wiedergabe verfügbar zu machen. Sie können dies in zwei Schritten bewerkstelligen: Erstellen Sie zunächst einen **Streaminglocator** und dann die Streaming-URLs, die Clients verwenden können.

### <a name="create-a-streaming-locator"></a>Erstellen eines Streaminglocators

```azurecli
az ams streaming-locator create -n testStreamingLocator --asset-name testOutputAssetName --streaming-policy-name Predefined_ClearStreamingOnly  -g amsResourceGroup -a amsaccount 
```

Sie erhalten eine Antwort, die in etwa wie folgt aussieht:

```
{
  "alternativeMediaId": null,
  "assetName": "output-3b6d7b1dffe9419fa104b952f7f6ab76",
  "contentKeys": [],
  "created": "2019-02-15T04:35:46.270750+00:00",
  "defaultContentKeyPolicyName": null,
  "endTime": "9999-12-31T23:59:59.999999+00:00",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingLocators/testStreamingLocator",
  "name": "testStreamingLocator",
  "resourceGroup": "amsResourceGroup",
  "startTime": null,
  "streamingLocatorId": "e01b2be1-5ea4-42ca-ae5d-7fe704a5962f",
  "streamingPolicyName": "Predefined_ClearStreamingOnly",
  "type": "Microsoft.Media/mediaservices/streamingLocators"
}
```

### <a name="get-streaming-locator-paths"></a>Abrufen der Streaminglocatorpfade

```azurecli
az ams streaming-locator get-paths -a amsaccount -g amsResourceGroup -n testStreamingLocator
```

Sie erhalten eine Antwort, die in etwa wie folgt aussieht:

```
{
  "downloadPaths": [],
  "streamingPaths": [
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=m3u8-aapl)"
      ],
      "streamingProtocol": "Hls"
    },
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=mpd-time-csf)"
      ],
      "streamingProtocol": "Dash"
    },
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest"
      ],
      "streamingProtocol": "SmoothStreaming"
    }
  ]
}
```

Kopieren Sie den Hls-Pfad. In diesem Fall lautet der Pfad wie folgt: `/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=m3u8-aapl)`.

## <a name="build-url"></a>Erstellen der URL 

### <a name="get-streaming-endpoint-host-name"></a>Abrufen des Hostnamens des Streamingendpunkts

```azurecli
az ams streaming-endpoint list -a amsaccount -g amsResourceGroup -n default
```

Kopieren Sie den `hostName`-Wert. In diesem Fall lautet der Wert wie folgt: `amsaccount-usw22.streaming.media.azure.net`.

### <a name="assemble-url"></a>Assemblieren der URL

„https://“ + &lt;hostName-Wert&gt; + &lt;Wert des Hls-Pfads&gt;

#### <a name="example"></a>Beispiel

`https://amsaccount-usw22.streaming.media.azure.net/7f19e783-927b-4e0a-a1c0-8a140c49856c/ignite.ism/manifest(format=m3u8-aapl)`

## <a name="test-playback-with-azure-media-player"></a>Testen der Wiedergabe mit dem Azure Media Player

Um den Stream zu testen, wird in diesem Artikel Azure Media Player verwendet. 

> [!NOTE]
> Wenn ein Player auf einer HTTPS-Website gehostet wird, stellen Sie sicher, die URL mit „https“ zu aktualisieren.

1. Öffnen Sie einen Webbrowser, und navigieren Sie zu [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/).
2. Fügen Sie die URL, die Sie im vorherigen Abschnitt erstellt haben, in das Feld **URL** ein. 

  Sie können die URL im HLS-, Dash- oder Smooth-Format einfügen. Der Azure Media Player wechselt auf Ihrem Gerät automatisch zu einem geeigneten Streamingprotokoll für die Wiedergabe.
3. Drücken Sie **Player aktualisieren**.

Azure Media Player kann zum Testen verwendet werden, sollte jedoch nicht in einer Produktionsumgebung zum Einsatz kommen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen in der Ressourcengruppe einschließlich der in dieser Schnellstartanleitung erstellten Media Services- und Speicherkonten nicht mehr benötigen, löschen Sie die Ressourcengruppe.

Führen Sie den folgenden CLI-Befehl aus:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="see-also"></a>Weitere Informationen

Siehe [Job error codes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode) (Auftragsfehlercodes).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [CLI-Beispiele](cli-samples.md)

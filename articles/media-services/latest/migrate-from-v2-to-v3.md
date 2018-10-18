---
title: Migrieren von Azure Media Services V2 zu V3 | Microsoft-Dokumentation
description: Dieser Artikel beschreibt Änderungen, die in Azure Media Services V3 eingeführt wurden, und zeigt Unterschiede zwischen den beiden Versionen.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, streamen, übertragen, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: a17bad5beb651aaa085c626296c200a00c30647e
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376361"
---
# <a name="migrate-from-media-services-v2-to-v3"></a>Migrieren von Media Services V2 zu V3

Dieser Artikel beschreibt Änderungen, die in Azure Media Services V3 eingeführt wurden, und zeigt die Unterschiede zwischen den beiden Versionen.

## <a name="why-should-you-move-to-v3"></a>Warum sollten Sie zu V3 wechseln?

### <a name="api-is-more-approachable"></a>Besser zugängliche API

*  V3 basiert auf einer einheitlichen API-Oberfläche, die sowohl Verwaltungs- als auch Betriebsfunktionen auf der Basis von Azure Resource Manager bereitstellt. Azure Resource Manager-Vorlagen können zum Erstellen und Bereitstellen von Transformationen, Streamingendpunkten, LiveEvents und mehr verwendet werden.
* Open API (oder auch Swagger)-Spezifikationsdokument.
* SDKs verfügbar für .NET, .Net Core, Node.js, Python, Java, Ruby.
* Azure-CLI-Integration.

### <a name="new-features"></a>Neue Funktionen

* Die Codierung unterstützt jetzt HTTPS-Erfassung (URL-gestützte Eingabe).
* Transformationen sind neu in V3. Eine Transformation wird zum Freigeben von Konfigurationen, zum Erstellen von Azure Resource Manager-Vorlagen und zum Isolieren von Codierungseinstellungen für einen bestimmten Kunden oder Mandanten verwendet. 
* Ein Medienobjekt kann mehrere StreamingLocators mit verschiedenen Einstellungen für die dynamische Paketerstellung und dynamische Verschlüsselung haben.
* Der Inhaltsschutz unterstützt Features mit mehreren Schlüsseln. 
* Die LiveEvent-Vorschau unterstützt die dynamische Paketerstellung und dynamische Verschlüsselung. Dadurch wird in der Vorschau der Inhaltsschutz sowie auch die Paketerstellung für DASH und HLS ermöglicht.
* LiveOuput ist einfacher zu verwenden als die ältere Programmentität. 
* RBAC-Unterstützung für Entitäten wurde hinzugefügt.

## <a name="changes-from-v2"></a>Änderungen von V2

* In Media Services v3 wird Speicherverschlüsselung (AES-256-Verschlüsselung) nur für die Abwärtskompatibilität unterstützt, wenn Ihre Ressourcen mit Media Services v2 erstellt wurden. Dies bedeutet, dass v3 mit vorhandenen speicherverschlüsselten Ressourcen funktioniert, jedoch nicht die Erstellung neuer zulässt.

    Für Ressourcen, die mit v3 erstellt wurden, unterstützt Media Services die serverseitige Speicherverschlüsselung, siehe [Azure Storage Service Encryption für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
    
* Die Media Services-SDKs wurden vom Storage-SDK entkoppelt, wodurch sich eine bessere Kontrolle über das verwendete Storage-SDK ergibt und Versionsprobleme vermieden werden. 
* In V3 werden alle Codierungsbitraten in Bits pro Sekunde angegeben. Dies unterscheidet sich von den Voreinstellungen von REST V2 Media Encoder Standard. Beispielsweise würde die Bitrate in V2 mit 128, in V3 jedoch mit 128.000 angegeben. 
* AssetFiles, AccessPolicies, IngestManifests sind in V3 nicht vorhanden.
* ContentKeys sind keine Entität mehr, sondern eine Eigenschaft von StreamingLocator.
* Die Event Grid-Unterstützung ersetzt NotificationEndpoints.
* Einige Entitäten wurden umbenannt

  * JobOutput ersetzt Aufgabe, jetzt nur Teil des Auftrags.
  * LiveEvent ersetzt Kanal.
  * LiveOutput ersetzt Programm.
  * StreamingLocator ersetzt Locator.

## <a name="code-changes"></a>Änderungen am Code

### <a name="create-an-asset-and-upload-a-file"></a>Erstellen eines Medienobjekts und Hochladen einer Datei 

#### <a name="v2"></a>V2

```csharp
IAsset asset = context.Assets.Create(assetName, storageAccountName, options);

IAssetFile assetFile = asset.AssetFiles.Create(assetFileName);

assetFile.Upload(filePath);
```

#### <a name="v3"></a>V3

```csharp
Asset asset = client.Assets.CreateOrUpdate(resourceGroupName, accountName, assetName, new Asset());

var response = client.Assets.ListContainerSas(resourceGroupName, accountName, assetName, permissions: AssetContainerPermission.ReadWrite, expiryTime: DateTime.Now.AddHours(1));

var sasUri = new Uri(response.AssetContainerSasUrls.First());
CloudBlobContainer container = new CloudBlobContainer(sasUri);

var blob = container.GetBlockBlobReference(Path.GetFileName(fileToUpload));
blob.UploadFromFile(fileToUpload);
```

### <a name="submit-a-job"></a>Übermitteln eines Auftrags

#### <a name="v2"></a>V2

```csharp
IMediaProcessor processor = context.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

IJob job = jobs.Create($"Job for {inputAsset.Name}");

ITask task = job.Tasks.AddNew($"Task for {inputAsset.Name}", processor, taskConfiguration);

task.InputAssets.Add(inputAsset);

task.OutputAssets.AddNew(outputAssetName, outputAssetStorageAccountName, outputAssetOptions);

job.Submit();
```

#### <a name="v3"></a>V3

```csharp
client.Assets.CreateOrUpdate(resourceGroupName, accountName, outputAssetName, new Asset());

JobOutput[] jobOutputs = { new JobOutputAsset(outputAssetName)};

JobInput jobInput = JobInputAsset(assetName: assetName);

Job job = client.Jobs.Create(resourceGroupName,
accountName, transformName, jobName,
new Job {Input = jobInput, Outputs = jobOutputs});
```

### <a name="publish-an-asset-with-aes-encryption"></a>Veröffentlichen eines Medienobjekts mit AES-Verschlüsselung 

#### <a name="v2"></a>V2

1. Erstellen von ContentKeyAuthorizationPolicyOption
2. Erstellen von ContentKeyAuthorizationPolicy
3. Erstellen von AssetDeliveryPolicy
4. Erstellen eines Medienobjekts und Hochladen von Inhalten ODER Übermitteln eines Auftrags und Verwenden des Ausgabemedienobjekts
5. Zuordnen von AssetDeliveryPolicy zu Medienobjekt
6. Erstellen von ContentKey
7. Zuordnen von ContentKey zu einem Medienobjekt
8. Erstellen von AccessPolicy
9. Erstellen von Locator

#### <a name="v3"></a>V3

1. Erstellen einer Richtlinie für Inhaltsschlüssel
2. Erstellen eines Medienobjekts
3. Hochladen von Inhalten oder Verwenden von Medienobjekt als JobOutput
4. Erstellen von StreamingLocator

## <a name="next-steps"></a>Nächste Schritte

Um zu erfahren, wie einfach es ist, mit der Codierung und dem Streaming von Videodateien zu beginnen, lesen Sie [Streamen von Dateien](stream-files-dotnet-quickstart.md). 


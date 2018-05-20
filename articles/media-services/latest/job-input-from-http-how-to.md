---
title: Erstellen einer Azure Media Services-Auftragseingabe aus einer HTTP(S)-URL | Microsoft-Dokumentation
description: In diesem Thema wird gezeigt, wie Sie eine Auftragseingabe aus einer HTTP(S)-URL erstellen.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: d429665de64dacc5818d1d26c2a9029531cd39b3
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="create-a-job-input-from-an-https-url"></a>Erstellen einer Auftragseingabe aus einer HTTP(S)-URL

Wenn Sie in Media Services v3 Aufträge zur Verarbeitung Ihrer Videos übermitteln, müssen Sie Media Services mitteilen, wo sich das Eingabevideo befindet. Eine der Optionen ist die Angabe einer HTTP(S)-URL als Auftragseingabe (wie im folgenden Beispiel gezeigt). Ein vollständiges Beispiel finden Sie in diesem [GitHub-Beispiel](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-Beispiel

Der folgende Code zeigt, wie Sie einen Auftrag mit einer HTTPS-URL-Eingabe erstellen.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md)

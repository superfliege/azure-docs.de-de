---
title: Abrufen eines Signaturschlüssels aus der vorhandenen Richtlinie mit Media Services V3 .NET SDK – Azure | Microsoft-Dokumentation
description: In diesem Thema wird gezeigt, wie Sie mit Media Services V3 .NET SDK einen Signaturschlüssel aus der vorhandenen Richtlinie abrufen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 04/09/2019
ms.author: juliako
ms.openlocfilehash: 49cc2b8c151053377f8f1da0792f10a06695b332
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471170"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Abrufen eines Signaturschlüssels aus der vorhandenen Richtlinie

Eines der wichtigsten Entwurfsprinzipien der v3-API ist es, die API sicherer zu machen. v3-APIs geben bei einem **Get**- oder **List**-Vorgang keine geheimen Schlüssel oder Anmeldeinformationen zurück. Die Schlüssel sind immer NULL, leer oder aus der Antwort bereinigt. Der Benutzer muss eine separate Aktionsmethode zum Abrufen von geheimen Schlüsseln oder Anmeldeinformationen aufrufen. Die **Leser**-Rolle kann keine Vorgänge aufrufen, also auch keine Vorgänge wie „Asset.ListContainerSas“, „StreamingLocator.ListContentKeys“ oder „ContentKeyPolicies.GetPolicyPropertiesWithSecrets“. Getrennte Aktionen ermöglichen es Ihnen, bei Bedarf noch genauer abgestimmte RBAC-Sicherheitsberechtigungen in einer benutzerdefinierten Rolle festzulegen.

Weitere Informationen finden Sie unter [RBAC and Media Services accounts (RBAC und Media Services-Konten)](rbac-overview.md).

Anhand des Beispiels in diesem Artikel wird veranschaulicht, wie .NET verwendet wird, um einen Signaturschlüssel aus der vorhandenen Richtlinie abzurufen. 
 
## <a name="download"></a>Download 

Klonen Sie ein GitHub-Repository auf Ihren Computer, das das vollständige .NET-Beispiel enthält, indem Sie den folgenden Befehl verwenden:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Die Richtlinie „ContentKeyPolicy“ mit Beispiel für Geheimnisse befindet sich im Ordner [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM).

## <a name="get-contentkeypolicy-with-secrets"></a>Abrufen von „ContentKeyPolicy“ mit Geheimnissen 

Um den Schlüssel abzurufen, verwenden Sie **GetPolicyPropertiesWithSecretsAsync**, wie im folgenden Beispiel gezeigt.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Nächste Schritte

[Entwurf eines Multi-DRM-Inhaltsschutzsystems mit Zugriffssteuerung](design-multi-drm-system-with-access-control.md) 

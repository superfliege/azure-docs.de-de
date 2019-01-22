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
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: 882f4650c0a3d558ee06c96658b779f9f0c76f76
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2019
ms.locfileid: "54322483"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Abrufen eines Signaturschlüssels aus der vorhandenen Richtlinie

Eines der wichtigsten Entwurfsprinzipien der v3-API ist es, die API sicherer zu machen. v3-APIs geben bei einem **Get**- oder **List**-Vorgang keine geheimen Schlüssel oder Anmeldeinformationen zurück. Die Schlüssel sind immer NULL, leer oder aus der Antwort bereinigt. Sie müssen eine separate Aktionsmethode zum Abrufen von geheimen Schlüsseln oder Anmeldeinformationen aufrufen. Separate Aktionen ermöglichen es Ihnen, verschiedene RBAC-Sicherheitsberechtigungen festzulegen, falls einige APIs geheime Schlüssel abrufen/anzeigen, während dies bei anderen APIs nicht der Fall ist. Informationen darüber, wie Sie den Zugriff über RBAC verwalten können, finden Sie unter [Verwenden von RBAC zum Verwalten des Zugriffs](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Beispiele hierfür sind: 

* Keine Rückgabe von ContentKey-Werten im Get-Vorgang von StreamingLocator. 
* Keine Rückgabe der Einschränkungsschlüssel im Get-Vorgang von ContentKeyPolicy. 
* Keine Rückgabe des Abfragezeichenfolgeteils der URL (um die Signatur zu entfernen) der HTTP-Eingabe-URLs von Aufträgen.

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

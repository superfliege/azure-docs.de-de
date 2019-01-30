---
title: Übersicht über Azure Media Services-Entitäten – Azure | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über Azure Media Services-Entitäten.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 902014ff3c242a18b3872ba490845eb0923f39a4
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54451186"
---
# <a name="azure-media-services-entities-overview"></a>Übersicht über Azure Media Services-Entitäten

Dieser Artikel bietet eine kurze Übersicht über Azure Media Services-Entitäten und verweist für jede Entität auf einen Artikel mit weiteren Informationen. 

| Thema | BESCHREIBUNG |
|---|---|
| [Konto- und Medienobjektfilter](filters-dynamic-manifest-overview.md)|Bei der Inhaltsbereitstellung für Ihre Kunden (Streaming von Liveereignissen oder Video on Demand) benötigen Ihre Kunden möglicherweise mehr Flexibilität als in der Manifestdatei für das Standardmedienobjekt beschrieben. Mit Azure Media Services können Sie [Kontofilter](https://docs.microsoft.com/rest/api/media/accountfilters) und [Medienobjektfilter](https://docs.microsoft.com/rest/api/media/assetfilters) definieren. Verwenden Sie anschließend **dynamische Manifeste** basierend auf den vordefinierten Filtern. |
| [Medienobjekte](assets-concept.md)|Die Entität [Medienobjekt](https://docs.microsoft.com/rest/api/media/assets) enthält digitale Dateien (z.B. Video, Audio, Bilder, Sammlungen von Miniaturansichten, Textspuren und Untertiteldateien) sowie die Metadaten zu diesen Dateien. Nachdem die digitalen Dateien in ein Medienobjekt geladen wurden, können Sie in den Media Services-Workflows zum Codieren, Streamen und Analysieren von Inhalten verwendet werden.|
| [Richtlinien für Inhaltsschlüssel](content-key-policy-concept.md)|Mit Media Services können Sie Ihre Medien ab dem Zeitpunkt, an dem sie Ihren Computer verlassen, während des gesamten Prozesses der Speicherung, Verarbeitung und Übermittlung sichern. Mit Media Services können Sie Ihre zu übermittelnden Live- und On-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients.|
| [LiveEvents und LiveOutputs](live-events-outputs-concept.md)|Mit Media Services können Sie Ihren Kunden Liveereignisse in der Azure-Cloud anbieten. Um Ihre Livestreamingereignisse in Media Services v3 zu konfigurieren, müssen Sie sich über [Liveereignisse](https://docs.microsoft.com/rest/api/media/liveevents) und [Liveausgaben](https://docs.microsoft.com/rest/api/media/liveoutputs) informieren.|
| [Streamingendpunkte](streaming-endpoint-concept.md)|Die Entität [Streamingendpunkte](https://docs.microsoft.com/rest/api/media/streamingendpoints) stellt einen Streamingdienst dar, der Inhalte zur weiteren Verteilung direkt in einer Clientwiedergabeanwendung oder einem Content Delivery Network (CDN) bereitstellen kann. Der ausgehende Stream des Streamingendpunkt-Diensts kann ein Livestream oder ein Video-on-Demand-Medienobjekt in Ihrem Media Services-Konto sein. Beim Erstellen eines Media Services-Kontos wird ein **Standard**-Streamingendpunkt mit dem Zustand „Beendet“ erstellt. Der **Standard**-Streamingendpunkt kann nicht gelöscht werden. Im Konto können zusätzliche Streamingendpunkte erstellt werden. Um das Streaming von Videos zu starten, muss der Streamingendpunkt gestartet werden, von dem aus Sie Ihr Video streamen möchten. |
| [Streaminglocators](streaming-locators-concept.md)|Sie müssen Ihren Clients eine URL bereitstellen, mit der sie codierte Video- oder Audiodateien wiedergeben können. Dazu müssen Sie einen [Streaminglocator](https://docs.microsoft.com/rest/api/media/streaminglocators) und die Streaming-URLs erstellen.|
| [Streamingrichtlinien](streaming-policy-concept.md)| Mithilfe von [Streamingrichtlinien](https://docs.microsoft.com/rest/api/media/streamingpolicies) können Sie Streamingprotokolle und Verschlüsselungsoptionen für Ihre StreamingLocators definieren. Sie können entweder den Namen einer von Ihnen erstellten benutzerdefinierten Streamingrichtlinie angeben oder eine der vordefinierten Streamingrichtlinien verwenden, die von Media Services angeboten werden. <br/><br/>Wenn Sie eine benutzerdefinierte Streamingrichtlinie verwenden, sollten Sie eine begrenzte Sammlung solcher Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Verschlüsselungsoptionen und Protokolle benötigt werden. Sie sollten nicht für jeden Streaminglocator eine neue Streamingrichtlinie erstellen.|
| [Transformationen und Aufträge](transforms-jobs-concept.md)|Verwenden Sie [Transformationen](https://docs.microsoft.com/rest/api/media/transforms), um allgemeine Aufgaben zur Codierung oder Analyse von Videos zu konfigurieren. Jede **Transformation** beschreibt eine Vorgehensweise oder einen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien.<br/><br/>Ein [Auftrag](https://docs.microsoft.com/rest/api/media/jobs) ist die eigentliche Anforderung an Azure Media Services, die erstellte **Transformation** auf ein bestimmtes Eingabevideo oder auf einen Audioinhalt anzuwenden. Der **Auftrag** gibt Informationen wie den Speicherort des Eingabevideos und den Speicherort für die Ausgabe an. Sie können den Speicherort Ihres Eingabevideos über HTTPS-URLs, SAS-URLs oder ein Medienobjekt angeben.|

## <a name="next-steps"></a>Nächste Schritte

[Streamen einer Datei](stream-files-dotnet-quickstart.md)

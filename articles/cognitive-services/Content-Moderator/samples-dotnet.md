---
title: Codebeispiele – Content Moderator, .NET
description: Verwenden von Content Moderator in Ihren .NET-Anwendungen über das SDK
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: sample
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 43aa1ca624bce78fa113c34fa19da9ccfea69bbc
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222098"
---
# <a name="content-moderator-net-sdk-samples"></a>Content Moderator: .NET-SDK-Beispiele

Die folgende Liste enthält Links zu den Codebeispielen, die mit dem Azure Content Moderator SDK für .NET erstellt wurden.

- **Unterstützende Bibliothek:** [Erstellen Sie einen Content Moderator-Client für die Verwendung in anderen Beispielen.](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs) Weitere Informationen finden Sie im [Schnellstart](content-moderator-helper-quickstart-dotnet.md).

## <a name="moderation"></a>Moderation

- **Bildmoderation:** [Werten Sie Bilder für nicht jugendfreie Inhalte, Texte und Gesichter aus.](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs) Weitere Informationen finden Sie im [Schnellstart](image-moderation-quickstart-dotnet.md).
- **Benutzerdefinierte Bilder:** [Moderieren Sie Bilder mit benutzerdefinierten Bildlisten.](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs) Weitere Informationen finden Sie im [Schnellstart](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Die Obergrenze liegt bei **fünf Bildlisten**, wobei jede Liste **max. 10.000 Bilder** enthalten kann.
>

- **Textmoderation:** [Prüfen Sie Texte auf unerwünschte Ausdrücke und personenbezogene Informationen (Personally Identifiable Information, PII).](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs) Weitere Informationen finden Sie im [Schnellstart](text-moderation-quickstart-dotnet.md).
- **Benutzerdefinierte Benennungen:** [Moderieren Sie Benennungen mit benutzerdefinierten Benennungslisten.](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs) Weitere Informationen finden Sie im [Schnellstart](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Die Obergrenze liegt bei **fünf Benennungslisten**, wobei jede Liste **max. 10.000 Benennungen** enthalten kann.
>

- **Videomoderation:** [Prüfen Sie ein Video auf nicht jugendfreie Inhalte, und rufen Sie die Ergebnisse ab.](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs) Weitere Informationen finden Sie im [Schnellstart](video-moderation-api.md).

## <a name="review"></a>Überprüfung

- **Bildaufträge:** [Starten Sie einen Moderationsauftrag, der Überprüfungen durchführt und erstellt.](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs) Weitere Informationen finden Sie im [Schnellstart](moderation-jobs-quickstart-dotnet.md).
- **Bildüberprüfungen:** [Erstellen Sie Überprüfungen für HITL-Simulationen (Human-in-the-Loop).](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs) Weitere Informationen finden Sie im [Schnellstart](moderation-reviews-quickstart-dotnet.md).
- **Videoüberprüfungen:** [Erstellen Sie Videoüberprüfungen für HITL-Simulationen (Human-in-the-Loop).](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs) Weitere Informationen finden Sie im [Schnellstart](video-reviews-quickstart-dotnet.md).
- **Überprüfungen von Videotranskripts:** [Erstellen Sie Überprüfungen von Videotranskripts für HITL-Simulationen (Human-in-the-Loop).](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Weitere Informationen finden Sie im [Schnellstart](video-reviews-quickstart-dotnet.md).

Alle .NET-Beispiele finden Sie auf [GitHub unter den .NET-Beispielen zu Content Moderator](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).

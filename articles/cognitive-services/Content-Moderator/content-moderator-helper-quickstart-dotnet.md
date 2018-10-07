---
title: 'Schnellstart: Content Moderator SDK für die .NET-Hilfsmethode'
titlesuffix: Azure Cognitive Services
description: Rückgabe eines Content Moderator-Clients mit dem Content Moderator SDK für .NET
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 0c167de3af8aea6820962e7cc8d0c21f5ba61807
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223269"
---
# <a name="quickstart-helper-code-to-return-a-content-moderator-client"></a>Schnellstart: Hilfscode zum Zurückgeben eines Content Moderator-Clients

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des Content Moderator SDK für .NET erleichtern, um einen Content Moderator-Client für Ihr Abonnement zu erstellen.

Die Bibliothek wird von anderen Schnellstarts in diesem Abschnitt verwendet.

In diesem Artikel wird davon ausgegangen, dass Sie bereits mit Visual Studio und C# vertraut sind.

> [!IMPORTANT]
> Diese Klassenbibliothek enthält Code, der nur für Demonstrationszwecke gedacht ist.
> Wenn Sie diesen Code für die Verwendung in der Produktion anpassen, verwenden Sie eine sichere Methode zum Speichern und Verwenden Ihres Content Moderator-Abonnementschlüssels.

## <a name="sign-up-for-content-moderator-services"></a>Registrieren für Content Moderator-Dienste

Um Content Moderator-Dienste über die REST-API oder über das SDK verwenden zu können, benötigen Sie einen Abonnementschlüssel.
Informationen zum Abrufen des Schlüssels finden Sie in [diesem Schnellstart](quick-start.md).

## <a name="create-your-visual-studio-project"></a>Erstellen Ihres Visual Studio-Projekts

1. Erstellen Sie ein neues **Class Library (.NET Framework)**-Projekt.

   In diesem Beispielcode habe ich das Projekt mit **ModeratorHelper** bezeichnet.

1. Fügen Sie einen Verweis auf die **System.Configuration**-Frameworkassembly hinzu.

### <a name="install-required-packages"></a>Installieren erforderlicher Pakete

Installieren Sie die folgenden NuGet-Pakete:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>Erstellen des Content Moderator-Clients

Ersetzen Sie den Inhalt der Datei „ModeratorHelper.cs“ durch den folgenden Code:

    using Microsoft.CognitiveServices.ContentModerator;

    namespace ModeratorHelper
    {
    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "myRegion";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "myKey";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }
    }


> [!IMPORTANT]
> Aktualisieren Sie die Felder **AzureRegion** und **CMSubscriptionKey** mit den Werten Ihres Regionsbezeichners und des Abonnementschlüssels.

Jetzt kennen Sie einen schnelle Möglichkeit, wie Sie einen Content Moderator-Client für Ihr Abonnement erstellen.

## <a name="next-steps"></a>Nächste Schritte

[Laden Sie die entsprechende Visual Studio-Projektmappe sowie andere Content Moderator-Schnellstarts für .NET herunter](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator), und beginnen Sie mit der Integration.

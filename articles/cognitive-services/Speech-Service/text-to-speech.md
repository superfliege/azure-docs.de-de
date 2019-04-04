---
title: Text-zu-Sprache mit den Azure-Spracherkennungsdiensten
titleSuffix: Azure Cognitive Services
description: Text-zu-Sprache aus den Azure-Spracherkennungsdiensten ist ein REST-basierter Dienst, der es Ihren Anwendungen, Tools oder Geräten ermöglicht, Text in natürliche, menschenähnliche synthetisierte Sprache zu konvertieren. Wählen Sie aus Standard- und neuronalen Stimmen, oder erstellen Sie Ihre eigene, benutzerdefinierte Stimme, die einzigartig für Ihr Produkt oder Ihre Marke ist. Mehr als 75 Standardstimmen sind in mehr als 45 Sprachen und Gebietsschemas verfügbar, und 5 neuronale Stimmen sind in 4 Sprachen und Gebietsschemas verfügbar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 1edc2587ef8680f60082bf6271b73d30184f331b
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521243"
---
# <a name="what-is-text-to-speech"></a>Was ist Text-zu-Sprache?

Text-zu-Sprache aus den Azure-Spracherkennungsdiensten ist ein REST-basierter Dienst, der es Ihren Anwendungen, Tools oder Geräten ermöglicht, Text in natürliche, menschenähnliche synthetisierte Sprache zu konvertieren. Wählen Sie aus Standard- und neuronalen Stimmen, oder erstellen Sie Ihre eigene, benutzerdefinierte Stimme, die einzigartig für Ihr Produkt oder Ihre Marke ist. Mehr als 75 Standardstimmen sind in mehr als 45 Sprachen und Gebietsschemas verfügbar, und 5 neuronale Stimmen sind in 4 Sprachen und Gebietsschemas verfügbar. Eine vollständige Liste finden Sie unter [Unterstützte Sprachen](language-support.md#text-to-speech).

Die Text-zu-Sprache-Technologie ermöglicht Inhaltserstellern, auf unterschiedliche Weise mit ihren Benutzern zu interagieren. Text-zu-Sprache kann die Barrierefreiheit verbessern, indem Benutzern eine Option für die akustische Interaktion mit dem Inhalt zur Verfügung gestellt wird. Unabhängig davon, ob der Benutzer eine Sehbehinderung oder eine Lernbehinderung hat oder während der Fahrt Navigationsinformationen benötigt, kann Text-zu-Sprache ein vorhandenes Erlebnis verbessern. Sprachsynthese ist auch ein wertvolles Add-On für Sprachbots und virtuelle Assistenten.

### <a name="neural-voices"></a>Neuronale Stimmen

Gestalten Sie mit neuronalen Stimmen Interaktionen mit Chatbots und virtuellen Helfern noch natürlicher und einladender, wandeln Sie digitale Texte wie E-Books in Audiobooks um, und verpassen Sie Ihrem Navigationssystem im Auto ein Upgrade. Durch natürliche, menschenähnliche Intonation und klare Aussprache von Wörtern können neuronale Stimmen die Hörermüdung bei der Interaktion mit KI-Systemen erheblich verringern. Weitere Informationen zu unterstützten neuronalen Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Benutzerdefinierte Stimmen

Durch Stimmanpassungen können Sie eine wiedererkennbare, einzigartige Stimme für Ihre Marke erstellen. Zur Erstellung Ihres benutzerdefinierten Voicefonts laden Sie eine Tonstudioaufnahme und die zugehörigen Skripts als Trainingsdaten hoch. Der Dienst erstellt daraufhin ein individuelles Stimmenmodell, das auf Ihre Aufnahme abgestimmt ist. Diesen benutzerdefinierten Voicefont können Sie zum Synthetisieren von Sprache verwenden. Weitere Informationen finden Sie unter [Benutzerdefinierte Stimmen](how-to-customize-voice-font.md).

## <a name="core-features"></a>Wichtige Funktionen

Die folgende Tabelle enthält die wichtigen Features für Text-zu-Sprache:

| Anwendungsfall | SDK | REST |
|----------|-----|------|
| Konvertieren von Text in Sprache. | Nein  | Ja |
| Hochladen von Datasets für die Stimmanpassung. | Nein  | Ja\* |
| Erstellen und Verwalten von Voicefont-Modellen. | Nein  | Ja\* |
| Erstellen und Verwalten von Voicefont-Bereitstellungen. | Nein  | Ja\* |
| Erstellen und Verwalten von Voicefont-Tests. | Nein  | Ja\* |
| Verwalten von Abonnements. | Nein  | Ja\* |

\* *Diese Dienste sind über den Endpunkt cris.ai verfügbar. Weitere Informationen finden Sie in der [Swagger-Referenz](https://westus.cris.ai/swagger/ui/index).*

> [!NOTE]
> Der Text-zu-Sprache-Endpunkt implementiert Drosselung, durch die Anforderungen auf 25 Anforderungen pro 5 Sekunden eingeschränkt werden. Wenn Drosselung auftritt, werden Sie über Nachrichtenheader benachrichtigt.

## <a name="get-started-with-text-to-speech"></a>Erste Schritte mit der Sprachsynthese

Wir bieten Schnellstarts an, die so konzipiert sind, dass Sie in weniger als 10 Minuten Code ausführen können. In dieser Tabelle werden Schnellstarts für Text-zu-Sprache nach Sprache aufgelistet.

| Schnellstart | Plattform | API-Referenz |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Window, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Window, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Beispielcode

Beispielcode für Text-zu-Sprache finden Sie auf GitHub. Diese Beispiele umfassen die Text-zu-Sprache-Konvertierung in den gängigsten Programmiersprachen.

* [Beispiele für Text-zu-Sprache (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)
* [Speech-Geräte-SDK](speech-devices-sdk.md)
* [REST-API: Spracherkennung](rest-speech-to-text.md)
* [REST-API: Sprachsynthese](rest-text-to-speech.md)
* [REST-API: Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen eines kostenlosen Speech Services-Abonnements](get-started.md)
* [Erstellen benutzerdefinierter Voicefonts](how-to-customize-voice-font.md)

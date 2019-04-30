---
title: Text-zu-Sprache mit den Azure-Spracherkennungsdiensten
titleSuffix: Azure Cognitive Services
description: Text-zu-Sprache aus den Azure-Spracherkennungsdiensten ist ein Dienst, der es Ihren Anwendungen, Tools oder Geräten ermöglicht, Text in natürliche, menschenähnliche synthetisierte Sprache zu konvertieren. Wählen Sie aus Standard- und neuronalen Stimmen, oder erstellen Sie Ihre eigene, benutzerdefinierte Stimme, die einzigartig für Ihr Produkt oder Ihre Marke ist. Mehr als 75 Standardstimmen sind in mehr als 45 Sprachen und Gebietsschemas verfügbar, und 5 neuronale Stimmen sind in 4 Sprachen und Gebietsschemas verfügbar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 2c5c2bc2b8dd1930efef9833bd442fcad5566e2a
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2019
ms.locfileid: "59578780"
---
# <a name="what-is-text-to-speech"></a>Was ist Text-zu-Sprache?

Text-zu-Sprache aus den Azure-Spracherkennungsdiensten ist ein Dienst, der es Ihren Anwendungen, Tools oder Geräten ermöglicht, Text in natürliche, menschenähnliche synthetisierte Sprache zu konvertieren. Wählen Sie aus Standard- und neuronalen Stimmen, oder erstellen Sie Ihre eigene, benutzerdefinierte Stimme, die einzigartig für Ihr Produkt oder Ihre Marke ist. Mehr als 75 Standardstimmen sind in mehr als 45 Sprachen und Gebietsschemas verfügbar, und 5 neuronale Stimmen sind in 4 Sprachen und Gebietsschemas verfügbar. Eine vollständige Liste finden Sie unter [Unterstützte Sprachen](language-support.md#text-to-speech).

Die Text-zu-Sprache-Technologie ermöglicht Inhaltserstellern, auf unterschiedliche Weise mit ihren Benutzern zu interagieren. Text-zu-Sprache kann die Barrierefreiheit verbessern, indem Benutzern eine Option für die akustische Interaktion mit dem Inhalt zur Verfügung gestellt wird. Unabhängig davon, ob der Benutzer eine Sehbehinderung oder eine Lernbehinderung hat oder während der Fahrt Navigationsinformationen benötigt, kann Text-zu-Sprache ein vorhandenes Erlebnis verbessern. Sprachsynthese ist auch ein wertvolles Add-On für Sprachbots und virtuelle Assistenten.

### <a name="standard-voices"></a>Standardstimmen

Standardstimmen werden mithilfe von Techniken für statistische parametrische Sprachsynthese oder verketteter Sprachsynthese erstellt. Diese Stimmen sind sehr gut verständlich und klingen ganz natürlich. Sie können für Ihre Anwendungen ganz einfach mehr als 45 Sprachen aktivieren und dabei eine Vielzahl von Stimmenoptionen nutzen. Diese Stimmen bieten eine sehr hohe Aussprachequalität, einschließlich der Unterstützung für Abkürzungen, Akronymerweiterungen, Interpretationen von Datum und Uhrzeit, Polyphone und mehr. Verwenden Sie Standardstimmen, um die Barrierefreiheit Ihrer Anwendungen und Dienste zu verbessern, indem Sie Benutzern die akustische Interaktion mit Ihren Inhalten ermöglichen.

### <a name="neural-voices"></a>Neuronale Stimmen

Neuronale Stimmen verwenden Deep Neural Networks, um die Beschränkungen herkömmlicher Sprachsynthesesysteme beim Abgleich der Muster für Betonung und Intonation bei gesprochener Sprache zu überwinden und die Spracheinheiten in eine Computerstimme zu synthetisieren. Die Standardsprachsynthese untergliedert den Satzrhythmus in separate linguistische Analyse- und akustische Vorhersageschritte, die von unabhängigen Modellen gesteuert werden. Dies kann zu einer dumpfen, dröhnenden Sprachsynthese führen. Unsere neuronalen Funktionen führen die Vorhersage der Satzgliederung und die Stimmensynthese gleichzeitig aus, sodass eine flüssigere und natürlicher klingende Sprache entsteht.

Gestalten Sie mit neuronalen Stimmen Interaktionen mit Chatbots und virtuellen Helfern noch natürlicher und einladender, wandeln Sie digitale Texte wie E-Books in Audiobooks um, und verpassen Sie Ihrem Navigationssystem im Auto ein Upgrade. Durch natürliche, menschenähnliche Intonation und klare Aussprache von Wörtern können neuronale Stimmen die Hörermüdung bei der Interaktion mit KI-Systemen erheblich verringern. Weitere Informationen zu unterstützten neuronalen Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).

Weitere Informationen zu den Vorteilen neuronaler Stimmen finden Sie unter [Microsoft’s new neural text-to-speech service helps machines speak like people](https://azure.microsoft.com/blog/microsoft-s-new-neural-text-to-speech-service-helps-machines-speak-like-people/) (Der neue neuronale Text-zu-Sprache-Dienst von Microsoft lässt Computer wie Menschen sprechen).

### <a name="custom-voices"></a>Benutzerdefinierte Stimmen

Durch Stimmanpassungen können Sie eine wiedererkennbare, einzigartige Stimme für Ihre Marke erstellen. Zur Erstellung Ihres benutzerdefinierten Voicefonts laden Sie eine Tonstudioaufnahme und die zugehörigen Skripts als Trainingsdaten hoch. Der Dienst erstellt daraufhin ein individuelles Stimmenmodell, das auf Ihre Aufnahme abgestimmt ist. Diesen benutzerdefinierten Voicefont können Sie zum Synthetisieren von Sprache verwenden. Weitere Informationen finden Sie unter [Benutzerdefinierte Stimmen](how-to-customize-voice-font.md).

## <a name="core-features"></a>Wichtige Funktionen

Die folgende Tabelle enthält die wichtigen Features für Text-zu-Sprache:

| Anwendungsfall | SDK | REST |
|----------|-----|------|
| Konvertieren von Text in Sprache. | Ja | Ja |
| Hochladen von Datasets für die Stimmanpassung. | Nein  | Ja\* |
| Erstellen und Verwalten von Voicefont-Modellen. | Nein  | Ja\* |
| Erstellen und Verwalten von Voicefont-Bereitstellungen. | Nein  | Ja\* |
| Erstellen und Verwalten von Voicefont-Tests. | Nein  | Ja\* |
| Verwalten von Abonnements. | Nein  | Ja\* |

\* *Diese Dienste sind über den Endpunkt cris.ai verfügbar. Weitere Informationen finden Sie in der [Swagger-Referenz](https://westus.cris.ai/swagger/ui/index). Diese benutzerdefinierten Stimmtrainings- und Management-APIs implementieren eine Drosselung, die die Anforderungen auf 25 pro 5 Sekunden begrenzt, während die Sprachsynthese-API selbst eine Drosselung implementiert, die 200 Anforderungen pro Sekunde als Höchstwert zulässt. Wenn Drosselung auftritt, werden Sie über Nachrichtenheader benachrichtigt.*

## <a name="get-started-with-text-to-speech"></a>Erste Schritte mit der Sprachsynthese

Wir bieten Schnellstarts an, die so konzipiert sind, dass Sie in weniger als 10 Minuten Code ausführen können. In dieser Tabelle werden Schnellstarts für Text-zu-Sprache nach Sprache aufgelistet.

### <a name="sdk-quickstarts"></a>SDK-Schnellstarts

| Schnellstart (SDK) | Plattform | API-Referenz |
|------------|----------|---------------|
| [C#, .NET Framework](quickstart-text-to-speech-dotnet-windows.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-text-to-speech-cpp-windows.md) | Windows | [Browse](https://aka.ms/csspeech/cppref) |
| [C++](quickstart-text-to-speech-cpp-linux.md) | Linux | [Browse](https://aka.ms/csspeech/cppref) |

### <a name="rest-quickstarts"></a>REST-Schnellstarts

| Schnellstart (REST) | Plattform | API-Referenz |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Window, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Window, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Beispielcode

Beispielcode für Text-zu-Sprache finden Sie auf GitHub. Diese Beispiele umfassen die Text-zu-Sprache-Konvertierung in den gängigsten Programmiersprachen.

* [Beispiele für Text-zu-Sprache (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
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

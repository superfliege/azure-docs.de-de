---
title: Worum handelt es sich beim Speech-Dienst (Vorschau)?
description: 'Der Speech-Dienst ist Bestandteil von Microsoft Cognitive Services und vereint mehrere Azure-Sprachdienste, die zuvor separat verfügbar waren: Bing-Spracheingabe (bestehend aus Spracherkennung und Sprachsynthese), Custom Speech und Sprachübersetzung.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 0cd6d984ac9329112aa388e8d8ee808d4c3e6227
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445633"
---
# <a name="what-is-the-speech-service-preview"></a>Worum handelt es sich beim Speech-Dienst (Vorschau)?

Der Speech-Dienst basiert auf den in anderen Microsoft-Produkten, z.B. Cortana und Microsoft Office, verwendeten Technologien.  Der gleiche Dienst steht allen Kunden als Cognitive Service zur Verfügung. 

> [!NOTE]
> Der Speech-Dienst ist derzeit als öffentliche Vorschauversion verfügbar. Auf dieser Seite finden Sie regelmäßig Aktualisierungen zur Dokumentation, zusätzliche Codebeispiele und weitere Informationen.

Mit nur einem Abonnement erhalten Entwickler mit dem Speech-Dienst die Möglichkeit, auf einfache Weise leistungsfähige Sprachfunktionen in ihre Anwendungen zu integrieren. Anwendungen können jetzt Sprachbefehle, Transkription, Diktatfunktion, Sprachsynthese und Übersetzung umfassen.

## <a name="speech-service-features"></a>Features des Speech-Diensts

|Funktion|BESCHREIBUNG|
|-|-|
|[Spracherkennung](speech-to-text.md)| Transkribiert Audiodatenströme in Text, der von Ihrer Anwendung als Eingabe akzeptiert werden kann. Zudem ist eine Integration in den [Language Understanding Intelligent Service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) möglich, um die Benutzerabsicht aus Äußerungen abzuleiten.|
|[Sprachsynthese](text-to-speech.md)| Konvertiert einfachen Text in natürlich klingende Sprache, die für Ihre Anwendung als Audiodatei bereitgestellt wird. Es sind mehrere Stimmen, die in Bezug auf das Geschlecht oder den Akzent variieren, für viele unterstützte Sprachen verfügbar. |
|[Sprachübersetzung](speech-translation.md)| Kann verwendet werden, um Audiodatenströme nahezu in Echtzeit zu übersetzen oder um aufgezeichnete Spracheingaben zu verarbeiten. |
|[Speech-Geräte-SDK](speech-devices-sdk.md)| Mit der Einführung des vereinheitlichten Speech-Diensts bieten Microsoft und seine Partner eine integrierte Hardware/Software-Plattform an, die für die Entwicklung von sprachaktivierten Geräten optimiert ist. |

## <a name="using-the-speech-service"></a>Verwenden des Speech-Diensts

Der Speech-Dienst wird auf zwei Arten zur Verfügung gestellt. Das [SDK](speech-sdk.md) kümmert sich um die Details der Netzwerkprotokolle. Die [REST-API](rest-apis.md) kann mit jeder Programmiersprache verwendet werden, bietet jedoch nicht alle mit dem SDK verfügbaren Funktionen.

|<br>Methode|Spracheingabe<br>Spracherkennung|Sprachsynthese<br>Spracheingabe|Spracheingabe<br>Sprachübersetzung|<br>BESCHREIBUNG|
|-|-|-|-|-|
|[SDKs](speech-sdk.md)|JA|Nein|JA|Bibliotheken für bestimmte Programmiersprachen, die die Entwicklung vereinfachen|
|[REST](rest-apis.md)|JA|JA|Nein |Eine einfache HTTP-basierte API, über die sich Sprache zu Ihrer Anwendung hinzufügen lässt.|

## <a name="next-steps"></a>Nächste Schritte

Rufen Sie den Abonnementschlüssel einer kostenlosen Testversion für den Speech-Dienst ab.

> [!div class="nextstepaction"]
> [Kostenloses Testen des Speech-Diensts](get-started.md)

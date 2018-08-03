---
title: Worum handelt es sich beim Speech-Dienst (Vorschau)? | Microsoft-Dokumentation
description: 'Der Speech-Dienst ist Bestandteil von Microsoft Cognitive Services und vereint mehrere Azure-Sprachdienste, die zuvor separat verfügbar waren: Bing-Spracheingabe (bestehend aus Spracherkennung und Sprachsynthese), Custom Speech und Sprachübersetzung.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: adfc854fc24b9e285c405f3038a21ec84cd2f4c2
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989334"
---
# <a name="what-is-the-speech-service-preview"></a>Worum handelt es sich beim Speech-Dienst (Vorschau)?

Der Speech-Dienst ist Bestandteil von Microsoft Cognitive Services und vereint mehrere Azure-Sprachdienste, die zuvor separat verfügbar waren: Bing-Spracheingabe (bestehend aus Spracherkennung und Sprachsynthese), Custom Speech und Sprachübersetzung. Wie seine Vorläufer basiert der Speech-Dienst auf den in anderen Microsoft-Produkten, z.B. Cortana und Microsoft Office, verwendeten Technologien.

> [!NOTE]
> Der Speech-Dienst ist derzeit als öffentliche Vorschauversion verfügbar. Auf dieser Seite finden Sie regelmäßig Aktualisierungen zur Dokumentation, zusätzliche Codebeispiele und weitere Informationen.

Mit nur einem Abonnement erhalten Entwickler mit dem vereinheitlichten Speech-Dienst die Möglichkeit, auf einfache Weise leistungsfähige Sprachfunktionen in ihre Anwendungen zu integrieren. Anwendungen können jetzt Sprachbefehle, Transkription, Diktatfunktion, Sprachsynthese und Übersetzung umfassen.

|Funktion|BESCHREIBUNG|
|-|-|
|Spracherkennung|Wandelt gesprochene Sprache in Text um, der als Eingabe für Ihre Anwendung verwendet werden kann Lässt sich in den [Language Understanding Intelligent Service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) integrieren, um die Benutzerabsicht aus Äußerungen abzuleiten|
|Text-zu-Sprache|Konvertiert Text in Audiodateien mit natürlich klingender synthetisierter Sprache|
|Sprachübersetzung&nbsp;|Stellt Übersetzungen von Spracheingaben in anderen Sprachen mit Text- oder Sprachausgabe bereit|

## <a name="using-the-speech-service"></a>Verwenden des Speech-Diensts

Der Speech-Dienst wird auf zwei Arten zur Verfügung gestellt. Das [SDK](speech-sdk.md) kümmert sich um die Details der Netzwerkprotokolle. Die [REST-API](rest-apis.md) kann mit jeder Programmiersprache verwendet werden, bietet jedoch nicht alle mit dem SDK verfügbaren Funktionen.

|<br>Methode|Spracheingabe<br>Spracherkennung|Sprachsynthese<br>Spracheingabe|Spracheingabe<br>Sprachübersetzung|<br>BESCHREIBUNG|
|-|-|-|-|-|
|[SDKs](speech-sdk.md)|JA|Nein|JA|Bibliotheken für bestimmte Programmiersprachen, die die Entwicklung vereinfachen|
|[REST](rest-apis.md)|JA|JA|Nein |Eine einfache HTTP-basierte API, über die sich Sprache zu Ihrer Anwendung hinzufügen lässt.|

## <a name="speech-to-text"></a>Spracherkennung

Mit der [Spracherkennungs-API](speech-to-text.md) (Speech-to-Text-API, STT) werden Audiodatenströme in Text transkribiert, der in Ihrer Anwendung als Eingabe akzeptiert werden kann. Ihre Anwendung kann dann beispielsweise den Text in ein Dokument eingeben oder basierend darauf einen Befehl ausführen.

Die Spracherkennung wurde separat für interaktive Szenarien, Konversationen und diktierte Texte optimiert. Im Folgenden sind häufige Anwendungsfälle für die Spracherkennungs-API aufgeführt. 

* Erkennen einer kurzen Äußerung, z.B. eines Befehls, ohne Zwischenergebnisse
* Transkribieren einer langen, zuvor aufgezeichneten Äußerung, z.B. einer Mailboxnachricht
* Transkribieren eines Sprachdatenstroms in Echtzeit mit Teilergebnissen für das Diktieren
* Ermitteln der Absicht von Benutzern anhand einer Anforderung in gesprochener natürlicher Sprache

Die Spracherkennungs-API unterstützt die interaktive Sprachtranskription mit fortlaufender Erkennung in Echtzeit und Zwischenergebnissen. Außerdem werden die Erkennung des Endes der Spracheingabe, optionales automatisches Umwandeln in Großschreibung und Zeichensetzung, Filterung von anstößigen Ausdrücken und Textnormalisierung unterstützt.

Sie können akustische und Sprachmodelle für die Spracherkennung anpassen, um spezielles Vokabular, laute Umgebungen und andere Sprechweisen abzudecken.

## <a name="text-to-speech"></a>Text-zu-Sprache

Mit der [Sprachsynthese-API](text-to-speech.md) (Text-to-Speech-API, TTS) wird der Text direkt in natürlich klingende Sprache konvertiert, die für Ihre Anwendung als Audiodatei bereitgestellt wird. Es sind mehrere Stimmen, die in Bezug auf das Geschlecht oder den Akzent variieren, für viele unterstützte Sprachen verfügbar.

Die API unterstützt [SSML-Tags (Speech Synthesis Markup Language)](speech-synthesis-markup.md), damit Sie für schwierige Wörter die genaue phonetische Aussprache angeben können. Mit SSML können auch besondere Sprachmerkmale (z.B. Betonung, Geschwindigkeit, Lautstärke, Geschlecht und Tonlage) direkt im Text angegeben werden.

Im Folgenden sind häufige Anwendungsfälle für die Text-to-Speech-API aufgeführt.

* Sprachausgabe als Alternative zur Bildschirmausgabe für Benutzer mit Sehbehinderung
* Sprachausgabe für Kfz-Anwendungen, z.B. Navigation
* Auf Unterhaltungen ausgelegte Benutzeroberflächen in Verbindung mit der Spracherkennungs-API

Für den Fall, dass Sie für Ihre Anwendung einen nicht unterstützten Dialekt oder eine einzigartige Stimme verwenden möchten, werden in der Text-to-Speech-API [benutzerdefinierte Stimmmodelle](how-to-customize-voice-font.md) unterstützt.

## <a name="speech-translation"></a>Sprachübersetzung

Mit der [Sprachübersetzungs-API](speech-translation.md) können entweder Audiodatenströme nahezu in Echtzeit übersetzt oder aufgezeichnete Spracheingaben verarbeitet werden. Bei der Übersetzung per Streaming gibt der Dienst Zwischenergebnisse zurück. Diese können für Benutzer angezeigt werden, um den Status der Übersetzung anzugeben. Die Ergebnisse können entweder als Text oder als Sprache zurückgegeben werden.

Es gibt beispielsweise folgende Anwendungsfälle für die Sprachübersetzung.

* Implementieren einer mobilen App bzw. eines Geräts für Reisende zum Übersetzen von Unterhaltungen 
* Bereitstellen von automatischen Übersetzungen zur Untertitelung von Audio- und Videoaufzeichnungen

## <a name="speech-devices-sdk"></a>Speech-Geräte-SDK

Mit der Einführung des vereinheitlichten Speech-Diensts bieten Microsoft und seine Partner eine integrierte Hardware/Software-Plattform an, die für die Entwicklung von sprachaktivierten Geräten optimiert ist: das [Speech-Geräte-SDK](speech-devices-sdk.md). Dieses SDK ist für die Entwicklung von intelligenten Sprachgeräten für alle Arten von Anwendungen geeignet.

Mit dem Speech-Geräte-SDK können Sie Ihre eigenen Ambient Devices mit einem benutzerdefinierten Codewort erstellen, das zu Ihrer Marke passt und mit dem die Erfassung von Audiodaten ausgelöst wird. Außerdem ermöglicht es eine bessere Audioverarbeitung für Quellen mit mehreren Kanälen, um eine genauere Spracherkennung zu erzielen, z.B. Geräuschunterdrückung, Fernfeldsprache und Beamforming.

Das SDK basiert auf Websockets, die Port 443 verwenden.

## <a name="next-steps"></a>Nächste Schritte

Rufen Sie den Abonnementschlüssel einer kostenlosen Testversion für den Speech-Dienst ab.

> [!div class="nextstepaction"]
> [Kostenloses Testen des Speech-Diensts](get-started.md)

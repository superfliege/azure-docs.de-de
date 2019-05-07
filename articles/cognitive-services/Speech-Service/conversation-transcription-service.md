---
title: Dienst für Unterhaltungstranskription – Speech-Dienste
titleSuffix: Azure Cognitive Services
description: Der Dienst für Unterhaltungstranskription ist ein erweitertes Feature der Speech-Dienste, bei dem Spracherkennung in Echtzeit, Sprecheridentifikation und Diarisierung miteinander kombiniert werden. Dieser Dienst eignet sich optimal für das Transkribieren persönlicher Besprechungen, wobei zwischen Sprechern unterschieden werden kann. Sie können sehen, wann jemand was gesagt hat, und die Teilnehmer können sich auf die Besprechung konzentrieren und die nächsten Schritte schnell nachverfolgen. Dieses Feature verbessert auch die Barrierefreiheit. Durch eine Transkription können Sie Teilnehmer mit vermindertem Hörvermögen aktiv einbeziehen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 2c3269a9e57c0392e152fedfb640b9fae91a4989
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025698"
---
# <a name="what-is-the-conversation-transcription-service"></a>Was ist der Dienst für Unterhaltungstranskription?

Der Dienst für Unterhaltungstranskription ist ein erweitertes Feature der Speech-Dienste, bei dem Spracherkennung in Echtzeit, Sprecheridentifikation und Diarisierung miteinander kombiniert werden. Dieser Dienst eignet sich optimal für das Transkribieren persönlicher Besprechungen, wobei zwischen Sprechern unterschieden werden kann. Sie können sehen, wann jemand was gesagt hat, und die Teilnehmer können sich auf die Besprechung konzentrieren und die nächsten Schritte schnell nachverfolgen. Dieses Feature verbessert auch die Barrierefreiheit. Durch eine Transkription können Sie Teilnehmer mit vermindertem Hörvermögen aktiv einbeziehen.   

Der Dienst für Unterhaltungstranskription bietet eine genau Erkennung mit anpassbaren Sprachmodellen, die Sie an das branchen- und unternehmensspezifische Vokabular anpassen können. Darüber hinaus können Sie den Dienst für Unterhaltungstranskription mit dem Speech-Geräte-SDK kombinieren, um die Funktionsweise für Geräte mit mehreren Mikrofonen zu optimieren.

>[!NOTE]
> Derzeit wird der Dienst für Unterhaltungstranskription für kleine Besprechungen empfohlen. Wenn Sie diesen Dienst für Besprechungen in großem Umfang erweitern möchten, setzen Sie sich mit uns in Verbindung.

Das folgende Diagramm zeigt die Hardware, Software und Dienste, die mit dem Dienst für Unterhaltungstranskription zusammenarbeiten.

![Diagramm zur Einbindung des Diensts für Unterhaltungstranskription](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> Es ist ein kreisförmiges Mikrofonarray mit sieben Mikrofonen und einer spezifischen Geometriekonfiguration erforderlich. Weitere Informationen zu Spezifikationen und Design finden Sie unter den [Mikrofonempfehlungen für das Speech-Geräte-SDK von Microsoft](https://aka.ms/cts/microphone). Weitere Informationen zum Development Kit und dessen Erwerb finden Sie unter [Abrufen des Speech-Geräte-SDK von Microsoft](https://aka.ms/cts/getsdk).

## <a name="get-started-with-conversation-transcription"></a>Erste Schritte mit der Unterhaltungstranskription

Es gibt drei Schritte, die Sie zum Einstieg in den Dienst für Unterhaltungstranskription ausführen müssen.

1. Sammeln von Sprachbeispielen der Benutzer
2. Generieren von Benutzerprofilen anhand der Sprachbeispiele
3. Verwenden des Speech SDK zum Identifizieren von Benutzern (Sprechern) und Transkribieren von gesprochener Sprache

## <a name="collect-user-voice-samples"></a>Sammeln von Sprachbeispielen der Benutzer

Zuerst müssen Sie Audioaufnahmen der einzelnen Benutzer sammeln. Die Sprache der Benutzer sollte in einer ruhigen Umgebung ohne Hintergrundgeräusche aufgezeichnet werden. Die empfohlene Länge der einzelnen Audiobeispiele liegt zwischen 30 Sekunden und zwei Minuten. Längere Audiobeispiele führen zu einer höheren Genauigkeit beim Identifizieren von Sprechern. Die Audioaufnahme muss Monokanal mit einer Abtastrate von 16 KHz sein.

Über die oben genannten Anleitungen hinaus liegt es an Ihnen, wie Audio aufgezeichnet und gespeichert wird – eine sichere Datenbank wird empfohlen. Im nächsten Abschnitt wird beschrieben, wie anhand dieser Audioaufnahmen Benutzerprofile generiert werden, die dann mit dem Speech SDK zum Erkennen von Sprechern verwendet werden.

## <a name="generate-user-profiles"></a>Generieren von Benutzerprofilen

Als Nächstes müssen Sie die gesammelten Audioaufnahmen an den Dienst für die Signaturgenerierung senden, um die Audioaufnahmen zu überprüfen und Benutzerprofile zu generieren. Beim [Dienst für die Signaturgenerierung](https://aka.ms/cts/signaturegenservice) handelt es sich um eine Reihe von REST-APIs, mit denen Sie Benutzerprofile generieren und abrufen können.

Zum Erstellen eines Benutzerprofils müssen Sie die `GenerateVoiceSignature`-API verwenden. Weitere Informationen zu Spezifikationen und Beispielcode stehen zur Verfügung:

> [!NOTE]
> Der Dienst für Unterhaltungstranskription ist zurzeit in den folgenden Regionen verfügbar: `centralus` und `eastasia`.

* [REST-Spezifikation](https://aka.ms/cts/signaturegenservice)
* [Verwenden des Diensts für Unterhaltungstranskription](https://aka.ms/cts/howto)

## <a name="transcribe-and-identify-speakers"></a>Transkribieren und Identifizieren von Sprechern

Der Dienst für Unterhaltungstranskription setzt Mehrkanal-Audiostreams und Benutzerprofile als Eingaben voraus, um Transkriptionen zu erstellen und Sprecher zu identifizieren. Audio- und Benutzerprofildaten werden mit dem Speech-Geräte-SDK an den Dienst für Unterhaltungstranskription gesendet. Wie bereits erwähnt sind ein kreisförmiges Mikrofonarray mit sieben Mikrofonen und das Speech-Geräte-SDK erforderlich, um den Dienst für Unterhaltungstranskription zu verwenden.

>[!NOTE]
> Weitere Informationen zu Spezifikationen und Design finden Sie unter den [Mikrofonempfehlungen für das Speech-Geräte-SDK von Microsoft](https://aka.ms/cts/microphone). Weitere Informationen zum Development Kit und dessen Erwerb finden Sie unter [Abrufen des Speech-Geräte-SDK von Microsoft](https://aka.ms/cts/getsdk).

Informationen zum Verwenden des Diensts für Unterhaltungstranskription mit dem Speech-Geräte-SDK finden Sie unter [Verwenden des Diensts für Unterhaltungstranskription](https://aka.ms/cts/howto).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Speech-Geräte-SDK](speech-devices-sdk.md)

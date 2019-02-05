---
title: Informationen zur Sprachsynthese – Speech Services
titleSuffix: Azure Cognitive Services
description: Die Text-to-Speech-API bietet mehr als 75 Stimmen in mehr als 45 Sprachen und Gebietsschemas. Wenn Sie die standardmäßigen „Voicefonts“ verwenden möchten, müssen Sie beim Aufrufen des Speech-Diensts nur den Namen der Stimme mit einigen anderen Parametern angeben.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: b413bd65582dceadd9aab912694c3b560070c4f3
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251553"
---
# <a name="about-the-text-to-speech-api"></a>Informationen zur Text-to-Speech-API

Die **Text-to-Speech**-API konvertiert Eingabetext in natürlich klingende Sprache (dies wird auch als *Sprachsynthese* bezeichnet).

Zur Generierung von Sprache sendet Ihre Anwendung HTTP POST-Anforderungen an die Text-to-Speech-API. Dort wird Text in menschliche Sprache synthetisiert und als Audiodatei zurückgegeben. Es werden viele Stimmen und Sprachen unterstützt.

Zu Szenarios, in denen die Sprachsynthese übernommen wird, zählen die folgenden:

* *Verbessern der Bedienungshilfen:* Mit der **Text-to-Speech**-Technologie können Inhaltsbesitzer und Verleger auf die unterschiedlichen Interaktionsarten von Personen mit ihren Inhalten reagieren. Personen mit einer Sehbehinderung oder Leseschwierigkeiten sind dankbar, wenn Ihnen Inhalte akustisch dargestellt werden. Darüber hinaus können Personen durch Sprachausgaben Textinhalte, wie z.B. Zeitungen oder Blogs, auf mobilen Geräten leichter genießen, während sie zur Arbeit pendeln oder trainieren.

* *Reagieren in Multitasking-Szenarien:* Durch die **Text-to-Speech**-Technologie können Personen wichtige Informationen schnell und komfortabel aufnehmen, während sie im Auto sitzen oder sich in einer anderen leseunfreundlichen Umgebung befinden. Die Navigation wird in diesem Bereich häufig angewendet.

* *Erweitertes Lernen durch mehrere Modi:* Es gibt verschiedene Methoden, mit denen unterschiedliche Personen jeweils am besten lernen. Experten für das Online-Learning haben gezeigt, dass Informationen durch die gemeinsame Bereitstellung von Sprache und Text besser gelernt und behalten werden können.

* *Bereitstellung intuitiver Bots oder Assistenten:* Die Möglichkeit zum Sprechen kann ein wesentlicher Bestandteil eines intelligenten Chatbots oder eines virtuellen Assistenten sein. Immer mehr Unternehmen entwickeln Chatbots, um Kunden ansprechende Erfahrungen mit dem Kundendienst bieten zu können. Durch die Sprache wird eine weitere Dimension hinzugefügt, indem die Antworten des Bots akustisch empfangen werden können (beispielsweise per Telefon).

## <a name="voice-support"></a>Sprachunterstützung

Der **Text-to-Speech**-Dienst von Microsoft bietet mehr als 75 Stimmen in mehr als 45 Sprachen und Gebietsschemas. Wenn Sie diese standardmäßigen „Voicefonts“ verwenden möchten, müssen Sie beim Aufrufen der REST-API nur den Namen der Stimme mit einigen anderen Parametern angeben. Weitere Informationen zu unterstützten Sprachen, Gebietsschemas und Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).

> [!IMPORTANT]
> Die Kosten sind für Standardstimmen, benutzerdefinierte und neuronale Stimmen unterschiedlich. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices"></a>Neuronale Stimmen

Gestalten Sie mit neuronaler Sprachsynthese Interaktionen mit Chatbots und virtuellen Helfern noch natürlicher und einladender, wandeln Sie digitale Texte wie E-Books in Audiobooks um, und verpassen Sie Ihrem Navigationssystem im Auto ein Upgrade. Durch natürliche, menschenähnliche Intonation und klare Aussprache von Wörtern kann die neuronale Sprachsynthese die Hörermüdung bei der Interaktion mit KI-Systemen erheblich verringern. Weitere Informationen zu unterstützten neuronalen Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Benutzerdefinierte Stimmen

Mit der Stimmanpassung für die Sprachsynthese erstellen Sie für Ihre Marke eine unverwechselbare Stimme, die als *Voicefont* bezeichnet wird. Zur Erstellung Ihres Voicefonts laden Sie eine Tonstudioaufnahme und die zugehörigen Skripts als Trainingsdaten hoch. Der Dienst erstellt daraufhin ein individuelles Stimmenmodell, das auf Ihre Aufnahme abgestimmt ist. Diesen Voicefont können Sie zum Synthetisieren von Sprache verwenden. Weitere Informationen finden Sie unter [Benutzerdefinierte Voicefonts](how-to-customize-voice-font.md).

## <a name="api-capabilities"></a>API-Funktionen

Viele der Funktionen der **Text-to-Speech**-API, insbesondere im Bereich der Anpassung, sind über REST verfügbar. In der folgenden Tabelle werden die Funktionen der einzelnen Methoden für den Zugriff auf die API beschrieben. Eine vollständige Liste der Funktionen und API-Details finden Sie in der [Swagger-Referenz](https://westus.cris.ai/swagger/ui/index).

| Anwendungsfall | REST | SDKs |
|-----|-----|-----|----|
| Hochladen von Datasets für die Stimmanpassung | JA | Nein  |
| Erstellen und Verwalten von Voicefont-Modellen | JA | Nein  |
| Erstellen und Verwalten von Voicefont-Bereitstellungen | JA | Nein  |
| Erstellen und Verwalten von Voicefont-Tests| JA | Nein  |
| Verwalten von Abonnements | JA | Nein  |

> [!NOTE]
> Der API implementiert Drosselung, durch die API-Anforderungen auf 25 Anforderungen pro 5 Sekunden eingeschränkt werden. Nachrichtenheader informieren über die Grenzwerte.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen eines kostenlosen Speech Services-Abonnements](https://azure.microsoft.com/try/cognitive-services/)
* [Schnellstart: Konvertieren von Text in Sprache, Python](quickstart-python-text-to-speech.md)
* [Schnellstart: Konvertieren von Text in Sprache, .NET Core](quickstart-dotnet-text-to-speech.md)
* [REST-API-Referenz](rest-apis.md)

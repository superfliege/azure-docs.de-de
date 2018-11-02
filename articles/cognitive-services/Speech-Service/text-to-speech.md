---
title: Informationen zu Text-to-Speech – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Die Text-to-Speech-API bietet mehr als 75 Stimmen in mehr als 45 Sprachen und Gebietsschemas. Wenn Sie die standardmäßigen „Voicefonts“ verwenden möchten, müssen Sie beim Aufrufen des Speech-Diensts nur den Namen der Stimme mit einigen anderen Parametern angeben.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: erhopf
ms.openlocfilehash: 7f01fe5c71cdd6f4c70527fcf2553374aae9a5d8
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469924"
---
# <a name="about-the-text-to-speech-api"></a>Informationen zur Text-to-Speech-API

Die **Text-to-Speech**-API des Speech Service konvertiert Eingabetext in natürlich klingende Sprache (dies wird auch als *Sprachsynthese* bezeichnet).

Zur Generierung einer Spracheingabe sendet Ihre Anwendung HTTP POST-Anforderungen an den Speech Service. Dort wird Text in menschliche Sprache synthetisiert und als Audiodatei zurückgegeben. Es werden viele Stimmen und Sprachen unterstützt.

Zu Szenarios, in denen die Sprachsynthese übernommen wird, zählen die folgenden:

* *Verbessern der Bedienungshilfen:* Mit der **Text-to-Speech**-Technologie können Inhaltsbesitzer und Verleger auf die unterschiedlichen Interaktionsarten von Personen mit ihren Inhalten reagieren. Personen mit einer Sehbehinderung oder Leseschwierigkeiten sind dankbar, wenn Ihnen Inhalte akustisch dargestellt werden. Darüber hinaus können Personen durch Sprachausgaben Textinhalte, wie z.B. Zeitungen oder Blogs, auf mobilen Geräten leichter genießen, während sie zur Arbeit pendeln oder trainieren.

* *Reagieren in Multitasking-Szenarios:* Durch die **Text-to-Speech**-Technologie können Personen wichtige Informationen schnell und komfortabel aufnehmen, während sie im Auto sitzen oder sich in einer anderen, leseunfreundlichen Umgebung befinden. Die Navigation wird in diesem Bereich häufig angewendet.

* *Erweitern von Lernen durch mehrere Modi:* Es gibt verschiedene Methoden, mit denen unterschiedliche Personen jeweils am besten lernen. Experten für das Online-Learning haben gezeigt, dass Informationen durch die gemeinsame Bereitstellung von Sprache und Text besser gelernt und behalten werden können.

* *Bereitstellung intuitiver Bots oder Assistenten:* Die Möglichkeit zum Sprechen kann wesentlicher Bestandteil eines intelligenten Chatbots oder eines virtuellen Assistenten sein. Immer mehr Unternehmen entwickeln Chatbots, um Kunden ansprechende Erfahrungen mit dem Kundendienst bieten zu können. Durch die Sprache wird eine weitere Dimension hinzugefügt, indem die Antworten des Bots akustisch empfangen werden können (beispielsweise per Telefon).

## <a name="voice-support"></a>Sprachunterstützung

Der **Text-to-Speech**-Dienst von Microsoft bietet mehr als 75 Stimmen in mehr als 45 Sprachen und Gebietsschemas. Wenn Sie diese standardmäßigen „Voicefonts“ verwenden möchten, müssen Sie beim Aufrufen der REST-API nur den Namen der Stimme mit einigen anderen Parametern angeben. Einzelheiten zu den unterstützten Stimmen finden Sie unter [Unterstützte Sprachen](language-support.md#text-to-speech).

Wenn Sie für Ihre Anwendung eine einzigartige Stimme verwenden möchten, können Sie aus Ihren eigenen Sprachbeispielen [benutzerdefinierte Voicefonts](how-to-customize-voice-font.md) erstellen.

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

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erfahren Sie, wie Sprache über die REST-API synthetisiert wird](how-to-text-to-speech.md)

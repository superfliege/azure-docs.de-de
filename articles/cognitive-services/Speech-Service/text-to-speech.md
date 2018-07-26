---
title: Text-to-Speech | Microsoft-Dokumentation
description: Übersicht über die Funktionen der Text-to-Speech-API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: e3543c2056934372a2795bd50f340432ceec3b6d
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257945"
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

Der **Text-to-Speech**-Dienst von Microsoft bietet mehr als 75 Stimmen in mehr als 45 Sprachen und Gebietsschemas. Wenn Sie diese standardmäßigen „Voicefonts“ verwenden möchten, müssen Sie beim Aufrufen der REST-API nur den Namen der Stimme mit einigen anderen Parametern angeben. Einzelheiten zu den unterstützten Stimmen finden Sie unter [Unterstützte Sprachen](https://docs.microsoft.com/azure/cognitive-services/speech-service/supported-languages#text-to-speech). 

Wenn Sie für Ihre Anwendung eine einzigartige Stimme verwenden möchten, können Sie aus Ihren eigenen Sprachbeispielen [benutzerdefinierte Voicefonts](how-to-customize-voice-font.md) erstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)

---
title: Informationen zur Sprachübersetzung
titlesuffix: Azure Cognitive Services
description: Übersicht über die Funktionen der Sprachübersetzung
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 04/28/2018
ms.author: erhopf
ms.openlocfilehash: af363a45ab113d89c7aad7a38c6e10d335bc6035
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469142"
---
# <a name="about-the-speech-translation-api"></a>Informationen zur Sprachübersetzungs-API

Mit der Speech-Dienst-API können Sie Ihren Anwendungen, Tools und Geräten End-to-End- und Echtzeit-Sprachübersetzungen sowie mehrsprachige Übersetzungen hinzufügen. Die gleiche API kann für Speech-to-Speech- und für Speech-to-Text-Übersetzungen verwendet werden.

Mit der Sprachübersetzungs-API streamen Clientanwendungen Spracheingaben an den Dienst und empfangen einen Stream mit Ergebnissen. Diese Ergebnisse umfassen den erkannten Text in der Quellsprache und die zugehörige Übersetzung in der Zielsprache. Vorläufige Übersetzungen können so lange bereitgestellt werden, bis eine Äußerung vollständig ist. Zu diesem Zeitpunkt wird dann eine finale Übersetzung zur Verfügung gestellt.

Optional kann eine synthetisierte Audioversion der finalen Übersetzung vorbereitet werden, wodurch eine korrekte Speech-to-Speech-Übersetzung ermöglicht wird.

Die Sprachübersetzungs-API verwendet für die Bereitstellung eines Vollduplex-Kommunikationskanals zwischen dem Client und dem Server ein WebSocket-Protokoll. Sie müssen sich jedoch nicht um WebSockets kümmern; das übernimmt das Sprach-SDK für Sie.

Die Sprachübersetzungs-API verwendet die gleichen Technologien, die auch für verschiedene Microsoft-Produkte und -Dienste zum Einsatz kommen. Dieser Dienst wird bereits weltweit von tausenden von Unternehmen in ihren Anwendungen und Workflows verwendet.

## <a name="about-the-technology"></a>Informationen zur Technologie

Der Übersetzungsengine von Microsoft liegen zwei unterschiedliche Ansätze zugrunde: statistische maschinelle Übersetzung (Statistical Machine Translation, SMT) und neuronale maschinelle Übersetzung (Neural Machine Translation, NMT). Bei dem zweiten Ansatz– ein KI-Ansatz, bei dem neuronale Netzwerke verwendet werden – handelt es sich um den moderneren Ansatz im Bereich der maschinellen Übersetzungen. NMT bietet bessere Übersetzungen: Sie sind nicht nur genauer, sondern klingen auch flüssiger und natürlicher. Der Hauptgrund für diesen Textfluss besteht darin, dass NMT bei der Übersetzung von Wörtern den gesamten Kontext eines Satzes berücksichtigt.

Heute ist Microsoft für die am häufigsten verwendeten Sprachen zu NMT migriert und verwendet SMT nur für weniger häufig verwendete Sprachen. Alle [für die Speech-to-Speech-Übersetzung verfügbaren Sprachen](language-support.md#speech-translation) werden von NMT unterstützt. Bei der Speech-to-Speech-Übersetzung kann je nach Sprachkombination SMT oder NMT verwendet werden. Wenn die Zielsprache von NMT unterstützt wird, wird die gesamte Übersetzung von NMT unterstützt. Wenn die Zielsprache nicht von NMT unterstützt wird, ist die Übersetzung ein Hybrid von NMT und SMT und verwendet Englisch als „Pivot“ zwischen den beiden Sprachen.

Die Unterschiede zwischen Modellen sind für das Übersetzungsmodul intern wahrnehmbar. Den Benutzern fällt nur die verbesserte Übersetzungsqualität auf, insbesondere bei Chinesisch, Japanisch und Arabisch.

> [!NOTE]
> Möchten Sie mehr über die Technologie der Übersetzungsengine von Microsoft erfahren? Weitere Informationen finden Sie unter [Maschinelle Übersetzung](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erfahren Sie, wie Sprache in C# übersetzt wird](how-to-translate-speech-csharp.md)
* [Erfahren Sie, wie Sprache in C++ übersetzt wird](how-to-translate-speech-cpp.md)
* [Erfahren Sie, wie Sprache in Java übersetzt wird](how-to-translate-speech-java.md)

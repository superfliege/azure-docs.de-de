---
title: Informationen zur Spracherkennung | Microsoft-Dokumentation
description: Übersicht zu den Funktionen der Spracherkennungs-API
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 2eee1c6f9158f128ed5ffe575f8f498f1d3eb5e9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35378453"
---
# <a name="about-the-speech-to-text-api"></a>Informationen zur Spracherkennungs-API

Die **Spracherkennungs**-API *überträgt* Audiodatenströme in Text, den Ihre Anwendung dem Benutzer anzeigen oder als eine Befehlseingabe verarbeiten kann. Die APIs können entweder mit einer SDK-Clientbibliothek (für unterstützte Plattformen und Sprachen) oder mit einer REST-API verwendet werden.

Die **Spracherkennungs**-API umfasst die folgenden Features:

- Erweiterte Spracherkennungstechnologie von Microsoft (wird auch von Cortana, Office und anderen Microsoft-Produkten verwendet).

- Kontinuierliche Erkennung in Echtzeit. Mithilfe der **Spracherkennung** können Benutzer in Echtzeit Audiodatenströme in Text übertragen. Sie unterstützt auch den Empfang von Zwischenergebnissen der Wörter, die bis zum jeweiligen Zeitpunkt erkannt worden sind. Der Dienst erkennt automatisch, wenn nichts mehr gesagt wird. Benutzer können außerdem zusätzliche Formatierungsoptionen auswählen, einschließlich Großbuchstaben und Interpunktion, Filterung von anstößigen Ausdrücken und Textnormalisierung.

- Optimierte Ergebnisse für die **Spracherkennung** für interaktive Szenarios, Konversation und diktierte Texte 

- Unterstützung vieler Sprachen in verschiedenen Dialekten. Eine Liste der unterstützten Sprachen für die einzelnen Erkennungsmodi finden Sie unter [Supported languages (Unterstützte Sprachen)](supported-languages.md#speech-to-text).

- Angepasste Modelle für Sprache und Akustik, damit Sie Ihre Anwendung an die Sprechweise des Benutzers, die Sprechumgebung und an besonderes Vokabular anpassen können.

- Verständnis natürlicher Sprache. Sie können über die Integration von [Language Understanding Intelligent Service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) Absichten und Entitäten aus dem Gesagten ableiten. Die Benutzer müssen die Terminologie Ihrer App nicht kennen und können ihre Wünsche in ihren eigenen Worten erklären.

## <a name="api-capabilities"></a>API-Funktionen

Einige Funktionen der **Spracherkennungs**-API sind nicht über REST verfügbar. In der folgenden Tabelle werden die Funktionen der einzelnen Methoden für den Zugriff auf die API beschrieben.

| Anwendungsfall | REST | SDKs |
|-----|-----|-----|----|
| Übertragen einer kurzen Äußerung wie einem Befehl (weniger als 15 Sekunden); keine Zwischenergebnisse | Ja | Ja |
| Übertragen einer längeren Äußerung (länger als 15 Sekunden) | Nein  | Ja |
| Übertragen von Audiodatenströmen mithilfe von optionalen Zwischenergebnissen | Nein  | Ja |
| Nachvollziehen von Sprecherabsichten mithilfe von LUIS | Nein\* | Ja |

\* *Sie können die LUIS-Absichten und Entitäten mithilfe eines separaten LUIS-Abonnements ableiten. Über dieses Abonnement kann das SDK LUIS aufrufen und bietet sowohl Ergebnisse für Absichten und Entitäten als auch Sprachtranskriptionen. Mithilfe der REST-API können Sie LUIS selbst aufrufen, um Absichten und Entitäten mithilfe Ihres LUIS-Abonnements abzuleiten.*

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für die Spracherkennung](https://azure.microsoft.com/try/cognitive-services/)
* [See how to recognize speech in C# (Erkennen von Sprache in C#)](quickstart-csharp-windows.md)

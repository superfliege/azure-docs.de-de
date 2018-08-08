---
title: Informationen zur Spracherkennung
description: Übersicht zu den Funktionen der Spracherkennungs-API
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: ba6710c8b5b8de1c63fa6778ea3853ab52365254
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325335"
---
# <a name="about-the-speech-to-text-api"></a>Informationen zur Spracherkennungs-API

Die **Spracherkennungs**-API *überträgt* Audiodatenströme in Text, den Ihre Anwendung dem Benutzer anzeigen oder als eine Befehlseingabe verarbeiten kann. Die APIs können entweder mit einer SDK-Clientbibliothek (für unterstützte Plattformen und Sprachen) oder mit einer REST-API verwendet werden.

Die **Spracherkennungs**-API umfasst die folgenden Features:

- Erweiterte Spracherkennungstechnologie von Microsoft (wird auch von Cortana, Office und anderen Microsoft-Produkten verwendet).

- Kontinuierliche Erkennung in Echtzeit. Mithilfe der **Spracherkennung** können Benutzer in Echtzeit Audiodatenströme in Text übertragen. Sie unterstützt auch den Empfang von Zwischenergebnissen der Wörter, die bis zum jeweiligen Zeitpunkt erkannt worden sind. Der Dienst erkennt automatisch, wenn nichts mehr gesagt wird. Benutzer können außerdem zusätzliche Formatierungsoptionen auswählen, einschließlich Großbuchstaben und Interpunktion, Filterung von anstößigen Ausdrücken und inverse Textnormalisierung.

- Optimierte Ergebnisse für die **Spracherkennung** für interaktive Szenarios, Konversation und diktierte Texte 

- Unterstützung vieler Sprachen und Dialekte. Eine Liste der unterstützten Sprachen für die einzelnen Erkennungsmodi finden Sie unter [Supported languages (Unterstützte Sprachen)](supported-languages.md#speech-to-text).

- Angepasste Modelle für Sprache und Akustik, damit Sie Ihre Anwendung an das spezielle Vokabular der Benutzer, die Sprechumgebung und an die Sprechweise anpassen können.

- Verständnis natürlicher Sprache. Sie können über die Integration von [Language Understanding Intelligent Service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) Absichten und Entitäten aus dem Gesagten ableiten. Die Benutzer müssen die Terminologie Ihrer App nicht kennen und können ihre Wünsche in ihren eigenen Worten erklären.

## <a name="api-capabilities"></a>API-Funktionen

Einige Funktionen der **Spracherkennungs**-API sind nicht über REST verfügbar. In der folgenden Tabelle werden die Funktionen der einzelnen Methoden für den Zugriff auf die API beschrieben.

| Anwendungsfall | REST | SDKs |
|-----|-----|-----|----|
| Übertragen einer kurzen Äußerung wie einem Befehl (weniger als 15 Sekunden); keine Zwischenergebnisse | JA | JA |
| Übertragen einer längeren Äußerung (länger als 15 Sekunden) | Nein  | JA |
| Übertragen von Audiodatenströmen mithilfe von optionalen Zwischenergebnissen | Nein  | JA |
| Nachvollziehen von Sprecherabsichten mithilfe von LUIS | Nein\* | JA |

\* *Sie können die LUIS-Absichten und Entitäten mithilfe eines separaten LUIS-Abonnements ableiten. Über dieses Abonnement kann das SDK LUIS aufrufen und bietet sowohl Ergebnisse für Absichten und Entitäten als auch Sprachtranskriptionen. Mithilfe der REST-API können Sie LUIS selbst aufrufen, um Absichten und Entitäten mithilfe Ihres LUIS-Abonnements abzuleiten.*

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Schnellstart: Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
* [Erfahren Sie, wie die Absichtserkennung anhand von Sprache in C# funktioniert](how-to-recognize-intents-from-speech-csharp.md)

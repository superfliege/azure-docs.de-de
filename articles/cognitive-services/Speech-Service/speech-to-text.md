---
title: Informationen zur Spracherkennung
titleSuffix: Azure Cognitive Services
description: Übersicht zu den Funktionen der Spracherkennungs-API
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: 041ec1c095ec604fed7906368ff266c1586df570
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471338"
---
# <a name="about-the-speech-to-text-api"></a>Informationen zur Spracherkennungs-API

Die **Spracherkennungs**-API *überträgt* Audiodatenströme in Text, den Ihre Anwendung dem Benutzer anzeigen oder als eine Befehlseingabe verarbeiten kann. Die APIs können entweder mit einer SDK-Clientbibliothek (für unterstützte Plattformen und Sprachen) oder mit einer REST-API verwendet werden.

Die **Spracherkennungs**-API umfasst die folgenden Features:

- Erweiterte Spracherkennungstechnologie von Microsoft (wird auch von Cortana, Office und anderen Microsoft-Produkten verwendet).

- Kontinuierliche Erkennung in Echtzeit. Mithilfe der **Spracherkennung** können Benutzer in Echtzeit Audiodatenströme in Text übertragen. Sie unterstützt auch den Empfang von Zwischenergebnissen der Wörter, die bis zum jeweiligen Zeitpunkt erkannt worden sind. Der Dienst erkennt automatisch, wenn nichts mehr gesagt wird. Benutzer können außerdem zusätzliche Formatierungsoptionen auswählen, einschließlich Großbuchstaben und Interpunktion, Filterung von anstößigen Ausdrücken und inverse Textnormalisierung.

- Die Ergebnisse werden sowohl in lexikalischer als auch in Anzeigeform zurückgegeben (Details zu den lexikalischen Ergebnissen finden Sie unter „DetailedSpeechRecognitionResult“ in den Beispielen oder in der API).

- Unterstützung vieler Sprachen und Dialekte. Eine Liste der unterstützten Sprachen für die einzelnen Erkennungsmodi finden Sie unter [Supported languages (Unterstützte Sprachen)](language-support.md#speech-to-text).

- Angepasste Modelle für Sprache und Akustik, damit Sie Ihre Anwendung an das spezielle Vokabular der Benutzer, die Sprechumgebung und an die Sprechweise anpassen können.

- Verständnis natürlicher Sprache. Sie können über die Integration von [Language Understanding Intelligent Service](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) Absichten und Entitäten aus dem Gesagten ableiten. Die Benutzer müssen die Terminologie Ihrer App nicht kennen und können ihre Wünsche in ihren eigenen Worten erklären.

- Die Zuverlässigkeitsbewertung wird vom Dienst zurückgegeben, wenn Sie im Konfigurationsobjekt für die Spracherkennung (SpeechConfig.OutputFormat-Eigenschaft) eine ausführliche Ausgabe angegeben haben. Sie können dann die Best()-Methode auf das Ergebnis anwenden oder das Ergebnis direkt aus dem vom Dienst zurückgegebenen JSON-Code abrufen (etwa result.Properties.GetProperty(PropertyId.SpeechServiceResponse_JsonResult)).

## <a name="api-capabilities"></a>API-Funktionen

Einige der Funktionen der **Spracherkennungs**-API (insbesondere im Bereich der Anpassung) sind über REST verfügbar. In der folgenden Tabelle werden die Funktionen der einzelnen Methoden für den Zugriff auf die API beschrieben. Eine vollständige Liste der Funktionen und API-Details finden Sie in der [Swagger-Referenz](https://westus.cris.ai/swagger/ui/index).

| Anwendungsfall | REST | SDKs |
|-----|-----|-----|----|
| Übertragen einer kurzen Äußerung wie einem Befehl (weniger als 15 Sekunden); keine Zwischenergebnisse | JA | JA |
| Übertragen einer längeren Äußerung (länger als 15 Sekunden) | Nein  | JA |
| Übertragen von Audiodatenströmen mithilfe von optionalen Zwischenergebnissen | Nein  | JA |
| Nachvollziehen von Sprecherabsichten mithilfe von LUIS | Nein\* | JA |
| Erstellen von Genauigkeitstests | JA | Nein  |
| Hochladen von Datasets für die Modellanpassung | JA | Nein  |
| Erstellen und Verwalten von Sprachmodellen | JA | Nein  |
| Erstellen und Verwalten von Modellimplementierungen | JA | Nein  |
| Verwalten von Abonnements | JA | Nein  |
| Erstellen und Verwalten von Modellimplementierungen | JA | Nein  |
| Erstellen und Verwalten von Modellimplementierungen | JA | Nein  |

> [!NOTE]
> Der REST-API implementiert Drosselung, durch die API-Anforderungen auf 25 Anforderungen pro 5 Sekunden eingeschränkt werden. Nachrichtenheader informieren über die Grenzwerte.

\* *Sie können die LUIS-Absichten und Entitäten mithilfe eines separaten LUIS-Abonnements ableiten. Über dieses Abonnement kann das SDK LUIS aufrufen und bietet sowohl Ergebnisse für Absichten und Entitäten als auch Sprachtranskriptionen. Mithilfe der REST-API können Sie LUIS selbst aufrufen, um Absichten und Entitäten mithilfe Ihres LUIS-Abonnements abzuleiten.*

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Schnellstart: Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
* [Erfahren Sie, wie die Absichtserkennung anhand von Sprache in C# funktioniert](how-to-recognize-intents-from-speech-csharp.md)

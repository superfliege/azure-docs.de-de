---
title: Was ist die visuelle Bing-Suche?
titleSuffix: Azure Cognitive Services
description: Die visuelle Bing-Suche stellt Details oder Informationen zu einem Bild bereit, wie z. B. ähnliche Bilder oder Einkaufsquellen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 9e323e9ec928d9f8b0592850adcd088a589ebf28
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863090"
---
# <a name="what-is-the-bing-visual-search-api"></a>Was ist die API für die visuelle Bing-Suche?

Die API für die visuelle Bing-Suche bietet ähnliche Bilddetails wie auf „bing.com/images“. Durch Hochladen oder Angabe einer URL für ein Bild kann diese API eine Vielzahl entsprechender Details ermitteln. Dazu gehören visuell ähnliche Bilder, Einkaufsquellen, Webseiten, die das Bild enthalten, und vieles mehr. Bei Verwendung der [Bing-Bildersuche-API](../bing-image-search/overview.md) können Sie Erkenntnistoken verwenden, die den Suchergebnissen der API angefügt sind, statt ein Bild hochzuladen.

## <a name="insights"></a>Einblicke

Mit der visuellen Suche erhalten Sie die folgenden Informationen:

| Erkenntnis                              | BESCHREIBUNG |
|--------------------------------------|-------------|
| Visuell ähnliche Bilder              | Eine Liste mit Bildern, die dem Eingabebild visuell ähnlich sind. |
| Visuell ähnliche Produkte            | Produkte, die dem gezeigten Produkt visuell ähnlich sind.            |
| Einkaufsquellen                     | Orte, an denen Sie das Produkt auf dem Eingabebild kaufen können.            |
| Verwandte Suchvorgänge                     | Verwandte Suchvorgängen, die von anderen durchgeführt wurden oder die auf dem Bildinhalt basieren.            |
| Webseiten, die das Bild enthalten     | Webseiten, die das Eingabebild enthalten.            |
| Rezepte                              | Webseiten, die Rezepte für das Gericht auf dem Eingabebild enthalten.            |

Zusätzlich zu diesen Informationen werden bei der visuellen Suche auch verschiedene Begriffe (Tags) zurückgegeben, die vom Eingabebild abgeleitet werden. Über diese Tags können Sie sich mit den zugeordneten Bildkonzepten vertraut machen. Zeigt das Eingabebild beispielsweise einen bekannten Sportler, könnte eines der Tags etwa den Namen des Sportlers darstellen, ein anderes könnte „Sport“ lauten. Oder bei dem Bild eines Apfelkuchens könnten die Tags z.B. „Apfelkuchen“, „Kuchen“, „Nachtisch“ lauten. Auf diese Weise können sich Benutzer auch mit verwandten Themen befassen.

Die Ergebnisse der visuellen Suche beinhalten auch Begrenzungsrahmen für relevante Bildbereiche. Sind auf dem Bild beispielsweise mehrere Prominente abgebildet, können die Ergebnisse etwa Begrenzungsrahmen für jeden erkannten Prominenten enthalten. Oder wenn Bing auf dem Bild ein Produkt oder Kleidungsstück erkennt, beinhaltet das Ergebnis möglicherweise einen Begrenzungsrahmen für das erkannte Produkt oder Kleidungsstück.

> [!IMPORTANT]
> Wenn Sie Bildauswertungen mit der Bing-Bildersuche-API abrufen, können Sie auch zur API für die visuelle Bing-Suche wechseln, die umfassendere Erkenntnisse bietet.

## <a name="workflow"></a>Workflow

Die API für die visuelle Bing-Suche ist ein RESTful-Webdienst und kann somit problemlos in jeder Programmiersprache aufgerufen werden, die HTTP-Anforderungen beherrscht und JSON analysieren kann. Der Dienst kann entweder über die REST-API oder über das SDK verwendet werden.

1. Erstellen Sie ein Cognitive Services-API-Konto mit Zugriff auf die Bing-Suche-APIs. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen.
2. Senden Sie eine Anforderung mit einer gültigen Suchabfrage an die API.
3. Analysieren Sie die zurückgegebene JSON-Nachricht, um die API-Antwort zu verarbeiten.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich zunächst die [interaktive Demo](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/) für die Bing-Bildersuche-API an.
Diese Demo zeigt, wie Sie schnell eine Suchabfrage anpassen und das Web nach Bildern durchsuchen.

Wenn Sie zum Aufrufen der API bereit sind, erstellen Sie ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses [Konto erstellen](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Die folgenden Schnellstartanleitungen ermöglichen einen schnellen Einstieg in die Verwendung Ihrer ersten Anforderung: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).


## <a name="see-also"></a>Weitere Informationen

* Die [Referenz zur visuellen Bing-Suche](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) enthält Definitionen und Informationen zu den Endpunkten, Headern, API-Antworten und Abfrageparametern, die Sie verwenden können, um bildbasierte Suchergebnisse anzufordern.

* In den [Verwendungs- und Anzeigeanforderungen für Bing](./use-and-display-requirements.md) erfahren Sie, wie Inhalte und Informationen verwendet werden dürfen, die über die Bing-Suche-APIs gefunden wurden.

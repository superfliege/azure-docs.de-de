---
title: Sprachunterstützung – Textanalyse-API
titleSuffix: Azure Cognitive Services
description: 'Eine Liste der von der Textanalyse-API unterstützten natürlichen Sprachen. In diesem Artikel wird erläutert, welche Sprachen für die einzelnen Vorgänge unterstützt werden: Standpunktanalyse, Schlüsselbegriffserkennung, Spracherkennung und Entitätserkennung.'
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: 63c8ebe8eda049a770cecfd445757e4b5a2de1ea
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067040"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Sprach- und Regionsunterstützung für die Textanalyse-API

In diesem Artikel wird erläutert, welche Sprachen für die einzelnen Vorgänge unterstützt werden: Standpunktanalyse, Schlüsselbegriffserkennung und Spracherkennung.

## <a name="language-detection"></a>Spracherkennung

Die Textanalyse-API erkennt bis zu 120 verschiedene Sprachen. Die Spracherkennung gibt das „Skript“ einer Sprache zurück. Für den englischen Satz „I have a dog“ wird beispielsweise `en` anstelle von `en-US` zurückgegeben. Der einzige Sonderfall tritt im Chinesischen auf. Für diese Sprache gibt die Spracherkennungsfunktion `zh_CHS` oder `zh_CHT` zurück, wenn sie das Skript anhand des verfügbaren Texts ermitteln kann. In Situationen, in denen kein bestimmtes Skript für ein Dokument auf Chinesisch ermittelt werden kann, wird nur `zh` zurückgegeben.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-recognition"></a>Standpunktanalyse, Schlüsselbegriffserkennung und Entitätserkennung

Für die Standpunktanalyse, Schlüsselbegriffserkennung und Entitätserkennung ist die Liste der unterstützten Sprachen selektiver, da die Analysetools auf die sprachlichen Regeln zusätzlicher Sprachen ausgelegt sind.

## <a name="language-list-and-status"></a>Liste und Status der Sprachen

Die Sprachunterstützung wird in der Vorschauversion eingeführt und soll allmählich in die allgemein verfügbare Version integriert werden. Die beiden Versionen sind unabhängig voneinander und vom Textanalysedienst im Allgemeinen. Sprachen können selbst dann noch ausschließlich für die Vorschauversion verfügbar sein, wenn eine Version der Textanalyse-API allgemein verfügbar ist.

| Sprache    | Sprachcode | Stimmung | Schlüsselwörter | Entitätserkennung |   Notizen  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Dänisch      | `da`          | ✔ \*     | ✔           |             |     |
| Niederländisch       | `nl`          | ✔ \*     | ✔          |             |     |
| Englisch     | `en`          | ✔        | ✔           |  ✔ \*   |      |
| Finnisch     | `fi`          | ✔ \*     | ✔           |             |     |
| Französisch      | `fr`          | ✔        | ✔           |             |     |
| Deutsch      | `de`          | ✔ \*     | ✔           |            |     |
| Griechisch       | `el`          | ✔ \*     |             |            |     |
| Italienisch     | `it`          | ✔ \*     | ✔           |             |     |
| Japanisch    | `ja`          |          | ✔           |            |     |
| Koreanisch      | `ko`          |          | ✔           |            |     |
| Norwegisch (Bokmål) | `no`          | ✔ \*     |  ✔          |             |     |
| Polnisch      | `pl`          | ✔ \*     |  ✔          |             |     |
| Portugiesisch (Portugal) | `pt-PT`| ✔        |  ✔          |       |`pt` wird ebenfalls akzeptiert.|
| Portugiesisch (Brasilien)   | `pt-BR`|          |  ✔   |         |     |
| Russisch     | `ru`          | ✔ \*     | ✔           |             |     |
| Spanisch     | `es`          | ✔        | ✔           |   ✔ \*\*      |     |
| Schwedisch     | `sv`          | ✔ \*     | ✔           |             |     |
| Türkisch     | `tr`          | ✔ \*     |             |             |  |

\* deutet auf Sprachunterstützung in der Vorschauversion hin.

\*\* Die Entitätsextraktion für Spanisch ist nur in [(Version 2.1 – Vorschau)](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) verfügbar.

## <a name="see-also"></a>Weitere Informationen

[Dokumentationsseite zu Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)   
[Produktseite zu Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)

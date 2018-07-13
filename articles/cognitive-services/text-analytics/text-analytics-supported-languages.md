---
title: Unterstützte Sprachen in der Textanalyse-API – Azure Cognitive Services | Microsoft-Dokumentation
description: Liste der allgemein verfügbaren Versionen und der Vorschauversionen der Sprachunterstützung für die Textanalyse-API. Gilt für die Standpunktanalyse, die Schlüsselbegriffserkennung und die Spracherkennung.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: 2d341cfaf261bea6367bb55dd5d322f419e22d34
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377810"
---
# <a name="supported-languages-in-the-text-analytics-api"></a>Unterstützte Sprachen in der Textanalyse-API

In diesem Artikel wird erläutert, welche Sprachen für die einzelnen Vorgänge unterstützt werden: Standpunktanalyse, Schlüsselbegriffserkennung und Spracherkennung.

## <a name="language-detection"></a>Spracherkennung

Die Textanalyse-API erkennt bis zu 120 verschiedene Sprachen. Die Spracherkennung gibt das „Skript“ einer Sprache zurück. Für den englischen Satz „I have a dog“ wird beispielsweise `en` anstelle von `en-US` zurückgegeben. Der einzige Sonderfall tritt im Chinesischen auf. Für diese Sprache gibt die Spracherkennungsfunktion `zh_CHS` oder `zh_CHT` zurück, wenn sie das Skript anhand des verfügbaren Texts ermitteln kann. In Situationen, in denen kein bestimmtes Skript für ein Dokument auf Chinesisch ermittelt werden kann, wird nur `zh` zurückgegeben.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-linking"></a>Standpunktanalyse, Schlüsselbegriffserkennung und Entitätsverknüpfung

Für die Standpunktanalyse, Schlüsselbegriffserkennung und Entitätsverknüpfung ist die Liste der unterstützten Sprachen selektiver, da die Analysetools auf die sprachlichen Regeln zusätzlicher Sprachen ausgelegt sind.

## <a name="language-list-and-status"></a>Liste und Status der Sprachen

Die Sprachunterstützung wird in der Vorschauversion eingeführt und soll allmählich in die allgemein verfügbare Version integriert werden. Die beiden Versionen sind unabhängig voneinander und vom Textanalysedienst im Allgemeinen. Sprachen können selbst dann noch ausschließlich für die Vorschauversion verfügbar sein, wenn eine Version der Textanalyse-API allgemein verfügbar ist.

| Sprache    | Sprachcode | Stimmung | Schlüsselwörter | Entitätsverknüpfung |   Notizen  |
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
| Spanisch     | `es`          | ✔        | ✔           |     |     |
| Schwedisch     | `sv`          | ✔ \*     | ✔           |             |     |
| Türkisch     | `tr`          | ✔ \*     |             |             |     |

\* deutet auf Sprachunterstützung in der Vorschauversion hin.

## <a name="see-also"></a>Weitere Informationen

[Dokumentationsseite zu Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)   
[Produktseite zu Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)

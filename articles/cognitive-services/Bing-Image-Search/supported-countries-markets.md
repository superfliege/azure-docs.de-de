---
title: Unterstützte Länder und Sprachen für die Bing-Bildersuche-API in Azure | Microsoft Docs
description: Finden Sie heraus, welche Länder und Sprachen durch die Bing-Bildersuche-API unterstützt werden.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: d0d33ee714ba5cd1ce4e846b96c04f755933bee1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376898"
---
# <a name="bing-image-search-countries-and-languages"></a>Länder und Sprachen der Bing-Bildersuche-API

Die Bing-Bildersuche-API unterstützt mehr als drei Dutzend Länder, viele davon mit mehreren Sprachen. Das Angeben eines Lands mit einer Abfrage dient in erster Linie dazu, Suchergebnisse auf der Grundlage von Interesse an diesem Land zu verfeinern. Außerdem können die Ergebnisse Links zu Bing enthalten, und diese Links können die Bing-Benutzeroberfläche entsprechend dem angegebenen Land oder der angegebenen Sprache lokalisieren.

Um das Land und die Sprache anzugeben, legen Sie den `mkt`-Abfrageparameter (Markt) auf einen Code aus der Tabelle **Märkte** unten fest. Der Markt gibt ein Land und eine Sprache an. Wenn der Benutzer es bevorzugt, Text in einer anderen Sprache anzuzeigen, legen Sie den `setLang`-Abfrageparameter auf den entsprechenden Sprachcode fest.

Alternativ können Sie das Land mit dem `cc`-Abfrageparameter angeben. Wenn Sie ein Land angeben, müssen Sie mithilfe des HTTP-Headers `Accept-Language` auch mindestens einen Sprachcode angeben. Die unterstützten Sprachen sind je nach Land unterschiedlich. Sie werden für jedes Land in der Tabelle „Märkte“ angegeben.

> [!NOTE]
> Die API für Bilder zu Trends unterstützt zurzeit nur die folgenden Märkte:
> - en-US (Englisch, USA) 
> - en-CA (Englisch, Kanada) 
> - en-AU (Englisch, Australien) 
> - zh-CN (Chinesisch, China)

## <a name="countries"></a>Länder

|Country|Code|
|-------|----|
|Argentinien|AR|
|Australien|AU|
|Österreich|AT|
|Belgien|BE|
|Brasilien|BR|
|Kanada|CA|
|Chile|CL|
|Dänemark|DK|
|Finnland|FI|
|Frankreich|BV|
|Deutschland|DE|
|Hongkong|HK|
|Indien|IN|
|Indonesien|ID|
|Italien|IT|
|Japan|JP|
|Korea|KR|
|Malaysia|MY|
|Mexiko|MX|
|Niederlande|NL|
|Neuseeland|NZ|
|Norwegen|NO|
|China|CN|
|Polen|PL|
|Portugal|PT|
|Philippinen|PH|
|Russland|RU|
|Saudi-Arabien|SA|
|Südafrika|ZA|
|Spanien|ES|
|Schweden|SE|
|Schweiz|CH|
|Taiwan|TW|
|Türkei|TR|
|Vereinigtes Königreich|GB|
|USA|US|


## <a name="markets"></a>Märkte

|Country|Sprache|Marktcode|
|-------|--------|-----------|
|Argentinien|Spanisch|es-AR|
|Australien|Englisch|en-AU|
|Österreich|Deutsch|de-AT|
|Belgien|Niederländisch|nl-BE|
|Belgien|Französisch|fr-BE|
|Brasilien|Portugiesisch|pt-BR|
|Kanada|Englisch|en-CA|
|Kanada|Französisch|fr-CA|
|Chile|Spanisch|es-CL|
|Dänemark|Dänisch|da-DK|
|Finnland|Finnisch|fi-FI|
|Frankreich|Französisch|fr-FR|
|Deutschland|Deutsch|de-DE|
|Hongkong|Chinesisch (traditionell)|zh-HK|
|Indien|Englisch|en-IN|
|Indonesien|Englisch|en-ID|
|Italien|Italienisch|it-IT|
|Japan|Japanisch|ja-JP|
|Korea|Koreanisch|ko-KR|
|Malaysia|Englisch|en-MY|
|Mexiko|Spanisch|es-MX|
|Niederlande|Niederländisch|nl-NL|
|Neuseeland|Englisch|en-NZ|
|China|Chinesisch|zh-CN|
|Polen|Polnisch|pl-PL|
|Portugal|Portugiesisch|pt-PT|
|Philippinen|Englisch|en-PH|
|Russland|Russisch|ru-RU|
|Saudi-Arabien|Arabisch|ar-SA|
|Südafrika|Englisch|en-ZA|
|Spanien|Spanisch|es-ES|
|Schweden|Schwedisch|sv-SE|
|Schweiz|Französisch|fr-CH|
|Schweiz|Deutsch|de-CH|
|Taiwan|Chinesisch (traditionell)|zh-TW|
|Türkei|Türkisch|tr-TR|
|Vereinigtes Königreich|Englisch|en-GB|
|USA|Englisch|en-US|
|USA|Spanisch|es-US|

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Endpunkten der Bing-News-Suche finden Sie unter [Referenz zur News-Bildersuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

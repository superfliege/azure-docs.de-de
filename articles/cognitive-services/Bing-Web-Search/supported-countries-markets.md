---
title: Durch die Bing-Websuche-API unterstützte Länder, Regionen und Sprachen
titleSuffix: Azure Cognitive Services
description: Finden Sie heraus, welche Länder/Regionen und Sprachen von der Bing-Websuche-API unterstützt werden.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 8/16/2018
ms.author: erhopf
ms.openlocfilehash: b954814d1c26e8afd24e0bb13e74c493f0fd2c55
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125118"
---
# <a name="countries-regions-and-languages-supported-by-the-bing-web-search-api"></a>Durch die Bing-Websuche-API unterstützte Länder, Regionen und Sprachen

Die Bing-Websuche-API unterstützt über drei Dutzend Länder oder Regionen, viele davon mit mehreren Sprachen. Die Angabe eines Lands oder einer Region mit einer Abfrage dient dazu, Suchergebnisse auf Grundlage der Interessen des Lands oder der Region zu verfeinern. Die Ergebnisse können Links zu Bing enthalten, und diese Links können die Bing-Benutzeroberfläche entsprechend dem angegebenen Land bzw. der angegebenen Region oder der angegebenen Sprache lokalisieren.

Sie können mit dem Abfrageparameter `cc` ein Land oder eine Region angeben. Bei der Angabe eines Lands oder einer Region müssen Sie mithilfe des [`Accept-Language`-Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers) auch mindestens einen Sprachcode angeben. Unter [Märkte](#Markets) finden Sie eine Aufstellung der in jedem Markt unterstützten Sprachen.

Alternativ können Sie den Markt mit dem Abfrageparameter `mkt` und einem Code aus der Tabelle **Märkte** angeben. Bei der Angabe eines Markts werden gleichzeitig ein Land oder eine Region und die bevorzugte Sprache angegeben. Mit dem Abfrageparameter `setLang` können Sie die Sprache ausdrücklich festlegen.

## <a name="countriesregions"></a>Länder/Regionen

|Land/Region|Code|
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

|Land/Region|Sprache|Marktcode|
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
|Norwegen|Norwegisch|no-NO|
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

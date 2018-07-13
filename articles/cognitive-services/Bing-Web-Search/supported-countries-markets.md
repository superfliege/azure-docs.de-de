---
title: Unterstützte Länder und Sprachen für die Bing-Websuche-API in Azure | Microsoft-Dokumentation
description: Finden Sie heraus, welche Länder und Sprachen von der Bing-Websuche-API unterstützt werden.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 7b62c4a4feb7144662a8fe4d692f11f1efe5db1b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373306"
---
# <a name="bing-web-search-countries-and-languages"></a>Länder und Sprachen der Bing-Websuche-API

Die Bing-Websuche-API unterstützt mehr als drei Dutzend Länder, viele davon mit mehreren Sprachen. Die Angabe eines Lands mit einer Abfrage dient in erster Linie dazu, Suchergebnisse auf Grundlage des Interesse an diesem Land zu verfeinern. Außerdem können die Ergebnisse Links zu Bing enthalten, und diese Links können die Bing-Benutzeroberfläche entsprechend dem angegebenen Land oder der angegebenen Sprache lokalisieren.

Sie können mit dem `cc`-Abfrageparameter ein Land angeben. Wenn Sie ein Land angeben, müssen Sie mithilfe des HTTP-Headers `Accept-Language` auch mindestens einen Sprachcode angeben. Die unterstützten Sprachen sind je nach Land unterschiedlich. Sie werden für jedes Land in der Tabelle „Märkte“ angegeben.

Alternativ können Sie den Markt mit dem Abfrageparameter `mkt` und einen Code aus der Tabelle **Märkte** angeben. Bei der Angabe eines Markts werden gleichzeitig ein Land und die bevorzugte Sprache angegeben. Der Abfrageparameter `setLang` kann in diesem Fall auf einen Sprachcode festgelegt sein. Dies ist in der Regel dieselbe von `mkt` angegebene Sprache, es sei denn, der Benutzer möchte Bing in einer anderen Sprache anzeigen.

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

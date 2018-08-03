---
title: Unterstützte Länder/Regionen und Sprachen für die Bing-Websuche-API in Azure | Microsoft-Dokumentation
description: Finden Sie heraus, welche Länder/Regionen und Sprachen von der Bing-Websuche-API unterstützt werden.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: e1994ddf3bf71d01adeac4ff8688bdd1e6eac660
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001717"
---
# <a name="bing-web-search-countriesregions-and-languages"></a>Länder/Regionen und Sprachen der Bing-Websuche

Die Bing-Websuche-API unterstützt mehr als drei Dutzend Länder/Regionen, viele davon mit mehreren Sprachen. Die Angabe eines Lands/einer Region mit einer Abfrage dient in erster Linie dazu, Suchergebnisse auf Grundlage des Interesses an diesem Land/dieser Region zu verfeinern. Außerdem können die Ergebnisse Links zu Bing enthalten, und diese Links können die Bing-Benutzeroberfläche entsprechend dem angegebenen Land/der angegebenen Region oder der angegebenen Sprache lokalisieren.

Sie können mit dem `cc`-Abfrageparameter ein Land/eine Region angeben. Wenn Sie ein Land/eine Region angeben, müssen Sie mithilfe des HTTP-Headers `Accept-Language` auch mindestens einen Sprachcode angeben. Die unterstützten Sprachen sind je nach Land/Region unterschiedlich. Sie werden für jedes Land/jede Region in der Tabelle „Märkte“ angegeben.

Alternativ können Sie den Markt mit dem Abfrageparameter `mkt` und einen Code aus der Tabelle **Märkte** angeben. Bei der Angabe eines Markts werden gleichzeitig ein Land/eine Region und die bevorzugte Sprache angegeben. Der Abfrageparameter `setLang` kann in diesem Fall auf einen Sprachcode festgelegt sein. Dies ist in der Regel dieselbe von `mkt` angegebene Sprache, es sei denn, der Benutzer möchte Bing in einer anderen Sprache anzeigen.

## <a name="countries"></a>Länder

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

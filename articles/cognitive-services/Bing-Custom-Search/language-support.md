---
title: Sprachunterstützung – API für die benutzerdefinierte Bing-Suche
titleSuffix: Azure Cognitive Services
description: Eine Liste der unterstützten Sprachen und Regionen für die API für die benutzerdefinierte Bing-Suche.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: v-gedod
ms.openlocfilehash: 24b14c4f0ca185a7916aeb19671a22ff46a23ee2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194847"
---
# <a name="language-and-region-support-for-the-bing-custom-search-api"></a>Sprach- und Regionsunterstützung für die API für die benutzerdefinierte Bing-Suche

Die API für die benutzerdefinierte Bing-Suche unterstützt mehr als drei Dutzend Länder/Regionen, viele davon mit mehreren Sprachen.

Obwohl es optional ist, sollte die Anforderung den Abfrageparameter [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#mkt) angeben, wodurch der Markt identifiziert wird, von dem die Ergebnisse stammen sollen. Unter [Abfrageparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) finden Sie eine Liste optionaler Abfrageparameter.

Sie können mit dem `cc`-Abfrageparameter ein Land/eine Region angeben. Wenn Sie ein Land/eine Region angeben, müssen Sie mithilfe des Headers `Accept-Language` auch mindestens einen Sprachcode angeben. Die unterstützten Sprachen sind je nach Land/Region unterschiedlich. Sie werden für jedes Land/jede Region in der Tabelle **Märkte** angegeben.

Der `Accept-Language`-Header und der `setLang`-Abfrageparameter schließen sich gegenseitig aus. Geben Sie daher nicht beide an. Weitere Informationen finden Sie unter [Accept-Language](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#acceptlanguage).

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

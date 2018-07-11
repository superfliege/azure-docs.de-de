---
title: Unterstützte Länder und Sprachen für die API für visuelle Bing-Suche | Microsoft-Dokumentation
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Finden Sie heraus, welche Länder und Sprachen durch die API für die visuelle Bing-Suche unterstützt werden.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 4723d028cc22caf8be3eb294b52506ec112cbab5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376418"
---
# <a name="bing-visual-search-countries-and-languages"></a>Länder und Sprachen der visuellen Bing-Suche

Die API für die visuelle Bing-Suche unterstützt mehr als drei Dutzend Länder, viele davon mit mehreren Sprachen. Jede Anforderung sollte das Land und die gewählte Sprache des Benutzers enthalten. Wenn der Markt des Benutzers bekannt ist, kann Bing entsprechende Ergebnisse zurückgeben. Wenn Sie kein Land und keine Sprache angeben, bemüht sich Bing, das Land und die Sprache des Benutzers zu ermitteln. Da die Ergebnisse Links zu Bing enthalten können, kann bei Kenntnis des Lands und der Sprache eine bevorzugte lokalisierte Bing-Benutzeroberfläche bereitgestellt werden, wenn der Benutzer auf die Bing-Links klickt.

Um das Land und die Sprache anzugeben, legen Sie den `mkt`-Abfrageparameter (Markt) auf einen Code aus der Tabelle **Märkte** unten fest. Der Markt gibt ein Land und eine Sprache an. Wenn der Benutzer es bevorzugt, Text in einer anderen Sprache anzuzeigen, legen Sie den `setLang`-Abfrageparameter auf den entsprechenden Sprachcode fest.

Alternativ können Sie das Land mit dem `cc`-Abfrageparameter angeben. Wenn Sie ein Land angeben, müssen Sie mithilfe des HTTP-Headers `Accept-Language` auch mindestens einen Sprachcode angeben. Die unterstützten Sprachen sind je nach Land unterschiedlich. Sie werden für jedes Land in der Tabelle „Märkte“ angegeben.



> [!NOTE]
> Es gelten folgende Markteinschränkungen:
> 
> - Anmerkungen zur Bilderkennung sind nur in Englisch verfügbar. 
> - Auswertungen in Bezug auf Rezepte, Einkaufsquellen und Seiten mit gleichem Inhalt sind nur für den englischsprachigen US-Markt verfügbar. 


## <a name="countries"></a>Länder

|Land|Code|
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
|Frankreich|FR|
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

|Land|Sprache|Marktcode|
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

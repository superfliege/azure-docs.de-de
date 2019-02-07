---
title: 'Sprachunterstützung: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Eine Liste der von QnA Maker für Ihre Wissensdatenbank unterstützten Kulturen und natürlichen Sprachen. Mischen Sie Sprachen nicht in derselben Wissensdatenbank.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/04/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 5a4938ec37cf006e5cd6342687e2c50cadd5272e
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55731069"
---
# <a name="language-and-region-support-for-qna-maker"></a>Sprach- und Regionsunterstützung für QnA Maker

Die Sprache einer Wissensdatenbank wirkt sich auf die Fähigkeit von QnA Maker zum automatischen Extrahieren von Fragen und Antworten aus [Quellen](../Concepts/data-sources-supported.md) aus. Sie beeinflusst auch die Relevanz der Ergebnisse, die QnA Maker als Antwort auf Benutzerabfragen zurückgibt.

## <a name="auto-extraction"></a>Automatisches Extrahieren
QnA Maker unterstützt das Extrahieren von Fragen und Antworten auf Seiten in jeder Sprache, aber für folgende Sprachen ist die Effektivität viel höher, da QnA Maker Stichwörter verwendet, um Fragen zu identifizieren.

|Unterstützte Sprachen| Gebietsschema|
|-----|----|
|Englisch|en-*|
|Französisch|fr-*|
|Italienisch|it-*|
|Deutsch|de-*|
|Spanisch|es-*|

## <a name="query-matching-and-relevance"></a>Abfrageabgleich und Relevanz
QnA Maker nutzt [Sprachanalysefunktionen](https://docs.microsoft.com/rest/api/searchservice/language-support) in der Azure-Suche, um Ergebnisse bereitzustellen. Für en-*-Sprachen sind spezielle Features zur erneuten Rangzuweisung verfügbar, die eine höhere Relevanz ermöglichen.

Während die Azure Search-Funktionen für unterstützte Sprachen ebenbürtig ist, verfügt QnA Maker über ein zusätzliches Rangfolgemodul, die oberhalb der Azure Search-Ergebnisse ansetzen. In diesem Rangfolgemodul verwenden wir einige besondere semantische und wortbasierte Funktionen in en-*, die für andere Sprachen noch nicht verfügbar sind. Wir machen diese nicht verfügbar, da sie Teil der internen Verarbeitung des Rangfolgemoduls sind. 

QnA Maker erkennt die Sprache der Wissensdatenbank während der Erstellung automatisch und richtet die Analysefunktion entsprechend ein. Sie können Wissensdatenbanken in den folgenden Sprachen erstellen. Lesen Sie [diesen Artikel](../How-To/language-knowledge-base.md), um weitere Informationen zur Verarbeitung von Sprachen in QnA Maker zu erhalten.


> [!Tip]
> Die Sprachanalysefunktion kann nach der Festlegung nicht mehr geändert werden. Die Sprachanalyse gilt zudem für alle Wissensdatenbanken in einem [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md). Wenn Sie Wissensdatenbanken in verschiedenen Sprachen planen, sollten Sie diese in separaten QnA Maker-Diensten erstellen.

|Unterstützte Sprachen|
|-----|
|Arabisch|
|Armenisch|,
Bangla|
|Baskisch|
|Bulgarisch|
|Katalanisch|
|Chinesisch (vereinfacht)|
|Chinesisch (traditionell)|
|Kroatisch|
|Tschechisch|
|Dänisch|
|Niederländisch|
|Englisch|
|Estnisch|
|Finnisch|
|Französisch|
|Galizisch|
|Deutsch|
|Griechisch|
|Gujarati|
|Hebräisch|
|Hindi|
|Ungarisch|
|Isländisch|
|Indonesisch|
|Irisch|
|Italienisch|
|Japanisch|
|Kannada|
|Koreanisch|
|Lettisch|
|Litauisch|
|Malayalam|
|Malaiisch|
|Norwegisch|
|Polnisch|
|Portugiesisch|
|Pandschabi|
|Rumänisch|
|Russisch|
|Serbisch (Kyrillisch)|
|Serbisch (Lateinisch)|
|Slowakisch|
|Slowenisch|
|Spanisch|
|Schwedisch|
|Tamilisch|
|Telugu|
|Thailändisch|
|Türkisch|
|Ukrainisch|
|Urdu|
|Vietnamesisch|

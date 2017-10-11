---
title: Fragen Sie Ihre Azure-Suchindex | Microsoft Docs
description: Erstellen Sie eine Suchabfrage in Azure Search, und verwenden Sie die Suchparameter zu filtern und Sortieren von Suchergebnissen.
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 69205d7a-363f-4b92-a53f-6ca818a3d2c7
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: ashmaka
ms.openlocfilehash: a22b82829df4659681940267e64c98d345453958
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="query-your-azure-search-index"></a>Fragen Sie Ihren Azure Search-index
> [!div class="op_single_selector"]
> * [Übersicht](search-query-overview.md)
> * [Verwaltungsportal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Wenn Sie Search-Anforderungen für Azure Search übermitteln zu können, stehen eine Anzahl von Parametern, die zusammen mit den tatsächlichen Wörtern angegeben werden können, die in das Suchfeld der Anwendung eingegeben werden. Diese Abfrageparameter können Sie einige eine umfassendere Steuerung erzielen die [Volltext-Suchfunktionen](search-lucene-query-architecture.md).

Im folgenden finden Sie eine Liste, die allgemeine Verwendungsmöglichkeiten der Abfrageparameter in Azure Search kurz erläutert. Vollständige Abdeckung von Abfrageparametern und deren Verhalten finden Sie unter den detaillierten Seiten für die [REST-API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) und [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters#microsoft_azure_search_models_searchparameters#properties_summary).

## <a name="types-of-queries"></a>Typen von Abfragen
Azure Search bietet viele Optionen zum Erstellen von äußerst wichtiger Abfragen. Die zwei Haupttypen von Abfragen, die Sie verwenden werden `search` und `filter`. Ein `search` Abfrage sucht nach einem oder mehreren Begriffen in allen *durchsuchbaren* Felder in Ihren Index und funktioniert wie erwartet eine Suchmaschine wie Google oder Bing verwendet würde. Ein `filter` Abfrage ergibt einen booleschen Ausdruck über alle *filterbaren* Felder in einem Index. Im Gegensatz zu `search` Abfragen `filter` Abgleich von Abfragen der genaue Inhalt eines Felds, d. h., sie sind für Zeichenfolgenfelder Groß-/Kleinschreibung.

Suchen und Filtern können separat oder zusammen verwendet werden. Wenn Sie diese zusammen verwenden, der Filter wird zuerst angewendet, für den gesamten Index, und klicken Sie dann die Suche durchgeführt wird, auf dem die Ergebnisse des Filters. Filter kann daher ein nützliches Verfahren, um die abfrageleistung zu verbessern, da sie den Satz von Dokumenten reduzieren, die die Suchabfrage verarbeiten muss.

Die Syntax für Filterausdrücke ist eine Teilmenge der [OData-Filtersprache](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Für Suchabfragen verwenden Sie entweder die [eine vereinfachte Syntax](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) oder [Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) wird die nachstehend erläutert.

### <a name="simple-query-syntax"></a>Einfache Abfragesyntax
Die [einfache Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) ist der Standard-Abfragesprache, die in Azure Search verwendet. Die einfache Abfragesyntax unterstützt eine Reihe von allgemeinen Suchoperatoren einschließlich AND, OR, NOT, Ausdruck, Suffix und von rangfolgenoperatoren.

### <a name="lucene-query-syntax"></a>Lucene-Abfragesyntax
Die [Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) können Sie mit der weithin und ausdrucksbasierte Abfragesprache entwickelt als Teil [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Mithilfe dieser Abfragesyntax können Sie einfach die folgenden Funktionen zu erreichen: [Feld bezogenen Abfragen](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields), [fuzzy-Suche](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy), [NEAR-Suche](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity), [Begriff verstärkungsfaktors](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost), [Suche mit regulärem Ausdruck](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex), [Platzhaltersuche](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard), [Syntax Grundlagen](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax), und [von Abfragen mit booleschen Operatoren](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean).

## <a name="ordering-results"></a>Anordnen von Ergebnissen
Wenn Sie Ergebnisse für eine Suchabfrage zu empfangen, können Sie anfordern, dass Azure Search die Ergebnisse sortiert nach Werten in einem bestimmten Feld dient. Standardmäßig ordnet Azure Search die Suchergebnisse basierend auf den Rang des suchbewertung jedes Dokument, das von abgeleitet ist [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Wenn Sie möchten, dass Azure Search zurückzugebenden Ihre Ergebnisse sortiert werden durch einen anderen Wert als der suchbewertung können Sie die `orderby` Parameter zu suchen. Können, geben Sie den Wert von der `orderby` Parameter Feldnamen und Aufrufe an die [ `geo.distance()` Funktion](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) für räumlichen Werte. Jeder Ausdruck kann folgen `asc` um anzugeben, dass die Ergebnisse in aufsteigender Reihenfolge angefordert werden und `desc` um anzugeben, dass die Ergebnisse in absteigender Reihenfolge angefordert werden. Die Standard-Rangfolge Aufsteigend.

## <a name="paging"></a>Paging
Azure Search vereinfacht die Auslagerung von Suchergebnissen zu implementieren. Mithilfe der `top` und `skip` Parameter, können Sie reibungslos Search-Anforderungen, mit denen Sie die gesamte Gruppe von Suchergebnissen in verwaltbare, geordnete Teilmengen zu erhalten, die einfach gutes Suche UI-Methoden ermöglichen ausgeben. Wenn Sie diese kleineren Teilmengen von Ergebnisse empfangen zu können, erhalten Sie auch die Anzahl der Dokumente in der gesamten Gruppe von Suchergebnissen.

Weitere Informationen finden Sie Informationen zu paging von Suchergebnissen im Artikel [wie auf der Seite Suchergebnisse in Azure Search](search-pagination-page-layout.md).

## <a name="hit-highlighting"></a>Drücken Sie die Hervorhebung
In Azure Search hervorgehoben werden die genauen Teil Suchergebnisse, die Suchabfrage entsprechen, wird leicht, mit der `highlight`, `highlightPreTag`, und `highlightPostTag` Parameter. Sie können angeben, welche *durchsuchbaren* Felder müssen ihre übereinstimmenden Text hervorgehoben sowie die genaue Zeichenfolge Tags anzufügende am Anfang und Ende des übereinstimmenden Texts, die Azure Search zurückgibt angeben.

## <a name="try-out-query-syntax"></a>Probieren Sie Abfragesyntax

Die beste Möglichkeit, die Syntaxunterschiede zu verstehen ist durch das Senden von Abfragen und Ergebnisse überprüfen.

+ Verwendung [-Explorer Durchsuchen](search-explorer.md) im Azure-Portal. Durch die Bereitstellung von [der Beispiel-Index](search-get-started-portal.md), können Sie den Index in Minuten, die mithilfe von Tools im Portal Abfragen.

+ Verwendung [Fiddler](search-fiddler.md) oder Chrome-Postman, um Abfragen auf einen Index zu übermitteln, die Sie in Ihrem Suchdienst hochgeladen haben. Beide Tools unterstützt REST-Aufrufe an einen HTTP-Endpunkt. 
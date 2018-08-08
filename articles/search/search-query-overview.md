---
title: Grundlagen zu Abfragen in Azure Search | Microsoft-Dokumentation
description: Enthält die Grundlagen zur Erstellung einer Suchabfrage in Azure Search, indem Parameter zum Filtern, Auswählen und Sortieren von Ergebnissen verwendet werden.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/27/2018
ms.openlocfilehash: 4650ad89850f32ae5e83a7ac1cd5eac096b180ed
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366445"
---
# <a name="query-fundamentals-in-azure-search"></a>Grundlagen zu Abfragen in Azure Search

Eine Abfragedefinition in Azure Search ist eine vollständige Spezifikation einer Anforderung mit den folgenden Bestandteilen: URL-Dienstendpunkt, Zielindex, API-Schlüssel zum Authentifizieren der Anforderung, API-Version und eine Abfragezeichenfolge. 

Die Komposition der Abfragezeichenfolge umfasst die Parameter, die in der [API zum Durchsuchen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/search-documents) definiert sind. Am wichtigsten ist der Parameter **search=**, der auch undefiniert bleiben kann, (`search=*`), aber meist aus Begriffen, Ausdrücken und Operatoren besteht, wie beispielsweise hier:

```
"search": "seattle townhouse +\"lake\""
```

Andere Parameter werden verwendet, um die Verarbeitung von Abfragen zu steuern oder die Antwort zu verbessern. Beispiele für die Verwendung von Parametern sind das Beschränken der Suche auf bestimmte Felder, das Festlegen eines Suchmodus zum Modulieren der Voreinstellung für Genauigkeit und Trefferquote und das Hinzufügen einer Anzahl, damit Sie die Ergebnisse nachverfolgen können. 

```
{  
    "search": "seattle townhouse* +\"lake\"",  
    "searchFields": "description, city",  
    "searchMode": "all",
    "count": "true", 
    "queryType": "simple" 
 } 
```

Die Abfragedefinition ist zwar von grundlegender Bedeutung, aber es ist genauso wichtig, wie Ihr Indexschema zulässige Vorgänge von Feld zu Feld angibt. Während der Indexentwicklung bestimmen die Attribute der Felder die zulässigen Vorgänge. Damit ein Feld für die Volltextsuche und die Einbindung in Suchergebnisse qualifiziert ist, muss es sowohl als *durchsuchbar* als auch als *abrufbar* gekennzeichnet werden.

Zur Abfragezeit erfolgt die Ausführung immer für einen Index, und für die Authentifizierung wird ein API-Schlüssel verwendet, der in der Anforderung angegeben ist. Es ist nicht möglich, Indizes zu verknüpfen oder benutzerdefinierte oder temporäre Datenstrukturen als Abfrageziel zu erstellen.  

Abfrageergebnisse werden als JSON-Dokumente in der REST-API gestreamt, aber wenn Sie .NET-APIs verwenden, wird die Serialisierung integriert. Sie können Ergebnisse steuern, indem Sie Parameter für die Abfrage festlegen und bestimmte Felder für das Ergebnis auswählen.

Zusammenfassend lässt sich also festhalten, dass für die Abfrageanforderung im Wesentlichen der Umfang und die Vorgänge angegeben werden: welche Felder in die Suche einbezogen werden sollen, welche Felder in das Resultset einbezogen werden sollen, ob sortiert oder gefiltert werden soll usw. Wenn keine Angaben vorhanden sind, wird eine Abfrage für alle durchsuchbaren Felder als Volltextsuchvorgang ausgeführt, und es wird ein Resultset ohne Bewertungen in willkürlicher Reihenfolge zurückgegeben.

<a name="types-of-queries"></a>

## <a name="types-of-queries-search-and-filter"></a>Typen von Abfragen: Suchen und Filtern

Azure Search bietet viele Optionen, um äußerst leistungsfähige Abfragen zu erstellen. Die zwei wichtigsten Abfragetypen, die Sie verwenden werden, sind `search` und `filter`. 

+ Bei `search`-Abfragen wird nach einem oder mehreren Begriffen in allen *durchsuchbaren* Feldern im Index gesucht. Sie funktionieren so, wie Sie es von einer Suchmaschine wie Google oder Bing erwarten. Für die Beispiele in der Einführung wird der Parameter `search` verwendet.

+ `filter`-Abfragen werten einen booleschen Ausdruck für alle *filterbaren* Felder in einem Index aus. Im Gegensatz zu `search` wird bei einer `filter`-Abfrage der genaue Inhalt eines Felds abgeglichen, einschließlich der Berücksichtigung von Groß-/Kleinschreibung für Zeichenfolgenfelder.

Sie können „search“ und „filter“ separat oder zusammen verwenden. Ein eigenständiger Filter ohne Abfragezeichenfolge ist nützlich, wenn der Filterausdruck Dokumente von Interesse vollständig qualifizieren kann. Ohne Abfragezeichenfolge gibt es keine lexikalische oder linguistische Analyse, Bewertung und Rangfolge. Beachten Sie, dass die Suchzeichenfolge leer ist.

```
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "count": "true"
    }
```

Wenn sie zusammen verwendet werden, wird der Filter zuerst auf den gesamten Index angewendet, und anschließend wird die Suche für die Ergebnisse des Filters ausgeführt. Filter können daher eine nützliche Methode zum Verbessern der Abfrageleistung darstellen, da sie die Menge der Dokumente reduzieren, die bei der Suchabfrage verarbeitet werden müssen.

Die Syntax für Filterausdrücke ist eine Teilmenge der [OData-Filtersprache](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Für Suchabfragen können Sie die [vereinfachte Syntax](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) oder die [Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) verwenden. Beide sind unten beschrieben.


## <a name="choose-a-syntax-simple-or-full"></a>Auswählen einer Syntax: einfach oder vollständig

Azure Search basiert auf Apache Lucene und ermöglicht Ihnen die Wahl zwischen zwei Abfrageparsern zur Verarbeitung von typischen und spezialisierten Abfragen. Typische Suchanforderungen werden mit der standardmäßigen [einfachen Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) formuliert. Diese Syntax unterstützt eine Reihe von allgemeinen Suchoperatoren, z.B. AND, OR, NOT, Begriff, Suffix und Rangfolgeoperatoren.

Die [Lucene-Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax) wird aktiviert, wenn Sie der Anforderung **queryType=full** hinzufügen. So wird die häufig genutzte und ausdrucksstarke Abfragesprache verfügbar gemacht, die im Rahmen von [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) entwickelt wurde. Wenn Sie diese Abfragesyntax verwenden, können Sie spezialisierte Abfragen nutzen:

+ [Feldbezogene Abfragen](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields)
+ [Fuzzysuche](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy)
+ [NEAR-Suche](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity)
+ [Term Boosting](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost)
+ [Suche mit regulären Ausdrücken](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex)
+ [Platzhaltersuche](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard)

Boolesche Operatoren sind in beiden Syntaxarten meist identisch, aber in der Lucene-Syntax stehen zusätzliche Formate zur Verfügung:

+ [Boolesche Operatoren in einfacher Syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search#operators-in-simple-search)
+ [Boolesche Operatoren in der vollständigen Lucene-Syntax](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean)

## <a name="required-and-optional-elements"></a>Erforderliche und optionale Elemente

Beim Senden von Suchanforderungen an Azure Search können Sie einige Parameter zusätzlich zu den eigentlichen Wörtern angeben, die Sie in das Suchfeld der Anwendung eingeben. Mit diesen Abfrageparametern haben Sie eine genauere Kontrolle über die [Volltext-Suchoberfläche](search-lucene-query-architecture.md).

Erforderliche Elemente in einer Abfrageanforderung enthalten die folgenden Komponenten:

+ Sammlung mit Dienstendpunkten und Indexdokumenten, hier ausgedrückt als URL `https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`.
+ API-Version (nur REST), ausgedrückt als **api-version=**
+ Abfrage- oder Administrator-API-Schlüssel, ausgedrückt als **api-key=**
+ Abfragezeichenfolge, ausgedrückt als **search=**. Die Angabe kann auch weggelassen werden, falls Sie eine leere Suche durchführen möchten. Sie können auch nur einen Filterausdruck als **$filter=** senden.
+ **queryType=**, entweder einfach oder vollständig. Kann weggelassen werden, falls Sie die einfache Standardsyntax nutzen möchten.

Alle anderen Suchparameter sind optional.

## <a name="apis-and-tools-for-testing"></a>APIs und Tools zum Testen

In der folgenden Tabelle sind die APIs und toolbasierten Ansätze zum Übermitteln von Abfragen aufgeführt.

| Methodik | BESCHREIBUNG |
|-------------|-------------|
| [SearchIndexClient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | Client, der zum Abfragen eines Azure Search-Index verwendet werden kann.  <br/>[Weitere Informationen.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Search Documents (REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Durchsuchen von Dokumenten (REST-API)) | GET- oder POST-Methoden für einen Index, wobei Abfrageparameter für zusätzliche Eingaben verwendet werden.  |
| [Fiddler, Postman oder anderes HTTP-Testtool](search-fiddler.md) | Es wird beschrieben, wie Sie einen Anforderungsheader und -text zum Senden von Abfragen an Azure Search einrichten.  |
| [Suchexplorer im Azure-Portal](search-explorer.md) | Stellt eine Suchleiste und Optionen für die Auswahl des Index und von API-Versionen bereit. Die Ergebnisse werden als JSON-Dokumente zurückgegeben. <br/>[Weitere Informationen.](search-get-started-portal.md#query-index) | 

## <a name="manage-search-results"></a>Verarbeiten von Suchergebnissen

Die Parameter der Abfrage können verwendet werden, um das Resultset wie folgt zu strukturieren:

+ Begrenzen der Anzahl von Dokumenten in den Ergebnissen (standardmäßig 50) bzw. Zusammenfassen zu Batches
+ Auswählen der Felder, die in den Ergebnissen enthalten sein sollen
+ Festlegen der Sortierreihenfolge
+ Hinzufügen von Treffermarkierungen, um übereinstimmende Begriffe im Text der Suchergebnisse hervorzuheben

### <a name="tips-for-unexpected-results"></a>Tipps zu unerwarteten Ergebnissen

Es kann vorkommen, dass der Inhalt und nicht die Struktur der Ergebnisse unerwartet ist. Wenn die Abfrageergebnisse nicht wie erwartet ausfallen, können Sie es mit diesen Änderungen der Abfragen versuchen, um zu ermitteln, ob sich die Ergebnisqualität verbessert:

+ Ändern Sie `searchMode=any` (Standard) in `searchMode=all`, damit Übereinstimmungen mit allen Kriterien erforderlich sind, nicht nur mit einem oder mehreren Kriterien. Dies gilt besonders, wenn die Abfrage boolesche Operatoren enthält.

+ Ändern Sie das Abfrageverfahren, wenn eine Textanalyse oder lexikalische Analyse erforderlich ist und der Abfragetyp eine linguistische Verarbeitung ausschließt. Bei der Volltextsuche werden im Rahmen der Textanalyse oder lexikalischen Analyse Rechtschreibfehler, Singular-/Pluralformen und sogar unregelmäßige Verben oder Substantive automatisch korrigiert. Bei einigen Abfragen, z.B. der Fuzzy- oder Platzhaltersuche, ist die Textanalyse nicht Teil der Pipeline für die Abfrageanalyse. Für einige Szenarien wurden reguläre Ausdrücke zur Problemumgehung verwendet. 

### <a name="paging-results"></a>Auslagerungsergebnisse
Mit Azure Search ist es einfach, das Paging (Anordnen auf Seiten) von Suchergebnissen zu implementieren. Mit den Parametern `top` und `skip` können Sie reibungslos Suchanforderungen ausgeben, die Ihnen das Empfangen aller Suchergebnisse in Form von verwaltbaren, sortierten Teilmengen und somit eine benutzerfreundliche Vorgehensweise auf der Suchoberfläche ermöglichen. Wenn Sie diese kleineren Teilmengen mit Ergebnissen empfangen, können Sie auch die Zahl der Dokumente in der Gesamtmenge der Suchergebnisse erhalten.

Weitere Informationen zum Paging von Suchergebnissen finden Sie im Artikel [Anordnen von Suchergebnissen auf Seiten in Azure Search](search-pagination-page-layout.md).

### <a name="ordering-results"></a>Sortieren von Ergebnissen
Wenn Sie Ergebnisse für eine Suchabfrage erhalten, können Sie anfordern, dass Azure Search die Ergebnisse sortiert nach den Werten in einem bestimmten Feld bereitstellt. Azure Search sortiert die Suchergebnisse standardmäßig basierend auf der Rangfolge der Suchbewertung eines Dokuments, die von [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)abgeleitet wird.

Falls Azure Search die Ergebnisse sortiert nach einem anderen Wert als der Suchbewertung zurückgeben soll, können Sie den Suchparameter `orderby` verwenden. Sie können den Wert des Parameters `orderby` angeben, um Feldnamen und Aufrufe der [`geo.distance()`-Funktion](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) für Geowerte einzubinden. Auf jeden Ausdruck kann `asc` folgen, um anzugeben, dass die Ergebnisse in aufsteigender Reihenfolge angefordert werden. Verwenden Sie `desc`, um anzugeben, dass die Ergebnisse in absteigender Reihenfolge zurückgegeben werden sollen. Standardmäßig wird die aufsteigende Sortierung verwendet.


### <a name="hit-highlighting"></a>Treffermarkierung
In Azure Search ist das Hervorheben der exakten Menge von Suchergebnissen, die mit der Suchabfrage übereinstimmen, dank der Parameter `highlight`, `highlightPreTag` und `highlightPostTag` einfach. Sie können angeben, für welche *durchsuchbaren* Felder der gefundene Text hervorgehoben werden soll. Außerdem können Sie die genauen Zeichenfolgentags angeben, die am Anfang und Ende des von Azure Search zurückgegebenen Übereinstimmungstexts angefügt werden sollen.

## <a name="see-also"></a>Weitere Informationen

+ [Funktionsweise der Volltextsuche in Azure Search (Architektur für Abfrageanalyse)](search-lucene-query-architecture.md)
+ [Suchexplorer](search-explorer.md)
+ [Abfragen des Azure Search-Index mit dem .NET SDK](search-query-dotnet.md)
+ [Abfragen des Azure Search-Index mit der REST-API](search-query-rest-api.md)

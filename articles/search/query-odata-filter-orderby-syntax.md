---
title: 'OData-Ausdruckssyntax für Filter und Sortierklauseln: Azure Search'
description: OData-Syntax für Filter- und Sortierausdrücke für Azure Search-Abfragen.
ms.date: 01/31/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: c6dbd95cfd17f5ce49245fcadea299c5a0dfd582
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "56007748"
---
# <a name="odata-expression-syntax-for-filters-and-order-by-clauses-in-azure-search"></a>OData-Ausdruckssyntax für Filter und Sortierklauseln in Azure Search

Azure Search unterstützt eine Teilmenge der OData-Ausdruckssyntax für **$filter**- und **$orderby**-Ausdrücke. Filterausdrücke werden während der Analyse der Abfrage ausgewertet. Sie beschränken die Suche auf bestimmte Felder oder fügen Übereinstimmungskriterien hinzu, die während Indexscans verwendet werden. Sortierausdrücke werden als Nachverarbeitungsschritt auf ein Resultset angewendet. Sowohl Filter als auch Sortierausdrücke sind in einer Abfrageanforderung enthalten und entsprechen unabhängig von der in einem **search**-Parameter verwendeten [einfachen](query-simple-syntax.md) oder [vollständigen](query-lucene-syntax.md) Abfragesyntax der OData-Syntax. Dieser Artikel enthält die Referenzdokumentation für die in Filtern und Sortierausdrücken verwendeten OData-Ausdrücke.

## <a name="filter-syntax"></a>Filtersyntax

Ein **$filter**-Ausdruck kann eigenständig als vollständig ausgedrückte Abfrage ausgeführt werden oder eine Abfrage mit zusätzlichen Parametern verfeinern. Die folgenden Beispiele veranschaulichen einige gängige Szenarien. Im ersten Beispiel ist der Filter der eigentliche Inhalt der Abfrage.


```POST
POST /indexes/hotels/docs/search?api-version=2017-11-11  
    {  
      "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'"  
    }  
```

Ein weiterer häufiger Anwendungsfall ist die Kombination von Filtern mit Faceting. Dabei reduziert der Filter die Oberfläche der Abfrage auf Grundlage einer benutzergesteuerten Facettennavigationsauswahl:

```POST
POST /indexes/hotels/docs/search?api-version=2017-11-11  
    {  
      "search": "test",  
      "facets": [ "tags", "baseRate,values:80|150|220" ],  
      "filter": "rating eq 3 and category eq 'Motel'"  
    }  
```

### <a name="filter-operators"></a>Filteroperatoren  

-   Logische Operatoren (AND, OR, NOT).  

-   Vergleichsausdrücke (`eq, ne, gt, lt, ge, le`). Bei Zeichenfolgenvergleichen wird die Groß-/Kleinschreibung beachtet.  

-   Konstanten der unterstützten [Entity Data Model](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model)-Typen (EDM) (eine Liste der unterstützten Typen finden Sie unter [Supported data types &#40;Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) (Unterstützte Datentypen für Azure Search)). Konstanten von Sammlungstypen werden nicht unterstützt.  

-   Verweise auf Feldnamen. In Filterausdrücken können nur Felder vom Typ `filterable` verwendet werden.  

-   `any` ohne Parameter. Damit wird überprüft, ob ein Feld vom Typ `Collection(Edm.String)` Elemente enthält.  

-   `any` und `all` mit begrenzter Unterstützung für Lambdaausdrücke. 
    
    -   `any/all` werden für Felder vom Typ `Collection(Edm.String)` unterstützt. 
    
    -   `any` kann nur mit einfachen Gleichheitsausdrücken oder mit einer `search.in`-Funktion verwendet werden. Einfache Ausdrücke bestehen aus einem Vergleich zwischen einem einzelnen Feld und einem Literalwert, z. B. `Title eq 'Magna Carta'`.
    
    -   `all` kann nur mit einfachen Ungleichheitsausdrücken oder mit einer `not search.in`-Funktion verwendet werden.   

-   Geofunktionen `geo.distance` und `geo.intersects`. Die Funktion `geo.distance` gibt die Entfernung zwischen zwei Punkten in Kilometern zurück, wobei es sich bei einem Punkt um ein Feld und bei dem anderen Punkt um eine Konstante handelt, die als Teil des Filters übergeben wird. Die Funktion `geo.intersects` gibt TRUE zurück, wenn ein bestimmter Punkt innerhalb eines angegebenen Polygons liegt. Der Punkt ist hierbei ein Feld, und das Polygon wird als Konstante als Teil des Filters übergeben.  

    Das Polygon ist eine zweidimensionale Fläche und wird als eine Reihe von Punkten gespeichert, die einen Begrenzungsring definieren (siehe unten stehendes Beispiel). Das Polygon muss geschlossen sein, d. h. der erste und der letzte Punkt müssen identisch sein. [Punkte in einem Polygon müssen entgegen dem Uhrzeigersinn angegeben werden](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    `geo.distance` gibt in Azure Search die Entfernung in Kilometern zurück. Bei anderen Diensten, die OData-Geovorgänge unterstützen, werden dagegen meist Entfernungen in Metern zurückgegeben.  

    > [!NOTE]  
    >  Wenn Sie „geo.distance“ in einem Filter verwenden, müssen Sie die von der Funktion zurückgegebene Entfernung mithilfe von `lt`, `le`, `gt` oder `ge` mit einer Konstante vergleichen. Die Operatoren `eq` und `ne` werden beim Vergleichen von Entfernungen nicht unterstützt. Eine korrekte Syntax für „geo.distance“ sieht beispielsweise wie folgt aus: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.  

-   Die Funktion `search.in` testet, ob ein bestimmtes Zeichenfolgenfeld einem der Werte in einer angegebenen Liste entspricht. Sie kann auch in Any- oder All-Ausdrücken verwendet werden, um einen einzelnen Wert eines Zeichenfolgensammlungsfelds mit einer Liste von Werten zu vergleichen. Die Gleichheit zwischen dem Feld und den einzelnen Werten in der Liste wird wie beim `eq`-Operator unter Berücksichtigung der Groß-/Kleinschreibung ermittelt. Ein Ausdruck wie `search.in(myfield, 'a, b, c')` entspricht daher `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`, allerdings bietet `search.in` eine deutlich bessere Leistung. 

    Der erste Parameter für die Funktion `search.in` ist der Verweis auf ein Zeichenfolgenfeld (oder eine Bereichsvariable für ein Zeichenfolgensammlungsfeld, sofern `search.in` innerhalb eines `any`- oder `all`- Ausdrucks verwendet wird). Der zweite Parameter ist eine Zeichenfolge, die die Liste der Werte enthält (getrennt durch Leerzeichen oder Kommas). Wenn Sie andere Trennzeichen als Leerzeichen und Kommas verwenden müssen, weil Ihre Werte diese Zeichen enthalten, können Sie einen optionalen dritten Parameter für `search.in` angeben. 

    Dieser dritte Parameter ist eine Zeichenfolge, bei der jedes Zeichen oder jede Teilmenge der Zeichenfolge als Trennzeichen behandelt wird, wenn die Liste der Werte im zweiten Parameter analysiert wird.

    > [!NOTE]   
    >  In manchen Szenarien muss ein Feld mit einer großen Anzahl von Konstantenwerten verglichen werden. Beispielsweise kann es bei der Implementierung von Sicherheitseinschränkungen mit Filtern erforderlich sein, das Dokument-ID-Feld mit einer Liste von IDs zu vergleichen, auf die dem anfordernden Benutzer Lesezugriff gewährt wird. In solchen Szenarien empfehlen wir ausdrücklich, die Funktion `search.in` anstelle einer komplizierten Disjunktion von Gleichheitsausdrücken zu nutzen. Verwenden Sie beispielsweise `search.in(Id, '123, 456, ...')` anstelle von `Id eq 123 or Id eq 456 or ....`. 

>  Bei der Verwendung von `search.in` ist eine Antwortzeit von unter einer Sekunde zu erwarten, wenn der zweite Parameter eine Liste von Hunderten oder Tausenden von Werten enthält. Für die Anzahl von Elementen, die Sie an `search.in` übergeben können, gibt es keinen expliziten Grenzwert. Sie müssen jedoch nach wie vor die maximale Anforderungsgröße einhalten. Die Wartezeit nimmt jedoch mit zunehmender Anzahl von Werten zu.

-   Die Funktion `search.ismatch` wertet die Suchabfrage als Teil eines Filterausdrucks aus. Die mit der Suchabfrage übereinstimmenden Dokumente werden im Resultset zurückgegeben. Die folgenden Überladungen dieser Funktion sind verfügbar:
    - `search.ismatch(search)`
    - `search.ismatch(search, searchFields)`
    - `search.ismatch(search, searchFields, queryType, searchMode)`

    Hierbei gilt: 
  
    - `search`: Die Suchabfrage (in der [einfachen](query-simple-syntax.md) oder [vollständigen](query-lucene-syntax.md) Abfragesyntax). 
    - `queryType`: „simple“ oder „full“. Der Standardwert ist „simple“. Gibt an, welche Abfragesprache im `search`-Parameter verwendet wurde.
    - `searchFields`: Durch Trennzeichen getrennte Liste von durchsuchbaren Feldern, in denen gesucht werden soll. Standardmäßig werden alle durchsuchbaren Felder im Index durchsucht.    
    - `searchMode`: „any“ oder „all“. Der Standardwert ist „any“. Gibt an, ob für beliebige oder alle der Suchbegriffe Übereinstimmungen gefunden werden müssen, damit das Dokument in den Ergebnissen zurückgegeben wird.

    Alle der oben aufgeführten Parameter sind gleichwertig mit den entsprechenden [Suchanforderungsparametern](https://docs.microsoft.com/rest/api/searchservice/search-documents).

-   Die Funktion `search.ismatchscoring` gibt ebenso wie die Funktion `search.ismatch` TRUE für Dokumente zurück, die mit der als Parameter übergebenen Suchabfrage übereinstimmen. Der Unterschied zwischen den beiden Funktionen besteht darin, dass die Relevanzbewertung der Dokumente, die mit der Abfrage `search.ismatchscoring` übereinstimmen, in die Gesamtbewertung des Dokuments einfließt, während sich die Dokumentbewertung im Fall von `search.ismatch` nicht ändert. Die folgenden Überladungen dieser Funktion sind verfügbar (mit den gleichen Parametern wie bei `search.ismatch`):
    - `search.ismatchscoring(search)`
    - `search.ismatchscoring(search, searchFields)`
    - `search.ismatchscoring(search, searchFields, queryType, searchMode)`

  Die Funktionen `search.ismatch` und `search.ismatchscoring` sind vollständig orthogonal zueinander und zur restlichen Filteralgebra. Dies bedeutet, dass beide Funktionen im selben Filterausdruck verwendet werden können. 

### <a name="geospatial-queries-and-polygons-spanning-the-180th-meridian"></a>Räumliche Abfragen und Polygone, die den 180. Längengrad umfassen  
 Bei vielen Bibliotheken für räumliche Abfragen ist die Erstellung einer Abfrage, die den 180. Längengrad (nahe der Datumsgrenze) beinhaltet, entweder nicht oder nur mit einer Behelfslösung möglich (z. B. durch Aufteilung des Polygons in zwei Teile auf jeweils einer Seite des Längengrads).  

 In Azure Search funktionieren räumliche Abfragen mit dem 180. Längengrad wie erwartet, wenn die Form der Abfrage rechteckig ist und die Koordinaten von Längen- und Breitengrad an einem Rasterlayout ausgerichtet sind (z. B. `geo.intersects(location, geography'POLYGON((179 65,179 66,-179 66,-179 65,179 65))'`). Erwägen Sie andernfalls für nicht rechteckige oder nicht ausgerichtete Formen die Aufteilung des Polygons.  

<a name="bkmk_limits"></a>

## <a name="filter-size-limitations"></a>Einschränkungen der Filtergröße 

 Für die Größe und Komplexität der Filterausdrücke, die Sie an Azure Search senden können, gelten Grenzwerte. Die Grenzwerte basieren ungefähr auf der Anzahl von Klauseln in Ihrem Filterausdruck. Gut merken lässt sich, dass Sie bei Hunderten von Klauseln Gefahr laufen, an den Grenzwert zu erreichen. Wir empfehlen Ihnen, Ihre Anwendung so zu entwerfen, dass sie keine Filter unbegrenzter Größe erzeugt.  


## <a name="filter-examples"></a>Beispiele für Filter  

 Suche nach allen Hotels mit einem Basistarif unter 100 $ mit einer Bewertung von mindestens 4:  

```  
$filter=baseRate lt 100.0 and rating ge 4  
```  

 Suche nach allen Hotels außer „Roach Motel“, die seit 2010 renoviert wurden:  

```  
$filter=hotelName ne 'Roach Motel' and lastRenovationDate ge 2010-01-01T00:00:00Z  
```  

 Suche nach allen Hotels mit einem Basistarif unter 200 $, die seit 2012 renoviert wurden und einen datetime-Literalwert mit Zeitzoneninformationen für Pacific Normalzeit aufweisen:  

```  
$filter=baseRate lt 200 and lastRenovationDate ge 2012-01-01T00:00:00-08:00  
```  

 Suche nach allen Hotels, bei denen die Parkgebühren im Preis enthalten sind und Rauchen nicht erlaubt ist:  

```  
$filter=parkingIncluded and not smokingAllowed  
```  

 \- ODER -  

```  
$filter=parkingIncluded eq true and smokingAllowed eq false  
```  

 Suche nach allen Hotels, die der Kategorie „Luxury“ angehören oder bei denen die Parkgebühren im Preis enthalten sind, mit einer Bewertung von 5:  

```  
$filter=(category eq 'Luxury' or parkingIncluded eq true) and rating eq 5  
```  

 Suche nach allen Hotels mit dem Tag „wifi“ (wobei für jedes Hotel Tags in einem Feld „Collection(EDM.String)“ gespeichert sind):  

```  
$filter=tags/any(t: t eq 'wifi')  
```  

 Suche nach allen Hotels ohne das Tag „motel“:  

```  
$filter=tags/all(t: t ne 'motel')  
```  

 Suche nach allen Hotels mit beliebigen Tags:  

```  
$filter=tags/any()  
```  

 Suche nach allen Hotels in einem Umkreis von 10 Kilometern von einem angegebenen Referenzpunkt (wobei „location“ ein Feld vom Typ „Edm.geographypoint“ ist):  

```  
$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10  
```  

 Suche nach allen Hotels in einem bestimmten Anzeigebereich, der als Polygon definiert ist (wobei „location“ ein Feld vom Typ „Edm.geographypoint“ ist). Beachten Sie, dass das Polygon geschlossen ist (der erste und der letzte Punkt müssen identisch sein), und [die Punkte müssen entgegen dem Uhrzeigersinn aufgelistet sein](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

```  
$filter=geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')  
```  

 Suche nach allen Hotels, bei denen das Feld „description“ keinen Wert enthält oder explizit auf NULL festgelegt ist:  

```  
$filter=description eq null  
```  

Suche nach allen Hotels, deren Namen „Roach motel“ oder „Budget hotel“ entspricht:  

```  
$filter=search.in(name, 'Roach motel,Budget hotel', ',') 
```

Suche nach allen Hotels, deren Namen „Roach motel“ oder „Budget hotel“ entspricht (getrennt durch „|“):  

```  
$filter=search.in(name, 'Roach motel|Budget hotel', '|') 
```

Suche nach allen Hotels mit dem Tag „wifi“ oder „pool“:  

```  
$filter=tags/any(t: search.in(t, 'wifi, pool'))  
```

Suche nach allen Hotels ohne das Tag „motel“ oder „cabin“:  

```  
$filter=tags/all(t: not search.in(t, 'motel, cabin'))  
```  

Suche nach Dokumenten mit dem Wort „waterfront“. Diese Filterabfrage ist identisch mit einer [Suchanforderung](https://docs.microsoft.com/rest/api/searchservice/search-documents) mit `search=waterfront`.

```
$filter=search.ismatchscoring('waterfront')
```

Suche nach Dokumenten mit dem Wort „hostel“ und einer Bewertung größer oder gleich 4 oder Dokumenten mit dem Wort „motel“ und einer Bewertung von 5. Beachten Sie, dass diese Anforderung nicht ohne die Funktion `search.ismatchscoring` ausgedrückt werden kann.

```
$filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5
```

Suche nach Dokumenten ohne das Wort „luxury“:

```
$filter=not search.ismatch('luxury') 
```

Suche nach Dokumenten mit dem Ausdruck „ocean view“ oder einer Bewertung von 5. Die Abfrage `search.ismatchscoring` wird nur für die Felder `hotelName` und `description` ausgeführt.
Beachten Sie, dass Dokumente, die nur der zweiten Klausel der Disjunktion entsprechen, ebenfalls zurückgegeben werden (Hotels mit einer Bewertung von 5). Um deutlich zu machen, dass diese Dokumente mit keinem der bewerteten Teile des Ausdrucks übereinstimmen, werden sie mit einem Ergebnis von Null zurückgegeben.

```
$filter=search.ismatchscoring('"ocean view"', 'description,hotelName') or rating eq 5
```

Suche nach Dokumenten, die die Begriffe „hotel“ und „airport“ in einem Abstand von fünf Wörtern voneinander in der Beschreibung des Hotels sowie ein Rauchverbot enthalten. Diese Abfrage verwendet die [vollständige Lucene-Abfragesprache](query-lucene-syntax.md).

```
$filter=search.ismatch('"hotel airport"~5', 'description', 'full', 'any') and not smokingAllowed 
```

## <a name="order-by-syntax"></a>Sortiersyntax

Der **$orderby**-Parameter akzeptiert eine durch Trennzeichen getrennte Liste von bis zu 32 Ausdrücken im Format `sort-criteria [asc|desc]`. Bei den Sortierkriterien kann es sich entweder um den Namen eines Felds vom Typ `sortable` oder um einen Aufruf der Funktion `geo.distance` oder `search.score` handeln. Sie können `asc` oder `desc` verwenden, um die Sortierreihenfolge explizit anzugeben. Die Standardreihenfolge ist aufsteigend.

Wenn für mehrere Dokumente die gleichen Sortierkriterien gelten und die Funktion `search.score` nicht verwendet wird (z. B. wenn Sie nach einem numerischen Feld `rating` sortieren und drei Dokumente eine Bewertung von 4 aufweisen), werden Verknüpfungen auf Grundlage des Dokumentergebnisses in absteigender Reihenfolge getrennt. Wenn Dokumentergebnisse identisch sind (z. B. wenn in der Anforderung eine Nicht-Volltextsuchabfrage angegeben ist), ist die relative Sortierung der verknüpften Dokumente unbestimmt.
 
Sie können mehrere Sortierkriterien angeben. Die Reihenfolge der Ausdrücke bestimmt die endgültige Sortierreihenfolge. Die Syntax zum Sortieren in absteigender Reihenfolge nach Suchergebnis und anschließend nach Bewertung lautet beispielsweise wie folgt: `$orderby=search.score() desc,rating desc`.

Die Syntax für `geo.distance` in **$orderby** entspricht der Syntax in **$filter**. Bei der Verwendung von `geo.distance` in **$orderby** muss das Feld, für das der Ausdruck gilt, vom Typ `Edm.GeographyPoint` und zudem `sortable` sein.  

Die Syntax für `search.score` in **$orderby** ist `search.score()`. Die Funktion `search.score` akzeptiert keine Parameter.  
 

## <a name="order-by-examples"></a>Beispiele für das Sortieren

Sortieren von Hotels in aufsteigender Reihenfolge nach Basistarif:

```
$orderby=baseRate asc
```

Sortieren von Hotels in absteigender Reihenfolge nach Bewertung und anschließend in aufsteigender Reihenfolge nach Basistarif (bedenken Sie, dass die Standardreihenfolge aufsteigend ist):

```
$orderby=rating desc,baseRate
```

Sortieren von Hotels in absteigender Reihenfolge nach Bewertung und anschließend in aufsteigender Reihenfolge nach der Entfernung von den angegebenen Koordinaten:

```
$orderby=rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```

Sortieren von Hotels in absteigender Reihenfolge nach Suchergebnis und Bewertung und anschließend in aufsteigender Reihenfolge nach der Entfernung von den angegebenen Koordinaten, sodass von zwei Hotels mit identischen Bewertungen das nächstgelegene zuerst aufgeführt wird:

```
$orderby=search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```
<a name="bkmk_unsupported"></a>

## <a name="unsupported-odata-syntax"></a>Nicht unterstützte OData-Syntax

-   Arithmetische Ausdrücke  

-   Funktionen (mit Ausnahme der Geofunktionen für Entfernung und Überschneidungen)  

-   `any/all` mit beliebigen Lambdaausdrücken  

## <a name="see-also"></a>Weitere Informationen  

+ [Facettennavigation in Azure Search](search-faceted-navigation.md) 
+ [Filter in Azure Search](search-filters.md) 
+ [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Suchen nach Dokumenten: REST-API für den Azure Search-Dienst) 
+ [Lucene-Abfragesyntax](query-lucene-syntax.md)
+ [Simple query syntax in Azure Search](query-simple-syntax.md) (Einfache Abfragesyntax in Azure Search)   
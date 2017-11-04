---
title: Facettenfilter in Azure Search | Microsoft-Dokumentation
description: Filtern Sie nach Kriterien wie Benutzersicherheits-ID, Sprache, geografischem Standort oder numerischen Werten, um Suchergebnisse bei Abfragen in Azure Search (in Microsoft Azure gehosteter Cloudsuchdienst) einzugrenzen.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/13/2017
ms.author: heidist
ms.openlocfilehash: 5b4d88cb9c9662fe45de8c11534232a2905cf5a4
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-build-a-facet-filter-in-azure-search"></a>Erstellen eines Facettenfilters in Azure Search 

Die Facettennavigation wird zum selbstgesteuerten Filtern von Abfrageergebnissen in einer Such-App verwendet, in der Ihre Anwendung Benutzeroberflächen-Steuerelemente zum Begrenzen der Suche auf bestimmte Gruppen von Dokumenten enthält (z.B. Kategorien oder Marken). Azure Search stellt hierfür die zugrunde liegende Datenstruktur bereit. Dieser Artikel enthält eine schnelle Einführung in die grundlegenden Schritte zum Erstellen einer Struktur für die Facettennavigation, die als Grundlage für die bereitzustellende Suchbenutzeroberfläche dient. 

> [!div class="checklist"]
> * Auswählen von Feldern zum Filtern und Durchführen von Faceting
> * Festlegen von Attributen für das Feld
> * Erstellen des Index und Laden von Daten
> * Hinzufügen von Facettenfiltern zu einer Abfrage
> * Verarbeiten von Ergebnissen

Facets sind dynamisch und werden für eine Abfrage zurückgegeben. Suchantworten verfügen über die Facetkategorien, die zum Navigieren durch die Ergebnisse verwendet werden. Wenn Sie mit Facets nicht vertraut sind, hilft Ihnen die Darstellung einer Struktur für die Facetnavigation im folgenden Beispiel weiter.

  ![](./media/search-filters/facet-nav.png)

Benötigen Sie ausführlichere Informationen zur Facetnavigation? Sie finden diese Informationen unter [Implementieren der Facettennavigation in Azure Search](search-faceted-navigation.md).

## <a name="choose-fields"></a>Auswählen von Feldern

Facets können über Felder mit einzelnen Werten und auch über Sammlungen berechnet werden. Felder, die für die Facetnavigation am besten funktionieren, weisen eine niedrige Kardinalität auf: eine geringe Anzahl von unterschiedlichen Werten, die sich in den Dokumenten Ihres Suchkorpus wiederholen (z.B. eine Liste mit Farben, Ländern oder Markennamen). 

Das Faceting wird pro Feld aktiviert, wenn Sie den Index erstellen, indem Sie die folgenden Attribute auf TRUE festlegen: `filterable`, `facetable`. Nur filterbare Felder können facettiert werden.

Alle [Feldtypen](https://docs.microsoft.com/rest/api/searchservice/supported-data-types), die für die Facetnavigation verwendet werden können, sind als „facetable“ (facettierbar) gekennzeichnet:

+ Edm.String
+ Edm.DateTimeOffset
+ Edm.Boolean
+ Edm.Collections (siehe [Facettieren von komplexen Datentypen](#facet-complex-fields) weiter unten in diesem Artikel)
+ Numerische Feldtypen: Edm.Int32, Edm.Int64, Edm.Double

Es ist nicht möglich, Edm.GeographyPoint für die Facettennavigation zu verwenden. Facets werden aus für Menschen lesbarem Text oder Zahlen erstellt. Daher werden Facets für Geokoordinaten nicht unterstützt. Sie benötigen ein Feld für einen Ort oder eine Region, um das Facettieren nach dem Standort durchzuführen.

## <a name="set-attributes"></a>Festlegen von Attributen

Indexattribute, mit denen gesteuert wird, wie ein Feld verwendet wird, werden einzelnen Felddefinitionen im Index hinzugefügt. Im folgenden Beispiel bestehen Felder mit geringer Kardinalität, die für das Faceting nützlich sind, aus folgenden Komponenten: Kategorie (hotel, motel, hostel), Annehmlichkeiten und Bewertungen. 

In der .NET-API müssen Filterattribute explizit festgelegt werden. In der REST-API sind das Faceting und die Filterung standardmäßig aktiviert. Dies bedeutet, dass Sie die Attribute nur dann explizit festlegen müssen, wenn Sie sie deaktivieren möchten. Es ist zwar technisch nicht erforderlich, aber die Attributzuordnungen sind im folgenden REST-Beispiel zu Lernzwecken angegeben. 

> [!Tip]
> Die bewährte Methode zur Leistungs- und Speicheroptimierung besteht darin, das Faceting für Felder zu deaktivieren, die niemals als Facet genutzt werden sollten. Insbesondere Zeichenfolgenfelder für Singleton-Werte (wie etwa eine ID oder ein Produktname) sollten auf „Facetable: false“ festgelegt werden, damit sie nicht versehentlich für die Facettennavigation verwendet werden.


```http
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String", "filterable": true, "facetable": true},
        {"name": "tags", "type": "Collection(Edm.String)", "filterable": true, "facetable": true},
        {"name": "parkingIncluded", "type": "Edm.Boolean",  "filterable": true, "facetable": true, "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true, "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32", "filterable": true, "facetable": true},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

> [!Note]
> Diese Indexdefinition wurde aus [Erstellen eines Azure Search-Indexes mit der REST-API](https://docs.microsoft.com/azure/search/search-create-index-rest-api) kopiert. Sie ist bis auf einige vernachlässigbare Unterschiede bei den Felddefinitionen identisch. Filterbare und facettierbare Attribute werden für die Felder „category“, „tags“, „parkingIncluded“, „smokingAllowed“ und „rating“ explizit hinzugefügt. In der Praxis erhalten Sie die Filterbarkeit und Facettierbarkeit standardmäßig für die Typen „Edm.String“, „Edm.Boolean“ und „Edm.Int32“. 

## <a name="build-and-load-an-index"></a>Erstellen und Laden eines Index

Ein (wahrscheinlich naheliegender) Zwischenschritt vor der Erstellung einer Abfrage ist das [Erstellen und Auffüllen des Index](https://docs.microsoft.com/azure/search/search-create-index-dotnet#create-the-index). Er wird daher nur der Vollständigkeit halber erwähnt. Die Verfügbarkeit des Index kann unter anderem durch das Überprüfen der Indexliste im [Portal](https://portal.azure.com) ermittelt werden.

## <a name="add-facet-filters-to-a-query"></a>Hinzufügen von Facettenfiltern zu einer Abfrage

Erstellen Sie im Anwendungscode eine Abfrage, mit der alle Teile einer gültigen Abfrage angegeben werden, z.B. Suchausdrücke, Facets, Filter, Bewertungsprofile – alle Elemente, die zum Formulieren einer Anforderung verwendet werden. Im folgenden Beispiel wird eine Anforderung erstellt, bei der die Facettennavigation basierend auf der Art der Unterbringung, der Bewertung und anderen Annehmlichkeiten erstellt wird.

```csharp
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "category", "rating", "parkingIncluded", "smokingAllowed" },
};
```

### <a name="return-filtered-results-on-click-events"></a>Zurückgeben von gefilterten Ergebnissen bei Klickereignissen

Der Filterausdruck behandelt das Klickereignis auf den Facettenwert. Beim Facet „Category“ wird das Klicken auf die Kategorie „motel“ über den Ausdruck `$filter` implementiert, mit dem Unterbringungsmöglichkeiten dieser Art ausgewählt werden. Wenn ein Benutzer auf „motels“ klickt, um anzugeben, dass nur Motels angezeigt werden sollen, enthält die nächste von der Anwendung gesendete Abfrage „$filter=category eq ‘motels’“.

Mit dem folgenden Codeausschnitt wird die Kategorie dem Filter hinzugefügt, wenn ein Benutzer einen Wert aus dem Facet „category“ auswählt.

```csharp
if (categoryFacet != "")
  filter = "category eq '" + categoryFacet + "'";
```
Bei Verwendung der REST-API würde die Anforderung wie folgt formuliert werden: `$filter=category eq 'c1'`. Verwenden Sie die folgende Syntax, um „category“ zu einem Feld mit mehreren Werten zu machen: `$filter=category/any(c: c eq 'c1')`

## <a name="tips-and-workarounds"></a>Tipps und Problemumgehungen

### <a name="initialize-a-page-with-facets-in-place"></a>Initialisieren einer Seite mit vorhandenen Facets

Wenn Sie eine Seite mit vorhandenen Facets initialisieren möchten, können Sie eine Abfrage als Teil der Seiteninitialisierung senden, um für die Seite das Seeding mit einer Facet-Anfangsstruktur durchzuführen.

### <a name="preserve-a-facet-navigation-structure-asynchronously-of-filtered-results"></a>Beibehalten einer Struktur für die Facetnavigation asynchron zu gefilterten Ergebnissen

Eine der Herausforderungen bei der Facetnavigation in Azure Search besteht darin, dass Facets nur für aktuelle Ergebnisse vorhanden sind. In der Praxis ist die Beibehaltung einer statischen Gruppe von Facets üblich, damit Benutzer die Navigation in umgekehrter Richtung durchführen und die Schritte nachvollziehen können, um andere Wege durch die Suchinhalte zu erkunden. 

Dies ist zwar ein häufiger Anwendungsfall, aber dieser Vorgang ist noch nicht standardmäßig im Lieferumfang der Struktur für die Facettennavigation enthalten. Entwickler, die statische Facets benötigen, umgehen diese Einschränkung normalerweise, indem sie zwei gefilterte Abfragen durchführen: eine für die Ergebnisse und die andere zum Erstellen einer statischen Liste mit Facets zu Navigationszwecken.

## <a name="see-also"></a>Weitere Informationen

+ [Filter in Azure Search](search-filters.md)
+ [Create Index (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Create Index (Azure Search-Dienst-REST-API))
+ [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Suchen nach Dokumenten (Azure Search Service-REST-API))


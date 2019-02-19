---
title: Erstellen der Indexdefinition und Konzepte – Azure Search
description: Einführung in Indexbegriffe und Konzepte in Azure Search, einschließlich Komponenten und physischer Struktur.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/01/2019
ms.custom: seodec2018
ms.openlocfilehash: 77f4b597ad4b87db7e720dd57191c6b192a4c93b
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000949"
---
# <a name="create-a-basic-index-in-azure-search"></a>Erstellen eines grundlegenden Index in Azure Search

In Azure Search ist ein *Index* ein dauerhafter Speicher von *Dokumenten* und anderen Konstrukten, die von einem Azure Search-Dienst für die gefilterte und Volltextsuche verwendet werden. Konzeptionell ist ein Dokument eine einzelne Einheit mit durchsuchbaren Daten im Index. Ein Internetversandhändler verfügt beispielsweise über ein Dokument für jeden angebotenen Artikel, eine Nachrichtenagentur über ein Dokument pro Zeitungsartikel usw. So lassen sich diese Konzepte vertrauteren Entsprechungen in der Datenbank zuordnen: Ein *Index* entspricht etwa einer *Tabelle*, und *Dokumente* entsprechen ungefähr den *Zeilen* einer Tabelle.

Wenn Sie einen Index hinzufügen oder hochladen, erstellt Azure Search physische Strukturen basierend auf dem von Ihnen bereitgestellten Schema. Wenn beispielsweise ein Feld in Ihrem Index als durchsuchbar markiert ist, wird ein invertierter Index für dieses Feld erstellt. Wenn Sie später Dokumente hinzufügen oder hochladen oder Suchabfragen an Azure Search übermitteln, senden Sie Anforderungen an einen bestimmten Index in Ihrem Suchdienst. Das Laden von Feldern mit Dokumentwerten wird als *Indizierung* oder Datenerfassung bezeichnet.

Sie können einen Index im Portal, in der [REST-API](search-create-index-rest-api.md) oder im [.NET SDK](search-create-index-dotnet.md) erstellen.

## <a name="components-of-an-index"></a>Komponenten eines Index

Schematisch besteht ein Azure Search-Index aus den folgenden Elementen. 

Die [*Feldsammlung*](#fields-collection) ist typischerweise der größte Teil eines Index, wobei jedes Feld benannt, typisiert und mit zulässigen Verhaltensweisen versehen wird, die bestimmen, wie es verwendet wird. Weitere Elemente sind [Vorschlagsfunktionen](#suggesters), [Bewertungsprofile](#scoring-profiles), [Analysetools](#analyzers) mit Komponenten zur Unterstützung der Anpassung sowie [CORS](#cors)-Optionen.

```json
{  
  "name": (optional on PUT; required on POST) "name_of_index",  
  "fields": [  
    {  
      "name": "name_of_field",  
      "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",  
      "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),  
      "filterable": true (default) | false,  
      "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),  
      "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),  
      "key": true | false (default, only Edm.String fields can be keys),  
      "retrievable": true (default) | false,  
      "analyzer": "name_of_analyzer_for_search_and_indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
      "searchAnalyzer": "name_of_search_analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
      "indexAnalyzer": "name_of_indexing_analyzer", (only if 'searchAnalyzer' is set and 'analyzer' is not set)
      "synonymMaps": [ "name_of_synonym_map" ] (optional, only one synonym map per field is currently supported)
    }  
  ],  
  "suggesters": [  
    {  
      "name": "name of suggester",  
      "searchMode": "analyzingInfixMatching",  
      "sourceFields": ["field1", "field2", ...]  
    }  
  ],  
  "scoringProfiles": [  
    {  
      "name": "name of scoring profile",  
      "text": (optional, only applies to searchable fields) {  
        "weights": {  
          "searchable_field_name": relative_weight_value (positive #'s),  
          ...  
        }  
      },  
      "functions": (optional) [  
        {  
          "type": "magnitude | freshness | distance | tag",  
          "boost": # (positive number used as multiplier for raw score != 1),  
          "fieldName": "...",  
          "interpolation": "constant | linear (default) | quadratic | logarithmic",  
          "magnitude": {  
            "boostingRangeStart": #,  
            "boostingRangeEnd": #,  
            "constantBoostBeyondRange": true | false (default)  
          },  
          "freshness": {  
            "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)  
          },  
          "distance": {  
            "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)  
            "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)  
          },  
          "tag": {  
            "tagsParameter": "..." (parameter to be passed in queries to specify a list of tags to compare against target fields)  
          }  
        }  
      ],  
      "functionAggregation": (optional, applies only when functions are specified)   
        "sum (default) | average | minimum | maximum | firstMatching"  
    }  
  ],  
  "analyzers":(optional)[ ... ],
  "charFilters":(optional)[ ... ],
  "tokenizers":(optional)[ ... ],
  "tokenFilters":(optional)[ ... ],
  "defaultScoringProfile": (optional) "...",  
  "corsOptions": (optional) {  
    "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],  
    "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)  
  }  
}
```

## <a name="fields-collection-and-attribution"></a>Feldsammlung und Zuordnung
Wenn Sie Ihr Schema definieren, müssen Sie den Namen, den Typ und die Attribute jedes Felds in Ihrem Index festlegen. Der Feldtyp klassifiziert die Daten, die in dem Feld gespeichert sind. Attribute werden für einzelne Felder festlegen, um anzugeben, wie das Feld verwendet wird. Die folgende Tabelle listet die Typen und Attribute auf, die Sie angeben können.

### <a name="data-types"></a>Datentypen
| Type | BESCHREIBUNG |
| --- | --- |
| *Edm.String* |Text, der optional für die Volltextsuche (Worttrennung, Wortstammerkennung usw.) mit einem Token versehen werden kann |
| *Collection(Edm.String)* |Eine Liste von Zeichenfolgen, die für die Volltextsuche mit einem Token versehen werden können. Für die Anzahl der Elemente in einer Sammlung gibt es keine Obergrenze, allerdings gilt die Obergrenze für die Größe der Nutzlast von 16 MB auch für Sammlungen. |
| *Edm.Boolean* |Enthält TRUE/FALSE-Werte |
| *Edm.Int32* |32-Bit-Ganzzahlwerte |
| *Edm.Int64* |64-Bit-Ganzzahlwerte |
| *Edm.Double* |Numerische Daten mit doppelter Genauigkeit |
| *Edm.DateTimeOffset* |Datums-/Uhrzeitwerte im OData V4-Format (z. B. `yyyy-MM-ddTHH:mm:ss.fffZ` oder `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`) |
| *Edm.GeographyPoint* |Ein Punkt, der einen weltweiten geografischen Standort darstellt |

Ausführlichere Informationen zu den von Azure Search unterstützten Datentypen finden Sie [hier](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

### <a name="index-attributes"></a>Indexattribute
| Attribut | BESCHREIBUNG |
| --- | --- |
| *Schlüssel* |Eine Zeichenfolge, die die eindeutige ID der einzelnen Dokumente darstellt und für die Dokumentsuche verwendet wird. Jeder Index muss über einen Schlüssel verfügen. Als Schlüssel kann immer nur ein einzelnes Feld fungieren, und sein Typ muss auf „Edm.String“ festgelegt sein. |
| *Abrufbar* |Gibt an, ob ein Feld in einem Suchergebnis zurückgegeben werden kann. |
| *Filterbar* |Ermöglicht die Verwendung des Felds in Filterabfragen. |
| *Sortierbar* |Ermöglicht einer Abfrage das Sortieren von Suchergebnissen mithilfe dieses Felds. |
| *Facettierbar* |Ermöglicht die Verwendung eines Felds in einer [Facettennavigationsstruktur](search-faceted-navigation.md) für benutzerdefiniertes Filtern. Repetitive Werte, mit denen sich mehrere Dokumente zu einer Gruppe zusammenfassen lassen (etwa mehrere Dokumente der gleichen Marken- oder Dienstleistungskategorie), sind in der Regel am besten für die Verwendung als Facetten geeignet. |
| *Durchsuchbar* |Markiert das Feld als in die Volltextsuche einbeziehbar. |

Ausführlichere Informationen zu den Indexattributen von Azure Search finden Sie [hier](https://docs.microsoft.com/rest/api/searchservice/Create-Index).

## <a name="suggesters"></a>Vorschläge
Eine Vorschlagsfunktion ist ein Abschnitt des Schemas, das definiert, welche Felder in einem Index verwendet werden, um AutoVervollständigen oder Eingabevorschläge für Abfragen bei Suchvorgängen zu unterstützen. In der Regel werden während der Eingabe einer Suchabfrage Teile von Suchzeichenfolgen an die Vorschlags-API (Azure Search-Dienst-REST-API) gesendet. Diese gibt daraufhin eine Reihe von Vorschlägen zurück. Wenn Sie im Index einen Vorschlag definieren, bestimmt dieser, welche Felder für Eingabevorschläge von Suchbegriffen verwendet werden. Weitere Informationen finden Sie unter [Hinzufügen von Vorschlagsfunktionen](index-add-suggesters.md).

## <a name="scoring-profiles"></a>Bewertungsprofile

Ein Bewertungsprofil ist ein Abschnitt des Schemas, das benutzerdefinierte Bewertungsverhalten definiert, mit denen Sie beeinflussen können, welche Elemente weiter oben in den Suchergebnissen angezeigt werden. Bewertungsprofile bestehen aus Feldgewichtungen und Funktionen. Um sie verwenden zu können, fügen Sie in die Abfragezeichenfolge den Profilnamen ein.

Ein standardmäßiges Bewertungsprofil berechnet im Hintergrund für jedes Element eines Ergebnissatzes eine Suchbewertung. Sie können das interne, unbenannte Bewertungsprofil verwenden. Oder legen Sie für „defaultScoringProfile“ ein benutzerdefiniertes Profil fest, das standardmäßig aufgerufen wird, wenn für eine Abfragezeichenfolge kein benutzerdefiniertes Profil angegeben wurde.

Weitere Informationen finden Sie unter [Hinzufügen von Bewertungsprofilen](index-add-scoring-profiles.md).

## <a name="analyzers"></a>Analysemodule

Das Analysetoolelement legt den Namen des Sprachanalysetools fest, das für das Feld verwendet werden soll. Die zulässigen Werte finden Sie unter [Sprachanalysetools in Azure Search](index-add-language-analyzers.md). Diese Option kann nur mit durchsuchbaren Feldern verwendet werden und weder zusammen mit **searchAnalyzer** noch mit **indexAnalyzer** festgelegt werden. Eine einmal für ein Feld gewählte Analysemethode kann nicht mehr geändert werden.

## <a name="cors"></a>CORS

Clientseitiger JavaScript-Code kann standardmäßig keine APIs aufrufen, da der Browser jegliche ursprungsübergreifenden Anforderungen verhindert. Um ursprungsübergreifende Abfragen für Ihren Index zu ermöglichen, aktivieren Sie CORS (Cross-Origin Resource Sharing), indem Sie das Attribut **corsOptions** festlegen. Aus Sicherheitsgründen wird CORS nur von Abfrage-APIs unterstützt. 

Die folgenden Optionen können für CORS festgelegt werden:

+ **allowedOrigins** (erforderlich): Dies ist eine Liste der Ursprünge, denen Zugriff auf Ihren Index gewährt wird. Dies bedeutet, dass jeglicher von diesen Ursprüngen bereitgestellte Javascript-Code Indexabfragen durchführen kann (sofern er den richtigen API-Schlüssel angibt). Ursprünge werden in der Regel im Format `protocol://<fully-qualified-domain-name>:<port>` angegeben, wobei `<port>` häufig weggelassen wird. Weitere Informationen finden Sie unter [Cross-Origin Resource Sharing, CORS (Wikipedia)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

  Wenn Sie den Zugriff auf alle Ursprünge zulassen möchten, beziehen Sie `*` als ein einzelnes Element in das Array **allowedOrigins** ein. *Dies ist keine empfohlene Vorgehensweise für Produktionssuchdienste*, ist für Entwicklung und Debugging jedoch oft nützlich.

+ **maxAgeInSeconds** (optional): Von Browsern wird dieser Wert verwendet, um die Dauer (in Sekunden) des Zwischenspeicherns von CORS-Preflight-Antworten zu ermitteln. Dies muss eine positive ganze Zahl sein. Mit dem Wert steigt auch die Leistung, aber es dauert auch länger, bis CORS-Richtlinienänderungen in Kraft treten. Wenn diese Einstellung nicht festgelegt ist, gilt die Standarddauer von 5 Minuten.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie mit den Grundlagen der Indexerstellung vertraut sind, können Sie im Portal fortfahren, Ihren ersten Index zu erstellen.

> [!div class="nextstepaction"]
> [Hinzufügen eines Index (Portal)](search-create-index-portal.md)
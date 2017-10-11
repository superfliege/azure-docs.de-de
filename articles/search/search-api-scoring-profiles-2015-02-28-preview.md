---
title: Bewertungsprofile (Azure Search REST-API-Version 2015-02-28-Preview) | Microsoft Docs
description: "Azure Search ist eine gehostete Cloud-Search-Dienst, die Optimierung der Ergebnisse nach Rang, die Bewertungsprofile benutzerdefinierten Grundlage unterstützt."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: bfd62649-13d7-40b3-a8fa-85521a15084d
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.author: heidist
ms.date: 10/27/2016
ms.openlocfilehash: a67637d149a84313270c03d21acf8a9c1870be05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Bewertungsprofile (Azure-Suchdienst-REST-API-Version 2015-02-28-Preview)
> [!NOTE]
> Dieser Artikel beschreibt, die Bewertungsprofile in der [2015-02-28-Preview](search-api-2015-02-28-preview.md). Derzeit besteht kein Unterschied zwischen der `2016-09-01` Version dokumentiert auf [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) und `2015-02-28-Preview` Version hier beschriebenen, jedoch empfehlen wir dieses Dokument trotzdem um Dokument Abdeckung über die gesamte-API bereit.
>
>

## <a name="overview"></a>Übersicht
Bewertung bezieht sich auf die Berechnung eine suchbewertung für jedes Element in den Suchergebnissen zurückgegeben. Das Ergebnis ist ein Indikator für die Relevanz eines Elements im Kontext des aktuellen Suchvorgangs. Je höher die Bewertung, desto relevanter des Elements. Elemente sind in den Suchergebnissen absteigende Rangfolge von hoch zu niedrig, basierend auf der suchbewertung für jedes Element berechnet.

Azure Search verwendet die standardbewertung zum Berechnen einer anfänglichen Bewertung, aber Sie können die Berechnung über ein Bewertungsprofil anpassen. Bewertungsprofile bieten Ihnen mehr Kontrolle über die Rangfolge der Elemente in den Suchergebnissen. Angenommen, möchten Sie Elemente auf Grundlage ihres umsatzpotenzials zu verstärken, neuere Elemente hochzustufen oder vielleicht Elemente, die im Lager zu lang sein.

Ein Bewertungsprofil ist Teil der Indexdefinition besteht aus Feldern, Funktionen und Parameter.

Das folgende Beispiel zeigt ein einfaches Profil mit der Bezeichnung "Geo", um Sie geben einen Überblick darüber, wie ein Bewertungsprofil aussieht. Diese werden Elemente verstärkt, die den Suchbegriff in das `hotelName` Feld. Darüber hinaus verwendet der `distance` Funktion, um Elemente zu bevorzugen, die sich innerhalb von zehn Kilometern von der aktuellen Position befinden. Wenn jemand nach dem Begriff "Inn sucht" und "Inn" Teil des Namens Hotel sein, werden Dokumente, die Hotels mit "Inn" enthalten weiter oben in den Suchergebnissen angezeigt.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Um dieses Bewertungsprofil zu verwenden, wird Ihre Abfrage so formuliert, um das Profil in der Abfragezeichenfolge anzugeben. Beachten Sie in der folgenden Abfrage den Abfrageparameter `scoringProfile=geo` in der Anforderung.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Diese Abfrage sucht nach dem Begriff "Inn" und übergibt die im aktuellen Verzeichnis. Beachten Sie, dass diese Abfrage den anderen Parametern, wie z. B. enthält `scoringParameter`. Abfrageparameter werden in beschrieben [Dokumente durchsuchen (Azure Search-API)](search-api-2015-02-28-preview.md#SearchDocs).

Klicken Sie auf [Beispiel](#example) um ein ausführlicheres Beispiel für ein Bewertungsprofil zu überprüfen.

## <a name="what-is-default-scoring"></a>Was ist standardbewertung?
Bewertung wird eine suchbewertung für jedes Element in einer Rangfolge sortierten Resultset berechnet. Jedes Element in einem Resultset wird einer suchbewertung zugeordnet und anschließend höchsten zur niedrigsten Rangfolge. Elemente mit höheren Bewertungen werden an die Anwendung zurückgegeben. Standardmäßig werden die ersten 50 zurückgegeben, aber Sie können die `$top` Parameter, um eine kleinere oder größere Anzahl von Elementen (bis zu 1000 in einer einzelnen Antwort) zurückzugeben.

Standardmäßig wird eine suchbewertung basierend auf der statistischen Eigenschaften der Daten und der Abfrage berechnet. Azure Search findet Dokumente, die die Suchbegriffe in der Abfragezeichenfolge enthalten (einige oder alle, je nach `searchMode`), vorzugsweise Dokumente, die viele Instanzen des Suchbegriffs enthalten. Die suchbewertung steigt sogar noch höher sein, wenn der Begriff im datenhauptteil, aber innerhalb des Dokuments häufig nur selten auftritt. Die Grundlage für diesen Ansatz zur Berechnung der Relevanz wird als TF-IDF bezeichnet oder (Term Frequency-Inverse dokumenthäufigkeit).

Sofern es keine benutzerdefinierten Sortieren, werden Ergebnisse dann nach suchbewertung sortiert, bevor sie an die aufrufende Anwendung zurückgegeben werden. Wenn `$top` nicht angegeben wird, 50 Elemente mit der höchsten Suche Ergebnis zurückgegeben.

Werte der suchbewertung können in einem Resultset wiederholt werden. Angenommen, Sie müssen möglicherweise 10 Elemente mit einer Bewertung von 1,2, 20 Elemente mit einer Bewertung von 1,0 sowie 20 Elemente mit einer Bewertung von 0,5. Wenn mehrere Treffer dieselbe suchbewertung aufweisen, die Reihenfolge von Elementen mit gleicher ist nicht definiert und nicht stabil ist. Führen Sie die Abfrage erneut aus, und Sie wird möglicherweise Elemente Position wechseln. Wenn zwei Elemente mit identischer Bewertung, ist nicht gewährleistet, welches Element zuerst angezeigt wird.

## <a name="when-to-use-custom-scoring"></a>Beim Verwenden der benutzerdefinierten Bewertung
Sie sollten eine oder mehrere Bewertungsprofile erstellen, wenn das standardmäßige rangfolgeverhalten geht nicht weit genug in erfüllen Ihrer geschäftlichen Ziele. Sie könnten z. B., dass der Suchrelevanz neu hinzugefügte Elemente bevorzugt sollte. Ebenso möglicherweise Sie ein Feld, die Gewinnspanne enthält, oder ein anderes Feld Umsatzpotenzial angibt. Die Verstärkung von Treffern, die von Vorteil, kann Ihr Unternehmen sein kann ein wichtiger Faktor bei der Entscheidung zur Verwendung von bewertungsprofilen sein.

Relevanz basierende Sortierung wird ebenfalls über Bewertungsprofile implementiert. Betrachten Sie Suchergebnisseiten, die Sie in der Vergangenheit verwendet haben, mit die Sie nach Preis, Datum, Bewertung oder Relevanz sortieren können. In Azure Search steuern Bewertungsprofile die Option "Relevanz". Die Definition der Relevanz wird von Ihnen gesteuert und basiert auf Geschäftszielen und dem Typ Suchfunktionalität, die Sie übermitteln möchten.

<a name="example"></a>

## <a name="example"></a>Beispiel
Wie bereits erwähnt, wird benutzerdefinierte Bewertung über in einem IndexSchema definiert Bewertungsprofile implementiert.

Dieses Beispiel zeigt das Schema eines Indexes mit zwei bewertungsprofilen (`boostGenre`, `newAndHighlyRated`). Legen Sie die Abfrage für diesen Index, der ein der Profile enthält, wie ein Abfrageparameter der Profil zum Bewerten des Resultsets verwendet wird.

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Workflow
Um benutzerdefinierte bewertungsverhalten zu implementieren, fügen Sie ein Bewertungsprofil auf das Schema, das den Index definiert. Kann Ihnen bis zu 16, die Bewertungsprofile innerhalb eines Indexes (finden Sie unter [Diensteinschränkungen](search-limits-quotas-capacity.md)), aber Sie können nur ein Profil zum Zeitpunkt in einer bestimmten Abfrage angeben.

Beginnen Sie mit der [Vorlage](#bkmk_template) in diesem Thema bereitgestellt.

Geben Sie einen Namen ein. Bewertungsprofile sind optional, aber wenn Sie eine hinzufügen, der Name ist erforderlich. Achten Sie darauf, dass Sie die Benennungskonventionen für Felder folgen (mit einem Buchstaben beginnen, wird vermieden, Sonderzeichen und reservierte Wörter). Finden Sie unter [Benennungsregeln für](http://msdn.microsoft.com/library/azure/dn857353.aspx) für Weitere Informationen.

Der Hauptteil des Bewertungsprofils wird aus gewichteten Feldern und Funktionen erstellt.

### <a name="weights"></a>Gewichtungen
Die `weights` Eigenschaft ein Bewertungsprofil gibt die Name-Wert-Paare, die eine relative Gewichtung ein Feld zuweisen. In der [Beispiel](#example), sind die Felder "albumtitle", "Genre" und ArtistName verstärkten 1.5, 5 und 2. Warum wird "Genre" so viel stärker als die anderen Felder erhöht? Wenn die Suche über Daten durchgeführt wird, die etwas homogen ist (wie bei "Genre" in der `musicstoreindex`), könnten Sie eine größere Varianz in der relativen Gewichtung. Beispielsweise ist in der `musicstoreindex`, "Rock" angezeigt wird, als beide "Genre" und an identisch genrebeschreibungen angezeigt. Wenn "Genre", "Genre" Beschreibung überwiegen werden sollen, benötigen das Feld "Genre" eine viel höhere relative Gewichtung.

### <a name="functions"></a>Funktionen
Funktionen werden verwendet, wenn zusätzliche Berechnungen für den jeweiligen Kontext erforderlich sind. Gültige Funktionstypen werden `freshness`, `magnitude`, `distance` und `tag`. Jede Funktion verfügt über Parameter, die eindeutig sind.

* `freshness`sollte verwendet werden, wenn Sie höher stufen möchten wie neu oder ALT ein Element ist. Diese Funktion kann nur mit Datetime-Felder verwendet werden (`Edm.DataTimeOffset`). Beachten Sie die `boostingDuration` Attribut wird nur mit der Aktualität-Funktion verwendet.
* `magnitude`sollte verwendet werden, wenn Sie höher stufen, auf Basis möchten oder ein numerischer Wert nicht ausreicht. Szenarien, die für diese Funktion aufrufen umfassen die Verstärkung nach Gewinnspanne, Höchstpreis, niedrigster Preis oder downloadanzahl. Gegebenenfalls das umgekehrte Muster anzuwenden (z. B. zum Boost günstigerer Elemente mehr als höhere Preis Elemente), können Sie den Bereich zu niedrig, hoch umkehren. Angesichts einer Preisspanne von 100 € bis 1, setzen Sie `boostingRangeStart` bei 100 und `boostingRangeEnd` auf 1 fest, um die günstigerer Artikeln zu fördern. Diese Funktion kann nur mit Double- und Integer-Feldern verwendet werden.
* `distance`sollte verwendet werden, wenn Sie Nähe oder geografischen Standort höher stufen möchten. Diese Funktion kann nur verwendet werden, mit `Edm.GeographyPoint` Felder.
* `tag`sollte verwendet werden, wenn Sie nach Tags Gemeinsamkeit zwischen Dokumenten und Suchabfragen höher stufen möchten. Diese Funktion kann nur verwendet werden, mit `Edm.String` und `Collection(Edm.String)` Felder.

#### <a name="rules-for-using-functions"></a>Regeln für die Verwendung von Funktionen
* Funktionstyp (Aktualität, Größe, Abstand, Tag) muss Kleinbuchstaben sein.
* Funktionen, darf keine leeren oder null-Werte enthalten. Wenn Sie Fieldname einschließen, müssen Sie insbesondere auf einen anderen Wert festlegen.
* Funktionen können nur auf filterbare Felder angewendet werden. Finden Sie unter [Create Index](search-api-2015-02-28-preview.md#CreateIndex) für Weitere Informationen zu filterbaren Feldern.
* Funktionen können nur auf Felder angewendet werden, die in der Fields-Auflistung eines Indexes definiert sind.

Nachdem der Index definiert wurde, erstellen Sie den Index durch Hochladen des Indexschemas, gefolgt von Dokumenten. Finden Sie unter [Create Index](search-api-2015-02-28-preview.md#CreateIndex) und [hinzufügen oder Update-Dokumente](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) Anweisungen zu diesen Vorgängen. Nachdem der Index erstellt wird, sollten Sie ein funktionsfähiges Bewertungsprofil verfügen, das mit Ihren Suchdaten arbeitet.

<a name="bkmk_template"></a>

## <a name="template"></a>Vorlage
Dieser Abschnitt zeigt die Syntax und die Vorlage für die Bewertungsprofile. Verweisen auf [Index Attributverweis](#bkmk_indexref) im nächsten Abschnitt, um Beschreibungen der Attribute.

    ...
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>

## <a name="scoring-profile-property-reference"></a>Eigenschaftsverweis Profil Bewertung
> [!NOTE]
> Eine Bewertungsfunktion kann nur auf Felder angewendet werden, die filterbar sind.
>
>

| Eigenschaft | Beschreibung |
| --- | --- |
| `name` |Erforderlich. Dies ist der Name des Bewertungsprofils. Es folgt die gleichen Namenskonventionen eines Felds. Es muss mit einem Buchstaben beginnen, Sie können keine Punkte, Doppelpunkte oder @-Symbole und darf nicht mit dem Begriff "AzureSearch" (Groß-/Kleinschreibung) beginnen. |
| `text` |Enthält die Weights-Eigenschaft. |
| `weights` |(Optional) Ein Name / Wert-Paar, einen Feldnamen und relative Gewichtung angibt. Relative Gewichtung muss eine positive ganze Zahl oder Gleitkommazahl sein. Sie können den Feldnamen ohne eine entsprechende Gewichtung angeben. Gewichtungen werden verwendet, um die Bedeutung der Felder untereinander anzugeben. |
| `functions` |(Optional) Beachten Sie, dass eine Bewertungsfunktion nur auf Felder angewendet werden kann, die filterbar sind. |
| `type` |Für bewertungsfunktionen erforderlich. Gibt den Typ der zu verwendenden Funktion an. Gültige Werte sind `magnitude`, `freshness`, `distance` und `tag`. Sie können in jedem Bewertungsprofil mehrere Funktionen einschließen. Der Funktionsname muss Kleinbuchstaben angegeben sein. |
| `boost` |Für bewertungsfunktionen erforderlich. Eine positive Zahl, die als Multiplikator für die rohbewertung verwendet wird. Es darf nicht gleich 1 sein. |
| `fieldName` |Für bewertungsfunktionen erforderlich. Eine Bewertungsfunktion kann nur auf Felder angewendet werden, Teil der feldauflistung für den Index und filterbar sind. Darüber hinaus führt jeder Funktionstyp zusätzliche Einschränkungen (Aktualität wird mit Datetime-Felder, die Größe für Integer- oder double-Felder, die Entfernung für Location-Felder und Transponder mit der Zeichenfolge oder Zeichenfolge auflistungsfeldern verwendet). Sie können nur ein einzelnes Feld pro Funktionsdefinition angeben. Um Größe zweimal in demselben Profil verwenden, müssen Sie z. B. zwei Definitionen-Größe, einen für jedes Feld enthalten. |
| `interpolation` |Für bewertungsfunktionen erforderlich. Definiert die Steigung für die die bewertungsoptimierung vom Anfang des Bereichs bis zum Ende des Bereichs. Gültige Werte sind `linear` (Standard), `constant`, `quadratic`, und `logarithmic`. Finden Sie unter [Festlegen von Interpolationen](#bkmk_interpolation) Details. |
| `magnitude` |Das Ausmaß bewerten die Funktion wird verwendet, um Rangfolgen auf Basis des Wertebereichs von Werten für ein numerisches Feld. Einige der am häufigsten verwendete Beispiele hierfür sind:<ul><li>Bewertungssterne: Ändern Sie die Bewertung auf Grundlage des Werts innerhalb des Felds "Sternbewertung". Wenn zwei Elemente relevant sind, wird das Element mit der höheren Bewertung zuerst angezeigt.</li><li>Rand: Wenn zwei Dokumente relevant sind, ein Einzelhändler möglicherweise Dokumente zu steigern, die zuerst höhere Gewinnspanne aufweisen.</li><li>Anzahl der Klicks: für Anwendungen, die zum Nachverfolgen, mithilfe von Aktionen mit Produkten oder Seiten klicken, können Sie Größe Boost-Elemente, die den meisten Datenverkehr tendenziell.</li><li>Anzahl der Downloads: haben Sie für Anwendungen, die Downloads nachverfolgt werden, die Sie höher stufen können die Elemente an, die meisten Downloads.</li></ul> |
| `magnitude:boostingRangeStart` |Legt den Startwert des Bereichs für die Größe bewertet wird. Der Wert muss eine ganze Zahl oder Gleitkommazahl sein. Für die sternbewertungen von 1 bis 4 würde dies 1 sein. Dazu gehören für Spannen über 50 % 50 Daten. |
| `magnitude:boostingRangeEnd` |Legt den Endwert des Bereichs für die Größe bewertet wird. Der Wert muss eine ganze Zahl oder Gleitkommazahl sein. Dazu gehören für die sternbewertungen von 1 bis 4 4 Daten. |
| `magnitude:constantBoostBeyondRange` |Gültige Werte sind "true" oder "false" (Standard). Bei Festlegung auf "true" wird die vollständige Verstärkung auf Dokumente angewendet, die einen Wert für das Zielfeld aufweisen, die größer als das obere Ende des Bereichs ist fortgesetzt. Wenn "false" wird nicht die Verstärkung dieser Funktion angewendet werden, auf Dokumente mit dem Wert für das Feld "Ziel", das außerhalb des Bereichs liegt. |
| `freshness` |Die Bewertungsfunktion Aktualität wird verwendet, um rangfolgenbewertungen für Elemente, die basierend auf Werten in DateTimeOffset-fieldern zu ändern. Ein Element mit einem aktuelleren Datum kann z. B. höher als ältere Elemente eingestuft werden. (Beachten Sie, dass es auch möglich, Rank Elemente wie Kalenderereignisse mit der Zukunft liegende Datumsangaben so, dass Elemente, die näher an der aktuellen höher als Elemente weiter in der Zukunft Rang haben können.) In der aktuellen Dienstversion wird ein Ende des Bereichs bis zum aktuellen Zeitpunkt behoben werden. Das andere Ende wird in der Vergangenheit liegen, basierend auf den `boostingDuration`. Verwenden Sie zur Verbesserung der ein Bereich von Zeiten in der Zukunft eine Negative `boostingDuration`. Die Rate, mit der die Verstärkung von einem maximalen und minimalen Bereich wird durch die Interpolation bestimmt, die auf das Bewertungsprofil angewendet (siehe folgende Abbildung). Wählen Sie einen Verstärkungsfaktor von weniger als 1, zum Umkehren des verstärkungsfaktors Faktor angewendet. |
| `freshness:boostingDuration` |Legt eine Gültigkeitsdauer nach der die Verstärkung für ein bestimmtes Dokument beendet wird. Finden Sie unter [Festlegen von BoostingDuration](#bkmk_boostdur) im folgenden Abschnitt zur Syntax und Beispiele. |
| `distance` |Schließen Sie die Entfernung, die Bewertungsprofile-Funktion verwendet wird, um zu beeinflussen, die Bewertung von Dokumenten auf Basis oder weit sie sind relativ zu einem geografischen Referenzstandort. Speicherort der als Teil der Abfrage in einem Parameter angegeben wird (mithilfe der `scoringParameter` Abfrageparameter) als Lon, Lat-Argument. |
| `distance:referencePointParameter` |Ein Parameter in Abfragen zur Verwendung als Verweis übergeben werden. "scoringparameter" ist ein Abfrageparameter. Finden Sie unter [Dokumente durchsuchen](search-api-2015-02-28-preview.md#SearchDocs) Beschreibungen der Abfrageparameter. |
| `distance:boostingDistance` |Eine Zahl, die Entfernung in Kilometern ab der Verweisposition angibt, in dem der verstärkungsbereich endet. |
| `tag` |Das Tag Bewertungsfunktion wird verwendet, um das Ergebnis von Dokumente basierend auf dem RFID-Transponder in Dokumenten und Suchabfragen beeinflusst. Dokumente mit Tags auch in zahlreichen die Suchabfrage werden erhöht werden. Die Tags für die Suchabfrage wird als bewerteten Parameter in jeder suchanforderung bereitgestellt (mithilfe der `scoringParameter` Abfrageparameter). |
| `tag:tagsParameter` |Ein Parameter zur Übergabe in Abfragen, um Kategorien für eine bestimmte Anforderung anzugeben. `scoringParameter`ist ein Abfrageparameter. Finden Sie unter [Dokumente durchsuchen](search-api-2015-02-28-preview.md#SearchDocs) Beschreibungen der Abfrageparameter. |
| `functionAggregation` |(Optional) Gilt nur, wenn Funktionen angegeben werden. Gültige Werte sind: `sum` (Standard), `average`, `minimum`, `maximum`, und `firstMatching`. Eine suchbewertung ist ein einzelner Wert, der aus mehreren Variablen, einschließlich mehrerer Funktionen berechnet wird. Diese Attribute gibt an, wie die verstärkungen aller Funktionen in eine einzelne aggregate Verstärkung kombiniert werden, die Sie dann auf die basisdokumentbewertung angewendet wird. Die basisbewertung basiert auf dem Tf-Idf-Wert, der aus dem Dokument und die Suchabfrage berechnet. |
| `defaultScoringProfile` |Wenn eine suchanforderung ausgeführt wird, wenn kein Bewertungsprofil angegeben ist, ist standardbewertung verwendet (Tf-Idf nur). Ein Standardname für das Bewertungsprofil kann hier festgelegt werden, verursacht Azure Search, dieses Profil zu verwenden, wenn in der suchanforderung kein bestimmtes Profil angegeben wird. |

<a name="bkmk_interpolation"></a>

## <a name="set-interpolations"></a>Festlegen von Interpolationen
Interpolationen gestatten Ihnen die Steigung für die die bewertungsoptimierung vom Anfang des Bereichs bis zum Ende des Bereichs zu definieren. Die folgenden Interpolationen können verwendet werden:

* `Linear`: Für Elemente, die innerhalb der maximalen und minimalen Bereichs befinden, erfolgt die auf das Element angewendete Verstärkung in konstant abnehmender Stärke. Linear ist die standardinterpolation für ein Bewertungsprofil.
* `Constant`: Für Elemente, die innerhalb der Anfangs- und Endbereichs befinden, wird eine Konstante Verstärkung auf die rangfolgeergebnisse angewendet werden.
* `Quadratic`: Im Vergleich zur linearen Interpolation, die eine konstant abnehmende Verstärkung aufweist, "quadratisch" anfänglich mit kleineren Geschwindigkeit absteigend und klicken Sie dann, wie sie das Bereichsende fast erreicht wird, es verringert Intervallen deutlich höher. Diese Interpolationsoption ist im Tag bewertungsfunktionen nicht zulässig.
* `Logarithmic`: Im Vergleich zur linearen Interpolation, die eine konstant abnehmende Verstärkung aufweist, logarithmisch anfänglich mit höherer Geschwindigkeit absteigend und klicken Sie dann, wie sie das Bereichsende fast erreicht wird, es verringert in viel kleineren Intervallen. Diese Interpolationsoption ist im Tag bewertungsfunktionen nicht zulässig.

<a name="Figure1"></a> ![][1]

<a name="bkmk_boostdur"></a>

## <a name="set-boostingduration"></a>Festlegen von boostingDuration
`boostingDuration`ist ein Attribut der Aktualität-Funktion. Sie können damit eine Ablaufdauer festlegen nach der die Verstärkung für ein bestimmtes Dokument beendet wird. Um eine Produktlinie oder Marke für einen 10-Tage-werbezeitraum zu verstärken würden Sie z. B. den 10-Tage-Zeitraum als "P10D" für diese Dokumente angeben. Oder geben Sie zur Verbesserung der bevorstehender Ereignisse in den nächsten Wochen "-P7D".

`boostingDuration`muss als eine XSD "DayTimeDuration"-Wert (eingeschränkte Teilmenge von einem ISO 8601-Zeitwerts) formatiert sein. Das Muster dafür lautet: `[-]P[nD][T[nH][nM][nS]]`.

Die folgende Tabelle enthält einige Beispiele für.

| Dauer | von boostingDuration |
| --- | --- |
| 1 Tag |"P1D" |
| 2 Tage und 12 Stunden |"P2DT12H" |
| 15 Minuten |"PT15M" |
| 30 Tage, 5 Stunden, 10 Minuten und 6.334 Sekunden |"P30DT5H10M6.334S" |

Weitere Beispiele finden Sie unter [XML-Schema: Datentypen (W3.org-Website)](http://www.w3.org/TR/xmlschema11-2/).

**Siehe auch**
[Azure-Suchdienst-REST-API](http://msdn.microsoft.com/library/azure/dn798935.aspx) auf MSDN <br/>
[Erstellen des Index (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) auf MSDN<br/>
[Fügen Sie ein Bewertungsprofil zu einem Suchindex](http://msdn.microsoft.com/library/azure/dn798928.aspx) auf MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png

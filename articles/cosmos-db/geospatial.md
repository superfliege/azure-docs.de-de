---
title: Arbeiten mit Geodaten in Azure-Cosmos-Datenbank | Microsoft Docs
description: "Grundlegendes zum Erstellen, index und Abfrage räumliche Objekte mit Azure-Cosmos-Datenbank und die DocumentDB-API."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 82ce2898-a9f9-4acf-af4d-8ca4ba9c7b8f
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/22/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d5785c81fb597e7d30eb7d3a880e7194d8358ed5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a>Arbeiten mit Geodaten und GeoJSON-Standortdaten in der Azure-Cosmos-DB
Dieser Artikel ist eine Einführung in die Geospatial-Funktionalität in [Azure Cosmos-DB](https://azure.microsoft.com/services/cosmos-db/). Nach dem Lesen, werden Sie die folgenden Fragen beantworten können:

* Wie werden räumliche Daten in Azure-Cosmos-Datenbank werden gespeichert?
* Wie kann ich Geodaten in Azure Cosmos-Datenbanken in SQL und LINQ-Abfragen ausführen?
* Wie ich aktivieren oder deaktivieren Sie die räumliche Indizierung in Azure-Cosmos-Datenbank?

In diesem Artikel wird die Arbeit mit räumlichen Daten mit der DocumentDB-API veranschaulicht. Finden Sie in der [GitHub Projekt](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) Codebeispiele.

## <a name="introduction-to-spatial-data"></a>Einführung in die räumlichen Daten
Räumliche Daten beschreibt die Position und die Form der Objekte im Raum. In den meisten Anwendungen entsprechen diese Objekte auf der Erde dar, d. h. Geodaten. Räumliche Daten können verwendet werden, um den Speicherort einer Person, einen Ort, von Interesse sind oder die Begrenzung einer Stadt oder einen See darzustellen. Einsatzbereiche umfassen häufig NEAR-Abfragen für "gefunden werden z. B. alle in Cafés meine aktuelle Position in der Nähe von". 

### <a name="geojson"></a>GeoJSON
Azure Cosmos-Datenbank unterstützt, Indizierung und Abfragen von Geodaten Verwaltungspunkt-Daten, die mit dargestellt, hat die [GeoJSON-Spezifikation](https://tools.ietf.org/html/rfc7946). GeoJSON-Datenstrukturen sind immer gültige JSON-Objekte, damit sie mithilfe der Azure-Cosmos-Datenbank ohne spezielle Tools und Bibliotheken abgefragt und gespeichert werden können. Die Azure-Cosmos-DB-SDKs bieten Hilfsklassen und Methoden, die zum Arbeiten mit räumlichen Daten zu erleichtern. 

### <a name="points-linestrings-and-polygons"></a>Punkte, LineStrings und Polygone
Ein **Punkt** kennzeichnet eine einzelne Position im Raum. In Geodaten stellt ein Punkt den exakten Speicherort, der eine Anschrift an einem Lebensmittelladen, öffentlich zugängliche Kioskcomputer, ein Auto oder eine Stadt sein könnte.  Ein Punkt wird im GeoJSON (und Azure-Cosmos-Datenbank) mit ihrer Koordinate Paar oder Längen- und Breitengrad dargestellt. Hier ist ein Beispiel für JSON für einen Punkt.

**Punkte in Azure-Cosmos-DB**

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> GeoJSON-Spezifikation gibt Längengrad First und Breitengrad zweiten. Wie in anderen Anwendungen Zuordnung Längen- und Breitengrad sind Winkel und Grad errechnet. Längengradwerte werden aus dem Nullmeridian gemessen und zwischen-180 und 180.0 Grad und Breitengrad sind Werte werden vom Äquator gemessen und sind im Bereich zwischen-90.0 und 90.0 Grad liegen. 
> 
> Azure Cosmos-DB interpretiert Koordinaten pro die Referenzsystem WGS 84 dargestellt. Nachstehen finden Sie ausführliche Informationen zur-Koordinate Referenzsysteme.
> 
> 

Dies kann in einem Azure-Cosmos-DB-Dokument eingebettet werden, wie in diesem Beispiel für ein Benutzerprofil mit Standortdaten dargestellt:

**Verwenden Sie Profil mit dem Speicherort gespeichert, in der Azure-Cosmos-Datenbank**

```json
{
    "id":"documentdb-profile",
    "screen_name":"@CosmosDB",
    "city":"Redmond",
    "topics":[ "global", "distributed" ],
    "location":{
        "type":"Point",
        "coordinates":[ 31.9, -4.8 ]
    }
}
```

Zusätzlich zu den Punkten unterstützt GeoJSON auch LineStrings und Polygone. **LineStrings** eine Reihe von zwei oder mehr Punkten als auch der Liniensegmente, die diese verbinden darstellen. In Geodaten sind LineStrings häufig zur Darstellung von Autobahnen oder Flüsse verwendet. Ein **Polygon** ist eine Begrenzung der miteinander verbundenen Punkten, die eine geschlossene LineString bildet. Polygone werden häufig zur Darstellung von natürlichen Formaten wie Seen oder politische Gerichtsbarkeiten wie Städte und Bundesstaaten verwendet. Hier ist ein Beispiel eines Vielecks, das in der Azure-Cosmos-Datenbank. 

**Polygone in GeoJSON**

```json
{
    "type":"Polygon",
    "coordinates":[
        [ 31.8, -5 ],
        [ 31.8, -4.7 ],
        [ 32, -4.7 ],
        [ 32, -5 ],
        [ 31.8, -5 ]
    ]
}
```

> [!NOTE]
> GeoJSON-Spezifikation erfordert, dass für gültige Polygone, die letzte Koordinatenpaar angegebene identisch mit der ersten eine geschlossene Form zu erstellen.
> 
> Punkte innerhalb eines Polygons müssen gegen den Uhrzeigersinn Reihenfolge angegeben werden. Ein Polygon im Uhrzeigersinn angegeben stellt die Umkehrung der Region darin dar.
> 
> 

Zusätzlich zur Point, LineString und Polygon, gibt GeoJSON auch die Darstellung für mehrere geografische Standorte gruppieren sowie zum Zuordnen von beliebiger Eigenschaften zu geografischen Standort wie einer **Feature**. Da diese Objekte gültiges JSON-Format sind, können sie alle gespeichert und verarbeitet werden im Azure-Cosmos-Datenbank. Azure-Cosmos-Datenbank unterstützt jedoch nur die automatische Indizierung von Punkten.

### <a name="coordinate-reference-systems"></a>Koordinatensysteme Verweis
Da die Form der Erde dar, auf unregelmäßiger festgelegt ist, wird Koordinaten von Geodaten in vielen-Koordinate Referenzsysteme (CR), jeweils mit ihren eigenen Frames als Referenz und Maßeinheiten dargestellt. Die "National Raster von Großbritannien" ist z. B. eine Referenzsystem ist sehr genau für Großbritannien, aber nicht außerhalb davon. 

Die am häufigsten verwendeten CRS verwendet ist heute die World Geodetic System [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). Verwenden WGS 84 GPS-Geräten und viele Zuordnungsdienste einschließlich Google Maps und Bing Maps-APIs. Azure Cosmos-Datenbank unterstützt die Volltextindizierung und-Abfrage mithilfe der WGS 84 CRS nur Geodaten. 

## <a name="creating-documents-with-spatial-data"></a>Erstellen von Dokumenten mit räumlichen Daten
Wenn Sie Dokumente, die GeoJSON-Werte enthalten erstellen, werden sie automatisch mit einem räumlichen Index unter Verwendung von Gerätepixeln die indizierungsrichtlinie der Auflistung indiziert. Wenn Sie ein Azure-Cosmos DB SDK in einer dynamisch typisierte Sprache wie z. B. Python oder Node.js arbeiten, müssen Sie gültige GeoJSON erstellen.

**Erstellen von Dokument mit Geodaten in Node.js**

```json
var userProfileDocument = {
    "name":"documentdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within the callback
});
```

Wenn Sie die DocumentDB-APIs arbeiten, können Sie mithilfe der `Point` und `Polygon` Klassen innerhalb der `Microsoft.Azure.Documents.Spatial` Namespace So betten Sie Standortinformationen in Ihrer Anwendungsobjekte ein. Diese Klassen vereinfachen, die Serialisierung und Deserialisierung von räumlichen Daten in GeoJSON.

**Erstellen von Dokument mit Geodaten in .NET**

```json
using Microsoft.Azure.Documents.Spatial;

public class UserProfile
{
    [JsonProperty("name")]
    public string Name { get; set; }

    [JsonProperty("location")]
    public Point Location { get; set; }

    // More properties
}

await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
    new UserProfile 
    { 
        Name = "documentdb", 
        Location = new Point (-122.12, 47.66) 
    });
```

Wenn Sie nicht die Breiten- und Längengrad Informationen haben, aber die physischen Adressen oder den Standortnamen wie Stadt und Land haben, können Sie die aktuellen Koordinaten nachschlagen, unter Verwendung eines Diensts geocodierung wie Bing Maps-REST-Dienste. Erfahren Sie mehr über die Bing Maps-geocodierung [hier](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Abfragen von räumliche Typen
Nun, dass wir einen Blick auf die einzufügende Geodaten übernommen haben, werfen wir einen Blick auf diese Daten mithilfe von Azure-Cosmos-Datenbank mithilfe von SQL- und LINQ-Abfragen.

### <a name="spatial-sql-built-in-functions"></a>Räumliche integrierte SQL-Funktionen
Azure Cosmos-Datenbank unterstützt die folgenden integrierten Open Geospatial Consortium (OGC)-Funktionen für Geospatial Abfragen. Weitere Informationen zu den vollständigen Satz von integrierten Funktionen in der SQL-Sprache, finden Sie in [Abfrage Azure Cosmos-DB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (Spatial_expr, Spatial_expr)</td>
  <td>Gibt den Abstand zwischen den beiden GeoJSON Punkt-, Polygon oder LineString Ausdrücken zurück.</td>
</tr>
<tr>
  <td>ST_WITHIN (Spatial_expr, Spatial_expr)</td>
  <td>Gibt einen booleschen Ausdruck, der angibt, ob das erste GeoJSON-Objekt (Punkt-, Polygon oder LineString) innerhalb des zweiten GeoJSON-Objekts (Punkt-, Polygon oder LineString) ist.</td>
</tr>
<tr>
  <td>ST_INTERSECTS (Spatial_expr, Spatial_expr)</td>
  <td>Gibt einen booleschen Ausdruck, der angibt, ob die beiden angegebenen GeoJSON-Objekte (Punkt-, Polygon oder LineString) überschneiden.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Gibt einen booleschen Wert, der angibt, ob der angegebene GeoJSON Punkt-, Polygon oder LineString-Ausdruck gültig ist.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Gibt ein JSON-Wert, der einen booleschen Wert enthält Wert, wenn der angegebene GeoJSON Punkt-, Polygon oder LineString-Ausdruck gültig ist, und wenn ungültige, außerdem den Grund als Zeichenfolge zurück.</td>
</tr>
</table>

Räumliche Funktionen können verwendet werden, um NEAR-Abfragen für räumliche Daten auszuführen. Hier ist z. B. eine Abfrage, die alle Familie Dokumente zurückgibt, die innerhalb von 30 km von der angegebenen Position, die mit der integrierten ST_DISTANCE-Funktion sind. 

**Abfrage**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Wenn Sie die räumliche Indizierung in Ihre indexrichtlinie angeben, werden "Abstand Abfragen" effizient über den Index bedient. Weitere Informationen zu räumlichen Indizierung finden Sie unter weiter unten im Abschnitt. Wenn Sie einen räumlichen Index für die angegebenen Pfade nicht haben, können Sie Abfragen nach räumliche Daten weiterhin ausführen, durch Angabe `x-ms-documentdb-query-enable-scan` Anforderungsheader mit der festgelegte Wert auf "True". In .NET hierzu übergeben Sie das optionale **FeedOptions** Argument zu Abfragen mit [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) auf "true" festgelegt ist. 

ST_WITHIN kann verwendet werden, zum Überprüfen, ob ein Punkt innerhalb eines Polygons liegt. Häufig sind Polygone verwendet, um die Grenzen wie Postleitzahlen, Status Grenzen oder natürliche Formaten darstellen. Erneut, wenn Sie die räumliche Indizierung in Ihre indexrichtlinie angeben, werden dann "within" Abfragen effizient über den Index bedient. 

Polygon-Argumente in ST_WITHIN können nur einen einzelnen Ring enthalten, d. h. die Polygone müssen Löcher darin nicht enthalten. 

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> Ähnlich wie nicht übereinstimmende Typen funktioniert in Azure-Cosmos-DB-Abfrage in der Location-Wert angegeben, entweder Argument ist falsch formatiert oder ungültig, und es ergeben, **undefined** und die ausgewerteten Dokument aus den Abfrageergebnissen übersprungen werden soll. Wenn die Abfrage keine Ergebnisse zurückgibt, führen Sie ST_ISVALIDDETAILED,-Debug-Warum Spatail Typ ungültig ist.     
> 
> 

Azure Cosmos-Datenbank unterstützt auch die inverse Abfragen ausführen, d. h. Sie indizieren Polygone oder Linien in Azure-Cosmos-Datenbank und anschließend Abfragen für die Bereiche, die einen bestimmten Punkt enthalten. Dieses Muster wird häufig in Logistik verwendet, um zu identifizieren, z. B., wenn ein LKW eingibt oder einen bestimmten Bereich verlässt. 

**Abfrage**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Ergebnisse**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID und ST_ISVALIDDETAILED kann verwendet werden, zu überprüfen, ob ein räumliches Objekt ungültig ist. Die folgende Abfrage wird z. B. die Gültigkeit eines Punkts mit einem Wert außerhalb des Gültigkeitsbereichs Breitengrad (-132.8) überprüft. ST_ISVALID nur einen booleschen Wert zurückgibt und ST_ISVALIDDETAILED gibt den booleschen Wert und eine Zeichenfolge, enthält den Grund, warum sie ungültig betrachtet wird.

** Fragen **

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Ergebnisse**

    [{
      "$1": false
    }]

Diese Funktionen können auch verwendet werden, um Polygone zu überprüfen. Beispielsweise verwenden Sie hier wir ST_ISVALIDDETAILED ein Polygon zu überprüfen, die nicht geschlossen ist. 

**Abfrage**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Ergebnisse**

    [{
       "$1": { 
            "valid": false, 
            "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a Polygon must have the same start and end points." 
          }
    }]

### <a name="linq-querying-in-the-net-sdk"></a>LINQ-Abfragen im .NET SDK
Das DocumentDB .NET SDK auch Anbieter stub-Methoden `Distance()` und `Within()` für die Verwendung in LINQ-Ausdrücke. Die DocumentDB-LINQ-Anbieter übersetzt diese Methodenaufrufe in die entsprechende SQL-integrierten Funktionsaufrufe (ST_DISTANCE und ST_WITHIN bzw.). 

Hier ist ein Beispiel für eine LINQ-Abfrage, die alle Dokumente in der Azure-Cosmos-DB-Auflistung, deren Wert "Location" ist in einem Umkreis von 30km des angegebenen zeigen Sie mithilfe von LINQ, findet.

**LINQ-Abfrage für Abstand**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Hier ist auf ähnliche Weise eine Abfrage für die Suche nach alle Dokumente, deren "Location" in das angegebene Feld/Polygon ist. 

**LINQ-Abfragen für in**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Nun, dass wir einen Blick auf zum Abfragen von Dokumenten mithilfe von LINQ und SQL übernommen haben, werfen wir einen Blick auf Azure-Cosmos-Datenbank für die räumliche Indizierung konfigurieren.

## <a name="indexing"></a>Indizierung
Wie in beschrieben der [Schema agnostisch Indizierung mit Azure-Cosmos-DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) Papier, wir entworfen Azure Cosmos DB Datenbankmodul werden tatsächlich Schema agnostisch, und geben Sie erstklassige Unterstützung für JSON. Datenbankmodul Schreibvorgänge optimiert der Azure-Cosmos-DB versteht systemintern räumliche Daten (Punkte, Polygone und Linien), die im GeoJSON-Standard dargestellt.

Kurz gesagt, die Geometrie mit geodätische Koordinaten auf einer 2D-Ebene projiziert wird progressiv in Zellen mit unterteilt eine **Quadtree**. Diese Zellen werden zugeordnet, und 1D basierend auf den Speicherort der Zelle innerhalb einer **Raum füllenden Hilbert Kurve**, durch die Positionierung von Punkten beibehalten. Darüber hinaus beim Standortdaten indiziert ist, erfolgt über genannten **Mosaik**, d. h. alle Zellen, die einen Ort schneiden identifiziert und als Schlüssel im Index Azure-Cosmos-Datenbank gespeichert sind. Zur Abfragezeit werden Argumente wie Punkten und Polygonen auch im Mosaikprozess berücksichtigt, um die entsprechende Zelle ID-Bereiche zu extrahieren und dann zum Abrufen von Daten aus dem Index verwendet.

Wenn Sie eine indizierungsrichtlinie angeben, die räumlichen Index für / * (alle Pfade), und klicken Sie dann alle Punkte in der Auflistung gefunden für effiziente Abfragen nach räumlichen Daten (ST_WITHIN und ST_DISTANCE) indiziert werden. Räumliche Indizes nicht über einen Genauigkeitswert mit einfacher verfügen, und verwenden Sie immer einen Standardwert für die Genauigkeit.

> [!NOTE]
> Azure Cosmos-Datenbank unterstützt die automatische Indizierung aus Punkten, Polygone und LineStrings
> 
> 

Der folgende JSON-Codeausschnitt zeigt ein indizierungsrichtlinie mit räumliche Indizierung aktiviert, d. h. index einen beliebigen GeoJSON Punkt innerhalb von Dokumenten für räumliche Abfragen gefunden. Wenn Sie die indizierungsrichtlinie über das Azure-Portal ändern, können Sie die folgenden JSON für indizierungsrichtlinie räumliche Indizierung für Ihre Sammlung aktivieren angeben.

**Auflistung indizieren von JSON mit räumlich für Punkten und Polygonen aktiviert Richtlinie**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Hier ist ein Codeausschnitt in .NET, das zum Erstellen einer Sammlung mit räumliche Indizierung für alle Pfade mit Punkten eingeschaltet veranschaulicht. 

**Erstellen Sie eine Sammlung mit räumliche Indizierung**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

Und hier ist zum Ändern einer vorhandenen Sammlung zum Nutzen der räumlichen Indizierung über keine Punkte, die in den Dokumenten gespeichert werden.

**Ändern einer vorhandenen Sammlung mit räumliche Indizierung**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [!NOTE]
> Wenn der Speicherort GeoJSON Wert innerhalb des Dokuments falsch formatiert oder ungültig ist, wird dann er nicht für räumliche Abfragen indiziert abrufen. Sie können die Werte für mit ST_ISVALID und ST_ISVALIDDETAILED überprüfen.
> 
> Enthält die Auflistungsdefinition einen Partitionsschlüssel eingeben, wird der Fortschritt der Transformation für die Indizierung nicht gemeldet. 
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Nun, da Sie Informationen zum Einstieg in Geospatial-Unterstützung in Azure-Cosmos-DB gelernt haben, können Sie folgende Schritte ausführen:

* Codieren mit beginnen die [Geospatial .NET Codebeispiele auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
* Abrufen von Hände auf Geospatial für Abfragen an die [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)
* Erfahren Sie mehr über [Azure Cosmos-DB-Abfrage](documentdb-sql-query.md)
* Erfahren Sie mehr über [Azure Cosmos DB Indizierung Richtlinien](indexing-policies.md)


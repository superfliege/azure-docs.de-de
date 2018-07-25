---
title: Übersicht über Azure Maps | Microsoft-Dokumentation
description: Einführung in Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 07/12/2018
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 93fe8dc3f8ff991cd6c48923d9e2073e4e93f1ad
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39040846"
---
# <a name="what-is-azure-maps"></a>Was ist Azure Maps?
Azure Maps ist eine Sammlung mit Geodiensten, die auf aktuellen Kartendaten basieren, damit Sie für Ihre Webanwendungen und mobilen Anwendungen den präzisen geografischen Kontext bereitstellen können. Die Sammlung umfasst REST-APIs zum Rendern von Karten, eine Suchfunktion für Points of Interest, Routen zu Points of Interest, Verkehrsinformationen, Zeitzonen und IP to Location-Dienste. Sie können diese APIs verwenden, um mithilfe vertrauter Entwicklungstools im Handumdrehen Lösungen für die Integration von Standortinformationen in Ihre Azure-Lösungen zu entwickeln und zu skalieren. Zusammen mit den REST-APIs wird ein webbasiertes JavaScript-Steuerelement bereitgestellt, um die Entwicklung für verschiedene Medien einfach, flexibel und portabel zu gestalten. 

Im folgenden Video wird Azure Maps ausführlich beschrieben:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="services-in-azure-maps"></a>Dienste in Azure Maps

Azure Maps besteht aus den folgenden sechs Diensten, die für Ihre Azure-Anwendungen geografischen Kontext liefern können. 

### <a name="render-service"></a>Renderingdienst

Der Renderingdienst ist für Entwickler konzipiert, die Web- und mobile Anwendungen mit Kartenfunktionen erstellen möchten. Für den Dienst werden entweder qualitativ hochwertige Rastergrafikbilder, die in 19 Zoomebenen verfügbar sind, oder vollständig anpassbare Vektorformat-Kartenbilder verwendet.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

Über den Renderingdienst werden jetzt Vorschau-APIs bereitgestellt, um Entwicklern die Verwendung von Satellitenbildern zu ermöglichen. Weitere Informationen finden Sie im Artikel zu [Azure Maps-Render-APIs](https://docs.microsoft.com/rest/api/maps/render).


### <a name="route-service"></a>Routendienst 

Der Routendienst umfasst stabile Geometrieberechnungen für reale Infrastruktur und Wegbeschreibungen für verschiedene Transportmodi. Entwickler können mithilfe des Diensts Wegbeschreibungen für eine Reihe von Fortbewegungsmitteln (z.B. PKW, LKW, Fahrrad oder zu Fuß) generieren. Zudem kann der Dienst Faktoren wie Verkehrslage, Gewichtsbeschränkungen oder Transport von Gefahrgütern berücksichtigen.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

Der Routendienst ermöglicht jetzt eine Vorschau für die erweiterten Features, z.B. Batchverarbeitung für mehrere Routenanforderungen, Matrizen für Reisezeit und Entfernung zwischen verschiedenen Ausgangsorten und Zielen und Ermittlung von Routen oder Entfernungen, die Sie basierend auf Ihren Zeit- oder Kraftstoffanforderungen zurücklegen können. Ausführliche Informationen zu den Routingfunktionen finden Sie im Artikel zu [Azure Maps-Routen-APIs](https://docs.microsoft.com/rest/api/maps/route).


### <a name="search-service"></a>Suchdienst

Der Suchdienst ist für Entwickler bestimmt, um nach Adressen, Orten, Geschäftseinträgen nach Name oder Kategorie und anderen geografischen Informationen zu suchen. Außerdem kann mit dem Suchdienst eine [Inverse Geocodierung](https://en.wikipedia.org/wiki/Reverse_geocoding) für Adressen und Querstraßen basierend auf Breiten- und Längengraden durchgeführt werden. 

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

Außerdem verfügt der Suchdienst über erweiterte Features, z.B. Suche entlang einer Route, Suche innerhalb eines weiteren Bereichs, Zusammenfassen einer Gruppe von Suchanfragen zu einem Batch und Suche nach einem größeren Bereich anstelle eines Standorts. Die APIs für Batches und die Bereichssuche befinden sich derzeit in der Vorschauphase. Weitere Informationen zu den Suchfunktionen finden Sie auf der Seite zu [Azure Maps-Such-APIs](https://docs.microsoft.com/rest/api/maps/search).


### <a name="time-zone-service"></a>Zeitzonendienst

Mit dem Zeitzonendienst können Sie aktuelle, vergangene und zukünftige Zeitzoneninformationen abfragen, indem Sie entweder Breitengrad/Längengrad-Paare oder eine [IANA-ID](http://www.iana.org/) verwenden. Außerdem können Sie mit dem Zeitzonendienst Microsoft Windows-Zeitzonen-IDs in IANA-Zeitzonen konvertieren und einen Zeitzonenoffset zu UTC und die aktuelle Zeit in einer entsprechenden Zeitzone abrufen. Eine typische JSON-Antwort für eine Abfrage mit dem Zeitzonendienst sieht wie folgt aus:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Ausführliche Informationen zu diesem Dienst finden Sie auf der Seite zu den [Azure Maps-Zeitzonen-APIs](https://docs.microsoft.com/rest/api/maps/timezone).

### <a name="traffic-service"></a>Verkehrsinfodienst

Der Verkehrsinfodienst ist eine Suite mit Webdiensten für Entwickler, mit denen Web- und mobile Anwendungen erstellt werden können, für die Verkehrsinformationen erforderlich sind. Der Dienst stellt zwei Typen von Daten bereit:
    * Verkehrsfluss: In Echtzeit ermittelte Geschwindigkeiten und Reisezeiten für alle wichtigen Straßen des Verkehrsnetzes 
    * Verkehrsunfälle: Präzise Übersicht über die Staus und Störungen bzw. Unfälle im Straßennetz

![Azure Maps-Ansicht des Verkehrs](media/about-azure-maps/Introduction_Traffic.png)

Weitere Informationen finden Sie auf der Seite zu den [Azure Maps-Verkehrsinfo-APIs](https://docs.microsoft.com/rest/api/maps/traffic).

### <a name="ip-to-location"></a>IP to Location

„IP to Location“ ist ein Vorschaudienst, mit dem Sie den zweistelligen Ländercode für eine bestimmte IP-Adresse abrufen können. Mit diesem Dienst können Sie Ihre Anwendung so anpassen, dass sie bestimmte geopolitische Einschränkungen erfüllt, und die Benutzerfreundlichkeit erhöhen, indem Sie den Inhalt der Anwendung basierend auf dem geografischen Standort ändern. 


## <a name="programming-model"></a>Programmiermodell

Azure Maps ist auf Mobilität ausgelegt und kann für plattformübergreifende Anwendungen eingesetzt werden. Es wird ein Programmiermodell genutzt, das sprachunabhängig ist und die JSON-Ausgabe über [REST-APIs](https://docs.microsoft.com/rest/api/maps/) unterstützt. 

Außerdem verfügt Azure Maps über ein komfortables [JavaScript-Kartensteuerelement](https://docs.microsoft.com/javascript/api/azure-maps-javascript/?view=azure-iot-typescript-latest) mit einem einfachen Programmiermodell für die schnelle und einfache Entwicklung webbasierter und mobiler Anwendungen. 


## <a name="usage"></a>Verwendung

Für den Zugriff auf die Maps-Dienste navigieren Sie zum [Azure-Portal](http://portal.azure.com) und erstellen ein Azure Maps-Konto. 

Für Azure Maps wird ein schlüsselbasiertes Authentifizierungsschema verwendet. Für Ihr Konto wurden zwei Schlüssel vorab generiert. Beginnen Sie damit, diese Standortfunktionen direkt in Ihre Anwendungen zu integrieren, indem Sie in den Anforderungen, die Sie an den Azure Maps-Dienst senden, einen Ihrer Schlüssel verwenden.

## <a name="supported-regions"></a>Unterstützte Regionen
Die Azure Maps-API ist derzeit in allen Ländern verfügbar, mit Ausnahme von: 

* Argentinien
* China
* Indien
* Marokko
* Pakistan
* Südkorea

Überprüfen Sie Ihre aktuelle IP-Adresse, und vergewissern Sie sich, dass sich der Standort der IP-Adresse nicht in einem der oben angegebenen nicht unterstützten Länder befindet.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu den neuen Features von Azure Maps: 
    - [Route Matrix, isochrones, IP lookup, and more added to Azure Maps](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/) (Hinzugefügte Features für Azure Maps: Routenmatrix, Isochronen, IP-Suche und mehr). 
- Probieren Sie als Nächstes eine Beispiel-App aus, mit der der Dienst veranschaulicht wird.
    - [Demo der interaktiven Kartensuche starten](quick-demo-map-app.md)

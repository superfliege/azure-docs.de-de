---
title: Übersicht über Azure Maps | Microsoft-Dokumentation
description: Einführung in Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 09/12/2018
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 9bf168a0f06d662526d172687e54327b55d50bd8
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104266"
---
# <a name="what-is-azure-maps"></a>Was ist Azure Maps?

Azure Maps ist eine Sammlung mit Geodiensten, die auf aktuellen Kartendaten basieren, damit Sie für Ihre Webanwendungen und mobilen Anwendungen den präzisen geografischen Kontext bereitstellen können. Sie enthält die REST-APIs für das Rendering von Karten und die Suche nach POIs (Points of Interest). Die APIs können zudem Routen zu Points of Interest, die Verkehrslage, Zeitzonen und einen Standort anhand einer IP-Adresse ermitteln. Die APIs sind mit vertrauten Entwicklungstools kompatibel, sodass im Handumdrehen Lösungen für die Integration von Standortinformationen in Ihre Azure-Lösungen entwickelt und skaliert werden können. Neben den REST-APIs wird das webbasierte [JavaScript-Kartensteuerelement](https://docs.microsoft.com/javascript/api/azure-maps-control) bereitgestellt, um die Entwicklung für verschiedene Medien einfach, flexibel und portabel zu gestalten.

Im folgenden Video wird Azure Maps ausführlich beschrieben:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Kartensteuerelemente

### <a name="web-control"></a>Websteuerelement

Mit dem Azure Maps-Websteuerelement können Sie interaktive Karten mit eigenen Inhalten und Bildern anpassen, die in Ihren webbasierten oder mobilen Anwendungen angezeigt werden sollen. Dieses Steuerelement nutzt WebGL, was das Rendern umfangreicher Datasets mit hoher Leistung ermöglicht. Verwenden Sie für die Entwicklung mit dem Steuerelement JavaScript oder TypeScript.

![Azure Maps-Websteuerelement](media/about-azure-maps/Introduction_WebMapControl.png)

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

Mit dem Zeitzonendienst können Sie aktuelle, vergangene und zukünftige Zeitzoneninformationen abfragen, indem Sie entweder Breitengrad/Längengrad-Paare oder eine [IANA-ID](https://www.iana.org/) verwenden. Außerdem können Sie mit dem Zeitzonendienst Microsoft Windows-Zeitzonen-IDs in IANA-Zeitzonen konvertieren und einen Zeitzonenoffset zu UTC und die aktuelle Zeit in einer entsprechenden Zeitzone abrufen. Eine typische JSON-Antwort für eine Abfrage mit dem Zeitzonendienst sieht wie folgt aus:

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

Der Dienst „IP to Location“ ermöglicht es Ihnen, eine Vorschau für den zweistelligen Ländercode für eine bestimmte IP-Adresse zu erstellen. Mithilfe dieses Diensts können Sie die Benutzeroberfläche anpassen und verbessern, indem Sie benutzerdefinierte Anwendungen basierend auf dem geografischen Standort einsetzen.

Informationen zu den REST-APIs für den IP to Location-Dienst finden Sie auf der Seite mit Informationen zu den [Geolocation-APIs von Azure Maps](https://docs.microsoft.com/rest/api/maps/geolocation).

## <a name="programming-model"></a>Programmiermodell

Azure Maps ist auf Mobilität ausgelegt und kann für plattformübergreifende Anwendungen eingesetzt werden. Es wird ein Programmiermodell genutzt, das sprachunabhängig ist und die JSON-Ausgabe über [REST-APIs](https://docs.microsoft.com/rest/api/maps/) unterstützt.

Außerdem verfügt Azure Maps über ein komfortables [JavaScript-Kartensteuerelement](https://docs.microsoft.com/javascript/api/azure-maps-control) mit einem einfachen Programmiermodell für die schnelle und einfache Entwicklung webbasierter und mobiler Anwendungen.

## <a name="usage"></a>Verwendung

Für den Zugriff auf die Maps-Dienste navigieren Sie zum [Azure-Portal](https://portal.azure.com) und erstellen ein Azure Maps-Konto.

Für Azure Maps wird ein schlüsselbasiertes Authentifizierungsschema verwendet. Für Ihr Konto wurden zwei Schlüssel vorab generiert. Beginnen Sie damit, diese Standortfunktionen in Ihre Anwendungen zu integrieren, indem Sie einen Ihrer Schlüssel verwenden und eine Anforderung an den Azure Maps-Dienst senden.

## <a name="supported-regions"></a>Unterstützte Regionen

Die Azure Maps-API ist derzeit in allen Ländern verfügbar, mit Ausnahme von:

* Argentinien
* China
* Indien
* Marokko
* Pakistan
* Südkorea

Vergewissern Sie sich, dass sich der Standort der aktuellen IP-Adresse nicht in einem der oben angegebenen nicht unterstützten Länder befindet.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den neuen Features von Azure Maps:

> [!div class="nextstepaction"]
> [Routenmatrix, Isochronen, IP-Suche und vieles mehr](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/)

Testen Sie als Nächstes eine Beispiel-App, mit der der Dienst veranschaulicht wird:

> [!div class="nextstepaction"]
> [Demo der interaktiven Kartensuche starten](quick-demo-map-app.md)

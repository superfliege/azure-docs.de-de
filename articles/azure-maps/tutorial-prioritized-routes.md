---
title: Mehrere Routen mit Azure Maps | Microsoft-Dokumentation
description: Ermitteln von Routen für verschiedene Fortbewegungsarten per Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 10/02/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 340bf83f07b9e730cc43baccc60a39f5ba1f9942
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815306"
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-maps"></a>Ermitteln von Routen für verschiedene Fortbewegungsarten per Azure Maps

In diesem Tutorial wird veranschaulicht, wie Sie Ihr Azure Maps-Konto und den Routendienst verwenden, um die Route zum Point of Interest je nach Fortbewegungsart zu ermitteln. Sie zeigen zwei verschiedene Routen auf Ihrer Karte an, eine für PKWs und eine für LKWs, die aufgrund von Höhe, Gewicht oder Gefahrgut Routeneinschränkungen haben können. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Visualisieren des Verkehrsflusses auf Ihrer Karte
> * Erstellen von Routenanfragen, die die Fortbewegungsart deklarieren
> * Anzeigen mehrerer Routen auf Ihrer Karte

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie vor dem Fortfahren die Schritte zum [Erstellen eines Azure Maps-Kontos](./tutorial-search-location.md#createaccount) und [Abrufen des Abonnementschlüssels für Ihr Konto](./tutorial-search-location.md#getkey) im ersten Tutorial aus.

## <a name="create-a-new-map"></a>Erstellen einer neuen Karte

Die folgenden Schritte veranschaulichen, wie Sie eine statische HTML-Seite erstellen, in die die Kartensteuerelement-API eingebettet ist.

1. Erstellen Sie auf dem lokalen Computer eine neue Datei, und nennen Sie sie **MapTruckRoute.html**.
2. Fügen Sie der Datei die folgenden HTML-Komponenten hinzu:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.min.js?api-version=1"></script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #map {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>

    <body>
        <div id="map"></div>
        <script>
            // Embed Map Control JavaScript code here
        </script>
    </body>

    </html>
    ```
    Über den HTML-Header werden die Ressourcenspeicherorte für CSS- und JavaScript-Dateien für die Azure Maps-Bibliothek eingebettet. Das *script*-Segment im Text der HTML-Datei enthält den JavaScript-Inlinecode für die Karte.
3. Fügen Sie dem *script*-Block der HTML-Datei den folgenden JavaScript-Code hinzu. Ersetzen Sie die Zeichenfolge **\<your account key\>** durch den Primärschlüssel, den Sie aus Ihrem Maps-Konto kopiert haben. Wenn Sie keine Angaben darüber machen, wo der Fokus auf der Karte liegen soll, wird die Weltansicht angezeigt. Dieser Code setzt den Mittelpunkt der Karte und deklariert einen Zoomfaktor, sodass der Fokus standardmäßig auf einen bestimmten Bereich gelegt wird.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
         center: [-118.2437, 34.0522],
         zoom: 12
    });
    ```
    Mit **atlas.Map** – einer Komponente der Azure-Kartensteuerelement-API – wird das Steuerelement für eine visuelle und interaktive Webkarte bereitgestellt.

4. Speichern Sie die Datei, und öffnen Sie sie im Browser. Nun besitzen Sie eine einfache Karte, die Sie weiter anpassen können. 

   ![Anzeigen der einfachen Karte](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Visualisieren des Verkehrsflusses

1. Fügen Sie die Verkehrsflussanzeige der Karte hinzu.  Mit **map.addEventListener** wird sichergestellt, dass alle Kartenfunktionen, die der Karte hinzugefügt werden, erst nach dem vollständigen Laden der Karte geladen werden.

    ```JavaScript
    map.addEventListener("load", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```
    Mit diesem Code wird der Verkehrsfluss auf `relative` festgelegt. Dies ist die Geschwindigkeit der Straße relativ zum freien Fluss. Sie können für die Geschwindigkeit der Straße auch `absolute` oder `relative-delay` festlegen. Hierbei wird angezeigt, wo die relative Geschwindigkeit vom freien Fluss abweicht.

2. Speichern Sie die Datei **MapTruckRoute.html**, und aktualisieren Sie die Seite in Ihrem Browser. Sie sollten die Straßen von Los Angeles mit ihren aktuellen Verkehrsdaten sehen.

   ![Anzeigen der Verkehrskarte](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="set-start-and-end-points"></a>Festlegen von Start- und Endpunkten

In diesem Tutorial legen Sie den Startpunkt auf eine fiktive Firma in Seattle namens Fabrikam und den Zielpunkt auf ein Microsoft-Büro fest.

1. Fügen Sie den folgenden JavaScript-Code hinzu, um die Pins für den Start- und Endpunkt der Route zu erstellen:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.356099, 47.580045]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Fabrikam, Inc.",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Microsoft",
        icon: "pin-blue"
    });
    ```
    Mit diesem Code werden zwei [GeoJSON-Objekte](https://en.wikipedia.org/wiki/GeoJSON) erstellt, die für den Start- bzw. Endpunkt der Route stehen.

2. Fügen Sie den folgenden JavaScript-Code hinzu, um der Karte den Start- und Endpunkt hinzuzufügen:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 100
    });
    
    map.addEventListener("load", function() { 
        // Add pins to the map for the start and end point of the route
        map.addPins([startPin, destinationPin], {
            name: "route-pins",
            textFont: "SegoeUi-Regular",
            textOffset: [0, -20]
        });
    });
    ```
    Mit dem Aufruf **map.setCameraBounds** wird das Kartenfenster gemäß den Koordinaten des Start- und Endpunkts angepasst. Mit **map.addEventListener** wird sichergestellt, dass alle Kartenfunktionen, die der Karte hinzugefügt werden, erst nach dem vollständigen Laden der Karte geladen werden. Mit der API **map.addPins** werden die Punkte dem Kartensteuerelement als visuelle Komponenten hinzugefügt.

3. Speichern Sie die Datei, und aktualisieren Sie Ihren Browser, damit die Stecknadeln auf Ihrer Karte angezeigt werden. Obwohl Sie Ihre Karte mit einem Mittelpunkt in Los Angeles deklariert haben, hat **map.setCameraBounds** die Ansicht verschoben, um den Start- und Endpunkt anzuzeigen.

   ![Anzeigen der Karte mit Start- und Endpunkten](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Rendern von Routen je nach Fortbewegungsart

In diesem Abschnitt wird veranschaulicht, wie Sie die Maps-Routendienst-API verwenden, um basierend auf der Fortbewegungsart mehrere Routen von einem bestimmten Startpunkt zu einem Ziel zu ermitteln. Der Routendienst stellt APIs zum Planen der *schnellsten*, *kürzesten*, *umweltfreundlichsten* oder *schönsten* Route zwischen zwei Orten in Abhängigkeit der aktuellen Verkehrslage bereit. Benutzer können zukünftige Routen planen, indem sie die umfassende Azure-Datenbank zum Verkehrsverlauf nutzen und die Routendauern für beliebige Tage und Uhrzeiten vorhersagen. Weitere Informationen finden Sie unter [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Abrufen von Wegbeschreibungen).  Alle folgenden Codeblöcke sollten **innerhalb des eventListener-Elements für das Laden der Karte** hinzugefügt werden, um sicherzustellen, dass sie erst nach dem vollständigen Laden der Karte geladen werden.

1. Fügen Sie der Karte zunächst eine neue Ebene (*LineString*) hinzu, um die Route anzuzeigen. In diesem Tutorial gibt es zwei verschiedene Routen, **car-route** und **truck-route**, jeweils mit einer eigenen Formatierung. Fügen Sie dem *script*-Block den folgenden JavaScript-Code hinzu:

    ```JavaScript
    // Place route layers on the map
    var carRouteLayerName = "car-route";
    map.addLinestrings([], {
        name: carRouteLayerName,
        color: "#B76DAB",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });

    var truckRouteLayerName = "truck-route";
    map.addLinestrings([], {
        name: truckRouteLayerName,
        color: "#2272B9",
        width: 9,
        cap: "round",
        join: "round",
        before: carRouteLayerName
    });
    ```

2. Fügen Sie dem *script*-Block den folgenden JavaScript-Code hinzu, um die Route für einen LKW anzufordern und die Ergebnisse in der Karte anzuzeigen:

    ```JavaScript
    // Instantiate the service client  
    var client = new atlas.service.Client(MapsAccountKey);

    // Construct the route query string
    var routeQuery = startPoint.coordinates[1] +
        "," +
        startPoint.coordinates[0] +
        ":" +
        destinationPoint.coordinates[1] +
        "," +
        destinationPoint.coordinates[0];

    // Execute the truck route query then add the route to the map once a response is received  
    client.route.getRouteDirections(routeQuery, {
        travelMode: "truck",
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: "USHazmatClass2"
    }).then(response => {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson
            .GeoJsonRouteDirectionsResponse(response);

        // Get the first in the array of routes and add it to the map
        map.addLinestrings([geoJsonResponse.getGeoJsonRoutes().features[0]], {
            name: truckRouteLayerName
        });
    });
    ```
    Mit dem obigen Codeausschnitt wird ein Dienstclient instanziiert und eine Routenabfragezeichenfolge erstellt. Anschließend wird der Azure Maps-Routingdienst über die [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections)-Methode abgefragt und die Antwort anschließend im GeoJSON-Format unter Verwendung von [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) analysiert. Als Nächstes wird ein Array mit Koordinaten für die zurückgegebene Route erstellt und der Ebene `truckRouteLayerName` der Karte hinzugefügt.

3. Fügen Sie den folgenden JavaScript-Code hinzu, um die Route für ein Auto anzufordern und die Ergebnisse anzuzeigen:

    ```JavaScript
    // Execute the car route query then add the route to the map once a response is received  
    client.route.getRouteDirections(routeQuery).then(response => {
        // Parse the response into GeoJSON
        var geoJsonResponse = new tlas.service.geojson
            .GeoJsonRouteDiraectionsResponse(response);

        // Get the first in the array of routes and add it to the map 
        map.addLinestrings([geoJsonResponse.getGeoJsonRoutes().features[0]], {
            name: carRouteLayerName
        });
    });
    ```
    In diesem Codeausschnitt wird die Abfrage der LKW-Route für einen PKW verwendet. Der Azure Maps-Routingdienst wird über die [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections)-Methode abgefragt und die Antwort anschließend im GeoJSON-Format unter Verwendung von [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) analysiert. Als Nächstes wird ein Array mit Koordinaten für die zurückgegebene Route erstellt und der Ebene `carRouteLayerName` der Karte hinzugefügt.

4. Speichern Sie die Datei **MapTruckRoute.html**, und aktualisieren Sie Ihren Browser, um das Ergebnis zu betrachten. Für eine erfolgreiche Verbindung mit den Maps-APIs sollten Sie eine Karte ähnlich der folgenden sehen.

    ![Priorisierte Routen mit dem Azure-Routendienst](./media/tutorial-prioritized-routes/prioritized-routes.png)

    Die LKW-Route wird in Blau und als stärkere Linie angezeigt, wohingegen die Autoroute eine violette, dünnere Linie ist. Die Autostrecke führt über den Lake Washington über die I-90, die durch Tunnel unter Wohngebieten führt und so die Gefahrgutfracht einschränkt. Die LKW-Route für den Frachttyp „USHazmatClass2“ verläuft daher ordnungsgemäß über eine andere Autobahn.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Visualisieren des Verkehrsflusses auf Ihrer Karte
> * Erstellen von Routenanfragen, die die Fortbewegungsart deklarieren
> * Anzeigen mehrerer Routen auf Ihrer Karte

Sie können auf das Codebeispiel für dieses Tutorial hier zugreifen:

> [Mehrere Routen mit Azure Maps](https://github.com/Azure-Samples/azure-maps-samples/blob/master/src/truckRoute.html)

Weitere Informationen zur Abdeckung und zu den Funktionen von Azure Maps finden Sie unter:

> [!div class="nextstepaction"]
> [Zoomfaktoren und Linienraster](zoom-levels-and-tile-grid.md)

Weitere Codebeispiele und eine Benutzeroberfläche für das interaktive Codieren:

> [!div class="nextstepaction"]
> [Verwenden des Kartensteuerelements](how-to-use-map-control.md)
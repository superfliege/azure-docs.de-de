---
title: Ermitteln einer Route mit Azure Maps | Microsoft-Dokumentation
description: Route zu einem Point of Interest mit Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 10/02/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 3bf1aa6d1b9bd65c28ef99ddbac71fb75daf99e7
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816717"
---
# <a name="route-to-a-point-of-interest-using-azure-maps"></a>Route zu einem Point of Interest mit Azure Maps

In diesem Tutorial wird veranschaulicht, wie Sie Ihr Azure Maps-Konto und das Routendienst-SDK verwenden, um die Route zum Point of Interest zu ermitteln. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Festlegen von Adressenkoordinaten
> * Abfragen des Routendiensts nach der Wegbeschreibung zum Point of Interest

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie vor dem Fortfahren die Schritte zum [Erstellen eines Azure Maps-Kontos](./tutorial-search-location.md#createaccount) und [Abrufen des Abonnementschlüssels für Ihr Konto](./tutorial-search-location.md#getkey) im vorherigen Tutorial aus.

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>Erstellen einer neuen Karte

Die folgenden Schritte veranschaulichen, wie Sie eine statische HTML-Seite erstellen, in die die Kartensteuerelement-API eingebettet ist.

1. Erstellen Sie auf dem lokalen Computer eine neue Datei, und nennen Sie sie **MapRoute.html**.
2. Fügen Sie der Datei die folgenden HTML-Komponenten hinzu:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css"/>
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
    Über den HTML-Header werden die Ressourcenspeicherorte für CSS- und JavaScript-Dateien für die Azure Maps-Bibliothek eingebettet. Das *script*-Segment im Text der HTML-Datei enthält den JavaScript-Inlinecode für den Zugriff auf die Azure Maps-APIs.

3. Fügen Sie dem *script*-Block der HTML-Datei den folgenden JavaScript-Code hinzu. Ersetzen Sie die Zeichenfolge **\<your account key\>** durch den Primärschlüssel, den Sie aus Ihrem Maps-Konto kopiert haben.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    Mit **atlas.Map** – einer Komponente der Azure-Kartensteuerelement-API – wird das Steuerelement für eine visuelle und interaktive Webkarte bereitgestellt.

4. Speichern Sie die Datei, und öffnen Sie sie im Browser. Nun besitzen Sie eine einfache Karte, die Sie weiter anpassen können.

   ![Anzeigen der einfachen Karte](./media/tutorial-route-location/basic-map.png)

## <a name="set-start-and-end-points"></a>Festlegen von Start- und Endpunkten

Legen Sie in diesem Tutorial als Startpunkt Microsoft und als Ziel eine Tankstelle in Seattle fest.

1. Fügen Sie im gleichen *script*-Block der Datei **MapRoute.html** den folgenden JavaScript-Code hinzu, um den Start- und Endpunkt für die Route zu erstellen:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Microsoft",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.3352, 47.61397]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Contoso Oil & Gas",
        icon: "pin-blue"
    });
    ```
    Mit diesem Code werden zwei [GeoJSON-Objekte](https://en.wikipedia.org/wiki/GeoJSON) erstellt, die für den Start- bzw. Endpunkt der Route stehen.

2. Fügen Sie den folgenden JavaScript-Code hinzu, um der Karte die Pins für den Start- und Endpunkt hinzuzufügen:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 50
    });

    map.addEventListener("load", function () { 
        // Add pins to the map for the start and end point of the route
        map.addPins([startPin, destinationPin], {
            name: "route-pins",
            textFont: "SegoeUi-Regular",
            textOffset: [0, -20]
        });
    });
    ```
    Mit **map.setCameraBounds** wird das Kartenfenster gemäß den Koordinaten des Start- und Endpunkts angepasst. Mit **map.addEventListener** wird sichergestellt, dass alle Kartenfunktionen, die der Karte hinzugefügt werden, erst nach dem vollständigen Laden der Karte geladen werden. Mit der API **map.addPins** innerhalb des Ereignislisteners werden die Punkte dem Kartensteuerelement als visuelle Komponenten hinzugefügt.

3. Speichern Sie die Datei **MapRoute.html**, und aktualisieren Sie den Browser. Die Karte ist nun auf Seattle zentriert, und Sie sehen die runde blaue Markierung für den Startpunkt und die blaue Markierung für das Ziel.

   ![Anzeigen der Karte mit markiertem Start- und Endpunkt](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Abrufen einer Wegbeschreibung

In diesem Abschnitt wird veranschaulicht, wie Sie die Routendienst-API von Maps verwenden, um die Route von einem bestimmten Startpunkt zu einem Ziel zu ermitteln. Der Routendienst stellt APIs zum Planen der *schnellsten*, *kürzesten*, *umweltfreundlichsten* oder *schönsten* Route zwischen zwei Orten bereit. Benutzer können zukünftige Routen planen, indem sie die umfassende Azure-Datenbank zum Verkehrsverlauf nutzen und die Routendauern für beliebige Tage und Uhrzeiten vorhersagen. Weitere Informationen finden Sie unter [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Abrufen von Wegbeschreibungen). Alle folgenden Funktionen sollten **innerhalb des eventListener-Elements für das Laden der Karte** hinzugefügt werden, um sicherzustellen, dass sie erst nach dem vollständigen Laden der Karte geladen werden.

1. Fügen Sie der Karte zunächst eine neue Ebene (*LineString*) hinzu, um die Route anzuzeigen. Fügen Sie dem *script*-Block den folgenden JavaScript-Code hinzu.

    ```JavaScript
    // Initialize the linestring layer for routes on the map
    var routeLinesLayerName = "routes";
    map.addLinestrings([], {
        name: routeLinesLayerName,
        color: "#2272B9",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });
    ```

2. Fügen Sie dem Skriptblock den folgenden Javascript-Code hinzu, um den Clientdienst zu instanziieren.
    ```JavaScript
    var client = new atlas.service.Client(MapsAccountKey);
    ```

3. Fügen Sie den folgenden Codeblock hinzu, um eine Routenabfragezeichenfolge zu erstellen.
    ```JavaScript
    // Construct the route query string
    var routeQuery = startPoint.coordinates[1] +
        "," +
        startPoint.coordinates[0] +
        ":" +
        destinationPoint.coordinates[1] +
        "," +
        destinationPoint.coordinates[0];
    ```

4. Fügen Sie zum Abrufen der Route den folgenden Codeblock zum Skript hinzu. Damit wird der Azure Maps-Routingdienst über die [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections)-Methode abgefragt und die Antwort anschließend im GeoJSON-Format unter Verwendung von [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes)analysiert. Alle Antwortlinien werden dann zur Karte hinzugefügt, um die Route zu rendern. Weitere Informationen finden Sie unter [Hinzufügen einer Linie zur Karte](./map-add-shape.md#addALine).

    ```JavaScript
    // Execute the query then add the route to the map once a response is received  
    client.route.getRouteDirections(routeQuery).then(response => {
         // Parse the response into GeoJSON
         var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

         // Get the first in the array of routes and add it to the map
         map.addLinestrings([geoJsonResponse.getGeoJsonRoutes().features[0]], {
             name: routeLinesLayerName
         });
    });
    ```

5. Speichern Sie die Datei **MapRoute.html**, und aktualisieren Sie den Webbrowser. Bei einer erfolgreichen Verbindungsherstellung mit den Maps-APIs sollte eine Karte angezeigt werden, die in etwa wie folgt aussieht:

    ![Azure-Kartensteuerelement und -Routendienst](./media/tutorial-route-location/map-route.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Festlegen von Adressenkoordinaten
> * Abfragen des Routendiensts nach der Wegbeschreibung zum Point of Interest

Das Codebeispiel für dieses Tutorial finden Sie hier:

> [Ermitteln einer Route mit Azure Maps](https://github.com/Azure-Samples/azure-maps-samples/blob/master/src/route.html)

Im nächsten Tutorial erfahren Sie, wie Sie eine Routenabfrage mit Einschränkungen wie Fortbewegungsart oder Frachttyp erstellen und anschließend mehrere Routen auf der gleichen Karte anzeigen.

> [!div class="nextstepaction"]
> [Ermitteln von Routen für verschiedene Fortbewegungsarten](./tutorial-prioritized-routes.md)

---
title: Ermitteln einer Route mit Azure Maps | Microsoft-Dokumentation
description: Route zu einem Point of Interest mit Azure Maps
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: fc5dfafec303a439d8a1092771fd2247ab305172
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601342"
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
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
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

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    Mit **map.setCameraBounds** wird das Kartenfenster gemäß den Koordinaten des Start- und Endpunkts angepasst. Mit der API **map.addPins** werden die Punkte dem Kartensteuerelement als visuelle Komponenten hinzugefügt.

7. Speichern Sie die Datei **MapRoute.html**, und aktualisieren Sie den Browser. Die Karte ist nun auf Seattle zentriert, und Sie sehen die runde blaue Markierung für den Startpunkt und die blaue Markierung für das Ziel.

   ![Anzeigen der Karte mit markiertem Start- und Endpunkt](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Abrufen einer Wegbeschreibung

In diesem Abschnitt wird veranschaulicht, wie Sie die Routendienst-API von Maps verwenden, um die Route von einem bestimmten Startpunkt zu einem Ziel zu ermitteln. Der Routendienst stellt APIs zum Planen der *schnellsten*, *kürzesten*, *umweltfreundlichsten* oder *schönsten* Route zwischen zwei Orten bereit. Benutzer können zukünftige Routen planen, indem sie die umfassende Azure-Datenbank zum Verkehrsverlauf nutzen und die Routendauern für beliebige Tage und Uhrzeiten vorhersagen. Weitere Informationen finden Sie unter [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Abrufen von Wegbeschreibungen).


1. Fügen Sie der Karte zunächst eine neue Ebene (*LineString*) hinzu, um die Route anzuzeigen. Fügen Sie dem *script*-Block den folgenden JavaScript-Code hinzu:

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

2. Erstellen Sie [XMLHttpRequest](https://xhr.spec.whatwg.org/), und fügen Sie einen Ereignishandler zum Analysieren der JSON-Antwort hinzu, die vom Maps-Routendienst gesendet wird. Dieser Code erstellt ein Array von Koordinaten für die Liniensegmente der Route und fügt der LineString-Ebene der Karte die Koordinaten hinzu. 

    ```JavaScript
    // Perform a request to the route service and draw the resulting route on the map
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(xhttp.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], { name: routeLinesLayerName });
        }
    };
    ```

3. Fügen Sie den folgenden Code hinzu, um die Abfrage zu erstellen und „XMLHttpRequest“ an den Maps-Routendienst zu senden:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "api-version=1.0";
    url += "&subscription-key=" + MapsAccountKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```

3. Speichern Sie die Datei **MapRoute.html**, und aktualisieren Sie den Webbrowser. Bei einer erfolgreichen Verbindungsherstellung mit den Maps-APIs sollte eine Karte angezeigt werden, die in etwa wie folgt aussieht: 

    ![Azure-Kartensteuerelement und -Routendienst](./media/tutorial-route-location/map-route.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Festlegen von Adressenkoordinaten
> * Abfragen des Routendiensts nach der Wegbeschreibung zum Point of Interest

Im nächsten Tutorial erfahren Sie, wie Sie eine Routenabfrage mit Einschränkungen wie Fortbewegungsart oder Frachttyp erstellen und anschließend mehrere Routen auf der gleichen Karte anzeigen. 

> [!div class="nextstepaction"]
> [Ermitteln von Routen für verschiedene Fortbewegungsarten](./tutorial-prioritized-routes.md)

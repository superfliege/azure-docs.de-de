---
title: Ermitteln einer Route mit Azure Location Based Services | Microsoft-Dokumentation
description: "Route zu einem Point of Interest in der Nähe mit Azure Location Based Services"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7303347444952d9c09dc6c04eea5b962e18729b4
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Route zu einem Point of Interest in der Nähe mit Azure Location Based Services

In diesem Tutorial wird veranschaulicht, wie Sie Ihr Azure Location Based Services-Konto und das Routendienst-SDK verwenden, um die Route zum Point of Interest zu ermitteln. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Beschaffen von Adressenkoordinaten
> * Abfragen des Routendiensts nach der Wegbeschreibung zum Point of Interest

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Fortfahren sicher, dass Sie [Ihr Azure Location Based Services-Konto erstellen](./tutorial-search-location.md#createaccount) und [den Abonnementschlüssel für Ihr Konto beschaffen](./tutorial-search-location.md#getkey). Sie können sich auch darüber informieren, wie Sie die Kartensteuerelement- und Suchdienst-APIs einsetzen können, die im Tutorial [Suchen nach einem Point of Interest in der Nähe mit Azure Location Based Services](./tutorial-search-location.md) beschrieben werden.


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Beschaffen von Adressenkoordinaten

Führen Sie die folgenden Schritte zur Erstellung einer statischen HTML-Seite aus, in die die Kartensteuerelement-API von Location Based Services eingebettet ist. 

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
    Beachten Sie, wie über den HTML-Header die Ressourcenspeicherorte für CSS- und JavaScript-Dateien für die Azure Location Based Services-Bibliothek eingebettet werden. Beachten Sie außerdem das *script*-Segment im Text der HTML-Datei, das den JavaScript-Inlinecode für den Zugriff auf die APIs von Azure Location Based Services enthält.

3. Fügen Sie dem *script*-Block der HTML-Datei den folgenden JavaScript-Code hinzu. Verwenden Sie den Primärschlüssel Ihres Location Based Services-Kontos im Skript.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    Mit **atlas.Map** – einer Komponente der Azure-Kartensteuerelement-API – wird das Steuerelement für eine visuelle und interaktive Webkarte bereitgestellt.

4. Fügen Sie dem *script*-Block den folgenden JavaScript-Code hinzu. Hierdurch wird dem Kartensteuerelement zum Anzeigen der Route eine Ebene mit *linestrings* hinzugefügt:

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

5. Fügen Sie den folgenden JavaScript-Code hinzu, um den Start- und Endpunkt für die Route zu erstellen:

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
    Mit diesem Code werden zwei [GeoJSON-Objekte](https://en.wikipedia.org/wiki/GeoJSON) erstellt, die für den Start- bzw. Endpunkt der Route stehen. Der Endpunkt ist die Kombination aus Breiten- und Längengrad für eine der *Tankstellen*, nach denen im vorherigen Tutorial [Suchen nach einem Point of Interest in der Nähe mit Azure Location Based Services](./tutorial-search-location.md) gesucht wurde.

6. Fügen Sie den folgenden JavaScript-Code hinzu, um der Karte die Pins für den Start- und Endpunkt hinzuzufügen:

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
    Mit der API **map.setCameraBounds** wird das Kartenfenster gemäß den Koordinaten des Start- und Endpunkts angepasst. Mit der API **map.addPins** werden die Punkte dem Kartensteuerelement als visuelle Komponenten hinzugefügt.

7. Speichern Sie die Datei **MapRoute.html** auf Ihrem Computer. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>Abfragen des Routendiensts nach der Wegbeschreibung zum Point of Interest

In diesem Abschnitt wird veranschaulicht, wie Sie die Routendienst-API von Azure Location Based Services verwenden, um die Routen von einem bestimmten Startpunkt zu einem Ziel zu ermitteln. Der Routendienst stellt APIs zum Planen der schnellsten, kürzesten oder umweltfreundlichsten Route zwischen zwei Orten in Abhängigkeit der Echtzeit-Verkehrslage bereit. Benutzer können zukünftige Routen planen, indem sie die umfassende Azure-Datenbank zum Verkehrsverlauf nutzen und die Routendauern für beliebige Tage und Uhrzeiten vorhersagen. 

1. Öffnen Sie die im vorherigen Abschnitt erstellte Datei **MapRoute.html**, und fügen Sie dem *script*-Block den folgenden JavaScript-Code hinzu, um den Routendienst darzustellen.

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
    Mit diesem Codeausschnitt wird ein [XMLHttpRequest](https://xhr.spec.whatwg.org/)-Element erstellt und ein Ereignishandler zum Analysieren der eingehenden Antwort hinzugefügt. Bei einer erfolgreichen Antwort wird ein Array mit Koordinaten für Zeilensegmente der ersten zurückgegebenen Route erstellt. Anschließend werden diese Koordinaten für die Route der Ebene *linestrings* der Karte hinzugefügt.

2. Fügen Sie dem *script*-Block den folgenden Code hinzu, um „XMLHttpRequest“ an den Azure Location Based Services-Routendienst zu senden:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "&api-version=1.0";
    url += "&subscription-key=" + LBSAccountKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```
    In der Anforderung oben werden die erforderlichen Parameter angezeigt, also Ihr Kontoschlüssel und die Koordinaten für Start- und Endpunkt in der angegebenen Reihenfolge. 

3. Speichern Sie die Datei **MapRoute.html** lokal, öffnen Sie sie in einem Webbrowser Ihrer Wahl, und sehen Sie sich das Ergebnis an. Bei einer erfolgreichen Verbindungsherstellung mit den APIs von Location Based Services sollte eine Karte angezeigt werden, die in etwa wie folgt aussieht: 

    ![Azure-Kartensteuerelement und -Routendienst](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Beschaffen von Adressenkoordinaten
> * Abfragen des Routendiensts nach der Wegbeschreibung zum Point of Interest

Informieren Sie sich als Nächstes im Tutorial [Ermitteln von Routen für verschiedene Fortbewegungsarten per Azure Location Based Services](./tutorial-prioritized-routes.md) darüber, wie Sie Azure Location Based Services verwenden, um Routen zum Point of Interest basierend auf der Fortbewegungsart zu priorisieren. 

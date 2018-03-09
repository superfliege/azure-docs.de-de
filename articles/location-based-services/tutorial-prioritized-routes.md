---
title: Mehrere Routen mit Azure Location Based Services | Microsoft-Dokumentation
description: "Ermitteln von Routen für verschiedene Fortbewegungsarten per Azure Location Based Services"
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
ms.openlocfilehash: 9501f3c8fa1abb6bcbfee086c542139596ab5504
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Ermitteln von Routen für verschiedene Fortbewegungsarten per Azure Location Based Services

In diesem Tutorial wird veranschaulicht, wie Sie Ihr Azure Location Based Services-Konto und das Routendienst-SDK verwenden, um die Route zum Point of Interest je nach Fortbewegungsart zu ermitteln. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren der Routendienstabfrage
> * Rendern von Routen je nach Fortbewegungsart

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Fortfahren sicher, dass Sie [Ihr Azure Location Based Services-Konto erstellen](./tutorial-search-location.md#createaccount) und [einen Schlüssel für Ihr Konto beschaffen](./tutorial-search-location.md#getkey). Sie können sich auch darüber informieren, wie Sie die Kartensteuerelement- und Suchdienst-APIs einsetzen können, die im Tutorial [Suchen nach einem Point of Interest in der Nähe mit Azure Location Based Services](./tutorial-search-location.md) beschrieben wurden. Im Tutorial [Route zu einem Point of Interest in der Nähe mit Azure Location Based Services](./tutorial-route-location.md) werden die Grundlagen der Nutzung von Routendienst-APIs beschrieben.


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Konfigurieren der Routendienstabfrage

Führen Sie die folgenden Schritte zur Erstellung einer statischen HTML-Seite aus, in die die Kartensteuerelement-API von Location Based Services eingebettet ist. 

1. Erstellen Sie auf dem lokalen Computer eine neue Datei, und nennen Sie sie **MapTruckRoute.html**. 
2. Fügen Sie der Datei die folgenden HTML-Komponenten hinzu:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
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
    Beachten Sie, dass über den HTML-Header die Ressourcenspeicherorte für CSS- und JavaScript-Dateien für die Azure Location Based Services-Bibliothek eingebettet werden. Beachten Sie außerdem, dass das *script*-Segment, das dem Text des HTML-Elements hinzugefügt wurde, den JavaScript-Inlinecode für den Zugriff auf die Azure-Kartensteuerelement-API enthält.
3. Fügen Sie dem *script*-Block der HTML-Datei den folgenden JavaScript-Code hinzu. Ersetzen Sie den Platzhalter *<insert-key>* durch den Primärschlüssel des Location Based Services-Kontos.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    Mit **atlas.Map** – einer Komponente der Azure-Kartensteuerelement-API – wird das Steuerelement für eine visuelle und interaktive Webkarte bereitgestellt.

4. Fügen Sie den folgenden JavaScript-Code dem *script*-Block hinzu, um der Karte die Anzeige mit dem Datenverkehrsfluss hinzuzufügen:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Mit diesem Code wird der Verkehrsfluss auf `relative` festgelegt. Dies ist die Geschwindigkeit der Straße relativ zum freien Fluss. Sie können für die Geschwindigkeit der Straße auch `absolute` oder `relative-delay` festlegen. Hierbei wird angezeigt, wo die relative Geschwindigkeit vom freien Fluss abweicht. 

5. Fügen Sie den folgenden JavaScript-Code hinzu, um die Pins für den Start- und Endpunkt der Route zu erstellen:

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

6. Fügen Sie den folgenden JavaScript-Code hinzu, um dem Kartensteuerelement Ebenen mit *linestrings* hinzuzufügen, damit Routen basierend auf der Fortbewegungsart angezeigt werden können, z.B. _car_ (PKW) und _truck_ (LKW).

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

7. Fügen Sie den folgenden JavaScript-Code hinzu, um der Karte den Start- und Endpunkt hinzuzufügen:

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

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    Mit der API **map.setCameraBounds** wird das Kartenfenster gemäß den Koordinaten des Start- und Endpunkts angepasst. Mit der API **map.addPins** werden die Punkte dem Kartensteuerelement als visuelle Komponenten hinzugefügt.

8. Speichern Sie die Datei **MapTruckRoute.html** auf Ihrem Computer. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Rendern von Routen je nach Fortbewegungsart

In diesem Abschnitt wird veranschaulicht, wie Sie die Routendienst-API von Azure Location Based Services verwenden, um basierend auf der Fortbewegungsart mehrere Routen von einem bestimmten Startpunkt zu einem Ziel zu ermitteln. Der Routendienst stellt APIs zum Planen der schnellsten, kürzesten oder umweltfreundlichsten Route zwischen zwei Orten in Abhängigkeit der Echtzeit-Verkehrslage bereit. Benutzer können zukünftige Routen planen, indem sie die umfassende Azure-Datenbank zum Verkehrsverlauf nutzen und die Routendauern für beliebige Tage und Uhrzeiten vorhersagen. 

1. Öffnen Sie die im vorherigen Abschnitt erstellte Datei **MapTruckRoute.html**, und fügen Sie dem *script*-Block den folgenden JavaScript-Code hinzu, um die Route für einen LKW per Routendienst zu erhalten.

    ```JavaScript
    // Perform a request to the route service and draw the resulting truck route on the map
    var xhttpTruck = new XMLHttpRequest();
    xhttpTruck.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: truckRouteLayerName
            });
        }
    };

    var truckRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    truckRouteUrl += "&api-version=1.0";
    truckRouteUrl += "&subscription-key=" + LBSAccountKey;
    truckRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];
    truckRouteUrl += "&travelMode=truck";
    truckRouteUrl += "&vehicleWidth=2";
    truckRouteUrl += "&vehicleHeight=2";
    truckRouteUrl += "&vehicleLength=5";
    truckRouteUrl += "&vehicleLoadType=USHazmatClass2";

    xhttpTruck.open("GET", truckRouteUrl, true);
    xhttpTruck.send();
    ```
    Mit diesem Codeausschnitt wird ein [XMLHttpRequest](https://xhr.spec.whatwg.org/)-Element erstellt und ein Ereignishandler zum Analysieren der eingehenden Antwort hinzugefügt. Bei einer erfolgreichen Antwort wird ein Array mit Koordinaten für die zurückgegebene Route erstellt und der Ebene `truckRouteLayerName` der Karte hinzugefügt. 
    
    Mit diesem Codeausschnitt wird die Abfrage außerdem an den Routendienst für Ihren Kontoschlüssel gesendet, um die Route für den angegebenen Start- und Endpunkt zu erhalten. Die folgenden optionalen Parameter werden verwendet, um die Route für einen schweren LKW anzugeben:
   - Mit dem Parameter `travelMode=truck` wird die Fortbewegungsart als *truck* (LKW) angegeben. Andere unterstützte Fortbewegungsarten sind *taxi*, *bus*, *van*, *motorcycle* und *car* (Standardeinstellung).
   - Mit den Parametern `vehicleWidth`, `vehicleHeight` und `vehicleLength` werden die Abmessungen des Fahrzeugs in Metern angegeben. Sie werden nur berücksichtigt, wenn die Fortbewegungsart *truck* (LKW) lautet.
   - Mit `vehicleLoadType` wird die Ladung als gefährlich mit ggf. eingeschränkter Nutzung einiger Straßen eingestuft. Derzeit gilt dies nur für den Modus *truck* (LKW).

2. Fügen Sie den folgenden JavaScript-Code hinzu, um die Route für einen PKW per Routendienst zu erhalten:

    ```JavaScript
    // Perform a request to the route service and draw the resulting car route on the map
    var xhttpCar = new XMLHttpRequest();
    xhttpCar.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: carRouteLayerName
            });
        }
    };

    var carRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    carRouteUrl += "&api-version=1.0";
    carRouteUrl += "&subscription-key=" + LBSAccountKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Mit diesem Codeausschnitt wird ein weiteres [XMLHttpRequest](https://xhr.spec.whatwg.org/)-Element erstellt und ein Ereignishandler zum Analysieren der eingehenden Antwort hinzugefügt. Bei einer erfolgreichen Antwort wird ein Array mit Koordinaten für die zurückgegebene Route erstellt und der Ebene `carRouteLayerName` der Karte hinzugefügt. 
    
    Mit diesem Codeausschnitt wird die Abfrage außerdem an den Routendienst für den Abonnementschlüssel Ihres Kontos gesendet, um die Route für den angegebenen Start- und Endpunkt zu erhalten. Da keine anderen Parameter verwendet werden, wird die Route für die Standardfortbewegungsart *car* (PKW) zurückgegeben. 

3. Speichern Sie die Datei **MapTruckRoute.html** lokal, öffnen Sie sie in einem Webbrowser Ihrer Wahl, und sehen Sie sich das Ergebnis an. Bei einer erfolgreichen Verbindungsherstellung mit den APIs von Location Based Services sollte eine Karte angezeigt werden, die in etwa wie folgt aussieht: 

    ![Priorisierte Routen mit dem Azure-Routendienst](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Beachten Sie, dass die LKW-Route blau dargestellt ist, während die PKW-Route violett ist.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren der Routendienstabfrage
> * Rendern von Routen je nach Fortbewegungsart

Fahren Sie mit den Artikeln zu den **Konzepten** und zur **Anleitung** fort, um sich eingehender mit dem Azure Location Based Services-SDK vertraut zu machen. 

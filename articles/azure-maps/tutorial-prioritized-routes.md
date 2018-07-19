---
title: Mehrere Routen mit Azure Maps | Microsoft-Dokumentation
description: Ermitteln von Routen für verschiedene Fortbewegungsarten per Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 83ca46ecb8f0cce2ff8c749016eb3ad1ac7df7cf
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988968"
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
    Über den HTML-Header werden die Ressourcenspeicherorte für CSS- und JavaScript-Dateien für die Azure Maps-Bibliothek eingebettet. Das *script*-Segment im Text der HTML-Datei enthält den JavaScript-Inlinecode für die Karte.
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

   ![Anzeigen der einfachen Karte](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Visualisieren des Verkehrsflusses

1. Wenn Sie keine Angaben darüber machen, wo der Fokus auf der Karte liegen soll, wird die Weltansicht angezeigt. Um die Verkehrsdaten sehen zu können, legen Sie einen Mittelpunkt und einen Zoomfaktor für Ihre Karte fest. Ersetzen Sie den Code, der eine `new atlas.Map` mit den folgenden JavaScript-Code deklariert: 
    
    ```JavaScript
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey,
        center: [-118.2437,34.0522],
        zoom: 12
    });
    ```

    Dieser Code setzt den Mittelpunkt der Karte und deklariert einen Zoomfaktor, sodass der Fokus standardmäßig auf einen bestimmten Bereich gelegt wird. 

1. Fügen Sie die Verkehrsflussanzeige zur Karte hinzu:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
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

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    Mit dem Aufruf **map.setCameraBounds** wird das Kartenfenster gemäß den Koordinaten des Start- und Endpunkts angepasst. Mit der API **map.addPins** werden die Punkte dem Kartensteuerelement als visuelle Komponenten hinzugefügt.

3. Speichern Sie die Datei, und aktualisieren Sie Ihren Browser, damit die Stecknadeln auf Ihrer Karte angezeigt werden. Obwohl Sie Ihre Karte mit einem Mittelpunkt in Los Angeles deklariert haben, hat **map.setCameraBounds** die Ansicht verschoben, um den Start- und Endpunkt anzuzeigen. 

   ![Anzeigen der Karte mit Start- und Endpunkten](./media/tutorial-prioritized-routes/pins-map.png)


<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Rendern von Routen je nach Fortbewegungsart

In diesem Abschnitt wird veranschaulicht, wie Sie die Maps-Routendienst-API verwenden, um basierend auf der Fortbewegungsart mehrere Routen von einem bestimmten Startpunkt zu einem Ziel zu ermitteln. Der Routendienst stellt APIs zum Planen der *schnellsten*, *kürzesten*, *umweltfreundlichsten* oder *schönsten* Route zwischen zwei Orten in Abhängigkeit der aktuellen Verkehrslage bereit. Benutzer können zukünftige Routen planen, indem sie die umfassende Azure-Datenbank zum Verkehrsverlauf nutzen und die Routendauern für beliebige Tage und Uhrzeiten vorhersagen. Weitere Informationen finden Sie unter [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Abrufen von Wegbeschreibungen).


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
    truckRouteUrl += "&subscription-key=" + MapsAccountKey;
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
    
    Dieser Codeausschnitt erstellt auch die Abfrage für den Maps-Routendienst unter Verwendung Ihres Kontoschlüssels. Die Abfrage enthält die Startpunktkoordinaten, Endpunktkoordinaten und optionale Parameter, um anzuzeigen, dass es sich um eine Route für einen Schwerlaster handelt.

2. Fügen Sie den folgenden JavaScript-Code hinzu, um die Route für ein Auto anzufordern und die Ergebnisse anzuzeigen:

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
    carRouteUrl += "&subscription-key=" + MapsAccountKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Mit diesem Codeausschnitt wird ein [XMLHttpRequest](https://xhr.spec.whatwg.org/)-Element erstellt und ein Ereignishandler zum Analysieren der eingehenden Antwort hinzugefügt. Bei einer erfolgreichen Antwort wird ein Array mit Koordinaten für die zurückgegebene Route erstellt und der Ebene `carRouteLayerName` der Karte hinzugefügt. 
    
    Dieser Codeausschnitt erstellt auch die Abfrage für den Maps-Routendienst unter Verwendung Ihres Kontoschlüssels. Die Abfrage umfasst die Start- und Endpunktkoordinaten. Da keine zusätzlichen Parameter zur Verfügung gestellt werden, ist der Routendienst standardmäßig auf den Reisemodus *Auto* eingestellt. 

3. Speichern Sie die Datei **MapTruckRoute.html**, und aktualisieren Sie Ihren Browser, um das Ergebnis zu betrachten. Für eine erfolgreiche Verbindung mit den Maps-APIs sollten Sie eine Karte ähnlich der folgenden sehen. 

    ![Priorisierte Routen mit dem Azure-Routendienst](./media/tutorial-prioritized-routes/prioritized-routes.png)

    Die LKW-Route wird in Blau und als stärkere Linie angezeigt, wohingegen die Autoroute eine violette, dünnere Linie ist. Die Autostrecke führt über den Lake Washington über die I-90, die durch Tunnel unter Wohngebieten führt und so die Gefahrgutfracht einschränkt. Die LKW-Route für den Frachttyp „USHazmatClass2“ verläuft daher ordnungsgemäß über eine andere Autobahn. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Visualisieren des Verkehrsflusses auf Ihrer Karte
> * Erstellen von Routenanfragen, die die Fortbewegungsart deklarieren
> * Anzeigen mehrerer Routen auf Ihrer Karte

Weitere Informationen zu der Abdeckung und den Funktionen von Azure Maps finden Sie unter [Zoomfaktoren und Linienraster](zoom-levels-and-tile-grid.md) und den anderen Konzeptartikeln. 

Weitere Codebeispiele und eine interaktive Oberfläche für die Codierung finden Sie unter [Verwenden des Kartensteuerelements](how-to-use-map-control.md) und den anderen Artikeln mit entsprechenden Anleitungen. 

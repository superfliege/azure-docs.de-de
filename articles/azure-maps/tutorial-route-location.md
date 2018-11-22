---
title: Ermitteln einer Route mit Azure Maps | Microsoft-Dokumentation
description: Route zu einem Point of Interest mit Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 11/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a3807dc792c2e56c3e7c1b74f7d3e8f73ac0f4b0
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705088"
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
    <html>
    <head>
        <title>Map Route</title>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
        
        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>
        
        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
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
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```
    
    Beachten Sie, dass der HTML-Header die CSS- und JavaScript-Ressourcendateien enthält, die von der Azure-Kartensteuerelement-Bibliothek gehostet werden. Beachten Sie das Ereignis `onload` im Textkörper der Seite. Dieses Ereignis ruft die Funktion `GetMap` auf, wenn der Textkörper der Seite geladen wurde. Diese Funktion enthält den JavaScript-Inlinecode für den Zugriff auf die Azure Maps-APIs. 

3. Fügen Sie der Funktion `GetMap` den folgenden JavaScript-Code hinzu. Ersetzen Sie die Zeichenfolge **\<Your Azure Maps Key\>** durch den Primärschlüssel, den Sie aus Ihrem Maps-Konto kopiert haben.

    ```JavaScript
    //Add your Azure Maps subscription key to the map SDK. 
    atlas.setSubscriptionKey('<Your Azure Maps Key>');

    //Initialize a map instance.
    map = new atlas.Map('myMap');
    ```

    Mit `atlas.Map` – einer Komponente der Azure-Kartensteuerelement-API – wird das Steuerelement für eine visuelle und interaktive Webkarte bereitgestellt.

4. Speichern Sie die Datei, und öffnen Sie sie im Browser. Nun besitzen Sie eine einfache Karte, die Sie weiter anpassen können.

   ![Anzeigen der einfachen Karte](./media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Definieren, wie die Route dargestellt werden soll

In diesem Tutorial wird eine einfache Route mit einem Symbol für Anfang und Ende und einer Linie für den Routenverlauf dargestellt.

1. Fügen Sie in der Funktion „GetMap“ nach der Karteninitialisierung den folgenden JavaScript-Code hinzu:

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: '#2272B9',
            strokeWidth: 5,
            lineJoin: 'round',
            lineCap: 'round',
            filter: ['==', '$type', 'LineString']
        }), 'labels');

        //Add a layer for rendering point data.
        map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: ['get', 'icon'],
                allowOverlap: true
           },
            textOptions: {
                textField: ['get', 'title'],
                offset: [0, 1.2]
            },
            filter: ['==', '$type', 'Point']
        }));
    });
    ```
    
    Der Karte wird ein Ladeereignis hinzugefügt, das ausgelöst wird, wenn die Kartenressourcen vollständig geladen wurden. Im Ereignishandler für das Laden der Karte wird eine Datenquelle zum Speichern der Routenlinie und der Start- und Endpunkte erstellt. Eine Linienebene wird erstellt und an die Datenquelle angefügt, um zu definieren, wie die Routenlinie dargestellt werden soll. Die Routenlinie wird in einem hübschen Blauton mit einer Stärke von 5 Pixeln sowie abgerundeten Linienverbindungen und -abschlüssen gerendert. Durch Hinzufügen eines Filters wird sichergestellt, dass auf dieser Ebene nur GeoJSON-Daten vom Typ „LineString“ gerendert werden. Beim Hinzufügen der Ebene zur Karte wird ein zweiter Parameter mit dem Wert `'labels'` übergeben. Dieser gibt an, dass diese Ebene unterhalb der Kartenbeschriftungen gerendert werden soll. Dadurch wird sichergestellt, dass die Routenlinie keine Straßenbezeichnungen verdeckt. Eine Symbolebene wird erstellt und an die Datenquelle angefügt. Diese Ebene gibt an, wie die Start- und Endpunkte gerendert werden sollen. In diesem Fall wurden Ausdrücke hinzugefügt, um das Symbol und die Beschriftungsinformationen aus Eigenschaften für das jeweilige Punktobjekt abzurufen. 
    
2. Legen Sie in diesem Tutorial als Startpunkt Microsoft und als Endpunkt eine Tankstelle in Seattle fest. Fügen Sie im Ereignishandler für das Laden der Karte den folgenden Code hinzu:

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: 'Microsoft',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: 'Contoso Oil & Gas',
        icon: 'pin-blue'
    });    
    ```

    Mit diesem Code werden zwei [GeoJSON-Punktobjekte](https://en.wikipedia.org/wiki/GeoJSON) erstellt, die für den Start- bzw. Endpunkt der Route stehen. Den Punkten wird jeweils eine Eigenschaft vom Typ `title` und `icon` hinzugefügt.
    
3. Fügen Sie als Nächstes den folgenden JavaScript-Code hinzu, um der Karte die Pins für die Start- und Endpunkte hinzuzufügen:

    ```JavaScript
    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);
    
    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });
    ```
    
    Die Start- und Endpunkte werden der Datenquelle hinzugefügt. Das umgebende Rechteck für die Start- und Endpunkte wird mithilfe der Funktion `atlas.data.BoundingBox.fromData` berechnet. Dieses umgebende Rechteck dient dazu, die Kameraansicht der Karte mithilfe der Funktion **map.setCamera** auf dem Start- bzw. Endpunkt zu platzieren. Zur Kompensierung der Pixeldimensionen der Symbole wird ein Abstand hinzugefügt.

3. Speichern Sie die Datei **MapRoute.html**, und aktualisieren Sie den Browser. Die Karte ist nun auf Seattle zentriert, und Sie sehen die runde blaue Markierung für den Startpunkt und die blaue Markierung für das Ziel.

   ![Anzeigen der Karte mit markiertem Start- und Endpunkt](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Abrufen einer Wegbeschreibung

In diesem Abschnitt wird veranschaulicht, wie Sie die Routendienst-API von Maps verwenden, um die Route von einem bestimmten Startpunkt zu einem Ziel zu ermitteln. Der Routendienst stellt APIs zum Planen der *schnellsten*, *kürzesten*, *umweltfreundlichsten* oder *schönsten* Route zwischen zwei Orten bereit. Benutzer können zukünftige Routen planen, indem sie die umfassende Azure-Datenbank zum Verkehrsverlauf nutzen und die Routendauern für beliebige Tage und Uhrzeiten vorhersagen. Weitere Informationen finden Sie unter [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Abrufen von Wegbeschreibungen). Alle folgenden Funktionen sollten **innerhalb des eventListener-Elements für das Laden der Karte** hinzugefügt werden, um sicherzustellen, dass sie erst nach dem vollständigen Laden der Karte geladen werden.

1. Instanziieren Sie den Dienstclient, indem Sie dem Ereignishandler für das Laden der Karte den folgenden JavaScript-Code hinzufügen:

    ```JavaScript
    //If the service client hasn't already been created, create an instance.
    if (!client) {
        client = new atlas.service.Client(atlas.getSubscriptionKey());
    }
    ```

2. Fügen Sie den folgenden Codeblock hinzu, um eine Routenabfragezeichenfolge zu erstellen.
    ```JavaScript
    //Create the route request with the query being the start and end point in the format 'startLongitude,startLatitude:endLongitude,endLatitude'.
    var routeQuery = startPoint.geometry.coordinates[1] +
        ',' +
        startPoint.geometry.coordinates[0] +
        ':' +
        endPoint.geometry.coordinates[1] +
        ',' +
        endPoint.geometry.coordinates[0];
    ```

3. Fügen Sie zum Abrufen der Route den folgenden Codeblock zum Skript hinzu. Damit wird der Azure Maps-Routingdienst über die [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections)-Methode abgefragt und die Antwort anschließend im GeoJSON-Format unter Verwendung von [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes)analysiert. Anschließend wird die Routenlinie aus der Antwort der Datenquelle hinzugefügt und automatisch auf der Karte gerendert.

    ```JavaScript
    //Execute the car route query then add the route to the map once a response is received.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Add the route line to the data source.
        datasource.add(geoJsonResponse.getGeoJsonRoutes().features[0]);
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

> [Ermitteln einer Route mit Azure Maps](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)

[Hier](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination) können Sie sich das Beispiel live ansehen.

Im nächsten Tutorial erfahren Sie, wie Sie eine Routenabfrage mit Einschränkungen wie Fortbewegungsart oder Frachttyp erstellen und anschließend mehrere Routen auf der gleichen Karte anzeigen.

> [!div class="nextstepaction"]
> [Ermitteln von Routen für verschiedene Fortbewegungsarten](./tutorial-prioritized-routes.md)

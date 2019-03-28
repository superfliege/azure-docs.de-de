---
title: Ermitteln einer Route mit Azure Maps | Microsoft-Dokumentation
description: Route zu einem Point of Interest mit Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 5c5465562c1af3dbd3fcaff2031149e510a43cfd
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540736"
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
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=2" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=2"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=2"></script>

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

            #myMap {
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
   //Instantiate a map object
   var map = new atlas.Map("myMap", {
        //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
        authOptions: {
           authType: 'subscriptionKey',
           subscriptionKey: '<Your Azure Maps Key>'
        }
   });
   ```

    Mit **atlas.Map** – einer Komponente der Azure-Kartensteuerelement-API – wird das Steuerelement für eine visuelle und interaktive Webkarte bereitgestellt.

4. Speichern Sie die Datei, und öffnen Sie sie im Browser. Nun besitzen Sie eine einfache Karte, die Sie weiter anpassen können.

   ![Anzeigen der einfachen Karte](./media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Definieren, wie die Route dargestellt werden soll

In diesem Tutorial wird eine einfache Route mit einem Symbol für Anfang und Ende und einer Linie für den Routenverlauf dargestellt.

1. Fügen Sie nach der Karteninitialisierung den folgenden JavaScript-Code hinzu:

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function() {

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

    Der Karte wird ein Ladeereignis hinzugefügt, das ausgelöst wird, wenn die Kartenressourcen vollständig geladen wurden. Im Ereignishandler für das Laden der Karte wird eine Datenquelle zum Speichern der Routenlinie und der Start- und Endpunkte erstellt. Eine Linienebene wird erstellt und an die Datenquelle angefügt, um zu definieren, wie die Routenlinie dargestellt werden soll. Die Routenlinie wird in einem Blauton mit einer Stärke von 5 Pixeln sowie abgerundeten Linienverbindungen und -abschlüssen gerendert. Durch Hinzufügen eines Filters wird sichergestellt, dass auf dieser Ebene nur GeoJSON-Daten vom Typ „LineString“ gerendert werden. Beim Hinzufügen der Ebene zur Karte wird ein zweiter Parameter mit dem Wert `'labels'` übergeben. Dieser gibt an, dass diese Ebene unterhalb der Kartenbeschriftungen gerendert werden soll. Dadurch wird sichergestellt, dass die Routenlinie keine Straßenbezeichnungen verdeckt. Eine Symbolebene wird erstellt und an die Datenquelle angefügt. Diese Ebene gibt an, wie die Start- und Endpunkte gerendert werden sollen. In diesem Fall wurden Ausdrücke hinzugefügt, um das Symbol und die Beschriftungsinformationen aus Eigenschaften für das jeweilige Punktobjekt abzurufen.

2. Legen Sie in diesem Tutorial als Startpunkt den Microsoft-Campus und als Endpunkt eine Tankstelle in Seattle fest. Fügen Sie im Ereignishandler für das Laden der Karte den folgenden Code hinzu:

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end points of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: "Redmond",
        icon: "pin-blue"
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: "Seattle",
        icon: "pin-round-blue"
    });

    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 80
    });
    ```

    Mit diesem Code werden zwei [GeoJSON-Punktobjekte](https://en.wikipedia.org/wiki/GeoJSON) erstellt, die für den Start- bzw. Endpunkt der Route stehen, und der Datenquelle hinzugefügt. Den Punkten wird jeweils eine Eigenschaft vom Typ `title` und `icon` hinzugefügt. Mit dem letzten Block wird mithilfe der Breiten- und Längengrade der Start- und Endpunkte sowie der Karteneigenschaft [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) die Kameraansicht festgelegt.

3. Speichern Sie die Datei **MapRoute.html**, und aktualisieren Sie den Browser. Die Karte ist nun auf Seattle zentriert, und Sie sehen die blaue Markierung für den Startpunkt und die runde blaue Markierung für das Ziel.

   ![Anzeigen der Karte mit markiertem Start- und Endpunkt](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Abrufen einer Wegbeschreibung

In diesem Abschnitt wird veranschaulicht, wie Sie die Routendienst-API von Azure Maps verwenden, um die Route von einem bestimmten Startpunkt zum Endpunkt zu ermitteln. Der Routendienst stellt APIs zum Planen der *schnellsten*, *kürzesten*, *umweltfreundlichsten* oder *schönsten* Route zwischen zwei Orten bereit. Benutzer können zukünftige Routen planen, indem sie die umfassende Azure-Datenbank zum Verkehrsverlauf nutzen und die Routendauern für beliebige Tage und Uhrzeiten vorhersagen. Weitere Informationen finden Sie unter [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Abrufen von Wegbeschreibungen). Alle folgenden Funktionen sollten **innerhalb des eventListener-Elements für das Laden der Karte** hinzugefügt werden, um sicherzustellen, dass sie erst nach dem vollständigen Laden der Karte geladen werden.

1. Fügen Sie in der GetMap-Funktion den folgenden JavaScript-Code hinzu:

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   **SubscriptionKeyCredential** erstellt ein **SubscriptionKeyCredentialPolicy**-Objekt, um HTTP-Anforderungen mit dem Abonnementschlüssel für Azure Maps zu authentifizieren. **atlas.service.MapsURL.newPipeline()** verwendet die Richtlinie **SubscriptionKeyCredential** und erstellt eine [Pipeline](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest)-Instanz. **routeURL** stellt eine URL zu [Routenvorgängen](https://docs.microsoft.com/rest/api/maps/route) von Azure Maps dar.

2. Fügen Sie nach dem Einrichten von Anmeldeinformationen und URL den folgenden JavaScript-Code hinzu, um die Route vom Start- zum Endpunkt zu erstellen. **routeURL** fordert beim Azure Maps-Routendienst die Berechnung von Wegbeschreibungen an. Anschließend wird mit der Methode **geojson.getFeatures()** eine GeoJSON-Merkmalsauswahl extrahiert und der Datenquelle hinzugefügt.

    ```JavaScript
    //Start and end point input to the routeURL
    var coordinates= [[startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]], [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]];

    //Make a search route request
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {
        //Get data features from response
        var data = directions.geojson.getFeatures();
        datasource.add(data);
    });
    ```

3. Speichern Sie die Datei **MapRoute.html**, und aktualisieren Sie den Webbrowser. Bei einer erfolgreichen Verbindungsherstellung mit den Maps-APIs sollte eine Karte angezeigt werden, die in etwa wie folgt aussieht:

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

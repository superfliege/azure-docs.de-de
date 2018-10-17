---
title: Suchen mit Azure Maps | Microsoft-Dokumentation
description: Suchen nach einem Point of Interest in der Nähe mit Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 10/02/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 761674c5839f0513532355116db07604f9e9d9dc
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816819"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Suchen nach Points of Interest in der Nähe mit Azure Maps

In diesem Tutorial wird gezeigt, wie Sie ein Konto für Azure Maps einrichten und anschließend mithilfe der Maps-APIs nach einem Point of Interest suchen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Maps-Kontos
> * Abrufen des Primärschlüssels für Ihr Maps-Konto
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Suchen eines Point of Interest in der Nähe mithilfe des Maps-Suchdiensts

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Erstellen eines Kontos mit Azure Maps

Erstellen Sie mithilfe der folgenden Schritte ein neues Maps-Konto:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) links oben auf **Ressource erstellen**.
2. Geben Sie im Feld *Marketplace durchsuchen* das Wort **Maps** ein.
3. Wählen Sie in den *Ergebnissen* die Option **Maps** aus. Klicken Sie auf die unterhalb der Karte angezeigte Schaltfläche **Erstellen**.
4. Geben Sie auf der Seite **Azure Maps-Konto erstellen** die folgenden Werte ein:
    * *Name* des neuen Kontos
    * *Abonnement*, das Sie für dieses Konto verwenden möchten
    * Name der *Ressourcengruppe* für dieses Konto. Sie können für die Ressourcengruppe die Option *Neu erstellen* oder die Option *Vorhandene verwenden* auswählen.
    * Wählen Sie den *Ressourcengruppenstandort*.
    * Lesen Sie die *Lizenzbedingungen* und die *Datenschutzerklärung*, und aktivieren Sie zum Akzeptieren der Bestimmungen das Kontrollkästchen.
    * Klicken Sie auf die Schaltfläche **Erstellen** .

    ![Erstellen eines Maps-Kontos im Portal](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Abrufen des Primärschlüssels für Ihr Konto

Rufen Sie nach der Erstellung des Maps-Kontos den Schlüssel ab, mit dem Sie die Maps-APIs abfragen können.

1. Öffnen Sie Ihr Maps-Konto im Portal.
2. Wählen Sie im Abschnitt „Einstellungen“ die Option **Schlüssel** aus.
3. Kopieren Sie den **Primärschlüssel** in die Zwischenablage. Speichern Sie ihn lokal zur späteren Verwendung in diesem Tutorial.

    ![Abrufen des Primärschlüssels im Portal](./media/tutorial-search-location/get-key.png)

<a id="createmap"></a>

## <a name="create-a-new-map"></a>Erstellen einer neuen Karte

Bei der Kartensteuerelement-API handelt es sich um eine praktische Clientbibliothek, die die einfache Integration von Maps in Ihre Webanwendung ermöglicht. Sie vereinfacht reine REST-Dienstaufrufe und steigert Ihre Produktivität mit anpassbaren Komponenten. Die folgenden Schritte veranschaulichen, wie Sie eine statische HTML-Seite erstellen, in die die Kartensteuerelement-API eingebettet ist.

1. Erstellen Sie auf dem lokalen Computer eine neue Datei, und nennen Sie sie **MapSearch.html**.
2. Fügen Sie der Datei die folgenden HTML-Komponenten hinzu:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

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
    Beachten Sie, dass der HTML-Header die CSS- und JavaScript-Ressourcendateien enthält, die von der Azure-Kartensteuerelement-Bibliothek gehostet werden. Beachten Sie außerdem das Segment *script*, das zum Abschnitt *body* der HTML-Datei hinzugefügt wurde. Dieses Segment enthält den JavaScript-Inlinecode für den Zugriff auf die Azure Maps-APIs.

3. Fügen Sie dem *script*-Block der HTML-Datei den folgenden JavaScript-Code hinzu. Ersetzen Sie die Zeichenfolge **\<your account key\>** durch den Primärschlüssel, den Sie aus Ihrem Maps-Konto kopiert haben.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    In diesem Segment wird die Kartensteuerelement-API für den Schlüssel Ihres Azure Maps-Kontos initialisiert. **Atlas** ist der Namespace, der die API und die zugehörigen visuellen Komponenten enthält. **Atlas.Map** stellt das Steuerelement für ein visuelles Element und eine interaktive Webkarte bereit.

4. Speichern Sie die Änderungen in der Datei, und öffnen Sie die HTML-Seite in einem Browser. Dies ist die einfachste Karte, die Sie durch das Aufrufen von **atlas.map** mit Ihrem Kontoschlüssel erstellen können.

   ![Anzeigen der Karte](./media/tutorial-search-location/basic-map.png)

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Hinzufügen von Suchfunktionen

In diesem Abschnitt wird veranschaulicht, wie mit der Such-API von Maps ein Point of Interest auf Ihrer Karte gesucht wird. Es handelt sich um eine RESTful-API für Entwickler für die Suche nach Adressen, Points of Interest und geografischen Informationen. Der Suchdienst ordnet einer bestimmten Adresse Informationen zu Breiten- und Längengrad zu. Das im Anschluss erläuterte **Dienstmodul** kann verwendet werden, um unter Verwendung der Such-API von Maps nach einem Ort zu suchen.

### <a name="service-module"></a>Dienstmodul

1. Fügen Sie Ihrer Karte eine neue Ebene hinzu, um die Suchergebnisse anzuzeigen. Fügen Sie dem Skriptblock nach dem Code für die Karteninitialisierung den folgenden JavaScript-Code hinzu:

    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    ```

2. Fügen Sie dem Skriptblock nach dem Code für die Karteninitialisierung den folgenden JavaScript-Code hinzu, um den Clientdienst zu instanziieren:

    ```JavaScript
    var client = new atlas.service.Client(MapsAccountKey);
    ```

3. Alle Funktionen der Karte sollten erst geladen werden, nachdem die Karte geladen wurde. Sie können dies sicherstellen, indem Sie alle Kartenfunktionen im eventListener-Block der Karte anordnen. Fügen Sie die folgenden Codezeilen ein, um der Karte einen [eventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) hinzuzufügen. So stellen Sie sicher, dass die Karte vollständig geladen wird, bevor Funktionalitäten hinzugefügt werden.
    
    ```JavaScript
         map.addEventListener("load", function() {
         });
    ```

4. Fügen Sie den folgenden Skriptblock **im eventListener zum Laden der Karte** hinzu, um die Abfrage zu erstellen. Hier wird der Dienst für die Fuzzysuche (eine einfache Such-API des Suchdiensts) verwendet. Der Dienst für die Fuzzysuche behandelt die meisten Fuzzyeingaben wie eine beliebige Kombination aus Adress- und POI-Token (Point of Interest). Er sucht innerhalb des angegebenen Radius nach Tankstellen in der Nähe. Die Antwort wird dann im GeoJSON Format analysiert und in Punktfeatures konvertiert, die der Karte als Stecknadeln hinzugefügt werden. Im letzten Teil des Skripts werden mithilfe der Maps-Eigenschaft [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) Kameragrenzen für die Karte hinzugefügt.

    ```JavaScript

            // Execute a POI search query then add pins to the map for each result once a response is received
            client.search.getSearchFuzzy("gasoline station", {
            lat: 47.6292,
            lon: -122.2337,
            radius: 100000
            }).then(response => {
       
            // Parse the response into GeoJSON 
            var geojsonResponse = new atlas.service.geojson.GeoJsonSearchResponse(response);

            // Create the point features that will be added to the map as pins
            var searchPins = geojsonResponse.getGeoJsonResults().features.map(poiResult => {
               var poiPosition = [poiResult.properties.position.lon, poiResult.properties.position.lat];
               return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                name: poiResult.properties.poi.name,
                address: poiResult.properties.address.freeformAddress,
                position: poiPosition[1] + ", " + poiPosition[0]
               });
            });

            // Add pins to the map for each POI
            map.addPins(searchPins, {
               name: searchLayerName,
               cluster: false, 
               icon: "pin-round-darkblue" 
            });

            // Set the camera bounds
            map.setCameraBounds({
               bounds: geojsonResponse.getGeoJsonResults().bbox,
               padding: 50
            );
        });
    ```
5. Speichern Sie die Datei **MapSearch.html**, und aktualisieren Sie den Browser. Nun sollte die Karte auf Seattle zentriert angezeigt werden, und die Standorte von Tankstellen in der Umgebung sollten mit runden blauen Markierungen gekennzeichnet sein.

   ![Anzeigen der Karte mit Suchergebnissen](./media/tutorial-search-location/pins-map.png)

6. Sie können sich die von der Karte gerenderten Rohdaten ansehen, indem Sie die folgende HTTP-Anforderung in Ihren Browser eingeben. Ersetzen Sie \<your account key\> durch Ihren Primärschlüssel.

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<your account key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Nun können auf der MapSearch-Seite die Standorte von Points of Interest angezeigt werden, die mit einer Fuzzysuchabfrage zurückgegeben werden. Fügen wir nun einige interaktive Funktionen und weitere Informationen zu den Standorten hinzu.

## <a name="add-interactive-data"></a>Hinzufügen interaktiver Daten

Die erstellte Karte verwendet bisher nur die Breiten-/Längengraddaten für die Suchergebnisse. Wenn Sie sich die vom Maps-Suchdienst zurückgegebenen JSON-Rohdaten ansehen, stellen Sie jedoch fest, dass sie zusätzliche Informationen zu den einzelnen Tankstellen enthalten, u.a. den Namen und die Adresse. Mit interaktiven Popupdialogfenstern können Sie diese Daten in die Karte integrieren.

1. Fügen Sie die folgenden Zeilen zum Block *script* hinzu, um Popups für die vom Suchdienst zurückgegebenen gewünschten Orte zu erstellen:

    ```JavaScript
    // Add a popup to the map which will display some basic information about a search result on hover over a pin
    var popup = new atlas.Popup();
    map.addEventListener("mouseover", searchLayerName, (e) => {
        var popupContentElement = document.createElement("div");
        popupContentElement.style.padding = "5px";

        var popupNameElement = document.createElement("div");
        popupNameElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupNameElement);

        var popupAddressElement = document.createElement("div");
        popupAddressElement.innerText = e.features[0].properties.address;
        popupContentElement.appendChild(popupAddressElement);

        var popupPositionElement = document.createElement("div");
        popupPositionElement.innerText = e.features[0].properties.position;
        popupContentElement.appendChild(popupPositionElement);

        popup.setPopupOptions({
            position: e.features[0].geometry.coordinates,
            content: popupContentElement
        });

        popup.open(map);
    });
    ```
    Die API **atlas.Popup** stellt ein Informationsfenster bereit, das an der erforderlichen Position auf der Karte verankert ist. In diesem Codeausschnitt werden der Inhalt und die Position für das Popupelement festgelegt. Außerdem wird dem `map`-Steuerelement ein Ereignislistener hinzugefügt, der darauf wartet, dass mit der _Maus_ ein Rollvorgang über das Popupelement durchgeführt wird.

2. Speichern Sie die Datei, und aktualisieren Sie Ihren Browser. Auf der Karte im Browser werden jetzt Popupelemente mit Informationen angezeigt, wenn Sie mit der Maus auf eine der angezeigten Suchstecknadeln zeigen.

    ![Azure-Kartensteuerelement und Suchdienst](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Kontos mit Azure Maps
> * Abrufen des Primärschlüssels für Ihr Konto
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Suchen eines Point of Interest in der Nähe mithilfe des Suchdiensts

Sie können auf das Codebeispiel für dieses Tutorial hier zugreifen:

> [Search location with Azure Maps](https://github.com/Azure-Samples/azure-maps-samples/blob/master/src/search.html) (Suchen nach dem Standort mit Azure Maps)

Das nächste Tutorial veranschaulicht, wie Sie eine Route zwischen zwei Orten anzeigen.

> [!div class="nextstepaction"]
> [Route zu einem Ziel](./tutorial-route-location.md)

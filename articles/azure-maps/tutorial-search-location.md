---
title: Suchen mit Azure Maps | Microsoft-Dokumentation
description: Suchen nach einem Point of Interest in der Nähe mit Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: ffc4b7625a6c43f8e2801313c61f14c785a3ec5f
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988873"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Suchen nach Points of Interest in der Nähe mit Azure Maps

In diesem Tutorial wird gezeigt, wie Sie ein Konto für Azure Maps einrichten und anschließend mithilfe der Maps-APIs nach einem Point of Interest suchen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Maps-Kontos
> * Abrufen des Primärschlüssels für Ihr Maps-Konto
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Suchen eines Point of Interest in der Nähe mithilfe des Maps-Suchdiensts

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Erstellen eines Kontos mit Azure Maps

Erstellen Sie mithilfe der folgenden Schritte ein neues Maps-Konto:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) links oben auf **Ressource erstellen**.
2. Geben Sie im Feld *Marketplace durchsuchen* das Wort **Maps** ein.
3. Wählen Sie in den *Ergebnissen* die Option **Maps** aus. Klicken Sie auf die unterhalb der Karte angezeigte Schaltfläche **Erstellen**. 
4. Geben Sie auf der Seite **Azure Maps-Konto erstellen** die folgenden Werte ein:
    - *Name* des neuen Kontos 
    - *Abonnement*, das Sie für dieses Konto verwenden möchten
    - Name der *Ressourcengruppe* für dieses Konto. Sie können für die Ressourcengruppe die Option *Neu erstellen* oder die Option *Vorhandene verwenden* auswählen.
    - Wählen Sie den *Ressourcengruppenstandort*.
    - Lesen Sie die *Lizenzbedingungen* und die *Datenschutzerklärung*, und aktivieren Sie zum Akzeptieren der Bestimmungen das Kontrollkästchen. 
    - Klicken Sie auf die Schaltfläche **Erstellen** .
   
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
    Beachten Sie, dass der HTML-Header die CSS- und JavaScript-Ressourcendateien enthält, die von der Azure-Kartensteuerelement-Bibliothek gehostet werden. Beachten Sie außerdem das Segment *script*, das zum Abschnitt *body* der HTML-Datei hinzugefügt wurde. Dieses Segment enthält den JavaScript-Inlinecode für den Zugriff auf die Azure Maps-APIs.
 
3. Fügen Sie dem *script*-Block der HTML-Datei den folgenden JavaScript-Code hinzu. Ersetzen Sie die Zeichenfolge **\<your account key\>** durch den Primärschlüssel, den Sie aus Ihrem Maps-Konto kopiert haben.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    In diesem Segment wird die Kartensteuerelement-API für den Schlüssel Ihres Azure Maps-Kontos initiiert. **Atlas** ist der Namespace, der die API und die zugehörigen visuellen Komponenten enthält. **Atlas.Map** stellt das Steuerelement für ein visuelles Element und eine interaktive Webkarte bereit. 
    
4. Speichern Sie die Änderungen in der Datei, und öffnen Sie die HTML-Seite in einem Browser. Dies ist die einfachste Karte, die Sie durch den Aufruf von **atlas.map** und die Angabe Ihres Kontoschlüssels erstellen können. 

   ![Anzeigen der Karte](./media/tutorial-search-location/basic-map.png)


<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Hinzufügen von Suchfunktionen

In diesem Abschnitt wird veranschaulicht, wie mit der Such-API von Maps ein Point of Interest auf Ihrer Karte gesucht wird. Es handelt sich um eine RESTful-API für Entwickler für die Suche nach Adressen, Points of Interest und geografischen Informationen. Der Suchdienst ordnet einer bestimmten Adresse Informationen zu Breiten- und Längengrad zu. 

1. Fügen Sie Ihrer Karte eine neue Ebene hinzu, um die Suchergebnisse anzuzeigen. Fügen Sie dem Block *script* nach dem Code zum Initialisieren der Karte den folgenden Javascript-Code hinzu: 

    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    map.addPins([], {
        name: searchLayerName,
        cluster: false,
        icon: "pin-round-darkblue"
    });
    ```

2. Erstellen Sie [XMLHttpRequest](https://xhr.spec.whatwg.org/), und fügen Sie einen Ereignishandler zum Analysieren der JSON-Antwort hinzu, die vom Maps-Suchdienst gesendet wird. Dieser Codeausschnitt erstellt den Ereignishandler zum Erfassen der Adressen, Namen und Informationen zu Breiten- und Längengraden für jeden Ort, der in der Variablen `searchPins` zurückgegeben wird. Schließlich wird diese Sammlung von Ortspunkten als Pins zum `map`-Steuerelement hinzugefügt. 

    ```JavaScript
    // Perform a request to the search service and create a pin on the map for each result
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        var searchPins = [];

        if (this.readyState === 4 && this.status === 200) {
            var response = JSON.parse(this.responseText);

            var poiResults = response.results.filter((result) => { return result.type === "POI" }) || [];

            searchPins = poiResults.map((poiResult) => {
                var poiPosition = [poiResult.position.lon, poiResult.position.lat];
                return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                    name: poiResult.poi.name,
                    address: poiResult.address.freeformAddress,
                    position: poiResult.position.lat + ", " + poiResult.position.lon
                });
            });

            map.addPins(searchPins, {
                name: searchLayerName
            });

            var lons = searchPins.map((pin) => { return pin.geometry.coordinates[0] });
            var lats = searchPins.map((pin) => { return pin.geometry.coordinates[1] });

            var swLon = Math.min.apply(null, lons);
            var swLat = Math.min.apply(null, lats);
            var neLon = Math.max.apply(null, lons);
            var neLat = Math.max.apply(null, lats);

            map.setCameraBounds({
                bounds: [swLon, swLat, neLon, neLat],
                padding: 50
            });
        }
    };
    ```

3. Fügen Sie den folgenden Code zum Block *script* hinzu, um die Abfrage zu erstellen und „XMLHttpRequest“ an den Maps-Suchdienst zu senden:

    ```JavaScript
    var url = "https://atlas.microsoft.com/search/fuzzy/json?";
    url += "api-version=1.0";
    url += "&query=gasoline%20station";
    url += "&subscription-key=" + MapsAccountKey;
    url += "&lat=47.6292";
    url += "&lon=-122.2337";
    url += "&radius=100000";

    xhttp.open("GET", url, true);
    xhttp.send();
    ``` 
    Dieser Codeausschnitt verwendet die Standardsuche-API (**Fuzzysuche**) des Suchdiensts. Diese Suche ist für ungenaue Angaben vorgesehen und verarbeitet beliebige Kombinationen aus Adress- oder POI-Token. Es wird nach nahegelegenen **Tankstellen** in einem bestimmten Radius der Längen- und Breitengradkoordinaten gesucht. Der Aufruf von Maps erfolgt unter Verwendung des Primärschlüssels für Ihr Konto, der zuvor in der Beispieldatei bereitgestellt wurde. Die Ergebnisse werden für die gefundenen Orte als Breiten-/Längengradpaare zurückgegeben. 
    
4. Speichern Sie die Datei **MapSearch.html**, und aktualisieren Sie den Browser. Nun sollte die Karte auf Seattle zentriert angezeigt werden, und die Standorte von Tankstellen in der Umgebung sind mit runden blauen Markierungen gekennzeichnet. 

   ![Anzeigen der Karte mit Suchergebnissen](./media/tutorial-search-location/pins-map.png)

5. Sie können die von der Karte gerenderten Rohdaten anzeigen. Geben Sie dazu das in der Datei erstellte XMLHTTPRequest-Element in Ihren Browser ein. Ersetzen Sie \<your account key\> durch Ihren Primärschlüssel. 

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
        popupPositionElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupPositionElement);

        popup.setPopupOptions({
            position: e.features[0].geometry.coordinates,
            content: popupContentElement
        });

        popup.open(map);
    });
    ```
    Die API **atlas.Popup** stellt ein Informationsfenster bereit, das an der erforderlichen Position auf der Karte verankert ist. Der Codeausschnitt legt den Inhalt und die Position für das Popup fest, fügt einen Ereignislistener zum `map`-Steuerelement hinzu und wartet, dass die _Maus_ über das Popup bewegt wird. 

4. Speichern Sie die Datei, und aktualisieren Sie Ihren Browser. Auf der Karte im Browser werden jetzt Popupelemente mit Informationen angezeigt, wenn Sie mit der Maus auf eine der angezeigten Suchstecknadeln zeigen. 

    ![Azure-Kartensteuerelement und Suchdienst](./media/tutorial-search-location/popup-map.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Kontos mit Azure Maps
> * Abrufen des Primärschlüssels für Ihr Konto
> * Erstellen einer neuen Webseite mit der Kartensteuerelement-API
> * Suchen eines Point of Interest in der Nähe mithilfe des Suchdiensts

Das nächste Tutorial veranschaulicht, wie Sie eine Route zwischen zwei Orten anzeigen. 

> [!div class="nextstepaction"]
> [Route zu einem Ziel](./tutorial-route-location.md)

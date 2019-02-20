---
title: Rendern benutzerdefinierter Daten auf einer Rasterkarte in Azure Maps | Microsoft-Dokumentation
description: Rendern von benutzerdefinierten Daten auf einer Rasterkarte in Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 9b3c0f7b1ff56cb269f6852be8fd2affeca8b8f1
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56119072"
---
# <a name="render-custom-data-on-raster-map"></a>Rendern von benutzerdefinierten Daten auf einer Rasterkarte

Dieser Artikel zeigt Ihnen, wie Sie den [statischen Bilddienst](https://docs.microsoft.com/rest/api/maps/render/getmapimage) mit der Bildzusammensetzungs-Funktionalität verwenden können, um Überlagerungen auf einer Rasterkarte zu ermöglichen. Die Bildzusammensetzung beinhaltet die Möglichkeit, eine Rasterkachel mit zusätzlichen Daten wie benutzerdefinierten Ortsmarken, Bezeichnungen und Geometrieüberlagerungen abzurufen. Um benutzerdefinierte Ortsmarken, Bezeichnungen und Geometrien zu rendern, werden wir die Postman-Anwendung verwenden. Öffnen Sie die Postman-App, klicken Sie auf „Neu | Neu erstellen“, wählen und benennen Sie eine Sammlung oder einen Ordner, in dem Sie sie speichern möchten, und klicken Sie auf „Speichern“.

Wir verwenden[Data Service-APIs](https://docs.microsoft.com/rest/api/maps/data) von Azure Maps zum Speichern und Rendern von Überlagerungen. 


## <a name="prerequisites"></a>Voraussetzungen

### <a name="create-an-azure-maps-account"></a>Erstellen eines Azure Maps-Kontos 

Um die Schritte in diesem Leitfaden auszuführen, müssen Sie zunächst [Verwalten von Konten und Schlüsseln](how-to-manage-account-keys.md) lesen, um Ihr Kontoabonnement mit S1-Tarif zu erstellen und zu verwalten.

## <a name="render-pushpins-with-labels-and-custom-image"></a>Rendern von Ortsmarken mit Bezeichnungen und benutzerdefinierten Bildern

> [!Note]
> Dieses Beispiel erfordert ein Azure Maps-Konto mit dem Tarif S0 oder S1. 

Das Azure Maps-Konto S0 SKU unterstützt nur eine einzige Instanz des `pins`-Parameters, sodass Benutzer bis zu 5 in der URL-Anfrage angegebene Ortsmarken mit einem benutzerdefinierten Bild rendern können.

Um Ortsmarken mit Bezeichnungen und einem benutzerdefinierten Bild zu rendern, gehen Sie wie folgt vor:

1. Öffnen Sie die Postman-App, und klicken Sie auf „Neu > „Neu erstellen. Wählen Sie anschließend die Option „Anfordern“ aus. Geben Sie einen Anforderungsnamen zum Rendern der Ortsmarken ein, wählen Sie eine Sammlung oder einen Ordner aus, in der bzw. dem die Anforderung gespeichert wird, und klicken Sie auf „Speichern“.
    
    ![Hochladen von Geofences mit Postman](./media/tutorial-geofence/postman-new.png)

2. Wählen Sie auf der Registerkarte „Generator“ die GET-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine GET-Anforderung zu senden.

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.98,%2040.77&pins=custom%7Cla15+50%7Cls12%7Clc003b61%7C%7C%27CentralPark%27-73.9657974+40.781971%7C%7Chttp%3A%2F%2Fazuremapscodesamples.azurewebsites.net%2FCommon%2Fimages%2Fpushpins%2Fylw-pushpin.png
    ```
    Im Folgenden finden Sie das Antwortbild.

    ![Rendern von benutzerdefinierten Ortsmarken mit Bezeichnungen](./media/how-to-render-custom-data/render-pins.png)


## <a name="get-data-from-azure-maps-data-storage"></a>Abrufen von Daten aus dem Azure Maps-Datenspeicher

> [!Note]
> Dieses Beispiel erfordert ein Azure Maps-Konto mit dem Tarif S1.

Die Informationen zur Position von Pfaden und Ortsmarken können auch über die [Data Upload-API](https://docs.microsoft.com/rest/api/maps/mapdata/upload) abgerufen werden. Gehen Sie folgendermaßen vor, um die Daten zu Pfaden und Ortsmarken hochladen.

1. Öffnen Sie in der Postman-App eine neue Registerkarte in derselben Sammlung, die Sie oben erstellt haben. Wählen Sie auf der Registerkarte „Generator“ die POST-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine POST-Anforderung zu senden:

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

2. Klicken Sie auf „Params“, und geben Sie die folgenden Schlüssel-Wert-Paare ein, die für die POST-Anforderungs-URL verwendet werden sollen. Ersetzen Sie den Abonnementschlüsselwert durch Ihren Azure Maps-Abonnementschlüssel.
    
    ![Schlüssel-Wert-Parameter: Postman](./media/how-to-render-custom-data/postman-key-vals.png)

3. Klicken Sie auf **Text**, und wählen Sie dann das Rohdaten-Eingabeformat aus. Wählen Sie in der Dropdownliste anschließend „JSON“ als Eingabeformat. Geben Sie den folgenden JSON-Code als hochzuladende Daten ein:
    
    ```JSON
    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -73.98235,
                  40.76799
                ],
                [
                  -73.95785,
                  40.80044
                ],
                [
                  -73.94928,
                  40.7968
                ],
                [
                  -73.97317,
                  40.76437
                ],
                [
                  -73.98235,
                  40.76799
                ]
              ]
            ]
          }
        },
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "LineString",
            "coordinates": [
              [
                -73.97624731063843,
                40.76560773817073
              ],
              [
                -73.97914409637451,
                40.766826609362575
              ],
              [
                -73.98513078689575,
                40.7585866048861
              ]
            ]
          }
        }
      ]
    }
    ```

4. Klicken Sie auf „Senden“, und sehen Sie sich den Antwortheader an. Der Adressheader enthält den URI, mit dem auf die Daten zur zukünftigen Verwendung zugegriffen wird bzw. über den diese Daten heruntergeladen werden. Darüber hinaus enthält er ein eindeutiges `udId`-Element für die hochgeladenen Daten.   

  ```HTTP
  https://atlas.microsoft.com/mapData/{udId}/status?api-version=1.0&subscription-key={Subscription-key}
  ```



5. Verwenden Sie den von der Data Upload-API empfangenen Wert von `udid`, um Features auf der Karte zu rendern. Öffnen Sie dafür eine neue Registerkarte in derselben Sammlung, die Sie oben erstellt haben. Wählen Sie auf der Registerkarte „Generator“ die GET-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine GET-Anforderung zu senden:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.96682739257812%2C40.78119135317995&pins=default|la-35+50|ls12|lc003C62|co9B2F15||'Times Square'-73.98516297340393 40.758781646381024|'Central Park'-73.96682739257812 40.78119135317995&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.30||udid-{udId}
    ```

6. Das Antwortbild sollte nun wie in der folgenden Abbildung aussehen:

    ![Hochgeladene Daten rendern](./media/how-to-render-custom-data/uploaded-path.png)

## <a name="render-polygon-with-color-and-opacity"></a>Rendern eines Polygons mit Farbe und Deckkraft

> [!Note]
> Dieses Beispiel erfordert ein Azure Maps-Konto mit dem Tarif S1.

Sie können das Aussehen eines Polygons ändern, indem Sie Stilmodifikatoren mit dem Parameter [path](https://docs.microsoft.com/rest-staging/api/maps/render/getmapimage#uri-parameters) verwenden.

1. Öffnen Sie in der Postman-App eine neue Registerkarte in derselben Sammlung, die Sie oben erstellt haben. Wählen Sie auf der Registerkarte „Generator“ die GET-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine GET-Anforderung zu senden, um ein Polygon mit Farbe und Deckkraft zu rendern:
    
    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&sku=S1&zoom=14&height=500&Width=500&center=-74.040701, 40.698666&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.50||-74.03995513916016 40.70090237454063|-74.04082417488098 40.70028420372218|-74.04113531112671 40.70049568385827|-74.04298067092896 40.69899904076542|-74.04271245002747 40.69879568992435|-74.04367804527283 40.6980961582905|-74.04364585876465 40.698055487620714|-74.04368877410889 40.698022951066996|-74.04168248176573 40.696444909137|-74.03901100158691 40.69837271818651|-74.03824925422668 40.69837271818651|-74.03809905052185 40.69903971085914|-74.03771281242369 40.699340668780984|-74.03940796852112 40.70058515602143|-74.03948307037354 40.70052821920425|-74.03995513916016 40.70090237454063
    &subscription-key={subscription--key}
    ```

Das Antwortbild sollte nun wie in der folgenden Abbildung aussehen:

![Nicht transparentes Polygon rendern](./media/how-to-render-custom-data/opaque-polygon.png)


## <a name="render-polygon-with-circle-and-push-pins-with-custom-labels"></a>Polygon mit Kreis und Ortsmarken mit benutzerdefinierten Bezeichnungen rendern

> [!Note]
> Dieses Beispiel erfordert ein Azure Maps-Konto mit dem Tarif S1.

Sie können Ortsmarken und ihre Bezeichnungen vergrößern oder verkleinern, indem Sie den Skalierungsmodifikator „sc“ verwenden. Dies ist ein Wert größer als 0. Der Wert 1 ist die Standardskalierung. Werte größer als 1 machen die Ortsmarken größer, und Werte kleiner als 1 machen sie kleiner. Weitere Informationen zu den Stilmodifikatoren finden Sie unter [Parameter für statischen Bilddienstpfad](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters).

Führen Sie die folgenden Schritte aus, um ein Polygon mit Kreis und Ortsmarken mit benutzerdefinierten Bezeichnungen zu rendern:

1. Öffnen Sie in der Postman-App eine neue Registerkarte in derselben Sammlung, die Sie oben erstellt haben. Wählen Sie auf der Registerkarte „Generator“ die GET-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine GET-Anforderung zu senden:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co002D62||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

Das Antwortbild sollte nun wie in der folgenden Abbildung aussehen:

![Kreis mit benutzerdefinierten Ortsmarken rendern](./media/how-to-render-custom-data/circle-custom-pins.png)

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit der API-Dokumentation zum [Get Map-Image-API von Azure Maps](https://docs.microsoft.com/rest/api/maps/search) vertraut.
* Um mehr über die Funktionalitäten des Azure Maps-Datendiensts zu erfahren, lesen Sie die [Dienstdokumentation](https://docs.microsoft.com/rest/api/maps/data).
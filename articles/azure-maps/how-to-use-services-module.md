---
title: Verwenden des Moduls „Dienste“ – Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Modul „Dienste“ von Azure Maps verwenden.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: e614758a91cb3ff02822eeeeb8ae7e80d2123e5d
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578729"
---
# <a name="using-the-azure-maps-services-module"></a>Verwenden des Moduls „Dienste“ von Azure Maps

Das Azure Maps Web SDK stellt das Modul „Dienste“ (eine unterstützende Bibliothek) bereit, das die Verwendung der Azure Maps REST-Dienste in Web- oder Node.js-Anwendungen vereinfacht, die JavaScript oder TypeScript verwenden.

## <a name="using-the-services-module-in-a-web-page"></a>Verwenden des Moduls „Dienste“ in einer Webseite

1. Erstellen Sie eine neue HTML-Datei.
2. Laden Sie sie in das Modul „Dienste“ von Azure Maps. Dies kann auf einem von zwei Wegen erfolgen:

    a. Verwenden Sie die global gehostete CDN-Version des Moduls „Dienste“ von Azure Maps, indem Sie eine Skriptreferenz auf das Element <head> der Datei hinzufügen:
    
    ```html
    <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=2"></script>
    ```
    
    b. Laden Sie alternativ den Quellcode des Azure Maps Web SDK lokal mithilfe des NPM-Pakets [azure-maps-rest](https://www.npmjs.com/package/azure-maps-rest), und hosten Sie es zusammen mit Ihrer App. Dieses Paket enthält außerdem TypeScript-Definitionen.
    
    > npm install azure-maps-rest
    
    Fügen Sie dann einen Skriptverweis zum `<head>`-Element der Datei hinzu:
    
    ```html
    <script src="node_modules/azure-maps-rest/dist/js/atlas-service.min.js"></script>
    ```

3. Um einen Clientendpunkt einer Dienst-URL zu initialisieren, müssen Sie zunächst eine Authentifizierungspipeline erstellen. Verwenden Sie Ihren eigenen Azure Maps-Kontoschlüssel oder Ihre Azure Active Directory-Anmeldeinformationen (AAD), um den Client des Suchdiensts zu authentifizieren. In diesem Beispiel wird der URL-Client des Suchdiensts erstellt. Falls ein Abonnementschlüssel für die Authentifizierung verwendet wird:

    ```javascript
    //Get an Azure Maps key at https://azure.com/maps
    var subscriptionKey = '<Your Azure Maps Key>';
    
    //Use SubscriptionKeyCredential with a subscription key.
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(subscriptionKey);
    
    //Use subscriptionKeyCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential, {
      retryOptions: { maxTries: 4 } // Retry options
    });
    
    //Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);
    ```
    
    Falls Azure Active Directory (AAD) für die Authentifizierung verwendet wird:

    ```javascript
    // Enter your Azure Actiuve Directory client ID.
    var clientId = "<Your Azure Active Directory Client Id>";
    
    // Use TokenCredential with OAuth token (AAD or Anonymous).
    var aadToken = await getAadToken();
    var tokenCredential = new atlas.service.TokenCredential(clientId, aadToken);
    
    // Create a repeating timeout that will renew the AAD token.
    // This timeout must be cleared once the TokenCredential object is no longer needed.
    // If the timeout is not cleared the memory used by the TokenCredential will never be reclaimed.
    var renewToken = async () => {
        try {
            console.log("Renewing token");
            var token = await getAadToken();
            tokenCredential.token = token;
            tokenRenewalTimer = setTimeout(renewToken, getExpiration(token));
        } catch (error) {
            console.log("Caught error when renewing token");
            clearTimeout(tokenRenewalTimer);
            throw error;
        }
    }
    tokenRenewalTimer = setTimeout(renewToken, getExpiration(aadToken));
    
    // Use tokenCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(tokenCredential, {
        retryOptions: { maxTries: 4 } // Retry options
    });
    
    //Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);

    function getAadToken() {
        //Use the logged in auth context to get a token.
        return new Promise((resolve, reject) => {
            //The resource should always be https://atlas.microsoft.com/.
            const resource = "https://atlas.microsoft.com/";
            authContext.acquireToken(resource, (error, token) => {
                if (error) {
                    reject(error);
                } else {
                    resolve(token);
                }
            });
        })
    }

    function getExpiration(jwtToken) {
        //Decode the JWT token to get the expiration timestamp.
        const json = atob(jwtToken.split(".")[1]);
        const decode = JSON.parse(json);

        //Return the milliseconds until the token needs renewed.
        //Reduce the time until renew by 5 minutes to avoid using an expired token.
        //The exp property is the timestamp of the expiration in seconds.
        const renewSkew = 300000;
        return (1000 * decode.exp) - Date.now() - renewSkew;
    }
    ```

    Weitere Informationen finden Sie unter [Authentifizierung mit Azure Maps](azure-maps-authentication.md).

4. Der folgende Code verwendet den neu erstellten URL-Client des Suchdiensts, um die Geocodierung mit der Funktion `searchAddress` auf eine Adresse (1 Microsoft Way, Redmond, WA) anzuwenden und die Ergebnisse als Tabelle im Hauptteil der Seite anzuzeigen. 

    ```javascript
    //Search for "1 microsoft way, redmond, wa".
    searchURL.searchAddress(atlas.service.Aborter.timeout(10000), '1 microsoft way, redmond, wa').then(response => {
      var html = [];
      
      //Display the total results.
      html.push('Total results: ', response.summary.numResults, '<br/><br/>');
     
      //Create a table of the results.
      html.push('<table><tr><td></td><td>Result</td><td>Latitude</td><td>Longitude</td></tr>');
      
      for(var i=0;i<response.results.length;i++){
        html.push('<tr><td>', (i+1), '.</td><td>', 
                    response.results[i].address.freeformAddress, 
                    '</td><td>', 
                    response.results[i].position.lat,
                    '</td><td>', 
                    response.results[i].position.lon,
                    '</td></tr>');
      }
      
      html.push('</table>');
      
      //Add the result HTML to the body of the page.
      document.body.innerHTML = html.join('');
    });
    ```

    Hier folgt das voll funktionsfähige Codebeispiel:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Verwenden des Moduls „Dienste“" src="//codepen.io/azuremaps/embed/zbXGMR/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/zbXGMR/'>Verwenden des Moduls „Dienste“</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [MapsURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.mapsurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SearchURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [RouteURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SubscriptionKeyCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.subscriptionkeycredential?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TokenCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.tokencredential?view=azure-iot-typescript-latest)

In den folgenden Artikeln finden Sie weitere Codebeispiele, die das Modul „Dienste“ verwenden:

> [!div class="nextstepaction"]
> [Anzeigen von Suchergebnissen auf der Karte](./map-search-location.md)

> [!div class="nextstepaction"]
> [Abrufen von Informationen von einer Koordinate](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [Anzeigen einer Wegbeschreibung von A nach B](./map-route.md)
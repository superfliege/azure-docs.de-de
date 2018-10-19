---
title: Anzeigen von Suchergebnissen mit Azure Maps | Microsoft-Dokumentation
description: So führen Sie eine Suchanfrage mit Azure Maps aus und zeigen dann die Ergebnisse auf einer Javascript-Karte an
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 78ffa42bcf57b7163afc13b2550abdbae240ef00
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729237"
---
# <a name="show-search-results-on-the-map"></a>Anzeigen von Suchergebnissen auf der Karte

In diesem Artikel erfahren Sie, wie nach einem bestimmten Ort gesucht wird und die Suchergebnisse auf der Karte angezeigt werden.

Es gibt zwei Möglichkeiten, um nach einem bestimmten Ort zu suchen. Eine Möglichkeit besteht darin, über ein Dienstmodul eine Suchanforderung zu stellen. Die andere Möglichkeit ist, eine Suchanforderung über [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) zu stellen. Beide Möglichkeiten werden im Folgenden erläutert.

## <a name="make-a-search-request-via-service-module"></a>Durchführen einer Suchanforderung über das Dienstmodul

<iframe height='500' scrolling='no' title='Anzeigen von Suchergebnissen auf einer Karte (Dienstmodul)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Siehe <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Anzeigen von Suchergebnissen auf einer Karte (Dienstmodul)</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der erste Codeblock erzeugt ein Kartenobjekt und initialisiert den Clientdienst. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock verwendet die [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) für die „Point of Interest“-Suche (Suche nach einem bestimmten Ort). Die API für die Fuzzysuche kann eine beliebige Kombination von Fuzzyeingaben verarbeiten. Die Antwort des Diensts für die Fuzzysuche wird dann im GeoJSON-Format mithilfe der Methode [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) analysiert. Die Stecknadeln werden dann der Karte hinzugefügt, um die relevanten Punkte auf der Karte anzuzeigen.

Im letzten Codeblock werden mithilfe der Maps-Eigenschaft [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) Kameragrenzen für die Karte angepasst.

## <a name="make-a-search-request-via-xmlhttprequest"></a>Durchführen einer Suchanforderung über XMLHttpRequest

<iframe height='500' scrolling='no' title='Anzeigen von Suchergebnissen auf einer Karte' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Show search results on a map</a> (Anzeigen von Suchergebnissen auf einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock fügt der Karte die Suchergebnissebene hinzu. Die Suchergebnissebene zeigt die Suchergebnisse als Markierungen auf der Karte an. Die Markierungen werden mit [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins) hinzugefügt.

Der dritte Codeblock sendet [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy), um nach dem bestimmten Ort zu suchen. Die API für die Fuzzysuche kann eine beliebige Kombination von Fuzzyeingaben verarbeiten.

Der letzte Codeblock analysiert die Antwort und passt die Kameragrenzen für die Karte an, indem er die [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest)-Einstellungen der Karte verwendet, um die Ergebnismarkierungen darzustellen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur **Fuzzysucheuche**:

> [!div class="nextstepaction"]
> [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Die folgenden Artikel enthalten vollständige Codebeispiele:

> [!div class="nextstepaction"]
> [Abrufen von Informationen von einer Koordinate](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [Anzeigen einer Wegbeschreibung von A nach B](./map-route.md)

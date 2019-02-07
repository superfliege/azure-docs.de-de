---
title: Anzeigen von Suchergebnissen mit Azure Maps | Microsoft-Dokumentation
description: So führen Sie eine Suchanfrage mit Azure Maps aus und zeigen dann die Ergebnisse auf einer Javascript-Karte an
author: jingjing-z
ms.author: jinzh
ms.date: 11/15/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c68b4bdffde5f987fe07d50d76fa83e7bdfa5235
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55755039"
---
# <a name="show-search-results-on-the-map"></a>Anzeigen von Suchergebnissen auf der Karte

In diesem Artikel erfahren Sie, wie nach einem bestimmten Ort gesucht wird und die Suchergebnisse auf der Karte angezeigt werden.

Es gibt zwei Möglichkeiten, um nach einem bestimmten Ort zu suchen. Eine Möglichkeit besteht darin, über ein Dienstmodul eine Suchanforderung zu stellen. Die andere Möglichkeit ist, eine Suchanforderung über [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) zu stellen. Beide Möglichkeiten werden im Folgenden erläutert.

## <a name="make-a-search-request-via-service-module"></a>Durchführen einer Suchanforderung über das Dienstmodul

<iframe height='500' scrolling='no' title='Anzeigen von Suchergebnissen auf einer Karte (Dienstmodul)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Siehe <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Anzeigen von Suchergebnissen auf einer Karte (Dienstmodul)</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der erste Codeblock im obigen Code erzeugt ein Kartenobjekt und initialisiert den Clientdienst. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock verwendet die [getSearchFuzzy](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchfuzzy-string--searchgetsearchfuzzyoptionalparams-)-Methode im [Dienstmodul](https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1). Sie ermöglicht über die [Rest-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) die Freitextsuche nach Points of Interest. Die API für die Fuzzysuche kann eine beliebige Kombination von Fuzzyeingaben verarbeiten. Die Antwort des Diensts für die Fuzzysuche wird dann im GeoJSON-Format mithilfe der Methode [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest) analysiert. 

Der dritte Codeblock erstellt mit der [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)-Klasse ein Datenquellenobjekt und fügt diesem Suchergebnisse hinzu. Eine [Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) verwendet Text oder Symbole zum Rendern punktbasierter Daten, die in [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als Symbole auf der Karte umschlossen sind.  Anschließend wird eine Symbolebene erstellt, und die Datenquelle wird der Symbolebene hinzugefügt, die dann der Karte hinzugefügt wird.

Im letzten Codeblock werden mithilfe der Maps-Eigenschaft [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) die Kameragrenzen für die Karte angepasst.

Die Suchanforderung, die Datenquelle, die Symbolebene und die Kameragrenzen werden im [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) der Karte erstellt und festgelegt, um sicherzustellen, dass die Ergebnisse nach dem vollständigen Laden der Karte angezeigt werden.


## <a name="make-a-search-request-via-xmlhttprequest"></a>Durchführen einer Suchanforderung über XMLHttpRequest

<iframe height='500' scrolling='no' title='Anzeigen von Suchergebnissen auf einer Karte' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Show search results on a map</a> (Anzeigen von Suchergebnissen auf einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock sendet eine [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy), um nach dem Point of Interest zu suchen. Die API für die Fuzzysuche kann eine beliebige Kombination von Fuzzyeingaben verarbeiten. 

Der dritte Codeblock analysiert die Suchantwort und speichert die Ergebnisse in einem Array, um die Begrenzungen zu berechnen. Anschließend gibt er die Suchergebnisse zurück.

Der vierte Codeblock erstellt mit der [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)-Klasse ein Datenquellenobjekt und fügt diesem Suchergebnisse hinzu. Eine [Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) verwendet Text oder Symbole zum Rendern punktbasierter Daten, die in [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als Symbole auf der Karte umschlossen sind. Anschließend wird eine Symbolebene erstellt, und die Datenquelle wird der Symbolebene hinzugefügt, die dann der Karte hinzugefügt wird.

Der letzte Codeblock erstellt anhand des Ergebnisarrays ein [BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest)-Objekt und passt dann die Kameragrenzen für die Karte mithilfe der Maps-Eigenschaft [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) an. Anschließend werden die Ergebnismarkierungen gerendert.

Die Suchanforderung, die Datenquelle, die Symbolebene und die Kameragrenzen werden im [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) der Karte festgelegt, um sicherzustellen, dass die Ergebnisse nach dem vollständigen Laden der Karte angezeigt werden.

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
<!-- Comment added to suppress false positive warning -->
> [!div class="nextstepaction"]
> [Anzeigen einer Wegbeschreibung von A nach B](./map-route.md)

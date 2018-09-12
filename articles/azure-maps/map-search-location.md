---
title: Anzeigen von Suchergebnissen mit Azure Maps | Microsoft-Dokumentation
description: So führen Sie eine Suchanfrage mit Azure Maps aus und zeigen dann die Ergebnisse auf einer Javascript-Karte an
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7d4eb5f9be4a6bcefe4b544d3f97a9b9391c0d81
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665776"
---
# <a name="show-search-results-on-the-map"></a>Anzeigen von Suchergebnissen auf der Karte

In diesem Artikel erfahren Sie, wie nach einem bestimmten Ort gesucht wird und die Suchergebnisse auf der Karte angezeigt werden. 

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='500' scrolling='no' title='Anzeigen von Suchergebnissen auf einer Karte (Dienstmodul)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Siehe <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Anzeigen von Suchergebnissen auf einer Karte (Dienstmodul)</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes werden ein Kartenobjekt erstellt und ein Clientdienst instanziiert. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock verwendet die Fuzzysuche ([Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)) für die Point of Interest-Suche. Die API für die Fuzzysuche kann eine beliebige Kombination von Fuzzyeingaben verarbeiten. Die Antwort des Diensts für die Fuzzysuche wird dann im GeoJSON-Format mithilfe der Methode [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) analysiert. Die Stecknadeln werden dann der Karte hinzugefügt, um die relevanten Punkte auf der Karte anzuzeigen.

Im letzten Codeblock werden mithilfe der Maps-Eigenschaft [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) Kameragrenzen für die Karte hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 

* [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln: 
* [Abrufen von Informationen von einer Koordinate](./map-get-information-from-coordinate.md)
* [Anzeigen einer Wegbeschreibung von A nach B](./map-route.md)

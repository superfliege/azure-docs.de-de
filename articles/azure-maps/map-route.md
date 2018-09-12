---
title: Anzeigen von Wegbeschreibungen mit Azure Maps | Microsoft-Dokumentation
description: Anzeigen von Wegbeschreibungen zwischen zwei Positionen in einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 80abd6db9888524aa6a66d9861d8dc2d05188e19
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43781495"
---
# <a name="show-directions-from-a-to-b"></a>Anzeigen einer Wegbeschreibung von A nach B 

In diesem Artikel erfahren Sie, wie Sie eine Route anfordern und die Route auf der Karte anzeigen. 

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='500' scrolling='no' title='Anzeigen einer Wegbeschreibung von A nach B auf einer Karte (Dienstmodul)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/RBZbep/'>Show directions from A to B on a map (Service Module)</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Mit der Zeile im zweiten Codeblock wird ein Dienstclient instanziiert.

Der dritte Codeblock initialisiert die [Linestring-Ebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) auf der Karte.

Der vierte Codeblock erstellt Stecknadeln und fügt diese auf der Karte hinzu, um den Start- und Endpunkt der Route zu markieren. Eine Anleitung finden Sie unter [Hinzufügen von Stecknadeln zur Karte](map-add-pin.md).

Im nächsten Codeblock wird die Funktion [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) der Map-Klasse verwendet, um den Begrenzungsrahmen der Karte basierend auf dem Start- und Endpunkt der Route festzulegen.

Der sechste Codeblock erstellt eine Routenabfrage.

Der letzte Codeblock fragt den Azure Maps-Routendienst über die Methode [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) ab, um eine Route zwischen dem Start- und Endpunkt abzurufen. Die Antwort wird dann im GeoJSON Format mithilfe der Methode [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) analysiert. Alle Linien werden dann zur Karte hinzugefügt, um die Route zu rendern. Weitere Informationen finden Sie unter [Hinzufügen einer Linie zur Karte](./map-add-shape.md#addALine).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 

* [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections)
* [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes)
* [Linestring-Ebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln: 
* [Anzeigen von Datenverkehr auf einer Karte](./map-show-traffic.md)
* [Interagieren mit der Karte – Mausereignisse](./map-events.md)

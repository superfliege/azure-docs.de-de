---
title: Anzeigen von Wegbeschreibungen mit Azure Maps | Microsoft-Dokumentation
description: Anzeigen von Wegbeschreibungen zwischen zwei Positionen in einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: ed522779f5a86e38ee12a246cea9ac85d0379f9e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729054"
---
# <a name="show-directions-from-a-to-b"></a>Anzeigen einer Wegbeschreibung von A nach B

In diesem Artikel erfahren Sie, wie Sie eine Route anfordern und die Route auf der Karte anzeigen.

Dafür gibt es zwei Möglichkeiten. Die erste Möglichkeit besteht darin, die [Azure Maps-Routen-API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) über ein Dienstmodul abzufragen. Die zweite Möglichkeit ist, [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die API zu senden. Beide Möglichkeiten werden im Folgenden erläutert.

## <a name="query-the-route-via-service-module"></a>Abfragen der Route über das Dienstmodul

<iframe height='500' scrolling='no' title='Anzeigen einer Wegbeschreibung von A nach B auf einer Karte (Dienstmodul)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/RBZbep/'>Show directions from A to B on a map (Service Module)</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Mit der Zeile im zweiten Codeblock wird ein Dienstclient instanziiert.

Der dritte Codeblock initialisiert die [Linestring-Ebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) auf der Karte.

Der vierte Codeblock erstellt Stecknadeln und fügt diese auf der Karte hinzu, um den Start- und Endpunkt der Route zu markieren. Eine Anleitung zum Verwenden von [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins) finden Sie unter [Hinzufügen von Stecknadeln zur Karte](map-add-pin.md).

Im nächsten Codeblock wird die Funktion [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) der Map-Klasse verwendet, um den Begrenzungsrahmen der Karte basierend auf dem Start- und Endpunkt der Route festzulegen.

Der sechste Codeblock erstellt eine Routenabfrage.

Der letzte Codeblock fragt den Azure Maps-Routendienst über die Methode [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) ab, um eine Route zwischen dem Start- und Endpunkt abzurufen. Die Antwort wird dann im GeoJSON Format mithilfe der Methode [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) analysiert. Alle Linien werden dann zur Karte hinzugefügt, um die Route zu rendern. Weitere Informationen finden Sie unter [Hinzufügen einer Linie zur Karte](./map-add-shape.md#addALine).

## <a name="query-the-route-via-xmlhttprequest"></a>Abfragen der Route über XMLHttpRequest

<iframe height='500' scrolling='no' title='Anzeigen einer Wegbeschreibung von A nach B auf einer Karte' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Show directions from A to B on a map</a> (Anzeigen einer Wegbeschreibung von A nach B auf einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt Stecknadeln und fügt diese auf der Karte hinzu, um den Start- und Endpunkt der Route zu markieren. Eine Anleitung zum Verwenden von [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins) finden Sie unter [Hinzufügen von Stecknadeln zur Karte](map-add-pin.md).

Im dritten Codeblock wird die Funktion [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) der Map-Klasse verwendet, um den Begrenzungsrahmen der Karte basierend auf dem Start- und Endpunkt der Route festzulegen.

Der vierte Codeblock sendet eine [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die [Azure Maps-Routen-API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

Der letzte Codeblock analysiert die eingehende Antwort. Für eine erfolgreiche Antwort werden die Breiten- und Längengraddaten für jeden Wegpunkt gesammelt. So wird ein Liniensystem erstellt, indem jeder Wegpunkt mit dem nachfolgenden Wegpunkt verbunden wird. Alle Linien werden dann zur Karte hinzugefügt, um die Route zu rendern. Eine Anleitung finden Sie unter [Hinzufügen einer Linie zur Karte](./map-add-shape.md#addALine).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Die folgenden Artikel enthalten vollständige Codebeispiele:

> [!div class="nextstepaction"]
> [Anzeigen von Datenverkehr auf einer Karte](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Interagieren mit der Karte – Mausereignisse](./map-events.md)

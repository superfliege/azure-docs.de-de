---
title: Anzeigen von Wegbeschreibungen mit Azure Maps | Microsoft-Dokumentation
description: Anzeigen von Wegbeschreibungen zwischen zwei Positionen in einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 5e9ab73ddc16517e17894cddd9bc102f3941f00c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "35772012"
---
# <a name="show-directions-from-a-to-b"></a>Anzeigen einer Wegbeschreibung von A nach B 

In diesem Artikel erfahren Sie, wie Sie eine Route anfordern und die Route auf der Karte anzeigen. 

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='500' scrolling='no' title='Anzeigen einer Wegbeschreibung von A nach B auf einer Karte' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Show directions from A to B on a map</a> (Anzeigen einer Wegbeschreibung von A nach B auf einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt Stecknadeln und fügt diese auf der Karte hinzu, um den Start- und Endpunkt der Route zu markieren. Eine Anleitung finden Sie unter [Hinzufügen einer Stecknadel zur Karte](map-add-pin.md).

Im dritten Codeblock wird die Funktion [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds) der Map-Klasse verwendet, um den Begrenzungsrahmen der Karte basierend auf dem Start- und Endpunkt der Route festzulegen.

Der vierte Codeblock sendet eine [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die [Azure Maps-Routen-API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

Der letzte Codeblock analysiert die eingehende Antwort. Für eine erfolgreiche Antwort werden die Breiten- und Längengraddaten für jeden Wegpunkt gesammelt. So wird ein Liniensystem erstellt, indem jeder Wegpunkt mit dem nachfolgenden Wegpunkt verbunden wird. Alle Linien werden dann zur Karte hinzugefügt, um die Route zu rendern. Eine Anleitung finden Sie unter [Hinzufügen einer Linie zur Karte](./map-add-shape.md#addALine).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 

* [Map](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln: 
* [Anzeigen von Datenverkehr auf einer Karte](./map-show-traffic.md)
* [Interagieren mit der Karte – Mausereignisse](./map-events.md)

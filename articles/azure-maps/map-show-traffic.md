---
title: Anzeigen von Datenverkehr mit Azure Maps | Microsoft-Dokumentation
description: Anzeigen von Datenverkehrsdaten auf einer JavaScript-basierten Karte
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6d5c721ab84c28bae9415dceeaa09fd12cc05824
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733017"
---
# <a name="show-traffic-on-the-map"></a>Anzeigen von Datenverkehr auf einer Karte

Dieser Artikel erläutert, wie Sie Datenverkehrs- und Incidentinformationen auf einer Karte anzeigen.

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='456' scrolling='no' title='Anzeigen von Datenverkehr auf einer Karte' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Show traffic on a map</a> (Anzeigen von Datenverkehr auf einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](map-create.md).

Der zweite Codeblock verwendet die [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#settraffic)-Funktion der map-Klasse, um die Datenverkehrsflüsse und Incidents auf der Karte zu rendern.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Die folgenden Artikel enthalten vollständige Codebeispiele:

> [!div class="nextstepaction"]
> [Seite für Beispielcode](http://aka.ms/AzureMapsSamples)

Verbessern der Benutzerfreundlichkeit:

> [!div class="nextstepaction"]
> [Karteninteraktion mit Mausereignissen](./map-events.md)

> [!div class="nextstepaction"]
> [Erstellen einer zugänglichen Karte](./map-accessibility.md)

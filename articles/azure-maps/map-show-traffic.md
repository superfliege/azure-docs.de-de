---
title: Anzeigen von Datenverkehr mit Azure Maps | Microsoft-Dokumentation
description: Anzeigen von Datenverkehrsdaten auf einer JavaScript-basierten Karte
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6ff7a0270509c244fc97bd04d8ba648fd262dc58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600118"
---
# <a name="show-traffic-on-the-map"></a>Anzeigen von Datenverkehr auf einer Karte

Dieser Artikel erläutert, wie Sie Datenverkehrs- und Incidentinformationen auf einer Karte anzeigen. 

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='456' scrolling='no' title='Anzeigen von Datenverkehr auf einer Karte' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Show traffic on a map</a> (Anzeigen von Datenverkehr auf einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der erste Block des oben gezeigten Codes erstellt ein Kartenobjekt. Eine Anleitung finden Sie unter [Erstellen einer Karte](map-create.md).

Der zweite Codeblock verwendet die [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic)-Funktion der map-Klasse, um die Datenverkehrsflüsse und Incidents auf der Karte zu rendern.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic)

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln: 
* [Interacting with the map – mouse events](./map-events.md) (Interagieren mit der Karte – Mausereignisse)
* [Erstellen einer zugänglichen Karte](./map-accessibility.md)

Weitere Kartenszenarien finden Sie auf unserer [Codebeispielseite](http://aka.ms/AzureMapsSamples).

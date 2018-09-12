---
title: Hinzufügen von Kartensteuerelementen in Azure Maps | Microsoft-Dokumentation
description: Hinzufügen von Steuerelementen für Zoom, Neigung und Drehen sowie einer Stilauswahl in Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 08/29/2018
ms.topic: how-to-guides
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 435c6545b69b4457c3e1035fb8125399d4c9c23a
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666130"
---
# <a name="add-map-controls-to-azure-maps"></a>Hinzufügen von Kartensteuerelementen zu Azure Maps

In diesem Artikel wird gezeigt, wie Sie einer Karte Kartensteuerelemente hinzufügen. Außerdem erfahren Sie, wie Sie eine Karte mit allen Steuerelementen und einer [Stilauswahl](https://docs.microsoft.com/azure/azure-maps/choose-map-style#adding-the-style-picker) erstellen.

## <a name="add-zoom-control"></a>Hinzufügen eines Zoomsteuerelements

<iframe height='500' scrolling='no' title='Hinzufügen eines Zoomsteuerelements' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>Adding a zoom control</a> (Hinzufügen eines Zoomsteuerelements) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der erste Codeblock im obigen Code erstellt ein Kartenobjekt. Anweisungen zum Erstellen einer Karte finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt ein Zoomsteuerelement-Objekt mit dem Atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest).

Das Zoomsteuerelement fügt die Möglichkeit hinzu, die Karte zu vergrößern und zu verkleinern. Der dritte Block fügt der Karte ein Zoomsteuerelement mithilfe der [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)-Methode von Maps hinzu.

## <a name="add-pitch-control"></a>Hinzufügen eines Neigungssteuerelements

<iframe height='500' scrolling='no' title='Hinzufügen eines Neigungssteuerelements' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>Adding a pitch control</a> (Hinzufügen eines Neigungssteuerelements) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des obigen Codes wird ein Kartenobjekt mit auf „Graustufe“ festgelegtem Stil erstellt. Anweisungen zum Erstellen einer Karte finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt ein Neigungssteuerelement-Objekt mit dem Atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest).

Das Neigungssteuerelement fügt die Möglichkeit hinzu, die Neigung der Karte zu ändern. Der dritte Block fügt der Karte ein Neigungssteuerelement mithilfe der [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)-Methode von Maps hinzu.

## <a name="add-compass-control"></a>Hinzufügen eines Kompasssteuerelements

<iframe height='500' scrolling='no' title='Hinzufügen eines Drehsteuerelements' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>Adding a rotate control</a> (Hinzufügen eines Drehsteuerelements) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der erste Codeblock im obigen Code erstellt ein Kartenobjekt. Anweisungen zum Erstellen einer Karte finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt ein Drehsteuerelement-Objekt mit dem Atlas [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol). Außerdem fügt er der Karte ein Kompasssteuerelement mithilfe der [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)-Methode von Maps hinzu.

## <a name="a-map-with-all-controls"></a>Eine Karte mit allen Steuerelementen

<iframe height='500' scrolling='no' title='Eine Karte mit allen Steuerelementen' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>A map with all the controls</a> (Eine Karte mit allen Steuerelementen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der erste Codeblock im obigen Code erstellt ein Kartenobjekt. Anweisungen zum Erstellen einer Karte finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt ein Kompasssteuerelement-Objekt mit dem Atlas [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol) und fügt es der Karte mithilfe der [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)-Methode von Maps hinzu.

Der dritte Codeblock erstellt ein Zoomsteuerelement-Objekt mit dem Atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest) und fügt es der Karte mithilfe der [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)-Methode von Maps hinzu.

Der vierte Codeblock erstellt ein Neigungssteuerelement-Objekt mit dem Atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest) und fügt es der Karte mithilfe der [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)-Methode von Maps hinzu.

Der letzte Codeblock erstellt ein Stilauswahlobjekt mit dem Atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) und fügt es der Karte mithilfe der [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)-Methode von Maps hinzu.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol)
    * [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest)
    * [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln: 
* [Hinzufügen von Stecknadeln zur Karte](./map-add-pin.md)
* [Hinzufügen eines Popups](./map-add-popup.md)
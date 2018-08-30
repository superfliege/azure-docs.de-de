---
title: Hinzufügen eines Popupfensters mit Azure Maps | Microsoft-Dokumentation
description: Hinzufügen eines Popupfensters zu einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 0f86578e33e5c6a2d6528e2deb1c8068a0c94d01
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2018
ms.locfileid: "42747104"
---
# <a name="add-a-popup-to-the-map"></a>Hinzufügen eines Popupfensters zu der Karte

In diesem Artikel wird gezeigt, wie Sie ein Popupfenster zu einer Karte hinzufügen.  

## <a name="understand-the-code"></a>Grundlegendes zum Code

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Hinzufügen eines Popupfensters zu einer Karte' src='//codepen.io/azuremaps/embed/zRyKxj/?height=545&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/zRyKxj/'>Add a popup to a map</a> (Hinzufügen eines Popupfensters zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt eine Stecknadel und fügt diese zur Karte hinzu. Eine Anleitung finden Sie unter [Hinzufügen einer Stecknadel zur Karte](./map-add-pin.md).

Der dritte Codeblock erzeugt den Inhalt, der im Popupfenster angezeigt wird. Beim Popupinhalt handelt es sich um ein HTML-Element. 

Der vierte Codeblock erstellt ein [Popupobjekt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) über `new atlas.Popup()`. Popupeigenschaften, z.B. Inhalte und Position, sind Teil der [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.popupoptions?view=azure-iot-typescript-latest). „PopupOptions“ können Sie im Popupkonstruktor oder über die Funktion [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) der Popup-Klasse definieren.

Im letzten Codeblock wird mithilfe der Funktion [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) der Map-Klasse nach Mouseoverereignissen an den Stecknadeln gelauscht. Zudem wird die Funktion [open](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) der Popup-Klasse verwendet, um bei einem Ereignis das Popupfenster zu öffnen.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 

* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener)
* [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [open](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open)
    * [close](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#close)
    
Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln: 
* [Hinzufügen einer Form](./map-add-shape.md)
* [Hinzufügen von benutzerdefinierter HTML](./map-add-custom-html.md)

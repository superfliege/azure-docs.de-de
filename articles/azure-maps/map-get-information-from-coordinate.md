---
title: Anzeigen von Informationen über eine Koordinate mit Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Informationen zu einer Adresse auf der Karte anzeigen, wenn ein Benutzer eine Koordinate auswählt.
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 331e687c40f21b0bf6074239969848c632682773
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43667480"
---
# <a name="get-information-from-a-coordinate"></a>Abrufen von Informationen von einer Koordinate

In diesem Artikel erfahren Sie, wie Sie eine umgekehrte Adresssuche ausführen, und wie Sie per Mausklick die Adresse der angeklickten Position in einem Popupfenster anzeigen.

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='500' scrolling='no' title='Abrufen von Informationen von einer Koordinate (Dienstmodul)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>Get information from a coordinate (Service Module)</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Mit der Zeile im zweiten Codeblock wird ein Dienstclient instanziiert.

Der dritte Codeblock aktualisiert den Stil des Mauscursors in einen Zeiger.

Der vierte Codeblock erstellt ein Popupfenster. Eine Anleitung finden Sie unter [Hinzufügen eines Popupfensters zur Karte](./map-add-popup.md).

Der letzte Codeblock fügt einen Ereignislistener für Mausklicks hinzu. Bei einem Mausklick wird eine Suchabfrage mit den Koordinaten des Punkts erstellt, auf den geklickt wurde. Anschließend wird der [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse)-Endpunkt der Karte zum Abfragen der Adresse für die Koordinaten verwendet.

Für eine erfolgreiche Antwort wird die Adresse für die angeklickte Position gesammelt sowie der Popupinhalt und die Position über die Funktion [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) der Popupklasse definiert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 
* [Umgekehrte Adresssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener)
* [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [open](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open)
    * [close](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#close)

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln:
* [Anzeigen einer Wegbeschreibung von A nach B](./map-route.md)
* [Anzeigen des Verkehrs](./map-show-traffic.md)

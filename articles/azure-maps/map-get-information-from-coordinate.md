---
title: Anzeigen von Informationen über eine Koordinate mit Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Informationen zu einer Adresse auf der Karte anzeigen, wenn ein Benutzer eine Koordinate auswählt.
author: jingjing-z
ms.author: jinzh
ms.date: 11/15/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: ada579af44d1d0b4ea08a8ae9eadbec386e44f08
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2018
ms.locfileid: "51823009"
---
# <a name="get-information-from-a-coordinate"></a>Abrufen von Informationen von einer Koordinate

In diesem Artikel wird gezeigt, wie Sie eine inverse Adresssuche durchführen, die die Adresse einer angeklickten Popup-Position anzeigt.

Es gibt zwei Möglichkeiten, eine inverse Adresssuche durchzuführen. Eine Möglichkeit besteht darin, die [Azure Maps-API für eine inverse Adresssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) über ein Dienstmodul abzufragen. Die andere Möglichkeit besteht darin, ein [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die API zu senden, um eine Adresse zu finden. Beide Möglichkeiten werden im Folgenden untersucht.

## <a name="make-a-reverse-search-request-via-service-module"></a>Durchführen einer inversen Suchanforderung über das Dienstmodul

<iframe height='500' scrolling='no' title='Abrufen von Informationen von einer Koordinate (Dienstmodul)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>Get information from a coordinate (Service Module)</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Mit der Zeile im zweiten Codeblock wird ein Clientdienst instanziiert.

Der dritte Codeblock aktualisiert den Stil des Mauscursors in einen Zeiger und ein Objekt vom Typ [popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Eine Anleitung finden Sie unter [Hinzufügen eines Popupfensters zur Karte](./map-add-popup.md).

Der vierte Codeblock fügt einen [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) für Mausklicks hinzu. Nach einem Mausklick wird eine Suchabfrage mit den Koordinaten des Punkts erstellt, auf den geklickt wurde. Anschließend wird der Endpunkt [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) der Karte verwendet, um die Adresse für die Koordinaten abzufragen.

Für eine erfolgreiche Antwort wird die Adresse für die angeklickte Position gesammelt sowie der Popupinhalt und die Position über die Funktion [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) der Popupklasse definiert.

Die Änderung des Cursors, ein Popup-Objekt und das Klickereignis werden jeweils im [Ladeereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) der Karte erstellt, um sicherzustellen, dass die Karte vollständig geladen wurde, bevor die Koordinateninformationen abgerufen werden können.

## <a name="make-a-reverse-search-request-via-xmlhttprequest"></a>Durchführen einer inversen Suchanforderung über XMLHttpRequest

<iframe height='500' scrolling='no' title='Abrufen von Informationen von einer Koordinate' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>Get information from a coordinate</a> (Abrufen von Informationen von einer Koordinate) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des obigen Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock aktualisiert den Stil des Mauscursors in einen Zeiger und ein Objekt vom Typ [popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Eine Anleitung finden Sie unter [Hinzufügen eines Popupfensters zur Karte](./map-add-popup.md).

Der dritte Codeblock fügt einen Ereignislistener für Mausklicks hinzu. Nach einem Mausklick sendet dieser eine Anforderung vom Typ [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die [Azure Maps-API für eine umgekehrte Adresssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), um die Adresse für die Koordinaten abzufragen, auf die geklickt wurde. Für eine erfolgreiche Antwort wird die Adresse für die angeklickte Position gesammelt sowie der Popupinhalt und die Position über die Funktion [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) der Popupklasse definiert.

Die Änderung des Cursors, ein Popup-Objekt und das Klickereignis werden jeweils im [Ladeereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) der Karte erstellt, um sicherzustellen, dass die Karte vollständig geladen wurde, bevor die Koordinateninformationen abgerufen werden können.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Die folgenden Artikel enthalten vollständige Codebeispiele:

> [!div class="nextstepaction"]
> [Anzeigen einer Wegbeschreibung von A nach B](./map-route.md)

> [!div class="nextstepaction"]
> [Anzeigen des Verkehrs](./map-show-traffic.md)

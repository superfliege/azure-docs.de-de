---
title: Anzeigen von Suchergebnissen mit Azure Maps | Microsoft-Dokumentation
description: Hier wird erläutert, wie Sie eine Suchanfrage mit Azure Maps ausführen und anschließend die Ergebnisse auf einer Javascrip-Karte anzeigen.
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b77737b16b23ed00c8f12f84e6a8558a665a7d15
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599880"
---
# <a name="show-search-results-on-the-map"></a>Anzeigen von Suchergebnissen auf der Karte

In diesem Artikel erfahren Sie, wie Sie eine Suchanfrage ausführen und die Suchergebnisse auf der Karte anzeigen. 

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='500' scrolling='no' title='Anzeigen von Suchergebnissen auf einer Karte' src='//codepen.io/azuremaps/embed/KQbaeM/?height=519&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Show search results on a map</a> (Anzeigen von Suchergebnissen auf einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der erste Block des oben gezeigten Codes erstellt ein Kartenobjekt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt eine Ebene mit Suchstecknadeln für die Karte und fügt sie hinzu. Eine Anleitung finden Sie unter [Hinzufügen von Stecknadeln zur Karte](./map-add-pin.md).

Der dritte Codeblock sendet [XMLHttpRequest](https://xhr.spec.whatwg.org/) an die [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

Der letzte Codeblock analysiert die eingehende Antwort. Für eine erfolgreiche Antwort werden die Breiten- und Längengraddaten für jeden zurückgegebenen Ort gesammelt. Alle Ortspunkte werden als Stecknadeln zur Karte hinzugefügt und die Grenzen der Karte zum Rendern aller Stecknadeln angepasst.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 

* [Azure Maps-API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    
Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln: 
* [Abrufen von Informationen von einer Koordinate](./map-get-information-from-coordinate.md)
* [Anzeigen einer Wegbeschreibung von A nach B](./map-route.md)

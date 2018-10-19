---
title: Hinzufügen von benutzerdefiniertem HTML-Code in Azure Maps | Microsoft-Dokumentation
description: Hinzufügen von benutzerdefiniertem HTML-Code zu einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: e5cfbc7ddc10edf9b21afce73e3b7f8795fcdac9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121956"
---
# <a name="add-custom-html-to-the-map"></a>Hinzufügen von benutzerdefiniertem HTML-Code zu der Karte

In diesem Artikel erfahren Sie, wie Sie einen benutzerdefinierten HTML-Code, wie eine Bilddatei, zur Karte hinzufügen.

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='466' scrolling='no' title='Hinzufügen von benutzerdefiniertem HTML-Code zu einer Karte – PNG' src='//codepen.io/azuremaps/embed/MVoeVw/?height=466&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>Add custom html to a map - png</a> (Hinzufügen von benutzerdefiniertem HTML-Code zu einer Karte – PNG) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt ein HTML-Element aus einem Bild.

Im letzten Codeblock wird die [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addhtml)-Funktion der Map-Klasse verwendet, um das Bild an der angegebenen Position auf der Karte hinzuzufügen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [Anzeigen von Suchergebnissen](./map-search-location.md)

> [!div class="nextstepaction"]
> [Abrufen von Informationen von einer Koordinate](./map-get-information-from-coordinate.md)
---
title: Hinzufügen eines Popupfensters mit Azure Maps | Microsoft-Dokumentation
description: Hinzufügen eines Popupfensters zu einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 11/09/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: f56e15e12bc176e6b6837e144494599ea4fb5403
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282531"
---
# <a name="add-a-popup-to-the-map"></a>Hinzufügen eines Popupfensters zu der Karte

In diesem Artikel wird gezeigt, wie Sie ein Popupfenster einem Punkt auf einer Karte hinzufügen.

## <a name="understand-the-code"></a>Grundlegendes zum Code

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Hinzufügen eines Popupfensters mit Azure Maps' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sehen Sie sich bei <a href='https://codepen.io'>CodePen</a> die Informationen unter <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Add a pop up using Azure Maps</a> (Hinzufügen eines Popupfensters mit Azure Maps) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) an.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md). Außerdem wird HTML-Inhalt erstellt, der im Popupfenster angezeigt wird.

Im zweiten Codeblock wird mit der [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)-Klasse ein Datenquellenobjekt erstellt. Ein Punkt ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) der [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest)-Klasse. Anschließend wird ein Punktobjekt mit einem Namen und Beschreibungseigenschaften erstellt und der Datenquelle hinzugefügt.

Eine [Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) verwendet Text oder Symbole zum Rendern punktbasierter Daten, die in [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als Symbole auf der Karte umschlossen sind.  Im dritten Codeblock wird eine Symbolebene erstellt. Die Datenquelle wird der Symbolebene hinzugefügt, die dann der Karte hinzugefügt wird.

Der vierte Codeblock erstellt ein [Popupobjekt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) über `new atlas.Popup()`. Popupeigenschaften, z.B. „position“ und „pixelOffset“ sind Teil von [PopupOptions](/javascript/api/azure-maps-control/atlas.popupoptions). „PopupOptions“ können Sie im Popupkonstruktor oder über die Funktion [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) der Popup-Klasse definieren. Anschließend wird ein `mouseover`-Ereignislistener für die Symbolebene erstellt.

Mit dem letzten Codeblock wird eine Funktion erstellt, die mit dem `mouseover`-Ereignislistener ausgelöst wird. Damit werden der Inhalt und die Eigenschaften des Popupfensters festgelegt, und das Popupobjekt wird der Karte hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Die folgenden interessanten Artikel enthalten vollständige Codebeispiele:

> [!div class="nextstepaction"]
> [Hinzufügen einer Form](./map-add-shape.md)

> [!div class="nextstepaction"]
> [Hinzufügen von benutzerdefinierter HTML](./map-add-custom-html.md)

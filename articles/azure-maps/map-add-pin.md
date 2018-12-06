---
title: Hinzufügen von Symbolen und Markern mit Azure Maps | Microsoft-Dokumentation
description: Hinzufügen von Symbolen und Markern zu einer Javascript-Karte
author: walsehgal
ms.author: v-musehg
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c56ac35f49c364b7b0f2ad26b82b178411419414
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282684"
---
# <a name="add-symbols-and-markers-to-a-map"></a>Hinzufügen von Symbolen und Markern zu einer Karte

In diesem Artikel erfahren Sie, wie Sie einer Karte mithilfe einer Datenquelle Symbole und Marker hinzufügen.

## <a name="add-a-symbol-marker"></a>Hinzufügen eines Symbolmarkers

<iframe height='500' scrolling='no' title='Ändern der Stecknadel eines Standorts' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>Ändern der Stecknadel eines Standorts</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des obigen Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird mit der Klasse [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) ein Datenquellenobjekt erstellt. Dann wird ein Punkt erstellt und der Datenquelle hinzugefügt. Ein Punkt ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest)-Objekt von [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest).

Der dritte Codeblock erstellt einen [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) und aktualisiert die Koordinaten des Punktes bei Mausklick mit der [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest#setcoordinates)-Methode der Shape-Klasse.

Eine [Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) verwendet Text oder Symbole zum Rendern punktbasierter Daten, die in [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als Symbole auf der Karte umschlossen sind.  Die Datenquelle, der Click-Ereignislistener und die Symbolebene werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit der Punkt angezeigt wird, nachdem die Karte vollständig geladen wurde.

## <a name="add-a-custom-symbol"></a>Hinzufügen eines benutzerdefinierten Symbols

<iframe height='500' scrolling='no' title='DataSource „HTML“' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>DataSource „HTML“</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock fügt der Karte mit der [markers](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers)-Eigenschaft der [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)-Klasse ein [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)-Objekt hinzu. Das HtmlMarker-Objekt wird der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit es angezeigt wird, nachdem die Karte vollständig geladen wurde.

## <a name="add-bubble-markers"></a>Hinzufügen von Blasenmarkern

<iframe height='500' scrolling='no' title='DataSource „BubbleLayer“' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>DataSource „BubbleLayer“</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird ein Array von Positionen definiert und ein [MultiPoint](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.multipoint?view=azure-iot-typescript-latest)-Objekt erstellt. Ein Datenquellenobjekt wird dann mit der [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)-Klasse erstellt, und das MultiPoint-Objekt wird der Datenquelle hinzugefügt.

Eine [Blasenebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) rendert punktbasierte Daten, die von der [Datenquelle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) umschlossen sind, als Kreise auf der Karte. Der letzte Codeblock erstellt eine Blasenebene und fügt sie der Karte hinzu. Die Eigenschaften einer Blasenebene finden Sie unter [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

Das MultiPoint-Objekt, die Datenquelle und die Blasenebene werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit der Kreis angezeigt wird, nachdem die Karte vollständig geladen wurde.

## <a name="add-bubble-markers-with-label"></a>Hinzufügen von Blasenmarkern mit Beschriftungen

<iframe height='500' scrolling='no' title='DataSource „MultiLayer“' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>DataSource „MultiLayer“</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der obige Code zeigt, wie Sie Daten auf der Karte visualisieren und beschriften können. Der erste Block des obigen Codes erstellt ein Kartenobjekt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Der zweite Codeblock erstellt ein [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest)-Objekt. Dann erstellt er ein Datenquellenobjekt mithilfe der [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)-Klasse und fügt den Punkt der Datenquelle hinzu.

Eine [Blasenebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) rendert punktbasierte Daten, die von der [Datenquelle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) umschlossen sind, als Kreise auf der Karte. Der dritte Codeblock erstellt eine Blasenebene und fügt sie der Karte hinzu. Die Eigenschaften einer Blasenebene finden Sie unter [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

Eine [Symbolebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) verwendet Text oder Symbole zum Rendern punktbasierter Daten, die in [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) als Symbole auf der Karte umschlossen sind. Der letzte Codeblock erstellt eine Symbolebene und fügt sie der Karte hinzu, die die Beschriftung für die Blase rendert. Die Eigenschaften einer Symbolebene finden Sie unter [SymbolLayerOptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions).

Die Datenquelle und die Ebenen werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit die Daten angezeigt werden, nachdem die Karte vollständig geladen wurde.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Hinzufügen eines Popups](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Form](./map-add-shape.md)
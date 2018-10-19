---
title: Hinzufügen einer Form mit Azure Maps | Microsoft-Dokumentation
description: Hinzufügen einer Form zu einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6b78c7f5d7d1b5ad4db9401bf2138502c9bbd397
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121903"
---
# <a name="add-a-shape-to-a-map"></a>Hinzufügen einer Form zu einer Karte

In diesem Artikel wird gezeigt, wie Sie eine Linie, einen Kreis und ein Polygon zur Karte hinzufügen. 

<a id="addALine"></a>

## <a name="add-a-line"></a>Hinzufügen einer Linie

<iframe height='500' scrolling='no' title='Hinzufügen einer Linie zu einer Karte' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>Add a line to a map</a> (Hinzufügen einer Linie zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

In der zweiten Codeblock wird eine Linie erstellt. Eine Linie ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) von „LineString“ mit „LineStringProperties“ als „Feature“-Eigenschaft. Verwenden Sie `new atlas.data.Feature(new atlas.data.LineString())`, um eine Linie zu erstellen und deren Eigenschaften zu definieren.

Bei einer Linienebene handelt es sich um ein Array von Linien. Im letzten Codeblock wird die Funktion [addLineStrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) der Map-Klasse verwendet, um die Linienebene zur Karte hinzuzufügen und die Eigenschaften der Linienebene zu definieren. Die Eigenschaften einer Linienebene finden Sie unter [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.linestringlayeroptions?view=azure-iot-typescript-latest).

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Hinzufügen eines Kreises

<iframe height='500' scrolling='no' title='Hinzufügen eines Kreises zu einer Karte' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>Add a circle to a map</a> (Hinzufügen eines Kreises zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

In der zweiten Codeblock wird ein Kreis erstellt. Ein Kreis ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) von [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) mit [CircleProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.circleproperties?view=azure-iot-typescript-latest) als „Feature“-Eigenschaft. Verwenden Sie `new atlas.data.Feature(new atlas.data.Point())`, um einen Kreis zu erstellen und dessen Eigenschaften zu definieren.

Eine Kreisebene ist ein Array von Kreisen. Im letzten Codeblock wird die Funktion [addCircle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcircles) der Map-Klasse verwendet, um die Kreisebene zur Karte hinzuzufügen und die Eigenschaften der Kreisebene zu definieren. Die Eigenschaften einer Kreisebene finden Sie unter [CircleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.circlelayeroptions?view=azure-iot-typescript-latest).

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Hinzufügen eines Polygons

<iframe height='500' scrolling='no' title='Hinzufügen eines Polygons zu einer Karte ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Add a polygon to a map</a> (Hinzufügen eines Polygons zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

In der zweiten Codeblock wird ein Polygon erstellt. Ein Polygon ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) von [Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) mit [PolygonProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonproperties?view=azure-iot-typescript-latest) als „Feature“-Eigenschaft. Verwenden Sie `new atlas.data.Feature(new atlas.data.Polygon())`, um ein Polygon zu erstellen und dessen Eigenschaften zu definieren. Geben Sie im Polygonkonstruktor die geordneten Koordinaten des Polygonpfads an.

Eine Polygonebene ist ein Array von Polygonen. Im letzten Codeblock wird die Funktion [addPolygons](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpolygons) der Map-Klasse verwendet, um die Polygonebene zur Karte hinzuzufügen und die Eigenschaften der Polygonebene zu definieren. Die Eigenschaften einer Polygonebene finden Sie unter [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.polygonlayeroptions?view=azure-iot-typescript-latest).

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [Hinzufügen von benutzerdefinierter HTML](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Anzeigen von Suchergebnissen](./map-search-location.md)

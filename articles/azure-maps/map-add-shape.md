---
title: Hinzufügen einer Form mit Azure Maps | Microsoft-Dokumentation
description: Hinzufügen einer Form zu einer JavaScript-Karte
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: codepen
ms.openlocfilehash: fb9ec0713d3db465cf835346465e70c4455b38ff
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="add-a-shape-to-a-map"></a>Hinzufügen einer Form zu einer Karte

In diesem Artikel wird gezeigt, wie Sie eine Linie, einen Kreis und ein Polygon zur Karte hinzufügen. 

<a id="addALine"></a>

## <a name="add-a-line"></a>Hinzufügen einer Linie

<iframe height='500' scrolling='no' title='Hinzufügen einer Linie zu einer Karte' src='//codepen.io/azuremaps/embed/qomaKv/?height=534&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>Add a line to a map</a> (Hinzufügen einer Linie zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

In der zweiten Codeblock wird eine Linie erstellt. Eine Linie ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) von „LineString“ mit „LineStringProperties“ als „Feature“-Eigenschaft. Verwenden Sie `new atlas.data.Feature(new atlas.data.LineString())`, um eine Linie zu erstellen und deren Eigenschaften zu definieren. 

Bei einer Linienebene handelt es sich um ein Array von Linien. Im letzten Codeblock wird die Funktion [addLineStrings](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addlinestrings) der Map-Klasse verwendet, um die Linienebene zur Karte hinzuzufügen und die Eigenschaften der Linienebene zu definieren. Die Eigenschaften einer Linienebene finden Sie unter [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/linestringlayeroptions?view=azure-iot-typescript-latest).

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Hinzufügen eines Kreises

<iframe height='500' scrolling='no' title='Hinzufügen eines Kreises zu einer Karte' src='//codepen.io/azuremaps/embed/PRmzJX/?height=538&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>Add a circle to a map</a> (Hinzufügen eines Kreises zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

In der zweiten Codeblock wird ein Kreis erstellt. Ein Kreis ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) von [Point](https://docs.microsoft.com/javascript/api/azure-maps-javascript/point?view=azure-iot-typescript-latest) mit [CircleProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/circleproperties?view=azure-iot-typescript-latest) als „Feature“-Eigenschaft. Verwenden Sie `new atlas.data.Feature(new atlas.data.Point())`, um einen Kreis zu erstellen und dessen Eigenschaften zu definieren.

Eine Kreisebene ist ein Array von Kreisen. Im letzten Codeblock wird die Funktion [addCircle](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addcircles) der Map-Klasse verwendet, um die Kreisebene zur Karte hinzuzufügen und die Eigenschaften der Kreisebene zu definieren. Die Eigenschaften einer Kreisebene finden Sie unter [CircleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/circlelayeroptions?view=azure-iot-typescript-latest).

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Hinzufügen eines Polygons
<iframe height='500' scrolling='no' title='Hinzufügen eines Polygons zu einer Karte ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Add a polygon to a map</a> (Hinzufügen eines Polygons zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

In der zweiten Codeblock wird ein Polygon erstellt. Ein Polygon ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-javascript/feature?view=azure-iot-typescript-latest) von [Polygon](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygon?view=azure-iot-typescript-latest) mit [PolygonProperties](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygonproperties?view=azure-iot-typescript-latest) als „Feature“-Eigenschaft. Verwenden Sie `new atlas.data.Feature(new atlas.data.Polygon())`, um ein Polygon zu erstellen und dessen Eigenschaften zu definieren. Geben Sie im Polygonkonstruktor die geordneten Koordinaten des Polygonpfads an.

Eine Polygonebene ist ein Array von Polygonen. Im letzten Codeblock wird die Funktion [addPolygons](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpolygons) der Map-Klasse verwendet, um die Polygonebene zur Karte hinzuzufügen und die Eigenschaften der Polygonebene zu definieren. Die Eigenschaften einer Polygonebene finden Sie unter [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/polygonlayeroptions?view=azure-iot-typescript-latest). 

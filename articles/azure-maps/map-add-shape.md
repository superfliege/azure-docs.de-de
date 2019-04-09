---
title: Hinzufügen einer Form mit Azure Maps | Microsoft-Dokumentation
description: Hinzufügen einer Form zu einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7598bbc879351752580247e46bc986ee84fa0d56
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58497243"
---
# <a name="add-a-shape-to-a-map"></a>Hinzufügen einer Form zu einer Karte

Dieser Artikel erläutert, wie Sie der Karte eine [Form](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) hinzufügen und die Eigenschaften einer vorhandenen Form auf der Karte aktualisieren.

<a id="addALine"></a>

## <a name="add-a-line"></a>Hinzufügen einer Linie

<iframe height='500' scrolling='no' title='Hinzufügen einer Linie zu einer Karte' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>Add a line to a map</a> (Hinzufügen einer Linie zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird mit der Klasse [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) ein Datenquellenobjekt erstellt. Ein [LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest)-Objekt wird erstellt und zur Datenquelle hinzugefügt.

Ein [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest)-Objekt rendert Linienobjekte, die von [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) umschlossen sind. Der letzte Codeblock erstellt eine Linienebene und fügt sie der Karte hinzu. Die Eigenschaften einer Linienebene finden Sie unter [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). Die Datenquelle und die Linienebene werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit die Linie angezeigt wird, nachdem die Karte vollständig geladen wurde.

## <a name="customize-a-line-layer"></a>Anpassen einer Linienebene

Die Linienebene verfügt über mehrere Formatierungsoptionen. Hier ist ein Tool, mit dem Sie diese testen können.

<br/>

<iframe height='700' scrolling='no' title='Linienebeneoptionen' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>Line Layer Options</a> (Linienebeneoptionen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle"></a>Hinzufügen eines Kreises

<iframe height='500' scrolling='no' title='Hinzufügen eines Kreises zu einer Karte' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>Add a circle to a map</a> (Hinzufügen eines Kreises zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird mit der Klasse [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) ein Datenquellenobjekt erstellt. Ein Kreis ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) von [Punkt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) und verfügt über eine `subType`-Eigenschaft, die auf „Kreis“ festgelegt ist, sowie und einen Wert für die Eigenschaft `radius`. Wird ein Punkt-Feature mit dem Untertyp „Kreis“ zu einer Datenquelle hinzugefügt, wird diese in ein zirkuläres Polygon in der Karte konvertiert.

Ein [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)-Objekt rendert Daten, die von [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) auf der Karte umschlossen sind. Der letzte Codeblock erstellt eine Polygonebene und fügt sie der Karte hinzu. Die Eigenschaften einer Polygonebene finden Sie unter [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Die Datenquelle und die Polygonebene werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit der Kreis angezeigt wird, nachdem die Karte vollständig geladen wurde.

<a id="addAPolygon"></a>

## <a name="add-a-polygon"></a>Hinzufügen eines Polygons

Es gibt zwei verschiedene Möglichkeiten, der Karte ein Polygon hinzuzufügen. Beide werden in den folgenden Beispielen erläutert.

### <a name="use-polygon-layer"></a>Verwenden der Polygonebene 

<iframe height='500' scrolling='no' title='Hinzufügen eines Polygons zu einer Karte ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Add a polygon to a map</a> (Hinzufügen eines Polygons zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird mit der Klasse [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) ein Datenquellenobjekt erstellt. Ein [Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) wird aus einem Array von Koordinaten erstellt und zur Datenquelle hinzugefügt. 

Ein [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)-Objekt rendert Daten, die von [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) auf der Karte umschlossen sind. Der letzte Codeblock erstellt eine Polygonebene und fügt sie der Karte hinzu. Die Eigenschaften einer Polygonebene finden Sie unter [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Die Datenquelle und die Polygonebene werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit das Polygon angezeigt wird, nachdem die Karte vollständig geladen wurde.

### <a name="use-polygon-and-line-layer"></a>Verwenden der Polygon- und Linienebene

<iframe height='500' scrolling='no' title='Polygon- und Linienebene zum Hinzufügen eines Polygons' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Polygon- und Linienebene zum Hinzufügen eines Polygons</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des oben gezeigten Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird mit der Klasse [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) ein Datenquellenobjekt erstellt. Ein [Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) wird aus einem Array von Koordinaten erstellt und zur Datenquelle hinzugefügt. 

Ein [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)-Objekt rendert Daten, die von [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) auf der Karte umschlossen sind. Die Eigenschaften einer Polygonebene finden Sie unter [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Bei einem [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest)-Objekt handelt es sich um ein Array von Linien. Die Eigenschaften einer Linienebene finden Sie unter [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). Der dritte Codeblock erstellt Polygon- und Linienebenen.

Der letzte Codeblock fügt der Karte die Polygon- und Linienebenen hinzu. Die Datenquelle und die Ebenen werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit das Polygon angezeigt wird, nachdem die Karte vollständig geladen wurde.

## <a name="customize-a-polygon-layer"></a>Anpassen einer Polygonebene

Die Polygonebene verfügt nur über ein paar Formatierungsoptionen. Hier ist ein Tool, mit dem Sie diese testen können.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="update-a-shape"></a>Aktualisieren einer Form

Eine Shape-Klasse umschließt ein [Geometry](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest)- oder [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest)-Objekt und vereinfacht seine Aktualisierung und Verwaltung.
`new Shape(data: Feature<data.Geometry, any>)` erstellt ein Shape-Objekt und initialisiert es mit der angegebenen Funktion.

<br/>

<iframe height='500' scrolling='no' title='Aktualisieren von Formeigenschaften' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>Aktualisieren von Formeigenschaften</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Mit dem ersten Block des obigen Codes wird ein Kartenobjekt erstellt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Ein Punkt ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest)-Objekt der [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest)-Klasse. Der zweite Codeblock initialisiert den Radiuswert für das HTML-Schiebereglerelement und erstellt und umschließt dann ein Point-Objekt in einem [Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest)-Klassenobjekt.

Der dritte Codeblock erstellt eine Funktion, die den Wert aus dem HTML-Bereichsschieberegler-Element übernimmt und den Radiuswert mit der [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest)-Methode der Shape-Klasse ändert.

Im vierten Codeblock wird mit der Klasse [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) ein Datenquellenobjekt erstellt. Dann wird der Punkt der Datenquelle hinzugefügt.

Ein [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)-Objekt rendert Daten, die von [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) auf der Karte umschlossen sind. Der dritte Codeblock erstellt eine Polygonebene. Die Eigenschaften einer Polygonebene finden Sie unter [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Die Datenquelle, der Click-Ereignislistener und die Polygonebene werden erstellt und der Karte innerhalb der Funktion [Ereignislistener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) hinzugefügt, damit der Punkt angezeigt wird, nachdem die Karte vollständig geladen wurde.

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [HTML-Marker](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Wärmebildebene](./map-add-heat-map-layer.md)

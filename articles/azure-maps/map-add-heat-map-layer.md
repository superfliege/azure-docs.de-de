---
title: Hinzufügen einer Wärmebildebene zu Azure Maps | Microsoft-Dokumentation
description: Hinzufügen einer Wärmebildebene zur JavaScript-Karte
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: cbf134640f981056c0996ffc6768ebc1381ce2ac
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2019
ms.locfileid: "53995149"
---
# <a name="add-a-heat-map-layer"></a>Hinzufügen einer Wärmebildebene

Wärmebilder, die auch als Punktdichtekarten bezeichnet werden, stellen eine Art der Datenvisualisierung dar, die verwendet wird, um die Dichte von Daten mit Farbbereichen darzustellen. Sie werden häufig verwendet, um die „Hotspots“ von Daten auf einer Karte anzuzeigen und sind gut zum Rendern von großen Punktdatasets geeignet.  Wenn Sie beispielsweise Zehntausende Punkte in der Kartenansicht als Symbole rendern würden, würden diese den Großteil der Karte bedecken. Zudem wären viele Symbole von anderen verdeckt, sodass ein nützlicher Einblick in die Daten kaum möglich ist. Wenn Sie dasselbe Dataset jedoch als Wärmebild visualisieren, können Sie einfach erkennen, wo die Punktdaten die größte Dichte aufweisen und wie die Dichte im Vergleich mit anderen Bereichen ausfällt. Für Wärmebilder gibt es viele Anwendungsfälle. Nachstehend finden Sie einige Beispiele:

* Temperaturdaten werden üblicherweise als Wärmebild gerendert, da dadurch Näherungswerte zu den Temperaturen zwischen zwei Datenpunkten bestimmt werden können.
* Wenn die Daten von Geräuschsensoren als Wärmebild gerendert werden, kann so nicht nur die Intensität der Geräusche am Standort des Sensors dargestellt werden, sondern auch deren Dissipation über eine gewisse Entfernung. Möglicherweise ist der Geräuschpegel an einem Standort zwar nicht besonders hoch, aber wenn sich die Geräuschebenen von mehreren Sensoren überlappen, können in diesem überlappenden Bereich höhere Geräuschpegel auftreten, die dann im Wärmebild sichtbar wären.
* Die Visualisierung einer GPS-Verfolgung, bei der die Geschwindigkeit als gewichtete Höhenkarte dargestellt wird, wobei die Intensität jedes Datenpunkts auf der Geschwindigkeit basiert, stellt eine gute Möglichkeit dar, um zu veranschaulichen, an welcher Stelle das Fahrzeug beschleunigt wurde.

> [!TIP]
> Blasenebenen rendern in der Standardeinstellung die Koordinaten aller Geometrien in einer Datenquelle. Legen Sie die Eigenschaft `filter` der Ebene auf `['==', '$type', 'Point']` fest, um die Ebene dahingehend zu beschränken, dass nur Punktgeometriefunktionen gerendert werden.

## <a name="add-a-heat-map-layer"></a>Hinzufügen einer Wärmebildebene

Wenn Sie eine Datenquelle, die aus Punkten besteht, als Wärmebild rendern möchten, übergeben Sie diese an eine Instanz der HeatMapLayer-Klasse, und fügen Sie sie der Karte wie im Folgenden dargestellt hinzu.

<br/>

<iframe height='500' scrolling='no' title='Einfache Wärmebildebene' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>Simple Heat Map Layer</a> (Einfache Wärmebildebene) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

In diesem Beispiel weist jeder Wärmepunkt in allen Zoomfaktoren einen Radius von 10 Pixeln auf. Wenn Sie eine Wärmebildebene zur Karte hinzufügen, wird sie in diesem Beispiel unter der Bezeichnungsebene hinzugefügt. Dadurch wird die Benutzeroberfläche verbessert, indem die Bezeichnungen deutlich über dem Wärmebild zu erkennen sind. Die Daten für dieses Beispiel stammen vom [USGS Earthquake Hazards Program](https://earthquake.usgs.gov/) und bestehen aus Punkten, die relevante Erdbeben der letzten 30 Tage darstellen.

## <a name="customizing-the-heat-map-layer"></a>Anpassen der Wärmebildebene

Im vorherigen Beispiel wurde das Wärmebild angepasst, indem der Radius und die Deckkraft angepasst wurden. Auf der Wärmebildebene können jedoch verschiedene Optionen angepasst werden:

* `radius`: Definiert einen Pixelradius, in dem die einzelnen Datenpunkte gerendert werden sollen. Dieser Radius kann als feste Zahl oder als Ausdruck festgelegt werden. Wenn Sie den Radius als Ausdruck festlegen, der den Radius je nach Zoomfaktor anpasst, kann dies dazu führen, dass das Wärmebild scheinbar einen Radius aufweist, der einen gleichbleibenden räumlichen Bereich auf der Karte darstellt.
* `color`: Gibt an, welche Farben für das Wärmebild verwendet werden. Häufig werden Farbpaletten mit Farbverlauf für Wärmebilder verwendet. Sie können jedoch auch abgestufte Farbpaletten verwenden, wenn Sie die Daten des Wärmebilds als Konturdaten darstellen möchten. Durch diese Farbpaletten werden die Farben kleinsten bis zum größten Dichtewert definiert. Die Farbwerte für Wärmebilder werden als Ausdruck im `heatmap-density`-Wert festgelegt. Die Farbe am Index „0“ in einem Ausdruck für eine Farbpalette mit Farbverlauf bzw. die Standardfarbe für eine abgestufte Farbpalette definiert die Farbe in einem Bereich ohne Daten, also die Hintergrundfarbe. Viele Benutzer legen hierfür einen transparenten Wert oder ein halbtransparentes Schwarz fest. Nachfolgend finden Sie zwei Beispiele für Farbausdrücke:

| Ausdruck für eine Farbpalette mit Farbverlauf | Ausdruck für eine abgestufte Farbpalette | 
|---------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['linear'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'purple',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'step',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.25, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.50, 'green',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.75, 'yellow',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1.00, 'red'<br/>\] |   

* `opacity`: Gibt an, wie undurchsichtig oder transparent die Wärmebildebene ist.
* `intensity`: Wendet einen Multiplikator auf die Gewichtung jedes Datenpunkts an, um die Gesamtintensität des Wärmebilds zu erhöhen. Dadurch können kleine Unterschiede in der Gewichtung von Datenpunkten einfacher visualisiert werden.
* `weight`: Standardmäßig weisen alle Datenpunkte eine Gewichtung von 1 auf. Das bedeutet, dass alle Datenpunkte gleichmäßig gewichtet werden. Die Option „weight“ fungiert als Multiplikator und kann als Zahl oder als Ausdruck festgelegt werden. Wenn eine Zahl festgelegt wird, beispielsweise 2, entspricht das der doppelten Platzierung jedes Datenpunkts auf der Karte und somit der Verdoppelung der Dichte. Wenn Sie die Option „weight“ auf eine Zahl festlegen, wird das Wärmebild ähnlich wie mit der Option „intensity“ gerendert. Wenn jedoch ein Ausdruck verwendet wird, kann die Gewichtung der einzelnen Datenpunkte auf unterschiedlichen Faktoren und einer Metrik in den Eigenschaften des Punkts basieren. Im Beispiel der Erdbebendaten steht jeder Datenpunkt für ein Erdbeben, und die Stärke stellt eine wichtige Metrik für jedes Erdbeben dar. Es kommt ständig zu Erdbeben. Die meisten weisen jedoch eine so geringe Stärke auf, dass sie nicht spürbar sind. Wenn Sie den Wert für die Stärke in einem Ausdruck verwenden, um die Option „weight“ zuzuordnen, können relevante Erdbeben besser auf dem Wärmebild dargestellt werden.
* Zusätzlich zu den Basisoptionen für Ebenen (minimaler/maximaler Zoomfaktor, Sichtbarkeit und Filter) ist auch die Option `source` vorhanden, um die Datenquelle zu aktualisieren, und die Option `source-layer`, wenn es sich bei Ihrer Datenquelle um eine Vektorkachelquelle handelt.

Mit diesem Tool können Sie die verschiedenen Optionen für Wärmebildebenen testen.

<br/>

<iframe height='700' scrolling='no' title='Optionen für Wärmebildebenen' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>Line Layer Options</a> (Linienebeneoptionen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Wenn Sie das Clustering für die Datenquelle aktivieren, werden die Punkte, die nahe beieinander liegen, zu einem Punkt gruppiert. Die Anzahl der Punkte von jedem Cluster kann als weight-Ausdruck für das Wärmebild verwendet werden. Somit kann die Anzahl der Punkte reduziert werden, die gerendert werden müssen. Die Anzahl der Punkte in einem Cluster wird wie im Folgenden veranschaulicht in der Eigenschaft „point_count“ gespeichert. 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Wenn der Radius für das Clustering nur wenige Pixel beträgt, gibt es beim Rendern nur geringe sichtbare Unterschiede. Bei einem größeren Radius werden mehr Punkte in jeden Cluster gruppiert, und die Leistung des Wärmebilds wird verbessert. Dafür sind die Unterschiede deutlicher sichtbar.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [HtmlMarker-Klasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions-Schnittstelle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](./map-add-pin.md)


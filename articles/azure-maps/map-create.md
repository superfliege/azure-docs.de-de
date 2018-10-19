---
title: Erstellen einer Karte mit Azure Maps | Microsoft-Dokumentation
description: Erstellen einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9759c4149c6b026837e550dcf3ab0a0156bbb736
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730008"
---
# <a name="create-a-map"></a>Erstellen einer Karte

In diesem Artikel wird gezeigt, wie Sie eine Karte erstellen.  

## <a name="understand-the-code"></a>Grundlegendes zum Code

Es gibt zwei Wege, um eine Karte zu erstellen. Sie können die Kamera der Karte festlegen, indem Sie den Mittelpunkt festlegen und die Zoomstufe angeben. Legen Sie die Kameragrenzen der Karte fest, indem Sie den südwestlichen und den nordöstlichen Grenzpunkt angeben.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Festlegen der Kamera

<iframe height='310' scrolling='no' title='Erstellen einer Karte über „CameraOptions“' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen zum <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>Erstellen einer Karte über `CameraOptions` </a>von Azure Location Based Services (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Im oben gezeigten Code wird über `new atlas.Map()` ein [Map-Objekt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) erstellt. Karteneigenschaften wie Mittelpunkt und Zoomfaktor sind Teil der [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). `CameraOptions` können Sie im Map-Konstruktor oder über die Funktion [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) der Map-Klasse definieren.

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Festlegen der Kameragrenzen

<iframe height='310' scrolling='no' title='Erstellen einer Karte über „CameraBoundsOptions“' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen zum <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>Erstellen einer Karte über `CameraBoundsOptions` </a>von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Im oben gezeigten Code wird über `new atlas.Map()` ein [Map-Objekt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) konstruiert. Karteneigenschaften wie ein Begrenzungsrahmen sind Teil der [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest). `CameraBoundsOptions` können Sie über die Funktion [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) der Map-Klasse definieren.

## <a name="try-out-the-code"></a>Testen Sie den Code

Sehen Sie sich den obigen Beispielcode an. Sie können den JavaScript-Code links auf der Registerkarte **JS** bearbeiten und sich die Änderungen an der Kartenansicht rechts auf der Registerkarte **Ergebnis** ansehen. Außerdem können Sie auf die Schaltfläche **Auf CodePen bearbeiten** klicken, und den Code auf CodePen bearbeiten.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Sehen Sie sich Codebeispiele an, die zeigen, wie Sie Ihrer App Funktionen hinzufügen:

> [!div class="nextstepaction"]
> [Auswählen eines Kartenstils](choose-map-style.md)

> [!div class="nextstepaction"]
> [Hinzufügen von Kartensteuerelementen](map-add-controls.md)

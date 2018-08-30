---
title: Hinzufügen einer Stecknadel mit Azure Maps | Microsoft-Dokumentation
description: Hinzufügen einer Stecknadel zu einer JavaScript-Karte
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: be3c9960961b5c4800faf476bd6a21f7395f057d
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746377"
---
# <a name="add-pins-to-the-map"></a>Hinzufügen von Stecknadeln zur Karte

In diesem Artikel wird gezeigt, wie Sie eine Stecknadel zu einer Karte hinzufügen.  

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='500' scrolling='no' title='Hinzufügen einer Stecknadel zu einer Karte' src='//codepen.io/azuremaps/embed/ZrVpEa/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/ZrVpEa/'>Add a pin to a map</a> (Hinzufügen einer Stecknadel zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Der erste Block des oben gezeigten Codes erstellt ein Kartenobjekt. Eine Anleitung finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird eine Stecknadel erstellt und zur Karte hinzugefügt. Eine Stecknadel ist ein [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.feature?view=azure-iot-typescript-latest) von [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.atlas.data.point?view=azure-iot-typescript-latest) mit [PinProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinproperties?view=azure-iot-typescript-latest) als Funktionseigenschaft. Verwenden Sie `new atlas.data.Feature(new atlas.data.Point())`, um eine Stecknadel zu erstellen und ihre Eigenschaften zu definieren. Bei einer Stecknadelebene handelt es sich um ein Array von Stecknadeln. Verwenden Sie die Funktion [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins) der map-Klasse, um eine Stecknadelebene zur Karte hinzuzufügen und die Eigenschaften der Stecknadelebene zu definieren. Die Eigenschaften einer Stecknadelebene finden Sie unter [PinLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinlayeroptions?view=azure-iot-typescript-latest). 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln: 
* [Hinzufügen eines Popups](./map-add-popup.md)
* [Hinzufügen einer Form](./map-add-shape.md)


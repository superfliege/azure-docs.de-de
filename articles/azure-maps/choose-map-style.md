---
title: Kartenstilfunktionen in Azure Maps | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zu stilbezogenen Funktionen von Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 08/31/2018
ms.topic: how-to-guides
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b95e3c325160d1c0b775320e25fdd40514b02b9f
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666870"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Auswählen eines Kartenstils in Azure Maps
In Azure Maps stehen vier verschiedene Kartenstile zur Auswahl. Weitere Informationen zu Kartenstilen finden Sie unter [Unterstützte Kartenstile in Azure Maps](./supported-map-styles.md). In diesem Artikel wird gezeigt, wie Sie mit den stilbezogenen Funktionen einen Stil für eine geladene Karte festlegen, einen neuen Stil festlegen und das Stilauswahlsteuerelement verwenden.

## <a name="setting-style-on-map-load"></a>Stil für geladene Karte festlegen

<iframe height='500' scrolling='no' title='Festlegen des Stils für eine geladene Karte' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>Setting the style on map load</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

Mit dem obigen Code wird ein Kartenobjekt mit dem auf „Graustufe“ festgelegten Stil erstellt. Anweisungen zum Erstellen einer Karte finden Sie unter [Erstellen einer Karte](./map-create.md).

## <a name="updating-the-style"></a>Aktualisieren des Stils

<iframe height='500' scrolling='no' title='Aktualisieren des Stils' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>Updating the style</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

Der erste Codeblock im obigen Code erstellt ein Kartenobjekt ohne vorab festgelegten Stil. Anweisungen zum Erstellen einer Karte finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird die [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle)-Methode der Karte verwendet, um den Kartenstil auf „Satellit“ festzulegen.

## <a name="adding-the-style-picker"></a>Hinzufügen der Stilauswahl

<iframe height='500' scrolling='no' title='Hinzufügen der Stilauswahl' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Adding the style picker</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

Der erste Codeblock im obigen Code erstellt ein Kartenobjekt ohne vorab festgelegten Stil. Anweisungen zum Erstellen einer Karte finden Sie unter [Erstellen einer Karte](./map-create.md).

Im zweiten Codeblock wird mithilfe des Atlas-[StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)-Konstruktors eine Stilauswahl erstellt.

Mit einer Stilauswahl können Sie den Stil für die Karte auswählen. Im dritten Codeblock wird der Karte mithilfe der [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)-Methode der Karte die Stilauswahl hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden: 
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln:
* [Hinzufügen von Kartensteuerelementen](./map-add-controls.md)
* [Hinzufügen von Stecknadeln zur Karte](./map-add-pin.md)

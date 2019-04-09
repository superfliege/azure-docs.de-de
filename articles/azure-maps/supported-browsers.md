---
title: Unterstützte Webbrowser – Azure Maps | Microsoft-Dokumentation
description: Weitere Informationen zu unterstützten Webbrowsern für das Azure Maps Web SDK
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: 2678fa7c9290c7ec0a27288e7739fde4bb0870fd
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500934"
---
# <a name="supported-web-browsers"></a>Unterstützte Webbrowser

Das Azure Maps Web SDK bietet die Hilfsfunktion [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-), um zu erkennen, ob ein Webbrowser über die minimal erforderlichen WebGL-Funktionen verfügt, um das Laden und Rendering des Kartensteuerelements zu unterstützen. 

```
if(!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if(!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    //Add your map code here.
}
```

Das Azure Maps Web SDK unterstützt die folgenden Webbrowser.

## <a name="desktop"></a>Desktop

- Die aktuelle und vorherige Version von Microsoft Edge 
- Die aktuelle und vorherige Version von Chrome 
- Die aktuelle und vorherige Version von Firefox 
- Die aktuelle und vorherige Version von Safari (Mac OS X) 

Weitere Informationen finden Sie auch unter [Ausrichtung auf Legacy-Browser](#Target-Legacy-Browsers).

## <a name="mobile"></a>Mobile

-  Android
    * Aktuelle Version von Chrome unter Android 6.0 und höher
    * Chrome WebView unter Android 6.0 und höher
- iOS
    * Mobile Safari für die aktuelle und vorherige Hauptversion von iOS
    * UIWebView und WKWebView für die aktuelle und vorherige Hauptversion von iOS
    * Aktuelle Version von Chrome für iOS

> [!TIP]
> Wenn Sie eine Karte in die Ansicht einer mobilen Anwendung einbetten, die ein WebView-Steuerelement enthält, bevorzugen Sie möglicherweise das [npm-Paket des Azure Maps Web SDKs](https://www.npmjs.com/package/azure-maps-control), um auf die von CDN gehostete Version des SDKs zu verweisen. Dadurch wird die Ladezeit verkürzt, da sich das SDK bereits auf dem Gerät des Benutzers befindet und nicht zur Laufzeit heruntergeladen werden muss.

## <a name="nodejs"></a>Node.js

Die folgenden Web SDK-Module werden auch in Node.js unterstützt:

- Modul „Dienste“ ([Dokumentation](how-to-use-services-module.md) | [npm-Modul](https://www.npmjs.com/package/azure-maps-rest))

## <a name="Target-Legacy-Browsers"></a>Ausrichtung auf Legacy-Browser

Wenn Sie ältere Webbrowser als Ziel haben, die WebGL möglicherweise nicht oder nur eingeschränkt unterstützen, wird empfohlen, die Azure Maps Services in Kombination mit einem Open Source-Kartensteuerelement wie [Broschüre](https://leafletjs.com/) zu verwenden. 

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Maps und Broschüre" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure Maps + Leaflet</a> (Azure Maps und Broschüre) in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>
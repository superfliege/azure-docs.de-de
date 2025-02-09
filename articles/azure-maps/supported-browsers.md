---
title: Vom Web SDK unterstützte Browser – Azure Maps | Microsoft-Dokumentation
description: Weitere Informationen zu unterstützten Browsern für das Azure Maps Web SDK
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: 84c5dbcf5073ba8c0ae662af019cde590a9adf10
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686648"
---
# <a name="web-sdk-supported-browsers"></a>Web SDK: unterstützte Browser

Das Web SDK von Azure Maps bietet eine Hilfsfunktion namens [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-). Diese Funktion erkennt, ob ein Webbrowser über den minimalen Satz an WebGL-Funktionen verfügt, die zum Laden und Rendern des Kartensteuerelements erforderlich sind. Im folgenden Ausschnitt wird die Verwendung der Funktion veranschaulicht:

```
if(!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if(!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    // Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Desktop

Das Web SDK von Azure Maps unterstützt die folgenden Desktopbrowser:

- Microsoft Edge (aktuelle und vorherige Version)
- Google Chrome (aktuelle und vorherige Version)
- Mozilla Firefox (aktuelle und vorherige Version)
- Apple Safari (Mac OS X) (aktuelle und vorherige Version)

Siehe auch [Ausrichtung auf Legacy-Browser](#Target-Legacy-Browsers) weiter unten in diesem Artikel.

## <a name="mobile"></a>Mobile

Das Web SDK von Azure Maps unterstützt die folgenden mobilen Browser:

- Android
  - Aktuelle Version von Chrome unter Android 6.0 und höher
  - Chrome WebView unter Android 6.0 und höher
- iOS
  - Mobile Safari für die aktuelle und vorherige Hauptversion von iOS
  - UIWebView und WKWebView für die aktuelle und vorherige Hauptversion von iOS
  - Aktuelle Version von Chrome für iOS

> [!TIP]
> Wenn Sie eine Karte über ein WebView-Steuerelement in eine mobile Anwendung einbetten, sollten Sie überlegen, das [npm-Paket des Web SDK von Azure Maps](https://www.npmjs.com/package/azure-maps-control) zu verwenden, statt auf die von Azure Content Delivery Network gehostete Version des SDK zu verweisen. Dieser Ansatz verkürzt die Ladedauer, da sich das SDK bereits auf dem Gerät des Benutzers befindet und nicht zur Laufzeit heruntergeladen werden muss.

## <a name="nodejs"></a>Node.js

Die folgenden Web SDK-Module werden auch in Node.js unterstützt:

- Modul „Dienste“ ([Dokumentation](how-to-use-services-module.md) | [npm-Modul](https://www.npmjs.com/package/azure-maps-rest))

## <a name="Target-Legacy-Browsers"></a>Ausrichtung auf Legacy-Browser

Möglicherweise möchten Sie ältere Browser als Ziel verwenden, die WebGL nicht oder nur eingeschränkt unterstützen. In solchen Fällen empfehlen wir, die Azure Maps-Dienste zusammen mit einem Open-Source-Kartensteuerelement wie [Leaflet](https://leafletjs.com/) zu verwenden. Hier sehen Sie ein Beispiel:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Maps und Broschüre" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure Maps + Leaflet</a> (Azure Maps und Broschüre) in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Web SDK von Azure Maps:

> [!div class="nextstepaction"]
> [Kartensteuerelement](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Services-Modul](how-to-use-services-module.md)

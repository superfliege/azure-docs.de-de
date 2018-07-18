---
title: Erstellen einer zugänglichen Anwendung mit Azure Maps | Microsoft-Dokumentation
description: Erstellen einer zugänglichen Anwendung mithilfe von Azure Maps
services: azure-maps
keywords: ''
author: chgennar
ms.author: chgennar
ms.date: 05/18/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.openlocfilehash: 2523287669628b8c58028cae9887743c20b6bc5e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659329"
---
# <a name="building-an-accessible-application"></a>Erstellen einer zugänglichen Anwendung

In diesem Artikel wird gezeigt, wie Sie eine Kartenanwendung erstellen, die von einer Sprachausgabe verwendet werden kann.

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='500' scrolling='no' title='Erstellen einer zugänglichen Anwendung' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen erhalten Sie über den Stift <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Erstellen einer zugänglichen Anwendung</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) unter <a href='https://codepen.io'>CodePen</a>.
</iframe>

Die Karte enthält bereits einige Barrierefreiheitsfeatures. Ein Benutzer kann auf der Karte über die Tastatur navigieren, wenn eine Sprachausgabe ausgeführt wird. Die Karte benachrichtigt den Benutzer über Änderungen an ihrem Zustand. Beispielsweise wird der Benutzer über den neuen Breitengrad, Längengrad, Zoom und Standort auf der Karte informiert, wenn diese geschwenkt oder herangezoomt wird. Zusätzliche Informationen, die auf die allgemeine Karte platziert werden, sollten über entsprechende Textinformationen für Sprachausgabenbenutzer verfügen. Eine Möglichkeit dazu ist die Verwendung von [Popup](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest). Im Suchbeispiel oben wird der Karte ein Popupmenü mit Textinformationen für jede Stecknadel, die auf der Karte platziert wird, hinzugefügt. Mithilfe der Anfügemethode von [Popup](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) kann das Popupmenü von einer Sprachausgabe gelesen werden, ohne dass es auf der Karte visuell dargestellt werden muss.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu der in diesem Artikel verwendeten Popup-Klasse und deren Methoden:

* [Popup](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [attach](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#attach)
    * [remove](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#remove)
    * [open](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [close](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Sehen Sie sich unsere [Codebeispielseite](http://aka.ms/AzureMapsSamples) für weitere Zuordnungsszenarios an.

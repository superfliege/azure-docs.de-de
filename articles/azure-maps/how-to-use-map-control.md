---
title: Verwenden des Azure Maps-Kartensteuerelements | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie die clientseitige JavaScript-Bibliothek des Azure Maps-Kartensteuerelements verwenden.
author: dsk-2015
ms.author: dkshir
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 850f9b28c112c11fd98a8abc81a1811cd26d81cc
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166028"
---
# <a name="use-the-azure-maps-map-control"></a>Verwenden des Azure Maps-Kartensteuerelements

Mit der clientseitigen JavaScript-Bibliothek des Kartensteuerelements können Sie Karten und eingebettete Azure Maps-Funktionen in Ihrer Web- oder Mobilanwendung rendern.

## <a name="create-a-new-map-in-a-web-page"></a>Erstellen einer neuen Karte auf einer Webseite

Sie können eine Karte in eine Webseite einbetten, indem Sie die clientseitige JavaScript-Bibliothek des Kartensteuerelements verwenden.

1. Erstellen Sie eine neue Datei, und geben Sie ihr den Namen **MapSearch.html**.

2. Fügen Sie die Verweise auf das Azure Maps-Stylesheet und die Skriptquelle dem `<head>`-Element der Datei hinzu:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
    ```

3. Fügen Sie zum Rendern einer neuen Karte in Ihrem Browser im `<style>`-Element einen **#map**-Verweis hinzu:

    ```html
    <style>
        #map {
            width: 100%;
            height: 100%;
        }
    </style>
    ```

4. Definieren Sie zum Initialisieren des Kartensteuerelements im HTML-Text einen neuen Abschnitt, und erstellen Sie ein Skript. Verwenden Sie Ihren eigenen Azure Maps-Kontoschlüssel im Skript. Wenn Sie ein Konto erstellen oder Ihren Schlüssel ermitteln müssen, lesen Sie die Informationen unter [Verwalten Ihres Azure Maps-Kontos und der dazugehörigen Schlüssel](how-to-manage-account-keys.md). Die **setLanguage**-Methode gibt die Sprache an, die für Kartenbeschriftungen und Steuerelemente verwendet werden soll. Weitere Informationen zu unterstützten Sprachen finden Sie unter [Unterstützte Sprachen](https://docs.microsoft.com/azure/azure-maps/supported-languages).

    ```html
    <div id="map">
        <script>
            atlas.setSubscriptionKey("<_your account key_>");
            atlas.setLanguage("en");
            var map = new atlas.Map("map", {
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```

5. Öffnen Sie die Datei in Ihrem Webbrowser, und zeigen Sie die gerenderte Karte an.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie eine Karte mit einem vollständigen Beispiel erstellen:

> [!div class="nextstepaction"]
> [Erstellen einer Karte](map-create.md)

Erfahren Sie, wie Sie den Stil einer Karte auswählen:

> [!div class="nextstepaction"]
> [Auswählen eines Kartenstils](choose-map-style.md)

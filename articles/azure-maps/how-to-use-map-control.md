---
title: Verwenden des Azure Maps-Kartensteuerelements | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie die clientseitige JavaScript-Bibliothek des Azure Maps-Kartensteuerelements verwenden.
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 45890b4bd474c010b2b086be0405b79d340aeebd
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603158"
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

4. Definieren Sie zum Initialisieren des Kartensteuerelements im HTML-Text einen neuen Abschnitt, und erstellen Sie ein Skript. Verwenden Sie Ihren eigenen Azure Maps-Kontoschlüssel im Skript. Wenn Sie ein Konto erstellen oder Ihren Schlüssel ermitteln müssen, lesen Sie die Informationen unter [Verwalten Ihres Azure Maps-Kontos und der dazugehörigen Schlüssel](how-to-manage-account-keys.md).

    ```html
    <div id="map">
        <script>
            var MapsAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": MapsAccountKey,
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

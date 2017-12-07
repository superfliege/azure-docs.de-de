---
title: 'Anleitung: Verwenden des Azure Location Based Services-Kartensteuerelements | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die clientseitige JavaScript-Bibliothek des Azure Location Based Services-Kartensteuerelements verwenden.
services: location-based-services
keywords: "Vermeiden Sie es, Schlüsselwörter hinzuzufügen oder zu bearbeiten, ohne Ihren SEO-Experten zurate zu ziehen."
author: philmea
ms.author: philmea
ms.date: 11/22/2017
ms.topic: how-to
ms.service: location-based-services
manager: timlt
ms.openlocfilehash: 9ddd11806accddb41c02c0c6681aac07bba25356
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="how-to-use-the-azure-location-based-services-map-control"></a>Anleitung: Verwenden des Azure Location Based Services-Kartensteuerelements
Mit der clientseitigen JavaScript-Bibliothek des Kartensteuerelements können Sie Karten und eingebettete Azure Location Based Services-Funktionen in Ihrer Web- oder mobilen Anwendung rendern. 

## <a name="prerequisites"></a>Voraussetzungen
Ein Azure Location Based Services-Konto und ein Abonnementschlüssel. Informationen zum Erstellen eines Kontos und Beschaffen eines Abonnementschlüssels finden Sie unter [Verwalten Ihres Azure Location Based Services-Kontos und der dazugehörigen Schlüssel](how-to-manage-account-keys.md). 

## <a name="create-a-new-map-in-a-web-page-using-the-map-control-api"></a>Erstellen einer neuen Karte auf einer Webseite mit der Kartensteuerelement-API
Sie können eine Karte in eine Webseite einbetten, indem Sie die clientseitige JavaScript-Bibliothek des Kartensteuerelements verwenden.

1. Erstellen Sie eine neue Datei, und geben Sie ihr den Namen „MapSearch.html“.

2. Fügen Sie die Verweise auf das Azure Location Based Services-Stylesheet und die Skriptquelle dem `<head>`-Element der Datei hinzu:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Fügen Sie zum Rendern einer neuen Karte in Ihrem Browser im `<style>`-Element einen **#map**-Verweis hinzu.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Definieren Sie zum Initialisieren des Kartensteuerelements im HTML-Text einen neuen Abschnitt, und erstellen Sie ein Skript. Verwenden Sie Ihren eigenen Abonnementschlüssel aus Ihrem Azure Location Based Services-Konto. 

    ```html
    <div id="map">
        <script>
            var subscriptionKey = "<_subscriptionKey_>";
            var map = new atlas.Map("map", {
                "subscription-key": subscriptionKey,
                center: [47.59093,-122.33263],
                zoom: 12
            });
        <script>
    <div>
    ```
    
5. Öffnen Sie die Datei in Ihrem Webbrowser, und zeigen Sie die gerenderte Karte an.
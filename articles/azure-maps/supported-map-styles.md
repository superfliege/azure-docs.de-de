---
title: Unterstützte Kartenstile in Azure Maps | Microsoft-Dokumentation
description: In Azure Maps unterstützte Kartenstile
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 76ab49c7f28260249483bf3bc4387e8cbaca13b2
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57570537"
---
# <a name="azure-maps-supported-map-styles"></a>Unterstützte Kartenstile in Azure Maps
Azure Maps unterstützt mehrere integrierte Kartenstile, die im Folgenden beschrieben werden.

## <a name="road"></a>Straße
Der Kartenstil **Straße** ist eine Standardkarte, die Straßen, natürliche und künstliche Strukturen zusammen mit den Bezeichnungen für diese Strukturen anzeigt.

![Straße](./media/supported-map-styles/road.png)

**Anwendbare APIs:**
* [Kartenbild](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Kartenkachel](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* JS-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="satellite"></a>Satellit 
Der Kartenstil **Satellit** ist eine Kombination aus Satellitenbildern und Luftaufnahmen.

![Satellit](./media/supported-map-styles/satellite.png)

**Anwendbare APIs:**
* [Satellitenkachel](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* JS-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="satelliteroadlabels"></a>Satellite_Road_Labels
Dieser Kartenstil ist eine Hybriddarstellung, bei der Straßen und Bezeichnungen Satellitenbilder und Luftaufnahmen überlagern.

![Satellite_Road_Labels](./media/supported-map-styles/satellite_road_labels.png)

**Anwendbare APIs:**
* JS-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="grayscaledark"></a>grayscale_dark
**grayscale_dark** ist eine dunkle Version des Kartenstils „Straße“.

![Gray_Scale](./media/supported-map-styles/grayscale_dark.png)

**Anwendbare APIs:**
* JS-Kartensteuerelement 
* Android-Kartensteuerelement

## <a name="night"></a>Nacht
**Nacht** ist eine dunkle Version des Kartenstils „Straße“ mit farbigen Straßen und Symbolen.

![Nacht](./media/supported-map-styles/night.png)

**Anwendbare APIs:**
* JS-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="roadshadedrelief"></a>road_shaded_relief
**road shaded relief** (Relief mit schattierten Straßen) ist eine Azure Maps-Hauptformatvorlage mit Konturen der Erde.

![schattiertes Relief](./media/supported-map-styles/shaded-relief.png)

**Anwendbare APIs:**
* [Kartenkachel](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* JS-Kartensteuerelement
* Android-Kartensteuerelement

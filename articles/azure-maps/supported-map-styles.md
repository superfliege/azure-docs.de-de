---
title: Unterstützte Kartenstile in Azure Maps | Microsoft-Dokumentation
description: In Azure Maps unterstützte Kartenstile
author: walsehgal
ms.author: v-musehg
ms.date: 10/02/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2f426ab85d9ab15dd080a487337b8d48358ed040
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888459"
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

## <a name="satellite"></a>Satellit 
Der Kartenstil **Satellit** ist eine Kombination aus Satellitenbildern und Luftaufnahmen.

![Satellit](./media/supported-map-styles/satellite.png)

**Anwendbare APIs:**
* [Satellitenkachel](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* JS-Kartensteuerelement

## <a name="satelliteroadlabels"></a>Satellite_Road_Labels
Dieser Kartenstil ist eine Hybriddarstellung, bei der Straßen und Bezeichnungen Satellitenbilder und Luftaufnahmen überlagern.

![Satellite_Road_Labels](./media/supported-map-styles/satellite_road_labels.png)

**Anwendbare APIs:**
* JS-Kartensteuerelement

## <a name="grayscaledark"></a>grayscale_dark
**grayscale_dark** ist eine dunkle Version des Kartenstils „Straße“.

![Gray_Scale](./media/supported-map-styles/grayscale_dark.png)

**Anwendbare APIs:**
* JS-Kartensteuerelement 

## <a name="night"></a>Nacht
**Nacht** ist eine dunkle Version des Kartenstils „Straße“ mit farbigen Straßen und Symbolen.

![Nacht](./media/supported-map-styles/night.PNG)

**Anwendbare APIs:**
* JS-Kartensteuerelement 
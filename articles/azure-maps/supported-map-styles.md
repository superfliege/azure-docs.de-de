---
title: Unterstützte Kartenstile in Azure Maps | Microsoft-Dokumentation
description: In Azure Maps unterstützte Kartenstile
author: walsehgal
ms.author: v-musehg
ms.date: 08/28/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 8f0910e9040c962bae30a33b91a93e71e692dfdb
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713441"
---
# <a name="azure-maps-supported-map-styles"></a>Unterstützte Kartenstile in Azure Maps
Azure Maps stellt vier verschiedene integrierte Kartenstile bereit. Die Stile und ihre Beschreibungen werden nachfolgend aufgeführt.

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

## <a name="grayscaledark"></a>Grayscale_Dark
**Grayscale_Dark** ist eine dunkle Version des Kartenstils „Straße“.

![Gray_Scale](./media/supported-map-styles/grayscale_dark.png)

**Anwendbare APIs:**
* JS-Kartensteuerelement 
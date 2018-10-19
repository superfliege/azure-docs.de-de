---
title: Generieren von Miniaturbildern – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zum Generieren von Miniaturbildern für Bilder mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: ff16fa4d30fb60e6b1376027a40242af23be04d1
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985345"
---
# <a name="generating-thumbnails"></a>Generieren von Miniaturbildern

Ein Miniaturbild ist eine kleine Darstellung eines Bilds. Verschiedene Geräte wie Smartphones, Tablets und PCs machen verschiedene Layouts der Benutzeroberfläche sowie unterschiedliche Miniaturbildgrößen erforderlich. Durch intelligente Zuschneidefunktionen bietet diese Maschinelles Sehen-Feature Hilfe bei der Lösung dieses Problems.

Nach dem Hochladen eines Bilds generiert das maschinelle Sehen ein hochwertiges Miniaturbild und analysiert dann die Objekte im Bild, um den *relevanten Bereich* (Region Of Interest, ROI) zu ermitteln. Optional kann das Bild an die Anforderungen des relevanten Bereichs angepasst werden. Das generierte Miniaturbild kann mit einem anderen Seitenverhältnis als dem des ursprünglichen Bilds angezeigt werden, um so Ihren Anforderungen zu entsprechen.

Der Miniaturbildalgorithmus funktioniert wie folgt:

1. Störende Elemente werden aus dem Bild entfernt, und das Hauptobjekt (der Interessensbereich) wird ermittelt.
2. Das Bild wird auf Grundlage des erkannten Interessensbereichs zugeschnitten.
3. Das Seitenverhältnis wird entsprechend den Abmessungen des Zielminiaturbilds geändert.

Das generierte Miniaturbild kann je nach den Angaben für Höhe, Breite und intelligenter Zuschneidefunktion stark variieren, wie im folgenden Bild dargestellt.

![Miniaturbilder](./Images/thumbnail-demo.png)

## <a name="thumbnail-generation-examples"></a>Beispiele zum Generieren von Miniaturbildern

In der folgenden Tabelle sind typische Miniaturbilder dargestellt, die vom maschinellen Sehen für die Beispielbilder generiert wurden. Die Miniaturbilder wurden für eine bestimmte Zielhöhe und -breite von 50 Pixel generiert, wobei die intelligente Zuschneidefunktion aktiviert ist.

| Image | Miniaturansicht |
|-------|-----------|
|![Outdoor Mountain](./Images/mountain_vista.png) | ![Miniaturbild: Outdoor Mountain](./Images/mountain_vista_thumbnail.png) |
|![Bildanalyse: Flower](./Images/flower.png) | ![Bildanalyse: Miniaturbild Flower](./Images/flower_thumbnail.png) |
|![Woman Roof](./Images/woman_roof.png) | ![Miniaturbild: Woman Roof](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Konzepte zum [Taggen von Bildern](concept-tagging-images.md) und [Kategorisieren von Bildern](concept-categorizing-images.md).
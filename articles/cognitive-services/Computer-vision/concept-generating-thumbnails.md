---
title: Generieren von Miniaturbildern – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zum Generieren von Miniaturbildern für Bilder mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pafarley
ms.openlocfilehash: 7d914f394ecfcf02ed26f41cd8fe2ef799cf6103
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966737"
---
# <a name="generating-thumbnails"></a>Generieren von Miniaturbildern

Ein Miniaturbild ist eine verkleinerte Darstellung eines Bilds. Miniaturbilder werden verwendet, um Bilder und andere Daten auf wirtschaftliche und layoutfreundliche Weise darzustellen. Die Maschinelles Sehen-API verwendet intelligente Funktionen zum Zuschneiden und Ändern der Größe von Bildern, um intuitive Miniaturbilder für ein bestimmtes Bild zu erstellen.

Der Algorithmus zum Generieren der Miniaturbilder der Maschinelles Sehen-API funktioniert wie folgt:
1. Entfernen störender Elemente aus dem Bild und Identifizieren des _relevanten Bereichs_ &mdash; das ist der Bildbereich, in dem die wichtigsten Objekte enthalten sind.
1. Das Bild wird auf Grundlage des erkannten _relevanten Bereichs_ zugeschnitten.
1. Das Seitenverhältnis wird entsprechend den Abmessungen des Zielminiaturbilds geändert.

## <a name="area-of-interest"></a>Relevanter Bereich

Wenn Sie ein Bild hochladen, analysiert die Maschinelles Sehen-API dieses, um den *relevanten Bereich* zu ermitteln. Anhand dieser Region kann dann bestimmt werden, wie das Bild zugeschnitten werden muss. Das Zuschneiden wird jedoch immer gemäß dem gewünschten Seitenverhältnis durchgeführt, sofern dieses angegeben wurde.

Sie können die Koordinaten des unbearbeiteten umgebenden Felds dieses *relevanten Bereichs* stattdessen auch durch Aufrufen der **areaOfInterest**-API abrufen. Mithilfe dieser Informationen können Sie dann das ursprüngliche Bild ändern, wie Sie möchten.

## <a name="examples"></a>Beispiele

Das generierte Miniaturbild kann je nach den Angaben für Höhe, Breite und intelligenter Zuschneidefunktion stark variieren, wie im folgenden Bild dargestellt.

![Miniaturbilder](./Images/thumbnail-demo.png)

In der folgenden Tabelle sind typische Miniaturbilder dargestellt, die vom maschinellen Sehen für die Beispielbilder generiert wurden. Die Miniaturbilder wurden für eine bestimmte Zielhöhe und -breite von 50 Pixel generiert, wobei die intelligente Zuschneidefunktion aktiviert ist.

| Image | Miniaturansicht |
|-------|-----------|
|![Outdoor Mountain](./Images/mountain_vista.png) | ![Miniaturbild: Outdoor Mountain](./Images/mountain_vista_thumbnail.png) |
|![Bildanalyse: Flower](./Images/flower.png) | ![Bildanalyse: Miniaturbild Flower](./Images/flower_thumbnail.png) |
|![Woman Roof](./Images/woman_roof.png) | ![Miniaturbild: Woman Roof](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Taggen von Bildern](concept-tagging-images.md) und das [Kategorisieren von Bildern](concept-categorizing-images.md).
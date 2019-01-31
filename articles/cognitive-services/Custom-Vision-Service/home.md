---
title: Was ist Azure Custom Vision?
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mithilfe des Custom Vision-Diensts benutzerdefinierte Bildklassifizierungen in der Azure-Cloud erstellen.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 01/10/2019
ms.author: anroth
ms.openlocfilehash: 887c2fa09a1108b6254f4dd13b1ca211c80f8f70
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219259"
---
# <a name="what-is-azure-custom-vision"></a>Was ist Azure Custom Vision?

Die Azure Custom Vision-API gehört zu Cognitive Services und ermöglicht das Erstellen, Bereitstellen und Optimieren benutzerdefinierter Bildklassifizierungen. Bei einer Bildklassifizierung handelt es sich um einen KI-Dienst, der Bilder anhand bestimmter Eigenschaften nach Klassen (Tags) sortiert. Im Gegensatz zum [Dienst für maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) können Sie mit Custom Vision Ihre eigenen Klassifizierungen erstellen.

## <a name="what-it-does"></a>Funktionsbeschreibung

Der Custom Vision-Dienst verwendet einen Machine Learning-Algorithmus, um Bilder zu klassifizieren. Entwickler müssen Gruppen von Bildern übermitteln, auf denen die betreffenden Klassifizierungen vorhanden bzw. nicht vorhanden sind. Bei der Übermittlung geben sie jeweils die korrekten Tags für die Bilder an. Diese Daten werden dann von dem Algorithmus als Trainingsdaten herangezogen, und er berechnet seine eigene Genauigkeit mithilfe von Tests, die auf diesen Daten basieren. Das trainierte Modell kann getestet, neu trainiert und schließlich gemäß den Anforderungen Ihrer App zur Klassifizierung neuer Bilder verwendet werden. Darüber hinaus kann das Modell exportiert und offline verwendet werden.

### <a name="classification-and-object-detection"></a>Klassifizierung und Objekterkennung

Custom Vision bietet zwei Features: Die **Bildklassifizierung** weist jedem Bild eine Klassifizierungsverteilung zu. Sowohl das Modell für die Multiklassenklassifizierung (ein Tag pro Bild) als auch das Modell für die Klassifizierung mit mehreren Bezeichnungen (beliebige Anzahl von Tags pro Bild) wird unterstützt. Die **Objekterkennung** ähnelt der Klassifizierung mit mehreren Bezeichnungen, gibt aber auch die Koordinaten zurück, an denen sich die angewendeten Bezeichnungen im Bild befinden.

### <a name="optimization"></a>Optimierung

Allgemein gilt: Die vom Custom Vision-Dienst verwendeten Methoden sind Unterschieden gegenüber unempfindlich, sodass eine Prototyperstellung bereits mit wenigen Daten möglich ist. 50 Bilder pro Tag sind im Allgemeinen ein guter Ausgangspunkt. Das bedeutet jedoch auch, dass der Dienst geringfügige Bildunterschiede (etwa kleinere Risse oder Dellen in Qualitätssicherungsszenarien) nicht optimal erkennen kann.

Darüber hinaus stehen verschiedene Varianten des Custom Vision-Algorithmus zur Verfügung, die jeweils für bestimmtes Material optimiert sind – etwa für Wahrzeichen oder Einzelhandelsartikel. Weitere Informationen finden Sie unter [Erstellen einer Klassifizierung](getting-started-build-a-classifier.md).

## <a name="what-it-includes"></a>Lieferumfang
Der Custom Vision-Dienst ist als Gruppe nativer SDKs sowie über eine webbasierte Oberfläche auf der [Custom Vision-Startseite](https://customvision.ai/) verfügbar. Beide Schnittstellen können zum Erstellen, Testen und Trainieren eines Modells verwendet werden.

![Custom Vision-Startseite in einem Chrome-Browserfenster](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Custom Vision-Dienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Unter [Erstellen einer Klassifizierung](getting-started-build-a-classifier.md) werden die ersten Schritte mit Custom Vision im Web beschrieben. Sie können aber auch ein [Bildklassifizierungstutorial](csharp-tutorial.md) absolvieren, um das Szenario mit Code zu implementieren.

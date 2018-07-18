---
title: Verbessern Ihrer Klassifizierung mit dem Custom Vision Service – Azure Cognitive Services | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie die Qualität Ihrer Custom Vision Service-Klassifizierung verbessern.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 65b1424f259066c7d5bd6b2b508d2a4052ff0527
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374723"
---
# <a name="how-to-improve-your-classifier"></a>Verbessern der Klassifizierung

Hier wird beschrieben, wie Sie die Qualität Ihrer Custom Vision Service-Klassifizierung verbessern. Die Qualität Ihrer Klassifizierung ist abhängig von der Qualität der von Ihnen bereitgestellten gekennzeichneten Daten. 

## <a name="train-more-varied-images"></a>Trainieren mit unterschiedlichen Bildern

Sie können die Klassifizierung verbessern, indem Sie mit Tags versehene Bilder mit verschiedenen Winkeln, Hintergründen, Objektgrößen, Gruppen von Fotos und anderen Variationen bereitstellen. Fotos im Kontext sind besser als Fotos vor neutralen Hintergründen geeignet. Binden Sie Bilder ein, die für die Daten repräsentativ sind, die während der normalen Nutzung an die Klassifizierung übermittelt werden.

Weitere Informationen zum Hinzufügen von Bildern finden Sie im Dokument [Erstellen einer Klassifizierung](getting-started-build-a-classifier.md).

> [!IMPORTANT]
> Denken Sie daran, die Klassifizierung zu trainieren, nachdem Sie Bilder hinzugefügt haben.

## <a name="use-images-submitted-for-prediction"></a>Verwenden von für die Vorhersage übermittelten Bildern

Der Custom Vision Service speichert Bilder, die an den Vorhersageendpunkt übermittelt werden. Führen Sie die folgenden Schritte aus, um diese Bilder zum Verbessern der Klassifizierung zu verwenden:

1. Um die an die Klassifizierung gesendeten Bilder anzuzeigen, öffnen Sie die [Custom Vision-Webseite](https://customvision.ai) und wählen die Registerkarte __Predictions__ (Vorhersagen) aus.

    ![Abbildung der Registerkarte „Predictions“ (Vorhersagen)](./media/getting-started-improving-your-classifier/predictions-tab.png)

    > [!TIP]
    > In der Standardansicht werden Bilder aus der aktuellen Iteration angezeigt. Sie können das Dropdownfeld __Iteration__ verwenden, um Bilder anzuzeigen, die während früherer Iterationen gesendet wurden.

2. Zeigen Sie mit dem Mauszeiger auf ein Bild, um die Kategorien anzuzeigen, die von der Klassifizierung vorhergesagt wurden.

    > [!TIP]
    > Die Bilder sind so nach einer Rangfolge geordnet, dass die Bilder, die für die Klassifizierung die höchste Bewertung erzielen, oben angezeigt werden. Wenn Sie eine andere Sortierung auswählen möchten, können Sie den Abschnitt __Sort__ (Sortieren) verwenden.

    Um Ihren Trainingsdaten ein Bild hinzuzufügen, wählen Sie das Bild, die Kategorie und dann die Option __Save and close__ (Speichern und schließen) aus. Das Bild wird aus dem Bereich __Predictions__ (Vorhersagen) entfernt und den Trainingsbildern hinzugefügt. Sie können es anzeigen, indem Sie die Registerkarte __Training Images__ (Trainingsbilder) auswählen.

    ![Bild der Kategorienseite](./media/getting-started-improving-your-classifier/tag-image.png)

3. Verwenden Sie die Schaltfläche __Train__ (Trainieren), um die Klassifizierung erneut zu trainieren.

## <a name="visually-inspect-predictions"></a>Visuelle Überprüfung von Vorhersagen

Wählen Sie die Registerkarte __Training Images__ (Trainingsbilder) und dann __Iteration History__ (Iterationsverlauf), um die Bildvorhersagen zu untersuchen. Bilder, die durch einen roten Rahmen gekennzeichnet sind, wurden richtig vorhergesagt.

![Abbildung des Iterationsverlaufs](./media/getting-started-improving-your-classifier/iteration-history.png)

Bei der visuellen Untersuchung werden ggf. Muster identifiziert, die Sie dann korrigieren können, indem Sie zusätzliche Trainingsdaten hinzufügen. Bei einer Klassifizierung für Rosen und Gänseblümchen kann es beispielsweise vorkommen, dass alle weißen Rosen fälschlicherweise als Gänseblümchen gekennzeichnet werden. Sie können dieses Problem unter Umständen korrigieren, indem Sie Trainingsdaten hinzufügen und bereitstellen, die mit Markierungen versehene Bilder von weißen Rosen enthalten.

## <a name="unexpected-classification"></a>Unerwartete Klassifizierung

In einigen Fällen werden bei der Klassifizierung Merkmale erlernt, die Ihre Bilder als Gemeinsamkeiten aufweisen. Angenommen, Sie möchten eine Klassifizierung für Rosen und Tulpen erstellen. Sie stellen Bilder von Tulpen auf Feldern und von Rosen in einer roten Vase vor einer blauen Wand bereit. Mit diesen Daten wird die Klassifizierung unter Umständen auf die Unterscheidung von Feld und Wand + Vase trainiert, anstatt auf Rosen und Tulpen.

Verwenden Sie zum Beheben dieses Problems die Anleitung zum Trainieren mit Bildern, die mehr Unterschiede aufweisen: Stellen Sie Bilder mit verschiedenen Winkeln, Hintergründen, Objektgrößen und anderen Variationen bereit.

## <a name="negative-image-handling"></a>Verwendung von negativen Bildern

Der Custom Vision Service unterstützt teilweise die automatische Verarbeitung von negativen Bildern. Wenn Sie eine Klassifizierung für die Unterscheidung von Katzen und Hunden erstellen und ein Bild eines Schuhs für die Vorhersage übermitteln, sollte die Klassifizierung dieses Bild in Bezug auf Katzen und Hunde mit nahezu 0% bewerten. 

> [!WARNING]
> Der automatische Ansatz funktioniert für Bilder, bei denen es sich eindeutig um negative Bilder handelt. Er ist nicht gut für Fälle geeignet, in denen die negativen Bilder lediglich eine Variation der für das Training genutzten Bilder darstellen. 
>
> Wenn Sie beispielsweise eine Klassifizierung zur Unterscheidung von Huskys und Corgis verwenden und ein Bild mit einem Zwergspitz übermitteln, wird dieser Hund ggf. als Husky eingestuft. Falls Sie über negative Bilder dieser Art verfügen, sollten Sie eine neue Kategorie (z.B. „Andere“) erstellen und auf die negativen Trainingsbilder anwenden.

## <a name="next-steps"></a>Nächste Schritte

[Verwenden der Vorhersage-API](use-prediction-api.md)
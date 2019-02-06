---
title: Verbessern der Klassifizierung – Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Hier wird beschrieben, wie Sie die Qualität Ihrer Klassifizierung verbessern.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: pafarley
ms.openlocfilehash: 72ba363201b27a8ca31c73af1d0cceb436de468d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209391"
---
# <a name="how-to-improve-your-classifier"></a>Verbessern der Klassifizierung

Hier wird beschrieben, wie Sie die Qualität Ihrer Custom Vision Service-Klassifizierung verbessern. Die Qualität Ihrer Klassifizierung ist abhängig von der Menge, Qualität und Vielseitigkeit der von Ihnen bereitgestellten gekennzeichneten Daten sowie von der Ausgewogenheit des Datasets. Eine gute Klassifizierung verfügt in der Regel über ein ausgewogenes Trainingsdataset, das repräsentativ für die Elemente ist, die an die Klassifizierung übermittelt werden. Der Prozess zur Erstellung einer solchen Klassifizierung ist häufig iterativ. Es ist üblich, dass einige Trainingsrunden erforderlich sind, um die erwarteten Ergebnisse zu erzielen.

Hier finden Sie einige allgemeine Schritte, mit denen sich eine Klassifizierung verbessern lässt. Diese Schritte sind keine in Stein gemeißelten Regeln, sondern heuristische Methoden, die Sie beim Erstellen einer besseren Klassifizierung unterstützen.

1. Erste Trainingsrunde
1. Hinzufügen weiterer Bilder und ausgewogene Daten
1. Erneutes Trainieren
1. Hinzufügen von Bildern mit Unterschieden hinsichtlich Hintergrund, Beleuchtung, Objektgröße, Kamerawinkel und Ausführung
1. Erneutes Trainieren und Einfügen in Bild für Vorhersagen
1. Untersuchen der Vorhersageergebnisse
1. Ändern der vorhandenen Trainingsdaten

## <a name="data-quantity-and-data-balance"></a>Menge und Ausgewogenheit der Daten

Der wichtigste Aspekt ist, genügend Bilder zum Trainieren der Klassifizierung hochzuladen. Für den Anfang werden mindestens 50 Bilder pro Bezeichnung für den Trainingssatz empfohlen. Bei einer geringeren Anzahl von Bildern besteht die große Gefahr der Überanpassung. Auch wenn Ihre Leistungsdaten eine gute Qualität vermuten lassen, haben Sie möglicherweise mit Daten aus der realen Welt zu kämpfen. Wenn Sie die Klassifizierung mit einer höheren Anzahl von Bildern trainieren, steigt im Allgemeinen die Genauigkeit der Vorhersageergebnisse.

Darüber hinaus sollten Sie sicherstellen, dass Ihre Daten ausgewogen sind. Wenn Sie beispielsweise 500 Bilder für eine Bezeichnung, aber nur 50 Bilder für eine andere Bezeichnung verwenden, erhalten Sie ein unausgewogenes Trainingsdataset, was dazu führt, dass das Modell bei der Vorhersage für eine Bezeichnung genauer ist als bei der Vorhersage für die andere Bezeichnung. Die besten Ergebnisse werden Sie wahrscheinlich erzielen, wenn zwischen der Bezeichnung mit den wenigsten Bildern und der Bezeichnung mit den meisten Bildern ein Verhältnis von 1:2 besteht. Wenn z.B. für die Bezeichnung mit der höchsten Bildanzahl 500 Bilder vorhanden sind, sollte die Bezeichnung mit der geringsten Bildanzahl mindestens 250 Bilder für das Training enthalten.

## <a name="train-more-diverse-images"></a>Trainieren vielfältigerer Bilder

Stellen Sie Bilder bereit, die für die Elemente repräsentativ sind, die während der normalen Nutzung an die Klassifizierung übermittelt werden. Wenn Sie z.B. eine Klassifizierung für „Apfel“ nur mit Bildern von Äpfeln auf Tellern trainieren, ist Ihre Klassifizierung weniger genau, wenn Sie Vorhersagen zu Fotos von Äpfeln an Bäumen durchführen möchten. Indem Sie unterschiedliche Arten von Bildern eingeben, stellen Sie sicher, dass Ihre Klassifizierung nicht einseitig verfälscht wird und gut generalisieren kann. Im Folgenden finden Sie einige Möglichkeiten, um Ihren Trainingssatz vielseitiger zu machen:

__Hintergrund:__ Stellen Sie Bilder Ihres Objekts vor verschiedenen Hintergründen bereit (also Obst auf Tellern im Gegensatz zu Obst in einer Lebensmitteltüte). Fotos im Kontext eignen sich besser als Fotos vor neutralen Hintergründen, da erstere weitere Informationen für die Klassifizierung bereitstellen.

![Abbildung: Beispiele für Hintergründe](./media/getting-started-improving-your-classifier/background.png)

__Beleuchtung:__ Stellen Sie Bilder mit unterschiedlicher Beleuchtung bereit (also mit Blitz, hohem Belichtungswert usw.), insbesondere dann, wenn die für die Vorhersage verwendeten Bilder unterschiedlich ausgeleuchtet sind. Es ist auch hilfreich, Bilder mit unterschiedlichen Werten hinsichtlich Sättigung, Farbton und Helligkeit bereitzustellen.

![Abbildung: Beispiele für Beleuchtung](./media/getting-started-improving-your-classifier/lighting.png)

__Objektgröße__: Stellen Sie Bilder bereit, in denen die Objekte eine unterschiedliche Größe aufweisen und in denen verschiedene Teile der Objekte erfasst sind. Beispiel: Ein Foto von einem Bund Bananen und eine Nahaufnahme einer einzelnen Banane. Durch unterschiedliche Objektgrößen kann die Klassifizierung besser generalisieren.

![Abbildung: Beispiele für verschiedene Größen](./media/getting-started-improving-your-classifier/size.png)

__Kamerawinkel__: Stellen Sie Bilder bereit, die aus verschiedenen Kamerawinkeln aufgenommen wurden. Wenn all Ihre Fotos mit fest montierten Kameras (beispielsweise Überwachungskameras) aufgenommen werden, stellen Sie zur Vermeidung einer Überanpassung sicher, dass Sie jeder Kamera eine andere Bezeichnung zuweisen, auch wenn sie die gleichen Objekte aufnehmen. Dabei müssen unabhängige Objekte (wie z.B. Laternenpfähle) als Schlüsselmerkmal modelliert werden.

![Abbildung: Beispiele für verschiedene Winkel](./media/getting-started-improving-your-classifier/angle.png)

__Ausführung__: Stellen Sie Bilder mit verschiedenen Ausführungen der gleichen Klasse bereit (z.B. unterschiedliche Arten von Zitrusfrüchten). Wenn Sie jedoch über Objekte mit erheblich unterschiedlichen Ausführungen verfügen (z.B. Micky Maus und eine echte Ratte), empfiehlt es sich, diese als separate Klassen zu bezeichnen, um die unterschiedlichen Merkmale besser zu repräsentieren.

![Abbildung: Beispiele für Ausführungen](./media/getting-started-improving-your-classifier/style.png)

## <a name="use-images-submitted-for-prediction"></a>Verwenden von für die Vorhersage übermittelten Bildern

Der Custom Vision Service speichert Bilder, die an den Vorhersageendpunkt übermittelt werden. Führen Sie die folgenden Schritte aus, um diese Bilder zum Verbessern der Klassifizierung zu verwenden:

1. Um die an die Klassifizierung gesendeten Bilder anzuzeigen, öffnen Sie die [Custom Vision-Webseite](https://customvision.ai) und wählen die Registerkarte __Vorhersagen__ aus. In der Standardansicht werden Bilder aus der aktuellen Iteration angezeigt. Sie können das Dropdownfeld __Iteration__ verwenden, um Bilder anzuzeigen, die während früherer Iterationen gesendet wurden.

    ![Abbildung der Registerkarte „Predictions“ (Vorhersagen)](./media/getting-started-improving-your-classifier/predictions.png)

2. Zeigen Sie mit dem Mauszeiger auf ein Bild, um die Kategorien anzuzeigen, die von der Klassifizierung vorhergesagt wurden. Die Bilder sind so angeordnet, dass die Bilder, die in der Klassifizierung die höchste Bewertung erzielen, oben angezeigt werden. Wenn Sie eine andere Sortierung auswählen möchten, können Sie den Abschnitt __Sort__ (Sortieren) verwenden. Um Ihren vorhandenen Trainingsdaten ein Bild hinzuzufügen, wählen Sie das Bild und das richtige Tag aus, und klicken Sie auf __Save and close__ (Speichern und schließen). Das Bild wird aus dem Bereich __Predictions__ (Vorhersagen) entfernt und den Trainingsbildern hinzugefügt. Sie können es anzeigen, indem Sie die Registerkarte __Training Images__ (Trainingsbilder) auswählen.

    ![Bild der Kategorienseite](./media/getting-started-improving-your-classifier/tag.png)

3. Verwenden Sie die Schaltfläche __Train__ (Trainieren), um die Klassifizierung erneut zu trainieren.

## <a name="visually-inspect-predictions"></a>Visuelle Überprüfung von Vorhersagen

Wählen Sie die Registerkarte __Training Images__ (Trainingsbilder) und dann __Iteration History__ (Iterationsverlauf), um die Bildvorhersagen zu untersuchen. Bilder, die durch einen roten Rahmen gekennzeichnet sind, wurden richtig vorhergesagt.

![Abbildung des Iterationsverlaufs](./media/getting-started-improving-your-classifier/iteration.png)

Bei der visuellen Untersuchung werden ggf. Muster identifiziert, die Sie dann korrigieren können, indem Sie zusätzliche Trainingsdaten hinzufügen oder vorhandene Trainingsdaten ändern. Ein Beispiel: Eine Klassifizierung zur Unterscheidung von Äpfeln und Limetten bezeichnet fälschlicherweise alle grünen Äpfel als Limetten. Sie können dieses Problem unter Umständen korrigieren, indem Sie Trainingsdaten hinzufügen und bereitstellen, die mit Tags versehene Bilder von grünen Äpfeln enthalten.

## <a name="unexpected-classification"></a>Unerwartete Klassifizierung

In einigen Fällen lernt die Klassifizierung fälschlicherweise Merkmale, die Ihren Bilder gemeinsam sind. Wenn Sie z.B. eine Klassifizierung zur Unterscheidung von Äpfeln und Zitrusfrüchten erstellen und dafür Bilder von Äpfeln in Händen und Zitrusfrüchten auf weißen Tellern bereitstellen, wird die Klassifizierung möglicherweise auf den Unterschied zwischen Händen und weißen Tellern trainiert, anstatt auf den Unterschied zwischen Äpfeln und Zitrusfrüchten.

![Abbildung: unerwartete Klassifizierung](./media/getting-started-improving-your-classifier/unexpected.png)

Um dieses Problem zu beheben, befolgen Sie die oben stehende Anleitung zum Trainieren mit Bildern, die mehr Unterschiede aufweisen: Stellen Sie Bilder mit verschiedenen Winkeln, Hintergründen, Objektgrößen und anderen Variationen bereit.

## <a name="negative-image-handling"></a>Verwendung von negativen Bildern

Der Custom Vision Service unterstützt teilweise die automatische Verarbeitung von negativen Bildern. Wenn Sie eine Klassifizierung zur Unterscheidung von Trauben und Bananen erstellen und ein Bild eines Schuhs für die Vorhersage übermitteln, sollte die Klassifizierung dieses Bild in Bezug auf Trauben und Bananen mit nahezu 0% bewerten.

Andererseits ist es in Fällen, in denen die negativen Bilder nur eine Variation der im Training verwendeten Bilder sind, wahrscheinlich, dass das Modell die negativen Bilder aufgrund der großen Ähnlichkeiten als eine bezeichnete Klasse klassifiziert. Wenn Sie z.B. über eine Klassifizierung zur Unterscheidung von Orangen und Grapefruits verfügen und das Bild einer Clementine einfügen, bewertet die Klassifizierung die Clementine möglicherweise als Orange. Dies liegt daran, dass viele Merkmale einer Clementine (Farbe, Form, Textur, Vorkommen in der Natur usw.) denen einer Orange entsprechen.  Wenn Ihre negativen Bilder dieser Art sind, empfiehlt es sich, mindestens ein separates Tag („Sonstiges“) zu erstellen und negative Bilder während des Trainings mit diesem Tag zu bezeichnen, damit das Modell besser zwischen diesen Klassen unterscheiden kann.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie heraus, wie Sie Bilder programmgesteuert testen, indem Sie sie an die Vorhersage-API senden.

> [!div class="nextstepaction"]
[Verwenden der Vorhersage-API](use-prediction-api.md)

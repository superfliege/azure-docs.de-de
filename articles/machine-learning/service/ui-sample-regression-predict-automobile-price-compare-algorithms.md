---
title: 'Regression: Vorhersagen des Preises und Vergleichen von Algorithmen'
titleSuffix: Azure Machine Learning service
description: Dieses Beispielexperiment für eine grafische Benutzeroberfläche zeigt, wie die Leistung von zwei Regressionsmodellen verglichen werden kann, die den Preis eines Autos vorhersagen. Der Prozess umfasst das Trainieren, Testen und Auswerten des Modells anhand des Datasets für Automobilpreisdaten (Rohdaten).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: 9748e9706e1140f200458ba77f8b2fc424be3b14
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027752"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>Beispiel 2 – Regression: Vorhersagen des Preises und Vergleichen von Algorithmen

Dieses Beispielexperiment für eine grafische Benutzeroberfläche zeigt, wie die Leistung von zwei Regressionsmodellen verglichen werden kann, die den Preis eines Autos vorhersagen. Der Prozess umfasst das Trainieren, Testen und Auswerten des Modells anhand des Datasets für **Automobilpreisdaten (Rohdaten)**.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Wählen Sie die Schaltfläche **Öffnen** für das Experiment „Beispiel 2“ aus:

    ![Öffnen des Experiments](media/ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="related-sample"></a>Ähnliches Beispiel

[Beispiel 1 – Regression: Automobile Price Prediction (Basic)](ui-sample-regression-predict-automobile-price-basic.md) (Automobilpreisvorhersage (einfach)) bietet ein einfacheres Experiment, das das gleiche Problem wie dieses Experiment löst, aber nur ein Regressionsmodell verwendet. Verwenden Sie es, wenn Sie nach einem einfachen Beispiel für Regression suchen.

## <a name="experiment-summary"></a>Experimentzusammenfassung

Wir verwenden die folgenden Schritte, um das Experiment zu erstellen:

1. Abrufen der Daten.
1. Vorverarbeiten der Daten.
1. Trainieren des Modells.
1. Testen, Auswerten und Vergleichen der Modelle.

So sieht das vollständige Diagramm des Experiments aus:

[ ![Diagramm des Experiments](media/ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png#lightbox)


## <a name="get-the-data"></a>Abrufen von Daten

In diesem Experiment verwenden wir das Dataset **Automobile Price Data (Raw)** (Automobilpreisdaten (Rohdaten)) aus dem UCI Machine Learning-Repository. Dieses Dataset enthält 26 Spalten mit Informationen zu Automobilen, darunter Marke, Modell, Preis, Fahrzeugmerkmale (etwa die Anzahl der Zylinder), Treibstoffverbrauch und eine Versicherungsrisikobewertung. Das Ziel dieses Experiments besteht darin, den Preis eines Autos vorherzusagen.

## <a name="pre-process-the-data"></a>Vorverarbeiten der Daten

Zu den Hauptaufgaben der Datenaufbereitung gehören die Datenbereinigung, Integration, Transformation, Reduktion und Diskretisierung oder Quantisierung. In der grafischen Benutzeroberfläche finden Sie Module zur Durchführung dieser Vorgänge und anderer Aufgaben der Datenvorverarbeitung in der Gruppe **Data Transformation** (Datentransformation) im linken Bereich.

In diesem Experiment verwenden wir das Modul **Select Columns in Dataset** (Spalten in Dataset auswählen), um normalisierte Verluste auszuschließen, die viele fehlende Werte aufweisen. Anschließend verwenden wir **Clean Missing Data** (Fehlende Daten bereinigen), um die Zeilen zu entfernen, die fehlende Werte aufweisen. Dies hilft, einen bereinigten Satz von Trainingsdaten zu erstellen.

![Datenvorverarbeitung](media/ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Modelltraining

Machine Learning-Probleme sind vielfältig. Machine Learning-Aufgaben sind z.B. Klassifizierung, Clustering, Regression und Empfehlungssysteme, die möglicherweise jeweils einen anderen Algorithmus erfordern. Die Auswahl des Algorithmus hängt häufig von den Anforderungen des Anwendungsfalls ab. Nachdem Sie einen Algorithmus ausgewählt haben, müssen Sie seine Parameter optimieren, um ein genaueres Modell zu trainieren. Sie müssen alle Modelle basierend auf Metriken wie Genauigkeit, Verständlichkeit und Effizienz auswerten.

Da das Ziel dieses Experiments darin besteht, Automobilpreise vorherzusagen, und die Bezeichnungsspalte (Preis) reelle Zahlen enthält, ist ein Regressionsmodell eine gute Wahl. In Anbetracht der Tatsache, dass die Anzahl der Features relativ klein ist (weniger als 100) und diese Features nicht spärlich sind, ist es wahrscheinlich, dass die Entscheidungsgrenze nichtlinear ist.

Um die Leistung verschiedener Algorithmen zu vergleichen, verwenden wir zwei nichtlineare Algorithmen (**Boosted Decision Tree Regression** und **Decision Forest Regression**), um Modelle zu erstellen. Beide Algorithmen weisen Parameter auf, die Sie ändern können, aber wir verwenden die Standardwerte für dieses Experiment.

Wir verwenden das Modul **Split Data** (Daten teilen), um nach dem Zufallsprinzip die eingegebenen Daten so aufzuteilen, dass das Trainingsdataset 70 % der ursprünglichen Daten enthält und das Testdataset 30 % der ursprünglichen Daten.

## <a name="test-evaluate-and-compare-the-models"></a>Testen, Auswerten und Vergleichen der Modelle

Wir verwenden zwei verschiedene Sätze von zufällig ausgewählte Daten zum Trainieren und anschließenden Testen des Modells, wie im vorherigen Abschnitt beschrieben. Wir teilen das Dataset auf und verwenden unterschiedliche Datasets zum Trainieren und Testen das Modell, um die Auswertung des Modells objektiver zu gestalten.

Nachdem das Modell trainiert wurde, verwenden wir die Module **Score Model** (Modell bewerten) und **Evaluate Model** (Modell auswerten), um die vorhergesagten Ergebnisse zu generieren und die Modelle auszuwerten. **Score Model** generiert Vorhersagen für das Testdataset mithilfe des trainierten Modells. Wir übergeben die Bewertungen dann an **Evaluate Model**, um Auswertungsmetriken zu generieren.

In diesem Experiment verwenden wir zwei Instanzen von **Evaluate Model**, um zwei Paare von Modellen zu vergleichen.

Zunächst vergleichen wir zwei Algorithmen für das Trainingsdataset.
Dann vergleichen wir zwei Algorithmen für das Testdataset.
Hier sehen Sie die Ergebnisse:

![Vergleichen der Ergebnisse](media/ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Diese Ergebnisse zeigen, dass das Modell, das mit **Boosted Decision Tree Regression** erstellt wurde, einen geringeren mittleren quadratischen Fehler als das Modell aufweist, das mit **Decision Forest Regression** erstellt wurde.

Beide Algorithmen weisen einen geringeren Fehlerwert für das Trainingsdataset als für das unsichtbare Testdataset auf.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Untersuchen Sie die anderen Beispiele, die für die grafische Benutzeroberfläche zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](ui-sample-regression-predict-automobile-price-basic.md)
- [Beispiel 3 – Klassifizierung: Vorhersagen des Kreditrisikos](ui-sample-classification-predict-credit-risk-basic.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Benutzerabwanderung](ui-sample-classification-predict-churn.md)

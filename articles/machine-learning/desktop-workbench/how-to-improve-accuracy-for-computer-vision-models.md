---
title: Höhere Genauigkeit von Modellen für maschinelles Sehen und Klassifizierung in Azure Machine Learning
description: Erfahren Sie, wie Sie mit dem Azure Machine Learning Package for Computer Vision die Genauigkeit Ihrer Modelle für Bildklassifizierung, Objekterkennung und Bildähnlichkeit verbessern können.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: 6d6c540cd8711ae89784cdc797ca56d312522a83
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989310"
---
# <a name="improve-the-accuracy-of-computer-vision-models"></a>Verbessern der Genauigkeit von Modellen für maschinelles Sehen

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Mit dem **Azure Machine Learning Package for Computer Vision** können Sie Modelle für Bildklassifizierung, Objekterkennung und Bildähnlichkeit entwickeln und bereitstellen. Erfahren Sie mehr über dieses Paket und seine Installation.

In diesem Artikel erfahren Sie, wie Sie diese Modelle optimieren, um ihre Genauigkeit zu erhöhen. 

## <a name="accuracy-of-image-classification-models"></a>Genauigkeit von Bildklassifizierungsmodellen

Das Computer Vision Package liefert für eine Vielzahl von Datasets gute Ergebnisse. Doch wie für die meisten anderen Machine Learning-Projekte gilt, dass zum Erzielen bestmöglicher Ergebnisse für ein neues Dataset eine sorgfältige Parameteroptimierung und Abwägung unterschiedlicher Entwurfsentscheidungen erforderlich sind. In den folgenden Abschnitten erfahren Sie, wie Sie die Genauigkeit für ein bestimmtes Dataset verbessern können, d.h. welche Parameter zuerst optimiert werden sollten, welche Werte für diese Parameter probiert werden sollten und welche Klippen zu umschiffen sind.

Im Allgemeinen ist beim Training von Deep-Learning-Modellen ein Kompromiss zwischen Trainingszeit und Modellgenauigkeit einzugehen. Das Computer Vision Package bietet voreingestellte Standardparameter (siehe die erste Zeile in der nachstehenden Tabelle), die sich auf eine hohe Trainingsgeschwindigkeit konzentrieren, während üblicherweise Modelle mit hoher Genauigkeit erstellt werden. Diese Genauigkeit kann z.B. durch eine höhere Bildauflösung oder tiefere Modelle weiter verbessert werden, allerdings auf Kosten einer um mindestens das 10-fache längeren Trainingszeit.

Es wird empfohlen, zuerst mit den Standardparametern zu arbeiten, ein Modell zu trainieren, die Ergebnisse zu überprüfen, Ground Truth-Anmerkungen nach Bedarf zu korrigieren und erst dann Parameter auszuprobieren, die die Trainingszeit verlangsamen (siehe die nachstehende Tabelle mit vorgeschlagenen Parameterwerten). Ein Verständnis dieser Parameter, wenngleich technisch nicht notwendig, wird empfohlen.


### <a name="best-practices-and-tips"></a>Bewährte Methoden und Tipps

* Datenqualität: Die Trainings- und Testsets müssen eine hohe Qualität aufweisen. Dies bedeutet Folgendes: Die Bilder sollten mit den richtigen Anmerkungen versehen sein, mehrdeutige Bilder sollten entfernt werden (z.B. wenn es für das menschliche Auge unklar ist, ob das Bild einen Tennisball oder eine Zitrone zeigt), und die Attribute sollten sich gegenseitig ausschließen (sodass jedes Bild genau einem Attribut zugeordnet werden kann).

* Vor der Optimierung des DNN (Deep Neural Network) muss ein SVM-Klassifizierer (Support Vector Machine) mit einem vortrainierten und festen DNN als Featurizer trainiert werden. Dies wird im Computer Vision Package unterstützt und erfordert kein langes Training, da das DNN selbst nicht verändert wird. Auch dieser einfache Ansatz erreicht oft hohe Genauigkeiten und stellt somit solide Grundwerte dar. Der nächste Schritt ist dann eine Optimierung des DNN, die zu einer höheren Genauigkeit führen soll.

* Wenn das betreffende Objekt im Bild klein ist, funktionieren Ansätze, die auf Bildklassifizierung basieren, meist nicht so gut. Erwägen Sie in solchen Fällen die Verwendung eines Objekterkennungsansatzes, wie z.B. auf Tensorflow basierendes Faster R-CNNN im Computer Vision Package.

* Je mehr Trainingsdaten, desto besser. Als Faustregel sollte eine Klasse mindestens 100 Beispiele enthalten, also 100 Bilder für „Hund“, 100 Bilder für „Katze“ usw. Das Trainieren eines Modells mit weniger Bildern ist möglich, führt aber möglicherweise nicht zu guten Ergebnissen.

* Die Trainingsbilder müssen sich lokal auf dem Computer mit dem Grafikprozessor und auf einem SSD-Laufwerk (nicht auf einer Festplatte) befinden. Andernfalls kann die Wartezeit beim Lesen von Bildern die Trainingsgeschwindigkeit drastisch verlangsamen (sogar um das 100-fache).


### <a name="parameters-to-optimize"></a>Zu optimierende Parameter

Das Finden optimaler Werte für diese Parameter ist wichtig und kann die Genauigkeit oft erheblich verbessern:
* Lernrate (`lr_per_mb`): Der wohl wichtigste Parameter für ein gutes Ergebnis. Wenn die Genauigkeit des Trainingsdatasets nach der DNN-Optimierung über ~5 % liegt, ist die Lernrate höchstwahrscheinlich entweder zu hoch oder die Anzahl der Trainingsepochen zu niedrig. Insbesondere bei kleinen Datasets neigt das DNN dazu, die Trainingsdaten zu stark anzupassen, was aber in der Praxis zu guten Modellen für das Testdataset führt. Wir arbeiten in der Regel mit 15 Epochen, wobei die anfängliche Lernrate zweimal reduziert wird. Durch Trainieren mit mehr Epochen lässt sich in einigen Fällen die Leistung verbessern.

* Eingabeauflösung (`image_dims`): Die Standardbildauflösung ist 224 x 224 Pixel. Wenn eine höhere Bildauflösung von beispielsweise 500 x 500 oder 1000 x 1000 Pixel verwendet wird, kann dies zu einer Verbesserung der Genauigkeit, aber gleichzeitig zu einer Verlangsamung der DNN-Optimierung führen. Das Computer Vision Package erwartet, dass die Eingabeauflösung ein Tupel aus Farbkanälen, Bildbreite und Bildhöhe ist (z.B. 3, 224, 224), wobei die Anzahl der Farbkanäle auf 3 (die Rot-Grün-Blau-Bänder) festgelegt werden muss.

* Modellarchitektur (`base_model_name`): Versuchen Sie, DNNs mit mehr Tiefe wie z.B. ResNet-34 oder ResNet-50 anstelle des Standardmodells ResNet-18 zu verwenden. Das Resnet-50-Modell ist nicht nur tiefer gehend, sondern die Ausgabe der vorletzten Schicht beträgt auch 2048 Gleitkommazahlen (gegenüber 512 Gleitkommazahlen bei den Modellen ResNet-18 und ResNet-34). Diese erhöhte Dimensionalität kann besonders vorteilhaft sein, wenn das DNN fest bleibt und stattdessen ein SVM-Klassifizierer trainiert wird.

* Minibatchgröße (`mb_size`): Hohe Minibatchgrößen führen zu einer schnelleren Trainingszeit, allerdings auf Kosten einer erhöhten DNN-Arbeitsspeicherbelegung. Bei Wahl von Modellen mit mehr Tiefe (z.B. ResNet-50 statt ResNet-18) und/oder höherer Bildauflösung (500 \* 500 Pixel statt 224 \* 224 Pixel) muss daher in der Regel die Minibatchgröße reduziert werden, um Fehler aufgrund von Speichermangel zu vermeiden. Bei einer Änderung der Minibatchgröße muss oft auch die Lernrate angepasst werden, was in der nachstehenden Tabelle ersichtlich ist.
* Abbruchrate (`dropout_rate`) und L2-Regularisierung (`l2_reg_weight`): Die DNN-Überanpassung kann durch Verwendung einer Abbruchrate von 0,5 (im Computer Vision Package ist 0,5 Standard) oder höher und durch Erhöhung der Regularisierungsgewichtung (im Computer Vision Package ist 0,0005 Standard) reduziert werden. Beachten Sie jedoch, dass vor allem bei kleinen Datasets eine DNN-Überpassung nur schwer und oft gar nicht zu vermeiden ist.


### <a name="parameter-definitions"></a>Parameterdefinitionen

- **Lernrate**: Die beim Machine Learning in Bezug auf den Gradientenabstieg verwendete Schrittgröße. Wenn sie zu niedrig festgelegt ist, benötigt das Modell für das Training viele Epochen. Wenn sie zu hoch festgelegt ist, nähert sich das Modell nicht einer guten Lösung an. Üblicherweise wird ein Zeitplan herangezogen, gemäß dem die Lernrate nach einer bestimmten Anzahl von Epochen reduziert wird. Beispiel: Der Zeitplan für die Lernraten `[0.05]*7 + [0.005]*7 + [0.0005]` entspricht einer anfänglichen Lernrate von 0,05 für die ersten sieben Epochen, gefolgt von einer um das 10-fache reduzierten Lernrate von 0,005 für weitere sieben Epochen und schließlich der Feinabstimmung des Modells für eine einzelne Epoche mit einer um das 100-fache reduzierten Lernrate von 0,0005.

- **Minibatchgröße**: Grafikprozessoren können mehrere Bilder parallel verarbeiten, um die Berechnung zu beschleunigen. Diese parallel verarbeiteten Bilder werden auch als Minibatch bezeichnet. Je höher die Minibatchgrößen, desto schneller die Trainingszeit, allerdings auf Kosten einer erhöhten DNN-Arbeitsspeicherbelegung.

### <a name="recommended-parameter-values"></a>Empfohlene Parameterwerte

Die folgende Tabelle enthält unterschiedliche Parametersätze, die für eine große Bandbreite von Bildklassifizierungsaufgaben nachweislich hochpräzise Modelle erzeugen. Die optimalen Parameter hängen vom jeweiligen Dataset und dem verwendeten Grafikprozessor ab, weshalb die Tabelle nur als Orientierungshilfe dienen soll. Nach dem Testen dieser Parameter sollten Sie auch Bildauflösungen von mehr als 500 x 500 Pixel oder Modelle mit mehr Tiefe wie Resnet-101 oder Resnet-152 in Erwägung ziehen.

Die erste Zeile der Tabelle entspricht den Standardparametern, die im Computer Vision Package festgelegt sind. Alle anderen Zeilen benötigen mehr Trainingszeit (siehe die erste Spalte), jedoch bei erhöhter Genauigkeit (die zweite Spalte zeigt die durchschnittliche Genauigkeit über drei interne Datasets). Beispiel: Die Parameter in der letzten Zeile benötigen eine 5-15 Mal längere Trainingszeit, führten jedoch bei drei internen Testdatasets zu einer erhöhten (gemittelten) Genauigkeit von 82,6 % auf 92,8 %.

Tiefere Modelle und höhere Eingabeauflösung benötigen mehr DNN-Speicher, weshalb die Minibatchgröße mit zunehmender Modellkomplexität verkleinert werden muss, um Fehler aufgrund von Speichermangel zu vermeiden. Wie die folgende Tabelle zeigt, ist es vorteilhaft, die Lernrate um den Faktor zwei zu verringern, wenn die Minibatchgröße um den gleichen Multiplikator verkleinert wird. Bei Grafikprozessoren mit kleinerem Speicher muss die Minibatchgröße eventuell weiter verkleinert werden.

| Trainingszeit (grobe Schätzung) | Beispielgenauigkeit | Minibatchgröße (*mb_size*) | Lernrate (*lr_per_mb*) | Bildauflösung (*image_dims*) | DNN-Architektur (*base_model_name*) |
|------------- |:-------------:|:-------------:|:-----:|:-----:|:---:|
| 1-fach (Referenz) | 82,6 % | 32 | [0,05]\*7 + [0,005]\*7 + [0,0005]  | (3, 224, 224) | ResNet18_ImageNet_CNTK |
| 2- bis 5-fach    | 90,2 % | 16 | [0,025]\*7 + [0,0025]\*7 + [0,00025] | (3, 500, 500) | ResNet18_ImageNet_CNTK |
| 2- bis 5-fach    | 87,5 % | 16 | [0,025]\*7 + [0,0025]\*7 + [0,00025] | (3, 224, 224) | ResNet50_ImageNet_CNTK |
| 5- bis 15-fach        | 92,8 % |  8 | [0,01]\*7 + [0,001]\*7 + [0,0001]  | (3, 500, 500) | ResNet50_ImageNet_CNTK |


## <a name="next-steps"></a>Nächste Schritte

Sie wünschen weitere Informationen zum Azure Machine Learning-Paket für maschinelles Sehen?
+ Lesen Sie die Referenzdokumentation.

+ Erfahren Sie mehr über [andere Python-Pakete für Azure Machine Learning](reference-python-package-overview.md).
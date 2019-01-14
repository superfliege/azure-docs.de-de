---
title: Automatisierte ML-Algorithmusauswahl und -optimierung
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie der Azure Machine Learning-Dienst automatisch einen Algorithmus für Sie auswählen und ein Modell daraus generieren kann, um Ihnen Zeit zu sparen, indem er die von Ihnen angegebenen Parameter und Kriterien verwendet, um den besten Algorithmus für Ihr Modell auszuwählen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 9e3297b2493ea12f9da50556e4fc9e72d625fd25
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583448"
---
# <a name="what-is-automated-machine-learning"></a>Was ist automatisiertes maschinelles Lernen?

Automatisiertes maschinelles Lernen ist der Vorgang, bei dem Trainingsdaten mit einer definierten Zielfunktion verwendet werden und durch Iterieren durch Kombinationen von Algorithmen und Featureauswahlen automatisch das beste Modell für Ihre Daten basierend auf den Trainingswerten ausgewählt wird. Der traditionelle Modellentwicklungsprozess für das maschinelle Lernen ist sehr ressourcenintensiv und erfordert erhebliches Fachwissen und großen Zeitaufwand, um die Ergebnisse von Dutzenden von Modellen auszuführen und zu vergleichen. Automatisiertes maschinelles Lernen vereinfacht diesen Vorgang, indem Modelle generiert werden, die anhand der Ziele und Einschränkungen optimiert werden, die Sie für Ihr Experiment definiert haben, z.B. die Zeit für die Ausführung des Experiments oder welche Modelle auf die schwarze Liste gesetzt werden sollen.

## <a name="how-it-works"></a>So funktioniert's

1. Sie konfigurieren die Art des zu lösenden Problems für das maschinelle Lernen. Die folgenden Kategorien des überwachten Lernens werden unterstützt:
   + Classification
   + Regression
   + Vorhersagen

   Während automatisiertes maschinelles Lernen allgemein verfügbar ist, befindet sich **die Prognosefunktion noch in der öffentlichen Vorschau.**

   Hier finden Sie die [Liste der Modelle](how-to-configure-auto-train.md#select-your-experiment-type), die Azure Machine Learning beim Trainieren ausprobieren kann.

1. Sie geben die Quelle und das Format für die Trainingsdaten an. Die Daten müssen bezeichnet sein und können in Ihrer Entwicklungsumgebung oder in Azure Blob Storage gespeichert werden. Wenn die Daten in Ihrer Entwicklungsumgebung gespeichert sind, müssen sie sich im gleichen Verzeichnis wie Ihre Trainingsskripts befinden. Dieses Verzeichnis wird auf das Computeziel kopiert, das Sie für das Training auswählen.

    In Ihrem Trainingsskript können die Daten in Numpy-Arrays oder einen Pandas-Datenrahmen gelesen werden.

    Sie können Teilungsoptionen für die Auswahl von Trainings- und Validierungsdaten konfigurieren, oder Sie können separate Trainings- und Validierungsdatasets angeben.

1. Konfigurieren Sie das [Computeziel](how-to-set-up-training-targets.md), das zum Trainieren des Modells verwendet wird.

1. Konfigurieren Sie das automatisierte maschinelle Lernen. Diese Konfiguration steuert die verwendeten Parameter, während Azure Machine Learning verschiedene Modelle durchläuft, sowie Hyperparametereinstellungen und anhand welcher Metriken das beste Modell ermittelt wird. 

1. Übermitteln Sie eine Trainingsausführung.

Während des Trainings erstellt der Azure Machine Learning-Dienst eine Reihe von Pipelines, die unterschiedliche Algorithmen und Parametern ausprobieren. Der Vorgang wird beendet, sobald die von Ihnen festgelegte Grenze für Iterationen bzw. der Zielwert für die von Ihnen festgelegte Metrik erreicht ist.

[ ![Automatisiertes maschinelles Lernen](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Sie können die protokollierten Ausführungsinformationen prüfen, die während der Ausführung erfasste Metriken enthalten. Bei der Trainingsausführung wird auch ein serialisiertes Python-Objekt (`.pkl`-Datei) generiert, das die Vorabverarbeitung des Modells und der Daten enthält.

## <a name="model-explainability"></a>Modellerklärung

Eine häufige Falle des automatisierten maschinellen Lernens ist die Unfähigkeit, den End-to-End-Prozess sehen zu können. Azure Machine Learning ermöglicht es Ihnen, detaillierte Informationen zu den Modellen anzuzeigen, um die Transparenz der Vorgänge zu erhöhen, die auf dem Back-End ausgeführt werden. Einige Modelle (wie die lineare Regression) gelten als ziemlich einfach und daher leicht verständlich. Aber wenn wir mehr Funktionen hinzufügen und kompliziertere maschinelle Lernmodelle verwenden, wird das Verständnis immer schwieriger. Es gibt zwei wesentliche Aspekte der Transparenz beim maschinellen Lernen:

1. Kenntnis der Machine Learning-Pipeline und aller damit verbundenen Schritte, einschließlich Datenvorverarbeitung/Featurisierung und Hyperparameterwerte.
1. Verständnis des Zusammenhangs zwischen Eingangsvariablen (auch als „Features“ bezeichnet) und der Modellausgabe.  Wenn Sie sowohl das Ausmaß als auch die Richtung der Auswirkungen der einzelnen Features auf den vorhergesagten Wert kennen, können Sie das Modell besser verstehen und erklären. Dies wird als „Featurgewichtung“ bezeichnet.

Sie können bedarfsgesteuert globale Featuregewichtung nach dem Training für die Pipeline Ihrer Wahl oder für alle Pipelines im Rahmen eines automatisierten ML-Trainings aktivieren.  Dies ist ein Vorschaufeature. Wir bemühen uns, weiterhin umfangreiche Informationen bereitzustellen, um Ihnen zu helfen, Ihre ML-Modelle besser zu verstehen.  

Verwenden Sie dieses [Beispiel-Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/model-explanation/auto-ml-model-explanation.ipynb) zum Experimentieren mit Modellerklärungen in Azure Machine Learning.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Modelle mithilfe des automatisierten maschinellen Lernens erstellt werden, und sehen Sie sich Beispiele dazu an:
+ [Beispiele: Verwenden von Jupyter-Notebooks zum Erkunden von Azure Machine Learning Service](samples-notebooks.md#automated-ml-setup)

+ [Tutorial: Automatisches Trainieren eines Klassifizierungsmodells mithilfe von Azure Automated Machine Learning](tutorial-auto-train-models.md)

+ [Verwenden von automatischem Training für eine Remoteressource](how-to-auto-train-remote.md)

+ [Konfigurieren der Einstellungen für automatisches Training](how-to-configure-auto-train.md)

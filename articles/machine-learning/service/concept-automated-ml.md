---
title: Automatisierte ML-Algorithmusauswahl und -optimierung
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie der Azure Machine Learning-Dienst automatisch einen Algorithmus für Sie auswählen und ein Modell daraus generieren kann, um Ihnen Zeit zu sparen, indem er die von Ihnen angegebenen Parameter und Kriterien verwendet, um den besten Algorithmus für Ihr Modell auszuwählen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 620dbd22613df37fdc3c20e34906684446b2251f
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546002"
---
# <a name="what-is-automated-machine-learning"></a>Was ist automatisiertes maschinelles Lernen?

Automatisiertes maschinelles Lernen ist der Vorgang, bei dem Trainingsdaten mit einer definierten Zielfunktion verwendet werden und durch Iterieren durch Kombinationen von Algorithmen und Featureauswahlen automatisch das beste Modell für Ihre Daten basierend auf den Trainingswerten ausgewählt wird. Der traditionelle Modellentwicklungsprozess für das maschinelle Lernen ist sehr ressourcenintensiv und erfordert erhebliches Fachwissen und großen Zeitaufwand, um die Ergebnisse von Dutzenden von Modellen auszuführen und zu vergleichen. Automatisiertes maschinelles Lernen vereinfacht diesen Vorgang, indem Modelle generiert werden, die anhand der Ziele und Einschränkungen optimiert werden, die Sie für Ihr Experiment definiert haben, z.B. die Zeit für die Ausführung des Experiments oder welche Modelle auf die Blockliste gesetzt werden sollen.

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

[![Automatisiertes maschinelles Lernen](./media/how-to-automated-ml/automated-machine-learning.png)](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Sie können die protokollierten Ausführungsinformationen prüfen, die während der Ausführung erfasste Metriken enthalten. Bei der Trainingsausführung wird auch ein serialisiertes Python-Objekt (`.pkl`-Datei) generiert, das die Vorabverarbeitung des Modells und der Daten enthält.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Modelle mithilfe des automatisierten maschinellen Lernens erstellt werden, und sehen Sie sich Beispiele dazu an:

+ [Tutorial: Automatisches Trainieren eines Klassifizierungsmodells mithilfe von Azure Automated Machine Learning](tutorial-auto-train-models.md)

+ [Notebook-Beispiele](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)

+ [Verwenden von automatischem Training für eine Remoteressource](how-to-auto-train-remote.md)

+ [Konfigurieren der Einstellungen für automatisches Training](how-to-configure-auto-train.md)

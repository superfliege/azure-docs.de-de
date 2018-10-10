---
title: Was ist automatisiertes maschinelles Lernen – Azure Machine Learning?
description: In diesem Artikel erhalten Sie Informationen zum automatisierten maschinellen Lernen. Der Azure Machine Learning-Dienst kann automatisch einen Algorithmus für Sie wählen und ein Modell daraus generieren. Mit dem automatisierten maschinellen Lernen sparen Sie Zeit, indem Sie die Parameter und Kriterien verwenden, die Sie angeben, um den besten Algorithmus für Ihr Modell auszuwählen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: krishnan
author: krishnaanumalasetty
ms.date: 9/24/2018
ms.openlocfilehash: 2a9c05b68d05102fab80b2aa8fb1c1dad8a367ea
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960038"
---
# <a name="what-is-automated-machine-learning"></a>Was ist automatisiertes maschinelles Lernen?

In diesem Artikel erhalten Sie Informationen zum automatisierten maschinellen Lernen. Der Azure Machine Learning-Dienst kann automatisch einen Algorithmus für Sie wählen und ein Modell daraus generieren. Mit dem automatisierten maschinellen Lernen sparen Sie Zeit, indem Sie die Parameter und Kriterien verwenden, die Sie angeben, um den besten Algorithmus für Ihr Modell auszuwählen.

## <a name="how-it-works"></a>So funktioniert's

1. Sie konfigurieren die Art des zu lösenden Problems für das maschinelle Lernen. Zwei Kategorien des überwachten Lernens werden unterstützt:
   + Classification
   + Regression

   Hier finden Sie die [Liste der Modelle](how-to-configure-auto-train.md#select-your-experiment-type), die Azure Machine Learning beim Trainieren ausprobieren kann.

1. Sie geben die Quelle und das Format für die Trainingsdaten an. Die Daten müssen bezeichnet sein und können in Ihrer Entwicklungsumgebung oder in Azure Blob Storage gespeichert werden. Wenn die Daten in Ihrer Entwicklungsumgebung gespeichert sind, müssen sie sich im gleichen Verzeichnis wie Ihre Trainingsskripts befinden. Dieses Verzeichnis wird auf das Computeziel kopiert, das Sie für das Training auswählen.

    In Ihrem Trainingsskript können die Daten in Numpy-Arrays oder einen Pandas-Datenrahmen gelesen werden.

    Sie können Teilungsoptionen für die Auswahl von Trainings- und Validierungsdaten konfigurieren, oder Sie können separate Trainings- und Validierungsdatasets angeben.

1. Konfigurieren Sie das [Computeziel](how-to-set-up-training-targets.md), das zum Trainieren des Modells verwendet wird.

1. Konfigurieren Sie das automatisierte maschinelle Lernen. Diese Konfiguration steuert die verwendeten Parameter, während Azure Machine Learning verschiedene Modelle durchläuft, sowie Hyperparametereinstellungen und anhand welcher Metriken das beste Modell ermittelt wird. 

1. Übermitteln Sie eine Trainingsausführung.

Während des Trainings erstellt der Azure Machine Learning-Dienst eine Reihe von Pipelines, die unterschiedliche Algorithmen und Parametern ausprobieren. Der Vorgang wird beendet, sobald die von Ihnen festgelegte Grenze für Iterationen bzw. der Zielwert für die von Ihnen festgelegte Metrik erreicht ist.

[ ![Automatisiertes maschinelles Lernen](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Sie können die protokollierten Ausführungsinformationen prüfen, die während der Ausführung erfasste Metriken enthalten. Bei der Trainingsausführung wird auch ein serialisiertes Python-Objekt (PKL-Datei) erzeugt, das die Vorabverarbeitung des Modells und der Daten enthält.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Modelle mithilfe des automatisierten maschinellen Lernens erstellt werden, und sehen Sie sich Beispiele dazu an:

+ [Tutorial: Automatisches Trainieren eines Klassifizierungsmodells mithilfe von Azure Automated Machine Learning](tutorial-auto-train-models.md)

+ [Konfigurieren der Einstellungen für das automatische Training](how-to-configure-auto-train.md)

+ [Trainieren von Modellen mit automatisiertem maschinellem Lernen in der Cloud](how-to-auto-train-remote.md) 

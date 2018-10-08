---
title: Optimieren von Hyperparametern für Ihr Modell mithilfe von Azure Machine Learning
description: Erfahren Sie, wie Sie die Hyperparameter für Ihr Deep Learning-/Machine Learning-Modell mithilfe des Azure Machine Learning Services optimieren. Sie erfahren, wie der Parametersuchraum definiert und eine primäre Metrik zum Optimieren angegeben und wie Konfigurationen mit schlechter Leistung früh beendet werden.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: b6370fd9125c5b14df781b27e028c139175b7589
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405800"
---
# <a name="tune-hyperparameters-for-your-model"></a>Optimieren von Hyperparametern für Ihr Modell

In diesem Artikel erfahren Sie, wie Sie Hyperparameter für Ihr Modell effizient optimieren. Sie erfahren, wie der Parametersuchraum definiert und eine primäre Metrik zum Optimieren angegeben und wie Konfigurationen mit schlechter Leistung früh beendet werden. Außerdem können Sie die verschiedenen Trainingsläufe visualisieren und die Konfiguration mit der besten Leistung für Ihr Modell auswählen.

## <a name="what-are-hyperparameters"></a>Was sind Hyperparameter?
Hyperparameter sind anpassbare Parameter, die vor dem Trainieren eines Modells ausgewählt werden und den eigentlichen Trainingsprozess steuern. Beispielsweise müssen Sie vor dem Trainieren eines Deep Neural Networks über die Anzahl der verborgenen Schichten im Netzwerk und die Anzahl der Knoten in jeder Schicht entscheiden. Diese Werte bleiben normalerweise während des Trainingsprozesses konstant.

In Deep Learning-/Machine Learning-Szenarien hängt die Leistung von Modellen stark von den gewählten Werten der Hyperparameter ab. Das Ziel der Untersuchung von Hyperparametern besteht darin, übergreifend verschiedene Hyperparameterkonfigurationen zu durchsuchen, um eine Konfiguration zu finden, die zur gewünschten Leistung führt. Normalerweise ist die Untersuchung von Hyperparametern ein mühevoller manueller Vorgang, angesichts des riesigen Suchbereichs und der hohen Kosten für die Bewertung der einzelnen Konfigurationen.

In Azure Machine Learning können Sie diese Untersuchung von Hyperparametern in effizienter Weise automatisieren, was in erheblichem Umfang Zeit und Ressourcen spart. Sie können den Bereich der zu untersuchenden Hyperparameterwerte und eine maximale Anzahl Trainingsläufe für diese Untersuchung angeben. Das System startet anschließend automatisch mehrere gleichzeitige Trainingsläufe mit verschiedenen Parameterkonfigurationen und findet die Konfiguration, die die beste Leistung ergibt, gemessen an einer vom Benutzer gewählten Metrik. Trainingsläufe mit schlechter Leistung werden automatisch frühzeitig beendet, um die Vergeudung von Computeressourcen zu verringern. Stattdessen werden diese Ressourcen dazu verwendet, andere Hyperparameterkonfigurationen zu untersuchen.

Um die Hyperparameter für Ihr Modell mithilfe des Azure Machine Learning Services zu optimieren, müssen Sie wie folgt vorgehen:
* Definieren des Suchbereichs für Hyperparameter
* Festlegen einer zu optimierenden primären Metrik
* Festlegen einer Richtlinie für die frühzeitige Beendigung
* Zuweisen von Ressourcen für die Optimierung der Hyperparameter
* Starten eines Experiments mit der oben dargestellten Konfiguration

## <a name="define-the-hyperparameter-search-space"></a>Definieren des Suchbereichs für Hyperparameter
Der Azure Machine Learning-Dienst optimiert Hyperparameter automatisch durch Untersuchen des für jeden Hyperparameter definierten Wertebereichs.

### <a name="types-of-hyperparameters"></a>Typen von Hyperparametern
Jede Hyperparameter kann entweder diskret oder kontinuierlich sein.

#### <a name="discrete-hyperparameters"></a>Diskrete Hyperparameter 
Diskrete Hyperparameter können als eine `choice` unter diskreten Werten angegeben werden. `choice` kann einen oder mehrere kommagetrennte Werte annehmen, ein `range`-Objekt oder ein arbiträres `list`-Objekt. Beispiel:  

```Python
    {    
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

In diesem Fall kann batch_size einen der Werte [16, 32, 64, 128] und number_of_hidden_layers einen der Werte [1, 2, 3, 4] annehmen.

Erweiterte diskrete Hyperparameter können ferner mithilfe einer Verteilung angegeben werden. Die folgenden Verteilungen werden unterstützt:
* `quniform(low, high, q)`: Gibt einen Wert wie „round(uniform(low, high) / q) * q“ zurück
* `qloguniform(low, high, q)`: Gibt einen Wert wie „round(exp(uniform(low, high)) / q) * q“ zurück
* `qnormal(mu, sigma, q)`: Gibt einen Wert wie „round(normal(mu, sigma) / q) * q“ zurück
* `qlognormal(mu, sigma, q)`: Gibt einen Wert wie „round(exp(normal(mu, sigma)) / q) * q“ zurück

#### <a name="continuous-hyperparameters"></a>Kontinuierliche Hyperparameter 
Kontinuierliche Hyperparameter können als Verteilung über einen kontinuierlichen Wertebereich angegeben werden. Zu den unterstützten Verteilungen gehören:
* `uniform(low, high)`:Gibt einen gleichmäßig zwischen niedrig und hoch verteilen Wert zurück
* `loguniform(low, high)`: Gibt einen gemäß „exp(uniform(low, high))“ ermittelten Wert zurück, sodass der Logarithmus des Rückgabewerts gleichmäßig verteilt ist
* `normal(mu, sigma)`: Gibt einen realen Wert zurück, der mit einem Mittelwert mu und der Standardabweichung sigma normalverteilt ist
* `lognormal(mu, sigma)`: Gibt einen gemäß „exp(normal(mu, sigma))“ ermittelten Wert zurück, sodass der Logarithmus des Rückgabewerts normalverteilt ist

Hier folgt ein Beispiel für eine Definition des Parameterraums:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Dieses Beispiel definiert einen Suchraum mit zwei Parametern: learning_rate und keep_probability. learning_rate weist eine Normalverteilung mit dem Mittelwert 10 und der Standardabweichung 3 auf. keep_probability hat eine gleichmäßige Verteilung mit dem Mindestwert 0,05 und dem Höchstwert 0,1.

### <a name="sampling-the-hyperparameter-space"></a>Stichprobenentnahme im Hyperparameterraum
Der Benutzer legt außerdem die für die angegebene Definition des Hyperparameterraums zu verwendende Methode für die Stichprobenentnahme (Sampling) von Parametern fest. Der Azure Machine Learning-Dienst unterstützt Zufallssampling, Rastersampling und Bayessches Sampling.

#### <a name="random-sampling"></a>Zufallssampling
Beim Zufallssampling werden Hyperparameterwerte zufällig aus dem definierten Suchraum ausgewählt. Bei Zufallssampling darf der Suchraum sowohl diskrete als auch kontinuierliche Hyperparameter enthalten. Beispiel:

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Rastersampling
Beim Rastersampling wird eine einfache Rastersuche über alle zulässigen Werte im definierten Suchraum ausgeführt. Es kann nur für Hyperparameter verwendet werden, die mithilfe von `choice` angegeben wurden. Beispielsweise weist der folgende Raum insgesamt sechs Stichproben auf:

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Bayessches Sampling
Bayessches Sampling basiert auf dem Bayesschen Optimierungsalgorithmus und trifft intelligente Entscheidungen zu den als nächstes zu sampelnden Hyperparameterwerten. Es sucht die betreffende Stichprobe auf der Grundlage der Leistung der vorherigen Stichprobe so aus, dass die neue Stichprobe die gemeldete primäre Metrik verbessert.

Bei der Verwendung von Bayesschem Sampling hat die Anzahl der gleichzeitig ausgeführten Läufe eine Auswirkung auf die Wirksamkeit des Optimierungsprozesses. Normalerweise kann eine kleinere Anzahl gleichzeitiger Läufe zu einer besseren Samplingkonvergenz führen. Das hat den Grund, dass durch den geringeren Parallelitätsgrad die Anzahl der Ausführungen steigt, die von zuvor abgeschlossenen Läufen profitieren.

Bayessches Sampling unterstützt nur `choice`- und `uniform`-Verteilungen über den Suchraum. Beispiel: 

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> Beim Bayesschen Sampling wird zurzeit keine Richtlinie für frühzeitige Beendigung unterstützt (Siehe dazu [Festlegen einer Richtlinie für die frühzeitige Beendigung](#specify-an-early-termination-policy)). Bei der Verwendung des Bayesschen Parametersamplings müssen Sie die Richtlinie auf `None` festlegen. Keine Richtlinie für die Beendigung beim Bayesschen Sampling anzugeben, hat die gleiche Wirkung.
>
> ```Python
> early_termination_policy = None
> ```

## <a name="specify-a-primary-metric-to-optimize"></a>Festlegen einer zu optimierenden primären Metrik
Beim Optimieren von Hyperparametern müssen Sie die primäre Metrik angeben, die vom Experiment für die Hyperparameteroptimierung optimiert werden soll. Jeder Trainingslauf wird für diese primäre Metrik ausgewertet, und Läufe mit schlechter Leistung (bei denen die primäre Metrik nicht den von der Richtlinie für frühzeitige Beendigung festgelegten Kriterien entspricht) werden beendet. Über den Namen der primären Metrik hinaus müssen Sie außerdem das Ziel ihrer Optimierung angeben – ob die primäre Metrik maxi- oder minimiert werden soll.
* `primary_metric_name`: Der Name der zu optimierenden primären Metrik. Der Name der primären Metrik muss exakt mit dem Namen der vom Trainingsskript protokollierten Metrik übereinstimmen. Weitere Informationen finden Sie unter [Protokollieren von Metriken für die Hyperparameteroptimierung](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: Kann entweder `PrimaryMetricGoal.MAXIMIZE` oder `PrimaryMetricGoal.MINIMIZE` sein und bestimmt, ob die primäre Metrik beim Bewerten der Läufe maximiert oder minimiert wird. 

Beispiel: 
```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```
Die Läufe werden zur Maximierung von "accuracy" (Genauigkeit) optimiert.

### <a name="log-metrics-for-hyperparameter-tuning"></a>Protokollieren von Metriken für die Hyperparameteroptimierung
Um den Azure Machine Learning Service für die Hyperparameteroptimierung zu verwenden, muss das Trainingsskript für Ihr Modell während der Ausführung des Modells relevante Metriken melden. Der Benutzer gibt die primäre Metrik an, die der Dienst zur Bewertung der Ausführungsleistung verwenden soll, und das Trainingsskript muss diese Metrik protokollieren. Siehe dazu [Festlegen einer zu optimierenden primären Metrik](#specify-a-primary-metric-to-optimize).

Sie können Ihr Trainingsskript so aktualisieren, dass es diese Metrik protokolliert, etwa auf der Grundlage dieses Beispielcodeausschnitts:

```Python
from azureml.core.run import Run
run_logger = Run.get_submitted_run()
run_logger.log("accuracy", float(val_accuracy))
```

In diesem Beispiel berechnet das Trainingsskript `val_accuracy` und protokolliert diese "accuracy", die als primäre Metrik verwendet wird. Die Entscheidung, wie häufig diese Metrik gemeldet werden soll, liegt beim Entwickler des Modells.

## <a name="specify-an-early-termination-policy"></a>Festlegen einer Richtlinie für die frühzeitige Beendigung
Bei der Verwendung des Azure Machine Learning Services zur Optimierung von Hyperparametern werden Läufe mit schlechter Leistung automatisch frühzeitig beendet. Dadurch wird die Vergeudung von Ressourcen verringert, die stattdessen zur Untersuchung weiterer Parameterkonfigurationen eingesetzt werden.

Wenn eine Richtlinie zur frühzeitigen Beendigung verwendet wird, kann ein Benutzer die folgenden Parameter konfigurieren, die steuern, wann eine Richtlinie angewendet wird:
* `evaluation_interval`: die Anwendungshäufigkeit der Richtlinie. Jede Protokollierung der primären Metrik durch das Trainingsskript zählt als ein Intervall. Also wird beim Wert 1 für `evaluation_interval` die Richtlinie jedes Mal angewendet, wenn das Trainingsskript die primäre Metrik meldet. Beim Wert 2 für `evaluation_interval` wird die Richtlinie bei jeder zweiten Meldung der primären Metrik durch das Trainingsskript angewendet. Dies ist ein optionaler Parameter. Wird er nicht angegeben, ist `evaluation_interval` standardmäßig auf 1 festgelegt.
* `delay_evaluation`: Verzögert die erste Auswertung der Richtlinie für eine angegebene Anzahl Intervalle. Dies ist ein optionaler Parameter, der die Ausführung aller Konfigurationen für eine anfängliche Minimalanzahl Intervalle ermöglicht, um die vorzeitige Beendigung von Trainingsläufen zu verhindern. Wenn er angegeben wird, wird dir Richtlinie bei jedem Vielfachen von evaluation_interval angewendet, das größer als oder gleich groß wie delay_evaluation ist.

Der Azure Machine Learning-Dienst unterstützt die folgenden Richtlinien für die frühzeitige Beendigung:

### <a name="bandit-policy"></a>Banditenrichtlinie
Eine Banditenrichtlinie ist eine Beendigungsrichtlinie, die auf einem Pufferbereich/Pufferbetrag und einem Bewertungsintervall basiert. Diese Richtlinie beendet frühzeitig alle Läufe, bei denen die primäre Metrik bezogen auf den Trainingslauf mit der besten Leistung nicht innerhalb des angegebenen Pufferbereichs/Pufferbetrags liegt. Sie nimmt die folgenden Konfigurationsparameter an:
* `slack_factor` oder `slack_amount`: Der bezogen auf den Trainingslauf mit der besten Leistung zulässige Puffer. `slack_factor` gibt den zulässigen Puffer als Verhältnis an. `slack_amount` gibt den zulässigen Puffer als absoluten Betrag anstelle eines Verhältnisses an.

    Betrachten Sie als Beispiel die Anwendung einer Banditenrichtlinie auf das Intervall 10. Angenommen, der Lauf mit der besten Leistung in Intervall 10 hat für das Ziel, die primäre Metrik zu maximieren, die primäre Metrik 0,8 gemeldet. Wenn die Richtlinie mit einem `slack_factor` von 0,2 angegeben wurde, werden alle Trainingsläufe, deren beste Metrik im Intervall 10 kleiner als 0,66 (0,8/(1+`slack_factor`)) ist, beendet. Wenn die Richtlinie stattdessen mit einem `slack_amount` von 0,2 angegeben wurde, werden alle Trainingsläufe, deren beste Metrik im Intervall 10 kleiner als 0,6 (0,8/(1+`slack_amount`)) ist, beendet.
* `evaluation_interval`: die Anwendungshäufigkeit der Richtlinie (optionaler Parameter).
* `delay_evaluation`: verzögert die erste Auswertung der Richtlinie für eine angegebene Anzahl Intervalle (optionaler Parameter).

Betrachten Sie dieses Beispiel:

```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

In diesem Beispiel wird die Richtlinie zur frühzeitigen Beendigung bei jedem Intervall angewendet, wenn Metriken gemeldet werden, beginnend bei Auswertungsintervall 5. Jeder Lauf, dessen beste Metrik kleiner als (1/(1+0,1) oder 91 % des Laufs mit der besten Leistung ist, wird beendet.

### <a name="median-stopping-policy"></a>Medianstopprichtlinie
Die Medianstopprichtlinie ist eine Richtlinie zur frühzeitigen Beendigung, die auf dem gleitenden Durchschnitt der von den Läufen gemeldeten primären Metriken basiert. Diese Richtlinie berechnet den gleitenden Durchschnitt über alle Trainingsläufe und beendet Läufe, deren Leistung schlechter als der Median der gleitenden Durchschnittswerte ist. Diese Richtlinie nimmt die folgenden Konfigurationsparameter an:
* `evaluation_interval`: die Anwendungshäufigkeit der Richtlinie (optionaler Parameter).
* `delay_evaluation`: verzögert die erste Auswertung der Richtlinie für eine angegebene Anzahl Intervalle (optionaler Parameter).

Betrachten Sie dieses Beispiel:

```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

In diesem Beispiel wird die Richtlinie zur frühzeitigen Beendigung bei jedem Intervall angewendet, beginnend bei Auswertungsintervall 5. Ein Lauf wird in Intervall 5 beendet, wenn seine beste primäre Metrik schlechter als der Median des gleitenden Durchschnitts der Intervalle 1:5 über alle Trainingsläufe ist.

### <a name="truncation-selection-policy"></a>Kürzungsauswahlrichtlinie
Die Kürzungsauswahlrichtlinie bricht bei jedem Auswertungsintervall einen angegebenen Prozentsatz der Läufe mit der schlechtesten Leistung ab. Die Läufe werden auf der Grundlage ihrer Leistung bezogen auf die primäre Metrik verglichen, und die niedrigsten X % werden beendet. Sie nimmt die folgenden Konfigurationsparameter an:
* `truncation_percentage`: der Prozentsatz der Läufe mit der schwächsten Leistung, die bei jedem Auswertungsintervall beendet werden sollen. Der Wert muss eine ganze Zahl zwischen 1 und 99 sein.
* `evaluation_interval`: die Anwendungshäufigkeit der Richtlinie (optionaler Parameter).
* `delay_evaluation`: verzögert die erste Auswertung der Richtlinie für eine angegebene Anzahl Intervalle (optionaler Parameter).

Betrachten Sie dieses Beispiel:

```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

In diesem Beispiel wird die Richtlinie zur frühzeitigen Beendigung bei jedem Intervall angewendet, beginnend bei Auswertungsintervall 5. Ein Lauf wird bei Intervall 5 beendet, wenn seine Leistung in den unteren 20 % der Leistung aller Läufe bei Intervall 5 liegt.

### <a name="no-termination-policy"></a>Keine Beendigungsrichtlinie
Wenn Sie alle Trainingsläufe bis zum Abschluss ausführen lassen möchten, verwenden Sie „NoTerminationPolicy“. Das hat die Wirkung, dass keine Richtlinie zur frühzeitigen Beendigung angewendet wird. Beispiel: 

```Python
from azureml.train.hyperdrive import NoTerminationPolicy
early_termination_policy = NoTerminationPolicy()
```

### <a name="default-policy"></a>Standardrichtlinie
Wenn keine Richtlinie angegeben wird, verwendet der Dienst zur Optimierung von Hyperparametern standardmäßig eine Medianstopprichtlinie mit `evaluation_interval` 1 und `delay_evaluation` 5. Dies ist eine konservative Einstellung, die annähernd 25 %–35 % Ersparnis ohne Verluste bei der primären Metrik erbringen kann (bezogen auf unsere Auswertungsdaten).

## <a name="allocate-resources-for-hyperparameter-tuning"></a>Zuweisen von Ressourcen für die Optimierung der Hyperparameter
Sie können Ihr Ressourcenbudget für Ihr Experiment zur Optimierung von Hyperparametern kontrollieren, indem Sie die maximale Gesamtzahl der Trainingsläufe und optional die maximale Dauer (in Minuten) für das Experiment zur Optimierung von Hyperparametern festlegen. 
* `max_total_runs`: die maximale Gesamtzahl von Trainingsläufen, die erstellt werden. Diese ist eine Obergrenze – es können weniger Läufe auftreten, beispielsweise wenn der Hyperparameterraum endlich ist und weniger Stichproben aufweist. Dies muss eine Zahl zwischen 1 und 1000 sein.
* `max_duration_minutes`: maximale Dauer des Experiments zur Optimierung von Hyperparametern in Minuten. Dies ist ein optionaler Parameter, und falls er angegeben wird, werden alle Läufe, die nach Ablauf dieses Zeitraums ausgeführt werden, automatisch abgebrochen.

>[!NOTE] 
>Wenn sowohl `max_total_runs` als auch `max_duration_minutes` angegeben werden, wird das Experiment zur Optimierung von Hyperparametern beim Erreichen des ersten dieser beiden Schwellenwerte beendet.

Darüber hinaus können Sie die maximale Anzahl der während Ihrer Optimierungssuche für Hyperparameter parallel auszuführenden Läufe angeben.
* `max_concurrent_runs`: Dies ist die maximale Anzahl der zu jedem bestimmten Zeitpunkt gleichzeitig auszuführenden Läufe. Ohne diese Angabe werden alle `max_total_runs` parallel gestartet. Wenn der Wert angegeben wird, muss er eine Zahl zwischen 1 und 100 sein.

>[!NOTE] 
>Die Anzahl der gleichzeitigen Läufe wird durch die im angegebenen Computeziel verfügbaren Ressourcen beschränkt. Daher müssen Sie sicherstellen, dass das Computeziel die verfügbaren Ressourcen für die gewünschte Parallelität aufweist.

Sie können Ressourcen für die Optimierung von Hyperparametern wie in diesem Beispiel gezeigt zuweisen:
```Python
max_total_runs=20,
max_concurrent_runs=4
```
Dadurch wird das Experiment zur Optimierung von Hyperparametern für ein Maximum von 20 Läufen insgesamt mit Ausführung von 4 Konfigurationen zur gleichen Zeit konfiguriert.

## <a name="configure-your-hyperparameter-tuning-experiment"></a>Konfigurieren Ihres Experiments zum Optimieren von Hyperparametern
Sie können Ihr Experiment zur Optimierung von Hyperparametern mithilfe des definierten Suchraums für Hyperparameter, der Richtlinie für die frühzeitige Beendigung und der Ressourcenzuordnung konfigurieren, die in den vorhergehenden Abschnitten beschrieben wurden. Darüber hinaus müssen Sie einen `estimator` bereitstellen, der mit den als Stichprobe entnommenen Hyperparametern aufgerufen wird. Der `estimator` beschreibt das auszuführende Trainingsskript, die Ressourcen pro Auftrag (einzelne oder mehrere GPUs) und das zu verwendende Computeziel. Da die Parallelität für Ihr Experiment zur Optimierung von Hyperparametern durch die verfügbaren Ressourcen beschränkt ist, müssen Sie sicherstellen, dass das von Ihnen im `estimator` angegebene Computeziel über ausreichend Ressourcen für die gewünschte Parallelität verfügt. (Weitere Informationen zu Kalkulatoren (estimators) finden Sie unter [how to train models](how-to-train-ml-models.md) (Trainieren von Modellen)).

Hier ist ein Beispiel einer möglichen Konfiguration Ihres Experiments zur Optimierung von Hyperparametern:

```Python
from azureml.train.hyperdrive import HyperDriveRunConfig
hyperdrive_run_config = HyperDriveRunConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-your-hyperparameter-tuning-experiment"></a>Absenden Ihres Experiments zur Optimierung von Hyperparametern
Nachdem Sie Ihre Konfiguration zur Optimierung von Hyperparametern definiert haben, können Sie ein Experiment absenden, das diese Konfiguration verwendet:

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

wobei `experiment_name` der Name, den Sie Ihrem Experiment zur Optimierung von Hyperparametern zuweisen, und `workspace` der Arbeitsbereich ist, in dem Sie das Experiment erstellen möchten (Weitere Informationen zu Experimenten finden Sie über diesen [Link](/concept-azure-machine-learning-architecture.md)).

## <a name="visualize-your-hyperparameter-tuning-experiment"></a>Visualisieren Ihres Experiments zum Optimieren von Hyperparametern
Das Azure Machine Learning-SDK stellt ein Notebook-Widget bereit, das zum Visualisieren des Fortschritts Ihrer Trainingsläufe verwendet werden kann. Der folgende Codeausschnitt kann dazu verwendet werden, alle Ihre Optimierungsläufe für Hyperparameter an einem Ort zu visualisieren:

```Python
from azureml.train.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Dadurch wird eine Tabelle mit Details zu den Trainingsläufen für jede der Hyperparameterkonfigurationen angezeigt. Beispiel:

![Optimierungstabelle für Hyperparameter](media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

Ferner können Sie die Leistung jedes dieser Läufe während des fortschreitenden Trainings visualisieren. Beispiel:

![Hyperparameter-Optimierungsplot](media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Schließlich können Sie die Korrelation zwischen der Leistung und den Werten einzelner Hyperparameter mithilfe von parallelen Koordinaten visuell erkennen. Beispiel: 

![Parallele Koordinaten einer Hyperparameteroptimierung](media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)

Alternativ können Sie alle Ihre Optimierungsläufe von Hyperparametern auch im Azure-Webportal visualisieren. Weitere Informationen zum Anzeigen von Experimenten im Webportal finden Sie über diesen [Link](/how-to-track-experiments.md/#view-the-experiment-in-the-web-portal). Beispiel:

![Hyperparameter-Optimierungsportal](media/how-to-tune-hyperparameters/HyperparameterTuningPortal.png)

## <a name="find-the-configuration-that-resulted-in-the-best-performance"></a>Finden der Konfiguration, die zur besten Leistung geführt hat
Nach Abschluss aller Optimierungsläufe für Hyperparameter können Sie die Konfiguration mit der besten Leistung und die korrespondierenden Werte der Hyperparameter mithilfe des folgenden Codeausschnitts identifizieren:

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Beispielnotebook
Unter 
* `training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb` finden Sie ein Tutorial zum Optimieren von Hyperparametern für ein TensorFlow-Modell. 

Notebook abrufen:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Nachverfolgen eines Experiments](how-to-track-experiments.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)

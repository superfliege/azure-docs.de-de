---
title: Erstellen automatisierter ML-Experimente
titleSuffix: Azure Machine Learning service
description: Das automatisierte maschinelle Lernen wählt einen Algorithmus für Sie aus und generiert ein Modell, das bereitgestellt werden kann. Lernen Sie die Optionen kennen, mit denen Sie Experimente mit automatisiertem maschinellem Lernen konfigurieren können.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 04/08/2019
ms.custom: seodec18
ms.openlocfilehash: 33d8e18dcec98710443623c03651aa568aa37009
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60010380"
---
# <a name="configure-automated-machine-learning-experiments"></a>Konfigurieren von automatisierten Machine Learning-Experimenten

Das automatisierte Machine Learning wählt einen Algorithmus und Hyperparameter für Sie aus und generiert ein Modell, das bereitgestellt werden kann. Es gibt verschiedene Optionen, mit denen Sie automatisierte Machine Learning-Experimente konfigurieren können. In diesem Leitfaden erfahren Sie, wie Sie verschiedene Konfigurationseinstellungen definieren.

Beispiele automatisierter Machine Learning-Experimente finden Sie in den Tutorials zum [ Trainieren eines Klassifizierungsmodells mit automatisiertem maschinellem Lernen](tutorial-auto-train-models.md) und zum [Trainieren von Modellen mit automatisiertem maschinellem Lernen in der Cloud](how-to-auto-train-remote.md).

Für das automatisierte maschinelle Lernen sind folgende Konfigurationsoptionen verfügbar:

* Wählen Sie die Experimentart aus: Klassifizierung, Regression oder Zeitreihenvorhersage
* Datenquelle, Datenformate und Abrufen von Daten
* Wählen Sie das Computeziel aus: lokal oder remote.
* Einstellungen für automatisierte Machine Learning-Experimente
* Ausführen eines automatisierten Machine Learning-Experiments
* Untersuchen von Modellmetriken
* Registrieren und Bereitstellen von Modellen

## <a name="select-your-experiment-type"></a>Auswählen der Experimentart
Legen Sie vor Experimentbeginn fest, welche Art von Problem des maschinellen Lernens Sie lösen möchten. Das automatisierte Machine Learning unterstützt die Aufgabentypen Klassifizierung, Regression und Vorhersage.

Das automatisierte Machine Learning unterstützt während des Automatisierungs- und Optimierungsprozesses die folgenden Algorithmen. Als Benutzer müssen Sie den Algorithmus nicht angeben. Obwohl während des Trainings DNN-Algorithmen zur Verfügung stehen, erstellt automatisiertes ML keine DNN-Modelle.

Classification | Regression | Zeitreihe und Vorhersage
|-- |-- |--
[Logistische Regression](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Elastisches Netz](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Elastisches Netz](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Gradient Boosting](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Gradient Boosting](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Gradient Boosting](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Entscheidungsstruktur](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Entscheidungsstruktur](https://scikit-learn.org/stable/modules/tree.html#regression)|[Entscheidungsstruktur](https://scikit-learn.org/stable/modules/tree.html#regression)
[K nächste Nachbarn](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K nächste Nachbarn](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K nächste Nachbarn](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Lineare SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[C-Support-Vektor-Klassifizierung (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Stochastisches Gradientenabstiegsverfahren (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Stochastisches Gradientenabstiegsverfahren (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Extremely Randomized Trees](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Extremely Randomized Trees](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Extremely Randomized Trees](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN-Klassifizierer](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN-Regressor](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN-Regressor](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[Linearer DNN-Klassifizierer](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Linearer Regressor](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Linearer Regressor](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|
[Stochastisches Gradientenabstiegsverfahren (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|


## <a name="data-source-and-format"></a>Datenquelle und Datenformat
Das automatisierte Machine Learning unterstützt Daten, die sich auf dem lokalen Desktop oder in der Cloud (z. B. in Azure Blob Storage) befinden. Von scikit-learn unterstützte Datenformate können gelesen werden. Sie können die Daten in Folgendes einlesen:
* Numpy-Arrays X (Funktionen) und y (Zielvariable, auch als Bezeichnung bekannt)
* Pandas-Datenrahmen

Beispiele:

*   NumPy-Arrays

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data
    y_digits = digits.target
    ```

*   Pandas-Datenrahmen

    ```python
    import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Abrufen von Daten zum Ausführen von Experimenten auf einem Remotecomputeziel

Wenn Sie Ihr Experiment mit einem Remotecomputeziel ausführen, muss der Datenabruf in ein separates Python-Skript `get_data()` eingebunden werden. Dieses Skript wird auf dem gleichen Remotecomputeziel ausgeführt wie das automatisierte Machine Learning-Experiment. Mit `get_data` müssen die Daten nicht bei jeder Iteration über die Leitung abgerufen werden. Ohne `get_data` schlägt das Experiment fehl, wenn Sie es auf einem Remotecomputeziel ausführen.

Hier ist ein Beispiel für `get_data`:

```python
%%writefile $project_folder/get_data.py
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
def get_data(): # Burning man 2016 data
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    le = LabelEncoder()
    le.fit(df["Label"].values)
    y = le.transform(df["Label"].values)
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    return { "X" : df, "y" : y }
```

Geben Sie in Ihrem `AutoMLConfig`-Objekt den Parameter `data_script` und den Pfad zur `get_data`-Skriptdatei folgendermaßen an:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "/get_data.py", **** )
```

Das `get_data`-Skript kann Folgendes zurückgeben:

Schlüssel | Type | Gegenseitiger Ausschluss mit    | BESCHREIBUNG
---|---|---|---
X | Pandas-Datenrahmen oder NumPy-Array | data_train, label, columns |  Alle Funktionen zum Trainieren.
y | Pandas-Datenrahmen oder NumPy-Array |   label   | Labeldaten zum Trainieren. Bei der Klassifizierung muss es ein Array von ganzen Zahlen sein.
X_valid | Pandas-Datenrahmen oder NumPy-Array   | data_train, label | _Optional:_ Alle Funktionen zum Überprüfen. Wenn nicht angegeben, wird „X“ zum Trainieren und Überprüfen aufgeteilt.
y_valid |   Pandas-Datenrahmen oder NumPy-Array | data_train, label | _Optional:_ Labeldaten zum Überprüfen. Wenn nicht angegeben, wird „Y“ zum Trainieren und Überprüfen aufgeteilt.
sample_weight | Pandas-Datenrahmen oder NumPy-Array |   data_train, label, columns| _Optional:_ Gewichtungswert für jede Stichprobe. Verwenden Sie ihn, wenn Sie den Datenpunkten unterschiedliche Bedeutung beimessen möchten.
sample_weight_valid | Pandas-Datenrahmen oder NumPy-Array | data_train, label, columns |    _Optional:_ Gewichtungswert für jede Überprüfungsstichprobe. Wenn nicht angegeben, wird „sample_weight“ zum Trainieren und Überprüfen aufgeteilt.
data_train |    Pandas-Datenrahmen |  X, y, X_valid, y_valid |    Alle Daten (Funktionen und Label) zum Trainieren.
label | Zeichenfolge  | X, y, X_valid, y_valid |  Spalte in „data_train“, die das Label darstellt.
columns | Array von Zeichenfolgen  ||  _Optional:_ Whitelist von Spalten, die für Funktionen verwendet werden.
cv_splits_indices   | Array mit ganzen Zahlen ||  _Optional:_ Liste von Indizes zum Aufteilen der Daten für die Kreuzvalidierung.

### <a name="load-and-prepare-data-using-dataprep-sdk"></a>Laden und Vorbereiten von Daten mit dem DataPrep SDK
Automatisierte Machine Learning-Experimente unterstützen das Laden und Transformieren von Daten mithilfe des DataPrep SDK. Das SDK bietet Ihnen verschiedene Möglichkeiten:

>* Laden von Daten aus einer Vielzahl von Dateitypen mit Analyseparameter-Rückschluss (Codierung, Trennzeichen, Header)
>* Typkonvertierung unter Verwendung der Ermittlung beim Laden der Datei
>* Verbindungsunterstützung für MS SQL Server und Azure Data Lake Storage
>* Hinzufügen einer Spalte mithilfe eines Ausdrucks
>* Imputieren von fehlenden Werten
>* Ableiten von Spalten anhand eines Beispiels
>* Filterung
>* Benutzerdefinierte Python-Transformationen

Informationen über das DataPrep SDK finden Sie im Artikel [How to prepare data for modeling (Vorbereiten von Daten für die Modellierung)](how-to-load-data.md).
Nachstehend finden Sie ein Beispiel für das Laden von Daten mit dem DataPrep SDK.
```python
# The data referenced here was pulled from `sklearn.datasets.load_digits()`.
simple_example_data_root = 'https://dprepdata.blob.core.windows.net/automl-notebook-data/'
X = dprep.auto_read_file(simple_example_data_root + 'X.csv').skip(1)  # Remove the header row.
# You can use `auto_read_file` which intelligently figures out delimiters and datatypes of a file.

# Here we read a comma delimited file and convert all columns to integers.
y = dprep.read_csv(simple_example_data_root + 'y.csv').to_long(dprep.ColumnSelector(term='.*', use_regex = True))
```

## <a name="train-and-validation-data"></a>Trainings- und Überprüfungsdaten

Sie können ein separates Trainings- und Validierungsset entweder über „get_data()“ oder direkt in der `AutoMLConfig`-Methode angeben.

## <a name="cross-validation-split-options"></a>Kreuzvalidierungsoptionen

### <a name="k-folds-cross-validation"></a>K-fache Kreuzvalidierung

Verwenden Sie die Einstellung `n_cross_validations`, um die Anzahl von Kreuzvalidierungen festzulegen. Das Trainingsdataset wird nach dem Zufallsprinzip in gleich große `n_cross_validations`-Teilmengen aufgeteilt. Bei jedem Kreuzvalidierungsdurchlauf wird eine Teilmenge zum Überprüfen des Modells verwendet – der Rest dient zu Trainingszwecken. Dieser Vorgang wird für die `n_cross_validations`-Durchläufe wiederholt, bis jede Teilmenge einmal als Validierungsset gedient hat. Die Durchschnittswerte für alle `n_cross_validations`-Durchläufe werden ermittelt, und das entsprechende Modell wird anhand des gesamten Trainingsdatasets erneut trainiert. 

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Monte-Carlo-Kreuzvalidierung (wiederholte zufällige Teilstichproben)

Verwenden Sie `validation_size`, um den Prozentsatz des Trainingsdatasets anzugeben, der zum Überprüfen verwendet werden soll, und geben Sie mit `n_cross_validations` die Anzahl von Kreuzvalidierungen an. Bei jedem Kreuzvalidierungs-Durchlauf wird eine Teilmenge der Größe `validation_size` zufällig ausgewählt, um das Modell zu testen. Die restlichen Daten dienen zu Trainingszwecken. Schließlich werden die Durchschnittswerte für alle `n_cross_validations`-Durchläufe ermittelt, und das entsprechende Modell wird anhand des gesamten Trainingsdatasets erneut trainiert. Monte Carlo wird für die Vorhersagen von Zeitreihen nicht unterstützt.

### <a name="custom-validation-dataset"></a>Benutzerdefiniertes Validierungsdataset

Verwenden Sie benutzerdefinierte Validierungsdatasets, wenn eine zufällige Aufteilung nicht akzeptabel ist (i.d.R. bei Zeitreihendaten oder unausgeglichenen Daten). Sie können einen eigenen Validierungsdataset angeben. Das Modell wird anhand des angegebenen Validierungsdatasets bewertet – anstelle des zufälligen Datasets.

## <a name="compute-to-run-experiment"></a>Computeziel zum Ausführen des Experiments

Legen Sie als Nächstes die Instanz fest, auf der das Modell trainiert werden soll. Ein automatisiertes Machine Learning-Trainingsexperiment kann unter folgenden Computeoptionen ausgeführt werden:
*   Lokaler Computer (z.B. lokaler Desktop oder Laptop): Diese Option wird i.d.R. für kleine Datasets und während der Untersuchungsphase verwendet.
*   Ein Remotecomputer in der Cloud: [Azure Machine Learning Managed Compute](concept-azure-machine-learning-architecture.md#managed-and-unmanaged-compute-targets) ist ein verwalteter Dienst, mit dem Machine Learning-Modelle in Clustern virtueller Azure-Computer trainiert werden können.

Beispiel-Notebooks mit lokalen und Remotecomputezielen enthält die [GitHub-Website](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning).

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Konfigurieren der Experimenteinstellungen

Es gibt verschiedene Optionen für das Konfigurieren Ihrer automatisierten Machine Learning-Experimente. Diese Parameter werden beim Instanziieren eines `AutoMLConfig`-Objekts festgelegt. Unter [AutoMLConfig-Klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py) finden Sie eine vollständige Liste der Parameter.  

Beispiele hierfür sind:

1.  Klassifizierungsexperimente mit dem primären Metrikwert „AUC_weighted“ und einer maximalen Zeit von 12.000 Sekunden pro Iteration, die nach 50 Iterationen und 2 Kreuzvalidierungen enden.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        blacklist_models='XGBoostClassifier',
        X=X,
        y=y,
        n_cross_validations=2)
    ```
2.  Hier sehen Sie ein Beispiel für ein Regressionsexperiment, das nach 100 Iterationen endet, wobei jede Iteration bis zu 600 Sekunden dauert und 5 Kreuzvalidierungen auftreten.

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        whitelist_models='kNN regressor'
        primary_metric='r2_score',
        X=X,
        y=y,
        n_cross_validations=5)
    ```

Die drei verschiedenen Werte des `task`-Parameters bestimmen die Liste der anzuwendenden Algorithmen.  Verwenden Sie die Parameter `whitelist` oder `blacklist`, um Iterationen mit den verfügbaren Algorithmen zum Ein- oder Ausschließen weiter zu modifizieren. Die Liste der unterstützten Modelle finden Sie unter der [SupportedAlgorithms-Klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.constants.supportedalgorithms?view=azure-ml-py).

## <a name="primary-metric"></a>Primäre Metrik
Die primäre Metrik – bestimmt wie in den obigen Beispielen gezeigt, die Metrik, die während des Modelltrainings für die Optimierung verwendet werden soll. Die primäre Metrik, die Sie auswählen können, richtet sich nach der Art der ausgewählten Aufgabe. Im Folgenden finden Sie eine Liste der verfügbaren Metriken.

|Classification | Regression | Zeitreihe und Vorhersage
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

## <a name="data-pre-processing-and-featurization"></a>Datenvorverarbeitung und Featurebereitstellung

Bei Verwendung von `preprocess=True` werden die folgenden Datenvorverarbeitungsschritte automatisch für Sie ausgeführt:
1.  Löschen von Funktionen mit hoher Kardinalität oder ohne Varianz
    * Löschen von Funktionen ohne nützliche Informationen aus den Trainings- und Validierungssets. Dazu gehören Funktionen, denen alle Werte fehlen, die denselben Wert für alle Zeilen haben oder die eine sehr hohe Kardinalität (z. B. Hashes, IDs oder GUIDs) aufweisen.
1.  Imputation fehlender Werte
    *   Bei numerischen Features werden fehlende Werte mit dem Durchschnitt der Werte in der Spalte imputiert.
    *   Bei kategorischen Features werden fehlende Werte mit dem am häufigsten vorkommenden Wert imputiert.
1.  Generieren zusätzlicher Funktionen
    * Für DateTime-Funktionen: Jahr, Monat, Tag, Tag der Woche, Tag des Jahres, Quartal, Woche des Jahres, Stunde, Minute, Sekunde.
    * Für Text-Funktionen: Ausdruckshäufigkeit basierend auf Wort-Unigrammen, -Bigrammen und -Trigrammen, Zähler, Vektorisierer.
1.  Transformationen und Codierungen
    * Numerische Features mit sehr wenigen eindeutigen Werten, die in kategorische Features umgewandelt werden.
    * Führen Sie abhängig von der Kardinalität kategorischer Features eine Bezeichnungscodierung oder (Hashing-)One-Hot-Codierung durch.

## <a name="ensemble-models"></a>Ensemblemodelle
Das Lernen mit Ensembles verbessert die Ergebnisse des maschinellen Lernens und die Vorhersageleistung, da nicht einzelne Modelle verwendet, sondern zahlreiche Modelle kombiniert werden. Bei Verwendung des automatisierten maschinellen Lernens können Sie Ensemblemodelle mithilfe des [Caruana-Algorithmus für die Ensembleauswahl mit sortierter Ensembleinitialisierung](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) trainieren. Die Ensembleiteration erscheint als letzte Iteration Ihrer Ausführung.

## <a name="run-experiment"></a>Ausführen des Experiments

Übermitteln Sie das auszuführende Experiment, um ein Modell zu generieren. Übergeben Sie die Methode `AutoMLConfig` an die `submit`-Methode, um das Modell zu generieren.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Abhängigkeiten werden zunächst auf einem neuen Computer installiert.  Es dauert bis zu 10 Minuten, bevor die Ausgabe angezeigt wird.
>Wenn Sie `show_output` auf `True` festlegen, wird die Ausgabe auf der Konsole angezeigt.

## <a name="exit-criteria"></a>Beendigungskriterium 
Es gibt einige Optionen, die Sie definieren können, um Ihr Experiment abzuschließen.
1. Keine Kriterien: Wenn Sie keine Beendigungsparameter definieren, wird das Experiment fortgesetzt, bis kein weiterer Fortschritt bei Ihrer primären Metrik erzielt wird. 
1. Anzahl der Iterationen: Sie definieren die Anzahl der Iterationen, die das Experiment ausführen soll. Sie können optional „Iteration_timeout_minutes“ hinzufügen, um ein Zeitlimit in Minuten pro Iteration zu definieren.
1. Nach gewisser Zeit beenden: Mithilfe von „experiment_timeout_minutes“ in Ihren Einstellungen können Sie festlegen, wie lange ein Experiment fortgesetzt werden soll (in Minuten).
1. Nach Erreichen eines Ergebnisses beenden: Mit „experiment_exit_score“ können Sie das Experiment abschließen, nachdem ein Ergebnis basierend auf Ihrer primären Metrik erreicht wurde.

## <a name="explore-model-metrics"></a>Untersuchen von Modellmetriken
Sie können die Ergebnisse in einem Widget oder in der Inlineansicht anzeigen, wenn Sie ein Notebook verwenden. Weitere Details finden Sie unter [Verfolgen und Auswerten von Modellen](how-to-track-experiments.md#view-run-details).


### <a name="classification-metrics"></a>Klassifizierungsmetrik
In jeder Iteration wird die folgende Metrik für eine Klassifizierungsaufgabe gespeichert:

|Metrik|BESCHREIBUNG|Berechnung|Zusätzliche Parameter
--|--|--|--|
AUC_Macro| AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Macro ist das arithmetische Mittel der AUC für jede Klasse.  | [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="macro"|
AUC_Micro| AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Micro wird global durch die Kombination der echt positiven und der falsch positiven Ergebnisse aus jeder Klasse berechnet.| [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="micro"|
AUC_Weighted  | AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Der gewichtete Wert ist das arithmetische Mittel des Ergebnisses für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.| [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|average="weighted"
accuracy|Die Genauigkeit ist der Prozentsatz der prognostizierten Bezeichnungen, die den TRUE-Bezeichnungen genau entsprechen. |[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Keine|
average_precision_score_macro|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. „Macro“ ist das arithmetische Mittel des durchschnittlichen Genauigkeitswerts jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="macro"|
average_precision_score_micro|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. „Micro“ wird global durch Kombinieren der echt positiven und der falsch positiven Ergebnisse bei jedem Cutoff berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="micro"|
average_precision_score_weighted|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. Der gewichtete Wert ist das arithmetische Mittel der durchschnittlichen Genauigkeit für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="weighted"|
balanced_accuracy|„Balanced accuracy“ ist das arithmetische Mittel des Recalls für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
f1_score_macro|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. „Macro“ ist das arithmetische Mittel des F1-Ergebnisses für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="macro"|
f1_score_micro|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. „Micro“ wird global durch Zählen der insgesamt echt positiven, falsch negativen und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="micro"|
f1_score_weighted|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. Gewichteter Mittelwert nach Klassenhäufigkeit des F1-Ergebnisses für jede Klasse|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="weighted"|
log_loss|Dies ist die Verlustfunktion, die bei der (multinomialen) logistischen Regression und deren Erweiterungen wie z. B. neuronalen Netzen verwendet wird. Sie ist definiert als die Negativ-Log-Wahrscheinlichkeit der True-Bezeichnungen bei den Vorhersagen eines probabilistischen Klassifizierers. Für eine einzelne Stichprobe mit der true-Bezeichnung yt in {0,1} und der geschätzten Wahrscheinlichkeit yp, dass yt = 1, beträgt der logarithmische Verlust -log P(yt&#124;yp) = -(yt log(yp) + (1 - yt) log(1 - yp)).|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Keine|
norm_macro_recall|Der normalisierte Makro-Recall wird normalisiert, damit die zufällige Leistung ein Ergebnis von 0 und die ideale Leistung einen Wert von 1 liefert. Dies kann erzielt werden durch norm_macro_recall := (recall_score_macro - R)/(1 - R), wobei R der erwartete Wert von recall_score_macro für zufällige Vorhersagen ist (d. h. R=0,5 für die binäre Klassifizierung und R=(1/C) für C-Klassen-Klassifizierungsprobleme)|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average = "macro" und danach (recall_score_macro - R)/(1 - R), wobei R der erwartete Wert von recall_score_macro für zufällige Vorhersagen ist (d. h., R=0,5 für die binäre Klassifizierung und R=(1/C) für C-Klassen-Klassifizierungsprobleme)|
precision_score_macro|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. „Macro“ ist das arithmetische Mittel der Genauigkeit für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="macro"|
precision_score_micro|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. „Micro“ wird global durch Zählen der insgesamt echt positiven und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="micro"|
precision_score_weighted|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. Der gewichtete Wert ist das arithmetische Mittel der Genauigkeit für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="weighted"|
recall_score_macro|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. Macro ist das arithmetische Mittel des Recalls für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
recall_score_micro|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. „Micro“ wird global durch Zählen der insgesamt echt positiven und falsch negativen Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="micro"|
recall_score_weighted|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. Der gewichtete Wert ist das arithmetische Mittel des Recalls für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="weighted"|
weighted_accuracy|Gewichtete Genauigkeit ist die Genauigkeit, bei der die Gewichtung, die jedem Beispiel zugewiesen wird, gleich dem Anteil der TRUE-Instanzen in der TRUE-Klasse dieses Beispiels ist.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight ist ein Vektor gleich dem Anteil der betreffenden Klasse für jedes Element im Ziel.|

### <a name="regression-and-time-series-forecasting-metrics"></a>Regressions- und Zeitreihenvorhersagemetrik
In jeder Iteration wird die folgende Metrik für eine Regressions- oder Vorhersageaufgabe gespeichert:

|Metrik|BESCHREIBUNG|Berechnung|Zusätzliche Parameter
--|--|--|--|
explained_variance|Die erläuterte Varianz ist der Anteil, mit dem ein mathematisches Modell für die Variation eines bestimmten Datasets verantwortlich ist. Hierbei handelt es sich um den prozentualen Rückgang der Varianz der ursprünglichen Daten im Vergleich zur Varianz der Fehler. Wenn der Mittelwert der Fehler 0 beträgt, entspricht er der erläuterten Varianz.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Keine|
r2_score|R2 ist der Ermittlungskoeffizient bzw. die prozentuale Reduzierung der quadratischen Fehler im Vergleich zu einem Baseline-Modell, das den Mittelwert ausgibt. Wenn der Mittelwert der Fehler 0 beträgt, entspricht er der erläuterten Varianz.|[Berechnung](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Keine|
spearman_correlation|Die Spearman-Korrelation ist ein nicht parametrisches Maß für die Monotonie der Beziehung zwischen zwei Datasets. Im Gegensatz zur Pearson-Korrelation geht die Spearman-Korrelation nicht davon aus, dass beide Datasets normal verteilt sind. Wie bei anderen Korrelationskoeffizienten variiert dieser Wert zwischen -1 und +1, wobei 0 für keine Korrelation steht. Korrelationen von -1 oder +1 implizieren eine exakt monotone Beziehung. Positive Korrelationen implizieren, dass sich x ebenso wie y erhöht. Negative Korrelationen implizieren, dass sich x erhöht und y niedriger wird.|[Berechnung](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Keine|
mean_absolute_error|Der mittlere absolute Fehler ist der erwartete Wert des absoluten Differenzwerts zwischen dem Ziel und der Vorhersage.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Keine|
normalized_mean_absolute_error|Der normalisierte mittlere absolute Fehler ist der mittlere absolute Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Division durch den Datenbereich|
median_absolute_error|Der mittlere absolute Fehler ist der Median aller absoluten Differenzen zwischen dem Ziel und der Vorhersage. Dieser Verlust ist stabil in Bezug auf Ausreißer.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Keine|
normalized_median_absolute_error|Der normalisierte mittlere absolute Fehler ist der mittlere absolute Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Division durch den Datenbereich|
root_mean_squared_error|Die Wurzel aus dem mittleren quadratischen Fehler ist die Quadratwurzel der erwarteten quadratischen Differenz zwischen dem Ziel und der Vorhersage.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Keine|
normalized_root_mean_squared_error|Die normalisierte Wurzel aus dem mittleren quadratischen Fehler ist die Wurzel aus dem mittleren quadratischen Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Division durch den Datenbereich|
root_mean_squared_log_error|Die Wurzel aus dem mittleren quadratischen logarithmischen Fehler ist die Quadratwurzel des erwarteten quadratischen logarithmischen Fehlers.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Keine|
normalized_root_mean_squared_log_error|Die normalisierte Wurzel aus dem mittleren quadratischen logarithmischen Fehler ist die Wurzel aus dem mittleren quadratischen logarithmischen Fehler dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Division durch den Datenbereich|

## <a name="explain-the-model"></a>Erläutern des Modells

Während die Funktionen für das automatisierte Machine Learning bereits allgemein verfügbar sind, befindet sich **die Funktion zur Erläuterbarkeit des Modells noch in der Phase einer öffentlichen Vorschau.**

Mit dem automatisierten Machine Learning können Sie die Featurewichtigkeit verstehen.  Während des Trainingsprozesses können Sie die globale Featurewichtigkeit für das Modell abrufen.  Für Klassifizierungsszenarios können Sie auch die Featurewichtigkeit auf Klassenebene abrufen.  Sie müssen ein Validierungsdataset (X_valid) bereitstellen, um die Featurewichtigkeit abzurufen.

Es gibt zwei Möglichkeiten, eine Featurewichtigkeit zu generieren.

*   Nachdem ein Experiment abgeschlossen ist, können Sie die `explain_model`-Methode für eine beliebige Iteration verwenden.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        explain_model(fitted_model, X_train, X_test)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

*   Um die Featurewichtigkeit für alle Iterationen anzuzeigen, legen Sie das Flag `model_explainability` in AutoMLConfig auf `True` fest.

    ```python
    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 primary_metric = 'AUC_weighted',
                                 max_time_sec = 12000,
                                 iterations = 10,
                                 verbosity = logging.INFO,
                                 X = X_train,
                                 y = y_train,
                                 X_valid = X_test,
                                 y_valid = y_test,
                                 model_explainability=True,
                                 path=project_folder)
    ```

    Danach können Sie die retrieve_model_explanation-Methode verwenden, um die Featurewichtigkeit für eine bestimmte Iteration abzurufen.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        retrieve_model_explanation(best_run)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

Sie können das Diagramm für die Featurewichtigkeit in Ihrem Arbeitsbereich im Azure-Portal anzeigen. Das Diagramm wird auch angezeigt, wenn Sie das Jupyter-Widget in einem Notebook verwenden. Weitere Informationen zu den Diagrammen finden Sie in dem Artikel [Azure Machine Learning Service-Beispiel-Notebooks](samples-notebooks.md).

```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```
![Featurewichtigkeitsdiagramm](./media/how-to-configure-auto-train/feature-importance.png)

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie, [wie und wo Sie Modelle bereitstellen](how-to-deploy-and-where.md) können.

Erfahren Sie mehr über das [Trainieren eines Regressionsmodells mit automatisiertem maschinellem Lernen](tutorial-auto-train-models.md) und das [Trainieren mit automatisiertem maschinellem Lernen auf einer Remoteressource](how-to-auto-train-remote.md).

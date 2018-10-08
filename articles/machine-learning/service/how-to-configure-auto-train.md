---
title: Konfigurieren von Experimenten mit automatisiertem maschinellem Lernen – Azure Machine Learning
description: Das automatisierte maschinelle Lernen wählt einen Algorithmus für Sie aus und generiert ein Modell, das bereitgestellt werden kann. Lernen Sie die Optionen kennen, mit denen Sie Experimente mit automatisiertem maschinellem Lernen konfigurieren können.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: cbd475ae4ce944db3ebf57b415b60e7abdd52677
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163850"
---
# <a name="configure-your-automated-machine-learning-experiment"></a>Konfigurieren von Experimenten mit automatisiertem maschinellem Lernen

Das automatisierte maschinelle Lernen wählt einen Algorithmus und Hyperparameter für Sie aus und generiert ein Modell, das bereitgestellt werden kann. Sie können das Modell herunterladen, um es weiter anzupassen. Es gibt mehrere Optionen, mit denen Sie Experimente mit automatisiertem maschinellem Lernen konfigurieren können. In diesem Leitfaden erfahren Sie, wie Sie verschiedene Konfigurationseinstellungen definieren.

Beispiele finden Sie in den Tutorials zum [Trainieren eines Klassifizierungsmodells mit automatisiertem maschinellem Lernen](tutorial-auto-train-models.md) und zum [Trainieren von Modellen mit automatisiertem maschinellem Lernen in der Cloud](how-to-auto-train-remote.md).

Für das automatisierte maschinelle Lernen sind folgende Konfigurationsoptionen verfügbar:

* Auswählen der Experimentart, z.B. Klassifizierung, Regression 
* Datenquelle, Datenformate und Abrufen von Daten
* Auswählen des Computeziels (lokal oder remote)
* Einstellungen für das Experiment mit automatisiertem maschinellem Lernen
* Ausführen eines Experiments mit automatisiertem maschinellem Lernen
* Untersuchen von Modellmetriken
* Registrieren und Bereitstellen von Modellen

## <a name="select-your-experiment-type"></a>Auswählen der Experimentart
Legen Sie vor Experimentbeginn fest, welche Art von Problem des maschinellen Lernens Sie lösen möchten. Das automatisierte maschinelle Lernen unterstützt zwei Kategorien von überwachtem Lernen: Klassifizierung und Regression. Das automatisierte maschinelle Lernen unterstützt die folgenden Algorithmen beim Automatisierungs- und Optimierungsprozess. Als Benutzer müssen Sie den Algorithmus nicht angeben.
Klassifizierung | Regression
--|--
sklearn.linear_model.LogisticRegression | sklearn.linear_model.ElasticNet
sklearn.linear_model.SGDClassifier  | sklearn.ensemble.GradientBoostingRegressor
sklearn.naive_bayes.BernoulliNB | sklearn.tree.DecisionTreeRegressor
sklearn.naive_bayes.MultinomialNB | sklearn.neighbors.KNeighborsRegressor
sklearn.svm.SVC | sklearn.linear_model.LassoLars
sklearn.svm.LinearSVC | sklearn.linear_model.SGDRegressor
sklearn.calibration.CalibratedClassifierCV |    sklearn.ensemble.RandomForestRegressor
sklearn.neighbors.KNeighborsClassifier |    sklearn.ensemble.ExtraTreesRegressor
sklearn.tree.DecisionTreeClassifier |   lightgbm.LGBMRegressor
sklearn.ensemble.RandomForestClassifier |
sklearn.ensemble.ExtraTreesClassifier   |
sklearn.ensemble.GradientBoostingClassifier |
lightgbm.LGBMClassifier |


## <a name="data-source-and-format"></a>Datenquelle und Datenformat
Das automatisierte maschinelle Lernen unterstützt Daten, die sich auf dem lokalen Desktop oder in der Cloud in Azure Blob Storage befinden. Von scikit-learn unterstützte Datenformate können gelesen werden. Sie können die Daten aus 1) NumPy-Arrays X (Funktionen) und Y (Zielvariable, auch „Label“ genannt) oder 2) Pandas-Datenrahmen lesen. 

Beispiele:

1.  NumPy-Arrays

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data 
    y_digits = digits.target
    ```

2.  Pandas-Datenrahmen

    ```python
    Import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Abrufen von Daten zum Ausführen von Experimenten auf einem Remotecomputeziel

Wenn Sie Ihr Experiment mit einem Remotecomputeziel ausführen, muss der Datenabruf in ein separates Python-Skript `get_data()` eingebunden werden. Dieses Skript wird auf dem gleichen Remotecomputeziel ausgeführt wie das Experiment mit automatisiertem maschinellem Lernen. Mit `get_data` müssen die Daten nicht bei jeder Iteration über die Leitung abgerufen werden. Ohne `get_data` schlägt das Experiment fehl, wenn Sie es auf einem Remotecomputeziel ausführen.

Hier ist ein Beispiel für `get_data`:

```python
%%writefile $project_folder/get_data.py 
import pandas as pd 
from sklearn.model_selection 
import train_test_split 
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
Schlüssel | Typ |    Gegenseitiger Ausschluss mit | BESCHREIBUNG
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

## <a name="train-and-validation-data"></a>Trainings- und Überprüfungsdaten

Sie können ein separates Trainings- und Validierungsset entweder über „get_data()“ oder direkt in der `AutoMLConfig`-Methode angeben.

## <a name="cross-validation-split-options"></a>Kreuzvalidierungsoptionen

### <a name="k-folds-cross-validation"></a>K-fache Kreuzvalidierung

Verwenden Sie die Einstellung `n_cross_validations`, um die Anzahl von Kreuzvalidierungen festzulegen. Das Trainingsdataset wird nach dem Zufallsprinzip in gleich große `n_cross_validations`-Teilmengen aufgeteilt. Bei jedem Kreuzvalidierungsdurchlauf wird eine Teilmenge zum Überprüfen des Modells verwendet – der Rest dient zu Trainingszwecken. Dieser Vorgang wird für die `n_cross_validations`-Durchläufe wiederholt, bis jede Teilmenge einmal als Validierungsset gedient hat. Schließlich werden die Durchschnittswerte für alle `n_cross_validations`-Durchläufe ermittelt, und das entsprechende Modell wird anhand des gesamten Trainingsdatasets erneut trainiert.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Monte-Carlo-Kreuzvalidierung (wiederholte zufällige Teilstichproben)

Verwenden Sie `validation_size`, um den Prozentsatz des Trainingsdatasets anzugeben, der zum Überprüfen verwendet werden soll, und geben Sie mit `n_cross_validations` die Anzahl von Kreuzvalidierungen an. Bei jedem Kreuzvalidierungs-Durchlauf wird eine Teilmenge der Größe `validation_size` zufällig ausgewählt, um das Modell zu testen. Die restlichen Daten dienen zu Trainingszwecken. Schließlich werden die Durchschnittswerte für alle `n_cross_validations`-Durchläufe ermittelt, und das entsprechende Modell wird anhand des gesamten Trainingsdatasets erneut trainiert.

### <a name="custom-validation-dataset"></a>Benutzerdefiniertes Validierungsdataset

Verwenden Sie benutzerdefinierte Validierungsdatasets, wenn eine zufällige Aufteilung nicht akzeptabel ist (i.d.R. bei Zeitreihendaten oder unausgeglichenen Daten). So können Sie einen eigenen Validierungsdataset angeben. Das Modell wird anhand des angegebenen Validierungsdatasets bewertet – anstelle des zufälligen Datasets.

## <a name="compute-to-run-experiment"></a>Computeziel zum Ausführen des Experiments

Legen Sie als Nächstes die Instanz fest, auf der das Modell trainiert werden soll. Ein Trainingsexperiment mit automatisiertem maschinellem Lernen wird auf einem Computeziel ausgeführt, das Sie besitzen und verwalten. 

Folgende Computeoptionen werden unterstützt:
1.  Lokaler Computer (z.B. lokaler Desktop oder Laptop): Diese Option wird i.d.R. für kleine Datasets und während der Untersuchungsphase verwendet.
2.  Remotecomputer in der Cloud – [Azure Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (DSVM) unter Linux: Verwenden Sie diese Option, wenn Sie ein großes Dataset haben und auf einem großen Computer zentral hochskalieren möchten, der in der Azure-Cloud verfügbar ist. 
3.  Azure Batch AI-Cluster: Bei dieser Option lässt sich ein verwaltetes Cluster so einrichten, dass es gleichzeitig horizontal hochskaliert und Iterationen des automatisierten maschinellen Lernens ausführt. 


## <a name="configure-your-experiment-settings"></a>Konfigurieren der Experimenteinstellungen

Es gibt mehrere Optionen, mit denen Sie Ihre Experimente mit automatisiertem maschinellem Lernen konfigurieren können. Diese Parameter werden beim Instanziieren eines `AutoMLConfig`-Objekts festgelegt.

Beispiele hierfür sind:

1.  Klassifizierungsexperimente mit dem primären Metrikwert „AUC_weighted“ und einer maximalen Zeit von 12.000 Sekunden pro Iteration, die nach 50 Iterationen und 2 Kreuzvalidierungen enden.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        X=X, 
        y=y,
        n_cross_validations=2)
    ```
2.  Hier sehen Sie ein Beispiel für ein Regressionsexperiment, das nach 100 Iterationen endet, wobei jede Iteration bis zu 600 Sekunden dauert und 5 Kreuzvalidierungen auftreten.

    ````python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        primary_metric='r2_score',
        X=X, 
        y=y,
        n_cross_validations=5)
    ````

Diese Tabelle führt die für das Experiment verfügbaren Parametereinstellungen und ihre Standardwerte auf.

Eigenschaft |  BESCHREIBUNG | Standardwert
--|--|--
`task`  |Geben Sie das zu lösende Problemen für das maschinelle Lernen an. Zulässige Werte sind <li>Klassifizierung</li><li>Regression</li>    | Keine |
`primary_metric` |Die Metrik, die Sie beim Erstellen Ihres Modells optimieren möchten. Wenn Sie beispielsweise Genauigkeit als „primary_metric“ angeben, sucht das automatisierte maschinelle Lernen nach einem Modell mit maximaler Genauigkeit. Pro Experiment lässt sich nur ein Wert für „primary_metric“ angeben. Zulässige Werte sind <br/>**Klassifizierung:**<br/><li> accuracy  </li><li> AUC_weighted</li><li> precision_score_weighted </li><li> balanced_accuracy </li><li> average_precision_score_weighted </li><br/>**Regression:** <br/><li> normalized_mean_absolute_error </li><li> spearman_correlation </li><li> normalized_root_mean_squared_error </li><li> normalized_root_mean_squared_log_error</li><li> R2_score    </li> | Für Klassifizierung: accuracy <br/>Für Regression: spearman_correlation <br/> |
`exit_score` |  Sie können einen Zielwert für „ primary_metric“ festlegen. Sobald ein Modell gefunden wurde, das dem „primary_metric“-Ziel entspricht, stoppt das automatisierte maschinelle Lernen die Iteration und das Experiment wird beendet. Wenn dieser Wert nicht festgelegt wird (Standardeinstellung), wird das Experiment mit automatisiertem maschinellem Lernen so lange fortgesetzt, bis die angegebene Anzahl von Iterationen erreicht ist. Ein Wert vom Typ „double“ wird verwendet. Wenn das Ziel nie erreicht wird, wird das Experiment so lange fortgesetzt, bis die angegebene Anzahl von Iterationen erreicht ist.|   Keine
`iterations` |Maximale Anzahl von Iterationen. Jede Iteration entspricht einem Trainingsauftrag, dessen Ergebnis eine Pipeline ist. Die Pipeline ist die Datenvorverarbeitung und ein Modell. Legen Sie für ein hochwertiges Modell mindestens 250 Iterationen fest. | 100
`Concurrent_iterations`|    Maximale Anzahl von Iterationen, die parallel ausgeführt werden sollen. Diese Einstellung wird nur für Remotecomputeziele unterstützt.|    1
`max_cores_per_iteration`   | Gibt die Anzahl von Kernen auf dem Computeziel zum Trainieren einer einzelnen Pipeline an. Wenn der Algorithmus mehrere Kerne nutzen kann, erhöht dies die Leistung eines Multicorecomputers. Sie können den Wert auf „–1“ festlegen, um alle auf dem Computer verfügbaren Kerne zu verwenden.|  1
`max_time_sec` |    Begrenzt die Zeit (in Sekunden), die eine bestimmte Iteration benötigt. Wenn eine Iteration den angegebenen Wert überschreitet, wird sie abgebrochen. Wenn dieser Wert nicht festgelegt wird, wird die Iteration weiter ausgeführt, bis sie beendet ist. |   Keine
`n_cross_validations`   |Anzahl von Kreuzvalidierungen| Keine
`validation_size`   |Größe des Validierungssets als Prozentsatz aller Trainingsstichproben.|  Keine
`preprocess` | True/False <br/>TRUE ermöglicht dem Experiment, eine Vorverarbeitung der Eingabe auszuführen. Es folgt eine Teilmenge der Vorverarbeitung.<li>Fehlende Daten: Impliziert die fehlenden Daten – numerisch mit Durchschnitt, Text mit häufigstem Vorkommen. </li><li>Kategoriewerte: Wenn der Datentyp numerisch und die Anzahl von eindeutigen Werten niedriger als 5 Prozent ist, erfolgt eine Konvertierung in den 1-aus-n-Code. </li><li>Weitere Informationen finden Sie im [GitHub-Repository](https://aka.ms/aml-notebooks).</li><br/>Hinweis: Bei einer geringen Datendichte ist eine Vorverarbeitung nicht möglich = TRUE. |  False | 
`blacklist_algos`   | Das Experiment mit automatisiertem maschinellem Lernen verfügt über viele verschiedene Algorithmen, die es ausprobiert. Konfigurieren Sie das Experiment, um bestimmte Algorithmen vom Experiment auszuschließen. Dies ist nützlich, wenn Sie wissen, dass bestimmte Algorithmen für Ihr Dataset nicht geeignet sind. Mit dem Ausschließen von Algorithmen können Sie Computeressourcen und Trainingszeit sparen.<br/>Zulässige Werte für die Klassifizierung sind:<br/><li>logistic regression</li><li>SGD classifier</li><li>MultinomialNB</li><li>BernoulliNB</li><li>SVM</li><li>LinearSVM</li><li>kNN</li><li>DT</li><li>RF</li><li>extra trees</li><li>gradient boosting</li><li>lgbm_classifier</li><br/>Zulässige Werte für die Regression sind:<br/><li>Elastic net</li><li>Gradient boosting regressor</li><li>DT regressor</li><li>kNN regressor</li><li>Lasso lars</li><li>SGD regressor</li><li>RF regressor</li><li>extra trees regressor</li>|   Keine
`verbosity` |Steuert den Protokolliergrad, wobei „INFO“ am ausführlichsten und „CRITICAL“ am wenigsten ausführlich ist.<br/>Zulässige Werte sind:<br/><li>logging.INFO</li><li>logging.WARNING</li><li>logging.ERROR</li><li>logging.CRITICAL</li>  | logging.INFO</li> 
`X` | Alle Funktionen zum Trainieren. |  Keine
`y` |   Labeldaten zum Trainieren. Bei der Klassifizierung muss es ein Array von ganzen Zahlen sein.|  Keine
`X_valid`|_Optional:_ Alle Funktionen zum Überprüfen. Wenn nicht angegeben, wird „X“ zum Trainieren und Überprüfen aufgeteilt. |   Keine
`y_valid`   |_Optional:_ Labeldaten zum Überprüfen. Wenn nicht angegeben, wird „Y“ zum Trainieren und Überprüfen aufgeteilt.    | Keine
`sample_weight` |   _Optional:_ Gewichtungswert für jede Stichprobe. Verwenden Sie ihn, wenn Sie den Datenpunkten unterschiedliche Bedeutung beimessen möchten. |   Keine
`sample_weight_valid`   |   _Optional:_ Gewichtungswert für jede Überprüfungsstichprobe. Wenn nicht angegeben, wird „sample_weight“ zum Trainieren und Überprüfen aufgeteilt.   | Keine
`run_configuration` |   „RunConfiguration“-Objekt.  Dies wird für Remoteausführungen verwendet. |Keine
`data_script`  |    Der Pfad zu einer Datei, die die „get_data“-Methode enthält.  Erforderlich für Remoteausführungen.   |Keine


## <a name="run-experiment"></a>Ausführen des Experiments

Als nächstes können Sie das Experiment initiieren, um ein Modell auszuführen und zu generieren. Übergeben Sie die Methode `AutoMLConfig` an die `submit`-Methode, um das Modell zu generieren.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Abhängigkeiten werden zunächst auf einer neuen DSVM-Instanz installiert.  Es dauert bis zu 10 Minuten, bevor die Ausgabe angezeigt wird.
>Wenn Sie `show_output` auf TRUE festlegen, wird die Ausgabe in der Konsole angezeigt.


## <a name="explore-model-metrics"></a>Untersuchen von Modellmetriken
Sie können die Ergebnisse in einem Widget oder in der Inlineansicht anzeigen, wenn Sie ein Notebook verwenden. In den Details finden Sie weitere Informationen zum Nachverfolgen und Bewerten von Modellen. (Achten Sie darauf, dass die Inhalte des automatisierten maschinellen Lernens relevante Informationen für das automatisierte maschinelle Lernen enthalten.)

Die folgenden Metriken werden in jeder Iteration gespeichert:
* AUC_macro
* AUC_micro
* AUC_weighted
* AUC_weighted_max
* accuracy
* average_precision_score_macro
* average_precision_score_micro
* average_precision_score_weighted
* balanced_accuracy
* f1_score_macro
* f1_score_micro
* f1_score_weighted
* log_loss
* norm_macro_recall
* precision_score_macro
* precision_score_micro
* precision_score_weighted
* recall_score_macro
* recall_score_micro
* recall_score_weighted
* weighted_accuracy

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie, [wie und wo Sie Modelle bereitstellen](how-to-deploy-and-where.md) können.

Erfahren Sie mehr über das[Trainieren eines Klassifizierungsmodells mit automatisiertem maschinellem Lernen](tutorial-auto-train-models.md) und über das [Trainieren mit automatisiertem maschinellem Lernen auf einer Remoteressource](how-to-auto-train-remote.md). 
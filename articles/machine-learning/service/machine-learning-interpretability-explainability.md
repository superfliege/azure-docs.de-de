---
title: Interpretierbarkeit von Modellen
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie mithilfe des Azure Machine Learning Interpretability SDK erklären können, warum Ihr Modell Vorhersagen trifft. Es kann während des Trainings und der Herleitung von Rückschlüssen verwendet werden, um zu verstehen, wie Ihr Modell Vorhersagen trifft.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 03/27/2019
ms.openlocfilehash: 1cd5f48e8e0e74dfa04465993246e5d68840a783
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2019
ms.locfileid: "58920001"
---
# <a name="azure-machine-learning-interpretability-sdk"></a>Azure Machine Learning Interpretability SDK

Erfahren Sie, wie Sie erklären können, warum Ihr Modell zu den Vorhersagen gelangt, die es trifft. Das Azure Machine Learning Interpretability SDK ermöglicht Ihnen, Ihr Modell zu erklären, was aus den folgenden Gründen wichtig ist:

* Kunden und andere Beteiligte möchten wissen, ob sie den **Vorhersagen, die Ihr Modell macht, vertrauen können**.
* Als Data Scientist möchten Sie verstehen, **wie Sie das Modell abfragen können, um Erkenntnisse zu gewinnen**. Sie benötigen auch Tools, um fundierte Entscheidungen darüber zu treffen, **wie Sie Ihr Modell verbessern können**.
* Als Unternehmen müssen Sie **das Verhalten des Modells mit unterschiedlichen Eingabeverteilungen** und **das Verhalten des Modells bei Analyse spezifischer Eingaben** verstehen.

Interpretierbarkeit beim maschinellem Lernen ist in zwei Phasen von des zugehörigen Entwicklungszyklus wichtig: **Training** und **Herleitung von Rückschlüssen**:

* Beim **Training**: Zuständige für den Entwurf und die Auswertung von Modellen benötigen Interpretationstools, um den beteiligten Akteuren die Ergebnisse eines Modells zu erklären und Vertrauen aufzubauen. Interpretationstools ermöglichen auch das Debuggen des Modells:

    * Entspricht sein Verhalten den Zielen und Vorgaben?
    * Ist es verzerrt?

* Beim **Herleiten von Rückschlüssen**: Vorhersagen müssen den Personen, die Ihr Modell verwenden, verständlich sein. Warum hat das Modell beispielsweise ein Hypothekendarlehen abgelehnt oder vorhergesagt, dass ein Anlageportfolio mit einem höheren Risiko behaftet ist?

Das Azure Machine Learning Interpretability SDK enthält von Microsoft entwickelte Technologien und bewährte Bibliotheken von Drittanbietern (z.B. SHAP und LIME). Es bietet eine gemeinsame API für die integrierten Bibliotheken und kann in Azure Machine Learning Services integriert werden. 

Mit diesem SDK können Sie Machine Learning-Modelle **global für alle Daten** oder **lokal für einen bestimmten Datenpunkt** mithilfe modernster Technologien auf einfache und skalierbare Weise erklären.

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Azure Machine Learning Interpretability kann angewendet werden, um das globale Verhalten des Modells oder eine bestimmte Vorhersage zu verstehen. Erstere wird als globale Erklärung und letztere als lokale Erklärung bezeichnet.

Azure Machine Learning Interpretability-Methoden können auch kategorisiert werden, je nachdem, ob die Methode modellunabhängig oder modellspezifisch ist. Einige Methoden zielen auf bestimmte Arten von Modellen ab. Der Tree Explainer von SHAP eignet sich nur für baumstrukturbasierte Modelle. Einige Methoden behandeln das Modell als Blackbox, wie z.B. der Mimic Explainer oder der Kernel Explainer von SHAP. Das Azure Machine Learning Interpretability SDK verfolgt diese verschiedenen Ansätze basierend auf Datasets, Modelltypen und Anwendungsfällen.

Azure Machine Learning Interpretability gibt eine Reihe von Informationen darüber zurück, wie ein Modell seine Vorhersage trifft. Die Informationen umfassen Elemente wie z.B.:

* Global/lokale relative Featurepriorität
* Global/lokale Beziehung zwischen Feature und Vorhersage
* Interaktive Visualisierungen für Folgendes:

    * Vorhersagen
    * Beziehungen zwischen Feature und Vorhersage
    * Globale und lokale relative Featureprioritätswerte

## <a name="architecture"></a>Architecture

Das Azure Machine Learning Interpretability SDK besteht aus zwei Python-Paketen:

* [azureml.explain.model](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py): das Hauptpaket, das die Funktionen enthält, die von Microsoft unterstützt werden.
* `azureml.contrib.explain.model` : Vorschau- und experimentelle Funktionen, die Sie ausprobieren können.

    > [!IMPORTANT]
    > Der Inhalt von „azureml.contrib.explain.model“ wird nicht vollständig unterstützt. Sobald die experimentellen Funktionen ausgereift sind, werden sie schrittweise in das Hauptpaket verlagert.

### <a name="explainers"></a>Explainer

Das Azure Machine Learning Interpretability SDK bietet zwei Gruppen von Explainern: Direkte Explainer und Meta-Explainer.

__Direkte Explainer__ stammen aus integrierten Bibliotheken. Das SDK bündelt alle Explainer so, dass sie eine gemeinsame API und ein gemeinsames Ausgabeformat verfügbar machen. Es folgt eine Liste der direkten Explainer, die im SDK verfügbar sind:

> [!TIP]
> Wenn Sie diese Explainer direkt verwenden möchten, können Sie sie direkt aufrufen, anstatt die gemeinsame API und das gemeinsame Ausgabeformat zu verwenden.

* **Tree Explainer**: Der Tree Explainer von SHAP, der sich auf den schnellen Wertschätzalgorithmus von SHAP für die Polynomialzeit konzentriert, der für Baumstrukturen und Gruppen von Baumstrukturen spezifisch ist.
* **Deep Explainer**: Basierend auf der Erläuterung von SHAP ist Deep Explainer „ein schneller Näherungsalgorithmus für SHAP-Werte in Deep-Learning-Modellen, der auf einer im SHAP NIPS-Papier beschriebenen Verbindung mit DeepLIFT aufbaut. TensorFlow- und Keras-Modelle, die das TensorFlow-Back-End verwenden, werden unterstützt (es gibt auch eine vorläufige Unterstützung für PyTorch)“.
* **Kernel Explainer**: Der Kernel Explainer von SHAP verwendet eine speziell gewichtete lokale lineare Regression, um SHAP-Werte für jedes Modell zu schätzen.
* **Mimic Explainer**: Der Mimic Explainer basiert auf der Idee globaler Surrogatmodelle. Ein globales Surrogatmodell ist ein eigenständig interpretierbares Modell, das trainiert wird, um die Vorhersagen eines Blackbox-Modells so genau wie möglich zu treffen. Der Data Scientist kann das Surrogatmodell interpretieren, um Rückschlüsse in Bezug auf das Blackbox-Modell zu ziehen.
* **LIME Explainer**: Basierend auf LIME verwendet der LIME Explainer den hochmodernen LIME-Algorithmus (Local Interpretable Model-Agnostic Explanations), um lokale Surrogatmodelle zu erstellen. Im Gegensatz zu den globalen Surrogatmodellen konzentriert sich LIME auf das Training lokaler Surrogatmodelle, um individuelle Vorhersagen zu erklären.
* **HAN Text Explainer**: Der HAN Text Explainer verwendet ein Hierarchical Attention Network, um Modellerklärungen aus Textdaten zu einem bestimmten Blackbox-Textmodell zu erhalten. Wir trainieren das HAN-Surrogatmodell anhand der vorhergesagten Ergebnisse eines gegebenen Lehrermodells. Nach einem globalen Training über den gesamten Textkorpus haben wir einen Optimierungsschritt für ein spezifisches Dokument hinzugefügt, um die Genauigkeit der Erklärungen zu verbessern. HAN verwendet ein rekurrentes neuronales Netz (RNN) mit zwei Aufmerksamkeitsebenen hinsichtlich Satz und Wort. Sobald das DNN (Deep Neural Network) am Lehrermodell trainiert und für ein bestimmtes Dokument optimiert wurde, können wir die Wichtigkeiten von Worten aus den Aufmerksamkeitsebenen extrahieren. Wir haben festgestellt, dass HAN bei Textdaten genauer ist als LIME oder SHAP, aber auch aufwendiger in Bezug auf die Trainingszeit. Wir haben jedoch die Trainingszeit verbessert, indem wir dem Benutzer die Möglichkeit gegeben haben, das Netzwerk mit GloVe-Worteinbettungen zu initialisieren, obwohl es immer noch langsam ist. Die Trainingszeit kann durch den Einsatz von HAN auf einer Azure GPU-Remote-VM deutlich verbessert werden. „Hierarchical Attention Networks for Document Classification (Yang u.a., 2016)“ ([https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf)) enthält eine Beschreibung der Implementierung von HAN.

__Meta-Explainer__ wählen automatisch einen geeigneten direkten Explainer aus und generieren die besten Erklärungsinformationen basierend auf dem gegebenen Modell und den gegebenen Datasets. Die Meta-Explainer nutzen alle Bibliotheken (SHAP, LIME, GA2M, Mimic usw.), die wir integriert oder entwickelt haben. Es folgt eine Liste der Meta-Explainer, die im SDK verfügbar sind:

* **Tabular Explainer**: Wird mit tabellarischen Datasets verwendet.
* **Text Explainer**: Wird mit Textdatasets verwendet.
* **Image Explainer**: Wird mit Bilddatasets verwendet.

Neben der Meta-Auswahl der direkten Explainer entwickeln Meta-Explainer zusätzliche Features mithilfe der zugrunde liegenden Bibliotheken und verbessern die Geschwindigkeit und Skalierbarkeit im Vergleich mit den direkten Explainern.

Derzeit verwendet `TabularExplainer` die folgende Logik, um die direkten Explainer aufzurufen:

1. Wenn es sich um eine baumstrukturbasiertes Modell handelt, `TreeExplainer` anwenden, andernfalls
2. Wenn es sich um eine DNN-Modell handelt, `DeepExplainer` anwenden, andernfalls
3. Als Blackbox-Modell behandeln und Folgendes anwenden `KernelExplainer`

Die in `TabularExplainer` integrierte Intelligenz wird ausgefeilter werden, da mehr Bibliotheken in das SDK integriert werden und wir mehr über die Vor- und Nachteile der einzelnen Explainer erfahren.

`TabularExplainer` hat auch signifikante Feature- und Leistungsverbesserungen gegenüber den direkten Explainern zu verzeichnen:

* **Zusammenfassung des Initialisierungsdatasets**. In Fällen, in denen die Erklärungsgeschwindigkeit am wichtigsten ist, fassen wir das Initialisierungsdataset zusammen und erzeugen einen kleinen Satz repräsentativer Stichproben, was sowohl die globale als auch die lokale Erklärung beschleunigt.
* **Stichprobenentnahme im Auswertungsdataset**. Wenn der Benutzer eine Vielzahl von Auswertungsstichproben übergibt, aber nicht alle davon ausgewertet werden müssen, kann der Stichprobenparameter auf TRUE festgelegt werden, um die globale Erklärung zu beschleunigen.
* **Schnelle KNN-Erklärung (K-Nearest Neighbor)**. Für den Fall, dass die Erklärung so schnell sein muss wie eine einzelne Bewertung/Vorhersage, kann eine KNN-Methode verwendet werden. Bei der globalen Erklärung bleiben sowohl die Initialisierungsstichproben als auch die entsprechenden top-k-Features erhalten. Um die Erklärung für jede Auswertungsstichprobe zu generieren, wird die KNN-Methode verwendet, um die am ähnlichsten Stichprobe in den Initialisierungsstichproben zu finden. Die top-k-Features der ähnlichsten Stichprobe werden als top-k-Features für die Auswertungsstichprobe zurückgegeben.

Das folgende Diagramm zeigt die Beziehung zwischen den beiden Gruppen von direkten und Meta-Explainern.

[![MArchitektur von Machine Learning Interpretability](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)

### <a name="models-supported"></a>Unterstützte Modelle

Alle Modelle, die mit Datasets im Python-Format `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData` oder `scipy.sparse.csr_matrix` trainiert wurden, werden vom Machine Learning Interpretability SDK unterstützt.

Die Erklärungsunktionen akzeptieren sowohl Modelle als auch Pipelines als Eingabe. Wenn ein Modell bereitgestellt wird, muss das Modell die Vorhersagefunktion `predict` oder `predict_proba` implementieren, die der Scikit-Konvention entspricht. Wenn eine Pipeline (Name des Pipelineskripts) angegeben wird, nimmt die Erklärungsfunktion an, dass das ausgeführte Pipelineskript eine Vorhersage zurückgibt.

### <a name="local-and-remote-compute-target"></a>Lokales- und Remotecomputeziel

Das Machine Learning Interpretability SDK funktioniert mit lokalen und Remotecomputezielen. 

* Bei **lokaler** Ausführung werden vom SDK keine Verbindungen mit Azure-Diensten hergestellt.

* Bei **Remote**ausführung werden Informationen zur Ausführung der Azure Machine Learning-Diensten für den Ausführungsverlauf protokolliert. Nachdem diese Informationen protokolliert wurden, stehen Berichte und Visualisierungen aus der Erklärung im Portal mit den Azure Machine Learning-Arbeitsbereichen zur Analyse durch den Benutzer bereit.

## <a name="train-and-explain-locally"></a>Lokales Trainieren und Erklären

1. Trainieren Sie Ihr Modell in einem lokalen Jupyter-Notebook. 

    ``` python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    # Split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data, breast_cancer_data.target, test_size=0.2, random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Rufen Sie den Explainer auf. Übergeben Sie beim Instanziieren eines Explainer-Objekts das Modell und die Trainingsdaten. Optional können Sie interessante Features übergeben. Übergeben Sie bei Verwenden der Klassifizierung die Namen der Ausgabeklassen.

    Das folgende Beispiel veranschaulicht die Erstellung eines Explainer-Objekts unter lokaler Verwendung von [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py), [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py) und `LimeExplainer`. `TabularExplainer` ruft einen der drei darunter liegenden Explainer (`TreeExplainer`, `DeepExplainer` oder `KernelExplainer`) auf und wählt automatisch den für Ihren Anwendungsfall am besten geeigneten aus. Sie können jedoch jeden der drei zugrunde liegenden Explainer direkt aufrufen.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    or
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
    explainer = MimicExplainer(model, x_train, LGBMExplainableModel, features=breast_cancer_data.feature_names, classes=classes)
    or
    from azureml.contrib.explain.model.lime.lime_explainer import LIMEExplainer
    explainer = LIMEExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    ```

3. Rufen Sie die Wichtigkeitswerte globaler Features ab.

    ```python
    # You can use the training data or the test data here. 
    global_explanation = explainer.explain_global(x_train)
    # Sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))
    ```

4. Wichtigkeitswerte lokaler Features: Verwenden Sie die folgenden Funktionsaufrufe, um eine einzelne Instanz oder eine Gruppe von Instanzen zu erklären.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0,:])
    or
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:4,:])
    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

## <a name="train-and-explain-remotely"></a>Remotetraining und -Erklärung

Sie können auf den verschiedenen vom Azure Machine Learning Service unterstützten Computezielen trainieren. Das Beispiel in diesem Abschnitt zeigt, wie Sie dies mit AMLCompute erfolgt.

1. Erstellen Sie ein Trainingsskript in einem lokalen Jupyter-Notebook (Beispiel: run_explainer.py).

    ``` python  
    run = Run.get_context()
    client = ExplanationClient.from_run(run)
    
    breast_cancer_data = load_breast_cancer()
    X_train, X_test, y_train, y_test = train_test_split(breast_cancer_data.data, breast_cancer_data.target, test_size = 0.2, random_state = 0)
    data = {
        "train":{"X": X_train, "y": y_train},        
        "test":{"X": X_test, "y": y_test}
    }
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(data['train']['X'], data['train']['y'])
    joblib.dump(value = clf, filename = 'model.pkl')
    # explain predictions on your local machine    
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(data["test"]["X"])
    # explain local data points (individual instances)
    local_explanation = explainer.explain_local(data["test"]["X"][0,:])
    # upload global and local explanation objects to Run History
    upload_model_explanation(run, local_explanation, top_k=2, comment='local explanation: top 2 features')
    # Uploading global model explanation data for storage or visualization in webUX
    # The explanation can then be downloaded on any compute
    # Multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # Or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Um eine Trainingsausführung zu übermitteln, befolgen Sie die Anweisungen im Artikel [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md#amlcompute). Führen Sie die Schritte zum Erstellen eines Azure Machine Learning Compute-Ziels aus, und übermitteln Sie dann eine Trainingsausführung.

3. Laden Sie die Erklärung in Ihr lokales Jupyter-Notebook herunter. 

    ``` python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    # Get model explanation data
    client = ExplanationClient.from_run(run)
    explanation = client.download_model_explanation()
    local_importance_values = explanation.local_importance_values
    expected_values = explanation.expected_values
    # Or you can use the saved run.id to retrive the feature importance values
    client = ExplanationClient.from_run_id(ws, experiment_name, run.id)
    explanation = client.download_model_explanation()
    local_importance_values = explanation.local_importance_values
    expected_values = explanation.expected_values
    # Get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="next-steps"></a>Nächste Schritte

Um eine Sammlung von Jupyter-Notebooks zu sehen, in denen die obigen Anweisungen demonstriert werden, lesen Sie die [Beispielnotebooks für Azure Machine Learning Interpretability](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
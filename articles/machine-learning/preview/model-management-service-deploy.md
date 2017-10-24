---
title: Bereitstellung eines Azure Machine Learning-Modells mithilfe der Modellverwaltung als Webdienst | Microsoft-Dokumentation
description: In diesem Dokument werden die Schritte zum Bereitstellen eines Machine Learning-Modells mithilfe der Azure Machine Learning-Modellverwaltung vorgestellt.
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 0ebf8964bc565e5d4f95fb1ad3edbc16f841851c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="deploying-a-machine-learning-model-as-a-web-service"></a>Bereitstellen eines Machine Learning-Modells als Webdienst

Die Azure Machine Learning-Modellverwaltung bietet Schnittstellen zum Bereitstellen von Modellen als Webdienste in Docker-Containern. Sie können Modelle bereitstellen, die Sie mit Frameworks wie Spark, Microsoft Cognitive Toolkit (CNTK), Keras, Tensorflow und Python erstellen. 

In diesem Dokument werden die Schritte zum Bereitstellen Ihrer Modelle als Webdienste mithilfe der CLI (Befehlszeilenschnittstelle) der Azure Machine Learning-Modellverwaltung beschrieben.

## <a name="deploying-web-services"></a>Bereitstellen von Webdiensten
Mithilfe der CLIs können Sie Webdienste zur Ausführung auf dem lokalen Computer oder in einem Cluster bereitstellen.

Es wird empfohlen, mit einer lokalen Bereitstellung anzufangen. Sie überprüfen zunächst, ob Ihr Modell und Code funktionieren. Anschließend stellen Sie den Webdienst in einem Cluster zur Nutzung in der Produktionsumgebung bereit. Weitere Informationen zum Einrichten der Umgebung für die Bereitstellung im Cluster finden Sie unter [Konfiguration der Modellverwaltung](model-management-configuration.md). 

Es folgen die Schritte zur Bereitstellung:
1. Verwenden eines gespeicherten, trainierten Machine Learning-Modells
2. Erstellen eines Schemas für die Ein- und Ausgabedaten des Webdiensts
3. Erstellen eines Docker-basierten Containerimages
4. Erstellen und Bereitstellen des Webdiensts

### <a name="1-save-your-model"></a>1. Speichern Ihres Modells
Beginnen Sie mit der gespeicherten trainierten Modelldatei, z.B. „mymodel.pkl“. Die Dateinamenerweiterung hängt von der Plattform ab, die Sie zum Trainieren und Speichern des Modells verwenden. 

Sie können beispielsweise die Bibliothek „Pickle“ von Python zum Speichern eines trainierten Modells in einer Datei verwenden. Es folgt ein [Beispiel](http://scikit-learn.org/stable/modules/model_persistence.html) für das Dataset „Iris“:

```python
import pickle
from sklearn import datasets
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf.fit(X, y)  
saved_model = pickle.dumps(clf)
```

### <a name="2-create-a-schemajson-file"></a>2. Erstellen der Datei „schema.json“
Dieser Schritt ist optional. 

Erstellen Sie ein Schema zum automatischen Überprüfen der Ein- und Ausgabe Ihres Webdiensts. Die CLIs nutzen das Schema auch zum Generieren eines Swagger-Dokuments für Ihren Webdienst.

Um das Schema zu erstellen, importieren Sie die folgenden Bibliotheken:

```python
from azureml.api.schema.dataTypes import DataTypes
from azureml.api.schema.sampleDefinition import SampleDefinition
from azureml.api.realtime.services import generate_schema
```
Definieren Sie als Nächstes die Eingabevariablen, wie z.B. einen Spark-Datenrahmen, Pandas-Datenrahmen oder ein NumPy-Array. Im folgenden Beispiel wird ein NumPy-Array verwendet:

```python
inputs = {"input_array": SampleDefinition(DataTypes.NUMPY, yourinputarray)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```
Im folgenden Beispiel wird ein Spark-Datenrahmen verwendet:

```python
inputs = {"input_df": SampleDefinition(DataTypes.SPARK, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

Im folgenden Beispiel wird ein Pandas-Datenrahmen verwendet:

```python
inputs = {"input_df": SampleDefinition(DataTypes.PANDAS, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

### <a name="3-create-a-scorepy-file"></a>3. Erstellen der Datei „score.py“
Sie stellen die Datei „score.py“ bereit, die das Modell lädt und die Vorhersageergebnisse mit dessen Hilfe zurückgibt.

Die Datei muss zwei Funktionen enthalten: „init“ und „run“.

Fügen Sie folgenden Code am Anfang der Datei „score.py“ hinzu, um Datenerfassungsfunktionalität zu aktivieren, die das Sammeln von Modelleingabe- und Vorhersagedaten erleichtert.

    ```
    from azureml.datacollector import ModelDataCollector
    ```

Im Artikel zum [Sammeln von Modelldaten](how-to-use-model-data-collection.md) erfahren Sie detailliert, wie Sie dieses Feature verwenden.

#### <a name="init-function"></a>„init“-Funktion
Verwenden Sie die „init“-Funktion, um das gespeicherte Modell zu laden.

Beispiel einer einfachen „init“-Funktion, die das Modell lädt:

```python
def init():  
    from sklearn.externals import joblib
    global model, inputs_dc, prediction_dc
    model = joblib.load('model.pkl')

    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
```

#### <a name="run-function"></a>„run“-Funktion
Die „run“-Funktion nutzt das Modell und die Eingabedaten, um eine Vorhersage zurückzugeben.

Beispiel einer einfachen „run“-Funktion, die die Eingabe verarbeitet und das Vorhersageergebnis zurückgibt:

```python
def run(input_df):
    global clf2, inputs_dc, prediction_dc
    try:
        prediction = model.predict(input_df)
        inputs_dc.collect(input_df)
        prediction_dc.collect(prediction)
        return prediction
    except Exception as e:
        return (str(e))
```

### <a name="4-register-a-model"></a>4. Registrieren eines Modells
Der folgende Befehl dient zum Registrieren eines Modells, das in Schritt 1 oben erstellt haben.

```
az ml model register --model [path to model file] --name [model name]
```

### <a name="5-create-manifest"></a>5. Erstellen eines Manifests
Der folgenden Befehl dient zum Erstellen eines Manifests für das Modell.

```
az ml manifest create --manifest-name [your new manifest name] -f [path to code file] -r [runtime for the image, e.g. spark-py]
```
Sie können ein zuvor registriertes Modell zum Manifest hinzufügen, indem Sie das Argument `--model-id` oder `-i` im zuvor gezeigten Befehl verwenden. Mehrere Modelle können mit zusätzlichen „-i“-Argumenten angegeben werden.

### <a name="6-create-an-image"></a>6. Erstellen eines Images 
Sie können ein Image erstellen, nachdem Sie sein Manifest zuvor erstellt zu haben. 

```
az ml image create -n [image name] -manifest-id [the manifest ID]
```

Oder Sie können das Manifest und das Image mit einem einzigen Befehl erstellen. 

```
az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime eg.g. spark-py which is the Docker container image base]
```

>[!NOTE]
>Um detaillierte Informationen zu den Befehlsparametern zu erhalten, geben Sie am Ende des Befehls „-h“ ein. Beispiel: az ml image create -h.


### <a name="7-create-and-deploy-the-web-service"></a>7. Erstellen und Bereitstellen des Webdiensts
Stellen Sie den Dienst mit dem folgenden Befehl bereit:

```
az ml service create realtime --image-id <image id> -n <service name>
```

>[!NOTE] 
>Sie können auch einen einzelnen Befehl verwenden, um die vorherigen vier Schritte auszuführen. Geben Sie „-h“ mit dem Befehl „create“ an, um mehr zu erfahren.

### <a name="8-test-the-service"></a>8. Testen des Diensts
Geben Sie den folgenden Befehl an, um Informationen zum Aufrufen des Diensts zu erhalten:

```
az ml service usage realtime -i <service id>
```

Rufen Sie den Dienst an der Eingabeaufforderung mithilfe der „run“-Funktion auf:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [INPUT DATA]}"
```

Im folgenden Beispiel wird der Beispielwebdienst „Iris“ aufgerufen:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [{\"sepal length\": 3.0, \"sepal width\": 3.6, \"petal width\": 1.3, \"petal length\":0.25}]}"
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihren Webdienst nun lokal getestet haben, können Sie ihn in einem Cluster für den großflächigen Einsatz bereitstellen. Weitere Informationen zum Einrichten eines Clusters für die Bereitstellung von Webdiensten finden Sie unter [Konfiguration der Modellverwaltung](model-management-configuration.md). 

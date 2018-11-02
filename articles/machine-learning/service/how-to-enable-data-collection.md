---
title: Aktivieren der Datensammlung für Modelle in der Produktion – Azure Machine Learning
description: Erfahren Sie, wie Daten des Azure Machine Learning-Eingabemodells in einem Azure-Blobspeicher gesammelt werden.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 09/24/2018
ms.openlocfilehash: 70c023fc8fe996060d3eff3d5a700b5f910097b4
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49113630"
---
# <a name="collect-data-for-models-in-production"></a>Sammeln von Daten für Modelle in der Produktion

In diesem Artikel erfahren Sie, wie Eingabemodelldaten aus den Azure Machine Learning Services gesammelt werden, die Sie im Azure Kubernetes-Cluster (AKS) in einem Azure-Blobspeicher bereitgestellt haben. 

Nach der Aktivierung ermöglichen Ihnen diese Daten Folgendes:
* Überwachen von Datenverschiebungen bei der Aufnahme von Produktionsdaten in das Modell

* Entscheidungsfindung in Bezug auf das erneute Trainieren und Optimieren des Modells

* Erneutes Trainieren des Modells mit den gesammelten Daten

## <a name="what-is-collected-and-where-does-it-go"></a>Welche Daten werden gesammelt, und wo werden sie gespeichert?

Die folgenden Daten können gesammelt werden:
* **Modelleingabedaten** aus Webdiensten, die im Azure Kubernetes-Cluster (AKS) bereitgestellt wurden (Sprachdaten, Bilder und Videos werden **nicht** erfasst) 
  
* Modellvorhersagen, die auf Eingabedaten aus der Produktion gestützt sind.

> [!Note]
> Vorab-Aggregation und -Berechnungen werden vom Dienst derzeit nicht abgedeckt.   

Die Ausgabe wird in einem Azure-Blob gespeichert. Da die Daten in einem Azure-Blob hinzugefügt werden, können Sie das bevorzugte Tool zum Ausführen der Analyse auswählen. 

Der Pfad zu den Ausgabedaten im Blob folgt dieser Syntax:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<identifier>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Ein Arbeitsbereich des Azure Machine Learning Service, ein lokales Verzeichnis mit Ihren Skripts und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erhalten dieser Voraussetzungen finden Sie im Dokument [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).

- Ein trainiertes Machine Learning-Modell, das im Azure Kubernetes Service (AKS) bereitgestellt werden soll. Wenn Sie keines besitzen, sehen Sie sich das Tutorial zum [Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md) an.

- Ein [AKS-Cluster](how-to-deploy-to-aks.md).

- Die folgenden [in Ihrer Umgebung](how-to-configure-environment.md) installierten Abhängigkeiten und Module:
  + Unter Linux:
    ```shell
    sudo apt-get install libxml++2.6-2v5
    pip install azureml-monitoring
    ```

  + Unter Windows:
    ```shell
    pip install azureml-monitoring
    ```

## <a name="enable-data-collection"></a>Aktivieren der Datensammlung
Die Datensammlung kann ungeachtet des über den Azure Machine Learning-Dienst oder andere Tools bereitgestellten Modells aktiviert werden. 

Zum Aktivieren müssen Sie folgende Schritte ausführen:

1. Öffnen Sie die Bewertungsdatei. 

1. Fügen Sie den folgenden Code am Anfang der Datei hinzu:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

2. Deklarieren Sie die Variablen für die Datensammlung in der `init()`-Funktion:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", identifier="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", identifier="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *CorrelationId* ist ein optionaler Parameter. Er muss nicht festgelegt werden, wenn dies vom Modell nicht gefordert wird. Eine vorhandene Korrelations-ID erleichtert die Zuordnung zu anderen Daten. (Beispiele: LoanNumber, CustomerId usw.)
    
    *Identifier* wird später zum Erstellen der Ordnerstruktur im Blob verwendet. Damit können Rohdaten von verarbeiteten Daten abgegrenzt werden.

3.  Fügen Sie die folgenden Codezeilen der `run(input_df)`-Funktion hinzu:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

4. Die Datensammlung wird **nicht** automatisch auf **true** festgelegt, wenn Sie einen Dienst in AKS bereitstellen. Daher müssen Sie Ihre Konfigurationsdatei wie folgt aktualisieren: 

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```
    AppInsights für die Dienstüberwachung kann ebenfalls aktiviert werden, indem diese Konfiguration geändert wird:
    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ``` 

5. [Erstellen Sie ein neues Image, und stellen Sie Ihren Dienst bereit.](how-to-deploy-to-aks.md) 


Wenn Sie in der **Umgebungsdatei** und **Bewertungsdatei** bereits einen Dienst mit den Abhängigkeiten installiert haben, aktivieren Sie die Datensammlung wie folgt:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

1. Öffnen Sie Ihren Arbeitsbereich.

1. Navigieren Sie zu **Bereitstellungen** -> **Dienst auswählen** -> **Bearbeiten**.

   ![Dienst bearbeiten](media/how-to-enable-data-collection/EditService.png)

1. Deaktivieren Sie in **Erweiterte Einstellungen** die Option **Modelldatensammlung aktivieren**. 

   ![Datensammlung deaktivieren](media/how-to-enable-data-collection/CheckDataCollection.png)

   In diesem Fenster können Sie auch die Option „AppInsights-Diagnose aktivieren“ auswählen, um die Integrität des Diensts zu verfolgen.  

1. Klicken Sie auf **Aktualisieren**, um die Änderungen zu übernehmen.


## <a name="disable-data-collection"></a>Datensammlung deaktivieren
Sie können die Datensammeln jederzeit beenden. Deaktivieren Sie die Datensammlung mithilfe von Python-Code oder im Azure-Portal.

+ Option 1 – Deaktivieren im Azure-Portal: 
  1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

  1. Öffnen Sie Ihren Arbeitsbereich.

  1. Navigieren Sie zu **Bereitstellungen** -> **Dienst auswählen** -> **Bearbeiten**.

     ![Dienst bearbeiten](media/how-to-enable-data-collection/EditService.png)

  1. Deaktivieren Sie in **Erweiterte Einstellungen** die Option **Modelldatensammlung aktivieren**. 

     ![Datensammlung deaktivieren](media/how-to-enable-data-collection/UncheckDataCollection.png) 

  1. Klicken Sie auf **Aktualisieren**, um die Änderungen zu übernehmen.

* Option 2 – Deaktivieren der Datensammlung mithilfe von Python-Code:

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="example-notebook"></a>Notebook mit Beispielen

Im Notebook [00.Getting Started/12.enable-data-collection-for-models-in-aks.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/12.enable-data-collection-for-models-in-aks) werden die in diesem Artikel behandelten Konzepte veranschaulicht.  

Notebook abrufen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
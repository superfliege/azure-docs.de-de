---
title: Ausführen von Azure Machine Learning-Workloads mit automatisiertem maschinellem Lernen (AutoML) in Apache Spark in Azure HDInsight
description: Hier erfahren Sie, wie Sie Azure Machine Learning-Workloads mit automatisiertem maschinellem Lernen (AutoML) in Apache Spark in Azure HDInsight ausführen.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/14/2019
ms.openlocfilehash: 896cae9b7fc43765e340ba3b92351e04b5512efd
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57762550"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-automl-on-apache-spark-in-azure-hdinsight"></a>Ausführen von Azure Machine Learning-Workloads mit automatisiertem maschinellem Lernen (AutoML) in Apache Spark in Azure HDInsight

Azure Machine Learning ist ein Drag & Drop-Tool für die Zusammenarbeit, mit dem Sie Predictive Analytics-Lösungen für Ihre Daten erstellen, testen und bereitstellen können. Azure Machine Learning veröffentlicht Modelle als Webdienste, die von benutzerdefinierten Apps oder BI-Tools wie Excel problemlos genutzt werden können. Automatisiertes maschinelles Lernen (AutoML) unterstützt Sie beim Erstellen von qualitativ hochwertigen Machine Learning-Modellen durch intelligente Automatisierung und Optimierung. AutoML wählt die richtigen Algorithmen und Hyperparameter für bestimmte Problemtypen aus.

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Installieren von Azure Machine Learning in einem HDInsight-Cluster

> [!Note]
> Der Azure ML-Arbeitsbereich ist derzeit in den folgenden Regionen verfügbar: „eastus“, „eastus2“ und „westcentralus“. Der HDInsight-Cluster sollte ebenfalls in einer dieser Regionen erstellt werden.

Allgemeine Tutorials zu Azure Machine Learning und zum automatisierten maschinellen Lernen finden Sie unter [Tutorial: Erstellen Ihres ersten Data Science-Experiments in Azure Machine Learning Studio](../../machine-learning/studio/create-experiment.md) und [Tutorial: Erstellen Ihres Regressionsmodells mit automatisiertem Machine Learning](../../machine-learning/service/tutorial-auto-train-models.md).
Führen Sie zum Installieren von AzureML in Ihrem Azure HDInsight-Cluster die Skriptaktion [install_aml](https://commonartifacts.blob.core.windows.net/automl/install_aml.sh) auf den Hauptknoten und Workerknoten eines HDInsight-Clusters vom Typ 3.6 Spark 2.3.0 (empfohlen) aus. Sie können diese Skriptaktion im Rahmen der Clustererstellung oder in einem vorhandenen Cluster über das Azure-Portal ausführen.

Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](../hdinsight-hadoop-customize-cluster-linux.md). Mit dem Skript installieren Sie nicht nur Azure Machine Learning-Pakete und -Abhängigkeiten, sondern laden auch ein Jupyter Notebook (in den Pfad `HdiNotebooks/PySpark` des Standardspeichers) herunter. Dieses Jupyter Notebook veranschaulicht, wie Sie eine automatisierte Machine Learning-Klassifizierung für ein einfaches Klassifizierungsproblem verwenden.

> [!Note]
> Azure Machine Learning-Pakete werden in einer Python3-Conda-Umgebung installiert. Das installierte Jupyter Notebook sollte mit dem PySpark3-Kernel ausgeführt werden.

## <a name="authentication-for-workspace"></a>Authentifizierung für Arbeitsbereich

Zum Erstellen von Arbeitsbereichen und Übermitteln von Experimenten benötigen Sie ein Authentifizierungstoken. Dieses Token können Sie mit einer [Azure AD-Anwendung](../../active-directory/develop/app-objects-and-service-principals.md) generieren. Mit einem [Azure AD-Benutzer](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python) können Sie auch das erforderliche Authentifizierungstoken generieren, wenn die mehrstufige Authentifizierung für das Konto nicht aktiviert ist.  

Im folgenden Codeausschnitt wird ein Authentifizierungstoken mit einer **Azure AD-Anwendung** erstellt.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
                tenant_id = '<Azure Tenant ID>',
                username = '<Azure AD Application ID>',
                password = '<Azure AD Application Key>'
                )
```
Im folgenden Codeausschnitt wird ein Authentifizierungstoken mit einem **Azure AD-Benutzer** erstellt.

```python
from azure.common.credentials import UserPassCredentials
credentials = UserPassCredentials('user@domain.com','my_smart_password')
```

## <a name="loading-dataset"></a>Laden von Datasets

Automatisiertes maschinelles Lernen in Spark verwendet automatisierte **Dataflows**. Dabei handelt es sich um verzögert ausgewertete, unveränderliche Vorgänge für Daten.  Ein Dataflow kann ein Dataset aus einem Blob mit öffentlichem Lesezugriff oder über eine Blob-URL mit einem SAS-Token laden.

```python
import azureml.dataprep as dprep

dataflow_public = dprep.read_csv(path='https://commonartifacts.blob.core.windows.net/automl/UCI_Adult_train.csv')

dataflow_with_token = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
```

Sie können zudem den Datenspeicher mit einer einmaligen Registrierung beim Arbeitsbereich registrieren.

## <a name="experiment-submission"></a>Experimentübermittlung

In der Konfiguration für automatisiertes maschinelles Lernen legen Sie die Konfiguration `spark_context` fest, um das Paket im verteilten Modus auszuführen. Mit der Eigenschaft `concurrent_iterations` bestimmen Sie, wie viele Iterationen parallel ausgeführt werden dürfen. Legen Sie eine Zahl fest, die kleiner ist als die Anzahl von Executorkernen für die Spark-App.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Motivation hinter automatisiertem Machine Learning finden Sie im Blogbeitrag [Release models at pace using Microsoft’s automated machine learning!](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/) (Schnelles Veröffentlichen von Modellen mit automatisiertem maschinellem Lernen von Microsoft).
* [AutoML-Projekt von Microsoft Research](https://www.microsoft.com/research/project/automl/)
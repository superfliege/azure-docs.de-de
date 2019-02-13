---
title: Machine Learning-CLI-Erweiterung
titleSuffix: Azure Machine Learning service
description: Hier erhalten Sie Informationen zur Azure Machine Learning-CLI-Erweiterung für die Azure CLI. Die Azure CLI ist ein plattformübergreifendes Befehlszeilenhilfsprogramm, das das Arbeiten mit Ressourcen in der Azure-Cloud ermöglicht. Die Machine Learning-Erweiterung ermöglicht Ihnen das Arbeiten mit dem Azure Machine Learning-Dienst.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 65936348dcb40c6ceb71ebf735da8bb2120af654
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694515"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Verwenden der CLI-Erweiterung für Azure Machine Learning Service

Der Azure Machine Learning-CLI ist eine Erweiterung der [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), eine plattformübergreifende Befehlszeilenschnittstelle für die Azure-Plattform. Diese Erweiterung unterstützt Befehle für die Arbeit mit dem Azure Machine Learning-Dienst über die Befehlszeile. Zudem können Sie Skripts erstellen, die Ihre Machine Learning-Workflows automatisieren. Beispielsweise können Sie Skripts erstellen, die die folgenden Aktionen ausführen:

+ Ausführen von Experimenten zum Erstellen von Machine Learning-Modellen

+ Registrieren von Machine Learning-Modellen zur Verwendung durch Kunden

+ Paketieren, Bereitstellen und Nachverfolgen des Lebenszyklus Ihrer Machine Learning-Modelle

Die CLI ist kein Ersatz für das Azure Machine Learning SDK. Sie stellt ein ergänzende Tool dar, das für die Verarbeitung hochgradig parametrisierter Aufgaben optimiert ist:

* Erstellen von Computeressourcen

* Übermittlung parametrisierter Experimente

* Modellregistrierung

* Erstellung von Images

* Dienstbereitstellung

## <a name="prerequisites"></a>Voraussetzungen


* Für die Verwendung der CLI benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie heute die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](http://aka.ms/AMLFree) aus.

* Die [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)

## <a name="install-the-extension"></a>Installieren der Erweiterung

Um die Machine Learning-CLI-Erweiterung zu installieren, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-1.0.10-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Klicken Sie bei Aufforderung auf `y`, um die Erweiterung zu installieren.

Um sicherzustellen, dass die Erweiterung installiert wurde, verwenden Sie den folgenden Befehl zum Anzeigen einer Liste von ML-spezifischen Unterbefehlen:

```azurecli-interactive
az ml -h
```

> [!TIP]
> Um die Erweiterung zu aktualisieren, müssen Sie sie __entfernen__ und dann __installieren__. So wird die aktuelle Version installiert.

## <a name="remove-the-extension"></a>Entfernen der Erweiterung

Verwenden Sie zum Entfernen der CLI-Erweiterung den folgenden Befehl:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Ressourcenverwaltung

Die folgenden Befehle veranschaulichen, wie Sie mit der CLI Ressourcen verwalten, die von Azure Machine Learning verwendet werden.


+ Hiermit erstellen Sie einen Azure Machine Learning-Dienstarbeitsbereich:

    ```azurecli-interactive
    az ml workspace create -n myworkspace -g myresourcegroup
    ```

+ Hiermit legen Sie einen Standardarbeitsbereich fest:

    ```azurecli-interactive
    az configure --defaults aml_workspace=myworkspace group=myresourcegroup
    ```
    
* Anfügen eines AKS-Clusters

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

## <a name="experiments"></a>Experimente

Die folgenden Befehle veranschaulichen, wie Sie die CLI zum Arbeiten mit Experimenten verwenden:

* Hiermit fügen Sie vor dem Absenden eines Experiments ein Projekt an (Laufzeitkonfiguration):

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Starten Sie eine Ausführung Ihres Experiments. Geben Sie bei der Verwendung dieses Befehls den Namen der `.runconfig`-Datei an, die die Ausführungskonfiguration enthält. Das Computeziel verwendet die Ausführungskonfiguration, um die Trainingsumgebung für das Modell zu erstellen. In diesem Beispiel wird die Ausführungskonfiguration aus der Datei `./aml_config/myrunconfig.runconfig` geladen.

    ```azurecli-interactive
    az ml run submit -c myrunconfig train.py
    ```

    Die `.runconfig`-Standarddateien namens `docker.runconfig` und `local.runconfig` werden erstellt, wenn Sie ein Projekt mit dem Befehl `az ml project attach` anfügen. Möglicherweise müssen Sie diese Dateien ändern, bevor Sie sie zum Trainieren eines Modells verwenden. 

    Sie können eine Ausführungskonfiguration auch mit der [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py)-Klasse programmgesteuert erstellen. Nach der Erstellung können Sie dann die `save()`-Methode verwenden, um die `.runconfig`-Datei zu erstellen.

* Hiermit zeigen Sie eine Liste der übermittelten Experimente an:

    ```azurecli-interactive
    az ml history list
    ```

## <a name="model-registration-image-creation--deployment"></a>Modellregistrierung, Imageerstellung und Bereitstellung

Die folgenden Befehle veranschaulichen, wie ein trainiertes Modell registriert und dann als Produktionsdienst bereitgestellt wird:

+ Hiermit registrieren Sie ein Modell bei Azure Machine Learning:

  ```azurecli-interactive
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Hiermit erstellen Sie ein Image, das Ihr Machine Learning-Modell und die zugehörigen Abhängigkeiten enthält: 

  ```azurecli-interactive
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Hiermit stellen Sie ein Image für ein Computeziel bereit:

  ```azurecli-interactive
  az ml service create aci -n myaciservice --image-id myimage:1
  ```

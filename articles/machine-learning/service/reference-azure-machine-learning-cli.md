---
title: 'Gewusst wie: Verwendung der Azure Machine Learning-CLI-Erweiterung'
description: Hier erhalten Sie Informationen zur Azure Machine Learning-CLI-Erweiterung für die Azure CLI. Die Azure CLI ist ein plattformübergreifendes Befehlszeilenhilfsprogramm, das das Arbeiten mit Ressourcen in der Azure-Cloud ermöglicht. Die Machine Learning-Erweiterung ermöglicht Ihnen das Arbeiten mit dem Azure Machine Learning-Dienst.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: 13d09471191deed670db97a9f18e15bc9577dd1a
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713417"
---
# <a name="use-the-azure-machine-learning-cli-extension"></a>Verwendung der Azure Machine Learning-CLI-Erweiterung

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

* Die [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)

> [!NOTE]
> Für die Verwendung der CLI benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLfree) erstellen, bevor Sie beginnen.

## <a name="install-the-extension"></a>Installieren der Erweiterung

Um die Machine Learning-CLI-Erweiterung zu installieren, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.68-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
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

+ Hiermit erstellen Sie eine DSVM (Data Science VM). Sie können auch BatchAI-Cluster für verteiltes Training oder AKS-Cluster für die Bereitstellung erstellen.


  ```azurecli-interactive
  az ml computetarget setup dsvm -n mydsvm
  ```

## <a name="experiments"></a>Experimente

Die folgenden Befehle veranschaulichen, wie Sie die CLI zum Arbeiten mit Experimenten verwenden:

* Hiermit fügen Sie vor dem Absenden eines Experiments ein Projekt an (Laufzeitkonfiguration):

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Starten Sie eine Ausführung Ihres Experiments. Wenn Sie diesen Befehl verwenden, geben Sie ein Computeziel an. In diesem Beispiel verwendet `local` den lokalen Computer, um das Modell über das Skript `train.py` zu trainieren:

    ```azurecli-interactive
    az ml run submit -c local train.py
    ```

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

---
title: Informationen zur Azure Machine Learning-CLI-Erweiterung
description: Hier erhalten Sie Informationen zur Machine Learning-CLI-Erweiterung für Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: 45ed1867d6d151250340bb21450b4b0d9b00e993
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243146"
---
# <a name="what-is-the-azure-machine-learning-cli"></a>Was ist die Azure Machine Learning-CLI?

Die Azure Machine Learning-CLI-Erweiterung (Command Line Interface, Befehlszeilenschnittstelle) richtet sich an Data Scientists und Entwickler, die mit dem Azure Machine Learning-Dienst arbeiten. Sie ermöglicht Ihnen eine schnelle Automatisierung von Machine Learning-Workflows und deren Aufnahme in die Produktion:
+ Ausführen von Experimenten zum Erstellen von Machine Learning-Modellen

+ Registrieren von Machine Learning-Modellen zur Verwendung durch Kunden

+ Paketieren, Bereitstellen und Verfolgen des Lebenszyklus Ihrer Machine Learning-Modelle

Diese Machine Learning-CLI ist eine Erweiterung der [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) und basiert auf dem Python-basierten <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> für den Azure Machine Learning-Dienst.

> [!NOTE]
> Die Befehlszeilenschnittstelle befindet sich derzeit in einer frühen Vorschauphase und wird noch aktualisiert.

## <a name="installing-and-uninstalling"></a>Installation und Deinstallation

Sie können die CLI mit dem folgenden Befehl über unseren Vorschau-PyPi-Index installieren:
```AzureCLI
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.68-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Mit diesem Befehl können Sie die CLI entfernen:
```AzureCLI
az extension remove -n azure-cli-ml
```

Sie können die CLI mit den oben genannten Schritten **remove** und **add** aktualisieren.

## <a name="using-the-cli-vs-the-sdk"></a>Verwenden der CLI im Vergleich zum SDK
Die CLI eignet sich besser für die Automatisierung durch eine DevOps-Persona oder als Teil einer Continuous Integration- und Continuous Delivery-Pipeline. Sie wurde für die Verarbeitung seltener und hoch parametrisierter Aufgaben optimiert. 

Beispiele:
- Computebereitstellung
- Übermittlung parametrisierter Experimente
- Modellregistrierung, Imageerstellung
- Dienstbereitstellung

Data Scientists wird die Verwendung des Azure ML SDK empfohlen.

## <a name="common-machine-learning-cli-commands"></a>Weitere gängige Befehle der Machine Learning-CLI
> [!NOTE]
> Beispieldateien, die Sie zur erfolgreichen Ausführung der folgenden Befehle verwenden können, finden Sie [hier](https://github.com/Azure/MachineLearningNotebooks/tree/cli/cli).

Verwenden Sie den umfassenden Satz an `az ml`-Befehlen, um mit dem Dienst in einer beliebigen Befehlszeilenumgebung zu interagieren, einschließlich der Cloud Shell im Azure-Portal.

Hier sehen Sie ein Beispiel für häufig verwendete Befehle:

### <a name="workspace-creation--compute-setup"></a>Arbeitsbereichserstellung und Computeeinrichtung

+ Hiermit erstellen Sie einen Arbeitsbereich für den Azure Machine Learning Service, die Ressource der obersten Ebene für Machine Learning.
   ```AzureCLI
   az ml workspace create -n myworkspace -g myresourcegroup
   ```

+ Hiermit richten Sie die CLI so ein, dass dieser Arbeitsbereich standardmäßig verwendet wird.
   ```AzureCLI
   az configure --defaults aml_workspace=myworkspace group=myresourcegroup
   ```

+ Hiermit erstellen Sie eine DSVM (Data Science VM). Sie können auch BatchAI-Cluster für verteiltes Training oder AKS-Cluster für die Bereitstellung erstellen.
  ```AzureCLI
  az ml computetarget setup dsvm -n mydsvm
  ```

### <a name="experiment-submission"></a>Experimentübermittlung
+ Hiermit fügen Sie das Experiment zur Übermittlung an ein Projekt an (Laufzeitkonfiguration). Diese Option wird verwendet, um Experimentausführungen zu verfolgen.
  ```AzureCLI
  az ml project attach --experiment-name myhistory
  ```

+ Hiermit übermitteln Sie ein Experiment für den Azure Machine Learning-Dienst auf dem Computeziel Ihrer Wahl. Dieses Beispiel wird für Ihre lokale Compute-Umgebung ausgeführt. Stellen Sie sicher, dass Ihre Conda-Umgebungsdatei Ihre Python-Abhängigkeiten erfasst.

  ```AzureCLI
  az ml run submit -c local train.py
  ```

+ Hiermit zeigen Sie eine Liste der übermittelten Experimente an.
```AzureCLI
az ml history list
```

### <a name="model-registration-image-ceation--deployment"></a>Modellregistrierung, Imageerstellung und Bereitstellung

+ Hiermit registrieren Sie ein Modell bei Azure Machine Learning.
  ```AzureCLI
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Hiermit erstellen Sie ein Image, das Ihr Machine Learning-Modell und die zugehörigen Abhängigkeiten enthält. 
  ```AzureCLI
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Hiermit stellen Sie Ihr paketiertes Modell für Ziele wie ACI und AKS bereit.
  ```AzureCLI
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
    
## <a name="full-command-list"></a>Vollständige Befehlsliste
Die vollständige Liste der Befehle für die CLI-Erweiterung (und die jeweils unterstützten Parameter) erhalten Sie, indem Sie ```az ml COMMANDNAME -h``` ausführen. 

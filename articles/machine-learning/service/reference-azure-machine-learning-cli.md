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
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 9cc6ad4f7b33de4d132efe63ff11c34f10b614af
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65023383"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Verwenden der CLI-Erweiterung für Azure Machine Learning Service

Der Azure Machine Learning-CLI ist eine Erweiterung der [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), eine plattformübergreifende Befehlszeilenschnittstelle für die Azure-Plattform. Diese Erweiterung unterstützt Befehle für die Arbeit mit dem Azure Machine Learning-Dienst über die Befehlszeile. Sie ermöglicht Ihnen, Ihre Machine Learning-Workflows zu automatisieren. Sie können beispielsweise die folgenden Aktionen ausführen:

+ Ausführen von Experimenten zum Erstellen von Machine Learning-Modellen

+ Registrieren von Machine Learning-Modellen zur Verwendung durch Kunden

+ Paketieren, Bereitstellen und Nachverfolgen des Lebenszyklus Ihrer Machine Learning-Modelle

Die CLI ist kein Ersatz für das Azure Machine Learning SDK. Sie stellt ein ergänzendes Tool dar, das für die Verarbeitung hochgradig parametrisierter Aufgaben optimiert ist.

## <a name="prerequisites"></a>Voraussetzungen

* Für die Verwendung der CLI benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie heute die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Die [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)

## <a name="install-the-extension"></a>Installieren der Erweiterung

Um die Machine Learning-CLI-Erweiterung zu installieren, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> Beispieldateien, die Sie mit den [hier](http://aka.ms/azml-deploy-cloud) aufgeführten Befehlen verwenden können.

Klicken Sie bei Aufforderung auf `y`, um die Erweiterung zu installieren.

Um sicherzustellen, dass die Erweiterung installiert wurde, verwenden Sie den folgenden Befehl zum Anzeigen einer Liste von ML-spezifischen Unterbefehlen:

```azurecli-interactive
az ml -h
```

## <a name="remove-the-extension"></a>Entfernen der Erweiterung

Verwenden Sie zum Entfernen der CLI-Erweiterung den folgenden Befehl:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Ressourcenverwaltung

Die folgenden Befehle veranschaulichen, wie Sie mit der CLI Ressourcen verwalten, die von Azure Machine Learning verwendet werden.

+ Erstellen Sie eine Ressourcengruppe, wenn noch keine vorhanden ist:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Hiermit erstellen Sie einen Azure Machine Learning-Dienstarbeitsbereich:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

+ Fügen Sie eine Arbeitsbereichskonfiguration einem Ordner hinzu, um CLI-Kontextfähigkeit zu aktivieren.
    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

+ Fügen Sie einen Azure-Blobcontainer als Datenspeicher an.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```
    
+ Fügen Sie einen AKS-Cluster als Computeziel an.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

## <a id="experiments"></a>Ausführen von Experimenten

* Starten Sie eine Ausführung Ihres Experiments. Wenn Sie diesen Befehl verwenden, geben Sie den Namen der Runconfig-Datei (den Text vor „\*.runconfig“, wenn Sie auf Ihr Dateisystem schauen) für den -c-Parameter an.

    ```azurecli-interactive
    az ml run submit-script -c local -e testexperiment train.py
    ```

* Zeigen Sie eine Liste der Experimente an:

    ```azurecli-interactive
    az ml experiment list
    ```

## <a name="model-registration-profiling--deployment"></a>Registrierung von Modellen, Profilerstellung und Bereitstellung

Die folgenden Befehle veranschaulichen, wie ein trainiertes Modell registriert und dann als Produktionsdienst bereitgestellt wird:

+ Hiermit registrieren Sie ein Modell bei Azure Machine Learning:

  ```azurecli-interactive
  az ml model register -n mymodel -p sklearn_regression_model.pkl
  ```

+ Bereitstellen Ihrer Modelldatei für AKS

  ```azurecli-interactive
  az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
  ```

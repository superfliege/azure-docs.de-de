---
title: Installieren und Verwenden der CLI für wichtige Aufgaben – Azure Machine Learning
description: Informationen zum Installieren und Verwenden der CLI für die gängigsten Machine Learning-Aufgaben in Azure Machine Learning.
services: machine-learning
author: haining
ms.author: haining
manager: cgronlun
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/10/2018
ms.openlocfilehash: 0e37e1839d2248507a30de08e2ac4c975bd3b859
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "35635656"
---
# <a name="install-and-use-the-machine-learning-cli-for-top-tasks-in-azure-machine-learning"></a>Installieren und Verwenden der Machine Learning-CLI für wichtige Aufgaben in Azure Machine Learning

Azure Machine Learning-Dienste sind eine integrierte End-to-End-Lösung für Data Science und Advanced Analytics. Professionelle Data Scientists können mit Azure Machine Learning-Diensten Daten vorbereiten, Experimente entwickeln und Modelle für die Cloud bereitstellen. 

Mit der Befehlszeilenschnittstelle (Command-Line Interface, CLI) von Azure Machine Learning können Sie:
+ Ihren Arbeitsbereich und Ihre Projekte verwalten
+ Computeziele einrichten
+ Trainingsexperimente ausführen
+ Verlauf und Metriken für die letzten Ausführungen anzeigen
+ Modelle in der Produktion als Webdienste bereitstellen
+ Produktionsmodelle und -dienste verwalten

In diesem Artikel können Sie die nützlichsten CLI-Befehle nachschlagen. 

![Azure Machine Learning-CLI](media/cli-for-azure-machine-learning/flow.png)

## <a name="what-you-need-to-get-started"></a>Voraussetzungen

Sie benötigen Mitwirkendenzugriff auf ein Azure-Abonnement oder eine Ressourcengruppe, wo Sie Ihre Modelle bereitstellen können. Außerdem müssen Sie Azure Machine Learning Workbench installieren, um die CLI auszuführen. 

>[!IMPORTANT]
>Die CLI der Azure Machine Learning-Dienste unterscheidet sich von der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), die für die Verwaltung von Azure-Ressourcen verwendet wird.

## <a name="get-and-start-cli"></a>Abrufen und starten der CLI

Um die CLI zu erhalten, installieren Sie die Azure Machine Learning Workbench, die Sie hier herunterladen können:
    + Windows – https://aka.ms/azureml-wb-msi 
    + MacOS - https://aka.ms/azureml-wb-dmg 

So starten Sie die CLI:
+ Starten Sie in der Azure Machine Learning Workbench die CLI im Menü **File -> Open Command Prompt** (Datei -> Eingabeaufforderung öffnen).

## <a name="get-command-help"></a>Hilfe zu Befehlen 

Sie können zusätzliche Informationen zu CLI-Befehlen mit `--debug` oder `--help` nach den Befehlen wie z.B. `az ml <xyz> --debug` erhalten, wobei `<xyz>` der Name des Befehls ist. Beispiel: 
```azurecli
az ml computetarget --debug 

az ml experiment --help
```

## <a name="common-cli-tasks-for-azure-machine-learning"></a>Gängige CLI-Aufgaben für Azure Machine Learning 

Erfahren Sie in diesem Abschnitt mehr über die gängigsten Aufgaben, die Sie mit der CLI ausführen können, einschließlich:
+ [Einrichten von Computezielen](#target)
+ [Senden von Aufträgen für die Remoteausführung](#jobs)
+ [Arbeiten mit Jupyter-Notebooks](#jupyter)
+ [Interaktion mit und Untersuchen von Ausführungsverläufen](#history)
+ [Konfigurieren der Umgebung zum Operationalisieren](#o16n)

<a name="target"></a>

### <a name="set-up-a-compute-target"></a>Einrichten von Computezielen

Sie können Ihr Machine Learning-Modell in verschiedenen Zielen berechnen, einschließlich:
+ im lokalen Modus
+ in einer Data Science-VM (DSVM)
+ auf einem HDInsight-Cluster

So fügen Sie ein Data Science-VM-Ziel an:
```azurecli
az ml computetarget attach remotedocker -n <target name> -a <ip address or FQDN> -u <username> -w <password>
``` 

So fügen Sie ein HDInsight-Ziel an:
```azurecli
az ml computetarget attach cluster -n <target name> -a <cluster name, e.g. myhdicluster-ssh.azurehdinsight.net> -u <ssh username> -w <ssh password>
```

Im Ordner **aml_config** können Sie die Conda-Abhängigkeiten ändern. 

Darüber hinaus können Sie mit PySpark, Python oder Python in einem GPU-DSVM arbeiten. 

So definieren Sie den Python-Betriebsmodus:
+ Fügen Sie für Python `Framework:Python` in `<target name>.runconfig` hinzu. 

+ Fügen Sie für PySpark `Framework:PySpark` in `<target name>.runconfig` hinzu. 

+ Fügen Sie für Python in einem GPU-DSVM
    1. `Framework:Python` in `<target name>.runconfig` hinzu. 

    1. Darüber hinaus fügen Sie `baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9 and nvidiaDocker:true` in `<target name>.compute` hinzu.

So bereiten Sie das Computeziel vor:
```azurecli
az ml experiment prepare -c <target name>
```

>[!TIP]
>So zeigen Sie Ihr Abonnement an:<br/>
>`az account show`<br/>
>
>So richten Sie Ihr Abonnement ein:<br/>
>`az account set –s "my subscription name" `

<a name="jobs"></a>

### <a name="submit-remote-jobs"></a>Senden von Aufträgen für die Remoteausführung

So senden Sie einen Auftrag an ein Remoteziel:
```azurecli
az ml experiment submit -c <target name> myscript.py
```

<a name="jupyter"></a>

### <a name="work-with-jupyter-notebooks"></a>Arbeiten mit Jupyter-Notebooks

So starten Sie ein Jupyter-Notebook:
```azurecli
az ml notebook start
```

Dieser Befehl startet ein Jupyter Notebook in „localhost“. Sie können lokal arbeiten, indem Sie den Kernel Python 3 auswählen, oder durch Auswählen des Kernels `<target name>` auf Ihrer Remote-VM.

<a name="history"></a>

### <a name="interact-with-and-explore-the-run-history"></a>Interaktion mit und Untersuchen von Ausführungsverläufen

So listen Sie den Ausführungsverlauf auf:
```azurecli
az ml history list -o table
```

So listen Sie alle abgeschlossenen Ausführungen auf:
```azurecli
az ml history list --query "[?status=='Completed']" -o table
```

So finden Sie Ausführungen mit der höchsten Genauigkeit:
```azurecli
az ml history list --query "@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy"
```

Sie können auch die bei jeder Ausführung generierten Dateien herunterladen. 

So stufen Sie ein Modell höher, das im Ordner „outputs“ gespeichert ist:
```azurecli
az ml history promote -r <run id> -ap outputs/model.pkl -n <model name>
```

So laden Sie dieses Modell herunter:
```azurecli
az ml asset download -l assets/model.pkl.link -d <model folder path>
```

<a name="o16n"></a>

### <a name="configure-your-environment-to-operationalize"></a>Konfigurieren der Umgebung zum Operationalisieren

Um Ihre Operationalisierungsumgebung einzurichten, müssen Sie Folgendes erstellen:

> [!div class="checklist"]
> * Eine Ressourcengruppe 
> * Ein Speicherkonto
> * Eine Azure Container Registry (ACR)
> * Ein Application Insights-Konto
> * Eine Kubernetes-Bereitstellung in einem Azure Container Service-Cluster (ACS)


So richten Sie eine lokale Bereitstellung zum Testen in einem Docker-Container ein:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name>
```

So richten Sie einen ACS-Cluster mit Kubernetes ein:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name> --cluster
```

So überwachen Sie den Status der Bereitstellung:
```azurecli
az ml env show -n <environment name> -g <resource group name>
```

So legen Sie die Umgebung fest, die verwendet werden soll:
```azurecli
az ml env set -n <environment name> -g <resource group name>
```

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit einem dieser Artikel: 
+ [Schnellstart: Installation und erste Schritte mit Azure Machine Learning-Dienste](../service/quickstart-installation.md)
+ [Tutorial 1: Klassifizieren von Iris – Vorbereiten der Daten](tutorial-classifying-iris-part-1.md)

Weiterführende Informationen bieten Ihnen diese Artikel:
+ [Referenz zur Befehlszeilenschnittstelle für die Modellverwaltung](model-management-cli-reference.md)

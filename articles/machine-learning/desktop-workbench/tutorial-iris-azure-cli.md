---
title: Tutorial für Funktionen der Azure Machine Learning-Vorschauversion – Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: Erläuterungen zu allen erforderlichen Schritte für eine lückenlose Iris-Klassifizierung über die Befehlszeilenschnittstelle
services: machine-learning
author: jpe316
ms.author: jordane
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 10/15/2017
ms.openlocfilehash: 10fe861682da6c1d1ac701a565cef11f9b44cd1e
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "41918222"
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Tutorial: Klassifizieren von Iris über die Befehlszeilenschnittstelle
Azure Machine Learning-Dienste (Vorschauversion) sind eine integrierte End-to-End-Lösung für Data Science und erweiterte Analysen, mit der professionelle Datenanalysten Daten aufbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

In diesem Tutorial wird erklärt, wie Sie über die CLI-Tools (Command-Line Interface, Befehlszeilenschnittstelle) der Funktionen der Azure Machine Learning-Vorschauversion folgende Aktionen ausführen können: 
> [!div class="checklist"]
> * Einrichten eines Experimentieren-Kontos und Erstellen eines Arbeitsbereichs
> * Erstellen eines Projekts
> * Übermitteln eines Experiments an mehrere Computeziele
> * Höherstufen und Registrieren eines trainierten Modells
> * Bereitstellen eines Webdiensts zur Bewertung von neuen Daten

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:
- Zugriff auf ein Azure-Abonnement und Berechtigungen zum Erstellen von Ressourcen in diesem Abonnement. 
  
  Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Azure Machine Learning Workbench-Anwendung, die gemäß der Anleitung unter [Quickstart: Install and get started with Azure Machine Learning services](../service/quickstart-installation.md) (Schnellstartanleitung: Installieren und Verwenden von Azure Machine Learning-Diensten) installiert ist. 

  >[!IMPORTANT]
  >Erstellen Sie keine Azure Machine Learning-Dienstkonten. Diese werden im vorliegenden Artikel über die Befehlszeilenschnittstelle erstellt.
 
## <a name="getting-started"></a>Erste Schritte
Über die Azure Machine Learning-Befehlszeilenschnittstelle (CLI) können Sie alle Aufgaben für einen lückenlosen Data Science-Workflow durchführen. Sie haben die folgenden Möglichkeiten, um auf die CLI-Tools zuzugreifen:

### <a name="option-1-launch-azure-ml-cli-from-azure-ml-workbench-log-in-dialog-box"></a>Option 1. Starten der Azure Machine Learning-CLI über das Dialogfeld zur Anmeldung bei Azure ML Workbench
Beim Starten von Azure ML Workbench wird bei der ersten Anmeldung der folgende Bildschirm angezeigt, sofern Sie noch keinen Zugriff auf ein Experimentieren-Konto haben:

![kein Konto gefunden](media/tutorial-iris-azure-cli/no_account_found.png)

Klicken Sie im Dialogfeld auf den Link **Befehlszeilenfenster**, um das Befehlszeilenfenster zu starten.

### <a name="option-2-launch-azure-ml-cli-from-azure-ml-workbench-app"></a>Option 2. Starten der Azure Machine Learning-CLI über die Azure ML Workbench-App
Wenn Sie bereits Zugriff auf ein Experimentieren-Konto besitzen, können Sie sich erfolgreich anmelden. Klicken Sie anschließend auf **Datei** --> **Eingabeaufforderung öffnen**, um das Befehlszeilenfenster zu öffnen.

### <a name="option-3-enable-azure-ml-cli-in-an-arbitrary-command-line-window"></a>Option 3. Aktivieren der Azure Machine Learning-CLI in einem beliebigen Befehlszeilenfenster
Sie können die Azure ML-CLI auch in jedem beliebigen Befehlszeilenfenster aktivieren. Starten Sie dazu ein Befehlsfenster, und geben Sie die folgenden Befehle ein:

```sh
# Windows Command Prompt
set PATH=%LOCALAPPDATA%\amlworkbench\Python;%LOCALAPPDATA%\amlworkbench\Python\Scripts;%PATH%

# Windows PowerShell
$env:Path = $env:LOCALAPPDATA+"\amlworkbench\Python;"+$env:LOCALAPPDATA+"\amlworkbench\Python\Scripts;"+$env:Path

# macOS Bash Shell
PATH=$HOME/Library/Caches/AmlWorkbench/Python/bin:$PATH
```
Für eine permanente Änderung können Sie unter Windows `SETX` verwenden. Verwenden Sie `setenv` unter macOS.

>[!TIP]
>Sie können die Azure-Befehlszeilenschnittstelle in Ihrem bevorzugten Terminalfenster aktivieren, indem Sie die oben angegebenen Umgebungsvariablen festlegen.

## <a name="step-1-log-in-to-azure"></a>Schritt 1: Anmelden an Azure
Öffnen Sie zunächst die CLI über die AMLWorkbench-App („Datei > Eingabeaufforderung öffnen“). Dadurch wird sichergestellt, dass Sie über die richtige Python-Umgebung verfügen und dass die ML-CLI-Befehle verfügbar sind. 

Nun können Sie in Ihrer Befehlszeilenschnittstelle den richtigen Kontext für den Zugriff und die Verwaltung von Azure-Ressourcen festlegen.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>Schritt 2: Erstellen eines neuen Experimentieren-Kontos und eines Arbeitsbereichs für Azure Machine Learning

In diesem Schritt erstellen Sie ein neues Experimentierkonto und einen neuen Arbeitsbereich. Weitere Informationen zu Experimentieren-Konten und Arbeitsbereichen finden Sie unter [Azure Machine Learning-Konzepte](overview-general-concepts.md).

> [!NOTE]
> Experimentieren-Konten erfordern ein Speicherkonto, in dem die Ausgaben der Experimentausführungen gespeichert werden. Der Name des Speicherkontos muss in Azure global eindeutig sein, da ihm eine URL zugeordnet wird. Wenn Sie ein vorhandenes Speicherkonto angeben, wird der Name Ihres Experimentieren-Kontos verwendet, um ein neues Speicherkonto zu erstellen. Stellen Sie sicher, dass der Name, den Sie verwenden, eindeutig ist. Andernfalls wird eine Fehlermeldung angezeigt, wie z.B. _„Das Speicherkonto mit dem Namen \<Speicherkontoname> wird bereits verwendet.“_ Mit dem Argument `--storage` können Sie auch ein vorhandenes Speicherkonto angeben.

```azure-cli
# create a resource group 
$ az group create --name <resource group name> --location <supported Azure region>

# create a new experimentation account with a new storage account
$ az ml account experimentation create --name <experimentation account name> --resource-group <resource group name>

# create a new experimentation account with an existing storage account
$ az ml account experimentation create --name <experimentation account name>  --resource-group <resource group name> --storage <storage account Azure Resource ID>

# create a workspace in the experimentation account
az ml workspace create --name <workspace name> --account <experimentation account name> --resource-group <resource group name>
```

## <a name="step-2a-optional-share-a-workspace-with-co-worker"></a>Schritt 2a: Optional: Freigabe des Arbeitsbereichs für Kollegen
Hier erfahren Sie, wie Sie einen Arbeitsbereich für einen Kollegen freigeben können. Die Vorgehensweise für die Freigabe eines Experimentieren-Kontos oder eines Projekts ist dieselbe. Sie müssen in diesem Fall nur die Methode aktualisieren, mit der die Azure-Ressourcen-ID abgerufen wird.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> `bob@contoso.com` im obigen Befehl muss eine gültige Azure AD-Identität in dem Verzeichnis sein, zu dem das aktuelle Abonnement gehört.

## <a name="step-3-create-a-new-project"></a>Schritt 3: Erstellen eines neuen Projekts
Im nächsten Schritt erstellen Sie ein neues Projekt. Dafür haben Sie verschiedene Möglichkeiten zur Auswahl:

### <a name="create-a-new-blank-project"></a>Erstellen eines neuen leeren Projekts

```azure-cli
# create a new project
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path>
```

### <a name="create-a-new-project-with-a-default-project-template"></a>Erstellen eines neuen Projekts mit einer Standardprojektvorlage
Sie können ein neues Projekt mit einer Standardvorlage erstellen.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template
```

### <a name="create-a-new-project-associated-with-a-cloud-git-repository"></a>Erstellen eines neuen Projekts mit Zuordnung zu einem Git-Repository in der Cloud
Sie können ein neues Projekt erstellen, das einem Git-Repository in VSTS (Visual Studio Team Service) zugeordnet ist. Jedes Mal, wenn ein Experiment gesendet wird, wird eine Momentaufnahme des gesamten Projektordners an das Remote-Git-Repository übergeben. Weitere Informationen finden Sie unter [Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt](using-git-ml-project.md).

> [!NOTE]
> Azure Machine Learning unterstützt nur leere Git-Repositorys in VSTS.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --repo <VSTS repo URL>
```
> [!TIP]
> Wenn der Fehler „Repository-URL ist möglicherweise ungültig, oder der Benutzer hat keinen Zugriff“ angezeigt wird, können Sie ein Sicherheitstoken in VSTS erstellen (unter _Sicherheit_ im Menü _Persönliche Zugriffstoken hinzufügen_). Verwenden Sie beim Erstellen des Projekts außerdem das Argument `--vststoken`. 

### <a name="sample_create"></a>Erstellen eines neuen Projekts über ein Beispiel
In diesem Beispiel erstellen Sie ein neues Projekt mit einem Beispielprojekt als Vorlage.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Nachdem das Projekt erstellt wurde, verwenden Sie den Befehl `cd`, um zum Projektverzeichnis zu wechseln.

## <a name="step-4-run-the-training-experiment"></a>Schritt 4: Ausführen des Trainingsexperiments 
Bei den folgenden Schritten wird davon ausgegangen, dass Sie über ein Projekt mit dem Beispiel „Iris“ verfügen. (Information hierzu finden Sie unter [Erstellen eines neuen Projekts über ein Onlinebeispiel](#sample_create).)

### <a name="prepare-your-environment"></a>Vorbereiten der Umgebung 
Für das Beispiel „Iris“ müssen Sie „matplotlib“ installieren.

```azure-cli
$ pip install matplotlib
```

###  <a name="submit-the-experiment"></a>Übermitteln des Experiments

```azure-cli
# Execute the file
$ az ml experiment submit --run-configuration local iris_sklearn.py
```

### <a name="iterate-on-your-experiment-with-descending-regularization-rates"></a>Durchlaufen des Experiments mit absteigenden Regularisierungsraten
Mit etwas Fantasie lässt sich ein Python-Skript ganz leicht zusammenstellen, das Experimente mit verschiedenen Regularisierungsraten übermittelt. (Möglicherweise müssen Sie die Datei bearbeiten, damit auf den richtigen Projektpfad verwiesen wird.)

```azure-cli
$ python run.py
```

## <a name="step-5-view-run-history"></a>Schritt 5: Anzeigen des Ausführungsverlaufs
Mit dem folgenden Befehl werden alle vorherigen Ausführungen angezeigt: 

```azure-cli
$ az ml history list -o table
```
Mit dem obigen Befehl wird eine Liste mit allen Ausführungen angezeigt, die zu diesem Projekt gehören. Dabei werden auch Metriken zur Genauigkeit und den Regularisierungsraten aufgeführt. Damit können Sie ganz leicht die beste Ausführung in der Liste identifizieren.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>Schritt 5a: Anzeigen der Anlage, die bei einer bestimmten Ausführung erstellt wurde 
Verwenden Sie den Befehl „info“ des Ausführungsverlaufs, um die einer bestimmten Ausführung zugeordnete Anlage anzuzeigen. Suchen Sie in der obigen Liste nach einer Ausführungs-ID einer bestimmten Ausführung.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Verwenden Sie den folgenden Befehl, um die Artefakte einer Ausführung herunterzuladen:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>Schritt 6: Höherstufen der Artefakte einer Ausführung 
Da eine der Ausführungen einen besseren AUC-Wert besitzt, wird diese Ausführung verwendet, um einen Bewertungswebdienst für die Bereitstellung in der Produktion zu erstellen. Dazu müssen zunächst die Artefakte zu einem Asset höher gestuft werden.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Dadurch wird im Projektverzeichnis ein Ordner `assets` mit einer Datei `model.pkl.link` erstellt. Diese Linkdatei wird verwendet, um auf ein höher gestuftes Asset zu verweisen.

## <a name="step-7-download-the-files-to-be-operationalized"></a>Schritt 7. Herunterladen der Dateien, die operationalisiert werden sollen
Laden Sie das höher gestufte Modell herunter, um die Dateien zum Erstellen eines Vorhersagewebdiensts verwenden zu können. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-set-up-your-model-management-environment"></a>Schritt 8: Einrichten der Modellverwaltungsumgebung 
Erstellen Sie eine Umgebung zur Bereitstellung von Webdiensten. Der Webdienst kann mithilfe von Docker auf dem lokalen Computer ausgeführt werden. Für hoch skalierbare Vorgänge können Sie ihn auch in einem ACS-Cluster bereitstellen. 

```azure-cli
# Create new local operationalization environment
$ az ml env setup -l <supported Azure region> -n <env name>
# Once setup is complete, set your environment for current context
$ az ml env set -g <resource group name> -n <env name>
```

## <a name="step-9-create-a-model-management-account"></a>Schritt 9 Erstellen eines Modellverwaltungskontos 
Zum Bereitstellen und Nachverfolgen Ihrer Modelle in der Produktion ist ein Konto für die Modellverwaltung erforderlich. 

```azure-cli
$ az ml account modelmanagement create -n <model management account name> -g <resource group name> -l <supported Azure region>
```

## <a name="step-10-create-a-web-service"></a>Schritt 10 Erstellen eines Webdiensts
Erstellen Sie einen Webdienst, der auf der Grundlage des bereitgestellten Modells eine Vorhersage zurückgibt. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score_iris.py -r python –n <web service name>
```

## <a name="step-11-run-the-web-service"></a>Schritt 11 Ausführen des Webdiensts
Verwenden Sie die Webdienst-ID aus der Ausgabe des vorherigen Schritts, um den Webdienst aufzurufen und zu testen. 

```azure-cli
# Get web service usage information 
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="step-12-deleting-all-the-resources"></a>Schritt 12 Löschen aller Ressourcen 
Zum Abschluss dieses Tutorials löschen wir alle erstellten Ressourcen (es sei denn, sie möchten die Ressourcen weiter verwenden). 

Löschen Sie dazu einfach die Ressourcengruppe mit den Ressourcen. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie Azure Machine Learning für Folgendes verwenden: 
> [!div class="checklist"]
> * Einrichten eines Experimentieren-Kontos und Erstellen eines Arbeitsbereichs
> * Erstellen von Projekten
> * Übermitteln von Experimenten an mehrere Computeziele
> * Höherstufen und Registrieren eines trainierten Modells
> * Erstellen eines Kontos für die Modellverwaltung
> * Erstellen einer Umgebung für die Bereitstellung von Webdiensten
> * Bereitstellen eines Webdiensts und Bewerten mit neuen Daten

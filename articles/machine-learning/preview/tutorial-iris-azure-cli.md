---
title: "Tutorial für Funktionen der Azure Machine Learning-Vorschauversion – Befehlszeilenschnittstelle | Microsoft-Dokumentation"
description: "Erläuterungen zu allen erforderlichen Schritte für eine lückenlose Iris-Klassifizierung über die Befehlszeilenschnittstelle"
services: machine-learning
author: ahgyger
ms.author: ahgyger, ritbhat
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 10/15/2017
ms.openlocfilehash: 453c774c97b77dd7829a50fa5e5668d06f817a1d
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2017
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Tutorial: Klassifizieren von Iris über die Befehlszeilenschnittstelle
Azure Machine Learning-Dienste (Vorschau) sind eine integrierte End-to-End-Lösung für den Bereich Data Science und Advanced Analytics, mit der professionelle Datenanalysten Daten vorbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

In diesem Tutorial wird erklärt, wie Sie über die CLI-Tools (Command-Line Interface, Befehlszeilenschnittstelle) der Funktionen der Azure Machine Learning-Vorschauversion folgende Aktionen ausführen können: 
> [!div class="checklist"]
> * Einrichten eines Experimentieren-Kontos und Erstellen eines Arbeitsbereichs
> * Erstellen eines Projekts
> * Übermitteln eines Experiments an mehrere Computeziele
> * Höherstufen und Registrieren eines trainierten Modells
> * Bereitstellen eines Webdiensts zur Bewertung von neuen Daten

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
- Sie benötigen Zugriff auf ein Azure-Abonnement und die Berechtigungen zum Erstellen von Ressourcen in diesem Abonnement. 
- Installieren Sie die Azure Machine Learning Workbench-Anwendung, indem Sie den Anweisungen im [Schnellstart zum Installieren und Erstellen](quickstart-installation.md) folgen. 

  >[!NOTE]
  >Sie müssen Azure Machine Learning Workbench nur lokal installieren. Sie müssen nur die Schritte in den Abschnitten zum „Installieren von Azure Machine Learning Workbench“ ausführen. Das Erstellen des Kontos und eines neuen Projekts erfolgen in diesem Artikel über die Befehlszeile.
 
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
>Sie können die Azure-CLI in Ihrem bevorzugten Terminalfenster aktivieren, indem Sie die oben genannten Umgebungsvariablen festlegen.

## <a name="step-1-log-in-to-azure"></a>Schritt 1: Anmelden an Azure
Öffnen Sie zunächst die CLI über die AMLWorkbench-App („Datei > Eingabeaufforderung öffnen“). Dadurch wird sichergestellt, dass Sie die richtige Python-Umgebung verwenden und dass die ML-CLI-Befehle verfügbar sind. 

Legen Sie anschließend den richtigen Kontext in Ihrer CLI für den Zugriff und die Verwaltung von Azure-Ressourcen fest.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>Schritt 2: Erstellen eines neuen Experimentieren-Kontos und eines Arbeitsbereichs für Azure Machine Learning
Erstellen Sie zunächst ein neues Experimentieren-Konto und einen neuen Arbeitsbereich. Weitere Informationen zu Experimentieren-Konten und Arbeitsbereichen finden Sie unter [Azure Machine Learning-Konzepte](overview-general-concepts.md).

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
Hier wird erklärt, wie Sie einen Arbeitsbereich für einen Kollegen freigeben können. Die Vorgehensweise für die Freigabe eines Experimentieren-Kontos oder eines Projekts ist dieselbe. Sie müssen in diesem Fall nur die Methode aktualisieren, mit der die Azure-Ressourcen-ID abgerufen wird.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> `bob@contoso.com` im obigen Befehl muss eine gültige Azure AD-Identität in dem Verzeichnis sein, zu dem das aktuelle Abonnement gehört.

## <a name="step-3-create-a-new-project"></a>Schritt 3. Erstellen eines neuen Projekts
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
In diesem Beispiel erstellen wir ein neues Projekt mit einem Beispielprojekt als Vorlage.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Nachdem das Projekt erstellt wurde, verwenden Sie den Befehl `cd`, um zum Projektverzeichnis zu wechseln.

## <a name="step-4-run-the-training-experiment"></a>Schritt 4: Ausführen des Trainingsexperiments 
Bei den folgenden Schritten wird davon ausgegangen, dass Sie über ein Projekt mit dem Beispiel „Iris“ verfügen (Information hierzu finden Sie unter [Erstellen eines neuen Projekts über ein Onlinebeispiel](#sample_create)).

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
Mit dem oben genannten Befehl wird eine Liste mit allen Ausführungen angezeigt, die zu diesem Projekt gehören. Dabei werden auch Metriken zur Genauigkeit und den Regularisierungsraten aufgeführt. Damit können Sie ganz leicht die beste Ausführung in der Liste identifizieren.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>Schritt 5a: Anzeigen der Anlage, die bei einer bestimmten Ausführung erstellt wurde 
Verwenden Sie den Befehl „Info“ im Ausführungsverlauf, um die einer bestimmten Ausführung zugeordnete Anlage anzuzeigen. Suchen Sie in der obigen Liste nach einer Ausführungs-ID einer bestimmten Ausführung.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Verwenden Sie den folgenden Befehl, um die Artefakte einer Ausführung herunterzuladen:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>Schritt 6: Höherstufen der Artefakte einer Ausführung 
Da eine der Ausführungen einen besseren AUC-Wert besitzt, wird diese Ausführung verwendet, um einen Bewertungswebdienst für die Bereitstellung in der Produktion zu erstellen. Dafür müssen zunächst die Artefakte in ein Asset höhergestuft werden.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Dadurch wird im Projektverzeichnis ein Ordner `assets` mit einer Datei `model.pkl.link` erstellt. Diese Linkdatei wird verwendet, um auf ein höher gestuftes Asset zu verweisen.

## <a name="step-7-download-the-files-to-be-operationalized"></a>Schritt 7. Herunterladen der Dateien, die operationalisiert werden sollen
Laden Sie das höher gestufte Modell herunter, und verwenden Sie es zum Erstellen des Vorhersagewebdiensts. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-setup-your-model-management-environment"></a>Schritt 8: Einrichten der Modellverwaltungsumgebung 
Erstellen Sie eine Umgebung zur Bereitstellung von Webdiensten. Der Webdienst kann auf dem lokalen Computer mithilfe von Docker ausgeführt werden. Für hoch skalierbare Vorgänge können Sie ihn auch in einem ACS-Cluster bereitstellen. 

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
Erstellen Sie anschließend einen Webdienst, der eine Vorhersage mithilfe des bereitgestellten Modells zurückgibt. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score.py -r python –n <web service name>
```

## <a name="step-10-run-the-web-service"></a>Schritt 10 Ausführen des Webdiensts
Mit der Webdienst-ID aus der Ausgabe des vorherigen Schritts können Sie den Webdienst aufrufen und testen. 

```azure-cli
# Get web service usage infomration
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="deleting-all-the-resources"></a>Löschen aller Ressourcen 
Wenn Sie dieses Tutorial beendet haben, löschen Sie alle erstellten Ressourcen. Sie können die Ressourcen natürlich auch behalten, wenn Sie weiter mit ihnen arbeiten möchten. 

Löschen Sie dazu einfach die Ressourcengruppe, die alle Ressourcen enthält. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurde erklärt, wie Sie die Funktionen der Azure Machine Learning-Vorschauversion für die folgenden Aktionen verwenden: 
> [!div class="checklist"]
> * Einrichten eines Experimentieren-Kontos und Erstellen eines Arbeitsbereichs
> * Erstellen von Projekten
> * Übermitteln von Experimenten an mehrere Computeziele
> * Höherstufen und Registrieren eines trainierten Modells
> * Erstellen eines Kontos für die Modellverwaltung
> * Erstellen einer Umgebung zur Bereitstellung eines Webdiensts
> * Bereitstellen eines Webdiensts und Bewerten mit neuen Daten
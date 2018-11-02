---
title: Handbuch zur Problembehandlung bei der Bereitstellung des Azure Machine Learning-Diensts
description: Erfahren Sie, wie Sie die häufigen Docker-Bereitstellungsfehler für den Azure Machine Learning-Dienst umgehen, lösen und beheben können.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: jmartens
ms.date: 10/01/2018
ms.openlocfilehash: a10b05e95fa719b80775191e48bd4117e3a785fd
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321681"
---
# <a name="troubleshooting-azure-machine-learning-service-deployments"></a>Problembehandlung von Bereitstellungen des Azure Machine Learning-Diensts

In diesem Artikel erfahren Sie, wie Sie die häufigen Docker-Bereitstellungsfehler für den Azure Machine Learning-Dienst umgehen oder lösen.

Bei der Bereitstellung eines Modells im Azure Machine Learning-Dienst führt das System eine Reihe von Aufgaben aus. Hierbei handelt es sich um eine komplexe Abfolge von Ereignissen, und manchmal treten Probleme auf. Dies sind die Aufgaben bei der Bereitstellung:

1. Registrieren des Modells in der Modellregistrierung des Arbeitsbereichs.

2. Erstellen eines Docker-Images, einschließlich:
    1. Herunterladen des registrierten Modells aus der Registry. 
    2. Erstellen eines Dockerfiles mit einer Python-Umgebung auf der Grundlage der Abhängigkeiten, die Sie in der YAML-Datei der Umgebung angeben.
    3. Hinzufügen Ihrer Modelldateien und des Bewertungsskripts, das Sie im Dockerfile bereitstellen.
    4. Erstellen eines neuen Docker-Images mithilfe des Dockerfiles.
    5. Registrieren des Docker-Images bei der Azure Container Registry, die dem Arbeitsbereich zugeordnet ist.

3. Bereitstellen des Docker-Images im ACI-Dienst (Azure Container Instance) oder in AKS (Azure Kubernetes Service).

4. Starten eines neuen Containers (oder mehrerer Container) in ACI oder AKS. 

Weitere Informationen über diesen Prozess finden Sie in der Einführung zur [Modellverwaltung](concept-model-management-and-deployment.md).

## <a name="before-you-begin"></a>Voraussetzungen

Beim Auftreten eines Problems besteht der erste Schritt darin, die (zuvor beschriebene) Bereitstellungsaufgabe in einzelne Schritte aufzuschlüsseln, um das Problem zu isolieren. 

Dies ist insbesondere nützlich, wenn Sie die `Webservice.deploy`-API oder `Webservice.deploy_from_model`-API verwenden, da diese Funktionen die zuvor erwähnten Schritte zu einer einzelnen Aktion zusammenfassen. Im Normalfall sind diese APIs ziemlich komfortabel, bei der Problembehandlung ist es aber hilfreich, die Schritte aufzuschlüsseln, indem Sie sie durch die unten angegebenen API-Aufrufe ersetzen.

1. Registrieren des Modells. Hier finden Sie Beispielcode dazu:

    ```python
    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Erstellen des Images. Hier finden Sie Beispielcode dazu:

    ```python
    # configure the image
    image_config = ContainerImage.image_configuration(runtime="python",
                                                      execution_script="score.py",
                                                      conda_file="myenv.yml")

    # create the image
    image = Image.create(name='myimg', models=[model], image_config=image_config, workspace=ws)

    # wait for image creation to finish
    image.wait_for_creation(show_output=True)
    ```

3. Bereitstellen des Images als Dienst. Hier finden Sie Beispielcode dazu:

    ```python
    # configure an ACI-based deployment
    aci_config = AciWebservice.deploy_configuration(cpu_cores=1, memory_gb=1)

    aci_service = Webservice.deploy_from_image(deployment_config=aci_config, 
                                               image=image, 
                                               name='mysvc', 
                                               workspace=ws)
    aci_service.wait_for_deployment(show_output=True)    
    ```

Nachdem Sie den Bereitstellungsvorgang in einzelne Aufgaben aufgeschlüsselt haben, können wir uns einige der häufigsten Fehler ansehen.

## <a name="image-building-fails"></a>Fehler bei der Image-Erstellung
Wenn das System nicht in der Lage ist, das Docker-Image zu erstellen, tritt beim Aufruf `image.wait_for_creation()` ein Fehler mit einigen Fehlermeldungen auf, die ein paar Hinweise geben können. Ferner finden Sie weitere Details zu den Fehlern in den Erstellungsprotokollen des Images. Unten sehen Sie etwas Beispielcode, aus dem Sie ersehen können, wie Sie den URI des Image-Erstellungsprotokolls ermitteln können.

```python
# if you already have the image object handy
print(image.image_build_log_uri)

# if you only know the name of the image (note there might be multiple images with the same name but different version number)
print(ws.images()['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images().items():
    print (img.name, img.version, img.image_build_log_uri)
```
Der URI des Imageprotokolls ist eine SAS-URL, die auf eine Protokolldatei verweist, die in Ihrem Azure Blob Storage gespeichert ist. Kopieren Sie einfach den URI, und fügen Sie ihn in ein Browserfenster ein, dann können Sie die Protokolldatei herunterladen und anzeigen.


## <a name="service-launch-fails"></a>Fehler beim Starten des Diensts
Nach der erfolgreichen Erstellung versucht das System – abhängig von Ihrer Bereitstellungskonfiguration – einen Container in ACI oder AKS zu starten. Es empfiehlt sich allgemein, zuerst eine ACI-Bereitstellung zu versuchen, da es sich dabei um die einfachere Einzelcontainerbereitstellung handelt. Auf diese Weise können Sie dabei alle AKS-spezifischen Probleme ausschließen.

Als Teil des Container-Startprozesses wird die `init()`-Funktion in Ihrem Bereitstellungsskript vom System aufgerufen. Wenn in der `init()`-Funktion nicht abgefangene Ausnahmen auftreten, ist in der Fehlermeldung möglicherweise ein **CrashLoopBackOff**-Fehler zu finden. Unten finden Sie einige Tipps zur Behandlung des Problems.

### <a name="inspect-the-docker-log"></a>Untersuchen des Docker-Protokolls
Das Dienstobjekt erlaubt die Ausgabe von detaillierten Protokollnachrichten der Docker-Engine.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices()['mysvc'].get_logs())
```

### <a name="debug-the-docker-image-locally"></a>Lokales Debuggen des Docker-Images
Manchmal gibt das Docker-Protokoll nicht genügend Informationen dazu her, was schief läuft. Sie können einen Schritt weiter gehen, das erstellte Docker-Image herunterladen und direkt interaktiv im Live-Container debuggen. Um einen lokalen Container zu starten, muss eine Docker-Engine lokal ausgeführt werden, und es erleichtert Ihnen die Arbeit erheblich, wenn Sie außerdem [azure-cli](/cli/azure/install-azure-cli?view=azure-cli-latest) installiert haben.

Zunächst müssen wir den Speicherort des Images ermitteln:

```python
# print image location
print(image.image_location)
```

Der Speicherort des Images weist dieses Format auf: `<acr-name>.azurecr.io/<image-name>:<version-number>`, wie etwa `myworkpaceacr.azurecr.io/myimage:3`. 

Wechseln Sie jetzt zum Befehlszeilenfenster. Wenn azure-cli auf Ihrem System installiert ist, können Sie die folgenden Befehle eingeben, um sich bei der ACR (Azure Container Registry) anzumelden, die dem Arbeitsbereich zugeordnet ist, in dem das Image gespeichert ist. 

```sh
# log on to Azure first if you haven't done so before
$ az login

# make sure you set the right subscription in case you have access to multiple subscriptions
$ az account set -s <subscription_name_or_id>

# now let's log in to the workspace ACR
# note the acr-name is the domain name WITHOUT the ".azurecr.io" postfix
# e.g.: az acr login -n myworkpaceacr
$ az acr login -n <acr-name>
```
Wenn azure-cli nicht installiert ist, können Sie den Befehl `docker login` für die Anmeldung bei der ACR verwenden. Allerdings müssen Sie zuerst den Benutzernamen und das Kennwort der ACR im Azure-Portal abrufen.

Nach der Anmeldung bei der ACR können Sie das Docker-Image herunterladen, lokal einen Container starten und dann mithilfe des Befehls `docker run` eine Bash-Sitzung zum Debuggen starten:

```sh
# note the image_id is <acr-name>.azurecr.io/<image-name>:<version-number>
# for example: myworkpaceacr.azurecr.io/myimage:3
$ docker run -it <image_id> /bin/bash
```

Nachdem Sie eine Bash-Sitzung im laufenden Container gestartet haben, finden Sie Ihre Bewertungsskripts im Ordner `/var/azureml-app`. Anschließend können Sie eine Python-Sitzung zum Debuggen Ihrer Bewertungsskripts starten. 

```sh
# enter the directory where scoring scripts live
cd /var/azureml-app

# find what Python packages are installed in the python environment
pip freeze

# sanity-check on score.py
# you might want to edit the score.py to trigger init().
# as most of the errors happen in init() when you are trying to load the model.
python score.py
```
Falls Sie einen Text-Editor zum Bearbeiten Ihrer Skripts benötigen, können Sie vim, nano, Emacs oder einen anderen bevorzugten Editor installieren.

```sh
# update package index
apt-get update

# install a text editor of your choice
apt-get install vim
apt-get install nano
apt-get install emacs

# launch emacs (for example) to edit score.py
emacs score.py

# exit the container bash shell
exit
```

Sie können auch den Webdienst lokal starten und HTTP-Verkehr an ihn senden. Der Flask-Server im Docker-Container wird an Port 5001 ausgeführt. Sie können ihn beliebigen anderen Ports zuordnen, die auf dem Hostcomputer verfügbar sind.
```sh
# you can find the scoring API at: http://localhost:8000/score
$ docker run -p 8000:5001 <image_id>
```

## <a name="function-fails-getmodelpath"></a>Fehler bei der Funktion: get_model_path()
Oftmals wird in der `init()`-Funktion im Bewertungsskript die Funktion `Model.get_model_path()` aufgerufen, um eine Modelldatei oder einen Ordner mit Modelldateien im Container zu finden. Dies stellt eine häufige Fehlerquelle dar, wenn die Modelldatei oder der Ordner nicht gefunden werden. Die einfachste Möglichkeit zum Debuggen dieses Fehlers besteht darin, den unten dargestellten Python-Code in der Containershell auszuführen:

```python
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Dadurch würde der lokale Pfad (relativ zu `/var/azureml-app`) im Container ausgegeben, unter dem Ihr Bewertungsskript die Modelldatei oder den Modellordner erwartet. Dann können Sie überprüfen, ob sich die Datei oder der Ordner wirklich dort befinden, wo sie erwartet werden.


## <a name="function-fails-runinputdata"></a>Fehler bei der Funktion: run(input_data)
Wenn der Dienst erfolgreich bereitgestellt wurde, aber beim Veröffentlichen von Daten am Bewertungsendpunkt abstürzt, können Sie Ihrer `run(input_data)`-Funktion eine Anweisung zum Abfangen von Fehlern hinzufügen, damit sie stattdessen eine detaillierte Fehlermeldung zurückgibt. Beispiel: 

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```
**Hinweis**: Das Zurückgeben von Fehlermeldungen aus dem Aufruf `run(input_data)` sollte nur zu Debugzwecken erfolgen. Aus Sicherheitsgründen empfiehlt sich dieses Vorgehen nicht in Produktionsumgebungen.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung finden Sie hier: 
* [Bereitstellen für ACI](how-to-deploy-to-aci.md)

* [Bereitstellen für AKS](how-to-deploy-to-aks.md)

* [Tutorial Teil 1: Trainieren des Modells](tutorial-train-models-with-aml.md)

* [Tutorial Teil 2: Bereitstellen des Modells](tutorial-deploy-models-with-aml.md)

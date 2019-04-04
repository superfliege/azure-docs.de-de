---
title: Leitfaden zur Problembehandlung bei der Bereitstellung
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie gängige Docker-Bereitstellungsfehler mit AKS und ACI mithilfe von Azure Machine Learning Service umgehen, lösen und beheben können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: chris-lauren
ms.author: clauren
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 815be7400e0a0560ace7e07b317aeb25c2feacd5
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57450973"
---
# <a name="troubleshooting-azure-machine-learning-service-aks-and-aci-deployments"></a>Problembehandlung von Bereitstellungen von Azure Machine Learning Service mit AKS und ACI

In diesem Artikel erfahren Sie, wie Sie die häufigsten Docker-Bereitstellungsfehler mit Azure Container Instances (ACI) und Azure Kubernetes Service (AKS) mit Azure Machine Learning Service umgehen oder beheben können.

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

Dies ist nützlich, wenn Sie die `Webservice.deploy`-API oder `Webservice.deploy_from_model`-API verwenden, da diese Funktionen die zuvor erwähnten Schritte zu einer einzelnen Aktion zusammenfassen. Im Normalfall sind diese APIs praktisch, bei der Problembehandlung ist es jedoch hilfreich, die Schritte aufzuschlüsseln, indem Sie sie durch die unten angegebenen API-Aufrufe ersetzen.

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
print(ws.images['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images.items():
    print (img.name, img.version, img.image_build_log_uri)
```
Der URI des Imageprotokolls ist eine SAS-URL, die auf eine Protokolldatei verweist, die in Ihrem Azure Blob Storage gespeichert ist. Kopieren Sie einfach den URI, und fügen Sie ihn in ein Browserfenster ein, dann können Sie die Protokolldatei herunterladen und anzeigen.

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure Key Vault-Zugriffsrichtlinie und Azure Resource Manager-Vorlagen

Die Imageerstellung kann auch aufgrund eines Problems mit der Zugriffsrichtlinie für den Azure Key Vault fehlschlagen. Dies kann auftreten, wenn Sie eine Azure Resource Manager-Vorlage mehrmals zum Erstellen des Arbeitsbereichs und der zugehörigen Ressourcen (einschließlich Azure Key Vault) verwenden. Dies ist beispielsweise der Fall, wenn Sie die Vorlage mehrere Male mit den gleichen Parametern in einer Continuous Integration- und Deployment-Pipeline verwenden.

Die meisten Vorgänge zum Erstellen von Ressourcen mit Vorlagen sind idempotent, Key Vault löscht die Zugriffsrichtlinien jedoch jedes Mal, wenn die Vorlage verwendet wird. Damit wird der Zugriff auf den Schlüsseltresor für vorhandene Arbeitsbereiche unterbrochen, die ihn verwenden. Dies führt zu Fehlern, wenn Sie versuchen, neue Images zu erstellen. Im Folgenden finden Sie Beispiele für Fehler, die angezeigt werden können:

__Portal__:
```text
Create image "myimage": An internal server error occurred. Please try again. If the problem persists, contact support.
```

__SDK:__
```python
image = ContainerImage.create(name = "myimage", models = [model], image_config = image_config, workspace = ws)
Creating image
Traceback (most recent call last):
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 341, in create
    resp.raise_for_status()
  File "C:\Python37\lib\site-packages\requests\models.py", line 940, in raise_for_status
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 500 Server Error: Internal Server Error for url: https://eastus.modelmanagement.azureml.net/api/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>/images?api-version=2018-11-19

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 346, in create
    'Content: {}'.format(resp.status_code, resp.headers, resp.content))
azureml.exceptions._azureml_exception.WebserviceException: Received bad response from Model Management Service:
Response Code: 500
Headers: {'Date': 'Tue, 26 Feb 2019 17:47:53 GMT', 'Content-Type': 'application/json', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'api-supported-versions': '2018-03-01-preview, 2018-11-19', 'x-ms-client-request-id': '3cdcf791f1214b9cbac93076ebfb5167', 'x-ms-client-session-id': '', 'Strict-Transport-Security': 'max-age=15724800; includeSubDomains; preload'}
Content: b'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}'
```

__CLI__:
```text
ERROR: {'Azure-cli-ml Version': None, 'Error': WebserviceException('Received bad response from Model Management Service:\nResponse Code: 500\nHeaders: {\'Date\': \'Tue, 26 Feb 2019 17:34:05
GMT\', \'Content-Type\': \'application/json\', \'Transfer-Encoding\': \'chunked\', \'Connection\': \'keep-alive\', \'api-supported-versions\': \'2018-03-01-preview, 2018-11-19\', \'x-ms-client-request-id\':
\'bc89430916164412abe3d82acb1d1109\', \'x-ms-client-session-id\': \'\', \'Strict-Transport-Security\': \'max-age=15724800; includeSubDomains; preload\'}\nContent:
b\'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}\'',)}
```

Folgende Ansätze werden empfohlen, um dieses Problem zu umgehen:

* Stellen Sie die Vorlage nicht mehrmals mit den gleichen Parametern bereit, oder löschen Sie die vorhandenen Ressourcen, bevor Sie die Vorlage verwenden, um sie neu zu erstellen.
* Untersuchen Sie die Key Vault-Zugriffsrichtlinien, und verwenden Sie sie zum Festlegen der `accessPolicies`-Eigenschaft der Vorlage.
* Überprüfen Sie, ob die Key Vault-Ressource bereits vorhanden ist. Wenn dies der Fall ist, erstellen Sie sie nicht mithilfe der Vorlage neu. Fügen Sie beispielsweise einen Parameter hinzu, mit dem Sie die Erstellung der Key Vault-Ressource deaktivieren können, wenn sie bereits vorhanden ist.

## <a name="service-launch-fails"></a>Fehler beim Starten des Diensts
Nach der erfolgreichen Erstellung versucht das System – abhängig von Ihrer Bereitstellungskonfiguration – einen Container in ACI oder AKS zu starten. Es wird empfohlen, zuerst eine ACI-Bereitstellung auszuprobieren, da es sich dabei um eine einfachere Einzelcontainerbereitstellung handelt. Auf diese Weise können Sie dabei alle AKS-spezifischen Probleme ausschließen.

Als Teil des Container-Startprozesses wird die `init()`-Funktion in Ihrem Bereitstellungsskript vom System aufgerufen. Wenn in der `init()`-Funktion nicht abgefangene Ausnahmen auftreten, ist in der Fehlermeldung möglicherweise ein **CrashLoopBackOff**-Fehler zu finden. Unten finden Sie einige Tipps zur Behandlung des Problems.

### <a name="inspect-the-docker-log"></a>Untersuchen des Docker-Protokolls
Das Dienstobjekt erlaubt die Ausgabe von detaillierten Protokollnachrichten der Docker-Engine.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

### <a name="debug-the-docker-image-locally"></a>Lokales Debuggen des Docker-Images
Manchmal gibt das Docker-Protokoll nicht genügend Informationen dazu her, was schief läuft. Sie können einen Schritt weiter gehen, das erstellte Docker-Image herunterladen und direkt interaktiv im Live-Container debuggen. Um einen lokalen Container zu starten, muss eine Docker-Engine lokal ausgeführt werden, und es erleichtert Ihnen die Arbeit erheblich, wenn Sie außerdem [azure-cli](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) installiert haben.

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
import logging
logging.basicConfig(level=logging.DEBUG)
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Dadurch würde der lokale Pfad (relativ zu `/var/azureml-app`) im Container ausgegeben, unter dem Ihr Bewertungsskript die Modelldatei oder den Modellordner erwartet. Dann können Sie überprüfen, ob sich die Datei oder der Ordner wirklich dort befinden, wo sie erwartet werden.

Das Festlegen der Protokollierungsstufe auf DEBUG kann dazu führen, dass zusätzliche Informationen protokolliert werden, was bei der Identifizierung des Fehlers nützlich sein kann.

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
**Hinweis**: Die Rückgabe von Fehlermeldungen aus dem Aufruf `run(input_data)` sollte nur zu Debugzwecken erfolgen. Aus Sicherheitsgründen empfiehlt sich dieses Vorgehen nicht in Produktionsumgebungen.

## <a name="http-status-code-503"></a>HTTP-Statuscode 503

Azure Kubernetes Service-Bereitstellungen unterstützen die automatische Skalierung, mit der das Hinzufügen von Replikaten ermöglicht wird, um zusätzliche Last zu unterstützen. Die Autoskalierung wurde jedoch für die Behandlung **stetiger** Veränderungen der Last konzipiert. Wenn Sie große Spitzen bei den Anforderungen pro Sekunde erhalten, erhalten Clients möglicherweise den HTTP-Statuscode 503.

Es gibt zwei Möglichkeiten, die beim Verhindern des Statuscodes 503 helfen können:

* Ändern Sie den Auslastungsgrad für die Erstellung neuer Replikate durch die automatische Skalierung.
    
    Der Auslastungsgrad der automatischen Skalierung ist standardmäßig auf 70 % festgelegt, was bedeutet, dass der Dienst Spitzen bei den Anforderungen pro Sekunde (RPS) von bis zu 30 % verarbeiten kann. Sie können den Auslastungsgrad anpassen, indem Sie einen niedrigeren Wert für `autoscale_target_utilization` festlegen.

    > [!IMPORTANT]
    > Durch diese Änderung werden Replikate *nicht schneller* erstellt. Stattdessen werden sie mit einem niedrigeren Schwellenwert für die Auslastung erstellt. Anstatt abzuwarten, bis der Dienst zu 70 % ausgelastet ist, werden Replikate schon bei 30 % Auslastung erstellt, wenn Sie den Wert in 30 % ändern.
    
    Wenn der Webdienst bereits die derzeit maximale Anzahl von Replikaten verwendet und Sie weiterhin den Statuscode 503 erhalten, erhöhen Sie den `autoscale_max_replicas`-Wert, um die maximale Anzahl der Replikate zu erhöhen.

* Ändern Sie die Mindestanzahl der Replikate. Indem Sie die Mindestanzahl der Replikate erhöhen, wird ein größerer Pool für die Verarbeitung eingehender Spitzen bereitgestellt.

    Legen Sie einen höheren Wert für `autoscale_min_replicas` fest, um die Mindestanzahl der Replikate zu erhöhen. Sie können die erforderliche Anzahl von Replikaten berechnen, indem Sie den folgenden Code verwenden und die Werte durch die Werten Ihres Projekts ersetzen:

    ```python
    from math import ceil
    # target requests per second
    targetRps = 20
    # time to process the request (in seconds)
    reqTime = 10
    # Maximum requests per container
    maxReqPerContainer = 1
    # target_utilization. 70% in this example
    targetUtilization = .7

    concurrentRequests = targetRps * reqTime / targetUtilization

    # Number of container replicas
    replicas = ceil(concurrentRequests / maxReqPerContainer)
    ```

    > [!NOTE]
    > Wenn Anforderungsspitzen eingehen, die die neue Mindestanzahl von Replikaten überschreiten, erhalten Sie möglicherweise wieder den Statuscode 503. Wenn sich der Datenverkehr Ihres Diensts beispielsweise erhöht, müssen Sie die Mindestanzahl von Replikaten möglicherweise erhöhen.

Weitere Informationen zum Festlegen von `autoscale_target_utilization`, `autoscale_max_replicas` und `autoscale_min_replicas` finden Sie in der Modulreferenz zu [AksWebservice](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung finden Sie hier: 
* [Bereitstellung: wie und wo?](how-to-deploy-and-where.md)

* [Tutorial: Trainieren und Bereitstellen von Modellen](tutorial-train-models-with-aml.md)

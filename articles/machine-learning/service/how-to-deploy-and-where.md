---
title: Bereitstellen von Modellen als Webdienste
titleSuffix: Azure Machine Learning service
description: 'Erfahren Sie, wie und wo Sie Ihre Azure Machine Learning Service-Modelle bereitstellen, wie z. B.: Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge und Field Programmable Gate Arrays.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 0c171ff768395540c123c4ef2a19168d926b0661
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633826"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst

Der Azure Machine Learning Service bietet mehrere Möglichkeiten zum Bereitstellen Ihres trainierten Modells mithilfe des Software Development Kits. In diesem Dokument erfahren Sie, wie Sie Ihr Modell als Webdienst in der Azure-Cloud oder auf IoT Edge-Geräten bereitstellen.

Sie können Modelle an folgenden Computezielen bereitstellen:

| Computeziel | Bereitstellungstyp | BESCHREIBUNG |
| ----- | ----- | ----- |
| [Azure Container Instances (ACI)](#aci) | Webdienst | Schnelle Bereitstellung. Gut geeignet für Entwicklungs- und Testzwecke. |
| [Azure Kubernetes Service (AKS)](#aks) | Webdienst | Geeignet für hochgradig skalierbare Produktionsbereitstellungen. Bietet automatische Skalierung und schnelle Reaktionszeiten. |
| [Azure IoT Edge](#iotedge) | IoT-Modul | Stellen Sie Modelle auf IoT-Geräten bereit. Rückschlüsse erfolgen auf dem Gerät. |
| [Field Programmable Gate Array (FPGA)](#fpga) | Webdienst | Extrem geringe Wartezeit für die Interferenzierung in Echtzeit. |

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning Service-Arbeitsbereich und das Azure Machine Learning SDK für Python müssen installiert sein. Unter [Schnellstart: erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md) erfahren Sie, wie Sie diese Komponenten installieren.

- Ein trainiertes Modell im Format Pickle (`.pkl`) oder ONNX (`.onnx`). Wenn Sie nicht über ein trainiertes Modell verfügen, verwenden Sie die Schritte im Tutorial [Train models (Trainieren von Modellen)](tutorial-train-models-with-aml.md), um ein Modell zu trainieren und beim Azure Machine Learning Service zu registrieren.

- In den Codeabschnitten wird angenommen, dass `ws` auf Ihren Machine Learning-Arbeitsbereich verweist. Beispiel: `ws = Workspace.from_config()`.

## <a name="deployment-workflow"></a>Bereitstellungsworkflow

Der Prozess zur Bereitstellung eines Modells ist für alle Computeziele ähnlich:

1. Trainieren des Modells.
1. Registrieren des Modells.
1. Erstellen einer Imagekonfiguration.
1. Erstellen Sie das Image.
1. Bereitstellen des Image für ein Computeziel.
1. Testen der Bereitstellung
1. (Optional) Bereinigen von Artefakten.

    * Bei der **Bereitstellung als Webdienst** gibt es drei Bereitstellungsoptionen:

        * [deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-): Wenn Sie diese Methode verwenden, ist es nicht erforderlich, das Modell zu registrieren oder das Image zu erstellen. Sie können jedoch nicht den Namen des Modells oder Images oder die zugeordneten Tags und Beschreibungen steuern.
        * [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-): Wenn Sie diese Methode verwenden, müssen Sie kein Image erstellen. Sie haben jedoch keine Kontrolle über den Namen des erstellten Images.
        * [deploy_from_image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-): Registrieren Sie das Modell, und erstellen Sie ein Image, bevor Sie diese Methode verwenden.

        In den Beispielen in diesem Dokument wird `deploy_from_image` verwendet.

    * Bei der **Bereitstellung als IoT Edge-Modul** müssen Sie das Modell registrieren und das Image erstellen.

## <a name="register-a-model"></a>Registrieren eines Modells

Nur trainierte Modelle können bereitgestellt werden. Das Modell kann mit Azure Machine Learning oder einem anderen Dienst trainiert werden. Verwenden Sie den folgenden Code, um ein Modell aus einer Datei zu registrieren:

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl",
                       model_name = "Mymodel",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

> [!NOTE]
> Zwar zeigt das Beispiel ein Modell, das als Pickle-Datei gespeichert wurde, Sie können aber auch ONNX-Modelle verwenden. Weitere Informationen zur Verwendung von ONNX-Modellen finden Sie im Dokument [ONNX und Azure Machine Learning (ONNX und Azure Machine Learning)](how-to-build-deploy-onnx.md).

Weitere Informationen finden Sie in der Referenzdokumentation zur [Modellklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Erstellen einer Imagekonfiguration

Bereitgestellte Modelle werden als Image gepackt. Das Image enthält die notwendigen Abhängigkeiten zum Ausführen des Modells.

Für Bereitstellungen von **Azure Container Instance**, **Azure Kubernetes Service** und **Azure IoT Edge** wird die Klasse `azureml.core.image.ContainerImage` verwendet, um eine Imagekonfiguration zu erstellen. Diese Imagekonfiguration wird dann verwendet, um ein neues Docker-Image zu erstellen. 

Der folgende Code zeigt, wie eine neue Imagekonfiguration erstellt wird:

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml",
                                                 description = "Image with ridge regression model",
                                                 tags = {"data": "diabetes", "type": "regression"}
                                                 )
```

Diese Konfiguration verwendet eine `score.py`-Datei, um Anforderungen an das Modell weiterzuleiten. Diese Datei umfasst zwei Funktionen:

* `init()`: Diese Funktion lädt üblicherweise das Modell in ein globales Objekt. Diese Funktion wird nur dann ausgeführt, wenn der Docker-Container gestartet wird. 

* `run(input_data)`: Diese Funktion verwendet das Modell zur Vorhersage eines Werts, der auf den Eingabedaten basiert. Ein- und Ausgaben zur Ausführung verwenden in der Regel JSON für die Serialisierung und Deserialisierung, allerdings werden auch andere Formate unterstützt.

Ein Beispiel einer `score.py`-Datei finden Sie im [Tutorial zur Bildklassifizierung](tutorial-deploy-models-with-aml.md#make-script). Ein Beispiel, in dem ein ONNX-Modell verwendet wird, finden Sie im Dokument [ONNX and Azure Machine Learning (ONNX und Azure Machine Learning)](how-to-build-deploy-onnx.md).

Der Parameter `conda_file` wird verwendet, um eine Conda-Umgebungsdatei bereitzustellen. Diese Datei definiert die Conda-Umgebung für das bereitgestellte Modell. Weitere Informationen zum Erstellen dieser Datei finden Sie unter [Create an environment file (myenv.yml) (Erstellen einer Umgebungsdatei (myenv.yml))](tutorial-deploy-models-with-aml.md#create-environment-file).

Weitere Informationen finden Sie in der Referenzdokumentation zur [ContainerImage-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a id="createimage"></a> Erstellen des Image

Nachdem Sie die Imagekonfiguration erstellt haben, können Sie diese verwenden, um ein Image zu erstellen. Dieses Image wird in der Containerregistrierung für Ihren Arbeitsbereich gespeichert. Nach der Erstellung können Sie dasselbe Image für mehrere Dienste bereitstellen.

```python
# Create the image from the image configuration
image = ContainerImage.create(name = "myimage", 
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**Geschätzter Zeitaufwand**: Ca. 3 Minuten.

Images werden automatisch mit einer Versionsangabe versehen, wenn Sie mehrere Images mit dem gleichen Namen registrieren. Dem ersten Bild beispielsweise, das als `myimage` registriert ist, wird die ID `myimage:1` zugewiesen. Wenn Sie das nächste Mal ein Bild als `myimage` registrieren, lautet die ID des neuen Image `myimage:2`.

Weitere Informationen finden Sie in der Referenzdokumentation zur [ContainerImage-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a name="deploy-the-image"></a>Bereitstellen des Image

Je nach dem Computeziel, auf dem Sie die Bereitstellung vornehmen, unterscheidet sich der Bereitstellungsprozess etwas. Anhand der Informationen in den folgenden Abschnitten erfahren Sie mehr zur Bereitstellung auf folgenden Zielen:

* [Azure Container Instances](#aci)
* [Azure Kubernetes Services](#aks)
* [Project Brainwave (Field Programmable Gate Arrays)](#fpga)
* [Azure IoT Edge-Geräte](#iotedge)

### <a id="aci"></a> Bereitstellen in Azure Container Instances

Verwenden Sie Azure Container Instances für die Bereitstellung Ihrer Modelle als Webdienst, wenn mindestens eine der folgenden Bedingungen zutrifft:

- Sie müssen Ihr Modell schnell bereitstellen und überprüfen. Eine ACI-Bereitstellung ist in weniger als 5 Minuten abgeschlossen.
- Sie testen ein Modell in der Entwicklungsphase. Um Kontingente und die Regionsverfügbarkeit für ACI anzuzeigen, beachten Sie das Dokument [Kontingente und Regionsverfügbarkeit für Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas).

Verwenden Sie die folgenden Schritte für Bereitstellungen in Azure Container Instances:

1. Definieren Sie die Bereitstellungskonfiguration. Das folgende Beispiel definiert eine Konfiguration, die einen CPU-Kern und 1 GB Speicher verwendet:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Um das Image bereitzustellen, das im Abschnitt [Erstellen des Image](#createimage) dieses Dokuments erstellt wurde, verwenden Sie den folgenden Code:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Geschätzter Zeitaufwand**: Ca. 3 Minuten.

    > [!TIP]
    > Wenn während der Bereitstellung Fehler auftreten, können Sie die AKS-Dienstprotokolle mit `service.get_logs()` anzeigen. Die protokollierten Informationen weisen eventuell auf die Ursache des Fehlers hin.

Weitere Informationen finden Sie in der Referenzdokumentation für die Klassen [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) und [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a> Bereitstellen für Azure Kubernetes Service

Um Ihr Modell als hochgradig skalierbaren Produktionswebdienst bereitzustellen, verwenden Sie Azure Kubernetes Service (AKS). Sie können einen vorhandenen AKS-Cluster verwenden oder mithilfe des Azure Machine Learning-SDK, der CLI oder des Azure-Portals einen neuen erstellen.

Das Erstellen eines AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Prozess. Sie können diesen Cluster für mehrere Bereitstellungen wiederverwenden. Wenn Sie den Cluster löschen, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen.

Azure Kubernetes Service bietet die folgenden Funktionen:

* Automatische Skalierung
* Protokollierung
* Modelldatensammlung
* Schnelle Reaktionszeiten für Ihre Webdienste

Führen Sie zum Bereitstellen in Azure Kubernetes Service die folgenden Schritte aus:

1. Verwenden Sie zum Erstellen eines AKS-Clusters den folgenden Code:

    > [!IMPORTANT]
    > Das Erstellen des AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Prozess. Nach der Erstellung können Sie diesen Cluster für mehrere Bereitstellungen wiederverwenden. Wenn Sie den Cluster oder die Ressourcengruppe, die ihn enthält, löschen, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen.
    > Wenn Sie für [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) benutzerdefinierte Werte für agent_count und vm_size auswählen, müssen Sie sicherstellen, dass agent_count multipliziert mit vm_size mindestens 12 virtuellen CPUs entspricht. Wenn Sie beispielsweise für vm_size „Standard_D3_v2“ verwenden, das 4 virtuelle CPUs hat, müssen Sie für agent_count einen Wert von mindestens 3 wählen.

    ```python
    from azureml.core.compute import AksCompute, ComputeTarget

    # Use the default configuration (you can also provide parameters to customize this)
    prov_config = AksCompute.provisioning_configuration()

    aks_name = 'aml-aks-1' 
    # Create the cluster
    aks_target = ComputeTarget.create(workspace = ws, 
                                        name = aks_name, 
                                        provisioning_configuration = prov_config)

    # Wait for the create process to complete
    aks_target.wait_for_completion(show_output = True)
    print(aks_target.provisioning_state)
    print(aks_target.provisioning_errors)
    ```

    **Geschätzter Zeitaufwand**: Ca. 20 Minuten.

    > [!TIP]
    > Wenn Sie in Ihrem Azure-Abonnement bereits über einen AKS-Cluster verfügen und dieser die Version 1.11.* hat, können Sie diesen für die Bereitstellung Ihres Image verwenden. Der folgende Code veranschaulicht das Anfügen eines vorhandenen Clusters an Ihren Arbeitsbereich:
    >
    > ```python
    > from azureml.core.compute import AksCompute, ComputeTarget
    > # Set the resource group that contains the AKS cluster and the cluster name
    > resource_group = 'myresourcegroup'
    > cluster_name = 'mycluster'
    > 
    > # Attatch the cluster to your workgroup
    > attach_config = AksCompute.attach_configuration(resource_group = resource_group,
    >                                          cluster_name = cluster_name)
    > aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
    > 
    > # Wait for the operation to complete
    > aks_target.wait_for_completion(True)
    > ```

2. Um das Image bereitzustellen, das im Abschnitt [Erstellen des Image](#createimage) dieses Dokuments erstellt wurde, verwenden Sie den folgenden Code:

    ```python
    from azureml.core.webservice import Webservice, AksWebservice

    # Set configuration and service name
    aks_config = AksWebservice.deploy_configuration()
    aks_service_name ='aks-service-1'
    # Deploy from image
    service = Webservice.deploy_from_image(workspace = ws, 
                                                name = aks_service_name,
                                                image = image,
                                                deployment_config = aks_config,
                                                deployment_target = aks_target)
    # Wait for the deployment to complete
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

    > [!TIP]
    > Wenn während der Bereitstellung Fehler auftreten, können Sie die AKS-Dienstprotokolle mit `service.get_logs()` anzeigen. Die protokollierten Informationen weisen eventuell auf die Ursache des Fehlers hin.

Weitere Informationen finden Sie in der Referenzdokumentation für die Klassen [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) und [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py).

### <a id="fpga"> Bereitstellen auf Field Programmable Gate Arrays (FPGAs)</a>

Mit Project Brainwave kann eine extrem kurze Wartezeit für Echtzeit-Inferenzierungsanforderungen erzielt werden. Project Brainwave beschleunigt tiefe neuronale Netzwerke (Deep Neural Networks, DNNS), die auf Field-Programmable Gate Arrays in der Azure-Cloud bereitgestellt sind. Häufig verwendete DNNs sind als Featurizer für das Lernen durch Transfer verfügbar oder können mit Gewichtungen angepasst werden, die mit Ihren eigenen Daten trainiert sind.

Eine exemplarische Vorgehensweise zur Bereitstellung eines Modells mithilfe von Project Brainwave finden Sie im Dokument [Deploy to a FPGA (Bereitstellen in einem FPGA)](how-to-deploy-fpga-web-service.md).

### <a id="iotedge"></a> Bereitstellen für Azure IoT Edge

Ein Azure IoT Edge-Gerät ist ein Linux- oder Windows-basiertes Gerät, das die Azure IoT Edge-Runtime ausführt. Machine Learning-Modelle können auf diesen Geräten als IoT Edge-Module bereitgestellt werden. Das Bereitstellen eines Modells auf einem IoT Edge-Gerät ermöglicht es dem Gerät, das Modell direkt zu verwenden, anstatt die Daten für die Verarbeitung an die Cloud zu senden. So können Sie die Antwortzeiten verkürzen und die Datenübertragung reduzieren.

Azure IoT Edge-Module werden aus einer Containerregistrierung auf Ihrem Gerät bereitgestellt. Wenn Sie ein Image aus Ihrem Modell erstellen, wird es in der Containerregistrierung für Ihren Arbeitsbereich gespeichert.

#### <a name="set-up-your-environment"></a>Einrichten der Umgebung

* Eine Entwicklungsumgebung. Weitere Informationen finden Sie in der Dokumentation [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).

* Ein [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement. 

* Ein trainiertes Modell. Ein Beispiel zum Trainieren eines Modells finden Sie in der Dokumentation [Train an image classification model with Azure Machine Learning (Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning)](tutorial-train-models-with-aml.md). Ein vortrainiertes Modell steht im [AI-Toolkit für Azure IoT Edge-GitHub-Repository](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial) zur Verfügung.

#### <a name="prepare-the-iot-device"></a>Vorbereiten des IoT-Geräts
Sie müssen einen IoT-Hub erstellen und mit [diesem Skript](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister) ein Gerät registrieren oder eines wiederverwenden, das Sie bereits haben.

``` bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister
sudo chmod +x createNregister
sudo ./createNregister <The Azure subscriptionID you want to use> <Resourcegroup to use or create for the IoT hub> <Azure location to use e.g. eastus2> <the Hub ID you want to use or create> <the device ID you want to create>
```

Speichern Sie die resultierende Verbindungszeichenfolge nach "cs":"{copy this string}".

Initialisieren Sie Ihr Gerät, indem Sie [dieses Skript](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge) auf einen UbuntuX64 IoT Edge-Knoten oder die DSVM herunterladen, um die folgenden Befehle auszuführen:

```bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge
sudo chmod +x installIoTEdge
sudo ./installIoTEdge
```

Der IoT Edge-Knoten ist bereit, die Verbindungszeichenfolge für Ihren IoT Hub zu empfangen. Suchen Sie nach der Zeile ```device_connection_string:```, und fügen Sie die Verbindungszeichenfolge von oben zwischen den Anführungszeichen ein.

Eine Schrittanleitung dazu, wie Sie Ihr Gerät registrieren und die IoT-Runtime installieren, finden Sie im Dokument [Schnellstart: Bereitstellen des ersten IoT Edge-Moduls auf einem Linux-basierten x64-Gerät](../../iot-edge/quickstart-linux.md).


#### <a name="get-the-container-registry-credentials"></a>Abrufen der Anmeldeinformationen für die Containerregistrierung
Um ein IoT Edge-Modul auf Ihrem Gerät bereitzustellen, benötigt Azure IoT die Anmeldeinformationen für die Containerregistrierung, in der der Azure Machine Learning Service Docker-Images speichert.

Sie können die erforderlichen Anmeldeinformationen für die Containerregistrierung bequem auf zwei Arten abrufen:

+ **Im Azure-Portal**:

  1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/signin/index) an.

  1. Wechseln Sie zum Arbeitsbereich Ihres Azure Machine Learning-Diensts, und klicken Sie auf __Übersicht__. Klicken Sie auf den Link __Registrierung__, um zu den Einstellungen der Containerregistrierung zu wechseln.

     ![Ein Bild des Containerregistrierungseintrags](./media/how-to-deploy-and-where/findregisteredcontainer.png)

  1. Klicken Sie in der Containerregistrierung auf **Zugriffsschlüssel**, und aktivieren Sie dann den Administratorbenutzer.
 
     ![Ein Bild des Bildschirms „Zugriffsschlüssel“](./media/how-to-deploy-and-where/findaccesskey.png)

  1. Speichern Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. 

+ **Mit einem Python-Skript**:

  1. Verwenden Sie das folgende Python-Skript nach dem Code, den Sie oben zum Erstellen eines Containers ausgeführt haben:

     ```python
     # Getting your container details
     container_reg = ws.get_details()["containerRegistry"]
     reg_name=container_reg.split("/")[-1]
     container_url = "\"" + image.image_location + "\","
     subscription_id = ws.subscription_id
     from azure.mgmt.containerregistry import ContainerRegistryManagementClient
     from azure.mgmt import containerregistry
     client = ContainerRegistryManagementClient(ws._auth,subscription_id)
     result= client.registries.list_credentials(resource_group_name, reg_name, custom_headers=None, raw=False)
     username = result.username
     password = result.passwords[0].value
     print('ContainerURL{}'.format(image.image_location))
     print('Servername: {}'.format(reg_name))
     print('Username: {}'.format(username))
     print('Password: {}'.format(password))
     ```
  1. Speichern Sie die Werte für ContainerURL, Servername, Benutzername und Kennwort. 

     Diese Anmeldeinformationen sind erforderlich, um dem IoT Edge-Gerät Zugriff auf Images in Ihrer privaten Containerregistrierung zu gewähren.

#### <a name="deploy-the-model-to-the-device"></a>Bereitstellen des Modells für das Gerät

Sie können ein Modell ganz einfach bereitstellen, indem Sie [dieses Skript](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel) ausführen und die folgenden Informationen aus den obigen Schritten bereitstellen: Name der Containerregistrierung, Benutzername, Kennwort, URL des Imagespeicherorts, gewünschter Bereitstellungsname, IoT Hub-Name und von Ihnen erstellte Geräte-ID. Dazu können Sie auf dem virtuellen Computer die folgenden Schritte ausführen: 

```bash 
wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel
sudo chmod +x deploymodel
sudo ./deploymodel <ContainerRegistryName> <username> <password> <imageLocationURL> <DeploymentID> <IoTHubname> <DeviceID>
```

Alternativ können Sie die Schritte im Dokument [Deploy Azure IoT Edge modules from the Azure portal (Bereitstellen von Azure IoT Edge-Modulen aus dem Azure-Portal)](../../iot-edge/how-to-deploy-modules-portal.md) beachten, um das Image auf Ihrem Gerät bereitzustellen. Verwenden Sie beim Konfigurieren der __Registrierungseinstellungen__ für das Gerät den __Anmeldeserver__, den __Benutzernamen__ und das __Kennwort__ für Ihre Arbeitsbereich-Containerregistrierung.

> [!NOTE]
> Wenn Sie nicht mit Azure IoT vertraut sind, finden Sie in den folgenden Dokumenten Informationen zu den ersten Schritten mit dem Dienst:
>
> * [Schnellstart: Deploy your first IoT Edge module to a Linux device (Bereitstellen Ihres ersten IoT Edge-Moduls auf einem Linux-Gerät)](../../iot-edge/quickstart-linux.md)
> * [Schnellstart: Deploy your first IoT Edge module to a Windows device (Bereitstellen Ihres ersten IoT Edge-Moduls auf einem Windows-Gerät)](../../iot-edge/quickstart.md)


## <a name="testing-web-service-deployments"></a>Testen der Webdienstbereitstellungen

Um eine Webdienstbereitstellung zu testen, können Sie die Methode `run` des Webdienstobjekts verwenden. Im folgenden Beispiel wird ein JSON-Dokument auf einen Webdienst festgelegt und das Ergebnis angezeigt. Die gesendeten Daten müssen mit dem übereinstimmen, was das Modell erwartet. In diesem Beispiel entspricht das Format der Daten der Eingabe, die vom Diabetes-Modell erwartet wird.

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data = test_sample)
print(prediction)
```

## <a name="update-the-web-service"></a>Aktualisieren des Webdiensts

Verwenden Sie die Methode `update`, um den Webdienst zu aktualisieren. Der folgende Code veranschaulicht, wie Sie den Webdienst so aktualisieren, dass er ein neues Image verwendet:

```python
from azureml.core.webservice import Webservice

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)
# Update the image used by the service
service.update(image = new-image)
print(service.state)
```

> [!NOTE]
> Wenn Sie ein Image aktualisieren, wird der Webdienst nicht automatisch aktualisiert. Sie müssen jeden Dienst, der das neue Image verwenden soll, manuell aktualisieren.

## <a name="clean-up"></a>Bereinigen

Verwenden Sie zum Löschen eines bereitgestellten Webdiensts `service.delete()`.

Verwenden Sie zum Löschen eines Image `image.delete()`.

Verwenden Sie zum Löschen eines registrierten Modells `model.delete()`.

## <a name="next-steps"></a>Nächste Schritte

* [Secure Azure Machine Learning web services with SSL (Sichere Azure Machine Learning-Webdienste mit SSL)](how-to-secure-web-service.md)
* [Consume a ML Model deployed as a web service (Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells)](how-to-consume-web-service.md).
* [How to run batch predictions (Ausführen von Batchvorhersagen)](how-to-run-batch-predictions.md)

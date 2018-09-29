---
title: Bereitstellen von Modellen des Azure Machine Learning-Diensts auf IoT Edge-Geräten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Modell des Azure Machine Learning-Diensts auf Azure IoT Edge-Geräten bereitstellen. Durch das Bereitstellen eines Modells auf einem Edgegerät können Sie Daten auf dem Gerät bewerten, anstatt diese an die Cloud zu senden und auf eine Antwort zu warten.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shipatel
author: shivanipatel
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 03d692ddfd6f41fd559e9b921f0214a9cd2ada22
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225224"
---
# <a name="prepare-to-deploy-models-on-iot-edge"></a>Vorbereiten der Bereitstellung von Modellen in IoT Edge

In diesem Dokument erfahren Sie, wie Sie den Azure Machine Learning-Dienst zum Vorbereiten eines trainierten Modells für die Bereitstellung auf einem Azure IoT Edge-Gerät verwenden.

Ein Azure IoT Edge-Gerät ist ein Linux- oder Windows-basiertes Gerät, das die Azure IoT Edge-Runtime ausführt. Machine Learning-Modelle können auf diesen Geräten als IoT Edge-Module bereitgestellt werden. Das Bereitstellen eines Modells auf einem IoT Edge-Gerät ermöglicht es dem Gerät, das Modell direkt zu verwenden, anstatt die Daten für die Verarbeitung an die Cloud zu senden. So können Sie die Antwortzeiten verkürzen und die Datenübertragung reduzieren.

Befolgen Sie vor dem Bereitstellen eines Modells auf einem Edgegerät die in dieser Dokumentation beschriebenen Schritte, um das Modell und das Gerät vorzubereiten.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Ein Azure Machine Learning-Dienstbereich. Führen Sie zum Erstellen eines Dienstbereichs die unter [Erste Schritte mit Azure Machine Learning-Diensten](quickstart-get-started.md) beschriebenen Schritte durch.

* Eine Entwicklungsumgebung. Weitere Informationen finden Sie in der Dokumentation [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).

* Ein [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement. 

* Ein trainiertes Modell. Ein Beispiel zum Trainieren eines Modells finden Sie in der Dokumentation [Train an image classification model with Azure Machine Learning (Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning)](tutorial-train-models-with-aml.md).

## <a name="prepare-the-iot-device"></a>Vorbereiten des IoT-Geräts

Führen Sie die Schritte in der Dokumentation [Schnellstart: Bereitstellen des ersten IoT Edge-Moduls auf einem Linux-basierten x64-Gerät](../../iot-edge/quickstart-linux.md) aus, um zu erfahren, wie Sie Ihr Gerät registrieren und die IoT-Runtime installieren.

## <a name="register-the-model"></a>Registrieren des Modells

Azure IoT Edge-Module basieren auf Container-Images. Führen Sie zum Bereitstellen Ihres Modells auf einem IoT Edge-Gerät zunächst die folgenden Schritte aus, um Ihr Modell in einem Azure Machine Learning-Arbeitsbereich zu registrieren und ein Docker-Image zu erstellen. 

> [!IMPORTANT]
> Wenn Sie Azure Machine Learning zum Trainieren Ihres Modells verwendet haben, ist es möglicherweise schon in Ihrem Arbeitsbereich registriert. Überspringen Sie Schritt 3, wenn dies der Fall ist.

1. Initialisieren Sie den Arbeitsbereich, und laden Sie die config.json-Datei:

    ```python
    from azureml.core  import Workspace

    #Load existing workspace from the config file info.
    ws  = Workspace.from_config()
    ```    

1. Registrieren Sie das Modell in Ihrem Arbeitsbereich. Ersetzen Sie den Standardtext durch den Pfad, den Namen, die Tags und die Beschreibung Ihres Modells.

    ```python
    from azureml.core.model import Model
    model = Model.register(model_path = "model.pkl", # this path points to the local file
                        model_name = "best_model", # the model gets registered as this name
                        tags = {'attribute': "myattribute", 'classification': "myclassification"},
                        description = "My awesome model",
                        workspace = ws)
    ```    

1. Rufen Sie das registrierte Modell ab: 

    ```python
    from azureml.core.model import Model

    model_name = "best_model"
    model = Model(ws, model_name)                     
    ```    

## <a name="create-a-docker-image"></a>Erstellen eines Docker-Image

1. Erstellen Sie ein **Bewertungsskript** namens `score.py`. Diese Datei wird verwendet, um das Modell im Image auszuführen. Die Datei muss die folgenden Funktionen enthalten:

    * Die `init()`-Funktion, die das Modell in der Regel in ein globales Objekt lädt. Diese Funktion wird nur dann ausgeführt, wenn der Docker-Container gestartet wird. 

    * Die `run(input_data)`-Funktion verwendet das Modell zur Vorhersage eines Werts, der auf den Eingabedaten basiert. Ein- und Ausgaben zur Ausführung verwenden in der Regel JSON für die Serialisierung und Deserialisierung, allerdings werden auch andere Formate unterstützt.

    Ein Beispiel hierfür finden Sie im Tutorial zur [Bildklassifizierung](tutorial-deploy-models-with-aml.md#make-script).

1. Erstellen Sie eine **Umgebungsdatei** namens `myenv.yml`. Diese Datei ist eine Spezifikation der Conda-Umgebung und zeigt eine Liste aller Abhängigkeiten an, die vom Skript und vom Modell benötigt werden. Ein Beispiel hierfür finden Sie im Tutorial zur [Bildklassifizierung](tutorial-deploy-models-with-aml.md#make-myenv).

1. Konfigurieren Sie das Docker-Image mithilfe der Dateien `score.py` und `myenv.yml`.
    
    ```python
    from azureml.core.image import Image, ContainerImage
    
    #Image configuration
    image_config = ContainerImage.image_configuration( runtime = "python", 
                           execution_script = "score.py",
                           conda_file = "myenv.yml", 
                           tags = {"attributes", "calssification"},
                           description = "Image that contains my model",
                           
                        )
    ```    

1. Erstellen Sie das Image mithilfe der Modell- und Imagekonfiguration:

    ```python
    image = ContainerImage.create (name = "myimage", 
                           models = [model], #this is the model object
                           image_config = image_config,
                           workspace = ws
                        )
    ```     

    Die Imageerstellung dauert ungefähr fünf Minuten.

## <a name="get-the-container-registry-credentials"></a>Abrufen der Anmeldeinformationen für die Containerregistrierung

Azure IoT benötigt die Anmeldeinformationen für die Containerregistrierung, in der der Azure Machine Learning-Dienst Docker-Images speichert. Führen Sie die folgenden Schritt aus, um die Anmeldeinformationen abzurufen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/signin/index) an.

1. Wechseln Sie zu Ihrem Azure Machine Learning-Arbeitsbereich, und klicken Sie auf __Übersicht__. Klicken Sie auf den Link __Registrierung__, um zu den Einstellungen der Containerregistrierung zu wechseln.

    ![Ein Bild des Containerregistrierungseintrags](./media/how-to-deploy-to-iot/findregisteredcontainer.png)

1. Klicken Sie in der Containerregistrierung auf **Zugriffsschlüssel**, und aktivieren Sie dann den Administratorbenutzer.

    ![Ein Bild des Bildschirms „Zugriffsschlüssel“](./media/how-to-deploy-to-iot/findaccesskey.png)

1. Speichern Sie die Werte für Anmeldeserver, Benutzername und Kennwort. 

   Diese Anmeldeinformationen sind erforderlich, um dem IoT Edge-Gerät Zugriff auf Images in Ihrer privaten Containerregistrierung zu gewähren.

## <a name="next-steps"></a>Nächste Schritte

Sie haben die Vorbereitungen für die Bereitstellung abgeschlossen. Führen Sie nun die unter [Bereitstellen von Azure IoT Edge-Modulen über das Azure-Portal](../../iot-edge/how-to-deploy-modules-portal.md) beschriebenen Schritte aus, um Module auf Ihrem Edgegerät bereitzustellen. Verwenden Sie beim Konfigurieren der __Registrierungseinstellungen__ für das Gerät die Anmeldeinformationen, die Sie zuvor konfiguriert haben.

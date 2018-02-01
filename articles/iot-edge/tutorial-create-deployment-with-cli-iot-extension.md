---
title: "Bereitstellen von Modulen auf IoT Edge-Geräten per IoT-Erweiterung für Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Bereitstellen von Modulen auf einem IoT Edge-Gerät per IoT-Erweiterung für Azure CLI 2.0"
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 01/11/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 26067187864f9a2a4c85c953ae8aca888458d245
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Bereitstellen von Modulen auf einem IoT Edge-Gerät per IoT-Erweiterung für Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) ist ein plattformübergreifendes Open-Source-Befehlszeilentool zum Verwalten von Azure-Ressourcen, z.B. IoT Edge. Azure CLI 2.0 ist unter Windows, Linux und MacOS verfügbar.

Mit Azure CLI 2.0 können Sie Azure IoT Hub-Ressourcen, Instanzen des Device Provisioning-Diensts und verknüpfte Hubs ohne weitere Tools verwalten. Mit der neuen IoT-Erweiterung wird Azure CLI 2.0 um Features wie beispielsweise die Geräteverwaltung und umfassende IoT Edge-Funktionen erweitert.

In diesem Tutorial führen Sie zuerst die Schritte zum Einrichten von Azure CLI 2.0 und der IoT-Erweiterung aus. Anschließend wird beschrieben, wie Sie Module auf einem IoT Edge-Gerät mit den verfügbaren CLI-Befehlen bereitstellen.

## <a name="installation"></a>Installation 

### <a name="step-1---install-python"></a>Schritt 1: Installieren von Python

[Python 2.7x oder Python 3.x](https://www.python.org/downloads/) ist erforderlich.

### <a name="step-2---install-azure-cli-20"></a>Schritt 2: Installieren von Azure CLI 2.0

Befolgen Sie die [Installationsanleitung](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), um Azure CLI 2.0 in Ihrer Umgebung einzurichten. Ihre Azure CLI 2.0-Version muss mindestens 2.0.24 lauten. Verwenden Sie `az –version`, um dies zu überprüfen. Diese Version unterstützt az-Erweiterungsbefehle, und das Framework für Knack-Befehle wird eingeführt. Eine einfache Möglichkeit zur Installation unter Windows besteht darin, den [Microsoft Windows Installer (MSI)](https://aka.ms/InstallAzureCliWindows) herunterzuladen und zu installieren.

### <a name="step-3---install-iot-extension"></a>Schritt 3: Installieren der IoT-Erweiterung

In der [Infodatei zur IoT-Erweiterung](https://github.com/Azure/azure-iot-cli-extension) werden mehrere Wege zum Installieren der Erweiterung beschrieben. Die einfachste Möglichkeit ist die Ausführung von `az extension add --name azure-cli-iot-ext`. Nach der Installation können Sie `az extension list` verwenden, um die derzeit installierten Erweiterungen zu überprüfen, oder mit `az extension show --name azure-cli-iot-ext` Details zur IoT-Erweiterung anzeigen. Sie können `az extension remove --name azure-cli-iot-ext` verwenden, um die Erweiterung zu entfernen.


## <a name="deploy-modules-to-an-iot-edge-device"></a>Bereitstellen von Modulen auf einem IoT Edge-Gerät
In diesem Tutorial wird beschrieben, wie Sie eine IoT Edge-Bereitstellung erstellen. Im Beispiel wird veranschaulicht, wie Sie sich an Ihrem Azure-Konto anmelden, eine Ressourcengruppe erstellen (einen Container, der Ressourcen für eine Azure-Lösung enthält), einen IoT Hub und drei IoT Edge-Geräteidentitäten erstellen, Tags festlegen und dann eine IoT Edge-Bereitstellung für diese Geräte erstellen. Führen Sie vor dem Fortfahren die oben beschriebenen Installationsschritte aus. Falls Sie noch kein Azure-Konto besitzen, können Sie sofort ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-login-to-the-azure-account"></a>1. Anmelden am Azure-Konto
  
    az login

![Anmeldung][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Erstellen der Ressourcengruppe „IoTHubBlogDemo“ für „USA, Osten“ (eastus)

    az group create -l eastus -n IoTHubBlogDemo

![Ressourcengruppe erstellen][2]


### <a name="3-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>3. Erstellen des IoT Hub „blogDemoHub“ unter der neu erstellten Ressourcengruppe

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub erstellen][3]


### <a name="4-create-an-iot-edge-device"></a>4. Erstellen eines IoT Edge-Geräts

    az iot hub device-identity create -d edge001 -n blogDemoHub --edge-enabled

![IoT Edge-Gerät erstellen][4]

### <a name="5-apply-configuration-to-the-iot-edge-device"></a>5. Anwenden der Konfiguration auf das IoT Edge-Gerät

Speichern Sie Ihre JSON-Vorlage für die Bereitstellung lokal als TXT-Datei. Sie benötigen den Pfad zur Datei beim Ausführen des Befehls „apply-configuration“.

Hier ist ein Beispiel für eine JSON-Bereitstellungsvorlage mit einem tempSensor-Modul angegeben:

```json
{
  "moduleContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": ""
          }
        },
        "systemModules": {
          "edgeAgent": {
            "type": "docker",
            "settings": {
              "image": "edgepreview.azurecr.io/azureiotedge/edge-agent:1.0-preview",
              "createOptions": "{}"
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "edgepreview.azurecr.io/azureiotedge/edge-hub:1.0-preview",
              "createOptions": "{}"
            }
          }
        },
        "modules": {
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "edgepreview.azurecr.io/azureiotedge/simulated-temperature-sensor:1.0-preview",
              "createOptions": "{}"
            }
          }
        }
      }
    },
    "$edgeHub": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 7200
        }
      }
    },
    "tempSensor": {
      "properties.desired": {}
    }
  }
}
```

    az iot hub apply-configuration --device-id edge001 --hub-name blogDemoHub --content C:\<yourLocation>\edgeconfig.txt

![Konfiguration anwenden][5]

### <a name="6-list-modules"></a>6. Auflisten von Modulen
    
    az iot hub module-identity list --device-id edge001 --hub-name blogDemoHub

![Module auflisten][6]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Funktion erstellt, die Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Wenn Sie Azure IoT Edge weiter erkunden möchten, können Sie nun erfahren, wie Sie ein IoT Edge-Gerät als Gateway verwenden. 

> [!div class="nextstepaction"]
> [Erstellen eines IoT Edge-Gatewaygeräts](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-create-deployment-with-cli-iot-extension/login.jpg
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.jpg
[3]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-hub.jpg
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[5]: ./media/tutorial-create-deployment-with-cli-iot-extension/apply-configuration.PNG
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.PNG


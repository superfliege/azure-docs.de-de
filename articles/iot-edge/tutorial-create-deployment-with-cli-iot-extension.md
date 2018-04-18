---
title: Bereitstellen von Modulen auf IoT Edge-Geräten per IoT-Erweiterung für Azure CLI 2.0 | Microsoft-Dokumentation
description: Bereitstellen von Modulen auf einem IoT Edge-Gerät per IoT-Erweiterung für Azure CLI 2.0
services: iot-edge
keywords: ''
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 03/02/2018
ms.topic: article
ms.service: iot-edge
ms.custom: ''
ms.reviewer: kgremban
ms.openlocfilehash: 1f71fdfb7090dce24ba73f1fa01e287c52b065f8
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Bereitstellen von Modulen auf einem IoT Edge-Gerät per IoT-Erweiterung für Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure?view=azure-cli-latest) ist ein plattformübergreifendes Open-Source-Befehlszeilentool zum Verwalten von Azure-Ressourcen, z.B. IoT Edge. Azure CLI 2.0 ist unter Windows, Linux und MacOS verfügbar.

Mit Azure CLI 2.0 können Sie Azure IoT Hub-Ressourcen, Instanzen des Device Provisioning-Diensts und verknüpfte Hubs ohne weitere Tools verwalten. Mit der neuen IoT-Erweiterung wird Azure CLI 2.0 um Features wie beispielsweise die Geräteverwaltung und umfassende IoT Edge-Funktionen erweitert.

In diesem Artikel richten Sie die Azure CLI 2.0 und die IoT-Erweiterung ein. Anschließend wird beschrieben, wie Sie Module auf einem IoT Edge-Gerät mit den verfügbaren CLI-Befehlen bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto. Wenn Sie noch kein Konto haben, können Sie jetzt ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?v=17.39a). 

* [Python 2.7x oder Python 3.x](https://www.python.org/downloads/).

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in Ihrer Umgebung. Ihre Azure CLI 2.0-Version muss mindestens 2.0.24 lauten. Verwenden Sie `az –-version`, um dies zu überprüfen. Diese Version unterstützt az-Erweiterungsbefehle, und das Framework für Knack-Befehle wird eingeführt. Eine einfache Möglichkeit zur Installation unter Windows besteht darin, den [Microsoft Windows Installer (MSI)](https://aka.ms/InstallAzureCliWindows) herunterzuladen und zu installieren.

* [Die IoT-Erweiterung für Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension):
   1. Führen Sie `az extension add --name azure-cli-iot-ext`aus. 
   2. Nach der Installation können Sie `az extension list` verwenden, um die derzeit installierten Erweiterungen zu überprüfen, oder mit `az extension show --name azure-cli-iot-ext` Details zur IoT-Erweiterung anzeigen.
   3. Um die Erweiterung zu entfernen, verwenden Sie `az extension remove --name azure-cli-iot-ext`.


## <a name="create-an-iot-edge-device"></a>Erstellen eines IoT Edge-Geräts
Dieser Artikel enthält Anweisungen zum Erstellen einer IoT-Edge-Bereitstellung. Im Beispiel wird veranschaulicht, wie Sie sich bei Ihrem Azure-Konto anmelden, eine Azure-Ressourcengruppe erstellen (einen Container, der Ressourcen für eine Azure-Lösung enthält), einen IoT Hub und drei IoT Edge-Geräteidentitäten erstellen, Tags festlegen und dann eine IoT Edge-Bereitstellung für diese Geräte erstellen. 

Melden Sie sich beim Azure-Konto an. Nachdem Sie den folgenden Befehl für die Anmeldung eingegeben haben, werden Sie aufgefordert, einen Webbrowser zu verwenden, um sich mit einem einmaligen Code anzumelden: 

   ```cli
   az login
   ```

Erstellen Sie eine neue Ressourcengruppe namens **IoTHubCLI** in der Region USA (Osten): 

   ```cli
   az group create -l eastus -n IoTHubCLI
   ```

   ![Ressourcengruppe erstellen][2]

Erstellen Sie in der neu erstellten Ressourcengruppe einen IoT Hub namens **CLIDemoHub**:

   ```cli
   az iot hub create --name CLIDemoHub --resource-group IoTHubCLI --sku S1
   ```

   >[!TIP]
   >Jedem Abonnement ist ein kostenloser IoT Hub zugewiesen. Um mit dem CLI-Befehl einen kostenlosen Hub zu erstellen, ersetzen Sie den SKU-Wert durch `--sku F1`. Wenn Sie bereits einen kostenlosen Hub in Ihrem Abonnement haben, erhalten Sie eine Fehlermeldung, wenn Sie versuchen, einen zweiten Hub zu erstellen. 

Erstellen Sie ein IoT Edge-Gerät:

   ```cli
   az iot hub device-identity create --device-id edge001 -hub-name CLIDemoHub --edge-enabled
   ```

   ![IoT Edge-Gerät erstellen][4]

## <a name="configure-the-iot-edge-device"></a>Konfigurieren des IoT Edge-Geräts

Erstellen Sie eine JSON-Vorlage für die Bereitstellung, und speichern Sie diese lokal als TXT-Datei. Sie benötigen den Pfad zur Datei beim Ausführen des Befehls „apply-configuration“.

JSON-Bereitstellungsvorlagen sollten immer die beiden Systemmodule edgeAgent und edgeHub enthalten. Neben diesen beiden Systemmodulen können Sie mit dieser Datei zusätzliche Module auf dem IoT Edge-Gerät bereitstellen. Verwenden Sie das folgende Beispiel, um das IoT Edge-Gerät mit einem tempSensor-Modul zu konfigurieren:

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
                 "image": "microsoft/azureiotedge-agent:1.0-preview",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "microsoft/azureiotedge-hub:1.0-preview",
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
                 "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
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

Wenden Sie die Konfiguration auf das IoT Edge-Gerät an:

   ```cli
   az iot hub apply-configuration --device-id edge001 --hub-name CLIDemoHub --content C:\<configuration.txt file path>
   ```

Zeigen Sie die Module auf Ihrem IoT Edge-Gerät an:
    
   ```cli
   az iot hub module-identity list --device-id edge001 --hub-name CLIDemoHub
   ```

   ![Module auflisten][6]

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie ein [IoT Edge-Gerät als Gateway verwenden](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.png
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.png


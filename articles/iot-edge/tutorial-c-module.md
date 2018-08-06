---
title: Tutorial für Azure IoT Edge C | Microsoft-Dokumentation
description: In diesem Tutorial wird gezeigt, wie Sie ein IoT Edge-Modul mit C-Code erstellen und dieses Modul auf einem Edge-Gerät bereitstellen.
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 07/30/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: c9d1931f1b78bb19f5e321a19baca45265ea7ab4
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413160"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Tutorial: Entwickeln und Bereitstellen eines C-IoT Edge-Moduls für Ihr simuliertes Gerät

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. In diesem Tutorial wird beschrieben, wie Sie ein IoT Edge-Modul, mit dem Sensordaten gefiltert werden, erstellen und bereitstellen. In diesem Tutorial lernen Sie Folgendes:    

> [!div class="checklist"]
> * Erstellen eines IoT Edge-Moduls in C mit Visual Studio Code
> * Erstellen und Veröffentlichen eines Docker-Images in Ihrer Containerregistrierung unter Verwendung von Visual Studio Code und Docker 
> * Bereitstellen des Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten


Das IoT Edge-Modul, das Sie in diesem Tutorial erstellen, filtert die von Ihrem Gerät generierten Temperaturdaten. Es leitet Nachrichten nur dann an nachgeschaltete Geräte weiter, wenn die Temperatur einen angegebenen Schwellenwert überschreitet. Diese Art der Analyse auf Edge-Ebene eignet sich zur Verringerung der Datenmenge, die in die Cloud weitergeleitet und dort gespeichert wird. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

* Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als Edge-Gerät verwenden, indem Sie die Schritte ausführen, die in der Schnellstartanleitung für [Linux](quickstart-linux.md)- oder [Windows](quickstart.md)-Geräte beschrieben sind.

Cloudressourcen:

* Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) mit Standardtarif in Azure. 

Entwicklungsressourcen:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [C/C++-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) für Visual Studio Code.
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) für Visual Studio Code
* [Docker CE](https://docs.docker.com/install/). 


## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung
In diesem Tutorial verwenden Sie die Azure IoT Edge-Erweiterung für VSCode zum Entwickeln eines Moduls und zum Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.  

Sie können für dieses Tutorial jede beliebige Docker-kompatible Registrierung verwenden. Zwei beliebte Docker-Registrierungsdienste, die in der Cloud verfügbar sind, sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet. 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Azure Container Registry** aus.
2. Benennen Sie die Registrierung, wählen Sie ein Abonnement und eine Ressourcengruppe aus, und legen Sie die SKU auf **Basic** fest. 
3. Klicken Sie auf **Erstellen**.
4. Navigieren Sie nach der Erstellung der Containerregistrierung zu dieser Registrierung, und wählen Sie **Zugriffsschlüssel** aus. 
5. Legen Sie **Administratorbenutzer** auf **Aktivieren** fest.
6. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. Sie verwenden diese Werte später in diesem Tutorial, wenn Sie das Docker-Image in der Registrierung veröffentlichen und wenn Sie die Anmeldeinformationen für die Registrierung der Edge-Runtime hinzufügen. 

## <a name="create-an-iot-edge-module-project"></a>Erstellen eines IoT Edge-Modulprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modulprojekt auf Basis von .NET Core 2.0 erstellen.
1. Wählen Sie in Visual Studio Code die Optionen **Ansicht** > **Integriertes Terminal** aus, um das in Visual Studio Code integrierte Terminal zu öffnen.
2. Klicken Sie auf **Ansicht** > **Befehlspalette**, um die VS Code-Befehlspalette zu öffnen. 
3. Geben Sie in der Befehlspalette den Befehl **Azure: Sign in** (Azure: Anmelden) ein, und führen Sie ihn aus. Befolgen Sie die Anweisungen zum Anmelden beim Azure-Konto. Wenn Sie bereits angemeldet sind, können Sie diesen Schritt überspringen.
4. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: Neue IoT Edge-Projektmappe) ein, und führen Sie ihn aus. Geben Sie in der Befehlspalette die folgenden Informationen an, um die Projektmappe zu erstellen: 
   1. Wählen Sie den Ordner aus, in dem die Projektmappe erstellt werden soll. 
   2. Geben Sie einen Namen für Ihre Projektmappe ein, oder übernehmen Sie den Standardnamen **EdgeSolution**.
   3. Wählen Sie **C-Modul** als Modulvorlage aus. 
   4. Nennen Sie das Modul **CModule**. 
   5. Geben Sie die Azure Container Registry-Instanz an, die Sie im vorherigen Abschnitt als Imagerepository für das erste Modul erstellt haben. Ersetzen Sie **localhost:5000** durch den kopierten Wert für den Anmeldeserver. Die endgültige Zeichenfolge sieht wie folgt aus: **\<Registrierungsname\>.azurecr.io/cmodule**.
 
4. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Er enthält einen Ordner für **Module**, den Ordner **.vscode**, eine Bereitstellungsmanifest-Vorlagendatei und eine Datei vom Typ **.env**. Der Standardmodulcode wird als Pipemodul implementiert. 

5. Zum Filtern von Nachrichten im JSON-Format muss eine JSON-Bibliothek für C importiert werden. Sie können eine beliebige JSON-Bibliothek auswählen oder eine eigene erstellen, um JSON in Ihrem C-Modul zu analysieren. Bei den folgenden Schritten wird [Parson](https://github.com/kgabis/parson) als Beispiel verwendet.
   1. Laden Sie **parson.c** und **parson.h** vom [Parson-Repository in GitHub](https://github.com/kgabis/parson) herunter. Kopieren Sie diese beiden Dateien, und fügen Sie sie im Ordner **CModule** ein.
   2. Öffnen Sie **Module** > **CModule** > **CMakeLists.txt**. Fügen Sie die folgenden Zeilen hinzu, um die Parson-Bibliothek als „my_parson“ zu importieren.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. Fügen Sie in `target_link_libraries` in **CMakeLists.txt** Folgendes hinzu: `my_parson`.
   4. Öffnen Sie **Module** > **CModule** > **main.c**. Fügen Sie unten im Abschnitt „include“ den folgenden Code hinzu, um `parson.h` für JSON-Unterstützung einzubeziehen:

      ```c
      #include "parson.h"
      ```

6. Fügen Sie die Variable `temperatureThreshold` als globale Variable nach dem Abschnitt „include“ ein. Mit dieser Variable wird der Wert festgelegt, den die gemessene Temperatur übersteigen muss, damit die Daten an IoT Hub gesendet werden. 

    ```c
    static double temperatureThreshold = 25;
    ```

7. Ersetzen Sie die gesamte Funktion `CreateMessageInstance` durch den folgenden Code. Diese Funktion weist einen Kontext für den Rückruf zu. 

    ```c
    static MESSAGE_INSTANCE* CreateMessageInstance(IOTHUB_MESSAGE_HANDLE message)
    {
        MESSAGE_INSTANCE* messageInstance = (MESSAGE_INSTANCE*)malloc(sizeof(MESSAGE_INSTANCE));
        if (NULL == messageInstance)
        {
            printf("Failed allocating 'MESSAGE_INSTANCE' for pipelined message\r\n");
        }
        else
        {
            memset(messageInstance, 0, sizeof(*messageInstance));

            if ((messageInstance->messageHandle = IoTHubMessage_Clone(message)) == NULL)
            {
                free(messageInstance);
                messageInstance = NULL;
            }
            else
            {
                messageInstance->messageTrackingId = messagesReceivedByInput1Queue;
                MAP_HANDLE propMap = IoTHubMessage_Properties(messageInstance->messageHandle);
                if (Map_AddOrUpdate(propMap, "MessageType", "Alert") != MAP_OK)
                {
                    printf("ERROR: Map_AddOrUpdate Failed!\r\n");
                }
            }
        }

        return messageInstance;
    }
    ```

8. Ersetzen Sie die gesamte Funktion `InputQueue1Callback` durch den folgenden Code. Diese Funktion implementiert den Nachrichtenfilter. 

    ```c
    static IOTHUBMESSAGE_DISPOSITION_RESULT InputQueue1Callback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
        IOTHUBMESSAGE_DISPOSITION_RESULT result;
        IOTHUB_CLIENT_RESULT clientResult;
        IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle = (IOTHUB_MODULE_CLIENT_LL_HANDLE)userContextCallback;

        unsigned const char* messageBody;
        size_t contentSize;

        if (IoTHubMessage_GetByteArray(message, &messageBody, &contentSize) != IOTHUB_MESSAGE_OK)
        {
            messageBody = "<null>";
        }

        printf("Received Message [%zu]\r\n Data: [%s]\r\n", 
                messagesReceivedByInput1Queue, messageBody);

        JSON_Value *root_value = json_parse_string(messageBody);
        JSON_Object *root_object = json_value_get_object(root_value);
        double temperature;
        if (json_object_dotget_value(root_object, "machine.temperature") != NULL && (temperature = json_object_dotget_number(root_object, "machine.temperature")) > temperatureThreshold)
        {
            printf("Machine temperature %f exceeds threshold %f\r\n", temperature, temperatureThreshold);
            // This message should be sent to next stop in the pipeline, namely "output1".  What happens at "outpu1" is determined
            // by the configuration of the Edge routing table setup.
            MESSAGE_INSTANCE *messageInstance = CreateMessageInstance(message);
            if (NULL == messageInstance)
            {
                result = IOTHUBMESSAGE_ABANDONED;
            }
            else
            {
                printf("Sending message (%zu) to the next stage in pipeline\n", messagesReceivedByInput1Queue);

                clientResult = IoTHubModuleClient_LL_SendEventToOutputAsync(iotHubModuleClientHandle, messageInstance->messageHandle, "output1", SendConfirmationCallback, (void *)messageInstance);
                if (clientResult != IOTHUB_CLIENT_OK)
                {
                    IoTHubMessage_Destroy(messageInstance->messageHandle);
                    free(messageInstance);
                    printf("IoTHubModuleClient_LL_SendEventToOutputAsync failed on sending msg#=%zu, err=%d\n", messagesReceivedByInput1Queue, clientResult);
                    result = IOTHUBMESSAGE_ABANDONED;
                }
                else
                {
                    result = IOTHUBMESSAGE_ACCEPTED;
                }
            }
        }
        else
        {
            printf("Not sending message (%zu) to the next stage in pipeline.\r\n", messagesReceivedByInput1Queue);
            result = IOTHUBMESSAGE_ACCEPTED;
        }

        messagesReceivedByInput1Queue++;
        return result;
    }
    ```

9. Fügen Sie die Funktion `moduleTwinCallback` hinzu. Diese Methode empfängt Aktualisierungen der gewünschten Eigenschaften aus dem Modulzwilling und aktualisiert die Variable **temperatureThreshold** entsprechend. Alle Module haben ihren eigenen Modulzwilling, über den Sie den in einem Modul ausgeführten Code direkt in der Cloud konfigurieren können.

    ```c
    static void moduleTwinCallback(DEVICE_TWIN_UPDATE_STATE update_state, const unsigned char* payLoad, size_t size, void* userContextCallback)
    {
        printf("\r\nTwin callback called with (state=%s, size=%zu):\r\n%s\r\n", 
            ENUM_TO_STRING(DEVICE_TWIN_UPDATE_STATE, update_state), size, payLoad);
        JSON_Value *root_value = json_parse_string(payLoad);
        JSON_Object *root_object = json_value_get_object(root_value);
        if (json_object_dotget_value(root_object, "desired.TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_dotget_number(root_object, "desired.TemperatureThreshold");
        }
        if (json_object_get_value(root_object, "TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_get_number(root_object, "TemperatureThreshold");
        }
    }
    ```

10. Fügen Sie einen neuen Rückruf für `moduleTwinCallback` in der Funktion `SetupCallbacksForModule` hinzu. Ersetzen Sie die gesamte Funktion `SetupCallbacksForModule` durch den folgenden Code.

    ```c
    static int SetupCallbacksForModule(IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle)
    {
        int ret;

        if (IoTHubModuleClient_LL_SetInputMessageCallback(iotHubModuleClientHandle, "input1", InputQueue1Callback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
        {
            printf("ERROR: IoTHubModuleClient_LL_SetInputMessageCallback(\"input1\")..........FAILED!\r\n");
            ret = __FAILURE__;
        }
        else if (IoTHubModuleClient_LL_SetModuleTwinCallback(iotHubModuleClientHandle, moduleTwinCallback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
        {
            printf("ERROR: IoTHubModuleClient_LL_SetModuleTwinCallback(default)..........FAILED!\r\n");
            ret = __FAILURE__;
        }
        else
        {
            ret = 0;
        }

        return ret;
    }
    ```

11. Speichern Sie diese Datei.

## <a name="build-your-iot-edge-solution"></a>Erstellen Ihrer IoT Edge-Projektmappe

Im vorherigen Abschnitt haben Sie eine IoT Edge-Projektmappe erstellt und Code zu CModule hinzugefügt, der Nachrichten herausfiltert, deren gemeldete Computertemperatur unter dem zulässigen Schwellenwert liegt. Nun müssen Sie die Projektmappe als Containerimage erstellen und per Push an die Containerregistrierung übertragen. 

1. Melden Sie sich durch Eingabe des folgenden Befehls im integrierten Visual Studio Code-Terminal bei Docker an, um das Modulimage per Push an ACR zu übertragen: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Verwenden Sie den Benutzernamen, das Kennwort und den Anmeldeserver, die Sie im ersten Abschnitt aus Azure Container Registry kopiert haben. Oder rufen Sie sie erneut über den Abschnitt **Zugriffsschlüssel** der Registrierung im Azure-Portal ab.

2. Öffnen Sie im VS Code-Explorer die Datei **deployment.template.json** im Arbeitsbereich für Ihre IoT Edge-Projektmappe. Diese Datei weist `$edgeAgent` an, zwei Module bereitzustellen: **tempSensor** und **CModule**. Der Wert `CModule.image` wird auf eine Linux-amd64-Version des Images festgelegt. Weitere Informationen zu Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können – Vorschau](module-composition.md).

3. Die Datei **deployment.template.json** enthält den Abschnitt **registryCredentials**, in dem die Anmeldeinformationen der Docker-Registrierung gespeichert sind. Die tatsächlichen Benutzername- und Kennwortpaare werden in der ENV-Datei gespeichert, die von Git ignoriert wird.

4. Fügen Sie den Modulzwilling „CModule“ zum Bereitstellungsmanifest hinzu. Fügen Sie am Ende des Abschnitts `moduleContent` nach dem Modulzwilling `$edgeHub` den folgenden JSON-Inhalt ein: 
    ```json
        "CModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Speichern Sie diese Datei.
5. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **deployment.template.json**, und klicken Sie dann auf **Build IoT Edge solution** (IoT Edge-Projektmappe erstellen). 

Wenn Sie Visual Studio Code anweisen, die Projektmappe zu erstellen, wird zuerst basierend auf den Informationen in der Bereitstellungsvorlage eine `deployment.json`-Datei in einem neuen Konfigurationsordner (**config**) generiert. Anschließend werden zwei Befehle im integrierten Terminal ausgeführt: `docker build` und `docker push`. Diese beiden Befehle erstellen den Code, packen `CModule.dll` in Container und führen dann eine Pushübertragung an die Containerregistrierung durch, die Sie beim Initialisieren der Projektmappe angegeben haben. 

Sie können die vollständige Adresse des Containerimages mit Tag im integrierten VS Code-Terminal anzeigen. Die Imageadresse wird auf der Grundlage der Informationen in der Datei `module.json` erstellt und hat folgendes Format: **\<Repository\>:\<Version\>-\<Plattform\>**. In diesem Tutorial sollte sie wie folgt aussehen: **registryname.azurecr.io/cmodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Bereitstellen und Ausführen der Projektmappe

1. Konfigurieren Sie die Verbindungszeichenfolge für Ihren IoT Hub in der Azure IoT-Toolkit-Erweiterung: 
    1. Öffnen Sie den Visual Studio Code-Explorer durch Auswählen von **Ansicht** > **Explorer**. 
    2. Klicken Sie im Explorer auf **AZURE IOT HUB DEVICES** (AZURE IOT HUB-GERÄTE), und klicken Sie dann auf **...**. Klicken Sie auf **Select IoT Hub** (IoT Hub auswählen). Befolgen Sie die Anweisungen zum Anmelden beim Azure-Konto, und wählen Sie Ihren IoT-Hub aus. 
       Beachten Sie, dass Sie die Einrichtung auch durch Klicken auf **IoT Hub-Verbindungszeichenfolge festlegen** vornehmen können. Geben Sie die Verbindungszeichenfolge für den IoT-Hub, mit dem Ihr IoT Edge-Gerät eine Verbindung herstellt, in das Popupfenster ein.

2. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf Ihr IoT Edge-Gerät, und klicken Sie dann auf **Create deployment for Edge device** (Bereitstellung für Edge-Gerät erstellen). Wählen Sie im Konfigurationsordner (**config**) die Datei **deployment.json** aus, und klicken Sie auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen).

3. Klicken Sie auf die Schaltfläche „Aktualisieren“. Nun sollte das neue **CModule**-Modul zusammen mit dem **TempSensor**-Modul sowie **$edgeAgent** und **$edgeHub** ausgeführt werden. 

## <a name="view-generated-data"></a>Anzeigen generierter Daten

1. Klicken Sie zum Überwachen der bei IoT Hub eingehenden Daten auf **...** und dann auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).
2. Wenn Sie die D2C-Nachrichten für ein bestimmtes Gerät überwachen möchten, klicken Sie mit der rechten Maustaste auf das Gerät in der Liste, und klicken Sie dann auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).
3. Wenn Sie die Datenüberwachung beenden möchten, führen Sie in der Befehlspalette den Befehl **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: Überwachung von D2C-Nachrichten beenden) aus. 
4. Klicken Sie zum Anzeigen oder Aktualisieren des Modulzwillings mit der rechten Maustaste auf das Modul in der Liste, und klicken Sie auf **Edit module twin** (Modulzwilling bearbeiten). Speichern Sie zum Aktualisieren des Modulzwillings die JSON-Datei des Zwillings, klicken Sie mit der rechten Maustaste auf den Editorbereich, und wählen Sie **Update Module Twin** (Modulzwilling aktualisieren).
5. Zum Anzeigen von Docker-Protokollen können Sie [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) für VS Code installieren und Ihre ausgeführten Module lokal im Docker-Explorer suchen. Klicken Sie zum Anzeigen im integrierten Terminal im Kontextmenü auf **Protokolle anzeigen**.
 
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die bereits erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden.

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 

> [!IMPORTANT]
> Das Löschen der Azure-Ressourcen und der Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. Wenn Sie die IoT Hub-Ressource in einer bereits vorhandenen Ressourcengruppe erstellt haben, die Ressourcen enthält, die Sie behalten möchten, löschen Sie nicht die Ressourcengruppe, sondern nur die IoT Hub-Ressource.
>

Wenn Sie nur die IoT Hub-Instanz löschen möchten, führen Sie den folgenden Befehl mit dem Namen des Hubs und dem Namen der Ressourcengruppe aus:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Löschen Sie die gesamte Ressourcengruppe wie folgt anhand des Namens:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.

2. Geben Sie im Textfeld **Nach Name filtern...** den Namen der Ressourcengruppe ein, die Ihre IoT Hub-Ressource enthält. 

3. Klicken Sie in der Ergebnisliste rechts neben Ihrer Ressourcengruppe auf **...** und dann auf **Ressourcengruppe löschen**.

4. Sie werden aufgefordert, das Löschen der Ressourcengruppe zu bestätigen. Geben Sie zur Bestätigung erneut den Namen Ihrer Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**. Daraufhin werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul erstellt, das Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Wenn Sie eigene Module erstellen möchten, finden Sie entsprechende Informationen unter [Use Visual Studio Code to develop and debug C modules for Azure IoT Edge (Entwickeln eines C-Moduls mit Azure IoT Edge für Visual Studio Code)](how-to-develop-c-module.md). Sie können mit den nächsten Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Speichern von Daten im Edge-Bereich mit SQL Server-Datenbanken](tutorial-store-data-sql-server.md)

<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md

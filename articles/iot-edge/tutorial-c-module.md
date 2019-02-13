---
title: 'Tutorial: Erstellen eines benutzerdefinierten C-Moduls – Azure IoT Edge | Microsoft-Dokumentation'
description: In diesem Tutorial wird gezeigt, wie Sie ein IoT Edge-Modul mit C-Code erstellen und dieses Modul auf einem Edge-Gerät bereitstellen.
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 798cf405c222a443dbbd3a316d20c482daf4429f
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2019
ms.locfileid: "55563251"
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
* C-Module für Azure IoT Edge unterstützen keine Windows-Container. Falls es sich bei Ihrem IoT Edge-Gerät um einen Windows-Computer handelt, vergewissern Sie sich, dass es zur Verwendung von Linux-Containern konfiguriert ist. Weitere Informationen zu den Unterschieden bei der Installation zwischen Windows- und Linux-Containern finden Sie unter [Installieren der Azure IoT Edge-Runtime unter Windows](how-to-install-iot-edge-windows.md).

Cloudressourcen:

* Eine [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-Instanz in Azure im Tarif „Free“ oder „Standard“.

Entwicklungsressourcen:

* [Visual Studio Code](https://code.visualstudio.com/).
* [C/C++-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) für Visual Studio Code
* [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) für Visual Studio Code.
* [Docker CE](https://docs.docker.com/install/).

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

In diesem Tutorial verwenden Sie die Azure IoT-Tools für Visual Studio Code zum Entwickeln eines Moduls und Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.

Sie können eine beliebige Docker-kompatible Registrierung zum Speichern Ihrer Containerimages verwenden. Zwei beliebte Docker-Registrierungsdienste sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet.

Sollten Sie noch keine Containerregistrierung besitzen, führen Sie die folgenden Schritte aus, um eine neue Containerregistrierung in Azure zu erstellen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Container Registry** aus.

2. Geben Sie die folgenden Werte an, um Ihre Containerregistrierung zu erstellen:

   | Feld | Wert |
   | ----- | ----- |
   | Registrierungsname | Geben Sie einen eindeutigen Namen an. |
   | Abonnement | Wählen Sie ein Abonnement aus der Dropdownliste aus. |
   | Ressourcengruppe | Es wird empfohlen, die gleiche Ressourcengruppe für alle Testressourcen zu verwenden, die Sie während der IoT Edge-Schnellstarts und -Tutorials erstellen. Beispielsweise **IoTEdgeResources**. |
   | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. |
   | Administratorbenutzer | Legen Sie **Aktivieren** fest. |
   | SKU | Wählen Sie **Basic** aus. |

5. Klicken Sie auf **Erstellen**.

6. Navigieren Sie nach der Erstellung der Containerregistrierung zu dieser Registrierung, und klicken Sie anschließend auf **Zugriffsschlüssel**.

7. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. Sie verwenden diese Werte an späterer Stelle des Tutorials, um Zugriff auf die Containerregistrierung zu gewähren.

## <a name="create-an-iot-edge-module-project"></a>Erstellen eines IoT Edge-Modulprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT-Tools ein IoT Edge-Modulprojekt auf Basis von .NET Core 2.0 erstellen.

### <a name="create-a-new-solution"></a>Erstellen einer neuen Lösung

Erstellen Sie eine C-Lösungsvorlage, die Sie mit eigenem Code anpassen können.

1. Klicken Sie auf **Ansicht** > **Befehlspalette**, um die VS Code-Befehlspalette zu öffnen.

2. Geben Sie in der Befehlspalette den Befehl **Azure: Sign in** ein, und führen Sie ihn aus, und befolgen Sie die Anweisungen zum Anmelden bei Ihrem Azure-Konto. Wenn Sie bereits angemeldet sind, können Sie diesen Schritt überspringen.

3. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge ein: New IoT Edge Solution** ein, und führen Sie ihn aus. Folgen Sie den Anweisungen in der Befehlspalette, um Ihre Projektmappe zu erstellen.

   | Feld | Wert |
   | ----- | ----- |
   | Ordner auswählen | Wählen Sie den Speicherort auf Ihrem Entwicklungscomputer aus, an dem Visual Studio Code die Projektmappendateien erstellen soll. |
   | Provide a solution name (Projektmappennamen angeben) | Geben Sie für Ihre Projektmappe einen aussagekräftigen Namen ein, oder übernehmen Sie den Standardnamen **EdgeSolution**. |
   | Select module template (Modulvorlage auswählen) | Wählen Sie **C-Modul** aus. |
   | Provide a module name (Modulname angeben) | Nennen Sie das Modul **CModule**. |
   | Provide Docker image repository for the module (Docker-Imagerepository für das Modul angeben) | Ein Imagerepository enthält den Namen Ihrer Containerregistrierung und den Namen Ihres Containerimages. Für das Containerimage wird der von Ihnen im vorherigen Schritt angegebene Name übernommen. Ersetzen Sie **localhost:5000** durch den Anmeldeserverwert aus Ihrer Azure-Containerregistrierung. Den Anmeldeserver können Sie im Azure-Portal auf der Übersichtsseite Ihrer Containerregistrierung ermitteln. <br><br> Das endgültige Imagerepository sieht wie folgt aus: \<Registrierungsname\>.azurecr.io/cmodule. |
 
   ![Bereitstellen eines Docker-Imagerepositorys](./media/tutorial-c-module/repository.png)

Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Der Arbeitsbereich der Projektmappe enthält fünf Komponenten auf oberster Ebene. Der Ordner **Module** enthält den C-Code für Ihre Module sowie Dockerfiles für die Erstellung Ihres Moduls als Containerimage. In der Datei **\.env** werden Ihre Anmeldeinformationen für die Containerregistrierung gespeichert. Die Datei **deployment.template.json** enthält die Informationen, die die IoT Edge-Runtime zum Bereitstellen von Modulen auf einem Gerät verwendet. Die Datei **deployment.debug.template.json** enthält die Debugversion der Module. In diesem Tutorial wird nicht der Ordner **\.vscode** oder die Datei **\.gitignore** bearbeitet.

Wenn Sie beim Erstellen Ihrer Lösung keine Containerregistrierung angegeben, aber den Standardwert „localhost:5000“ übernommen haben, besitzen Sie keine Datei vom Typ „\.env“.

<!--
   ![C solution workspace](./media/tutorial-c-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Hinzufügen von Registrierungsanmeldeinformationen

Die Umgebungsdatei speichert die Anmeldeinformationen für Ihre Containerregistrierung und gibt sie an die IoT Edge-Runtime weiter. Die Runtime benötigt diese Anmeldeinformationen, um Ihre privaten Images per Pull auf das IoT Edge-Gerät zu übertragen.

1. Öffnen Sie im VS Code-Explorer die ENV-Datei.
2. Aktualisieren Sie die Felder mit den Werten für **Benutzername** und **Kennwort**, die Sie aus der Azure-Containerregistrierung kopiert haben.
3. Speichern Sie diese Datei.

### <a name="update-the-module-with-custom-code"></a>Aktualisieren des Moduls mit benutzerdefiniertem Code

Fügen Sie dem C-Modul Code hinzu, der ihm das Lesen der Daten vom Sensor ermöglicht. Überprüfen Sie, ob die gemeldete Computertemperatur einen sicheren Schwellenwert überschritten hat, und übergeben Sie dann diese Informationen an IoT Hub.

5. Die Sensordaten in diesem Szenario liegen im JSON-Format vor. Importieren Sie zum Filtern von Nachrichten im JSON-Format eine JSON-Bibliothek für C. In diesem Tutorial wird Parson verwendet.

   1. Laden Sie das [Parson-Repository von GitHub](https://github.com/kgabis/parson) herunter. Kopieren Sie die Dateien **parson.c** und **parson.h** in den Ordner **CModule**.

   2. Öffnen Sie **Module** > **CModule** > **CMakeLists.txt**. Importieren Sie am Anfang der Datei die Parson-Dateien als Bibliothek namens **my_parson**.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. Fügen Sie in der Funktion **target_link_libraries** von „CMakeLists.txt“ der Liste der Bibliotheken **my_parson** hinzu.

   4. Speichern Sie die Datei **CMakeLists.txt**.

   5. Öffnen Sie **Module** > **CModule** > **main.c**. Fügen Sie am Ende der Liste mit include-Anweisungen eine neue Anweisung hinzu, um `parson.h` zur JSON-Unterstützung einzuschließen:

      ```c
      #include "parson.h"
      ```

6. Fügen Sie in der Datei **main.c** nach dem include-Abschnitt eine globale Variable namens `temperatureThreshold` ein. Mit dieser Variable wird der Wert festgelegt, den die gemessene Temperatur übersteigen muss, damit die Daten an IoT Hub gesendet werden.

    ```c
    static double temperatureThreshold = 25;
    ```

7. Ersetzen Sie die gesamte `CreateMessageInstance`-Funktion durch den folgenden Code. Diese Funktion weist einen Kontext für den Rückruf zu.

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

8. Ersetzen Sie die gesamte `InputQueue1Callback`-Funktion durch den folgenden Code. Diese Funktion implementiert den Nachrichtenfilter.

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

10. Ersetzen Sie die Funktion `SetupCallbacksForModule` durch den folgenden Code:

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

11. Speichern Sie die Datei „main.c“.

12. Öffnen Sie im VS Code-Explorer die Datei **deployment.template.json** im Arbeitsbereich für Ihre IoT Edge-Projektmappe. Über diese Datei erhält der IoT Edge-Agent die Informationen dazu, welche Module bereitgestellt werden sollen (hier: **tempSensor** und **CModule**). Außerdem wird der IoT Edge-Hub angewiesen, wie die Nachrichten weitergeleitet werden sollen. Die Visual Studio Code-Erweiterung fügt zwar automatisch die meisten Informationen ein, die Sie für die Bereitstellungsvorlage benötigen, aber Sie sollten überprüfen, ob alle Angaben für Ihre Lösung stimmen: 

   1. Die Standardplattform Ihrer IoT Edge-Instanz ist auf Ihrer VS Code-Statusleiste auf **amd64** festgelegt. Dies bedeutet, dass **CModule** auf die Linux-amd64-Version des Images festgelegt ist. Ändern Sie die Standardplattform auf der Statusleiste von **amd64** in **arm32v7**, sofern es sich dabei um die Architektur Ihres IoT Edge-Geräts handelt. 

      ![Aktualisieren der Modulimageplattform](./media/tutorial-c-module/image-platform.png)

   2. Stellen Sie sicher, dass die Vorlage den richtigen Modulnamen und nicht den Standardnamen **SampleModule** besitzt, den Sie beim Erstellen der IoT Edge-Lösung geändert haben.

   3. Im Abschnitt **registryCredentials** werden Ihre Anmeldeinformationen für die Docker-Registrierung gespeichert, damit der IoT Edge-Agent Ihr Modulimage per Pullvorgang abrufen kann. Die tatsächlichen Benutzername-/Kennwortpaare werden in der ENV-Datei gespeichert. Diese Datei wird von Git ignoriert. Fügen Sie Ihre Anmeldeinformationen der ENV-Datei hinzu, falls Sie dies noch nicht durchgeführt haben.  

   4. Weitere Informationen zu Bereitstellungsmanifesten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen in IoT Edge](module-composition.md).

13. Fügen Sie den Modulzwilling „CModule“ zum Bereitstellungsmanifest hinzu. Fügen Sie am Ende des Abschnitts `moduleContent` nach dem Modulzwilling `$edgeHub` den folgenden JSON-Inhalt ein:

    ```json
        "CModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

   ![Hinzufügen des CModule-Zwillings zur Bereitstellungsvorlage](./media/tutorial-c-module/module-twin.png)

14. Speichern Sie die Datei **deployment.template.json**.

## <a name="build-and-push-your-solution"></a>Erstellen und Pushen Ihrer Projektmappe

Im vorherigen Abschnitt haben Sie eine IoT Edge-Projektmappe erstellt und Code zu CModule hinzugefügt, der Nachrichten herausfiltert, deren gemeldete Computertemperatur innerhalb der zulässigen Grenzwerte liegt. Nun müssen Sie die Projektmappe als Containerimage erstellen und per Push an die Containerregistrierung übertragen.

1. Öffnen Sie das in VS Code integrierte Terminal, indem Sie **Ansicht** > **Integriertes Terminal** auswählen.

1. Melden Sie sich bei Docker an. Geben Sie dazu im integrierten Terminal von Visual Studio Code den folgenden Befehl ein. Sie müssen sich mit den Anmeldeinformationen für Azure Container Registry anmelden, damit Sie Ihr Modulimage an die Registrierung übertragen können.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Verwenden Sie den Benutzernamen, das Kennwort und den Anmeldeserver, die Sie im ersten Abschnitt aus Azure Container Registry kopiert haben. Oder rufen Sie sie erneut über den Abschnitt **Zugriffsschlüssel** der Registrierung im Azure-Portal ab.

2. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **deployment.template.json**, und klicken Sie anschließend auf **Build and Push IoT Edge solution** (IoT Edge-Projektmappe erstellen und übertragen).

Wenn Sie Visual Studio Code anweisen, die Projektmappe zu erstellen, wird zuerst die Datei `deployment.json` in einem neuen Konfigurationsordner (**config**) generiert. Die Informationen für die Datei „deployment.json“ stammen aus der von Ihnen aktualisierten Vorlagendatei, der ENV-Datei, die Sie zum Speichern der Anmeldeinformationen für Ihre Containerregistrierung verwendet haben, und der Datei „module.json“ im Ordner „CModule“.

Anschließend werden von Visual Studio Code zwei Befehle im integrierten Terminal ausgeführt: `docker build` und `docker push`. Diese beiden Befehle erstellen den Code, packen `CModule.dll` in Container und führen dann eine Pushübertragung an die Containerregistrierung durch, die Sie beim Initialisieren der Projektmappe angegeben haben.

Sie können die vollständige Adresse des Containerimages mit Tag im integrierten VS Code-Terminal anzeigen. Die Imageadresse wird auf der Grundlage der Informationen in der Datei `module.json` erstellt und hat folgendes Format: **\<Repository\>:\<Version\>-\<Plattform\>**. In diesem Tutorial sollte sie wie folgt aussehen: **myregistry.azurecr.io/cmodule:0.0.1-amd64**.

>[!TIP]
>Wenn beim Versuch, Ihr Modul zu erstellen und zu pushen, ein Fehler auftritt, führen Sie die folgenden Überprüfungen durch:
>* Haben Sie sich mit den Anmeldeinformationen aus Ihrer Containerregistrierung bei Docker in Visual Studio Code angemeldet? Dabei handelt es sich um andere Anmeldeinformationen, als Sie zur Anmeldung beim Azure-Portal verwenden.
>* Ist Ihr Containerrepository richtig? Öffnen Sie **modules** > **cmodule** > **module.json**, und suchen Sie das Feld **Repository**. Das Imagerepository sollte wie folgt aussehen: **\<Registrierungsname\>.azurecr.io/cmodule**. 
>* Erstellen Sie die gleiche Art von Containern, die von Ihrem Entwicklungscomputer ausgeführt wird? Visual Studio Code ist standardmäßig auf Container vom Typ „Linux amd64“ festgelegt. Wenn auf Ihrem Entwicklungscomputer Container vom Typ „Linux arm32v7“ ausgeführt werden, aktualisieren Sie die Plattform auf der blauen Statusleiste am unteren Rand des VS Code-Fensters so, dass sie Ihrer Containerplattform entspricht. C-Module können nicht als Windows-Container erstellt werden. 

## <a name="deploy-and-run-the-solution"></a>Bereitstellen und Ausführen der Projektmappe

Im Schnellstartartikel, anhand dessen Sie Ihr IoT Edge-Gerät eingerichtet haben, haben Sie ein Modul über das Azure-Portal bereitgestellt. Sie können Module auch mithilfe der Azure IoT Hub-Toolkit-Erweiterung (bisher als Azure IoT-Toolkit-Erweiterung bekannt) für Visual Studio Code bereitstellen. Sie haben bereits ein Bereitstellungsmanifest für Ihr Szenario vorbereitet: die Datei **deployment.json**. Nun müssen Sie nur noch ein Gerät auswählen, um die Bereitstellung zu empfangen.

1. Führen Sie in der Befehlspalette von VS Code den Befehl **Azure IoT Hub: Select IoT Hub** aus.

2. Wählen Sie das Abonnement und den IoT-Hub aus, die das zu konfigurierende IoT Edge-Gerät enthalten.

3. Erweitern Sie im VS Code-Explorer den Abschnitt **Azure IoT Hub-Geräte**.

4. Klicken Sie mit der rechten Maustaste auf den Namen Ihres IoT Edge-Geräts, und klicken Sie dann auf **Create Deployment for Single Device** (Bereitstellung für einzelnes Gerät erstellen).

   ![Erstellen einer Bereitstellung für ein einzelnes Gerät](./media/tutorial-c-module/create-deployment.png)

5. Wählen Sie im Konfigurationsordner (**config**) die Datei **deployment.json** aus, und klicken Sie auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen). Verwenden Sie nicht die Datei „deployment.template.json“.

6. Klicken Sie auf die Schaltfläche „Aktualisieren“. Nun sollte das neue **CModule**-Modul zusammen mit dem **TempSensor**-Modul sowie **$edgeAgent** und **$edgeHub** ausgeführt werden.

## <a name="view-generated-data"></a>Anzeigen generierter Daten

Sobald Sie das Bereitstellungsmanifest auf Ihr IoT Edge-Gerät angewendet haben, erfasst die IoT Edge-Runtime auf dem Gerät die neuen Bereitstellungsinformationen und verwendet sie bei der Ausführung. Alle auf dem Gerät ausgeführten Module, die nicht im Bereitstellungsmanifest enthalten sind, werden beendet. Module, die auf dem Gerät fehlen, werden gestartet.

Der Status Ihres IoT Edge-Geräts wird im Abschnitt **Azure IoT Hub Devices** (Azure IoT Hub-Geräte) des Visual Studio Code-Explorers angezeigt. Erweitern Sie die Details Ihres Geräts, um eine Liste mit den bereitgestellten und ausgeführten Modulen anzuzeigen.

Auf dem IoT Edge-Gerät selbst können Sie den Status Ihrer Bereitstellungsmodule mithilfe des Befehls `iotedge list` anzeigen. Es sollten vier Module angezeigt werden: die beiden IoT Edge-Runtime-Module, „tempSensor“ und das benutzerdefinierte Modul, das Sie in diesem Tutorial erstellt haben. Es kann einige Minuten dauern, bis alle Module gestartet wurden. Führen Sie den Befehl daher erneut aus, falls nicht gleich alle Module angezeigt werden.

Mit dem Befehl `iotedge logs <module name>` können Sie die Nachrichten anzeigen, die von einem Modul generiert werden.

Mithilfe von Visual Studio Code können Sie die bei Ihrem IoT-Hub eingehenden Nachrichten anzeigen.

1. Klicken Sie zum Überwachen der bei IoT Hub eingehenden Daten auf **...** und dann auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).
2. Wenn Sie die D2C-Nachrichten für ein bestimmtes Gerät überwachen möchten, klicken Sie mit der rechten Maustaste auf das Gerät in der Liste, und klicken Sie dann auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).
3. Wenn Sie die Datenüberwachung beenden möchten, führen Sie in der Befehlspalette den Befehl **Azure IoT Hub: Stop monitoring D2C message** aus.
4. Klicken Sie zum Anzeigen oder Aktualisieren des Modulzwillings mit der rechten Maustaste auf das Modul in der Liste, und klicken Sie auf **Edit module twin** (Modulzwilling bearbeiten). Speichern Sie zum Aktualisieren des Modulzwillings die JSON-Datei des Zwillings, klicken Sie mit der rechten Maustaste auf den Editorbereich, und wählen Sie **Update Module Twin** (Modulzwilling aktualisieren).
5. Zum Anzeigen von Docker-Protokollen können Sie [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) für VS Code installieren und Ihre ausgeführten Module lokal im Docker-Explorer suchen. Klicken Sie zum Anzeigen im integrierten Terminal im Kontextmenü auf **Protokolle anzeigen**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden. Sie können auch dasselbe IoT Edge-Gerät als Testgerät weiter nutzen.

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul erstellt, das Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Wenn Sie eigene Module erstellen möchten, finden Sie entsprechende Informationen unter [Use Visual Studio Code to develop and debug C modules for Azure IoT Edge (Entwickeln eines C-Moduls mit Azure IoT Edge für Visual Studio Code)](how-to-develop-c-module.md). Sie können mit den nächsten Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Speichern von Daten im Edge-Bereich mit SQL Server-Datenbanken](tutorial-store-data-sql-server.md)


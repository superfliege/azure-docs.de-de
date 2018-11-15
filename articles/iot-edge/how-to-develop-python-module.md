---
title: Debuggen von Python-Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Entwickeln, Erstellen und Debuggen eines Python-Moduls für Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/13/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 0dfe096bb3a2a2116ead2423f53a5e44c8f02630
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567518"
---
# <a name="use-visual-studio-code-to-develop-and-debug-python-modules-for-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Debuggen von Python-Modulen für Azure IoT Edge

Sie können Ihre Geschäftslogik in Azure IoT Edge-Module umwandeln. In diesem Artikel wird veranschaulicht, wie Sie Visual Studio Code (VS Code) als Haupttool zum Entwickeln und Debuggen von Python-Modulen verwenden.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Darüber hinaus simulieren Sie Ihr IoT Edge-Gerät auf Ihrem Entwicklungscomputer mit IoT Edge-Sicherheits-Daemon.

> [!NOTE]
> In diesem Debugartikel werden das Anfügen eines Prozesses in einem Modulcontainer und das Debuggen mit Visual Studio Code beschrieben. Sie können nur Python-Module in Containern vom Typ „Linux amd64“ debuggen. Wenn Sie mit den Debugfunktionen von Visual Studio Code nicht vertraut sind, informieren Sie sich über [Debuggen](https://code.visualstudio.com/Docs/editor/debugging). 

Weil in diesem Artikel Visual Studio Code als Hauptentwicklungstool verwendet wird, installieren Sie VS Code. Fügen Sie dann die erforderlichen Erweiterungen hinzu:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python) für Visual Studio Code 
* [Python](https://www.python.org/downloads/).
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) zum Installieren von Python-Paketen (normalerweise in der Python-Installation enthalten).
* Installieren Sie **Cookiecutter** mithilfe des folgenden Befehls:
   
    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

Um ein Modul zu erstellen, benötigen Sie Docker zum Erstellen des Modulimages und eine Containerregistrierung zum Speichern des Modulimages:
* [Docker Community Edition](https://docs.docker.com/install/) auf Ihrem Entwicklungscomputer. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden. 

Um das Modul auf einem Gerät zu testen, benötigen Sie einen aktiven IoT Hub mit mindestens einem IoT Edge-Gerät. Um Ihren Computer als IoT Edge-Gerät zu verwenden, führen Sie die Schritte in der Schnellstartanleitung für [Linux](quickstart-linux.md) aus. 

## <a name="create-a-new-solution-template"></a>Erstellen einer neuen Lösungsvorlage

Gehen Sie wie folgt vor, um mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis des Azure IoT Python SDK zu erstellen. Als Erstes erstellen Sie eine Projektmappe, und dann generieren Sie das erste Modul in dieser Projektmappe. Jede Projektmappe kann mehrere Module enthalten. 

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Integriertes Terminal**.

2. Wählen Sie **Ansicht** > **Befehlspalette** aus. 

3. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus.

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

4. Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll. Wählen Sie **Ordner auswählen** aus. 

5. Geben Sie einen Namen für die Lösung ein. 

6. Wählen Sie **Python-Modul** als Vorlage für das erste Modul in der Projektmappe aus.

7. Geben Sie einen Namen für Ihr Modul ein. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist. 

8. Geben Sie den Namen des Imagerepositorys des Moduls an. VS Code trägt als Modulnamen automatisch **localhost:5000** ein. Ersetzen Sie ihn durch Ihre eigenen Registrierungsinformationen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie **localhost** nutzen. Nutzen Sie den Anmeldeserver aus Ihren Registrierungseinstellungen, wenn Sie Azure Container Registry verwenden. Der Anmeldeserver hat die Form **\<Registrierungsname\>.azurecr.io**. Ersetzen Sie nur den localhost-Teil der Zeichenfolge, löschen Sie nicht Ihren Modulnamen. 

   ![Bereitstellen eines Docker-Imagerepositorys](./media/how-to-develop-c-module/repository.png)

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe, und lädt diese in einem neuen Fenster.

Es gibt vier Elemente in der Projektmappe: 
* Einen Ordner **.vscode**, der Debugkonfigurationen enthält.
* Einen Ordner **modules**, der Unterordner für jedes Modul enthält. An diesem Punkt haben Sie nur einen. Sie können aber über die Befehlspalette mit dem Befehl **Azure IoT Edge: Add IoT Edge Module** weitere hinzufügen. 
* Eine **.env**-Datei, in der Ihre Umgebungsvariablen aufgeführt werden. Wenn Azure Container Registry Ihre Registrierung ist, enthält sie einen Azure Container Registry-Benutzernamen und ein Kennwort. 

   > [!NOTE]
   > Die Umgebungsdatei wird nur erstellt, wenn Sie ein Imagerepository für das Modul angeben. Wenn Sie die „localhost“-Standardeinstellungen zum lokalen Testen und Debuggen akzeptiert haben, müssen Sie keine Umgebungsvariablen deklarieren. 

* Eine Datei **deployment.template.json**, in der Ihr neues Modul zusammen mit dem Beispielmodul **tempSensor**, mit dem Daten zum Testen simuliert werden, aufgeführt ist. Weitere Informationen zur Funktionsweise von Bereitstellungsmanifesten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen mithilfe von Bereitstellungsmanifesten](module-composition.md). 

## <a name="develop-your-module"></a>Entwickeln Ihres Moduls

Der in der Projektmappe enthaltene Python-Standardmodulcode befindet sich unter **modules** > [Name Ihres Moduls] > **main.py**. Das Modul und die Datei „deployment.template.json“ werden so eingerichtet, dass Sie die Projektmappe erstellen, in Ihre Containerregistrierung verschieben und zum Starten des Tests für ein Gerät bereitstellen können, ohne Code ändern zu müssen. Das Modul ist so konzipiert, dass es einfach eine Eingabe aus einer Quelle akzeptiert (in diesem Fall das Daten simulierende tempSensor-Modul) und IoT Hub übergibt. 

Wenn Sie bereit sind, die Python-Vorlage mit Ihrem eigenen Code anzupassen, erstellen Sie mit den [Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md) Module, die die wesentlichen Anforderungen für IoT-Lösungen wie Sicherheit, Geräteverwaltung und Zuverlässigkeit abdecken. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Erstellen und Bereitstellen Ihres Moduls zum Debuggen

In jedem Modulordner gibt es mehrere Docker-Dateien für unterschiedliche Containertypen. Verwenden Sie jede Datei mit der Erweiterung **.debug**, um Ihr Modul zum Testen zu erstellen. Python-Module unterstützen derzeit nur das Debuggen in Containern vom Typ „Linux-amd64“. 

1. Wechseln Sie in Visual Studio Code zu der Datei `deployment.template.json`. Aktualisieren Sie Ihre Modulimage-URL, indem Sie am Ende **.debug** hinzufügen.

2. Ersetzen Sie „createOptions“ für das Python-Modul in **deployment.template.json** durch den unten angegebenen Code, und speichern Sie die Datei: 
    
    ```json
    "createOptions": "{\"ExposedPorts\":{\"5678/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"5678/tcp\":[{\"HostPort\":\"5678\"}]}}}"
    ```

3. Navigieren Sie zu `main.py`, und fügen Sie den folgenden Code nach dem Importabschnitt hinzu:
    
    ```python
    import ptvsd
    ptvsd.enable_attach(('0.0.0.0',  5678))
    ```

4. Fügen Sie in den Rückruf, den Sie debuggen möchten, die folgende Codezeile ein:

    ```python
    ptvsd.break_into_debugger()
    ```

   Nehmen wir beispielsweise an, Sie möchten die Methode `receive_message_callback` debuggen. In diesem Fall können Sie die Codezeile wie folgt einfügen:

    ```python
    def receive_message_callback(message, hubManager):
        ptvsd.break_into_debugger()
        global RECEIVE_CALLBACKS
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode('utf-8'), size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

2. Geben Sie in der Befehlspalette von Visual Studio Code den Befehl **Azure IoT Edge: Build and Push IoT Edge solution** ein, und führen Sie ihn aus.
3. Wählen Sie in der Befehlspalette die Datei `deployment.template.json` für die Projektmappe aus. 
4. Klicken Sie im Azure IoT Hub Device Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID. Klicken Sie dann auf **Create Deployment for Single Device** (Bereitstellung für einzelnes Gerät erstellen). 
5. Öffnen Sie den **config**-Ordner Ihrer Projektmappe. Wählen Sie dann die `deployment.json`-Datei aus. Wählen Sie **Edge-Bereitstellungsmanifest auswählen** aus. 

Sie sehen nun, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Überprüfen Sie Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker ps` im Terminal.

## <a name="start-debugging-python-module-in-vs-code"></a>Debuggen des Python-Moduls in VS Code
VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird. 

1. Navigieren Sie zur VS Code-Debugansicht. Wählen Sie die Debugkonfigurationsdatei für Ihr Modul aus. Der Name der Debugoption sollte **<Modulename> Remote Debug (Python)** ähneln.

2. Navigieren Sie zu `main.c`. Fügen Sie in die Rückrufmethode, die Sie mit `ptvsd.break_into_debugger()` versehen haben, einen Breakpoint ein.

3. Wählen Sie **Debuggen starten** oder **F5** aus. Wählen Sie den Prozess zum Anfügen aus.

4. In der Debugansicht von VS Code sehen Sie die Variablen im linken Bereich. 

Das obigen Beispiel zeigt, wie Sie Python-IoT Edge-Module in Containern debuggen. Dem createOptions-Modulcontainer wurden verfügbar gemachte Ports hinzugefügt. Nachdem Sie das Debuggen Ihrer Python-Module abgeschlossen haben, empfiehlt es sich, diese verfügbar gemachten Ports für produktionsbereite IoT Edge-Module zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).

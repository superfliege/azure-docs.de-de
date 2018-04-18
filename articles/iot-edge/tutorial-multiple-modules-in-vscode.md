---
title: Arbeiten mit mehreren IoT Edge-Modulen in Visual Studio Code | Microsoft Docs
description: Bereitstellen von Azure Machine Learning als Modul auf einem Edgegerät
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 6c94701507f86f6ecab2875f952215cc3e4cc719
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code---preview"></a>Entwickeln einer IoT Edge-Projektmappe mit mehreren Modulen in Visual Studio Code – Vorschau
Sie können Visual Studio Code verwenden, um Ihre IoT Edge-Projektmappe mit mehreren Modulen zu entwickeln. In diesem Artikel wird Schritt für Schritt das Erstellen, Aktualisieren und Bereitstellen einer IoT Edge-Projektmappe beschrieben, die Sensordaten des simulierten IoT Edge-Geräts in Visual Studio Code weiterleitet. In diesem Artikel werden folgende Vorgehensweisen behandelt:

* Verwenden von Visual Studio Code zum Erstellen einer IoT Edge-Projektmappe
* Verwenden von VS Code zum Hinzufügen eines neuen Moduls zu Ihrer funktionierenden IoT Edge-Projektmappe. 
* Bereitstellen der IoT Edge-Projektmappe (mehrere Module) auf Ihrem IoT Edge-Gerät
* Anzeigen generierter Daten

## <a name="prerequisites"></a>Voraussetzungen
* Absolvieren der unten genannten Tutorials
  * [Bereitstellen eines C#-Moduls](tutorial-csharp-module.md)
  * [Bereitstellen einer C#-Funktion](tutorial-deploy-function.md)
  * [Bereitstellen eines Python-Moduls](tutorial-python-module.md)
* [Docker für VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) mit Explorerintegration für das Verwalten von Images und Containern.


## <a name="prepare-your-first-iot-edge-solution"></a>Vorbereiten Ihrer ersten IoT Edge-Projektmappe
1. Geben Sie in der VS Code-Befehlspalette den Befehl **Edge: New IoT Edge solution** ein, und führen Sie ihn aus. Wählen Sie dann Ihren Arbeitsbereichsordner aus, geben Sie den Namen der Projektmappe ein (der Standardname ist **EdgeSolution**), und erstellen Sie ein C#-Modul (**SampleModule**) als erstes Benutzermodul in dieser Projektmappe. Sie müssen außerdem das Docker-Image-Repository für Ihr erstes Modul angeben. Das Image-Standardrepository basiert auf einer lokalen Docker-Registrierung (`localhost:5000/<first module name>`). Sie können es aber auch in die Azure-Containerregistrierung oder den Docker-Hub ändern.

> [!NOTE]
> Wenn Sie eine lokale Docker-Registrierung verwenden, vergewissern Sie sich, dass die Registrierung ausgeführt wird, indem Sie den Befehl `docker run -d -p 5000:5000 --restart=always --name registry registry:2` in Ihr Konsolenfenster eingeben.

2. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Im Stammordner befinden sich ein Ordner `modules`, ein Ordner `.vscode` und eine Bereitstellungsmanifest-Vorlagendatei. Debugkonfigurationen können Sie im Ordner `.vscode` sehen. Jeglicher Code von Benutzermodulen wird in Unterordnern des Ordners `modules` gespeichert. `deployment.template.json` ist die Vorlage für das Bereitstellungsmanifest. Einige der Parameter in dieser Datei werden durch Analyse von `module.json` gewonnen, die in jedem Modulordner vorhanden ist.

3. Fügen Sie diesem Projektmappenprojekt Ihr zweites Modul hinzu. Geben Sie diesmal **Edge: Add IoT Edge module** ein, führen Sie den Befehl aus, und wählen Sie die zu aktualisierende Bereitstellungs-Vorlagendatei aus. Wählen Sie dann eine **Azure-Funktion – C#** mit dem Namen **SampleFunction** und das Docker-Imagerepository aus, dem sie hinzugefügt werden soll.

4. Jetzt ist Ihre erste IoT Edge-Projektmappe mit zwei einfachen Modulen fertig. Das C#-Standardmodul fungiert als Nachrichten-Pipe-Modul, während die C#-Funktion als Nachrichten-Pipe-Funktion arbeitet. In der `deployment.template.json` können Sie sehen, dass diese Projektmappe drei Module enthält. Die Nachricht wird vom `tempSensor`-Modul generiert, direkt über `SampleModule` und `SampleFunction` weitergeleitet und dann an Ihren IoT-Hub gesendet. Aktualisieren Sie die Routen für diese Module mit den unten aufgeführten Inhalten. 
   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Speichern Sie diese Datei.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Erstellen und Bereitstellen Ihrer IoT Edge-Projektmappe
1. Geben Sie in der VS Code-Befehlspalette den Befehl **Edge: IoT Edge-Projektmappe erstellen** ein, und führen Sie ihn aus. Auf der Grundlage der `module.json`-Datei in den einzelnen Modulordnern überprüft dieser Befehl das Docker-Image jedes Moduls, beginnt mit seiner Erstellung, verpackt es in einen Container und überträgt es per Push. Anschließend analysiert es den erforderlichen Wert für `deployment.template.json` und generiert `deployment.json` mit dem tatsächlichen Wert im Ordner `config`. Sie können den Buildstatus im integrierten Terminal von VS Code ablesen.

2. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Create deployment for Edge device** (Bereitstellung für Edge-Gerät erstellen) aus. Wählen Sie die Datei `deployment.json` unter dem Ordner `config` aus. Sie können nun sehen, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

3. Wenn Sie auf dem Bereitstellungscomputer [ein IoT Edge-Gerät simulieren](tutorial-simulate-device-linux.md), werden Sie feststellen, dass alle Container mit Modul-Images innerhalb weniger Minuten gestartet werden.

## <a name="view-generated-data"></a>Anzeigen generierter Daten

1. Zum Überwachen von Daten, die beim IoT Hub eingehen, wählen Sie **Ansicht** > **Befehlspalette...** aus, und suchen Sie nach **IoT: Start monitoring D2C message** (Überwachung von Gerät-zu-Cloud-Nachricht starten). 
2. Zum Beenden der Datenüberwachung verwenden Sie den Befehl **IoT: Stop monitoring D2C message** (IoT: Überwachung von Gerät-zu-Cloud-Nachricht beenden). 

## <a name="next-steps"></a>Nächste Schritte

Sie können mit einem der beiden folgenden Artikel fortfahren, um andere Szenarien bei der Entwicklung von Azure IoT Edge in Visual Studio Code kennenzulernen:

* [Verwenden von Visual Studio Code zum Debuggen eines C#-Moduls mit Azure IoT Edge](how-to-vscode-debug-csharp-module.md)
* [Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge](how-to-vscode-debug-azure-function.md)
---
title: Verwalten mehrerer Azure IoT Edge-Module in VS Code | Microsoft-Dokumentation
description: Verwenden Sie Visual Studio Code, um IoT Edge-Lösungen zu entwickeln, für die mehrere Module genutzt werden.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4d9caa7aa95c99fa30e8ec76c5b6362615725622
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34166351"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code---preview"></a>Entwickeln einer IoT Edge-Projektmappe mit mehreren Modulen in Visual Studio Code – Vorschau
Sie können Visual Studio Code verwenden, um Ihre IoT Edge-Projektmappe mit mehreren Modulen zu entwickeln. In diesem Artikel wird Schritt für Schritt das Erstellen, Aktualisieren und Bereitstellen einer IoT Edge-Projektmappe beschrieben, die Sensordaten des simulierten IoT Edge-Geräts in Visual Studio Code weiterleitet. 

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein, damit Sie alle Schritte dieses Artikels ausführen können:

- [Visual Studio Code](https://code.visualstudio.com/) 
- [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [C#-Erweiterung für Visual Studio Code (unterstützt von OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 
- AzureIoTEdgeModule-Vorlage (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Ein aktiver IoT Hub mit mindestens einem IoT Edge-Gerät


* [Docker für VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) mit Explorerintegration für das Verwalten von Images und Containern.


## <a name="prepare-your-first-iot-edge-solution"></a>Vorbereiten Ihrer ersten IoT Edge-Projektmappe
1. Geben Sie in der VS Code-Befehlspalette den Befehl **Edge: New IoT Edge solution** ein, und führen Sie ihn aus. Wählen Sie dann Ihren Arbeitsbereichsordner aus, geben Sie den Namen der Projektmappe ein (der Standardname ist **EdgeSolution**), und erstellen Sie ein C#-Modul (**SampleModule**) als erstes Benutzermodul in dieser Projektmappe. Sie müssen außerdem das Docker-Image-Repository für Ihr erstes Modul angeben. Das Image-Standardrepository basiert auf einer lokalen Docker-Registrierung (`localhost:5000/<first module name>`). Sie können es aber auch in die Azure-Containerregistrierung oder den Docker-Hub ändern.

   > [!NOTE]
   > Wenn Sie eine lokale Docker-Registrierung verwenden, sollten Sie sich vergewissern, dass die Registrierung ausgeführt wird, indem Sie den Befehl `docker run -d -p 5000:5000 --restart=always --name registry registry:2` in Ihr Konsolenfenster eingeben.

2. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Der Stammordner enthält den Ordner `modules`, den Ordner `.vscode` und eine Bereitstellungsmanifest-Vorlagendatei. Debugkonfigurationen können Sie im Ordner `.vscode` sehen. Jeglicher Code von Benutzermodulen wird in Unterordnern des Ordners `modules` gespeichert. `deployment.template.json` ist die Vorlage für das Bereitstellungsmanifest. Einige der Parameter in dieser Datei werden durch Analyse von `module.json` gewonnen, die in jedem Modulordner vorhanden ist.

3. Fügen Sie diesem Projektmappenprojekt Ihr zweites Modul hinzu. Geben Sie diesmal **Edge: Add IoT Edge module** ein, führen Sie den Befehl aus, und wählen Sie die zu aktualisierende Bereitstellungs-Vorlagendatei aus. Wählen Sie dann eine **Azure-Funktion – C#** mit dem Namen **SampleFunction** und das Docker-Imagerepository aus.

4. Öffnen Sie die Datei `deployment.template.json`, und stellen Sie sicher, dass darin zusätzlich zur Laufzeit drei Module deklariert werden. Die Nachricht wird vom `tempSensor`-Modul generiert, direkt über `SampleModule` und `SampleFunction` weitergeleitet und dann an Ihren IoT-Hub gesendet. 
5. Aktualisieren Sie die Routen für diese Module mit dem folgenden Inhalt:
   ```json
   "routes": {
      "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
      "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
      "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
   },
   ```

6. Speichern Sie diese Datei.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Erstellen und Bereitstellen Ihrer IoT Edge-Projektmappe
1. Geben Sie in der VS Code-Befehlspalette den Befehl **Edge: IoT Edge-Projektmappe erstellen** ein, und führen Sie ihn aus. Auf Grundlage der `module.json`-Datei in den einzelnen Modulordnern beginnt dieser Befehl damit, das Docker-Image jedes Moduls zu erstellen, in einen Container zu verpacken und per Pushvorgang zu übertragen. Anschließend wird der erforderliche Wert an `deployment.template.json` übergeben und die Datei `deployment.json` mit Informationen aus dem Ordner `config` generiert. Sie können den Buildstatus im integrierten Terminal von VS Code ablesen.

2. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Create deployment for Edge device** (Bereitstellung für Edge-Gerät erstellen) aus. Wählen Sie die Datei `deployment.json` unter dem Ordner `config` aus. Sie können nun sehen, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

3. Wenn Sie ein IoT Edge-Gerät auf Ihrem Entwicklungscomputer simulieren, sehen Sie, dass alle Modulimagecontainer in wenigen Minuten gestartet werden.

## <a name="view-generated-data"></a>Anzeigen generierter Daten

1. Zum Überwachen von Daten, die beim IoT Hub eingehen, wählen Sie **Ansicht** > **Befehlspalette...** aus, und suchen Sie nach **IoT: Start monitoring D2C message** (Überwachung von Gerät-zu-Cloud-Nachricht starten). 
2. Zum Beenden der Datenüberwachung verwenden Sie den Befehl **IoT: Stop monitoring D2C message** (IoT: Überwachung von Gerät-zu-Cloud-Nachricht beenden). 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über andere Szenarien zur Entwicklung von Azure IoT Edge in Visual Studio Code:

* [Verwenden von Visual Studio Code zum Debuggen eines C#-Moduls mit Azure IoT Edge](how-to-vscode-debug-csharp-module.md)
* [Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge](how-to-vscode-debug-azure-function.md)
---
title: Verwalten mehrerer Azure IoT Edge-Module in VS Code | Microsoft-Dokumentation
description: Verwenden Sie Visual Studio Code, um Azure IoT Edge-Lösungen zu entwickeln, die mehrere Module nutzen.
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 4a624994f93e7a3cbb04db2a0ec0b2b823a12ee7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763033"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code-preview"></a>Entwickeln einer IoT Edge-Lösung mit mehreren Modulen in Visual Studio Code (Vorschauversion)

Sie können Visual Studio Code verwenden, um eine Azure IoT Edge-Lösung mit mehreren Modulen zu entwickeln. In diesem Artikel erfahren Sie, wie Sie in VS Code eine IoT Edge-Lösung erstellen, aktualisieren und bereitstellen, die Sensordaten für ein simuliertes IoT Edge-Gerät weiterleitet. 

## <a name="prerequisites"></a>Voraussetzungen

Für die Schritte in diesem Artikel ist Folgendes erforderlich:

- [Visual Studio Code](https://code.visualstudio.com/)
- Die [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- Die [C#-Erweiterung für Visual Studio Code (unterstützt von OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- Das [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd)
- Die AzureIoTEdgeModule-Vorlage (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Ein aktiver IoT Hub mit mindestens einem IoT Edge-Gerät

Außerdem benötigen Sie [Docker für VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) mit Azure IoT Hub Device Explorer-Integration für die Verwaltung von Images und Containern.

## <a name="prepare-your-first-iot-edge-solution"></a>Vorbereiten Ihrer ersten IoT Edge-Projektmappe

1. Geben Sie in der **Befehlspalette** von VS Code den Befehl **Edge: New IoT Edge solution** ein, und führen Sie ihn aus. Wählen Sie Ihren Arbeitsbereichsordner aus, und geben Sie den Namen der Lösung (Standardwert: EdgeSolution) an. Erstellen Sie ein C#-Modul (mit dem Namen **SampleModule**) als erstes Benutzermodul in dieser Lösung. Sie müssen außerdem das Docker-Image-Repository für Ihr erstes Modul angeben. Das Image-Standardrepository basiert auf einer lokalen Docker-Registrierung (**localhost:5000/<first module name>**). Es kann aber in Azure Container Registry oder Docker Hub geändert werden.

   > [!NOTE]
   > Vergewissern Sie sich bei Verwendung einer lokalen Docker-Registrierung, dass die Registrierung aktiv ist. Geben Sie im Konsolenfenster den folgenden Befehl ein:
   > 
   > `docker run -d -p 5000:5000 --restart=always --name registry registry:2`

2. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Der Stammordner enthält den Ordner **modules**, den Ordner **.vscode** und eine Bereitstellungsmanifest-Vorlagendatei. Debugkonfigurationen befinden sich im Ordner „.vscode“. Alle Benutzermodulcodes sind Unterordner des Ordners „modules“. Die Datei „deployment.template.json“ ist die Bereitstellungsmanifestvorlage. Einige der Parameter in dieser Datei werden mittels Analyse der Datei „module.json“ gewonnen, die in jedem Modulordner vorhanden ist.

3. Fügen Sie diesem Lösungsprojekt Ihr zweites Modul hinzu. Geben Sie den Befehl **Edge: Add IoT Edge module** ein, und führen Sie ihn aus. Wählen Sie die zu aktualisierende Bereitstellungsvorlagendatei aus. Wählen Sie ein Modul vom Typ **Azure-Funktion – C#** mit dem Namen **SampleFunction** und das dazugehörige Docker-Imagerepository aus.

4. Öffnen Sie die Datei „deployment.template.json“. Vergewissern Sie sich, dass die Datei drei Module und die Laufzeit deklariert. Die Nachricht wird auf der Grundlage des Moduls „tempSensor“ generiert. Sie wird direkt über die Module „SampleModule“ und „SampleFunction“ weitergeleitet und dann an Ihre IoT Hub-Instanz gesendet. 

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

1. Geben Sie in der **Befehlspalette** von VS Code den Befehl **Edge: Build IoT Edge solution** ein, und führen Sie ihn aus. Dieser Befehl beginnt damit, auf der Grundlage der Datei „module.json“ in den einzelnen Modulordnern das Docker-Image jedes Moduls zu erstellen, in einen Container zu verpacken und zu pushen. Anschließend übergibt der Befehl den erforderlichen Wert an die Datei „deployment.template.json“ und generiert die Datei „deployment.json“ mit Informationen aus dem Ordner „config“. Der Buildstatus wird im integrierten Terminal in VS Code angezeigt.

2. Klicken Sie in Azure IoT Hub im **Device Explorer** mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und klicken Sie dann auf **Create deployment for Edge device** (Bereitstellung für Edge-Gerät erstellen) aus. Wählen Sie die Datei „deployment.json“ im Ordner „config“ aus. Im integrierten Terminal von VS Code sehen Sie, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID erstellt wurde.

3. Wenn Sie ein IoT Edge-Gerät auf Ihrem Entwicklungscomputer simulieren, beachten Sie, dass alle Modulimagecontainer innerhalb weniger Minuten gestartet werden.

## <a name="view-the-generated-data"></a>Anzeigen der generierten Daten

1. Klicken Sie zum Überwachen der Daten, die bei der IoT Hub-Instanz eingehen, auf **Ansicht** > **Befehlspalette**. Klicken Sie anschließend auf den Befehl **IoT: Start monitoring D2C message** (IoT:Überwachung von Gerät-zu-Cloud-Nachricht starten). 
2. Wenn Sie die Datenüberwachung beenden möchten, verwenden Sie den Befehl **IoT: Stop monitoring D2C message** (IoT: Überwachung von Gerät-zu-Cloud-Nachricht beenden) in der **Befehlspalette**. 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über weitere Szenarien zur Entwicklung mit Azure IoT Edge in Visual Studio Code:

* [Verwenden von Visual Studio Code zum Debuggen eines C#-Moduls mit Azure IoT Edge](how-to-vscode-debug-csharp-module.md)
* [Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge](how-to-vscode-debug-azure-function.md)

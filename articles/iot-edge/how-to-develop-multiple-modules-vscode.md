---
title: Arbeiten mit mehreren Azure IoT Edge-Modulen in Visual Studio Code | Microsoft-Dokumentation
description: Verwenden der IoT-Erweiterung für Visual Studio Code zum Entwickeln mehrerer Module für Azure IoT Edge gleichzeitig
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: d895e10c6b667a05156b2ce0f83f7776b9ba4f0a
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568759"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code"></a>Entwickeln einer IoT Edge-Projektmappe mit mehreren Modulen in Visual Studio Code

Sie können Visual Studio Code verwenden, um eine Azure IoT Edge-Lösung mit mehreren Modulen zu entwickeln. In diesem Artikel erfahren Sie, wie Sie in VS Code eine IoT Edge-Lösung erstellen, aktualisieren und bereitstellen, die Sensordaten für ein simuliertes IoT Edge-Gerät weiterleitet. 

## <a name="prerequisites"></a>Voraussetzungen

Für die Schritte in diesem Artikel ist Folgendes erforderlich:

- [Visual Studio Code](https://code.visualstudio.com/)
- Die [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- Die [C#-Erweiterung für Visual Studio Code (unterstützt von OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- Das [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)
- Ein aktiver IoT Hub mit mindestens einem IoT Edge-Gerät

Außerdem benötigen Sie [Docker für VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) mit Azure IoT Hub Device Explorer-Integration für die Verwaltung von Images und Containern.

## <a name="create-your-iot-edge-solution"></a>Erstellen Ihrer IoT Edge-Projektmappe

1. Öffnen Sie das in Visual Studio Code integrierte Terminal, indem Sie **Ansicht** > **Integriertes Terminal** auswählen. 

1. Geben Sie in der **Befehlspalette** von Visual Studio Code den Befehl **Azure IoT Edge: New IoT Edge solution** ein, und führen Sie ihn aus. Wählen Sie Ihren Arbeitsbereichsordner aus, und geben Sie den Namen der Lösung (Standardwert: EdgeSolution) an. Erstellen Sie ein C#-Modul (mit dem Namen **PipeModule**) als erstes Benutzermodul in dieser Projektmappe. Die Standardvorlage des C#-Moduls ist ein Pipemodul, das Nachrichten direkt von vorgeschalteten an nachgeschaltete Elemente weitergeleitet. Sie müssen außerdem das Docker-Image-Repository für Ihr erstes Modul angeben. Das Image-Standardrepository basiert auf einer lokalen Docker-Registrierung (**localhost:5000/<first module name>**). Es kann aber in Azure Container Registry oder Docker Hub geändert werden. 

2. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Der Stammordner enthält den Ordner **modules**, den Ordner **.vscode** und eine Bereitstellungsmanifest-Vorlagendatei. Debugkonfigurationen befinden sich im Ordner „.vscode“. Alle Benutzermodulcodes sind Unterordner des Ordners „modules“. Die Datei „deployment.template.json“ ist die Bereitstellungsmanifestvorlage. Einige der Parameter in dieser Datei werden mittels Analyse der Datei „module.json“ gewonnen, die in jedem Modulordner vorhanden ist.

3. Fügen Sie diesem Lösungsprojekt Ihr zweites Modul hinzu. Es gibt verschiedene Möglichkeiten zum Hinzufügen eines neuen Moduls zur aktuellen Projektmappe. Geben Sie den Befehl **Azure IoT Edge: Add IoT Edge module** ein, und führen Sie ihn aus. Wählen Sie die zu aktualisierende Bereitstellungsvorlagendatei aus. Sie können auch mit der rechten Maustaste auf den Ordner „modules“ oder auf die Datei „deployment.template.json“ klicken und **Add IoT Edge Module** auswählen. In der daraufhin angezeigten Dropdownliste können Sie den Modultyp auswählen. Wählen Sie ein Modul vom Typ **Azure Functions – C#** mit dem Namen **PipeFunction** und das zugehörige Docker-Imagerepository aus. Die Standardvorlage des C#-Functions-Moduls ist ein Pipemodul, das Nachrichten direkt von vorgeschalteten an nachgeschaltete Elemente weitergeleitet.

4. Öffnen Sie die Datei „deployment.template.json“. Vergewissern Sie sich, dass die Datei drei Module und die Laufzeit deklariert. Die Nachricht wird auf der Grundlage des Moduls „tempSensor“ generiert. Sie wird direkt über die Module „SampleModule“ und „SampleFunction“ weitergeleitet und dann an Ihre IoT Hub-Instanz gesendet. 

5. Aktualisieren Sie die Routen für diese Module mit dem folgenden Inhalt:

   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/PipeModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/PipeModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/PipeFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/PipeFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Speichern Sie diese Datei.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Erstellen und Bereitstellen Ihrer IoT Edge-Projektmappe

1. Geben Sie in der **Befehlspalette** von Visual Studio Code den Befehl **Azure IoT Edge: Build IoT Edge solution** ein, und führen Sie ihn aus. Dieser Befehl beginnt damit, auf der Grundlage der Datei „module.json“ in den einzelnen Modulordnern das Docker-Image jedes Moduls zu erstellen, in einen Container zu verpacken und zu pushen. Anschließend übergibt der Befehl den erforderlichen Wert an die Datei „deployment.template.json“ und generiert die Datei „deployment.json“ mit Informationen aus dem Ordner „config“. Der Buildstatus wird im integrierten Terminal in VS Code angezeigt. 

2. Klicken Sie in Azure IoT Hub im **Device Explorer** mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und klicken Sie dann auf **Create deployment for Edge device** (Bereitstellung für Edge-Gerät erstellen) aus. Wählen Sie die Datei „deployment.json“ im Ordner „config“ aus. Im integrierten Terminal von VS Code sehen Sie, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID erstellt wurde.

3. Wenn Sie ein IoT Edge-Gerät auf Ihrem Entwicklungscomputer simulieren, können Sie sehen, dass alle Modulimagecontainer innerhalb weniger Minuten gestartet werden.

## <a name="view-the-generated-data"></a>Anzeigen der generierten Daten

1. Klicken Sie zum Überwachen der Daten, die bei der IoT Hub-Instanz eingehen, auf **Ansicht** > **Befehlspalette**. Klicken Sie anschließend auf den Befehl **IoT: Start monitoring D2C message** (IoT:Überwachung von Gerät-zu-Cloud-Nachricht starten). 
2. Wenn Sie die Datenüberwachung beenden möchten, verwenden Sie den Befehl **IoT: Stop monitoring D2C message** (IoT: Überwachung von Gerät-zu-Cloud-Nachricht beenden) in der **Befehlspalette**. 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über weitere Szenarien zur Entwicklung mit Azure IoT Edge in Visual Studio Code:

* Entwickeln von Modulen in VS Code mit [C#](how-to-develop-csharp-module.md) oder [Node.js](how-to-develop-node-module.md).
* Entwickeln von Azure Functions in VS Code mit [C#](how-to-develop-csharp-function.md).

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).
---
title: Erste Schritte mit Azure IoT Edge (Windows) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Azure IoT Edge-Gateway auf einem Windows-Computer erstellen, und lernen Sie wichtige Konzepte in Azure IoT Edge kennen, zum Beispiel Module und JSON-Konfigurationsdateien.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 262963250921ceb6f1a2fed0f68c36a9f052b47b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Untersuchen der Azure IoT Edge-Architektur unter Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Ausführen des Beispiels

Das Skript **build.cmd** generiert die Ausgabe im Ordner **build** Ihrer lokalen Kopie des Repositorys **iot-edge**. Diese Ausgabe enthält zahlreiche Dateien, dieses Beispiel konzentriert sich jedoch auf die drei folgenden:
- Zwei IoT Edge-Module: **build\\modules\\logger\\Debug\\logger.dll** und **build\\modules\\hello_world\\Debug\\hello\_world.dll**. 
- Eine ausführbare Datei: **build\\samples\\hello\_world\\Debug\\hello\_world\_sample.exe**. Dieser Prozess akzeptiert eine JSON-Konfigurationsdatei als Befehlszeilenargument.

Die vierte Datei, die Sie in diesem Beispiel verwenden, befindet sich nicht im Ordner „build“, war jedoch beim Klonen im Repository „iot-edge“ enthalten:
- Eine JSON-Konfigurationsdatei: **samples\\hello\_world\\src\\hello\_world\_win.json**. Diese Datei enthält die Pfade zu den zwei Modulen. Sie deklariert außerdem, wohin „logger.dll“ die Ausgabe schreibt. Standardmäßig erfolgt die Ausgabe in **log.txt** in Ihrem aktuellen Arbeitsverzeichnis. 

   >[!NOTE]
   >Wenn Sie die Beispielmodule verschieben oder ein eigenes Modul zu Testzwecken hinzufügen, aktualisieren Sie die Werte für **module.path** in der Konfigurationsdatei entsprechend. Die Modulpfade gelten relativ zum Verzeichnis, in dem sich **hello\_world\_sample.exe** befindet. 

Gehen Sie folgendermaßen vor, um das Beispiel auszuführen:

1. Navigieren Sie zum **build**-Ordner im Stamm Ihrer lokalen Kopie des Repositorys **iot-edge**.

1. Führen Sie den folgenden Befehl aus:

   ```cmd
   samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
   ```

1. Die folgende Ausgabe bedeutet, dass das Beispiel erfolgreich ausgeführt wird:

   ```cmd
   gateway successfully created from JSON
   gateway shall run until ENTER is pressed
   ```
  
1. Drücken Sie die EINGABETASTE****, um den Prozess zu beenden.


[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

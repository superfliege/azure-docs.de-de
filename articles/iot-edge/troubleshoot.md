---
title: Beheben von Problemen mit Azure IoT Edge | Microsoft-Dokumentation
description: "Beheben häufig auftretender Probleme und Aneignen von Fähigkeiten bei der Problembehandlung bei Azure IoT Edge"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3f61f0bf8234e747ae38146d1a5ea030e3163fa3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2017
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Häufig auftretende Probleme und Lösungen für Azure IoT Edge

Wenn in Ihrer Umgebung Probleme bei der Ausführung von Azure IoT Edge auftreten, verwenden Sie diesen Artikel als Leitfaden für die Behandlung und Behebung von Problemen. 

## <a name="standard-diagnostic-steps"></a>Standarddiagnoseschritte 

Wenn ein Problem auftritt, erfahren Sie mehr über den Status Ihres IoT Edge-Geräts anhand der Protokolle für Container und Nachrichten, die an das Gerät und von diesem übermittelt werden. Verwenden Sie die Befehle und Tools in diesem Abschnitt zum Sammeln von Informationen. 

* Überprüfen Sie die Protokolle der Docker-Container, um Probleme zu erkennen. Beginnen Sie mit den bereitgestellten Containern, und sehen Sie sich dann die Container der IoT Edge-Runtime an: „Edge Agent“ und „Edge Hub“. Die Edge Agent-Protokolle bieten in der Regel Informationen zum Lebenszyklus der einzelnen Container. Die Edge Hub-Protokolle bieten Informationen zu Messaging und Routing. 

   ```cmd
   docker logs <container name>
   ```

* Zeigen Sie die Nachrichten an, die Edge Hub durchlaufen, und verschaffen Sie sich mit ausführlichen Protokollen von den Runtimecontainern Einblicke in Aktualisierungen der Geräteeigenschaften.

   ```cmd
   iotedgectl setup --runtime-log-level DEBUG
   ```

* Wenn Verbindungsprobleme auftreten, überprüfen Sie die Umgebungsvariablen Ihres Edge-Geräts, etwa die Verbindungszeichenfolge des Geräts:

   ```cmd
   docker exec edgeAgent printenv
   ```

Sie können auch die Nachrichten zwischen IoT Hub und den IoT Edge-Geräten überprüfen. Zeigen Sie diese Nachrichten mithilfe der [Azure IoT-Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)-Erweiterung für Visual Studio Code an. Weitere Hinweise finden Sie unter [Praktisches Tool bei der Entwicklung mit Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

Nachdem Sie in den Protokollen und Meldungen nach Informationen gesucht haben, können Sie auch versuchen, die Azure IoT Edge-Runtime neu zu starten:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>Edge Agent wird nach ungefähr einer Minute beendet.

Edge Agent startet und wird ungefähr eine Minute lang erfolgreich ausgeführt, dann wird er beendet. Die Protokolle zeigen an, dass Edge Agent versucht, über AMQP eine Verbindung mit IoT Hub herzustellen, und dann ungefähr 30 Sekunden später versucht, eine Verbindung mithilfe von AMQP über Websocket herzustellen. Wenn hierbei ein Fehler auftritt, wird Edge Agent beendet. 

Beispielprotokolle von Edge Agent:

```
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Grundursache
Eine Netzwerkkonfiguration im Hostnetzwerk hindert Edge Agent daran, das Netzwerk zu erreichen. Der Agent versucht zuerst, eine Verbindung über AMQP (Port 5671) herzustellen. Wenn hierbei ein Fehler auftritt, verwendet er Websockets (Port 443).

Die IoT Edge-Runtime richtet ein Netzwerk für die Kommunikation der einzelnen Module ein. Unter Linux ist dieses Netzwerk ein Bridgenetzwerk. Unter Windows wird NAT verwendet. Dieses Problem tritt häufiger auf Windows-Geräten mit Windows-Containern auf, die das NAT-Netzwerk verwenden. 

### <a name="resolution"></a>Lösung
Stellen Sie sicher, dass für die diesem Bridge-/NAT-Netzwerk zugewiesenen IP-Adressen eine Route zum Internet besteht. In einigen Fällen hat eine VPN-Konfiguration auf dem Host Vorrang vor dem IoT Edge-Netzwerk. 

## <a name="edge-hub-fails-to-start"></a>Edge Hub kann nicht gestartet werden.

Edge Hub kann nicht gestartet werden und gibt folgende Fehlermeldung in den Protokollen aus: 

```
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Grundursache
Ein anderer Prozess auf dem Hostcomputer hat Port 443 gebunden. Edge Hub ordnet Ports 5671 und 443 für Szenarien im Gateway zu. Bei dieser Portzuordnung tritt ein Fehler auf, wenn ein anderer Prozess diesen Port bereits gebunden hat. 

### <a name="resolution"></a>Lösung
Suchen Sie den Prozess, der Port 443 verwendet, und beenden Sie ihn. Dieser Prozess ist in der Regel ein Webserver.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Edge Agent kann nicht auf das Image eines Moduls (403) zugreifen.
Ein Container kann nicht ausgeführt werden, und die Edge Agent-Protokolle weisen einen Fehler 403 auf. 

### <a name="root-cause"></a>Grundursache
Edge Agent hat keine Zugriffsberechtigungen für das Image eines Moduls. 

### <a name="resolution"></a>Lösung
Führen Sie den Befehl `iotedgectl login` erneut aus.

## <a name="next-steps"></a>Nächste Schritte
Sind Sie der Meinung, dass Sie in der IoT Edge-Plattform einen Fehler gefunden haben? [Übermitteln Sie Ihr Problem](https://github.com/Azure/iot-edge/issues), damit wir die Plattform weiter verbessern können. 

---
title: Beheben von Problemen mit Azure IoT Edge | Microsoft-Dokumentation
description: Beheben häufig auftretender Probleme und Aneignen von Fähigkeiten bei der Problembehandlung bei Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/23/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ad22b0cd1457c1d4146a75047ff18e916c0c7ccd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633535"
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
   iotedgectl setup --connection-string "{device connection string}" --runtime-log-level debug
   ```
   
* Anzeigen von ausführlichen Protokollen aus iotedgectl-Befehlen:

   ```cmd
   iotedgectl --verbose DEBUG <command>
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

```output
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

```output
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

## <a name="iotedgectl-cant-find-docker"></a>„iotedgectl“ findet Docker nicht.

Bei dem Befehl `iotedgectl setup` oder `iotedgectl start` tritt ein Fehler auf, und die folgende Meldung wird in die Protokolle eingetragen:
```output
File "/usr/local/lib/python2.7/dist-packages/edgectl/host/dockerclient.py", line 98, in get_os_type
  info = self._client.info()
File "/usr/local/lib/python2.7/dist-packages/docker/client.py", line 174, in info
  return self.api.info(*args, **kwargs)
File "/usr/local/lib/python2.7/dist-packages/docker/api/daemon.py", line 88, in info
  return self._result(self._get(self._url("/info")), True)
```

### <a name="root-cause"></a>Grundursache
„iotedgectl“ kann Docker, eine erforderliche Komponente, nicht finden.

### <a name="resolution"></a>Lösung
Installieren Sie Docker, stellen Sie sicher, dass das Tool ausgeführt wird, und versuchen Sie es noch einmal.

## <a name="iotedgectl-setup-fails-with-an-invalid-hostname"></a>Im Setup von „iotedgectl“ tritt ein Fehler mit einem ungültigen Hostnamen auf.

Bei dem Befehl `iotedgectl setup` tritt ein Fehler auf, und die folgende Meldung wird ausgegeben: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### <a name="root-cause"></a>Grundursache
Die IoT-Edge-Runtime unterstützt nur Hostnamen, die kürzer als 64 Zeichen sind. Dies ist in der Regel für physische Computer kein Problem, kann aber auftreten, wenn Sie die Runtime auf einem virtuellen Computer einrichten. Die automatisch generierten Hostnamen für in Azure gehostete virtuelle Windows-Computer sind tendenziell lang sein. 

### <a name="resolution"></a>Lösung
Wenn dieser Fehler angezeigt wird, können Sie ihn durch Konfigurieren des DNS-Namens des virtuellen Computers und anschließendes Festlegen des DNS-Namens als Hostname im Setupbefehl beheben.

1. Navigieren Sie im Azure-Portal zur Übersichtsseite Ihrer VM. 
2. Wählen Sie unter „DNS-Name“ **Konfigurieren**. Wenn für Ihren virtuellen Computer bereits ein DNS-Name konfiguriert wurde, müssen Sie keinen neuen konfigurieren. 

   ![Konfigurieren des DNS-Namens](./media/troubleshoot/configure-dns.png)

3. Geben Sie einen Wert für **DNS-Namensbezeichnung** ein, und wählen Sie **Speichern**.
4. Kopieren Sie den neuen DNS-Namen, der das Format **\<DNS-Namensbezeichnung\>.\< VM-Speicherort\>. cloudapp.azure.com** aufweisen sollte.
5. Verwenden Sie innerhalb des virtuellen Computers den folgenden Befehl zum Einrichten der IoT Edge-Runtime mit Ihrem DNS-Namen:

   ```input
   iotedgectl setup --connection-string "<connection string>" --nopass --edge-hostname "<DNS name>"
   ```

## <a name="next-steps"></a>Nächste Schritte
Sind Sie der Meinung, dass Sie in der IoT Edge-Plattform einen Fehler gefunden haben? [Übermitteln Sie Ihr Problem](https://github.com/Azure/iot-edge/issues), damit wir die Plattform weiter verbessern können. 

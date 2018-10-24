---
title: Beheben von Problemen mit Azure IoT Edge | Microsoft-Dokumentation
description: Beheben häufig auftretender Probleme und Aneignen von Fähigkeiten bei der Problembehandlung bei Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a63a31c5ceb4298829f85627196fea5d7a38ca4b
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068501"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Häufig auftretende Probleme und Lösungen für Azure IoT Edge

Wenn in Ihrer Umgebung Probleme bei der Ausführung von Azure IoT Edge auftreten, verwenden Sie diesen Artikel als Leitfaden für die Behandlung und Behebung von Problemen. 

## <a name="standard-diagnostic-steps"></a>Standarddiagnoseschritte 

Wenn ein Problem auftritt, erfahren Sie mehr über den Status Ihres IoT Edge-Geräts anhand der Protokolle für Container und Nachrichten, die an das Gerät und von diesem übermittelt werden. Verwenden Sie die Befehle und Tools in diesem Abschnitt zum Sammeln von Informationen. 

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>Überprüfen Sie den Status von IoT Edge Security Manager und den zugehörigen Protokollen:

Unter Linux:
- Zeigen Sie den Status von IoT Edge Security Manager wie folgt an:

   ```bash
   sudo systemctl status iotedge
   ```

- Zeigen Sie die Protokolle von IoT Edge Security Manager wie folgt an:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- Zeigen Sie ausführlichere Protokolle von IoT Edge Security Manager wie folgt an:

   - Bearbeiten Sie die iotedge-Daemon-Einstellungen:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Aktualisieren Sie die folgenden Zeilen:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - Starten Sie den Daemon für die IoT Edge-Sicherheit neu:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

Unter Windows:
- Zeigen Sie den Status von IoT Edge Security Manager wie folgt an:

   ```powershell
   Get-Service iotedge
   ```

- Zeigen Sie die Protokolle von IoT Edge Security Manager wie folgt an:

   ```powershell
   # Displays logs from today, newest at the bottom.
 
   Get-WinEvent -ea SilentlyContinue `
   -FilterHashtable @{ProviderName= "iotedged";
     LogName = "application"; StartTime = [datetime]::Today} |
   select TimeCreated, Message |
   sort-object @{Expression="TimeCreated";Descending=$false} |
   format-table -autosize -wrap
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>Überprüfen der YAML-Konfigurationsdatei, wenn der IoT Edge Security Manager nicht ausgeführt wird

> [!WARNING]
> YAML-Dateien können keine Tabstopps für Einzüge enthalten. Verwenden Sie stattdessen zwei Leerzeichen.

Unter Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Unter Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Überprüfen von Containerprotokollen auf Probleme

Wenn der Daemon für die IoT Edge-Sicherheit ausgeführt wird, können Sie sich die Protokolle der Container ansehen, um Probleme zu erkennen. Beginnen Sie mit den bereitgestellten Containern, und sehen Sie sich dann die Container der IoT Edge-Runtime an: „Edge Agent“ und „Edge Hub“. Die Edge Agent-Protokolle bieten in der Regel Informationen zum Lebenszyklus der einzelnen Container. Die Edge Hub-Protokolle bieten Informationen zu Messaging und Routing. 

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-edge-hub"></a>Anzeigen der Nachrichten, die den Edge Hub durchlaufen

Zeigen Sie die Nachrichten an, die den Edge Hub durchlaufen, und verschaffen Sie sich mit ausführlichen Protokollen der Runtimecontainer „edgeAgent“ und „edgeHub“ Einblicke in Aktualisierungen der Geräteeigenschaften. Legen Sie die `RuntimeLogLevel` in Ihrer YAML-Konfigurationsdartei fest, um für diese Container ausführliche Protokolle zu aktivieren. So öffnen Sie die Datei

Unter Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Unter Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

Das `agent`-Element sieht standardmäßig in etwa wie folgt aus:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Ersetzen Sie `env: {}` durch:

> [!WARNING]
> YAML-Dateien können keine Tabstopps für Einzüge enthalten. Verwenden Sie stattdessen zwei Leerzeichen.

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

Speichern Sie die Datei, und starten Sie den IoT Edge-Sicherheits-Manager neu.

Sie können auch die Nachrichten zwischen IoT Hub und den IoT Edge-Geräten überprüfen. Zeigen Sie diese Nachrichten mithilfe der [Azure IoT-Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)-Erweiterung für Visual Studio Code an. Weitere Hinweise finden Sie unter [Praktisches Tool bei der Entwicklung mit Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

### <a name="restart-containers"></a>Neustarten von Containern
Nachdem Sie die Protokolle und Nachrichten auf Informationen untersucht haben, können Sie versuchen, die Container neu zu starten:

```
iotedge restart <container name>
```

Starten Sie die IoT Edge-Runtime-Container neu:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>Neustarten von IoT Edge Security Manager

Falls das Problem weiterhin besteht, können Sie versuchen, den IoT Edge Security Manager neu zu starten.

Unter Linux:

   ```cmd
   sudo systemctl restart iotedge
   ```

Unter Windows:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
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
Stellen Sie sicher, dass Ihre Anmeldeinformationen für die Registrierung im Bereitstellungsmanifest richtig angegeben sind.

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>Fehler für den Daemon für die IoT Edge-Sicherheit aufgrund eines ungültigen Hostnamens

Bei dem Befehl `sudo journalctl -u iotedge` tritt ein Fehler auf, und die folgende Meldung wird ausgegeben: 

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

   - Unter Linux:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - Unter Windows:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="stability-issues-on-resource-constrained-devices"></a>Stabilitätsprobleme für Geräte mit Ressourceneinschränkungen 
Auf eingeschränkten Geräten wie dem Raspberry Pi kann es zu Stabilitätsproblemen kommen, vor allem bei der Verwendung als Gateway. Zu den Symptomen gehören Speicherausnahmen im Edge-Hub-Modul; nachgeschaltete Geräte können sich nicht verbinden, oder das Gerät sendet nach einigen Stunden keine Telemetrienachrichten mehr.

### <a name="root-cause"></a>Grundursache
Der zur Edgeruntime gehörige Edgehub ist standardmäßig für die Leistungserbringung optimiert und versucht, große Blöcke des Speichers zuzuordnen. Dies ist für eingeschränkte Edgegeräte nicht ideal und kann zu Stabilitätsproblemen führen.

### <a name="resolution"></a>Lösung
Setzen Sie für den Edgehub eine Umgebungsvariable **OptimizeForPerformance** auf **FALSE**. Dies kann auf zwei Arten erreicht werden:

In der Benutzeroberfläche: 

Erstellen Sie im Portal über *Gerätedetails*->*Module festlegen*->*Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren* eine Umgebungsvariable namens *OptimizeForPerformance*, die für den *Edgehub* auf *FALSE* gesetzt ist.

![optimizeforperformance][img-optimize-for-perf]

**OR**

Im Bereitstellungsmanifest:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```
## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>IoT Edge-Daemonprotokolle in Windows können nicht abgerufen werden.
Wenn Sie bei Verwendung von `Get-WinEvent` unter Windows eine EventLogException-Meldung erhalten, überprüfen Sie Ihre Registrierungseinträge.

### <a name="root-cause"></a>Grundursache
Damit der `Get-WinEvent`-PowerShell-Befehl Protokolle anhand eines bestimmten `ProviderName` suchen kann, muss ein Registrierungseintrag vorhanden sein.

### <a name="resolution"></a>Lösung
Legen Sie einen Registrierungseintrag für den IoT Edge-Daemon fest. Erstellen Sie eine Datei vom Typ **iotedge.reg** mit dem folgenden Inhalt, und importieren Sie sie in die Windows-Registrierung, indem Sie auf die Datei doppelklicken oder den Befehl `reg import iotedge.reg` ausführen:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>Beim Versuch des IoT Edge-Moduls, eine Nachricht an edgeHub zu senden, tritt ein 404-Fehler auf.

Beim Versuch eines benutzerdefinierten IoT Edge-Moduls, eine Nachricht an edgeHub zu senden, tritt ein 404-Fehler (`Module not found`) auf. Der IoT Edge-Daemon gibt folgende Meldung an die Protokolle aus: 

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 ) 
```

### <a name="root-cause"></a>Grundursache
Der IoT Edge-Daemon erzwingt aus Sicherheitsgründen die Prozessidentifizierung für alle Module, die eine Verbindung mit edgeHub herstellen. Dadurch wird sichergestellt, dass alle von einem Modul gesendeten Nachrichten von der Hauptprozess-ID des Moduls stammen. Sendet ein Modul eine Nachricht, die nicht von der anfangs festgelegten Prozess-ID stammt, wird die Nachricht mit dem Fehlercode 404 abgelehnt.

### <a name="resolution"></a>Lösung
Stellen Sie sicher, dass das benutzerdefinierte IoT Edge-Modul beim Senden von Nachrichten an edgeHub immer die gleiche Prozess-ID verwendet. Verwenden Sie in Ihrer Docker-Datei beispielsweise `ENTRYPOINT` anstelle des Befehls `CMD`, da `CMD` zu zwei Prozess-IDs (eine für das Modul und eine andere für den Bash-Befehl zum Ausführen des Hauptprogramms) führt, während bei `ENTRYPOINT` nur eine einzelne Prozess-ID verwendet wird.


## <a name="firewall-and-port-configuration-rules-for-iot-edge-deployment"></a>Firewall- und Portkonfigurationsregeln für die IoT Edge-Bereitstellung
Azure IoT Edge ermöglicht die Kommunikation eines lokalen Edgeservers mit der Azure-Cloud über unterstützte IoT Hub-Protokolle (siehe [Referenz – Auswählen eines Kommunikationsprotokolls](../iot-hub/iot-hub-devguide-protocols.md)). Aus Sicherheitsgründen sind Kommunikationskanäle zwischen Azure IoT Edge und Azure IoT Hub immer als ausgehend konfiguriert. Die Grundlage hierfür bildet das [dienstunterstütze Kommunikationsmuster](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/), das dazu dient, die Angriffsfläche für potenzielle Angreifer zu minimieren. Eingehende Kommunikation ist nur in bestimmten Szenarien erforderlich, in denen Azure IoT Hub Nachrichten an den Azure IoT Edge-Server pushen muss (etwa beim C2D-Messaging). Diese werden ebenfalls durch die Verwendung sicherer TLS-Kanäle geschützt, und der Schutz kann mithilfe von X.509-Zertifikaten und TPM-Gerätemodulen noch weiter erhöht werden. Der Azure IoT Edge-Sicherheits-Manager steuert, wie diese Kommunikation zustande kommt (siehe [Azure IoT Edge-Sicherheits-Manager](../iot-edge/iot-edge-security-manager.md)).

IoT Edge bietet zwar erweiterte Konfigurationsmöglichkeiten zum Schutz der Azure IoT Edge-Runtime und der bereitgestellten Module, hängt aber dennoch von der zugrunde liegenden Computer- und Netzwerkkonfiguration ab. Für eine sichere Kommunikation zwischen Edge und Cloud müssen daher unbedingt geeignete Netzwerk- und Firewallregeln vorhanden sein. Beim Konfigurieren von Firewallregeln für die zugrunde liegenden Server, auf denen die Azure IoT Edge-Runtime gehostet wird, können Sie sich an Folgendem orientieren:

|Protokoll|Port|Eingehend|Ausgehend|Anleitungen|
|--|--|--|--|--|
|MQTT|8883|BLOCKIERT (Standard)|BLOCKIERT (Standard)|<ul> <li>Konfigurieren Sie eingehende Verbindungen als „Offen“, wenn Sie MQTT als Kommunikationsprotokoll verwenden.<li>1883 für MQTT wird von IoT Edge nicht unterstützt. <li>Eingehende Verbindungen sollten blockiert werden.</ul>|
|AMQP|5671|BLOCKIERT (Standard)|OFFEN (Standard)|<ul> <li>Standardkommunikationsprotokoll für IoT Edge. <li> Muss als „Offen“ konfiguriert werden, wenn Azure IoT Edge nicht für andere unterstützte Protokolle konfiguriert oder AMQP das gewünschte Kommunikationsprotokoll ist.<li>5672 für AMQP wird von IoT Edge nicht unterstützt.<li>Blockieren Sie diesen Port, wenn Azure IoT Edge ein anderes von IoT Hub unterstütztes Protokoll verwendet.<li>Eingehende Verbindungen sollten blockiert werden.</ul></ul>|
|HTTPS|443|BLOCKIERT (Standard)|OFFEN (Standard)|<ul> <li>Konfigurieren Sie ausgehende Verbindungen am Port 443 für die IoT Edge-Bereitstellung als „Offen“. Dies ist erforderlich, wenn Sie manuelle Skripts oder den Azure IoT Device Provisioning Service (DPS) verwenden. <li>Die eingehende Verbindung sollte nur für bestimmte Szenarien geöffnet werden: <ul> <li>  Wenn Sie über ein transparentes Gateway mit Blattknotengeräten verfügen, die ggf. Methodenanforderungen senden. In diesem Fall muss der Port 443 nicht für externe Netzwerke geöffnet werden, um eine Verbindung mit IoT Hub herzustellen oder IoT Hub-Dienste über Azure IoT Edge bereitzustellen. Die eingehende Regel kann somit darauf beschränkt werden, nur eingehende Verbindungen aus dem internen Netzwerk zu öffnen. <li> In C2D-Szenarien.</ul><li>80 für HTTP wird von IoT Edge nicht unterstützt.<li>Falls im Unternehmen keine HTTP-fremden Protokolle (etwa AMQP oder MQTT) konfiguriert werden können, können die Nachrichten über WebSockets gesendet werden. In diesem Fall wird der Port 443 für die WebSocket-Kommunikation verwendet.</ul>|


## <a name="next-steps"></a>Nächste Schritte
Sind Sie der Meinung, dass Sie in der IoT Edge-Plattform einen Fehler gefunden haben? [Übermitteln Sie Ihr Problem](https://github.com/Azure/iotedge/issues), damit wir die Plattform weiter verbessern können. 

<!-- Images -->
[img-optimize-for-perf]: ./media/troubleshoot/OptimizeForPerformanceFalse.png

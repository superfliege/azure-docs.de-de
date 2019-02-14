---
title: Konfigurieren von Geräten für Netzwerkproxys – Azure IoT Edge | Microsoft-Dokumentation
description: Erläutert das Konfigurieren der Azure IoT Edge-Runtime und der IoT Edge-Module mit Internetverbindung für die Kommunikation über einen Proxyserver.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 12/17/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 58a51fd90eb0b89048eca7c95272523ffd10c24a
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2019
ms.locfileid: "55982317"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver

IoT Edge-Geräte senden HTTPS-Anforderungen, um mit IoT Hub zu kommunizieren. Wenn Ihr Gerät mit einem Netzwerk verbunden ist, das einen Proxyserver verwendet, müssen Sie die IoT Edge-Runtime für die Kommunikation über den Server konfigurieren. Proxyserver können auch einzelne IoT Edge-Module beeinflussen, wenn sie HTTP- oder HTTPS-Anforderungen senden, die nicht über den IoT Edge-Hub weitergeleitet werden. 

Das Konfigurieren eines IoT Edge-Geräts zur Nutzung eines Proxyservers erfolgt mit den folgenden grundlegenden Schritten: 

1. Installieren Sie die IoT Edge-Runtime auf Ihrem Gerät. 
2. Konfigurieren Sie den Docker-Daemon und den IoT Edge-Daemon auf Ihrem Gerät, sodass sie den Proxyserver verwenden.
3. Konfigurieren Sie die edgeAgent-Eigenschaften in der Datei „config.yaml“ auf Ihrem Gerät.
4. Legen Sie Umgebungsvariablen für die IoT Edge-Runtime und andere IoT Edge-Module im Bereitstellungsmanifest fest. 

## <a name="know-your-proxy-url"></a>Ermitteln der Webproxy-URL

Um den Docker-Daemon und IoT Edge auf Ihrem Gerät zu konfigurieren, müssen Sie Ihre Webproxy-URL kennen. 

Proxy-URLs haben das folgende Format: **Protokoll**://**Proxyhost**:**Proxyport**. 

* Das **Protokoll** ist entweder HTTP oder HTTPS. Der Docker-Daemon kann abhängig von den Einstellungen Ihrer Containerregistrierung beide Protokolle verwenden, aber für den IoT Edge-Daemon und die Runtimecontainer sollte immer HTTPS verwendet werden.

* Der **Proxyhost** ist eine Adresse für den Proxyserver. Wenn Ihr Proxyserver eine Authentifizierung erfordert, können Sie Ihre Anmeldeinformationen als Teil von „Proxyhost“ im Format **Benutzer**:**Kennwort**@**Proxyhost** eingeben. 

* Der **Proxyport** ist der Netzwerkport, an dem der Proxy auf Netzwerkdatenverkehr antwortet. 

## <a name="install-the-runtime"></a>Installieren der Runtime

Wenn Sie IoT Edge-Runtime auf einem Linux-Gerät installieren, konfigurieren Sie den Paket-Manager so, dass er über den Proxyserver auf das Installationspaket zugreift. Beispiel: [Richten Sie „apt-get“ zur Verwendung eines http-Proxys ein](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy). Sobald Ihr Paket-Manager konfiguriert ist, befolgen Sie einfach die Anweisungen in [Installieren von Azure IoT Edge-Runtime unter Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) oder [Installieren von Azure IoT Edge-Runtime unter Linux (x64)](how-to-install-iot-edge-linux.md). 

Wenn Sie die IoT Edge-Runtime auf einem Windows-Gerät installieren, müssen Sie den Proxyserver einmal durchlaufen, um die Installationsskriptdatei herunterzuladen, und dann wieder während der Installation, um die erforderlichen Komponenten herunterzuladen. Sie können Proxyinformationen in den Windows-Einstellungen konfigurieren oder Ihre Proxyinformationen direkt in das Installationsskript aufnehmen. Das folgende PowerShell-Skript ist ein Beispiel für eine Windows-Installation mithilfe des `-proxy`-Arguments:

```powershell
. {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -proxy <proxy URL>
```

Wenn Sie komplexe Anmeldeinformationen für den Proxyserver verwenden, die nicht in die URL eingeschlossen werden können, verwenden Sie den Parameter `-ProxyCredential` innerhalb von `-InvokeWebRequestParameters`. Beispiel:

```powershell
$proxyCredential = (Get-Credential).GetNetworkCredential()
. {Invoke-WebRequest -proxy <proxy URL> -ProxyCredential $proxyCredential -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -InvokeWebRequestParameters @{ '-Proxy' = '<proxy URL>'; '-ProxyCredential' = $proxyCredential }
```

Weitere Informationen zu Proxyparametern finden Sie unter [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest). Weitere Informationen zu Installationsoptionen finden Sie unter [Installieren der Azure IoT Edge-Runtime auf Windows](how-to-install-iot-edge-windows.md).

Verwenden Sie nach der IoT Edge-Runtime-Installation den folgenden Abschnitt, um sie mit Ihren Proxyinformationen zu konfigurieren. 

## <a name="configure-the-daemons"></a>Konfigurieren der Daemons

Die Docker- und IoT Edge-Daemons, die auf Ihrem IoT Edge-Gerät ausgeführt werden, müssen so konfiguriert werden, dass sie den Proxyserver verwenden. Der Docker-Daemon ruft mit Webanforderungen Containerimages aus Containerregistrierungen ab. Der IoT Edge-Daemon kommuniziert über Webanforderungen mit IoT Hub.

### <a name="docker-daemon"></a>Docker-Daemon

In der Docker-Dokumentation wird erläutert, wie der Docker-Daemon mit Umgebungsvariablen konfiguriert wird. Die meisten Containerregistrierungen (einschließlich DockerHub und Azure Container Registry-Instanzen) unterstützen HTTPS-Anforderungen, daher sollten Sie den Parameter **HTTPS_PROXY** festlegen. Wenn Sie Images aus einer Registrierung abrufen, die Transport Layer Security (TLS) nicht unterstützt, sollten Sie den Parameter **HTTP_PROXY** festlegen. 

Wählen Sie den Artikel aus, der für Ihre Docker-Version gilt: 

* [Docker](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
* [Docker für Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon#proxy-configuration)

### <a name="iot-edge-daemon"></a>IoT Edge-Daemon

Der IoT Edge-Daemon wird in ähnlicher Weise wie der Docker-Daemon konfiguriert. Alle Anforderungen, die IoT Edge an IoT Hub sendet, verwenden HTTPS. Verwenden Sie je nach Betriebssystem die folgenden Schritte, um eine Umgebungsvariable für den Dienst festzulegen. 

#### <a name="linux"></a>Linux

Öffnen Sie einen Editor im Terminal, um den IoT Edge-Daemon zu konfigurieren. 

```bash
sudo systemctl edit iotedge
```

Geben Sie den folgenden Text ein, und ersetzen Sie dabei **\<proxy URL>** mit der Adresse und dem Port Ihres Proxyservers. Wählen Sie dann „Speichern“ und „Beenden“ aus. 

```ini
[Service]
Environment="https_proxy=<proxy URL>"
```

Aktualisieren Sie den Dienst-Manager, um die neue Konfiguration für IoT Edge zu übernehmen.

```bash
sudo systemctl daemon-reload
```

Starten Sie IoT Edge neu, damit die Änderungen wirksam werden.

```bash
sudo systemctl restart iotedge
```

Stellen Sie sicher, dass die Umgebungsvariable erstellt wurde und die neue Konfiguration geladen wurde. 

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

Öffnen Sie ein PowerShell-Fenster als Administrator, und führen Sie den folgenden Befehl zum Bearbeiten der Registrierung mit der neuen Umgebungsvariable aus. Ersetzen Sie **\<proxy URL>** mit der Adresse und dem Port Ihres Proxyservers. 

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

Starten Sie IoT Edge neu, damit die Änderungen wirksam werden.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-iot-edge-agent"></a>Konfigurieren des IoT Edge-Agents

Der IoT Edge-Agent ist das erste Modul, das auf einem IoT Edge-Gerät gestartet wird. Es wird zum ersten Mal mit den Informationen in der IoT Edge-Datei „config.yaml“ gestartet. Der IoT Edge-Agent stellt anschließend eine Verbindung mit IoT Hub her, um Bereitstellungsmanifeste abzurufen, die deklarieren, welche weiteren Module auf dem Gerät bereitgestellt werden sollen.

Öffnen Sie die Datei „config.yaml“ auf Ihrem IoT Edge-Gerät. In Linux-Systemen befindet sich diese Datei unter **/etc/iotedge/config.yaml**. In Windows-Systemen befindet sich diese Datei unter **C:\ProgramData\iotedge\config.yaml**. Die Konfigurationsdatei ist geschützt. Sie benötigen also Administratorrechte, um darauf zuzugreifen. Bei Linux-Systemen bedeutet dies, dass Sie den `sudo`-Befehl verwenden müssen, bevor Sie die Datei in Ihrem bevorzugten Text-Editor öffnen. Bei Windows bedeutet dies, einen Text-Editor wie Editor als Administrator zu öffnen und dann die Datei zu öffnen. 

Suchen Sie in der Datei „config.yaml“ nach dem Abschnitt **Edge Agent module spec**. Die IoT Edge-Agent-Definition enthält einen **env**-Parameter, dem Sie Umgebungsvariablen hinzufügen können. 

<!--
![edgeAgent definition](./media/how-to-configure-proxy-support/edgeagent-unedited.png)
-->

Entfernen Sie die geschweiften Klammern, die Platzhalter für den env-Parameter darstellen, und fügen Sie die neue Variable in einer neuen Zeile hinzu. Denken Sie daran, dass Einzüge in YAML zwei Leerzeichen sind. 

```yaml
https_proxy: "<proxy URL>"
```

Die IoT Edge-Runtime verwendet standardmäßig AMQP für die Kommunikation mit IoT Hub. Einige Proxyserver blockieren AMQP-Ports. Wenn dies der Fall ist, müssen Sie „edgeAgent“ auch so konfigurieren, dass AMQP über WebSocket verwendet wird. Fügen Sie eine zweite Umgebungsvariable hinzu.

```yaml
UpstreamProtocol: "AmqpWs"
```

![edgeAgent-Definition mit Umgebungsvariablen](./media/how-to-configure-proxy-support/edgeagent-edited.png)

Speichern Sie die Änderungen an „config.yaml“, und schließen Sie den Editor. Starten Sie IoT Edge neu, damit die Änderungen wirksam werden. 

* Linux: 

   ```bash
   sudo systemctl restart iotedge
   ```

* Windows:

   ```powershell
   Restart-Service iotedge
   ```

## <a name="configure-deployment-manifests"></a>Konfigurieren von Bereitstellungsmanifesten  

Nachdem Ihr IoT Edge-Gerät für die Verwendung Ihres Proxyservers konfiguriert wurde, müssen Sie die Umgebungsvariablen auch in allen zukünftigen Bereitstellungsmanifesten deklarieren. Konfigurieren Sie die beiden Runtimemodule („edgeAgent“ und „edgeHub“) immer so, dass sie über den Proxyserver kommunizieren, damit sie eine Verbindung mit IoT Hub aufrechterhalten können. Weitere IoT Edge-Module, die eine Verbindung mit dem Internet herstellen, müssen für den Proxyserver konfiguriert werden. Module, die ihre Nachrichten über „edgeHub“ weiterleiten oder nur mit anderen Modulen auf dem Gerät kommunizieren, benötigen die Details zum Proxyserver nicht. 

Sie können die Bereitstellungsmanifeste mit dem Azure-Portal oder manuell durch die Bearbeitung einer JSON-Datei erstellen. 

### <a name="azure-portal"></a>Azure-Portal

Wenn Sie den Assistenten **Module festlegen** zum Erstellen von Bereitstellungen für IoT Edge-Geräte verwenden, verfügt jedes Modul über einen Abschnitt **Umgebungsvariablen**, mit dem Sie Proxyserververbindungen konfigurieren können. 

Wählen Sie zum Konfigurieren der IoT Edge-Agent- und der IoT Edge-Hubmodule **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** im ersten Schritt des Assistenten aus. 

![Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren](./media/how-to-configure-proxy-support/configure-runtime.png)

Fügen Sie die Umgebungsvariable **https_proxy** den Definitionen der IoT Edge-Agent- und IoT Edge-Hub-Module hinzu. Wenn Sie die Umgebungsvariable **UpstreamProtocol** in die Datei „config.yaml“ auf Ihrem IoT Edge-Gerät eingefügt haben, fügen Sie sie auch der IoT Edge-Agent-Moduldefinition hinzu. 

![Festlegen der https_proxy-Umgebungsvariable](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

Alle anderen Module, die Sie einem Bereitstellungsmanifest hinzufügen, folgen dem gleichen Muster. Auf der Seite, auf der Sie den Modulnamen und das Image festlegen, gibt es einen Abschnitt für Umgebungsvariablen.

### <a name="json-deployment-manifest-files"></a>JSON-Bereitstellungsmanifestdateien

Wenn Sie Bereitstellungen für IoT Edge-Geräte mithilfe von Vorlagen in Visual Studio Code oder durch das manuelle Erstellen von JSON-Dateien erstellen, können Sie die Umgebungsvariablen direkt jeder Moduldefinition hinzufügen. 

Verwenden Sie das folgende JSON-Format: 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

Mit den Umgebungsvariablen sollte Ihre Moduldefinition wie das folgende edgeHub-Beispiel aussehen:

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": ""
    },
    "env": {
        "https_proxy": {
            "value": "https://proxy.example.com:3128"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Wenn Sie die Umgebungsvariable **UpstreamProtocol** in die Datei „confige.yaml“ auf Ihrem IoT Edge-Gerät eingefügt haben, fügen Sie sie auch der IoT Edge-Agent-Moduldefinition hinzu. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Rollen der [IoT Edge-Runtime](iot-edge-runtime.md).

Behandeln Sie Installations- und Konfigurationsfehler mit [Häufig auftretende Probleme und Lösungen für Azure IoT Edge](troubleshoot.md).


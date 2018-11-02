---
title: Aktualisieren von Geräten auf die neueste Version von Azure IoT Edge | Microsoft-Dokumentation
description: Aktualisieren von IoT Edge-Geräten auf die neuesten Versionen des Sicherheitsdaemons und der IoT Edge-Runtime
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b04f909d58e1555cad9f34b682f9062bbd96cd0e
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394730"
---
# <a name="update-the-iot-edge-runtime"></a>Aktualisieren der IoT Edge-Runtime

Wenn neue Versionen des IoT Edge-Diensts veröffentlicht werden, sollten Sie Ihre IoT Edge-Geräte aktualisieren, damit sie über die neuesten Funktionen und Verbesserungen der Sicherheit verfügen. Dieser Artikel enthält Informationen zum Aktualisieren Ihrer IoT Edge-Geräte, wenn eine neue Version verfügbar ist. 

Zwei Komponenten eines IoT Edge-Geräts müssen bei der Umstellung auf eine neue Version aktualisiert werden. Die erste ist der Sicherheitsdaemon, der auf dem Gerät ausgeführt wird und beim Starten des Geräts die Runtime startet. Der Sicherheitsdaemon kann zurzeit nur vom Gerät selbst aktualisiert werden. Die zweite Komponente ist die Runtime, die aus den Modulen Edge-Hub und Edge-Agent besteht. Je nachdem, wie Ihre Bereitstellung strukturiert ist, kann die Runtime vom Gerät oder remote aktualisiert werden. 

Informationen zur neuesten Version von Azure IoT Edge finden Sie unter [Azure IoT Edge releases](https://github.com/Azure/azure-iotedge/releases) (Azure IoT Edge-Versionen).

## <a name="update-the-security-daemon"></a>Aktualisieren des Sicherheitsdaemons

Der IoT Edge-Sicherheitsdaemon ist eine native Komponente, die mithilfe des Paket-Managers auf dem IoT Edge-Gerät aktualisiert werden muss. 

Überprüfen Sie die Version des auf Ihrem Geräts ausgeführten Sicherheitsdaemons mit dem Befehl `iotedge version`. 

### <a name="linux-devices"></a>Linux-Geräte

Verwenden Sie auf Linux-Geräten „apt-get“ oder Ihren geeigneten Paket-Manager, um den Sicherheitsdaemon zu aktualisieren. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

### <a name="windows-devices"></a>Windows-Geräte

Verwenden Sie auf Windows-Geräten das PowerShell-Skript, um den Sicherheitsdaemon zu deinstallieren und dann neu zu installieren. Das Installationsskript lädt per Pull automatisch die neueste Version des Sicherheitsdaemons herunter. Sie müssen die Verbindungszeichenfolge für Ihr Gerät während des Installationsvorgangs erneut angeben. 

Deinstallieren Sie den Sicherheitsdaemon in einer PowerShell-Administratorsitzung. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
UnInstall-SecurityDaemon
```

Installieren Sie den Sicherheitsdaemon erneut, abhängig davon, ob Ihr IoT Edge-Gerät Windows-Container oder Linux-Container verwendet. Ersetzen Sie den Ausdruck **\<Windows oder Linux\>** durch eins der Containerbetriebssysteme. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOS <Windows or Linux>
```

## <a name="update-the-runtime-containers"></a>Aktualisieren der Runtimecontainer

Die Weise, in der Sie die Edge-Agent- und Edge-Hub-Container aktualisieren, hängt davon ab, ob Sie in Ihrer Bereitstellung fortlaufende Versionstags (wie 1.0) oder spezifische Versionstags (wie 1.0.2) verwenden. 

Überprüfen Sie die Version der aktuell auf Ihrem Gerät ausgeführten IoT Edge-Agent und Edge-Hub-Module mithilfe der Befehle `iotedge logs edgeAgent` und `iotedge logs edgeHub`. 

  ![Anzeigen der Containerversion](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>Grundlagen von IoT Edge-Tags

Die Edge-Agent- und Edge-Hub-Images sind im Tag mit der IoT Edge-Version gekennzeichnet, der sie zugeordnet sind. Es gibt zwei Möglichkeiten zum Verwenden von Tags mit den Runtime-Images: 

* **Fortlaufende Tags**: Verwenden Sie nur die ersten zwei Stellen der Versionsnummer, um zum neuesten Image zu gelangen, das mit diesen Stellen übereinstimmt. Beispielsweise wird, sobald eine neue Version veröffentlicht wird, 1.0 immer so aktualisiert, dass auf die neueste 1.0.x-Version verwiesen wird. Wenn die Containerruntime auf Ihrem IoT Edge-Gerät das Image erneut per Pull herunterlädt, werden die Laufzeitmodule auf die neueste Version aktualisiert. Dieses Vorgehen wird für Entwicklungszwecke vorgeschlagen. Bereitstellungen aus dem Azure-Portal weisen standardmäßig fortlaufende Tags auf. 
* **Spezifische Tags**: Verwenden Sie alle drei Stellen der Versionsnummer, um die Imageversion explizit festzulegen. Beispielsweise ändert sich 1.0.2 nach der ursprünglichen Veröffentlichung nicht. Sie können im Bereitstellungsmanifest eine neue Versionsnummer deklarieren, wenn Sie zur Aktualisierung bereit sind. Dieses Vorgehen wird für Produktionszwecke vorgeschlagen.

### <a name="update-a-rolling-tag-image"></a>Aktualisieren eines Images mit fortlaufendem Tag

Wenn Sie in Ihrer Bereitstellung fortlaufende Tags verwenden (Beispiel: mcr.microsoft.com/azureiotedge-hub:**1.0**), müssen Sie die Containerruntime auf Ihrem Gerät dazu zwingen, per Pull die neueste Version des Images zu laden. 

Löschen Sie die lokale Version des Images von Ihrem IoT Edge-Gerät. 

```cmd/sh
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Möglicherweise müssen Sie das force-Flag `-f` verwenden, um die Images zu entfernen. 

Der IoT Edge-Dienst lädt per Pull die neuesten Versionen der Runtimeimages herunter und startet sie automatisch wieder auf Ihrem Gerät. 

### <a name="update-a-specific-tag-image"></a>Aktualisieren eines Images mit spezifischem Tag

Wenn Sie in Ihrer Bereitstellung spezifische Tags verwenden (Beispiel: mcr.microsoft.com/azureiotedge-hub:**1.0.2**), brauchen Sie lediglich das Tag in Ihrem Bereitstellungsmanifest zu aktualisieren und die Änderungen auf Ihr Gerät anzuwenden. 

Im Azure-Portal sind die Images zum Bereitstellen der Runtime im Abschnitt **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** deklariert. 

[Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren](./media/how-to-update-iot-edge/configure-runtime.png)

Aktualisieren Sie in einem JSON-Bereitstellungsmanifest die Modulimages im Abschnitt **systemModules**. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.2",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.2",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="next-steps"></a>Nächste Schritte

Zeigen Sie die neuesten [Azure IoT Edge releases](https://github.com/Azure/azure-iotedge/releases) (Azure IoT Edge-Versionen) an.

Bleiben Sie mit neuesten Updates und Bekanntmachungen im [Internet of Things-Blog](https://azure.microsoft.com/blog/topics/internet-of-things/) auf dem Laufenden 
---
title: Installieren von Azure IoT Edge unter Linux | Microsoft-Dokumentation
description: Anweisungen zur Installation von Azure IoT Edge unter Linux
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: kgremban
ms.openlocfilehash: be8fb801acfb625685604a302051d813bfd97939
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325301"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Installieren der Azure IoT Edge-Runtime unter Linux (x64)

Die Azure IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie besteht aus drei Komponenten. Der **Daemon für IoT Edge-Sicherheit** dient zum Bereitstellen und Einhalten von Sicherheitsstandards auf dem Edge-Gerät. Der Daemon wird bei jedem Start gestartet und führt durch Starten des IoT Edge-Agents einen Bootstrap für das Gerät aus. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem Edge-Gerät, einschließlich des IoT Edge-Hubs. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub.

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime auf dem Linux-x64-Edge-Gerät (Intel/AMD) aufgeführt.

>[!NOTE]
>Pakete in den Linux-Softwarerepositorys unterliegen den Lizenzbedingungen im jeweiligen Paket (/usr/share/doc/*Paketname*). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Registrieren des Feeds für Microsoft-Schlüssel und Softwarerepositorys

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/

# Perform apt upgrade
sudo apt-get upgrade
```

## <a name="install-the-container-runtime"></a>Installieren der Containerruntime 

Azure IoT Edge basiert auf einer [OCI-kompatiblen][lnk-oci] Containerruntime. Für Produktionsszenarien wird dringend empfohlen, das unten angegebene [Moby-basierte][lnk-moby] Modul zu verwenden. Es ist das einzige Containermodul, das mit Azure IoT Edge offiziell unterstützt wird. Docker CE/EE-Containerimages sind mit der Moby-Runtime kompatibel.

Aktualisieren Sie „apt-get“.

```bash
sudo apt-get update
```

Installieren Sie die Moby-Engine. 

```bash
sudo apt-get install moby-engine
```

Installieren Sie die Moby-Befehlszeilenschnittstelle (Command-Line Interface, CLI). Die CLI ist nützlich für die Entwicklung, für Produktionsbereitstellungen jedoch optional.

```bash
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>Installieren des Daemons für Azure IoT Edge-Sicherheit

Mit den folgenden Befehlen wird darüber hinaus die Standardversion von **iothsmlib** installiert, sofern noch nicht vorhanden.

```bash
sudo apt-get update
sudo apt-get install iotedge
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Konfigurieren des Daemons für Azure IoT Edge-Sicherheit

Der Daemon kann mithilfe der Konfigurationsdatei unter `/etc/iotedge/config.yaml` konfiguriert werden. Die Datei ist standardmäßig schreibgeschützt. Sie benötigen möglicherweise erhöhte Berechtigungen, um sie bearbeiten können.

```bash
sudo nano /etc/iotedge/config.yaml
```

Das Edgegerät kann manuell über eine [Verbindungszeichenfolge für Geräte][lnk-dcs] oder [automatisch über Device Provisioning Service][lnk-dps] konfiguriert werden.

* Heben Sie bei einer manuellen Konfiguration die Auskommentierung des **manuellen** Bereitstellungsmodus auf. Aktualisieren Sie den Wert von **device_connection_string** durch die Verbindungszeichenfolge Ihres IoT Edge-Geräts.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

* Heben Sie bei einer automatischen Konfiguration die Auskommentierung des **DPS**-Bereitstellungsmodus auf. Aktualisieren Sie die Werte von **scope_id** und **registration_id** durch die Werte aus Ihrer IoT Hub-DPS-Instanz und Ihrem IoT Edge-Gerät mit TPM. 

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfiguration den Daemon neu:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Wenn Sie im vorherigen Abschnitt die Schritte für die **manuelle Konfiguration** ausgeführt haben, müsste die IoT Edge-Runtime erfolgreich bereitgestellt worden sein und auf Ihrem Gerät ausgeführt werden. Wenn Sie die Schritte für die **automatische Konfiguration** ausgeführt haben, sind einige weitere Schritte erforderlich, damit die Runtime Ihr Gerät in Ihrem Namen bei Ihrem IoT-Hub registrieren kann. Informationen zu den nächsten Schritten finden Sie unter [Create and provision a simulated TPM Edge device on a Linux virtual machine (Erstellen und Bereitstellen eines simulierten TPM-Edge-Geräts auf einem virtuellen Linux-Computer)](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Sie können den Status des IoT Edge-Daemons wie folgt überprüfen:

```cmd/sh
systemctl status iotedge
```

Untersuchen Sie die Daemonprotokolle wie folgt:

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Führen Sie die ausgeführten Module wie folgt auf:

```cmd/sh
sudo iotedge list
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Probleme mit der ordnungsgemäßen Installation der Edge-Runtime haben, lesen Sie die Informationen auf der Seite [Problembehandlung][lnk-trouble].

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md

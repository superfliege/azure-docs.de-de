---
title: Installieren von Azure IoT Edge unter Linux | Microsoft-Dokumentation
description: Anweisungen zur Installation von Azure IoT Edge unter Linux in ARM32
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: ad70fcc6b9779cb33772a3fce2fb11b4cec804ee
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062597"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Installieren der Azure IoT Edge-Runtime unter Linux (ARM32v7/armhf)

Die Azure IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie besteht aus drei Komponenten. Der **Daemon für IoT Edge-Sicherheit** dient zum Bereitstellen und Einhalten von Sicherheitsstandards auf dem Edge-Gerät. Der Daemon wird bei jedem Start gestartet und bootstrappt das Gerät durch Starten des IoT Edge-Agents. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem Edge-Gerät, einschließlich IoT Edge-Hub. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub.

In diesem Artikel werden die Schritte zum Installieren der Azure IoT Edge-Runtime auf einem Edge-Gerät des Typs Linux-ARM32v7/armhf (z.B. Raspberry Pi) erläutert.

>[!NOTE]
>Pakete in den Linux-Softwarerepositorys unterliegen den Lizenzbedingungen im jeweiligen Paket (/usr/share/doc/*Paketname*). Lesen Sie die Lizenzbedingungen vor Verwendung des Pakets. Mit der Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

## <a name="install-the-container-runtime"></a>Installieren der Containerruntime

Azure IoT Edge basiert auf einer [OCI-kompatiblen][lnk-oci]-Containerruntime (z.B. Docker). Wenn Sie auf Ihrem Edge-Gerät bereits Docker CE/EE installiert haben, können Sie diese Anwendung weiterhin für die Entwicklung und Tests mit Azure IoT Edge verwenden. 

Für Produktionsszenarien wird dringend empfohlen, das unten angegebene [Moby-basierte][lnk-moby] Modul zu verwenden. Es ist das einzige Containermodul, das mit Azure IoT Edge offiziell unterstützt wird. Docker CE/EE-Containerimages sind mit der Moby-Runtime vollständig kompatibel.

Über die nachstehenden Befehle werden sowohl das Moby-Modul als auch die Befehlszeilenschnittstelle (Command-Line Interface, CLI) installiert. Die CLI ist nützlich für die Entwicklung, für Produktionsbereitstellungen jedoch optional.

```cmd/sh

# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f

```

## <a name="install-the-iot-edge-security-daemon"></a>Installieren des Daemons für IoT Edge-Sicherheit

```cmd/sh
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Konfigurieren des Daemons für IoT Edge-Sicherheit

Der Daemon kann mithilfe der Konfigurationsdatei unter `/etc/iotedge/config.yaml` konfiguriert werden. Das Edge-Gerät kann <!--[automatically via Device Provisioning Service][lnk-dps] or--> manuell mit einer [Geräteverbindungs-Zeichenfolge][lnk-dcs] konfiguriert werden.

Geben Sie für die manuelle Konfiguration die Geräteverbindungs-Zeichenfolge in den Abschnitt **provisioning** der Datei **config.yaml** ein.

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

*Die Datei ist standardmäßig schreibgeschützt. Sie müssen möglicherweise `sudo` verwenden, um sie zu bearbeiten. Beispiel: `sudo nano /etc/iotedge/config.yaml`*

Starten Sie nach Eingabe der Bereitstellungsinformationen in die Konfiguration den Daemon neu:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Sie können den Status des IoT Edge-Daemons wie folgt überprüfen:

```cmd/sh
systemctl status iotedge
```

Untersuchen der Daemonprotokolle mithilfe von:

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Und Auflisten der ausgeführten Module mit:

```cmd/sh
iotedge list
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Probleme mit der ordnungsgemäßen Installation der Edge-Runtime haben, überprüfen Sie die Seite [Problembehandlung][lnk-trouble].

<!-- Links -->
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
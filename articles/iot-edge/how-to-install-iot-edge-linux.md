---
title: Installieren von Azure IoT Edge unter Linux | Microsoft-Dokumentation
description: Anweisungen zur Installation von Azure IoT Edge auf Linux AMD64-Geräten mit ausgeführtem Ubuntu
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 298acb446516a4cdc5057cd6bdcf10422d992ff3
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54900595"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Installieren der Azure IoT Edge-Runtime unter Linux (x64)

Die Azure IoT Edge-Runtime verwandelt ein Gerät in ein IoT Edge-Gerät. Die Runtime kann auf verschiedensten Geräten bereitgestellt werden – vom kleinen Raspberry Pi bis hin zum großen industriellen Server. Wenn ein Gerät mit der IoT Edge-Runtime konfiguriert wurde, können Sie darauf Geschäftslogik aus der Cloud bereitstellen.

Weitere Informationen zur Funktionsweise und zu den Komponenten der IoT Edge-Runtime finden Sie unter [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md).

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime auf dem Linux-x64-Edge-Gerät (Intel/AMD) aufgeführt. Unter [Azure IoT Edge-Support](support.md#operating-systems) finden Sie eine Liste mit derzeit unterstützten AMD64-Betriebssystemen.

> [!NOTE]
> Pakete in den Linux-Softwarerepositorys unterliegen den Lizenzbedingungen im jeweiligen Paket (/usr/share/doc/*Paketname*). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Registrieren des Feeds für Microsoft-Schlüssel und Softwarerepositorys

Bereiten Sie Ihr Gerät für die Installation der IoT Edge-Runtime vor, und ersetzen Sie ```<release>``` abhängig von Ihrer Ubuntu-Version durch **16.04** oder **18.04**.

```bash
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/<release>/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/

# Perform apt upgrade
sudo apt-get upgrade
```

## <a name="install-the-container-runtime"></a>Installieren der Containerruntime

Azure IoT Edge basiert auf einer [OCI-kompatiblen](https://www.opencontainers.org/) Containerruntime. Für Produktionsszenarien wird dringend empfohlen, das unten angegebene [Moby-basierte](https://mobyproject.org/) Modul zu verwenden. Es ist das einzige Containermodul, das mit Azure IoT Edge offiziell unterstützt wird. Docker CE/EE-Containerimages sind mit der Moby-Runtime kompatibel.

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

Der **Daemon für IoT Edge-Sicherheit** dient zum Bereitstellen und Einhalten von Sicherheitsstandards auf dem Edge-Gerät. Der Daemon wird bei jedem Start gestartet und führt durch Starten der restlichen IoT Edge-Runtime einen Bootstrap für das Gerät aus.

Der Installationsbefehl installiert außerdem die Standardversion von **iothsmlib**, falls sie noch nicht vorhanden ist.

Aktualisieren Sie „apt-get“.

```bash
sudo apt-get update
```

Installieren Sie den Sicherheits-Daemon. Das Paket wird unter `/etc/iotedge/` installiert.

```bash
sudo apt-get install iotedge
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Konfigurieren des Daemons für Azure IoT Edge-Sicherheit

Konfigurieren Sie die IoT Edge-Runtime für die Verknüpfung Ihres physischen Geräts mit einer Geräteidentität, die in einem Azure IoT Hub vorhanden ist.

Der Daemon kann mithilfe der Konfigurationsdatei unter `/etc/iotedge/config.yaml` konfiguriert werden. Die Datei ist standardmäßig schreibgeschützt. Sie benötigen möglicherweise erhöhte Berechtigungen, um sie bearbeiten können.

Ein einzelnes IoT Edge-Gerät kann manuell bereitgestellt werden, indem eine von IoT Hub bereitgestellte Gerät-Verbindungszeichenfolge verwendet wird. Oder Sie können den Device Provisioning-Dienst verwenden, um automatisch Geräte bereitzustellen. Dies ist hilfreich, wenn es sich um eine große Zahl von Geräten handelt. Je nach gewählter Bereitstellung können Sie dazu das richtige Installationsskript auswählen.

### <a name="option-1-manual-provisioning"></a>Option 1: Manuelle Bereitstellung

Zur manuellen Bereitstellung eines Geräts müssen Sie es mit einer [Geräteverbindungszeichenfolge](how-to-register-device-portal.md) bereitstellen, die Sie durch die Registrierung eines neuen Geräts in Ihrem IoT-Hub erstellen können.

Öffnen Sie die Konfigurationsdatei.

```bash
sudo nano /etc/iotedge/config.yaml
```

Suchen Sie den Bereitstellungsabschnitt der Datei, und heben Sie die Auskommentierung des **manuellen** Bereitstellungsmodus auf. Aktualisieren Sie den Wert von **device_connection_string** durch die Verbindungszeichenfolge Ihres IoT Edge-Geräts.

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

Speichern und schließen Sie die Datei.

   `CTRL + X`, `Y`, `Enter`

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfigurationsdatei den Daemon neu:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Option 2: Automatische Bereitstellung

Um ein Gerät automatisch bereitzustellen, müssen Sie [den Device Provisioning-Dienst einrichten und die Registrierungs-ID Ihres Geräts abrufen](how-to-auto-provision-simulated-device-linux.md). Die automatische Bereitstellung funktioniert nur mit Geräten, die über einen Trusted Platform Module (TPM)-Chip verfügen. Raspberry Pi-Geräte sind beispielsweise nicht standardmäßig mit TPM ausgestattet.

Öffnen Sie die Konfigurationsdatei.

```bash
sudo nano /etc/iotedge/config.yaml
```

Suchen Sie den Bereitstellungsabschnitt der Datei, und heben Sie die Auskommentierung des **DPS**-Bereitstellungsmodus auf. Aktualisieren Sie die Werte von **scope_id** und **registration_id** mit den Werten aus Ihrer IoT Hub Device Provisioning Service-Instanz und Ihrem IoT Edge-Gerät mit TPM.

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

Speichern und schließen Sie die Datei.

   `CTRL + X`, `Y`, `Enter`

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfigurationsdatei den Daemon neu:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Wenn Sie im vorherigen Abschnitt die Schritte für die **manuelle Konfiguration** ausgeführt haben, müsste die IoT Edge-Runtime erfolgreich bereitgestellt worden sein und auf Ihrem Gerät ausgeführt werden. Wenn Sie die Schritte für die **automatische Konfiguration** ausgeführt haben, sind einige weitere Schritte erforderlich, damit die Runtime Ihr Gerät in Ihrem Namen bei Ihrem IoT-Hub registrieren kann. Informationen zu den nächsten Schritten finden Sie unter [Create and provision a simulated TPM Edge device on a Linux virtual machine (Erstellen und Bereitstellen eines simulierten TPM-Edge-Geräts auf einem virtuellen Linux-Computer)](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Sie können den Status des IoT Edge-Daemons wie folgt überprüfen:

```bash
systemctl status iotedge
```

Untersuchen Sie die Daemonprotokolle wie folgt:

```bash
journalctl -u iotedge --no-pager --no-full
```

Führen Sie die ausgeführten Module wie folgt auf:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>Tipps und Vorschläge

Sie benötigen erhöhte Rechte zum Ausführen von `iotedge`-Befehlen. Melden Sie sich der Installation der Runtime von Ihrem Computer ab und anschließend wieder an, um Ihres Berechtigungen automatisch zu aktualisieren. Verwenden Sie bis dahin **sudo** vor allen `iotedge`-Befehlen.

Auf Geräten mit Ressourceneinschränkungen wird dringend empfohlen, die Umgebungsvariable *OptimizeForPerformance* auf *false* festzulegen, wie im [Leitfaden zur Problembehandlung](troubleshoot.md) beschrieben.

Wenn in Ihrem Netzwerk ein Proxyserver vorhanden ist, führen Sie die Schritte in [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md) aus.

## <a name="uninstall-iot-edge"></a>Deinstallieren von IoT Edge

Wenn Sie die IoT Edge-Installation von Ihrem Linux-Gerät entfernen möchten, verwenden Sie die folgenden Befehle auf der Befehlszeile.

Entfernen Sie die IoT Edge-Runtime.

```bash
sudo apt-get remove --purge iotedge
```

Sobald die IoT Edge-Runtime entfernt wird, werden die Container, die sie erstellt hat, beendet. Sie sind jedoch weiterhin auf dem Gerät vorhanden. Zeigen Sie alle Container an, um die verbliebenen zu sehen.

```bash
sudo docker ps -a
```

Löschen Sie die Container von Ihrem Gerät, einschließlich der zwei Laufzeitcontainer.

```bash
sudo docker rm -f <container name>
```

Entfernen Sie schließlich die Containerruntime von Ihrem Gerät.

```bash
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ein IoT Edge-Gerät für die installierte Runtime bereitgestellt haben, können Sie [IoT Edge-Module bereitstellen](how-to-deploy-modules-portal.md).

Wenn Sie Probleme mit der ordnungsgemäßen Installation der Edge-Runtime haben, lesen Sie die Informationen auf der Seite [Problembehandlung](troubleshoot.md).

Weitere Informationen zum Aktualisieren einer vorhandenen Installation auf die aktuelle Version von IoT Edge finden Sie unter [Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime](how-to-update-iot-edge.md).

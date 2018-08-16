---
title: Installieren von Azure IoT Edge unter Windows mit Linux-Containern | Microsoft-Dokumentation
description: Anweisungen zur Azure IoT Edge-Installation unter Windows mit Linux-Containern
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: kgremban
ms.openlocfilehash: ea576c0d434d4db7077fc41bc1f5bbbc89e7779e
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576646"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Installieren der Azure IoT Edge-Runtime unter Windows zur Verwendung mit Linux-Containern

Die Azure IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie besteht aus drei Komponenten. Der **Daemon für IoT Edge-Sicherheit** dient zum Bereitstellen und Einhalten von Sicherheitsstandards auf dem Edge-Gerät. Der Daemon wird bei jedem Start gestartet und führt durch Starten des IoT Edge-Agents einen Bootstrap für das Gerät aus. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem Edge-Gerät, einschließlich des IoT Edge-Hubs. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub.

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime auf dem Windows-x64-System (AMD/Intel) aufgeführt. Die Windows-Unterstützung befindet sich derzeit in der Vorschauphase.

>[!NOTE]
Die Verwendung von Linux-Containern auf Windows-Systemen ist keine empfohlene oder unterstützte Produktionskonfiguration für Azure IoT Edge. Die Container können jedoch zu Entwicklungs- und Testzwecken eingesetzt werden.

## <a name="supported-windows-versions"></a>Unterstützte Windows-Versionen
Azure IoT Edge kann bei Verwendung von Linux-Containern unter folgenden Windows-Versionen zu Entwicklungs- und Testzwecken genutzt werden:
  * Windows 10 oder neuere Desktopbetriebssysteme
  * Windows Server 2016 oder neuere Serverbetriebssysteme

## <a name="install-the-container-runtime"></a>Installieren der Containerruntime 

Azure IoT Edge basiert auf einer [OCI-kompatiblen][lnk-oci] Containerruntime (z.B. Docker). 

Sie können [Docker für Windows][lnk-docker-for-windows] für Entwicklungs- und Testzwecke nutzen. Konfigurieren Sie Docker für Windows für die [Verwendung von Linux-Containern][lnk-docker-config].

## <a name="install-the-azure-iot-edge-security-daemon"></a>Installieren des Daemons für Azure IoT Edge-Sicherheit

>[!NOTE]
>Für Azure IoT Edge-Softwarepakete gelten die in den Paketen enthaltenen Lizenzbedingungen (im Verzeichnis „LICENSE“). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

Ein einzelnes IoT Edge-Gerät kann manuell bereitgestellt werden, indem eine von IoT Hub bereitgestellte Gerät-Verbindungszeichenfolge verwendet wird. Oder Sie können den Device Provisioning-Dienst verwenden, um automatisch Geräte bereitzustellen. Dies ist hilfreich, wenn es sich um eine große Zahl von Geräten handelt. Je nach gewählter Bereitstellung können Sie dazu das richtige Installationsskript auswählen. 

### <a name="install-and-manually-provision"></a>Installation und manuelle Bereitstellung

1. Führen Sie die Schritte zum [Registrieren eines neuen Azure IoT Edge-Geräts][lnk-dcs] aus, um Ihr Gerät zu registrieren und die Gerät-Verbindungszeichenfolge abzurufen. 

2. Führen Sie PowerShell auf Ihrem IoT Edge-Gerät als Administrator aus. 

3. Laden Sie die IoT Edge-Runtime herunter, und führen Sie die Installation durch. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Linux
   ```

4. Geben Sie die Verbindungszeichenfolge an, die Sie vom IoT Hub abgerufen haben, wenn die Aufforderung zur Eingabe der **DeviceConnectionString** angezeigt wird. Geben Sie die Anführungszeichen um die Verbindungszeichenfolge nicht mit an. 

### <a name="install-and-automatically-provision"></a>Installation und automatische Bereitstellung

1. Führen Sie die Schritte unter [Erstellen und Bereitstellen eines simulierten TPM-Edge-Geräts unter Windows][lnk-dps] aus, um den Device Provisioning-Dienst einzurichten und die **Bereichs-ID** abzurufen, ein TPM-Gerät zu simulieren und die zugehörige **Registrierungs-ID** abzurufen. Erstellen Sie anschließend eine individuelle Registrierung. Nachdem Ihr Gerät in Ihrem IoT Hub registriert wurde, können Sie mit der Installation fortfahren.  

   >[!TIP]
   >Lassen Sie das Fenster, in dem der TPM-Simulator ausgeführt wird, während der Installation und beim Testen geöffnet. 

2. Führen Sie PowerShell auf Ihrem IoT Edge-Gerät als Administrator aus. 

3. Laden Sie die IoT Edge-Runtime herunter, und führen Sie die Installation durch. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Dps -ContainerOs Linux
   ```

4. Geben Sie bei entsprechender Aufforderung die **ScopeID** und **RegistrationID** für Ihren Provisioning-Dienst und das Gerät an.

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Sie können den Status des IoT Edge-Diensts wie folgt überprüfen: 

```powershell
Get-Service iotedge
```

Untersuchen Sie die Dienstprotokolle der letzten fünf Minuten wie folgt:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Führen Sie die ausgeführten Module wie folgt auf:

```powershell
iotedge list
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ein IoT Edge-Gerät für die installierte Runtime bereitgestellt haben, können Sie [IoT Edge-Module bereitstellen][lnk-modules].

Wenn Sie Probleme mit der ordnungsgemäßen Installation der Edge-Runtime haben, lesen Sie die Informationen auf der Seite [Problembehandlung][lnk-trouble].


<!-- Images -->
[img-docker-nat]: ./media/how-to-install-iot-edge-windows-with-linux/dockernat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-modules]: how-to-deploy-modules-portal.md

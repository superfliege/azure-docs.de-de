---
title: Installieren von Azure IoT Edge unter Windows mit Linux-Containern | Microsoft-Dokumentation
description: Anweisungen zur Azure IoT Edge-Installation unter Windows mit Linux-Containern
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: kgremban
ms.openlocfilehash: 2ff7c3482100545c476040ba556d464b9f44e434
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031117"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Installieren der Azure IoT Edge-Runtime unter Windows zur Verwendung mit Linux-Containern

Die Azure IoT Edge-Runtime verwandelt ein Gerät in ein IoT Edge-Gerät. Die Runtime kann auf verschiedensten Geräten bereitgestellt werden – vom kleinen Raspberry Pi bis hin zum großen industriellen Server. Wenn ein Gerät mit der IoT Edge-Runtime konfiguriert wurde, können Sie darauf Geschäftslogik aus der Cloud bereitstellen. 

Weitere Informationen zur Funktionsweise und zu den Komponenten der IoT Edge-Runtime finden Sie unter [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md).

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime mit Linux-Containern auf Ihrem Windows-x64-System (AMD/Intel) aufgeführt. Die Windows-Unterstützung befindet sich derzeit in der Vorschauphase.

>[!NOTE]
Die Verwendung von Linux-Containern auf Windows-Systemen ist keine empfohlene oder unterstützte Produktionskonfiguration für Azure IoT Edge. Die Container können jedoch zu Entwicklungs- und Testzwecken eingesetzt werden.

## <a name="supported-windows-versions"></a>Unterstützte Windows-Versionen
Azure IoT Edge kann bei Verwendung von Linux-Containern unter folgenden Windows-Versionen zu Entwicklungs- und Testzwecken genutzt werden:
  * Windows 10 oder neuere Desktopbetriebssysteme
  * Windows Server 2016 oder neuere Serverbetriebssysteme

Weitere Informationen zu den aktuell unterstützten Betriebssystemen finden Sie unter [Azure IoT Edge-Support](support.md#operating-systems). 

## <a name="install-the-container-runtime"></a>Installieren der Containerruntime 

Azure IoT Edge basiert auf einer [OCI-kompatiblen][lnk-oci] Containerruntime (z.B. Docker). 

Sie können [Docker für Windows][lnk-docker-for-windows] für Entwicklungs- und Testzwecke nutzen. Konfigurieren Sie Docker für Windows für die [Verwendung von Linux-Containern][lnk-docker-config].

## <a name="install-the-azure-iot-edge-security-daemon"></a>Installieren des Daemons für Azure IoT Edge-Sicherheit

>[!NOTE]
>Für Azure IoT Edge-Softwarepakete gelten die in den Paketen enthaltenen Lizenzbedingungen (im Verzeichnis „LICENSE“). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

Ein einzelnes IoT Edge-Gerät kann manuell bereitgestellt werden, indem eine von IoT Hub bereitgestellte Gerät-Verbindungszeichenfolge verwendet wird. Oder Sie können den Device Provisioning-Dienst verwenden, um automatisch Geräte bereitzustellen. Dies ist hilfreich, wenn es sich um eine große Zahl von Geräten handelt. Je nach gewählter Bereitstellung können Sie dazu das richtige Installationsskript auswählen. 

### <a name="option-1-install-and-manually-provision"></a>Option 1: Installation und manuelle Bereitstellung

1. Führen Sie die Schritte zum [Registrieren eines neuen Azure IoT Edge-Geräts][lnk-dcs] aus, um Ihr Gerät zu registrieren und die Gerät-Verbindungszeichenfolge abzurufen. 

2. Führen Sie PowerShell auf Ihrem IoT Edge-Gerät als Administrator aus. 

3. Laden Sie die IoT Edge-Runtime herunter, und führen Sie die Installation durch. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Linux
   ```

4. Geben Sie die Verbindungszeichenfolge an, die Sie vom IoT Hub abgerufen haben, wenn die Aufforderung zur Eingabe der **DeviceConnectionString** angezeigt wird. Geben Sie die Anführungszeichen um die Verbindungszeichenfolge nicht mit an. 

### <a name="option-2-install-and-automatically-provision"></a>Option 2: Installation und automatische Bereitstellung

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

## <a name="tips-and-suggestions"></a>Tipps und Vorschläge

Wenn in Ihrem Netzwerk ein Proxyserver vorhanden ist, führen Sie die Schritte in [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md) aus, um IoT Edge-Runtime zu installieren und zu starten.

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

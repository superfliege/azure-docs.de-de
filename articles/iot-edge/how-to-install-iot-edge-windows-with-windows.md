---
title: Installieren von Azure IoT Edge unter Windows mit Windows-Containern | Microsoft-Dokumentation
description: Anweisungen zur Azure IoT Edge-Installation unter Windows mit Windows-Containern
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: 5fc1163f590b2408fca913e35e57f014424b225c
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308397"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-windows-containers"></a>Installieren der Azure IoT Edge-Runtime unter Windows zur Verwendung mit Windows-Containern

Die Azure IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie besteht aus drei Komponenten. Der **Daemon für IoT Edge-Sicherheit** dient zum Bereitstellen und Einhalten von Sicherheitsstandards auf dem Edge-Gerät. Der Daemon wird bei jedem Start gestartet und führt durch Starten des IoT Edge-Agents einen Bootstrap für das Gerät aus. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem Edge-Gerät, einschließlich des IoT Edge-Hubs. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub.

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime auf dem Windows-x64-System (AMD/Intel) aufgeführt. 

Die Windows-Unterstützung befindet sich derzeit in der Vorschauphase.

## <a name="supported-windows-versions"></a>Unterstützte Windows-Versionen
Azure IoT Edge mit Windows-Containern kann mit Folgendem verwendet werden:
  * Windows 10/IoT Enterprise/IoT Core mit Update von April 2018 (Build 17134)
  * Windows Server 1803

## <a name="install-the-container-runtime"></a>Installieren der Containerruntime 

>[!NOTE]
>Führen Sie zum Installieren der Container-Engine unter Windows IoT Core die Schritte unter [Installieren der IoT Edge-Runtime unter Windows IoT Core – Vorschau][lnk-iot-core] aus, und fahren Sie dann mit den unten aufgeführten Anweisungen fort.

Azure IoT Edge basiert auf einer [OCI-kompatiblen][lnk-oci] Containerruntime (z.B. Docker). Sie können [Docker für Windows][lnk-docker-for-windows] für Entwicklungs- und Testzwecke nutzen. 

**Stellen Sie sicher, dass Docker für Windows [für die Verwendung von Windows-Containern konfiguriert ist][lnk-docker-config].**

## <a name="install-the-azure-iot-edge-security-daemon"></a>Installieren des Daemons für Azure IoT Edge-Sicherheit

>[!NOTE]
>Für Azure IoT Edge-Softwarepakete gelten die in den Paketen enthaltenen Lizenzbedingungen (im Verzeichnis „LICENSE“). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

### <a name="download-the-edge-daemon-package-and-install"></a>Herunterladen und Installieren des Edge-Daemonpakets

Führen Sie in einem PowerShell-Administratorfenster die folgenden Befehle aus:

```powershell
Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
rmdir C:\ProgramData\iotedge\iotedged-windows
$sysenv = "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment"
$path = (Get-ItemProperty -Path $sysenv -Name Path).Path + ";C:\ProgramData\iotedge"
Set-ItemProperty -Path $sysenv -Name Path -Value $path
```

Installieren Sie die vcruntime wie unten angegeben. (Dieser Schritt kann auf einem IoT Core-Edge-Gerät übersprungen werden.)

```powershell
Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
.\vc_redist.exe /quiet /norestart
 ```

Erstellen und starten Sie den **iotedge**-Dienst:

```powershell
New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
Start-Service iotedge
```

Fügen Sie Firewallausnahmen für die Ports hinzu, die vom Dienst verwendet werden:

```powershell
New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
```

Erstellen Sie eine Datei vom Typ **iotedge.reg** mit dem folgenden Inhalt, und importieren Sie sie in die Windows-Registrierung, indem Sie auf die Datei doppelklicken oder den Befehl `reg import iotedge.reg` ausführen:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Konfigurieren des Daemons für Azure IoT Edge-Sicherheit

Der Daemon kann mithilfe der Konfigurationsdatei unter `C:\ProgramData\iotedge\config.yaml` konfiguriert werden.

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

Rufen Sie mit dem Befehl `hostname` in PowerShell den Namen des Edge-Geräts ab, und legen Sie ihn als Wert für **hostname:** in „configuration yaml“ fest. Beispiel: 

```yaml
  ###############################################################################
  # Edge device hostname
  ###############################################################################
  #
  # Configures the environment variable 'IOTEDGE_GATEWAYHOSTNAME' injected into
  # modules.
  #
  ###############################################################################

  hostname: "edgedevice-1"
```

Geben Sie als Nächstes die IP-Adresse und den Port für **workload_uri** und **management_uri** in den Abschnitten **connect:** und **listen:** der Konfiguration an.

Geben Sie zum Abrufen Ihrer IP-Adresse `ipconfig` in das PowerShell-Fenster ein, und kopieren Sie die IP-Adresse der Schnittstelle **vEthernet (nat)**, wie im folgenden Beispiel gezeigt (die IP-Adresse Ihres Systems weicht unter Umständen davon ab):  

![NAT][img-nat]

Aktualisieren Sie **workload_uri** und **management_uri** im Abschnitt **connect:** der Konfigurationsdatei. Ersetzen Sie **\<GATEWAY_ADDRESS\>** durch die von Ihnen kopierte vEthernet-IP-Adresse.

```yaml
connect:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

Geben Sie die gleichen Adressen in den Abschnitt **listen:** ein.

```yaml
listen:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

Erstellen Sie im PowerShell-Fenster eine Umgebungsvariable vom Typ **IOTEDGE_HOST** mit der Adresse für **management_uri**.

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<GATEWAY_ADDRESS>:15580")
```

Behalten Sie die Umgebungsvariable nach einem Neustart bei.

```powershell
SETX /M IOTEDGE_HOST "http://<GATEWAY_ADDRESS>:15580"
```

Vergewissern Sie sich abschließend, dass die Auskommentierung für die Einstellung **network:** unter **moby_runtime:** aufgehoben und die Einstellung auf **nat** festgelegt wurde.

```yaml
moby_runtime:
  docker_uri: "npipe://./pipe/docker_engine"
  network: "nat"
```

Speichern Sie die Konfigurationsdatei, und starten Sie den Dienst neu:

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Wenn Sie im vorherigen Abschnitt die Schritte für die **manuelle Konfiguration** ausgeführt haben, müsste die IoT Edge-Runtime erfolgreich bereitgestellt worden sein und auf Ihrem Gerät ausgeführt werden. Wenn Sie die Schritte für die **automatische Konfiguration** ausgeführt haben, sind einige weitere Schritte erforderlich, damit die Runtime Ihr Gerät in Ihrem Namen bei Ihrem IoT-Hub registrieren kann. Informationen zu den nächsten Schritten finden Sie unter [Erstellen und Bereitstellen eines simulierten TPM-Edge-Geräts unter Windows](how-to-auto-provision-simulated-device-windows.md#create-a-tpm-environment-variable).

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

Wenn Sie Probleme mit der ordnungsgemäßen Installation der Edge-Runtime haben, lesen Sie die Informationen auf der Seite [Problembehandlung][lnk-trouble].


<!-- Images -->
[img-nat]: ./media/how-to-install-iot-edge-windows-with-windows/nat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-iot-core]: how-to-install-iot-core.md


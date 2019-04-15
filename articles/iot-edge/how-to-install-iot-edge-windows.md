---
title: Installieren von Azure IoT Edge unter Windows | Microsoft-Dokumentation
description: Anweisungen zur Azure IoT Edge-Installation unter Windows 10, Windows Server und Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 95e984f6f08af01a2ffd7b9b4e0ec598d73f4d05
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621072"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Installieren der Azure IoT Edge-Runtime unter Windows

Die Azure IoT Edge-Runtime verwandelt ein Gerät in ein IoT Edge-Gerät. Die Runtime kann auf verschiedensten Geräten bereitgestellt werden – vom kleinen Raspberry Pi bis hin zum großen industriellen Server. Wenn ein Gerät mit der IoT Edge-Runtime konfiguriert wurde, können Sie darauf Geschäftslogik aus der Cloud bereitstellen. 

Weitere Informationen zur IoT Edge-Runtime finden Sie unter [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md).

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime auf dem Windows-x64-System (AMD/Intel) aufgeführt. Die Windows-Unterstützung befindet sich derzeit in der Vorschauphase.

> [!NOTE]
> Ein bekanntes Problem des Windows-Betriebssystems verhindert den Übergang in den Energiesparmodus und den Ruhezustand, wenn IoT Edge-Module (vom Prozess isolierte Windows Nano Server-Container) ausgeführt werden. Dieses Problem wirkt sich auf die Akkulaufzeit des Geräts aus.
>
> Geben Sie als Problemumgehung den Befehl `Stop-Service iotedge` ein, um die Ausführung aller IoT Edge-Module zu beenden, bevor Sie diese Energiesparzustände verwenden. 

<!--
> [!NOTE]
> Using Linux containers on Windows systems is not a recommended or supported production configuration for Azure IoT Edge. However, it can be used for development and testing purposes.
-->

Die Verwendung von Linux-Containern auf Windows-Systemen ist keine empfohlene oder unterstützte Produktionskonfiguration für Azure IoT Edge. Die Container können jedoch zu Entwicklungs- und Testzwecken eingesetzt werden. 

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie anhand dieses Abschnitts, ob Ihr Windows-Gerät IoT Edge unterstützt, und bereiten Sie es vor der Installation für eine Container-Engine vor. 

### <a name="supported-windows-versions"></a>Unterstützte Windows-Versionen

Azure IoT Edge unterstützt verschiedene Windows-Versionen. Diese hängen davon ab, ob Sie Windows- oder Linux-Container verwenden. 

Die aktuelle Version von Azure IoT Edge mit Windows-Containern kann unter folgenden Windows-Versionen ausgeführt werden:
* Windows 10 oder IoT Core mit Update von Oktober 2018 (Build 17763)
* Windows Server 2019 (Build 17763)

Die aktuelle Version von Azure IoT Edge mit Linux-Containern kann unter folgenden Windows-Versionen ausgeführt werden: 
* Windows 10 Anniversary Update (Build 14393) oder höher
* Windows Server 2016 oder höher

Weitere Informationen zu den aktuell unterstützten Betriebssystemen finden Sie unter [Azure IoT Edge-Support](support.md#operating-systems). 

Weitere Informationen zur aktuellen Version von IoT Edge finden Sie auf der Website für die [Releases von IoT Edge](https://github.com/Azure/azure-iotedge/releases).

### <a name="prepare-for-a-container-engine"></a>Vorbereitungen für eine Container-Engine 

Azure IoT Edge basiert auf einer [OCI-kompatiblen](https://www.opencontainers.org/) Container-Engine. Verwenden Sie für Produktionsszenarios die Moby-Engine, die im Installationsskript enthalten ist, um Windows-Container auf Ihren Windows-Geräten auszuführen. Für Entwicklungs- und Testzwecke können Sie Linux-Container auf Ihren Windows-Geräten ausführen. Sie müssen jedoch eine Container-Engine installieren und konfigurieren, bevor Sie IoT Edge installieren. Für beide Szenarios finden Sie in den folgenden Abschnitten die Voraussetzungen, die Ihre Geräte erfüllen müssen. 

Wenn Sie IoT Edge auf einer VM installieren möchten, sollten Sie die geschachtelte Virtualisierung aktivieren und mindestens 2 GB Arbeitsspeicher zuordnen. Wie Sie die geschachtelte Virtualisierung aktivieren, hängt vom verwendeten Hypervisor ab. Für Hyper-V verfügen virtuelle Computer der zweiten Generation über eine geschachtelte Virtualisierung, die standardmäßig aktiviert ist. Für VMware gibt es einen Umschalter zum Aktivieren des Features auf dem virtuellen Computer. 

#### <a name="moby-engine-for-windows-containers"></a>Moby-Engine für Windows-Container

Für Windows-Geräte in Produktionsszenarios, auf denen IoT Edge ausgeführt wird, ist Moby die einzige Container-Engine, die offiziell unterstützt wird. Das Installationsskript installiert die Moby-Engine automatisch auf Ihrem Gerät, bevor IoT Edge installiert wird. Bereiten Sie Ihr Gerät vor, indem Sie das Feature für Container aktivieren. 

1. Suchen Sie in der Startleiste nach **Windows-Features aktivieren oder deaktivieren**, und öffnen Sie die Systemsteuerung.
2. Suchen Sie die Option **Container**, und wählen Sie diese aus.
3. Klicken Sie auf **OK**. 

#### <a name="docker-for-linux-containers"></a>Docker für Linux-Container

Wenn Sie Windows verwenden, um Container für Linux-Geräte zu entwickeln und zu testen, können Sie [Docker für Windows](https://www.docker.com/docker-windows) als Container-Engine verwenden. Docker kann für die [Verwendung von Linux-Containern](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) konfiguriert werden. Sie müssen Docker installieren und konfigurieren, bevor Sie IoT Edge installieren. Für Windows-Geräte in Produktionsszenarios werden Linux-Container nicht unterstützt. 

Wenn es sich bei Ihrem IoT Edge-Gerät um einen Windows-Computer handelt, sollten Sie überprüfen, ob dieser die [Systemanforderungen](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) für Hyper-V erfüllt.

## <a name="install-iot-edge-on-a-new-device"></a>Installieren von IoT Edge auf einem neuen Gerät

>[!NOTE]
>Für Azure IoT Edge-Softwarepakete gelten die in den Paketen enthaltenen Lizenzbedingungen (im Verzeichnis „LICENSE“). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

Der Azure IoT Edge-Sicherheitsdaemon wird über ein PowerShell-Skript heruntergeladen und installiert. Der Sicherheitsdaemon startet dann das erste der beiden Runtimemodule, den IoT Edge-Agent. Dieser ermöglicht die Remotebereitstellung anderer Module. 

Wenn Sie die IoT Edge-Runtime zum ersten Mal auf einem Gerät installieren, müssen Sie dem Gerät eine Identität von einem IoT-Hub bereitstellen. Ein einzelnes IoT Edge-Gerät kann manuell bereitgestellt werden, indem eine von IoT Hub bereitgestellte Gerät-Verbindungszeichenfolge verwendet wird. Alternativ können Sie IoT Hub Device Provisioning Service verwenden, um automatisch Geräte bereitzustellen. Dieses Verfahren ist hilfreich, wenn viele Geräte eingerichtet werden müssen. Je nach gewählter Bereitstellung können Sie dazu das richtige Installationsskript auswählen. 

In den folgenden Abschnitten werden die häufigsten Anwendungsfälle und Parameter für IoT Edge-Installationsskripts auf einem neuen Gerät erläutert. 

### <a name="option-1-install-and-manually-provision"></a>Option 1: Installation und manuelle Bereitstellung

Hier stellen Sie eine Geräteverbindungszeichenfolge bereit, die von IoT Hub generiert wurde, um das Gerät bereitzustellen. 

Führen Sie die unter [Registrieren eines neuen Azure IoT Edge-Geräts](how-to-register-device-portal.md) aufgeführten Schritte aus, um Ihr Gerät zu registrieren und die Geräteverbindungszeichenfolge abzurufen. 

In diesem Beispiel wird die manuelle Installation mit Windows-Containern veranschaulicht:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -DeviceConnectionString '<connection-string>'
```

Wenn Sie ein Gerät manuell installieren und bereitstellen, können Sie zusätzliche Parameter verwenden, um die Installation folgendermaßen anzupassen:
* Sie können direkten Datenverkehr über einen Proxyserver leiten.
* Sie können das Installationsprogramm auf ein Offlineverzeichnis verweisen.
* Sie können ein bestimmtes Containerimage für den Agent deklarieren und Anmeldeinformationen bereitstellen, wenn dieses sich in einer privaten Registrierung befindet.
* Sie können die Installation der Moby-CLI überspringen.

Weitere Informationen zu diesen Installationsoptionen finden Sie in diesem Artikel. Sie können diesen auch überspringen und [hier](#all-installation-parameters) mehr über alle Installationsparameter erfahren.

### <a name="option-2-install-and-automatically-provision"></a>Option 2: Installation und automatische Bereitstellung

Hier stellen Sie das Gerät mithilfe von IoT Hub Device Provisioning Service bereit. Geben Sie die **Bereichs-ID** aus einer Instanz von IoT Hub Device Provisioning Service und die **Registrierungs-ID** Ihres Geräts an.

Führen Sie die unter [Erstellen und Bereitstellen eines simulierten TPM-Edge-Geräts unter Windows](how-to-auto-provision-simulated-device-windows.md) aufgeführten Schritte aus, um den Device Provisioning-Dienst einzurichten und die **Bereichs-ID** abzurufen, ein TPM-Gerät zu simulieren und die zugehörige **Registrierungs-ID** abzurufen. Erstellen Sie anschließend eine individuelle Registrierung. Nachdem Ihr Gerät in Ihrem IoT Hub registriert wurde, können Sie mit der Installation fortfahren.  

   >[!TIP]
   >Lassen Sie das Fenster, in dem der TPM-Simulator ausgeführt wird, während der Installation und beim Testen geöffnet. 

Im folgenden Beispiel wird die automatische Installation mit Windows-Containern veranschaulicht:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Dps -ContainerOs Windows -ScopeId <DPS scope ID> -RegistrationId <device registration ID>
```

Wenn Sie ein Gerät manuell installieren und bereitstellen, können Sie zusätzliche Parameter verwenden, um die Installation folgendermaßen anzupassen:
* Sie können direkten Datenverkehr über einen Proxyserver leiten.
* Sie können das Installationsprogramm auf ein Offlineverzeichnis verweisen.
* Sie können ein bestimmtes Containerimage für den Agent deklarieren und Anmeldeinformationen bereitstellen, wenn dieses sich in einer privaten Registrierung befindet.
* Sie können die Installation der Moby-CLI überspringen.

Weitere Informationen zu diesen Installationsoptionen finden Sie in diesem Artikel. Sie können diesen auch überspringen und [hier](#all-installation-parameters) mehr über alle Installationsparameter erfahren.

## <a name="update-an-existing-installation"></a>Aktualisieren einer bestehenden Installation

Wenn Sie die IoT Edge-Runtime bereits auf einem Gerät installiert und mit einer Identität von IoT Hub bereitgestellt haben, können Sie ein vereinfachtes Installationsskript verwenden. Das Flag `-ExistingConfig` gibt an, dass auf dem Gerät bereits eine IoT Edge-Konfigurationsdatei vorhanden ist. Die Konfigurationsdatei enthält Informationen zur Identität des Geräts sowie zu den Zertifikaten und Netzwerkeinstellungen. Sie können diese Installationsoption unabhängig davon verwenden, ob Ihr Gerät manuell oder automatisch bereitgestellt wurde. 

Weitere Informationen finden Sie unter [Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime](how-to-update-iot-edge.md).

In diesem Beispiel wird eine Installation veranschaulicht, die auf eine vorhandene Konfigurationsdatei verweist und Windows-Container verwendet: 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -ExistingConfig -ContainerOs Windows
```

Wenn Sie IoT Edge über eine vorhandene Konfigurationsdatei installieren, können Sie zusätzliche Parameter verwenden, um die Installation folgendermaßen anzupassen:
* Sie können direkten Datenverkehr über einen Proxyserver leiten.
* Sie können das Installationsprogramm auf ein Offlineverzeichnis verweisen. 
* Sie können die Installation der Moby-CLI überspringen. 

Sie können das Containerimage eines IoT Edge-Agents nicht mit Skriptparametern deklarieren, da diese Informationen durch die vorherige Installation bereits in der Konfigurationsdatei festgelegt wurden. Wenn Sie das Containerimage eines Agents ändern möchten, müssen Sie dies in der config.yaml-Datei durchführen. 

Weitere Informationen zu diesen Installationsoptionen finden Sie in diesem Artikel. Sie können diesen auch überspringen und [hier](#all-installation-parameters) mehr über alle Installationsparameter erfahren.

## <a name="offline-installation"></a>Offlineinstallation

Während der Installation werden vier Dateien mit folgendem Inhalt heruntergeladen: 
* IoT Edge-Sicherheitsdaemon (ZIP-Datei) 
* Moby-Engine (ZIP-Datei)
* Moby-CLI (ZIP-Datei)
* Visual C++ Redistributable Package (VC-Runtime, MSI-Datei)

Sie können eine oder alle diese Dateien vorher auf das Gerät herunterladen und das Verzeichnis dieser Dateien im Installationsskript angeben. Das Installationsprogramm überprüft dieses Verzeichnis dann zuerst und lädt nur Komponenten herunter, die nicht gefunden werden konnten. Wenn alle vier Dateien offline verfügbar sind, können Sie die Installation ohne Internetverbindung durchführen. Sie können dieses Feature außerdem verwenden, um die Onlineversionen dieser Komponenten außer Kraft zu setzen.  

Die aktuellen Installationsdateien und die früheren Versionen finden Sie auf der Seite für [Azure IoT Edge-Releases](https://github.com/Azure/azure-iotedge/releases).

Wenn Sie die Installation mit Offlinekomponenten durchführen möchten, müssen Sie den Parameter `-OfflineInstallationPath` verwenden und den absoluten Pfad zum Dateiverzeichnis angeben. Beispiel:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -DeviceConnectionString '<connection-string>' -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

## <a name="all-installation-parameters"></a>Alle Installationsparameter

In den vorherigen Abschnitten wurden gängige Installationsszenarios vorgestellt. Dabei wurden Beispiele angegeben, wie Sie Parameter verwenden können, um das Installationsskript anzupassen. Dieser Abschnitt enthält eine Verweistabelle mit den gültigen Parametern, die Sie für die Installation von IoT Edge verwenden können. Führen Sie `get-help Install-SecurityDaemon -full` in einem PowerShell-Fenster aus, um weitere Informationen zu erhalten. 

Sie können folgende allgemeine Parameter in Ihrem Installationsbefehl verwenden, um IoT Edge mit einer vorhandenen Konfiguration zu installieren: 

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| **Manuell** | Keine | **Switch-Parameter:** Jede Installation muss auf „Manual“, „Dps“ oder „ExistingConfig“ festgelegt werden.<br><br>Gibt an, dass Sie eine Geräteverbindungszeichenfolge angeben, um das Gerät manuell bereitzustellen. |
| **Dps** | Keine | **Switch-Parameter:** Jede Installation muss auf „Manual“, „Dps“ oder „ExistingConfig“ festgelegt werden.<br><br>Gibt an, dass Sie eine Bereichs-ID von einem Gerätebereitstellungsdienst (Device Provisioning Service, DPS) und die Registrierungs-ID Ihres Geräts angeben, damit die Bereitstellung über einen Gerätebereitstellungsdienst durchgeführt wird.  |
| **ExistingConfig** | Keine | **Switch-Parameter:** Jede Installation muss auf „Manual“, „Dps“ oder „ExistingConfig“ festgelegt werden.<br><br>Gibt an, dass eine config.yaml-Datei mit den erforderlichen Bereitstellungsinformationen bereits auf dem Gerät vorhanden ist. |
| **DeviceConnectionString** | Eine Verbindungszeichenfolge, die von einem IoT Edge-Gerät stammt, das in einem IoT-Hub registriert ist (in einfachen Anführungszeichen). | Dieser Parameter ist für die manuelle Installation **erforderlich**. Wenn Sie keine Verbindungszeichenfolge in den Skriptparametern angeben, werden Sie während der Installation dazu aufgefordert. |
| **ScopeId** | Eine Bereichs-ID, die aus der Instanz des Gerätebereitstellungsdiensts stammt, die Ihrem IoT-Hub zugeordnet ist. | Dieser Parameter ist für die Installation über einen Gerätebereitstellungsdienst **erforderlich**. Wenn Sie keine Bereichs-ID in den Skriptparametern angeben, werden Sie während der Installation dazu aufgefordert. |
| **RegistrationId** | Eine Registrierungs-ID, die von Ihrem Gerät generiert wurde. | Dieser Parameter ist für die Installation über einen Gerätebereitstellungsdienst **erforderlich**. Wenn Sie keine Registrierungs-ID in den Skriptparametern angeben, werden Sie während der Installation dazu aufgefordert. |
| **ContainerOs** | **Windows** oder **Linux** | Wenn kein Containerbetriebssystem angegeben wird, wird der Standardwert „Linux“ verwendet. Für Windows-Container wird eine Container-Engine im Rahmen der Installation installiert. Für Linux-Container müssen Sie eine Container-Engine installieren, bevor Sie mit der Installation beginnen. Das Ausführen von Linux-Containern unter Windows kann in Entwicklungsszenarios nützlich sein, wird in Produktionsszenarios jedoch nicht unterstützt. |
| **Proxy** | Proxy-URL | Verwenden Sie diesen Parameter, wenn Ihr Gerät die Internetverbindung über einen Proxyserver herstellen muss. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Hashtabelle mit Parametern und Werten | Während der Installation werden mehrere Webanforderungen durchgeführt. Verwenden Sie dieses Feld, um die Parameter für diese Webanforderungen festzulegen. Dieser Parameter ist bei der Konfiguration der Anmeldeinformationen für Proxyserver hilfreich. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Verzeichnispfad | Wenn dieser Parameter verwendet wird, überprüft das Installationsprogramm das Verzeichnis auf die Dateien für den IoT Edge-Sicherheitsdaemon (ZIP), die Moby-Engine (ZIP), die Moby-CLI (ZIP) und die VC-Runtime (MSI). Diese sind für die Installation erforderlich. Wenn alle vier Dateien sich im Verzeichnis befinden, können Sie die Installation von IoT Edge offline durchführen. Sie können diesen Parameter zudem verwenden, um die Onlineversion einer bestimmten Komponente außer Kraft zu setzen. |
| **AgentImage** | URI des IoT Edge-Agent-Images | Wenn Sie IoT Edge neu installieren, wird standardmäßig das neueste fortlaufende Tag für das IoT Edge-Agent-Image verwendet. Verwenden Sie diesen Parameter, um ein bestimmtes Tag für die Imageversion festzulegen, oder stellen Sie ein eigenes Agent-Image bereit. Weitere Informationen finden Sie unter [Grundlagen von IoT Edge-Tags](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Benutzername** | Benutzername der Containerregistrierung | Verwenden Sie diesen Parameter nur, wenn Sie den Parameter „-AgentImage“ auf einen Container in einer privaten Registrierung festlegen. Geben Sie einen Benutzernamen an, der auf die Registrierung zugreifen kann. |
| **Kennwort** | Sichere Kennwortzeichenfolge | Verwenden Sie diesen Parameter nur, wenn Sie den Parameter „-AgentImage“ auf einen Container in einer privaten Registrierung festlegen. Geben Sie das Kennwort an, mit dem auf die Registrierung zugegriffen werden kann. | 
| **SkipMobyCli** | Keine | Dieser Parameter kann nur verwendet werden, wenn „-ContainerOS“ auf „Windows“ festgelegt ist. Installieren Sie die Moby-CLI (docker.exe) nicht unter „$MobyInstallDirectory“. |

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

Nach einer Neuinstallation sollte als einziges Modul **edgeAgent** ausgeführt werden. Nachdem Sie die [IoT Edge-Module bereitgestellt](how-to-deploy-modules-portal.md) haben, werden weitere angezeigt. 

## <a name="manage-module-containers"></a>Verwalten von Modulcontainern

Der IoT Edge-Dienst erfordert eine Containerengine, die auf Ihrem Gerät ausgeführt wird. Wenn Sie ein Modul auf einem Gerät bereitstellen, verwendet die IoT Edge-Runtime die Containerengine, um das Containerimage per Pullvorgang aus einer Registrierung in der Cloud abzurufen. Der IoT Edge-Dienst ermöglicht es Ihnen, mit Ihren Modulen zu interagieren und Protokolle abzurufen, aber unter Umständen möchten Sie die Containerengine verwenden, um mit dem Container selbst zu interagieren. 

Weitere Informationen zu Modulkonzepten finden Sie unter [Grundlegendes zu Azure IoT Edge-Modulen](iot-edge-modules.md). 

Wenn Sie Windows-Container auf Ihrem Windows IoT Edge-Gerät ausgeführt haben, dann umfasste die IoT Edge-Installation die Moby-Containerengine. Wenn Sie Linux-Container auf Ihrem Windows-Entwicklungscomputer entwickeln, verwenden Sie wahrscheinlich Docker Desktop. Die Moby-Engine basierte auf denselben Standards wie Docker und wurde für die parallele Ausführung auf demselben Computer wie Docker Desktop entwickelt. Wenn Sie von der Moby-Engine verwaltete Container als Ziel festlegen möchten, müssen Sie diese Engine anstelle von Docker als Ziel auswählen. 

Verwenden Sie z. B. den folgenden Befehl, um alle Docker-Images aufzulisten:

```powershell
docker images
```

Ändern Sie denselben Befehl mit einem Zeiger auf die Moby-Engine, um alle Moby-Images aufzulisten: 

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

Die Engine-URI ist in der Ausgabe des Installationsskripts aufgeführt, oder Sie finden sie im Abschnitt für die Einstellungen der Containerruntime der Datei „config.yaml“. 

![moby_runtime uri in config.yaml](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Weitere Informationen zu Befehlen, mit denen Sie mit Containern und Images interagieren können, die auf Ihrem Gerät ausgeführt werden, finden Sie unter [Docker-Befehlszeilenschnittstellen](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="uninstall-iot-edge"></a>Deinstallieren von IoT Edge

Wenn Sie IoT Edge von Ihrem Windows-Gerät entfernen möchten, sollten Sie folgenden Befehl in einem PowerShell-Fenster ausführen, das mit Administratorrechten geöffnet wurde. Dieser Befehl entfernt die IoT Edge-Runtime, die vorhandenen Konfigurationen und die Daten der Moby-Engine. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-SecurityDaemon -DeleteConfig -DeleteMobyDataRoot
```

Wenn Sie IoT Edge erneut auf Ihrem Gerät installieren möchten, sollten Sie die Parameter `-DeleteConfig` und `-DeleteMobyDataRoot` nicht verwenden, damit Sie den Sicherheitsdaemon in Zukunft erneut installieren können, ohne die Konfigurationsinformationen erneut festlegen zu müssen. 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ein IoT Edge-Gerät für die installierte Runtime bereitgestellt haben, können Sie [IoT Edge-Module bereitstellen](how-to-deploy-modules-portal.md).

Wenn Probleme bei der Installation von IoT Edge auftreten, finden Sie auf der Seite für die [Problembehandlung](troubleshoot.md) weitere Informationen.

Weitere Informationen zum Aktualisieren einer vorhandenen Installation auf die aktuelle Version von IoT Edge finden Sie unter [Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime](how-to-update-iot-edge.md).

---
title: Installieren von Azure IoT Edge unter IoT Core | Microsoft-Dokumentation
description: Installieren der Azure IoT Edge-Runtime auf einem Windows IoT Core-Gerät
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: af6ce22d1b41ebfe9b64ea5fc1a2cf99478af925
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34630033"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Installieren der IoT Edge-Runtime unter Windows IoT Core – Vorschau

Azure IoT Edge und [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) arbeiten zusammen, um Edge Computing auf kleinen Geräten zu ermöglichen. Die Azure IoT Edge-Runtime kann sogar auf kleinsten Einplatinencomputern ausgeführt werden, die in der IoT-Branche sehr weit verbreitet sind. 

Dieser Artikel leitet Sie durch die Bereitstellung der Runtime auf einer [MinnowBoard Turbot][lnk-minnow]-Entwicklerplatine, auf der Windows IoT Core ausgeführt wird. Windows IoT Core unterstützt Azure IoT Edge nur auf Intel x64-basierten Prozessoren. 

## <a name="install-the-runtime"></a>Installieren der Runtime

1. Installieren Sie [Windows 10 IoT Core Dashboard][lnk-core] auf einem Hostsystem.
1. Führen Sie die unter [Einrichten Ihres Geräts][lnk-board] beschriebenen Schritte durch, um das MinnowBoard Turbot/MAX Build 16299-Image auf Ihrer Platine zu konfigurieren. 
1. Schalten Sie das Gerät ein, und [melden Sie sich remote bei PowerShell][lnk-powershell] an.
1. Installieren Sie die Containerruntime in der PowerShell-Konsole: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-0.0.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >Diese Containerruntime stammt vom Buildserver des Moby-Projekts und dient lediglich zu Auswertungszwecken. Sie wird von Docker nicht getestet, empfohlen oder unterstützt.

1. Installieren Sie die IoT Edge-Runtime, und überprüfen Sie die Konfiguration:

   ```powershell
   Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
   ```

   Dieses Skript stellt Folgendes bereit: 
   * Python 3.6
   * Das IoT Edge-Steuerskript („iotedgectl.exe“)

Möglicherweise werden vom Tool „iotedgectl.exe“ Informationen in grüner Farbe im PowerShell-Remotefenster ausgegeben. Dies weist nicht unbedingt auf Fehler hin. 

## <a name="next-steps"></a>Nächste Schritte

Sie verfügen nun über ein Gerät, auf dem die IoT-Edge-Runtime ausgeführt wird. Als Nächstes erfahren Sie, wie Sie [IoT Edge-Module im größeren Maßstab bereitstellen und überwachen][lnk-deploy].

<!--Links-->
[lnk-minnow]: https://minnowboard.org/ 
[lnk-core]: https://docs.microsoft.com/windows/iot-core/connect-your-device/iotdashboard
[lnk-board]: https://developer.microsoft.com/windows/iot/Docs/GetStarted/mbm/sdcard/stable/getstartedstep2
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
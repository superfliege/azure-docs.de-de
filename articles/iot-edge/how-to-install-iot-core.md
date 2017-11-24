---
title: Installieren von Azure IoT Edge unter IoT Core | Microsoft-Dokumentation
description: "Installieren der Azure IoT Edge-Runtime auf einem Windows IoT Core-Gerät"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: be2a80645d23e709d6c5cfb3978498bbe85eca34
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Installieren der IoT Edge-Runtime unter Windows IoT Core – Vorschau

Die Azure IoT Edge-Runtime kann sogar auf kleinsten Einplatinencomputern ausgeführt werden, die in der IoT-Branche sehr weit verbreitet sind. Dieser Artikel leitet Sie durch die Bereitstellung der Runtime auf einer [MinnowBoard Turbot][lnk-minnow]-Entwicklerplatine, auf der Windows IoT Core ausgeführt wird.

## <a name="install-the-runtime"></a>Installieren der Runtime

1. Installieren Sie [Windows 10 IoT Core Dashboard][lnk-core] auf einem Hostsystem.
1. Führen Sie die unter [Einrichten Ihres Geräts][lnk-board] beschriebenen Schritte durch, um das MinnowBoard Turbot/MAX Build 16299-Image auf Ihrer Platine zu konfigurieren. 
1. Schalten Sie das Gerät ein, und [melden Sie sich remote bei PowerShell][lnk-powershell] an.
1. [Installieren Sie die Docker-Binärdateien][lnk-docker-install] in der PowerShell-Konsole.
1. Führen Sie den folgenden Befehl in der PowerShell-Konsole aus, um die IoT Edge-Runtime zu installieren, und überprüfen Sie Ihre Konfiguration:

   ```powershell
   Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
   ```

   Dieses Skript stellt Folgendes bereit: 
   * Python 3.6
   * Das IoT Edge-Steuerskript („iotedgectl.exe“)

Möglicherweise werden vom Tool „iotedgectl.exe“ Informationen in roter Farbe im PowerShell-Remotefenster ausgegeben. Dies weist nicht unbedingt auf Fehler hin. 

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
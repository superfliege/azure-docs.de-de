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
ms.openlocfilehash: f57db00894dab80f96f45111331d47a173520ced
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39575997"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Installieren der IoT Edge-Runtime unter Windows IoT Core – Vorschau

Azure IoT Edge und [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) arbeiten zusammen, um Edge Computing auf kleinen Geräten zu ermöglichen. Die Azure IoT Edge-Runtime kann sogar auf kleinsten Einplatinencomputern ausgeführt werden, die in der IoT-Branche sehr weit verbreitet sind. 

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie die Runtime auf einer Entwicklerplatine mit Windows IoT Core bereitstellen. 

**Azure IoT Edge wird von Windows IoT Core aktuell nur auf Intel x64-basierten Prozessoren unterstützt.**

## <a name="install-the-container-runtime"></a>Installieren der Containerruntime

1. Konfigurieren Sie Ihre Platine mit **Build 17134 (RS4)** des IoT Core-Images. 
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

## <a name="finish-installing"></a>Abschließen der Installation

Installieren Sie den Daemon für IoT Edge-Sicherheit, und konfigurieren Sie ihn gemäß den Anweisungen in [diesem Artikel][lnk-install-windows-on-windows].

## <a name="next-steps"></a>Nächste Schritte

Sie verfügen nun über ein Gerät, auf dem die IoT-Edge-Runtime ausgeführt wird. Als Nächstes erfahren Sie, wie Sie [IoT Edge-Module im größeren Maßstab bereitstellen und überwachen][lnk-deploy].

<!--Links-->
[lnk-install-windows-on-windows]: how-to-install-iot-edge-windows-with-windows.md
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers

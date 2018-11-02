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
ms.openlocfilehash: ac729963e63bd97c83719e21dad3ad2cfc9b4fee
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392793"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Installieren der IoT Edge-Runtime unter Windows IoT Core – Vorschau

Azure IoT Edge und [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) arbeiten zusammen, um Edge Computing auf kleinen Geräten zu ermöglichen. Die Azure IoT Edge-Runtime kann sogar auf kleinsten Einplatinencomputern ausgeführt werden, die in der IoT-Branche sehr weit verbreitet sind. 

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie die Runtime auf einer Entwicklerplatine mit Windows IoT Core bereitstellen. 

**Azure IoT Edge wird von Windows IoT Core aktuell nur auf Intel x64-basierten Prozessoren unterstützt.**

## <a name="install-the-container-runtime"></a>Installieren der Containerruntime

1. Konfigurieren Sie Ihre Platine mit **Build 17134 (RS4)** des IoT Core-Images. 
1. Schalten Sie das Gerät ein, und [melden Sie sich remote bei PowerShell](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell) an.
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

Installieren Sie den Daemon für IoT Edge-Sicherheit, und konfigurieren Sie ihn gemäß den Anweisungen in [diesem Artikel](how-to-install-iot-edge-windows-with-windows.md).

## <a name="next-steps"></a>Nächste Schritte

Sie verfügen nun über ein Gerät, auf dem die IoT-Edge-Runtime ausgeführt wird. Als Nächstes erfahren Sie, wie Sie [IoT Edge-Module im größeren Maßstab bereitstellen und überwachen](how-to-deploy-monitor.md).
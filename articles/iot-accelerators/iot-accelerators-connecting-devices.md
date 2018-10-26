---
title: Bereitstellen von Windows-Geräten für die Remoteüberwachung in C – Azure | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie ein Gerät mit dem Solution Accelerator für Remoteüberwachung verbinden, indem Sie eine in C geschriebene Anwendung unter Windows ausführen.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: dobett
ms.openlocfilehash: 55c8ff799ba3ff7fe9691d46dc90a00d5182d390
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829409"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Herstellen einer Verbindung zwischen Ihrem Gerät und dem Solution Accelerator für Remoteüberwachung (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

In diesem Tutorial wird gezeigt, wie Sie eine Verbindung zwischen einem physischen Gerät und dem Solution Accelerator für Remoteüberwachung herstellen.

Wie bei den meisten eingebetteten Anwendungen, die auf eingeschränkten Geräten ausgeführt werden, wird der Clientcode für die Geräteanwendung in C geschrieben. In diesem Tutorial erstellen Sie die Geräteclientanwendung auf einem Computer, auf dem Windows ausgeführt wird.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie für diese Schrittanleitung die Schritte zum [Einrichten der Windows-Entwicklungsumgebung](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) aus, um Ihrem Windows-Computer die erforderlichen Entwicklungstools und -bibliotheken hinzuzufügen.

## <a name="view-the-code"></a>Anzeigen des Codes

Der in dieser Anleitung verwendete [Beispielcode](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) ist im GitHub-Repository mit den Azure IoT C SDKs verfügbar.

### <a name="download-the-source-code-and-prepare-the-project"></a>Herunterladen des Quellcodes und Vorbereiten des Projekts

Klonen Sie zum Vorbereiten des Projekts das [Repository mit Azure IoT C SDKs](https://github.com/Azure/azure-iot-sdk-c) auf GitHub, oder laden Sie es herunter.

Das Beispiel befindet sich im Ordner **samples/solutions/remote_monitoring_client**.

Öffnen Sie die Datei **remote_monitoring.c** im Ordner **samples/solutions/remote_monitoring_client** in einem Text-Editor.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Bearbeiten Sie die Datei **remote_monitoring.c**. Ersetzen Sie `<connectionstring>` durch die Geräteverbindungszeichenfolge, die Sie zu Beginn dieser Schrittanleitung notiert haben, als Sie ein Gerät dem Solution Accelerator hinzugefügt haben.

1. Führen Sie die Schritte in [Build the C SDK in Windows (Erstellen des C SDK in Windows)](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#build-the-c-sdk-in-windows) aus, um das SDK und die Clientanwendung zur Remoteüberwachung zu erstellen.

1. Führen Sie an der Befehlszeile, die Sie zum Erstellen der Lösung verwendet haben, Folgendes aus:

    ```cmd
    samples\solutions\remote_monitoring_client\Release\remote_monitoring_client.exe
    ```

    Die Konsole zeigt Meldungen an wie:

    - Die Anwendung sendet Beispieltelemetriedaten an den Solution Accelerator.
    - Reagiert auf Methoden, die über das Lösungsdashboard aufgerufen werden.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

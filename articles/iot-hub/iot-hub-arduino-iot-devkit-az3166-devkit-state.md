---
title: Steuern der Benutzer-LED des MXChip IoT DevKit mithilfe von Azure-Gerätezwillingen | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie mithilfe von Azure IoT Hub-Gerätezwillingen DevKit-Status überwacht und die Benutzer-LED gesteuert werden.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2018
ms.author: liydu
ms.openlocfilehash: 33d8a36cc88bd1c263f2c4a38a59e04e1253357c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="mxchip-iot-devkit"></a>MXChip IoT DevKit

Sie können dieses Beispiel verwenden, um mithilfe von Azure IoT Hub-Gerätezwillingen die WLAN-Informationen und Sensorstatus des MXChip IoT DevKit zu überwachen und die Farbe der Benutzer-LED zu steuern.

## <a name="what-you-learn"></a>Lerninhalt

- Überwachen der Sensorstatus des MXChip IoT DevKit
- Steuern der Farbe der RGB-LED des DevKit mithilfe von Azure-Gerätezwillingen

## <a name="what-you-need"></a>Voraussetzungen

- Richten Sie Ihre Entwicklungsumgebung anhand des [Leitfadens zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) ein.
- Geben Sie in Ihrem GitBash-Terminalfenster (oder an einer anderen Git-Befehlszeilenschnittstelle) die folgenden Befehle ein:
    - `git clone https://github.com/DevKitExamples/DevKitState.git`
    - `cd DevKitState`
    - `code .`

## <a name="provision-azure-services"></a>Bereitstellen von Azure-Diensten

1. Klicken Sie in Visual Studio Code auf das Dropdownmenü **Aufgabe**, und wählen Sie **Aufgabe ausführen...** - **cloud-provision**.
2. Der Fortschritt wird unter der Registerkarte **TERMINAL** des Bereichs **Willkommen** angezeigt.
3. Wenn eine Meldung mit der Frage angezeigt wird, *welches Abonnement Sie wählen möchten*, wählen Sie ein Abonnement aus.
4. Wählen Sie eine Ressourcengruppe aus. 
 
    > [!NOTE]
    > Wenn Sie bereits über eine kostenlose IoT Hub-Instanz verfügen, wird dieser Schritt übersprungen werden.

5. Wenn eine Meldung mit der Frage angezeigt wird, *welche IoT Hub-Instanz Sie wählen möchten*, wählen Sie eine IoT Hub-Instanz aus, oder erstellen Sie eine.
6. Die Anzeige sieht etwa wie folgt aus: *Funktions-App: Name der Funktions-App: xxx*. Notieren Sie sich den Namen der Funktions-App; er wird in einem späteren Schritt verwendet.
7. Warten Sie, bis die Bereitstellung der Azure Resource Manager-Vorlage abgeschlossen ist. Dies wird durch die Meldung *Resource Manager template deployment: Done* (Bereitstellung der Resource Manager-Vorlage: Abgeschlossen) angegeben.

## <a name="deploy-function-app"></a>Bereitstellen der Funktionen-App

1. Klicken Sie in Visual Studio Code auf das Dropdownmenü **Aufgabe**, und wählen Sie **Aufgabe ausführen...** - **cloud-deploy**.
2. Warten Sie, bis der Funktions-App-Code hochgeladen ist; die Meldung *function app deploys: Done* (Funktions-App-Bereitstellungen: Abgeschlossen) wird angezeigt.

## <a name="configure-iot-hub-device-connection-string-in-devkit"></a>Konfigurieren der Verbindungszeichenfolge für das IoT Hub-Gerät in DevKit

1. Stellen Sie eine Verbindung zwischen dem MXChip IoT DevKit und Ihrem Computer her.
2. Klicken Sie in Visual Studio Code auf das Dropdownmenü **Aufgabe**, und wählen Sie **Aufgabe ausführen...** - **config-device-connection**.
3. Halten Sie im MXChip IoT DevKit die Taste **A** gedrückt, drücken Sie die **Rücksetztaste**, und lassen Sie dann die Taste **A** los, um den Konfigurationsmodus für das DevKit zu aktivieren.
4. Warten Sie, bis der Konfigurationsprozess für die Verbindungszeichenfolge abgeschlossen ist.

## <a name="upload-arduino-code-to-devkit"></a>Hochladen von Arduino-Code zum DevKit

Führen Sie die folgenden Schritte aus, während Ihr MXChip IoT DevKit mit Ihrem Computer verbunden ist:
1. Klicken Sie in Visual Studio Code auf das Dropdownmenü **Aufgabe**, und wählen Sie **Buildtask ausführen...**. Die Arduino-Skizze wird kompiliert und zum DevKit hochgeladen.
2. Nach dem erfolgreichen Hochladen der Skizze wird die Meldung *Build & Upload Sketch: success* (Skizze erstellen und hochladen: Erfolg) angezeigt.

## <a name="monitor-devkit-state-in-browser"></a>Überwachen des DevKit-Status im Browser

1. Öffnen Sie in einem Webbrowser die Datei `DevKitState\web\index.html`, welche im Schritt [Voraussetzungen](#whatyouneed) erstellt wurde.
2. Die folgende Webseite wird angezeigt:![Geben Sie den Namen der Funktions-App an.](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state-function-app-name.png)
1. Geben Sie Funktions-App-Namen ein, den Sie zuvor notiert haben.
2. Klicken Sie auf die Schaltfläche **Verbinden**.
3. Innerhalb weniger Sekunden wird die Seite aktualisiert und zeigt den WLAN-Verbindungsstatus des DevKit und den Status aller integrierten Sensoren an.

## <a name="control-the-devkits-user-led"></a>Steuern der Benutzer-LED des DevKit

1. Klicken Sie auf der Abbildung der Webseite auf die Grafik der Benutzer-LED.
2. Innerhalb weniger Sekunden wird der Bildschirm aktualisiert und zeigt den aktuellen Farbstatus der Benutzer-LED an.
3. Klicken Sie auf den RGB-Schiebereglern an verschiedenen Stellen, um den Farbwert der RGB-LED zu ändern.

## <a name="example-operation"></a>Beispielvorgang

![Beispieltestprozedur](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state.gif)

## <a name="next-steps"></a>Nächste Schritte

Folgendes wurde vermittelt:
- Verbinden eines MXChip IoT DevKit-Geräts mit Ihrer Azure IoT Suite-Instanz
- Verwenden der Gerätezwillingsfunktion von Azure IoT, um die Farbe der RGB-LED des DevKit zu erkennen und zu steuern

Hier sind die empfohlenen nächsten Schritte aufgeführt:

* [Übersicht über Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/)
* [Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Microsoft IoT Central-Anwendung](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)

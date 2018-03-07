---
title: "IoT DevKit in der Cloud – Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie, wie der Status von Sensoren auf IoT DevKit AZ3166 zur Überwachung und Visualisierung an Azure IoT Suite gesendet wird."
services: iot-hub
documentationcenter: 
author: liydu
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2018
ms.author: liydu
ms.openlocfilehash: b43061f5af3e836ba3f0b37eb11b351a769890be
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-suite-for-remote-monitoring"></a>Verbinden von IoT DevKit AZ3166 mit Azure IoT Suite zur Remoteüberwachung

In diesem Tutorial erfahren Sie, wie Sie eine Beispiel-App auf dem DevKit ausführen, um Sensordaten an die Azure IoT Suite zu senden.

Das [MXChip IoT DevKit](https://aka.ms/iot-devkit) ist ein mit Arduino kompatibles All-in-One-Board mit umfangreichen Peripheriegeräten und Sensoren. Sie können dafür mithilfe der [Visual Studio Code-Erweiterung für Arduino](https://aka.ms/arduino) entwickeln. Im Lieferumfang enthalten ist ein wachsender [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), der Sie durch das Erstellen eines Prototyps von Lösungen für das Internet der Dinge (Internet of Things, IoT) führt, die Microsoft Azure-Dienste nutzen.

## <a name="what-you-need"></a>Voraussetzungen

Befolgen Sie die Anweisungen im [Leitfaden zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started), um folgende Voraussetzungen zu erfüllen:

* Herstellen einer Wi-Fi-Verbindung mit dem DevKit
* Vorbereiten der Entwicklungsumgebung

Ein aktives Azure-Abonnement. Wenn Sie kein Abonnement haben, können Sie sich mit einer der beiden folgenden Methoden registrieren:

* [Kostenloses Microsoft Azure-Testkonto für 30 Tage](https://azureinfo.microsoft.com/us-freetrial.html) aktivieren
* [Azure-Gutschrift](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) beanspruchen, wenn Sie MSDN- oder Visual Studio-Abonnent sind

## <a name="create-an-azure-iot-suite"></a>Erstellen einer Azure IoT Suite

1. Wechseln Sie zur [Azure IoT Suite-Website](https://www.azureiotsuite.com/), und klicken Sie auf **Neue Lösung erstellen**.
  ![Auswählen des Azure IoT Suite-Typs](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-solution-types.png)
  > [!NOTE]
  > Standardmäßig wird mit diesem Beispiel nach dem Erstellen einer IoT Suite ein S2 IoT Hub erstellt. Wenn dieser IoT Hub nicht für eine große Anzahl von Geräten verwendet wird, wird dringend empfohlen, ein Downgrade von S2 auf S1 durchzuführen und die IoT Suite zu löschen, damit auch der zugehörige IoT Hub gelöscht werden kann, wenn Sie ihn nicht mehr benötigen. 

2. Wählen Sie **Remoteüberwachung** aus.

3. Geben Sie einen Lösungsnamen ein, wählen Sie ein Abonnement und eine Region aus, und klicken Sie dann auf **Lösung erstellen**. Es kann eine Weile dauern, bis die Lösung bereitgestellt wird.
  ![Erstellen einer Lösung](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution.png)

4. Sobald die Bereitstellung abgeschlossen ist, klicken Sie auf **Starten**. Während des Bereitstellungsprozesses werden einige simulierte Geräte für die Lösung erstellt. Klicken Sie auf **GERÄTE**, um diese anzuzeigen. ![Dashboard](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution-created.png)
  ![Konsole](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-console.png)

5. Klicken Sie auf **GERÄT HINZUFÜGEN**.

6. Klicken Sie unter **Benutzerdefiniertes Gerät** auf **Neu hinzufügen**.
  ![Hinzufügen eines neuen Geräts](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-add-new-device.png)

7. Klicken Sie auf **Ich möchte meine eigene Geräte-ID definieren**, geben Sie `AZ3166` ein, und klicken Sie dann auf **Erstellen**.
  ![Erstellen eines Geräts mit ID](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-device-configuration.png)

8. Notieren Sie sich den **IoT Hub-Hostnamen**, und klicken Sie auf **Fertig**.

## <a name="open-the-remotemonitoring-sample"></a>Öffnen des Beispiels „RemoteMonitoring“

1. Trennen Sie das DevKit von Ihrem Computer, falls eine Verbindung besteht.

2. Starten Sie Visual Studio Code.

3. Verbinden Sie das DevKit mit Ihrem Computer. Visual Studio Code erkennt automatisch das DevKit und öffnet die folgenden Seiten:
  * DevKit-Einführungsseite
  * Arduino-Beispiele: Praktische Beispiele für die ersten Schritte mit DevKit

4. Erweitern Sie auf der linken Seite **ARDUINO-BEISPIELE**, wechseln Sie zu **Beispiele für MXCHIP AZ3166 > AzureIoT**, und wählen Sie **RemoteMonitoring** aus. Daraufhin wird ein neues Visual Studio Code-Fenster mit einem Projektordner geöffnet.
  > [!NOTE]
  > Falls Sie den Bereich schließen sollten, können Sie ihn erneut öffnen. Öffnen Sie mit der Tastenkombination `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) die Befehlspalette, geben Sie **Arduino** ein, suchen Sie dann nach **Arduino: Beispiele**, und wählen Sie sie aus.

## <a name="provision-required-azure-services"></a>Bereitstellen erforderlicher Azure-Dienste

Führen Sie im Lösungsfenster die Aufgabe über `Ctrl+P` (macOS: `Cmd+P`) durch Eingabe von `task cloud-provision` im bereitgestellten Textfeld aus.

Im Visual Studio Code-Terminal werden Sie über eine interaktive Befehlszeile durch die Bereitstellung der erforderlichen Azure-Dienste geführt:

![Bereitstellen von Azure-Ressourcen](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/provision.png)

## <a name="build-and-upload-the-device-code"></a>Erstellen und Hochladen des Gerätecodes

1. Verwenden Sie `Ctrl+P` (macOS: `Cmd + P`), und geben Sie **task config-device-connection** ein.

2. Das Terminal fragt, ob Sie die Verbindungszeichenfolge verwenden möchten, die aus dem Schritt `task cloud-provision` abgerufen wird. Sie können auch eine eigene Geräte-Verbindungszeichenfolge eingeben, indem Sie auf „Neu erstellen...“ klicken.

3. Das Terminal fordert Sie zum Wechseln in den Konfigurationsmodus auf. Halten Sie hierfür Taste A gedrückt, und drücken Sie anschließend gleichzeitig einmal die Reset-Taste. Auf dem Bildschirm werden die DevKit-ID und „Konfiguration“ angezeigt.
  ![Eingeben einer Verbindungszeichenfolge](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/config-device-connection.png)

4. Sobald `task config-device-connection` abgeschlossen ist, klicken Sie auf `F1`, um Visual Studio Code-Befehle zu laden, und wählen Sie `Arduino: Upload` aus. Anschließend beginnt Visual Studio Code mit dem Überprüfen und Hochladen des Arduino-Sketches: ![Überprüfen und Hochladen des Arduino-Sketches](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/arduino-upload.png)

Das DevKit startet neu und beginnt mit der Ausführung des Codes.

## <a name="test-the-project"></a>Testen des Projekts

Beim Ausführen der Beispiel-App sendet DevKit Sensordaten über Wi-Fi an die Azure IoT Suite. Gehen Sie wie folgt vor, um das Ergebnis anzuzeigen:

1. Wechseln Sie zur Azure IoT Suite, und klicken Sie auf **DASHBOARD**.

2. In der Azure IoT Suite-Lösungskonsole wird der Status des DevKit-Sensors angezeigt.

![Sensordaten in Azure IoT Suite](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/sensor-status.png)

## <a name="change-device-id"></a>Ändern der Geräte-ID

Sie können die Geräte-ID in IoT Hub anhand [dieser Anleitung](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/) ändern. Wenn Sie die hartcodierte ID **AZ3166** im Code in eine angepasste Geräte-ID ändern möchten, finden Sie [hier](https://github.com/Microsoft/devkit-sdk/blob/master/AZ3166/src/libraries/AzureIoT/examples/RemoteMonitoring/RemoteMonitoring.ino#L23) die Codezeile, die Sie ändern können.

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie ein DevKit-Gerät mit der Azure IoT Suite verbinden und die Sensordaten darstellen, werden die folgenden nächsten Schritte empfohlen:

* [Übersicht über Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/)
* [Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Microsoft IoT Central-Anwendung](https://docs.microsoft.com/en-us/microsoft-iot-central/howto-connect-devkit)

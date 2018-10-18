---
title: 'IoT DevKit in der Cloud: Verbinden von IoT DevKit AZ3166 mit dem Solution Accelerator für die IoT-Remoteüberwachung | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie der Status von Sensoren auf IoT DevKit AZ3166 zur Überwachung und Visualisierung an den Solution Accelerator für die IoT-Remoteüberwachung gesendet wird.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: isacabe
ms.openlocfilehash: 32742b2a680370f443051e2d86f90d94e8632850
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720581"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>Verbinden von MXChip IoT DevKit AZ3166 mit dem Solution Accelerator für die IoT-Remoteüberwachung

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Sie erfahren, wie Sie eine Beispiel-App auf Ihrem IoT DevKit ausführen, um Sensordaten an Ihren Solution Accelerator zu senden.

Das [MXChip IoT DevKit](https://aka.ms/iot-devkit) ist ein mit Arduino kompatibles All-in-One-Board mit umfangreichen Peripheriegeräten und Sensoren. Sie können dafür in Visual Studio Code mithilfe der [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) entwickeln. Im Lieferumfang enthalten ist ein wachsender [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), der Sie durch das Erstellen eines Prototyps von Lösungen für das Internet der Dinge (Internet of Things, IoT) führt, die Microsoft Azure-Dienste nutzen.

## <a name="what-you-need"></a>Voraussetzungen

Gehen Sie den [Leitfaden für erste Schritte](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) durch, und **beenden Sie nur die folgenden Abschnitte**:

* Vorbereiten der Hardware
* Konfigurieren von WLAN
* Starten unter Verwendung von DevKit
* Vorbereiten der Entwicklungsumgebung

## <a name="open-the-remote-monitoring-sample-in-vs-code"></a>Öffnen des Remote Monitoring-Beispiels in VS Code

1. Stellen Sie sicher, dass Ihr IoT-DevKit **nicht** mit Ihrem Computer verbunden ist. Starten Sie zuerst Visual Studio Code, und stellen Sie dann eine Verbindung von DevKit mit Ihrem Computer her.

2. Klicken Sie auf `F1`, um die Befehlspalette zu öffnen, geben Sie **IoT Workbench: Beispiele** ein, und wählen Sie es aus. Wählen Sie dann **IoT DevKit** als Board aus.

3. Suchen Sie **Remoteüberwachung**, und klicken Sie auf **Beispiel öffnen**. Daraufhin wird ein neues VS Code-Fenster mit einem Projektordner geöffnet.
  ![IoT Workbench, Beispiel für „Remoteüberwachung“ auswählen](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-iot-hub-device-connection-string"></a>Konfigurieren der Verbindungszeichenfolge für das IoT Hub-Gerät

1. Schalten Sie das IoT DevKit in den **Konfigurationsmodus** um. Gehen Sie dazu wie folgt vor:
   * Halten Sie die Taste **A** gedrückt.
   * Drücken Sie gleichzeitig einmal die **Reset**-Taste.

2. Auf dem Bildschirm werden die DevKit-ID und „Konfiguration“ angezeigt.
   
  ![IoT DevKit-Konfigurationsmodus](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

3. Klicken Sie auf `F1`, um die Befehlspalette zu öffnen, geben Sie **IoT Workbench: Gerät > Geräteeinstellungen konfigurieren** ein, und wählen Sie dies aus.

4. Fügen Sie die soeben kopierte Verbindungszeichenfolge ein, und klicken Sie auf `Enter`, um sie zu konfigurieren.

## <a name="build-and-upload-the-device-code"></a>Erstellen und Hochladen des Gerätecodes

1. Klicken Sie auf `F1`, um die Befehlspalette zu öffnen, geben Sie **IoT Workbench: Gerät > Gerät hochladen** ein, und wählen Sie dies aus.
  ![IoT Workbench: Gerät – > Hochladen](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-upload.png)

1. Dann beginnt VS Code, den Code zu kompilieren und auf Ihr DevKit hochzuladen.
  ![IoT Workbench: Gerät – > Hochgeladen](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

Das DevKit startet neu und beginnt mit der Ausführung des Codes.

## <a name="test-the-project"></a>Testen des Projekts

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>Anzeigen der Telemetriedaten, die an die Lösung für die Remoteüberwachung gesendet wurden

Beim Ausführen der Beispiel-App sendet DevKit Sensordaten über WLAN an Ihre Lösung für die Remoteüberwachung. Gehen Sie wie folgt vor, um das Ergebnis anzuzeigen:

1. Wechseln Sie zu Ihrem Lösungsdashboard, und klicken Sie auf **Geräte**.

2. Klicken Sie auf den Gerätenamen; auf der Registerkarte rechts sehen Sie den Sensorstatus auf DevKit in Echtzeit.
  ![Sensordaten in Azure IoT Suite](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="send-a-c2d-message"></a>Senden einer C2D-Nachricht

Mit der Lösung für die Remoteüberwachung können Sie die Remotemethode auf dem Gerät aufrufen. Der Beispielcode veröffentlicht drei Methoden, die Sie im Abschnitt **Methode** sehen können, wenn der Sensor ausgewählt ist.

![IoT DevKit-Methoden](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

Wir wollen jetzt versuchen, die Farbe einer der DevKit-LEDs mithilfe der Methode „LedColor“ ändern.

1. Wählen Sie den Gerätenamen aus der Geräteliste aus, und klicken Sie auf die **Aufträge**.

  ![Erstellen eines Auftrags](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

2. Konfigurieren Sie die Aufträge wie unten gezeigt, und klicken Sie auf **Übernehmen**.
  * Wählen Sie „Auftrag: **Run-Methode**“ aus.
  * Methodenname: **LedColor**
  * Auftragsname: **ChangeLedColor**
  
  ![Auftragseinstellungen](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

Innerhalb weniger Sekunden sollte Ihr DevKit die Farbe der RGB-LED ändern (unterhalb der A-Taste).

![IoT DevKit, rote LED](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Tutorials fortfahren möchten, können Sie die Bereitstellung des Solution Accelerators für die Remoteüberwachung beibehalten.

Falls Sie den Solution Accelerator nicht mehr benötigen, können Sie ihn auf der Seite „Bereitgestellte Lösungen“ löschen, indem Sie ihn auswählen und dann auf „Lösung löschen“ klicken:

![Löschen der Lösung](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [häufig gestellten Fragen zum IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie die Verbindung eines DevKit-Geräts mit dem Solution Accelerator für die Azure IoT-Remoteüberwachung herstellen und die Sensordaten darstellen, werden die folgenden nächsten Schritte empfohlen:

* [Was sind die IoT Solution Accelerators?](https://docs.microsoft.com/azure/iot-suite/)
* [Anpassen der Benutzeroberfläche](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md)
* [Herstellen einer Verbindung zwischen IoT DevKit und Ihrer Azure IoT Central-Anwendung](../iot-central/howto-connect-devkit.md)
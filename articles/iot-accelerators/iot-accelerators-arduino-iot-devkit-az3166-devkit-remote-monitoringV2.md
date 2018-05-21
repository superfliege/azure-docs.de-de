---
title: IoT DevKit in der Cloud – Verbinden von IoT DevKit AZ3166 mit Azure IoT Suite Remote Monitoring v2 | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie der Status von Sensoren auf IoT DevKit AZ3166 zur Überwachung und Visualisierung an Remote Monitoring v2 von Azure IoT Suite gesendet wird.
services: iot-hub
documentationcenter: ''
author: isabelcabezasm
manager: ''
tags: ''
keywords: ''
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: 667e51acd5ac1367e185fb9bf9a7949a13e061af
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="connect-mxchip-iot-devkit-az3166-to-azure-iot-suite-for-remote-monitoring-v2"></a>Verbinden von MXChip IoT DevKit AZ3166 mit Azure IoT Suite Remote Monitoring v2


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

In diesem Tutorial erfahren Sie, wie Sie eine Beispiel-App auf dem DevKit ausführen, um Sensordaten an die Azure IoT Suite zu senden.

Das [MXChip IoT DevKit](https://aka.ms/iot-devkit) ist ein mit Arduino kompatibles All-in-One-Board mit umfangreichen Peripheriegeräten und Sensoren. Sie können dafür mithilfe der [Visual Studio Code-Erweiterung für Arduino](https://aka.ms/arduino) entwickeln. Im Lieferumfang enthalten ist ein wachsender [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), der Sie durch das Erstellen eines Prototyps von Lösungen für das Internet der Dinge (Internet of Things, IoT) führt, die Microsoft Azure-Dienste nutzen.

## <a name="what-you-need"></a>Voraussetzungen

Befolgen Sie die Anweisungen im [Leitfaden zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started), um folgende Voraussetzungen zu erfüllen:

* Herstellen einer Wi-Fi-Verbindung mit dem DevKit
* Vorbereiten der Entwicklungsumgebung


## <a name="open-the-remotemonitoring-sample"></a>Öffnen des Beispiels „RemoteMonitoring“

1. Trennen Sie das DevKit von Ihrem Computer, falls eine Verbindung besteht.

2. Starten Sie Visual Studio Code.

3. Verbinden Sie das DevKit mit Ihrem Computer. Visual Studio Code erkennt automatisch das DevKit und öffnet die folgenden Seiten:
  * DevKit-Einführungsseite
  * Arduino-Beispiele: Praktische Beispiele für die ersten Schritte mit DevKit

4. Erweitern Sie auf der linken Seite **ARDUINO-BEISPIELE**, wechseln Sie zu **Beispiele für MXCHIP AZ3166 > AzureIoT**, und wählen Sie **RemoteMonitoringv2** aus. Daraufhin wird ein neues Visual Studio Code-Fenster mit einem Projektordner geöffnet.

  ![Öffnen des Projekts für die Remoteüberwachung](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)


  > [!NOTE]
  > Falls Sie den Bereich schließen sollten, können Sie ihn erneut öffnen. Öffnen Sie mit der Tastenkombination `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) die Befehlspalette, geben Sie **Arduino** ein, suchen Sie dann nach **Arduino: Beispiele**, und wählen Sie sie aus.

## <a name="add-a-new-physical-device"></a>Hinzufügen eines neuen physischen Geräts

Wechseln Sie im Portal zum Abschnitt **Geräte**, und klicken Sie dort auf die Schaltfläche **+ Neues Gerät**. 

![Hinzufügen eines neuen Geräts](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device.png)

Das *Formular für neue Geräte* muss ausgefüllt sein.
1. Klicken Sie auf im Abschnitt **Gerätetyp** auf *Physisch*.
2. Definieren Sie Ihre eigenen Geräte-ID (z.B. *MXChip* oder *AZ3166*).
3. Wählen Sie im Abschnitt **Authentifizierungsschlüssel** die Option *Schlüssel automatisch generieren*.
4. Klicken Sie auf die Schaltfläche *Anwenden*.

![Hinzufügen eines Formulars für neue Geräte](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-new-device-form.png)

Warten Sie, bis die Bereitstellung des neuen Geräts im Portal abgeschlossen ist.

![Bereitstellen eines neuen Geräts ](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device-provisioning.png)


Anschließend wird die Konfiguration des neuen Geräts angezeigt.
Kopieren Sie die generierte **Verbindungszeichenfolge**.

![Verbindungszeichenfolge von Gerät](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-new-device-connstring.png)


Diese Verbindungszeichenfolge wird im nächsten Abschnitt benötigt.





## <a name="build-and-upload-the-device-code"></a>Erstellen und Hochladen des Gerätecodes

Wechseln Sie zurück zum Visual Studio-Code: 

1. Verwenden Sie `Ctrl+P` (macOS: `Cmd + P`), und geben Sie **task config-device-connection** ein.

  ![Auswählen Ihres Azure-Abonnements und Ihres IoT Hubs](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. Sie werden gefragt, ob Sie die Verbindungszeichenfolge des IoT-Geräts verwenden möchten. Wählen Sie *Neue erstellen*, und fügen Sie sie jetzt ein.

  ![Einfügen der Verbindungszeichenfolge](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. In manchen Fällen werden Sie vom Terminal aufgefordert, in den Konfigurationsmodus zu wechseln. Halten Sie dazu die Taste A gedrückt, drücken Sie die Reset-Taste und lassen Sie sie wieder los, und lassen Sie dann die Taste A wieder los. Auf dem Bildschirm werden die DevKit-ID und „Konfiguration“ angezeigt.

  ![DevKit-Bildschirm des Geräts](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > Die Verbindungszeichenfolge sollte in der Zwischenablage gespeichert sein, wenn Sie dem letzten Abschnitt des Tutorials gefolgt sind. Wenn nicht, sollten Sie zum Azure-Portal wechseln und nach dem IoT Hub Ihrer Remote Monitoring-Ressourcengruppe suchen. Dort können Sie die mit IoT Hub verbundenen Geräte sehen und die Verbindungszeichenfolge des Geräts kopieren.

  ![Suchen nach der Verbindungszeichenfolge](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)


Jetzt können Sie Ihr neues physisches Gerät im VS Code-Abschnitt „Azure IoT Hub-Geräte“ sehen:

![Anzeige des neuen IoT Hub-Geräts](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

## <a name="test-the-project"></a>Testen des Projekts

Beim Ausführen der Beispiel-App sendet DevKit Sensordaten über Wi-Fi an die Azure IoT Suite. Gehen Sie wie folgt vor, um das Ergebnis anzuzeigen:

1. Wechseln Sie zur Azure IoT Suite, und klicken Sie auf **DASHBOARD**.

2. In der Azure IoT Suite-Lösungskonsole wird der Status des DevKit-Sensors angezeigt. 

![Sensordaten in Azure IoT Suite](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Wenn Sie auf den Sensornamen (AZ3166) klicken, öffnet sich auf der rechten Seite des Dashboards eine Registerkarte, in der Sie das Diagramm mit den MX-Chipsensoren in Echtzeit sehen können.


## <a name="send-a-c2d-message"></a>Senden einer C2D-Nachricht
Mit Remote Monitoring v2 können Sie die Remotemethode auf dem Gerät aufrufen.
Der MX-Chipbeispielcode veröffentlicht drei Methoden, die Sie im Abschnitt „Methode“ sehen können, wenn der Sensor ausgewählt ist.

![MX-Chip – Methoden](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

Sie können die Farbe einer MX-Chip-LED mithilfe der Methode „LedColor“ ändern. Aktivieren Sie dazu das Kontrollkästchen des Geräts, und klicken Sie auf die Schaltfläche „Zeitplan“. 

![MX-Chip – Methoden](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Wählen Sie die Methode „ChangeColor“ in der Dropdownliste, in der alle Methoden angezeigt werden, geben Sie einen Namen ein, und wählen Sie „Anwenden“.

![MX-Chip – Dropdownliste](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

Innerhalb weniger Sekunden sollte Ihr physikalischer MX-Chip die Farbe der RGB-LED ändern (unterhalb der A-Taste).

![MX-Chip – LED](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)


## <a name="change-device-id"></a>Ändern der Geräte-ID

Sie können die Geräte-ID in IoT Hub anhand [dieser Anleitung](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/) ändern.


## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie ein DevKit-Gerät mit der Azure IoT Suite verbinden und die Sensordaten darstellen, werden die folgenden nächsten Schritte empfohlen:

* [Übersicht über Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/)
* [Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Microsoft IoT Central-Anwendung](https://docs.microsoft.com/en-us/microsoft-iot-central/howto-connect-devkit)

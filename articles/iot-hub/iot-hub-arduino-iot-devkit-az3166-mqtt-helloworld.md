---
title: Senden von Nachrichten an einen MQTT-Server mithilfe der Azure-MQTT-Clientbibliothek | Microsoft-Dokumentation
description: Verwenden Sie das DevKit als Client, um Nachrichten an einen MQTT-Server zu senden.
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
ms.date: 04/02/2018
ms.author: liydu
ms.openlocfilehash: 121bd8a21e74a836fcca74af97fc20cae297d9a2
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="send-messages-to-an-mqtt-server"></a>Senden von Nachrichten an einen MQTT-Server

In Systemen des Internets der Dinge (IoT) sind Internetverbindungen häufig langsam oder von schlechter Qualität oder werden zeitweilig unterbrochen. MQTT ist ein M2M-Konnektivitätsprotokoll (Machine-to-Machine), das für solche Herausforderungen entwickelt wurde. 

Die hier verwendete MQTT-Clientbibliothek gehört zum [Eclipse Paho](http://www.eclipse.org/paho/)-Projekt, das APIs für die Verwendung von MQTT über verschiedene Transportmittel bereitstellt.

## <a name="what-you-learn"></a>Lerninhalt

In diesem Projekt lernen Sie Folgendes:
- Verwenden der MQTT-Clientbibliothek zum Senden von Nachrichten an einen MQTT-Broker
- Konfigurieren Ihres MXChip-Iot-DevKits als MQTT-Client.

## <a name="what-you-need"></a>Voraussetzungen

Befolgen Sie die Anweisungen im [Leitfaden zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started), um folgende Voraussetzungen zu erfüllen:

* Herstellen einer Wi-Fi-Verbindung mit dem DevKit
* Vorbereiten der Entwicklungsumgebung

## <a name="open-the-project-folder"></a>Öffnen des Projektordners

1. Trennen Sie das DevKit von Ihrem Computer, falls eine Verbindung besteht.

2. Starten Sie Visual Studio Code.

3. Verbinden Sie das DevKit mit Ihrem Computer.
    Visual Studio Code erkennt automatisch das DevKit und öffnet die folgenden Seiten:
    * DevKit-Einführungsseite
    * Arduino-Beispiele: Praktische Beispiele für die ersten Schritte mit Ihrem DevKit.

    ![mini-solution-vscode](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/vscode-start.png)

## <a name="open-the-mqttclient-sample"></a>Öffnen des MQTTClient-Beispiels

Erweitern Sie auf der linken Seite **ARDUINO-BEISPIELE**, wechseln Sie zu **Beispiele für MXCHIP AZ3166 > MQTT**, und wählen Sie **MQTTClient**. Daraufhin wird ein neues VS Code-Fenster mit einem Projektordner geöffnet.

![Ordner für Beispiele](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/examples.png)

> [!NOTE]
> Falls Sie den Bereich schließen sollten, können Sie ihn erneut öffnen. Öffnen Sie mit der Tastenkombination `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) die Befehlspalette, geben Sie **Arduino** ein, suchen Sie dann nach **Arduino: Beispiele**, und wählen Sie sie aus.

## <a name="build-and-upload-the-arduino-sketch-to-the-devkit"></a>Erstellen und Hochladen des Arduino-Sketches in das DevKit

Drücken Sie `Ctrl+P` (macOS: `Cmd+P`), um `task device-upload` auszuführen. Sobald der Upload abgeschlossen ist, wird das DevKit neu gestartet und führt den Sketch aus.

![device-upload](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/device-upload.jpg)

> [!NOTE]
> Möglicherweise erhalten Sie eine Fehlermeldung der Art „Fehler: AZ3166: Unbekanntes Paket“. Dieser Fehler tritt auf, wenn der Boardpaketindex nicht richtig aktualisiert wurde. Informationen zur Behebung dieses Fehlers finden Sie in den [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Testen des Projekts

Führen Sie in Visual Studio-Code die folgenden Schritte aus, um den seriellen Monitor zu starten und einzurichten:

1. Klicken Sie in der Statusleiste auf das `COM[X]`-Wort, um den rechten COM-Anschluss auf `STMicroelectronics` festzulegen: ![COM-Anschluss einrichten](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-com-port.jpg)

2. Klicken Sie in der Statusleiste auf das Netzsteckersymbol, um den seriellen Monitor zu öffnen: ![Serieller Monitor](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-monitor.jpg)
  
3. Klicken Sie in der Statusleiste auf die Zahl, die die Baudrate darstellt, und legen Sie diese auf `115200` fest: ![Baudrate festlegen](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-baud-rate.jpg)

Der serielle Monitor zeigt alle Nachrichten an, die vom Beispielsketch gesendet wurden. Der Sketch verbindet das DevKit mit dem WLAN. Sobald die WLAN-Verbindung erfolgreich hergestellt wurde, sendet der Sketch eine Nachricht an den MQTT-Broker. Danach sendet das Beispiel wiederholt zwei „iot.eclipse.org“-Nachrichten über QoS 0 bzw. QoS 1.

![Serielle Ausgabe](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-output.jpg)

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, helfen Ihnen die Informationen in den [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) weiter, oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="see-also"></a>Weitere Informationen

* [Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub in der Cloud]({{"/docs/getting-started/" | absolute_url }})
* [Shake, Shake for a Tweet]({{"/docs/projects/shake-shake/" | absolute_url }})

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Ihr MXChip-Iot-DevKit als MQTT-Client konfigurieren und die MQTT-Clientbibliothek verwenden, um Nachrichten an einen MQTT-Broker zu senden. Dies sind die empfohlenen nächsten Schritte:

* [Übersicht über Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/)
* [Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Microsoft IoT Central-Anwendung](https://docs.microsoft.com/en-us/microsoft-iot-central/howto-connect-devkit)

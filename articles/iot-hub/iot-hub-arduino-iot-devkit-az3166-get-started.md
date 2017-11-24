---
title: "IoT DevKit in der Cloud – Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub | Microsoft-Dokumentation"
description: In diesem Tutorial erfahren Sie, wie Sie IoT DevKit AZ3166 einrichten und eine Verbindung mit Azure IoT Hub herstellen, um Daten an die Azure-Cloudplattform zu senden.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: xshi
ms.openlocfilehash: 0b8ae318fab2eaa186dca050ce2710b1ff232783
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/18/2017
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a>Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub in der Cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Sie können das [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/)-Board zur Entwicklung von IoT-Lösungen (Internet of Things, Internet der Dinge), die Microsoft Azure-Dienste nutzen, sowie zum Erstellen eines Prototyps dieser Lösungen verwenden. Es enthält ein Arduino-kompatibles Board mit leistungsstarken Peripheriegeräten und Sensoren, ein Open Source-Boardpaket und einen wachsenden [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Aufgaben
Sie verbinden [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) mit einer Azure IoT Hub-Instanz, die Sie erstellen, erfassen die Temperatur- und Luftfeuchtigkeitsdaten von Sensoren und senden die Daten an IoT Hub.

Sie haben noch kein DevKit-Board? [Jetzt kaufen](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Lerninhalt

* Herstellen einer Verbindung von IoT DevKit mit einem drahtlosen Zugriffspunkt und Vorbereiten der Entwicklungsumgebung.
* Erstellen einer IoT Hub-Instanz und Registrieren eines Geräts für das MXChip IoT DevKit.
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf dem MXChip IoT DevKit-Board.
* Senden der Sensordaten an Ihren IoT Hub

## <a name="what-you-need"></a>Erforderliches Element

* Ein MXChip IoT DevKit-Board mit einem Micro-USB-Kabel. [Jetzt kaufen](https://aka.ms/iot-devkit-purchase).
* Ein Computer (Windows 10 oder macOS 10.10+).
* Ein aktives Azure-Abonnement. [Kostenloses Microsoft Azure-Testkonto für 30 Tage](https://azureinfo.microsoft.com/us-freetrial.html) aktivieren.
  

## <a name="prepare-your-hardware"></a>Vorbereiten der Hardware

Schließen Sie die Hardware an Ihren Computer an.

Sie benötigen diese Hardware:

* DevKit-Board
* Micro-USB-Kabel

![Erforderliche Hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

So stellen Sie eine Verbindung des DevKit mit Ihrem Computer her:

1. Stellen Sie eine Verbindung der USB-Seite mit Ihrem Computer her.
2. Stellen Sie eine Verbindung der Micro-USB-Seite mit dem DevKit her.
3. Die grüne Betriebs-LED bestätigt die Verbindung.

![Hardwareverbindungen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wi-fi"></a>Konfigurieren von WLAN

Für IoT-Projekte ist Internetkonnektivität erforderlich. Befolgen Sie die nachstehenden Anweisungen, um DevKit für die Herstellung einer WLAN-Verbindung zu konfigurieren.

### <a name="enter-ap-mode"></a>Wechseln in den AP-Modus

Halten Sie die Taste B gedrückt, drücken Sie gleichzeitig einmal die Resettaste, und lassen Sie dann Taste B los. Ihr DevKit-Board wechselt zur WLAN-Konfiguration in den AP-Modus. Auf dem Display werden die SSID (Service Set Identifier) von DevKit sowie die IP-Adresse des Konfigurationsportals angezeigt.

![Resettaste, Taste B und SSID](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a>Herstellen einer Verbindung mit DevKit im AP-Modus

Verbinden Sie nun ein anderes WLAN-fähiges Gerät (Computer oder Mobiltelefon) mit der DevKit-SSID (im obigen Bild hervorgehoben). Lassen Sie das Kennwort leer.

![Netzwerkinfo und Schaltfläche „Verbinden“](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wi-fi-for-the-devkit"></a>Konfigurieren von WLAN für das DevKit

Geben Sie in den Browser Ihres Computers oder Mobiltelefons die IP-Adresse ein, die auf dem DevKit-Display angezeigt wird. Wählen Sie das WLAN-Netzwerk aus, mit dem DevKit eine Verbindung herstellen soll, und geben Sie das Kennwort ein. Wählen Sie **Verbinden**aus.

![Kennwortfeld und Schaltfläche „Verbinden“](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Sobald die Verbindung hergestellt wird, wird DevKit innerhalb weniger Sekunden neu gestartet. Anschließend werden der WLAN-Name und die IP-Adresse auf dem Display angezeigt:

![WLAN-Name und IP-Adresse](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
> Die im Foto angezeigte IP-Adresse entspricht möglicherweise nicht der tatsächlichen IP-Adresse, die zugewiesen ist und auf dem DevKit-Display angezeigt wird. Dies ist normal, da bei WLAN das DHCP zur dynamischen Zuweisung von IP-Adressen verwendet wird.

Nach der WLAN-Konfiguration werden Ihre Anmeldeinformationen für diese Verbindung auf dem Gerät beibehalten, auch wenn es nicht angeschlossen ist. Wenn Sie die WLAN-Konfiguration für DevKit zu Hause vorgenommen und DevKit dann zu Ihrem Arbeitsplatz mitgenommen haben, müssen Sie den AP-Modus neu konfigurieren (beginnend mit Schritt „Wechseln in den AP-Modus“), um die Verbindung von DevKit mit dem WLAN Ihres Arbeitsplatzes herzustellen. 

## <a name="start-using-the-devkit"></a>Starten unter Verwendung von DevKit

Die auf DevKit ausgeführte Standard-App überprüft auf die neueste Firmwareversion und zeigt einige Sensordiagnosedaten für Sie an.

### <a name="upgrade-to-the-latest-firmware"></a>Durchführen eines Upgrades auf die neueste Firmware

> [!NOTE] 
> Seit v1.1 ermöglicht DevKit ST-SAFE im Bootloader. Bei einer Ausführung unter v1.1 müssen Sie die Firmware aktualisieren, damit sie ordnungsgemäß funktioniert.

Wenn Sie ein Firmwareupgrade benötigen, werden im Bildschirm die aktuelle und neueste Firmware-Version angezeigt. Befolgen Sie zum Durchführen eines Upgrades die Anweisungen im Leitfaden [Upgrade firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) (Upgrade der Firmware).

![Anzeigen der aktuellen und der neuesten Firmwareversion](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
> Dies ist ein einmaliger Aufwand. Wenn Sie mit der Entwicklung auf DevKit begonnen haben und Ihre App hochladen, wird die neueste Firmware für Ihre App installiert.

### <a name="test-various-sensors"></a>Testen von verschiedenen Sensoren

Drücken Sie Taste B, um die Sensoren zu testen. Drücken Sie Taste B wiederholt, um alle Sensoren zu durchlaufen.

![Taste B und Sensoranzeige](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

Nun erfolgt die Einrichtung der Entwicklungsumgebung: In dieser stehen Ihnen Tools und Pakete für die Erstellung von leistungsstarken IoT-Anwendungen zur Verfügung. Sie können die Ihrem Betriebssystem entsprechende Windows- oder macOS-Version auswählen.

### <a name="windows"></a>Windows

Es wird empfohlen, das Installationspaket zur Vorbereitung der Entwicklungsumgebung zu verwenden. Wenn Probleme auftreten, können Sie hierfür die [manuellen Schritte](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) durchführen.

#### <a name="download-the-latest-package"></a>Herunterladen des aktuellen Pakets

Die von Ihnen heruntergeladene ZIP-Datei enthält alle erforderlichen Tools und Pakete für die DevKit-Entwicklung.

> [!div class="button"]
[Download](https://aka.ms/devkit/prod/installpackage/latest)

Die ZIP-Datei enthält die folgenden Tools und Pakete. Wenn bereits einige Komponenten installiert sind, erkennt das Skript diese und überspringt sie.

* Node.js und Yarn: Runtime für das Setupskript und automatisierte Aufgaben.
* [Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows): plattformübergreifende Befehlszeilenumgebung von Azure zum Verwalten von Azure-Ressourcen. Die MSI-Datei enthält abhängige Python- und PIP-Inhalte.
* [Visual Studio Code](https://code.visualstudio.com/) (VS-Code): einfacher Code-Editor für die DevKit-Entwicklung.
* [Visual Studio Code-Erweiterung für Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Erweiterung, die Arduino-Entwicklung in Visual Studio Code ermöglicht.
* [Arduino-IDE](https://www.arduino.cc/en/Main/Software): das Tool, auf dem die Erweiterung für Arduino basiert.
* DevKit-Boardpaket: Toolketten, Bibliotheken und Projekte für DevKit.
* Hilfsprogramm ST-Link: wichtige Dienstprogramme und Treiber.

#### <a name="run-the-installation-script"></a>Ausführen des Installationsskripts

Suchen Sie im Windows-Datei-Explorer die ZIP-Datei, und extrahieren Sie sie. Suchen Sie die Datei `install.cmd`, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Administrator ausführen**.

![Datei-Explorer](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Während der Installation wird der Status jedes Tools oder Pakets angezeigt.

![Installationsstatus](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

> [!NOTE] 
> Abhängig von Ihrer Umgebung erhalten Sie manchmal bei der Installation von Arduino IDE Fehlermeldungen. Sie können in diesem Fall versuchen, [Arduino IDE individuell zu installieren](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) und „install.cmd“ erneut ausführen. Führen Sie andernfalls die [manuellen Schritte](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) aus, um alle erforderlichen Tools und Pakete zu installieren.

#### <a name="install-drivers"></a>Installieren von Treibern

Die Visual Studio Code-Erweiterung für Arduino basiert auf der Arduino-IDE. Wenn Sie die Arduino-IDE zum ersten Mal installieren, werden Sie zur Installation der relevanten Treiber aufgefordert:

![getting-started-driver](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Die Installation dauert abhängig von der Internetgeschwindigkeit ungefähr 10 Minuten. Nach Abschluss der Installation sollten die Verknüpfungen für Visual Studio Code und die Arduino-IDE auf Ihrem Desktop angezeigt werden.

> [!NOTE] 
> Gelegentlich kann es vorkommen, dass beim Starten von Visual Studio Code eine Fehlermeldung angezeigt wird, die besagt, dass die Arduino-IDE oder das jeweilige Boardpaket nicht gefunden werden kann. Um dieses Problem zu lösen, schließen Sie Visual Studio Code, und starten Sie die Arduino-IDE erneut. Visual Studio Code sollte dann den Pfad von Arduino-IDE ordnungsgemäß finden.

### <a name="macos"></a>macOS

Sie sollten die One-Click-Installationsbenutzeroberfläche zur Vorbereitung der Entwicklungsumgebung verwenden. Wenn Probleme auftreten, können Sie hierfür die [manuellen Schritte](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) durchführen.

#### <a name="install-homebrew"></a>Installieren von Homebrew

> [!NOTE] 
> Wenn Sie Homebrew installiert haben, können Sie diesen Schritt überspringen.

Befolgen Sie die [Installationsanweisungen zu Homebrew](https://docs.brew.sh/Installation.html).

#### <a name="download-the-latest-package"></a>Herunterladen des aktuellen Pakets
Die von Ihnen heruntergeladene ZIP-Datei enthält alle erforderlichen Tools und Pakete für die DevKit-Entwicklung.

> [!div class="button"]
[Download](https://aka.ms/devkit/prod/installpackage/mac/latest)

Die ZIP-Datei enthält die folgenden Tools und Pakete. Wenn bereits einige Komponenten installiert sind, erkennt das Skript diese und überspringt sie.

* Node.js und Yarn: Runtime für das Setupskript und automatisierte Aufgaben.
* [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest#a-namemacosinstall-on-macos): plattformübergreifende Befehlszeilenumgebung von Azure zum Verwalten von Azure-Ressourcen.
* [Visual Studio Code](https://code.visualstudio.com/) (VS-Code): einfacher Code-Editor für die DevKit-Entwicklung.
* [Visual Studio Code-Erweiterung für Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Erweiterung, die Arduino-Entwicklung in Visual Studio Code ermöglicht.
* [Arduino-IDE](https://www.arduino.cc/en/Main/Software): das Tool, auf dem die Erweiterung für Arduino basiert.
* DevKit-Boardpaket: Toolketten, Bibliotheken und Projekte für DevKit.
* Hilfsprogramm ST-Link: wichtige Dienstprogramme und Treiber.

#### <a name="run-the-installation-script"></a>Ausführen des Installationsskripts

Suchen Sie die ZIP-Datei, und extrahieren Sie sie:

Starten Sie die Terminal-App, suchen Sie den Ordner, in dem Sie die ZIP-Datei extrahieren möchten, und führen Sie die Extraktion aus:

```bash
./install.sh
```

> [!NOTE] 
> Wenn ein Homebrew-Berechtigungsfehler auftritt, führen Sie `brew doctor` aus, um dieses Problem zu beheben. Weitere Informationen finden Sie in den [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#homebrew-permission-error-on-macos).

Nun sind alle erforderlichen Tools und Pakete für macOS installiert.

## <a name="open-the-project-folder"></a>Öffnen des Projektordners

Sie beginnen mit dem Erstellen einer Azure IoT Hub-Instanz, stellen eine Verbindung mit DevKit her, erfassen die Temperatur- und Luftfeuchtigkeitsdaten von Sensoren und senden die Daten an IoT Hub.

### <a name="start-vs-code"></a>Starten von Visual Studio Code

Stellen Sie sicher, dass DevKit nicht verbunden ist. Starten Sie zuerst Visual Studio Code, und stellen Sie eine Verbindung von DevKit mit Ihrem Computer her. Visual Studio Code sucht das DevKit automatisch und öffnet eine Einführungsseite:

![Seite „Einführung“](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
> Gelegentlich kann es vorkommen, dass beim Starten von Visual Studio Code eine Fehlermeldung angezeigt wird, die besagt, dass die Arduino-IDE oder das jeweilige Boardpaket nicht gefunden werden kann. Schließen Sie Visual Studio Code, und starten Sie erneut die Arduino-IDE. Visual Studio Code sollte dann ordnungsgemäß zum Pfad der Arduino-IDE navigieren.


### <a name="open-the-arduino-examples-folder"></a>Öffnen des Ordners „Arduino-Beispiele“

Erweitern Sie auf der linken Seite **ARDUINO-BEISPIELE**, wechseln Sie zu **Beispiele für MXCHIP AZ3166 > AzureIoT**, und wählen Sie **GetStarted**. Dadurch wird ein neues Visual Studio Code-Fenster mit Projektordner geöffnet.

![Registerkarte „Arduino-Beispiele“](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

Falls Sie den Bereich schließen sollten, können Sie ihn erneut öffnen. Öffnen Sie mit der Tastenkombination `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) die Befehlspalette, geben Sie **Arduino** ein, suchen Sie dann nach **Arduino: Beispiele**, und wählen Sie sie aus.

## <a name="provision-azure-services"></a>Bereitstellen von Azure-Diensten

Führen Sie im Projektmappenfenster Ihre Aufgabe über `Ctrl+P` (macOS: `Cmd+P`) durch Eingabe von `task cloud-provision` aus:

Im Visual Studio Code-Terminal werden Sie über eine interaktive Befehlszeile durch die Bereitstellung der erforderlichen Azure-Dienste geführt:

![Interaktive Befehlszeile](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-the-arduino-sketch"></a>Erstellen und Hochladen des Arduino-Sketches

### <a name="install-the-required-library"></a>Installieren der erforderlichen Bibliothek

1. Drücken Sie `F1` oder `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`), um die Befehlspalette zu öffnen, geben Sie **Arduino** ein, suchen Sie dann nach **Arduino: Bibliothek-Manager**, und wählen Sie ihn aus.

2. Suchen Sie nach der **ArduinoJson**-Bibliothek, und wählen Sie **Installieren**: ![Arduino-Bibliothek installieren](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/arduino-json.png).

### <a name="build-and-upload-the-device-code-windows"></a>Erstellen und Hochladen des Gerätecodes (Windows)
1. Verwenden Sie `Ctrl+P` zum Ausführen von `task device-upload`.
2. Das Terminal fordert Sie zum Wechseln in den Konfigurationsmodus auf. Halten Sie hierfür Taste A gedrückt, und drücken Sie anschließend gleichzeitig einmal die Reset-Taste. Auf dem Bildschirm werden die DevKit-ID und „Konfiguration“ angezeigt.

Auf diesem Display wird die Verbindungszeichenfolge festgelegt, die im Schritt `task cloud-provision` abgerufen wird.

Daraufhin beginnt das Terminal mit Überprüfen und Hochladen des Arduino-Sketches:

![Überprüfen und Hochladen des Arduino-Sketches](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

Das DevKit startet neu und beginnt mit der Ausführung des Codes.

### <a name="build-and-upload-the-device-code-mac"></a>Erstellen und Hochladen des Gerätecodes (Mac)

1. Setzen Sie DevKit in den Konfigurationsmodus: Halten Sie hierfür Taste A gedrückt, und drücken Sie anschließend gleichzeitig einmal die Reset-Taste. Auf dem Bildschirm wird „Konfiguration“ angezeigt.
2. Verwenden Sie `Cmd+P` zum Ausführen von `task device-upload`.

Auf diesem Display wird die Verbindungszeichenfolge festgelegt, die im Schritt `task cloud-provision` abgerufen wird.

Daraufhin beginnt VS Code mit Überprüfen und Hochladen des Arduino-Sketches:

![device-upload](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

Das DevKit startet neu und beginnt mit der Ausführung des Codes.

## <a name="test-the-project"></a>Testen des Projekts

Öffnen Sie in Visual Studio-Code mit folgenden Schritten den seriellen Monitor, und richten Sie ihn ein:

1. Klicken Sie in der Statusleiste auf das `COM[X]`-Wort, um den rechten COM-Anschluss mit `STMicroelectronics`: ![com-port](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/com-port.png) einzustellen.

2. Klicken Sie in der Statusleiste auf das Symbol für den Netzstecker, um den seriellen Monitor zu öffnen: ![serial-monitor](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/serial-monitor.png).

3. Klicken Sie in der Statusleiste auf die Zahl, die die Baudrate darstellt, und stellen Sie `115200`: ![baud-rate](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/baud-rate.png) ein.

Die Beispielanwendung wird erfolgreich ausgeführt, wenn folgende Ergebnisse angezeigt werden:

* Der serielle Monitor zeigt dieselben Informationen an wie den unten stehenden Inhalt im folgenden Screenshot.
* Die LED auf dem MXChip IoT DevKit-Board blinkt.

![Endgültige Ausgabe in Visual Studio Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, stehen Ihnen [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) zur Verfügung. Sie können uns auch Feedback senden, indem Sie einen Kommentar auf dieser Seite hinterlassen.

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfolgreich eine Verbindung eines MXChip IoT DevKit-Boards mit IoT Hub hergestellt und die erfassten Sensordaten an IoT Hub gesendet.

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer Azure IoT-Szenarien finden Sie in den folgenden Artikeln:

- [Verwalten von Cloudgerät-Nachrichten mit iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [Speichern von IoT Hub-Nachrichten im Azure-Datenspeicher](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Power BI](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Visualisieren von Echtzeit-Sensordaten über Azure IoT Hub mithilfe des Web-Apps-Features von Azure App Service](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning) (Wettervorhersage mithilfe von Sensordaten Ihres IoT Hub in Azure Machine Learning)
- [Geräteverwaltung mit iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Remoteüberwachung und Benachrichtigungen mit Logic Apps](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)

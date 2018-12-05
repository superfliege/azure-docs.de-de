---
title: IoT DevKit in der Cloud – Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie IoT DevKit AZ3166 einrichten und eine Verbindung mit Azure IoT Hub herstellen, um Daten an die Azure-Cloudplattform zu senden.
author: rangv
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 08/27/2018
ms.author: rangv
ms.openlocfilehash: 2ece10c43f25ac637a29324f46a88e50d9655431
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620437"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Sie können das [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/)-Board zur Entwicklung von IoT-Lösungen (Internet of Things, Internet der Dinge), die Microsoft Azure-Dienste nutzen, sowie zum Erstellen eines Prototyps dieser Lösungen verwenden. Es enthält ein Arduino-kompatibles Board mit leistungsstarken Peripheriegeräten und Sensoren, ein Open Source-Boardpaket und einen wachsenden [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Aufgaben

Sie verbinden [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) mit einer Azure IoT Hub-Instanz, die Sie erstellen, erfassen die Temperatur- und Luftfeuchtigkeitsdaten von Sensoren und senden die Daten an IoT Hub.

Sie haben noch kein DevKit-Board? Probieren Sie den [DevKit-Simulator](https://azure-samples.github.io/iot-devkit-web-simulator/) aus, oder [kaufen Sie ein DevKit](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Lerninhalt

* Herstellen einer Verbindung von IoT DevKit mit einem drahtlosen Zugriffspunkt und Vorbereiten der Entwicklungsumgebung.
* Erstellen einer IoT Hub-Instanz und Registrieren eines Geräts für das MXChip IoT DevKit.
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf dem MXChip IoT DevKit-Board.
* Senden der Sensordaten an Ihren IoT Hub

## <a name="what-you-need"></a>Voraussetzungen

* Ein MXChip IoT DevKit-Board mit einem Micro-USB-Kabel. [Jetzt kaufen](https://aka.ms/iot-devkit-purchase).
* Ein Computer (Windows 10 oder macOS 10.10+).
* Ein aktives Azure-Abonnement. [Kostenloses Microsoft Azure-Testkonto für 30 Tage](https://azureinfo.microsoft.com/us-freetrial.html) aktivieren.
  
## <a name="prepare-your-hardware"></a>Vorbereiten der Hardware

Schließen Sie die folgende Hardware an Ihren Computer an:

* DevKit-Board
* Micro-USB-Kabel

![Erforderliche Hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

Führen Sie diese Schritte aus, um das DevKit mit Ihrem Computer zu verbinden:

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
> Seit v1.1 ermöglicht DevKit ST-SAFE im Bootloader. Sie müssen die Firmware aktualisieren, wenn Sie eine ältere Version als v1.1 ausführen.

Wenn Sie ein Firmwareupgrade benötigen, werden im Bildschirm die aktuelle und neueste Firmware-Version angezeigt. Befolgen Sie zum Durchführen eines Upgrades die Anweisungen im Leitfaden [Upgrade firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) (Upgrade der Firmware).

![Anzeigen der aktuellen und der neuesten Firmwareversion](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE]
> Dies ist ein einmaliger Aufwand. Wenn Sie mit der Entwicklung auf DevKit begonnen haben und Ihre App hochladen, wird die neueste Firmware für Ihre App installiert.

### <a name="test-various-sensors"></a>Testen von verschiedenen Sensoren

Drücken Sie Taste B, um die Sensoren zu testen. Drücken Sie Taste B wiederholt, um alle Sensoren zu durchlaufen.

![Taste B und Sensoranzeige](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

### <a name="install-azure-iot-workbench"></a>Installieren von Azure IoT Workbench

Wir empfehlen die Erweiterung [Azure IoT-Workbench](https://aka.ms/iot-workbench) für Visual Studio Code zum Entwickeln im DevKit.

Azure IoT Workbench bietet eine integrierte Umgebung zum Entwickeln von IoT-Lösungen. Sie bietet Unterstützung für die Entwicklung auf Geräten sowie in der Cloud mithilfe von Azure IoT und anderen Diensten. In diesem [Channel 9-Video](https://channel9.msdn.com/Shows/Internet-of-Things-Show/IoT-Workbench-extension-for-VS-Code) erhalten Sie einen Überblick über die bereitgestellten Funktionen.

Führen Sie die folgenden Schritte durch, um die Entwicklungsumgebung für DevKit vorzubereiten:

1. Laden Sie die [Arduino-IDE](https://www.arduino.cc/en/Main/Software) herunter, und installieren Sie sie. Sie stellt die erforderliche Toolkette für das Kompilieren und Hochladen von Arduino-Code bereit.
    * **Windows**: Verwenden Sie die Windows Installer-Version. Führen Sie die Installation nicht aus dem App Store durch.
    * **macOS**: Ziehen Sie die extrahierte Datei **Arduino.app** in den Ordner `/Applications`.
    * **Ubuntu**: Entpacken Sie die Datei in einem Ordner wie `$HOME/Downloads/arduino-1.8.5`.

2. Installieren Sie [Visual Studio Code](https://code.visualstudio.com/), einen plattformübergreifenden Quellcode-Editor mit Entwicklertools wie leistungsstarken Developer Tools wie IntelliSense-Codevervollständigung und Debuggen.

3. Suchen Sie im Marketplace für Erweiterungen nach **Azure IoT Workbench**, und installieren Sie die Erweiterung.
    ![Installieren von Azure IoT Workbench](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-workbench.png): Zusammen mit IoT Workbench werden weitere abhängige Erweiterungen installiert.

4. Konfigurieren von Arduino
    * **Windows**: Klicken Sie unter **Windows** auf **Datei > Voreinstellungen > Einstellungen**, klicken Sie auf **...**, und öffnen Sie die Datei „settings.json“. Fügen Sie dann die folgenden Zeilen zum Konfigurieren von Arduino ein. 
      
    ```json
    "arduino.path": "C:\\Program Files (x86)\\Arduino",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

    * **macOS**: Klicken Sie unter **macOS** auf **Code > Voreinstellungen > Einstellungen**, klicken Sie auf **...**, und öffnen Sie die Datei „settings.json“. Fügen Sie dann die folgenden Zeilen zum Konfigurieren von Arduino ein.

    ```json
    "arduino.path": "/Applications",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

    * **Ubuntu**:

    ```json
    "arduino.path": "/home/{username}/Downloads/arduino-1.8.5",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

5. Klicken Sie auf `F1`, um die Befehlspalette zu öffnen, geben Sie „Arduino“ ein, und wählen Sie **Board Manager** aus. Suchen Sie nach **AZ3166**, und installieren Sie die aktuelle Version.
    ![Installieren des DevKit-SDKs](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-sdk.png)

### <a name="install-st-link-drivers"></a>Installieren von ST-Link-Treibern

[ST-Link/V2](http://www.st.com/en/development-tools/st-link-v2.html) ist die USB-Schnittstelle, die vom IoT-DevKit für die Kommunikation mit Ihrem Entwicklungscomputer verwendet wird. Führen Sie die Schritte für das jeweilige Betriebssystem aus, um dem Computer Zugriff auf Ihr Gerät zu gewähren.

* **Windows**: Laden Sie den USB-Treiber von der [STMicroelectronics-Website](http://www.st.com/en/development-tools/stsw-link009.html) herunter, und installieren Sie ihn.
* **MacOS**: Für macOS ist kein Treiber erforderlich.
* **Ubuntu**: Führen Sie den folgenden Befehl im Terminal aus, melden Sie sich ab und wieder an, damit die Gruppenänderung wirksam wird:
    ```bash
    # Copy the default rules. This grants permission to the group 'plugdev'
    sudo cp ~/.arduino15/packages/AZ3166/tools/openocd/0.10.0/linux/contrib/60-openocd.rules /etc/udev/rules.d/
    sudo udevadm control --reload-rules

    # Add yourself to the group 'plugdev'
    # Logout and log back in for the group to take effect
    sudo usermod -a -G plugdev $(whoami)
    ```

Jetzt sind Sie mit der Vorbereitung und Konfiguration Ihrer Entwicklungsumgebung fertig. Wir erstellen das Beispiel „Hallo Welt“ für IoT: Senden von Telemetriedaten für Temperatur an Azure IoT Hub.

## <a name="build-your-first-project"></a>Erstellen Ihres ersten Projekts

1. Stellen Sie sicher, dass Ihr IoT-DevKit **nicht** mit Ihrem Computer verbunden ist. Starten Sie zuerst Visual Studio Code, und stellen Sie dann eine Verbindung von DevKit mit Ihrem Computer her.

1. Überprüfen Sie rechts unten auf der Statusleiste, ob **MXCHIP AZ3166** als ausgewähltes Board angezeigt und ein serieller Port mit **STMicroelectronics** verwendet wird.
    ![Board und COM auswählen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-board.png)

1. Klicken Sie auf `F1`, um die Befehlspalette zu öffnen, geben Sie **IoT Workbench: Beispiele** ein, und wählen Sie es aus. Wählen Sie dann **IoT DevKit** als Board aus.

1. Suchen Sie auf der Seite mit IoT Workbench-Beispielen nach **Erste Schritte**, und klicken Sie auf **Beispiel öffnen**. Wählen Sie dann den Standardpfad zum Herunterladen des Beispielcodes aus.
    ![Beispiel öffnen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

1. Wenn die Arduino-Erweiterung nicht in Visual Studio Code installiert ist, klicken Sie im Benachrichtigungsbereich auf **Installieren**.
    ![Installieren der Arduino-Erweiterung](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino-ext.png)

1. Klicken Sie im neu geöffneten Projektfenster auf `F1`, um die Befehlspalette zu öffnen, geben Sie **IoT Workbench: Cloud** ein, wählen Sie es aus, und wählen Sie dann **Azure-Bereitstellung** aus. Folgen Sie der Schritt-für-Schritt-Anleitung, um die Bereitstellung Ihrer Azure IoT Hub-Instanz und das Erstellen des Geräts fertigzustellen.
    ![Cloudbereitstellung](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/cloud-provision.png)

1. Klicken Sie auf `F1`, um die Befehlspalette zu öffnen, geben Sie **IoT Workbench: Gerät** ein, wählen Sie es aus, und wählen Sie dann **Geräteeinstellungen konfigurieren > Verbindungszeichenfolge für das Gerät konfigurieren > Verbindungszeichenfolge für das IoT Hub-Gerät auswählen** aus.

1. Halten Sie im DevKit die **Taste A** gedrückt, und lassen Sie **Rücksetztaste**  los. Lassen Sie dann die **Taste A** los. Ihr DevKit wechselt in den Konfigurationsmodus und speichert die Verbindungszeichenfolge.
    ![Verbindungszeichenfolge](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. Klicken Sie erneut auf `F1`, wählen Sie **IoT Workbench: Gerät** und dann **Geräteupload** aus.
    ![Arduino-Upload](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

Das DevKit startet neu und beginnt mit der Ausführung des Codes.

> [!NOTE]
> Wenn Fehler oder Unterbrechungen auftreten, können Sie den Befehl jederzeit erneut ausführen.

## <a name="test-the-project"></a>Testen des Projekts

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>Anzeigen der an Azure IoT Hub gesendeten Telemetriedaten

Klicken Sie auf der Statusleiste auf das Netzsteckersymbol, um den seriellen Monitor zu öffnen: ![Serieller Monitor](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

Die Beispielanwendung wird erfolgreich ausgeführt, wenn folgende Ergebnisse angezeigt werden:

* Der serielle Monitor zeigt die an IoT Hub gesendete Nachricht an.
* Die LED auf dem MXChip IoT DevKit-Board blinkt.

![Ausgabe des seriellen Monitors](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>Anzeigen der von Azure IoT Hub empfangenen Telemetriedaten

Sie können das [Azure IoT-Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) verwenden, um D2C-Nachrichten (Gerät-zu-Cloud) in IoT Hub zu überwachen.

1. Suchen Sie in Visual Studio Code im Marketplace für Erweiterungen nach **Azure IoT Toolkit** , und installieren Sie die Erweiterung.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, suchen Sie die von Ihnen erstellte IoT Hub-Instanz.
    ![Azure-Portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. Klicken Sie im Bereich **Freigegebene Zugriffsrichtlinien** auf die Richtlinie **iothubowner**, und notieren Sie die Verbindungszeichenfolge Ihrer IoT Hub-Instanz.
    ![Azure IoT Hub-Verbindungszeichenfolge](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. Erweitern Sie in Visual Studio Code unten links **AZURE IOT HUB-GERÄTE**, und klicken Sie auf **Festlegen der IoT Hub-Verbindungszeichenfolge**.
    ![Festlegen der Azure IoT Hub-Verbindungszeichenfolge](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-toolkit-conn-string.png)

1. Klicken Sie im Kontextmenü auf **IoT: Überwachung von D2C-Nachricht starten**.

1. Im Bereich **AUSGABE** sehen Sie die eingehenden D2C-Nachrichten an IoT Hub.
    ![D2C-Nachricht](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-toolkit-console.png)

## <a name="problems-and-feedback"></a>Probleme und Feedback

Falls Probleme auftreten, können Sie in den [häufig gestellten Fragen zu IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) nach einer Lösung suchen oder sich über [Gitter](https://gitter.im/Microsoft/azure-iot-developer-kit) an uns wenden. Sie können uns auch Feedback senden, indem Sie einen Kommentar auf dieser Seite hinterlassen.

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfolgreich eine Verbindung eines MXChip IoT DevKit-Boards mit IoT Hub hergestellt und die erfassten Sensordaten an IoT Hub gesendet.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]

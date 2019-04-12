---
title: IoT DevKit in der Cloud – Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie IoT DevKit AZ3166 einrichten und eine Verbindung mit Azure IoT Hub herstellen, um Daten an die Azure-Cloudplattform zu senden.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/21/2019
ms.author: wesmc
ms.openlocfilehash: 0c6189dfd02a4bdd3662f4fa50dbb812fe995884
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438481"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Sie können das [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/)-Board zur Entwicklung von IoT-Lösungen (Internet of Things, Internet der Dinge), die Microsoft Azure-Dienste nutzen, sowie zum Erstellen eines Prototyps dieser Lösungen verwenden. Es enthält ein Arduino-kompatibles Board mit leistungsstarken Peripheriegeräten und Sensoren, ein Open Source-Boardpaket und einen wachsenden [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Aufgaben

In diesem Artikel verwenden Sie [Visual Studio Code](https://code.visualstudio.com/), einen plattformübergreifenden Quellcode-Editor, zusammen mit dem Erweiterungspaket [Azure IoT Tools](https://aka.ms/azure-iot-tools).

Sie verbinden das DevKit mit einem von Ihnen erstellten Azure IoT-Hub. Sammeln Sie anschließend die Temperatur- und Feuchtigkeitsdaten von Sensoren, und senden Sie die Daten an den IoT-Hub.

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

![Festlegen des AP-Modus](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-ap-mode.gif)

### <a name="connect-to-devkit-ap"></a>Herstellen einer Verbindung mit DevKit im AP-Modus

Verbinden Sie nun ein anderes WLAN-fähiges Gerät (Computer oder Mobiltelefon) mit der DevKit-SSID (im obigen Bild hervorgehoben). Lassen Sie das Kennwort leer.

![Netzwerkinfo und Schaltfläche „Verbinden“](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wi-fi-for-the-devkit"></a>Konfigurieren von WLAN für das DevKit

Geben Sie in den Browser Ihres Computers oder Mobiltelefons die IP-Adresse ein, die auf dem DevKit-Display angezeigt wird. Wählen Sie das WLAN-Netzwerk aus, mit dem DevKit eine Verbindung herstellen soll, und geben Sie das Kennwort ein. Wählen Sie **Verbinden**aus.

![Kennwortfeld und Schaltfläche „Verbinden“](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Sobald die Verbindung hergestellt wird, wird DevKit innerhalb weniger Sekunden neu gestartet. Anschließend werden der WLAN-Name und die IP-Adresse auf dem Display angezeigt:

![WLAN-Name und IP-Adresse](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE]
> Sie benötigen ein 2,4-GHz-Netzwerk, um IoT DevKit einzusetzen. Das WLAN-Modul von IoT DevKit ist nicht mit dem 5-GHz-Netzwerk kompatibel. Weitere Informationen finden Sie in den [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#wi-fi-configuration).

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

### <a name="install-azure-iot-tools"></a>Installieren von Azure IoT-Tools

In diesem Abschnitt installieren Sie die [Arduino IDE](https://www.arduino.cc/en/Main/Software) zusammen mit [Visual Studio Code](https://code.visualstudio.com/), einem plattformübergreifenden Quellcode-Editor.

Sie werden auch das Erweiterungspaket [Azure IoT Tools](https://aka.ms/azure-iot-tools) für Visual Studio Code installieren. Wir empfehlen die Verwendung des Erweiterungspakets [Azure IoT Tools](https://aka.ms/azure-iot-tools) für Visual Studio Code zum Entwickeln von Anwendungen im DevKit. Das Erweiterungspaket Azure IoT Tools enthält die [Azure IoT Device Workbench](https://aka.ms/iot-workbench), mit der das Entwickeln und Debuggen auf verschiedenen IoT Devkit-Geräten durchgeführt werden kann. Das [Azure IoT Hub-Toolkit](https://aka.ms/iot-toolkit), das auch im Erweiterungspaket Azure IoT Tools enthalten ist, wird zur Verwaltung und Interaktion mit Azure IoT Hubs verwendet.

Einen Überblick ihrer Funktion können Sie in diesen [Channel 9](https://channel9.msdn.com/)-Videos erhalten:
* [Introduction to the new IoT Workbench extension for VS Code](https://channel9.msdn.com/Shows/Internet-of-Things-Show/IoT-Workbench-extension-for-VS-Code) (Einführung in die neue IoT Workbench-Erweiterung für VS Code)
* [What's new in the IoT Toolkit extension for VS Code](https://channel9.msdn.com/Shows/Internet-of-Things-Show/Whats-new-in-the-IoT-Toolkit-extension-for-VS-Code) (Neues in der IoT Toolkit-Erweiterung für VS Code)

Führen Sie die folgenden Schritte durch, um die Entwicklungsumgebung für DevKit vorzubereiten:

1. Installieren Sie die [Arduino-IDE](https://www.arduino.cc/en/Main/Software). Sie stellt die erforderliche Toolkette für das Kompilieren und Hochladen von Arduino-Code bereit.
    * **Windows:** Verwenden Sie die Windows Installer-Version. Führen Sie die Installation nicht aus dem App Store durch.
    * **macOS**: Ziehen Sie die extrahierte Datei **Arduino.app** in den Ordner `/Applications`.
    * **Ubuntu**: Entpacken Sie die Datei in einem Ordner wie `$HOME/Downloads/arduino-1.8.8`.

2. Installieren Sie [Visual Studio Code](https://code.visualstudio.com/), einen plattformübergreifenden Quellcode-Editor mit Entwicklertools wie leistungsstarken Developer Tools wie IntelliSense-Codevervollständigung und Debuggen.

3. Starten Sie VS Code, suchen Sie im Erweiterungs-Marketplace nach **Arduino**, und installieren Sie es. Diese Erweiterung bietet erweiterte Funktionen für die Entwicklung auf der Arduino-Plattform.
    ![Installation von Arduino](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino.png)

4. Suchen Sie im Erweiterungs-Marketplace nach **Azure IoT Tools**, und installieren Sie es.
    ![Installation von Azure IoT Tools](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-azure-iot-tools.png)

    > [!div class="nextstepaction"]
    > [Installation des Azure IoT Tools-Erweiterungspakets](vscode:extension/vsciot-vscode.azure-iot-tools)

5. Konfigurieren Sie VS Code mit Arduino-Einstellungen.

    Klicken Sie in Visual Studio Code auf **Datei > Einstellung > Einstellungen**. Klicken Sie dann auf **...** und **„Settings.JSON“ öffnen**.
    ![Installation von Azure IoT Tools](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/user-settings-arduino.png)
    
    Fügen Sie die folgenden Zeilen hinzu, um Arduino abhängig von Ihrer Plattform zu konfigurieren: 

    * **Windows:**
      
        ```json
        "arduino.path": "C:\\Program Files (x86)\\Arduino",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **macOS**:

        ```json
        "arduino.path": "/Applications",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **Ubuntu**:
    
        Ersetzen Sie den Platzhalter **{Username}** unten durch Ihren Benutzernamen.

        ```json
        "arduino.path": "/home/{username}/Downloads/arduino-1.8.8",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

6. Klicken Sie auf `F1`, um die Befehlspalette zu öffnen, geben Sie **Arduino: Board Manager** ein, und wählen Sie den Eintrag aus. Suchen Sie nach **AZ3166**, und installieren Sie die aktuelle Version.
    ![Installieren des DevKit-SDKs](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-az3166-sdk.png)

### <a name="install-st-link-drivers"></a>Installieren von ST-Link-Treibern

[ST-Link/V2](https://www.st.com/en/development-tools/st-link-v2.html) ist die USB-Schnittstelle, die vom IoT-DevKit für die Kommunikation mit Ihrem Entwicklungscomputer verwendet wird. Sie müssen sie unter Windows installieren, damit der kompilierte Gerätecode zum DevKit übertragen werden kann. Führen Sie die Schritte für das jeweilige Betriebssystem aus, um dem Computer Zugriff auf Ihr Gerät zu gewähren.

* **Windows:** Laden Sie den USB-Treiber von der [STMicroelectronics-Website](https://www.st.com/en/development-tools/stsw-link009.html) herunter, und installieren Sie ihn für die [direkte Verknüpfung](https://aka.ms/stlink-v2-windows).
* **macOS**: Für macOS ist kein Treiber erforderlich.
* **Ubuntu**: Führen Sie die Befehle im Terminal aus. Melden Sie sich ab und wieder an, damit die Gruppenänderung wirksam wird:
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

### <a name="open-sample-code-from-sample-gallery"></a>Öffnen von Beispielcode aus dem Beispielkatalog

1. Stellen Sie sicher, dass Ihr IoT-DevKit **nicht** mit Ihrem Computer verbunden ist. Starten Sie zuerst Visual Studio Code, und stellen Sie dann eine Verbindung von DevKit mit Ihrem Computer her.

1. Drücken Sie `F1`, um die Befehlspalette zu öffnen, und geben Sie **Azure IoT Device Workbench: Open Examples...** (Azure IoT Device Workbench: Beispiele öffnen) ein. Wählen Sie dann **IoT DevKit** als Board aus.

1. Suchen Sie auf der Seite mit IoT Workbench-Beispielen nach **Erste Schritte**, und klicken Sie auf **Beispiel öffnen**. Wählen Sie dann den Standardpfad zum Herunterladen des Beispielcodes aus.
    ![Beispiel öffnen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

### <a name="provision-azure-iot-hub-and-device"></a>Bereitstellen von Azure IoT Hub und dem Gerät

1. Klicken Sie im neu geöffneten Projektfenster auf `F1`, um die Befehlspalette zu öffnen, geben Sie **Azure IoT Workbench: Provision Azure Services...** (Azure IoT Workbench: Azure-Dienste bereitstellen) ein, und wählen Sie den Eintrag aus. Befolgen Sie die schrittweise Anleitung, um die Bereitstellung Ihres Azure IoT Hubs abzuschließen und das IoT Hub-Gerät zu erstellen.
    ![Bereitstellungsbefehl](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision.png)

    > [!NOTE]
    > Wenn Sie sich nicht bei Azure angemeldet sind. Befolgen Sie die Popupbenachrichtigung zur Anmeldung.

1. Wählen Sie das Abonnement aus, das Sie verwenden möchten.
    ![Abonnement auswählen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-subscription.png)

1. Wählen oder erstellen Sie dann eine neue [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#terminology).
    ![Ressourcengruppe auswählen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-resource-group.png)

1. Folgen Sie in der von Ihnen angegebenen Ressourcengruppe der Anleitung, um einen neuen Azure IoT Hub auszuwählen oder zu erstellen.
    ![IoT Hub-Schritte auswählen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provision.png)

    ![IoT Hub auswählen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-hub.png)

    ![Ausgewählter IoT Hub](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-selected.png)

1. Im Ausgabefenster wird der bereitgestellte Azure IoT Hub ![Bereitgestellter IoT Hub](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provisioned.png) angezeigt.

1. Wählen oder erstellen Sie ein neues Gerät im bereitgestellten Azure IoT Hub.
    ![IoT-Geräteschritte auswählen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-device-provision.png)

    ![Bereitgestelltes IoT-Gerät auswählen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-device.png)

1. Jetzt haben Sie Azure IoT Hub bereitgestellt und das Gerät darin erstellt. Außerdem wird die Geräteverbindungszeichenfolge in Visual Studio Code zur späteren Konfiguration des IoT DevKit gespeichert.
    ![Bereitstellung abgeschlossen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision-done.png)

### <a name="configure-and-compile-device-code"></a>Konfigurieren und Kompilieren des Gerätecodes

1. Überprüfen Sie rechts unten auf der Statusleiste, ob **MXCHIP AZ3166** als ausgewähltes Board angezeigt und ein serieller Port mit **STMicroelectronics** verwendet wird.
    ![Board und COM auswählen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-com.png)

1. Drücken Sie `F1`, um die Befehlspalette zu öffnen, und geben Sie **Azure IoT Device Workbench: Configure Device Settings...** (Azure IoT Device Workbench: Geräteeinstellungen konfigurieren) ein, wählen Sie den Eintrag aus, und wählen Sie dann **Config Device Connection String > Select IoT Hub Device Connection String** (Geräte-Verbindungszeichenfolge konfigurieren > IoT Hub-Geräte-Verbindungszeichenfolge auswählen) aus.

1. Halten Sie im DevKit die **Taste A** gedrückt, und lassen Sie **Rücksetztaste**  los. Lassen Sie dann die **Taste A** los. Ihr DevKit wechselt in den Konfigurationsmodus und speichert die Verbindungszeichenfolge.
    ![Verbindungszeichenfolge](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. Klicken Sie erneut auf `F1`, geben Sie **Azure IoT Device Workbench: Upload Device Code** (Azure IoT Device Workbench: Gerätecode hochladen) ein, und wählen Sie den angezeigten Befehl aus. Daraufhin wird der Code kompiliert und auf das DevKit hochgeladen.
    ![Arduino-Upload](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

Das DevKit startet neu und beginnt mit der Ausführung des Codes.

> [!NOTE]
> Wenn Fehler oder Unterbrechungen auftreten, können Sie den Befehl jederzeit erneut ausführen.

## <a name="test-the-project"></a>Testen des Projekts

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>Anzeigen der an Azure IoT Hub gesendeten Telemetriedaten

Klicken Sie in der Statusleiste auf das Symbol für den Netzstecker, um den seriellen Monitor zu öffnen: ![Serieller Monitor](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

Die Beispielanwendung wird erfolgreich ausgeführt, wenn folgende Ergebnisse angezeigt werden:

* Der serielle Monitor zeigt die an IoT Hub gesendete Nachricht an.
* Die LED auf dem MXChip IoT DevKit-Board blinkt.

![Ausgabe des seriellen Monitors](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>Anzeigen der von Azure IoT Hub empfangenen Telemetriedaten

Sie können [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) verwenden, um D2C-Nachrichten (Gerät-zu-Cloud) in IoT Hub zu überwachen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, suchen Sie die von Ihnen erstellte IoT Hub-Instanz.
    ![Azure-Portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. Klicken Sie im Bereich **Freigegebene Zugriffsrichtlinien** auf die Richtlinie **iothubowner**, und notieren Sie die Verbindungszeichenfolge Ihrer IoT Hub-Instanz.
    ![Azure IoT Hub-Verbindungszeichenfolge](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. Klicken Sie in VS Code auf `F1`, geben Sie **Azure IoT Device Workbench: Set IoT Hub Connection String** (Azure IoT Device Workbench: IoT Hub-Verbindungszeichenfolge festlegen) ein, und wählen Sie den Befehl aus. Kopieren Sie die Verbindungszeichenfolge hinein.
    ![Festlegen der Azure IoT Hub-Verbindungszeichenfolge](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-iothub-connection-string.png)

1. Erweitern Sie den Bereich **AZURE IOT HUB DEVICES** (AZURE IOT HUB-GERÄTE) auf der rechten Seite, klicken Sie mit der rechten Maustaste auf den Namen des von Ihnen erstellten Geräts, und wählen Sie **Start Monitoring D2C Message** (Überwachung von D2C-Nachrichten starten) aus.
    ![D2C-Nachrichten überwachen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/monitor-d2c.png)

1. Im Bereich **AUSGABE** sehen Sie die eingehenden D2C-Nachrichten an IoT Hub.
    ![D2C-Nachricht](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/d2c-output.png)

## <a name="review-the-code"></a>Überprüfen des Codes

Die `GetStarted.ino` ist die Arduino-Hauptskizzendatei.

![D2C-Nachricht](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/code.png)

Um zu prüfen, wie die Gerätetelemetriedaten an den Azure IoT Hub gesendet werden, öffnen Sie die Datei `utility.cpp` in demselben Ordner. Unter [API-Referenz](https://microsoft.github.io/azure-iot-developer-kit/docs/apis/arduino-language-reference/) erfahren Sie, wie Sie Sensoren und Peripheriegeräte mit IoT DevKit verwenden.

Der verwendete `DevKitMQTTClient` ist ein Wrapper des **iothub_client** aus den [Microsoft Azure IoT-SDKs und -Bibliotheken für C](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client) zur Interaktion mit dem Azure IoT Hub.

## <a name="problems-and-feedback"></a>Probleme und Feedback

Falls Probleme auftreten, können Sie in den [häufig gestellten Fragen zu IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) nach einer Lösung suchen oder sich über [Gitter](https://gitter.im/Microsoft/azure-iot-developer-kit) an uns wenden. Sie können uns auch Feedback senden, indem Sie einen Kommentar auf dieser Seite hinterlassen.

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfolgreich eine Verbindung eines MXChip IoT DevKit-Boards mit IoT Hub hergestellt und die erfassten Sensordaten an IoT Hub gesendet.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]

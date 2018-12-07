---
title: Durchführen der Massenverwaltung von Geräten mit Remoteüberwachungsverbindung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie die Massenverwaltung für Geräte durchführen, die mit einer Remoteüberwachungslösung verbunden sind.
author: aditidugar
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: adugar
ms.openlocfilehash: 640eb8800f9593aef510d99713595bdd0c844263
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52683428"
---
# <a name="tutorial-manage-your-connected-devices-in-bulk"></a>Tutorial: Durchführen der Massenverwaltung für Ihre verbundenen Geräte

In diesem Tutorial verwenden Sie den Solution Accelerator für die Remoteüberwachung, um für die Konfiguration Ihrer verbundenen Geräte die Massenverwaltung durchzuführen.

Als Bediener bei Contoso müssen Sie eine Gruppe mit Geräten mit einer neuen Firmwareversion konfigurieren. Sie möchten vermeiden, dass Sie die Firmware für jedes Gerät einzeln aktualisieren müssen. Zum Aktualisieren der Firmware für mehrere Geräte können Sie Gerätegruppen und die automatische Geräteverwaltung im Solution Accelerator für die Remoteüberwachung verwenden. Alle Geräte, die Sie der Gerätegruppe hinzufügen, erhalten die aktuelle Firmware, sobald das Gerät in den Onlinezustand versetzt wird.

In diesem Tutorial führen Sie Folgendes durch:

>[!div class="checklist"]
> * Erstellen einer Gerätegruppe
> * Vorbereiten und Hosten der Firmware
> * Erstellen einer Gerätekonfiguration im Azure-Portal
> * Importieren einer Gerätekonfiguration in Ihre Remoteüberwachungslösung
> * Bereitstellen der Konfiguration auf den Geräten der Gerätegruppe
> * Überwachen der Bereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

<!--
If this is going to be a tutorial - we need to split this include into two so that we can accommodate the additional prerequisites:

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]
-->

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz des Solution Accelerators für die Remoteüberwachung in Ihrem Azure-Abonnement.

Falls Sie den Solution Accelerator für die Remoteüberwachung noch nicht bereitgestellt haben, sollten Sie den Schnellstart [Bereitstellen einer cloudbasierten Lösung für die Remoteüberwachung](quickstart-remote-monitoring-deploy.md) durcharbeiten.

Sie benötigen ein Azure-Speicherkonto, um Ihre Firmwaredateien zu hosten. Sie können ein vorhandenes Speicherkonto verwenden oder unter Ihrem Abonnement ein [neues Speicherkonto erstellen](../storage/common/storage-quickstart-create-account.md).

Im Tutorial wird ein [IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/)-Gerät als Beispielgerät verwendet.

Auf Ihrem lokalen Computer muss die folgende Software installiert sein:

* [Visual Studio Code (VS Code)](https://code.visualstudio.com/)
* VS Code-Erweiterung [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench)

Vorbereitung:

* Stellen Sie sicher, dass das [Startladeprogramm auf Ihrem IoT DevKit-Gerät Version 1.4.0 oder höher](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) aufweist.
* Vergewissern Sie sich, dass die Version des IoT DevKit SDK der Version des Startladeprogramms entspricht. Sie können das IoT DevKit SDK mit Azure IoT Workbench in VS Code aktualisieren. Öffnen Sie die Befehlspalette, und geben Sie **Arduino: Board Manager** ein. Weitere Informationen finden Sie unter [Vorbereiten der Entwicklungsumgebung](../iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started.md#prepare-the-development-environment).

Außerdem müssen Sie für mindestens ein IoT DevKit-Gerät eine Verbindung mit Ihrem Solution Accelerator für die Remoteüberwachung herstellen. Wenn Sie keine Verbindung mit einem IoT DevKit-Gerät hergestellt haben, helfen Ihnen die Informationen unter [Verbinden von MXChip IoT DevKit AZ3166 mit dem Solution Accelerator für die IoT-Remoteüberwachung](iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2.md) weiter.

## <a name="navigate-to-the-dashboard"></a>Navigieren Sie zum Dashboard.

Wenn Sie das Dashboard der Remoteüberwachungslösung im Browser anzeigen möchten, navigieren Sie zunächst zu [Microsoft Azure IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators#dashboard). Sie werden unter Umständen aufgefordert, sich mit den Anmeldeinformationen Ihres Azure-Abonnements anzumelden.

Klicken Sie dann auf der Kachel für den im [Schnellstart](quickstart-remote-monitoring-deploy.md) bereitgestellten Solution Accelerator für Remoteüberwachung auf **Starten**.

## <a name="create-a-device-group"></a>Erstellen einer Gerätegruppe

Die Geräte müssen Mitglieder einer Gerätegruppe Ihrer Remoteüberwachungslösung sein, damit die Firmware für eine Gruppe mit Geräten automatisch aktualisiert werden kann:

1. Wählen Sie auf der Seite **Geräte** alle **IoT DevKit**-Geräte aus, für die Sie eine Verbindung mit dem Solution Accelerator hergestellt haben. Klicken Sie anschließend auf **Aufträge**.

1. Wählen Sie unter **Aufträge** die Option **Tags**, legen Sie den Auftragsnamen auf **AddDevKitTag** fest, und fügen Sie dann ein Texttag mit dem Namen **IsDevKitDevice** und dem Wert **Y** hinzu. Klicken Sie anschließend auf **Übernehmen**.

1. Sie können jetzt die Tagwerte zum Erstellen einer Gerätegruppe verwenden. Klicken Sie auf der Seite **Geräte** auf **Manage device groups** (Gerätegruppen verwalten).

1. Erstellen Sie einen Textfilter, für den in der Bedingung der Tagname **IsDevKitDevice** und der Wert **Y** verwendet werden. Speichern Sie die Gerätegruppe als **IoT DevKit-Geräte**.

Später in diesem Tutorial verwenden Sie diese Gerätegruppe, um eine Gerätekonfiguration anzuwenden, mit der die Firmware aller Mitglieder aktualisiert wird.

## <a name="prepare-and-host-the-firmware"></a>Vorbereiten und Hosten der Firmware

Die VS Code-Erweiterung [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) enthält den Beispielgerätecode für das Firmwareupdate.

### <a name="open-the-firmware-ota-sample-in-vs-code"></a>Öffnen des Beispiels „Firmware OTA“ in VS Code

1. Stellen Sie sicher, dass Ihr IoT DevKit nicht mit Ihrem Computer verbunden ist. Starten Sie zuerst VS Code, und stellen Sie für das DevKit dann eine Verbindung mit Ihrem Computer her.

1. Drücken Sie **F1**, um die Befehlspalette zu öffnen, geben Sie **IoT Workbench: Beispiele** ein, und wählen Sie den Eintrag aus. Wählen Sie anschließend **IoT DevKit** als Board aus.

1. Suchen Sie nach **Firmware OTA**, und klicken Sie auf **Beispiel öffnen**. Ein neues VS Code-Fenster wird geöffnet, und der Projektordner **firmware_ota** wird angezeigt:

    ![IoT Workbench, Auswählen des Beispiels „Firmware OTA“](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-firmware-example.png)

### <a name="build-the-new-firmware"></a>Erstellen der neuen Firmware

Die erste Version der Gerätefirmware ist 1.0.0. Die neue Firmware sollte eine höhere Versionsnummer aufweisen.

1. Öffnen Sie in VS Code die Datei **FirmwareOTA.ino**, und ändern Sie `currentFirmwareVersion` von `1.0.0` in `1.0.1`:

    ![Ändern der Firmwareversion](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Öffnen Sie die Befehlspalette, geben Sie **IoT Workbench: Gerät** ein, und wählen Sie den Eintrag aus. Wählen Sie anschließend die Option **Device Compile** (Gerät kompilieren), um den Code zu kompilieren:

    ![Gerät kompilieren](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-compile.png)

    VS Code speichert die kompilierte Datei im Ordner `.build` des Projekts. Je nach Ihren Einstellungen blendet VS Code den Ordner `.build` in der Explorer-Ansicht aus.

### <a name="generate-the-crc-value-and-calculate-the-firmware-file-size"></a>Generieren des CRC-Werts und Berechnen der Größe der Firmwaredatei

1. Öffnen Sie die Befehlspalette, geben Sie **IoT Workbench: Gerät** ein, und wählen Sie den Eintrag aus. Wählen Sie anschließend die Option **Generate CRC** (CRC generieren):

    ![CRC generieren](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-crc.png)

1. VS Code führt die Generierung und Ausgabe für den CRC-Wert, den Dateinamen und Pfad der Firmware und die Dateigröße im Ausgabefenster durch. Notieren Sie sich diese Werte zur späteren Verwendung:

    ![CRC-Informationen](media/iot-accelerators-remote-monitoring-bulk-configuration-update/crc-info.png)

### <a name="upload-the-firmware-to-the-cloud"></a>Hochladen der Firmware in die Cloud

Verwenden Sie Ihr Azure-Speicherkonto zum Hosten Ihrer neuen Firmwaredatei in der Cloud.

1. Navigieren Sie zum Speicherkonto im Azure-Portal. Wählen Sie im Abschnitt „Dienste“ die Option **Blobs**. Erstellen Sie einen öffentlichen Container mit dem Namen **firmware**, in dem Ihre Firmwaredateien gespeichert werden sollen:

    ![Erstellen eines Ordners](media/iot-accelerators-remote-monitoring-bulk-configuration-update/blob-folder.png)

1. Wählen Sie zum Hochladen Ihrer Firmware in den Container den Container **firmware** aus, und klicken Sie auf **Hochladen**.

1. Wählen Sie die Datei **FirmwareOTA.ino.bin** aus. Im vorherigen Abschnitt haben Sie sich den vollständigen Pfad dieser Datei notiert.

1. Notieren Sie sich die Datei-URL, nachdem der Upload der Firmwaredatei abgeschlossen ist.

### <a name="build-and-upload-the-original-firmware-to-the-iot-devkit-device"></a>Erstellen und Hochladen der ursprünglichen Firmware auf das IoT DevKit-Gerät

1. Öffnen Sie in VS Code die Datei **FirmwareOTA.ino**, und ändern Sie `currentFirmwareVersion` zurück in `1.0.0`:

    ![Version 1.0.0](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Öffnen Sie die Befehlspalette, geben Sie **IoT Workbench: Gerät** ein, und wählen Sie den Eintrag aus. Wählen Sie dann die Option **Device Upload** (Gerät hochladen):

    ![Gerät hochladen](media/iot-accelerators-remote-monitoring-bulk-configuration-update/device-upload.png)

1. VS Code überprüft den Code und lädt ihn auf Ihr IoT DevKit-Gerät hoch.

1. Nachdem der Upload abgeschlossen ist, wird das IoT DevKit-Gerät neu gestartet. Nach Abschluss des Neustarts wird auf dem IoT DevKit-Bildschirm **FW version: 1.0.0** und ein Hinweis angezeigt, dass nach neuer Firmware gesucht wird:

    ![ota-1](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-1.jpg)

## <a name="create-a-device-configuration"></a>Erstellen einer Gerätekonfiguration

Mit einer Gerätekonfiguration wird der gewünschte Zustand Ihrer Geräte angegeben. Normalerweise führt ein Entwickler die [Erstellung der Konfiguration](../iot-hub/iot-hub-auto-device-config.md#create-a-configuration) im Azure-Portal auf der Seite **IoT-Gerätekonfiguration** durch. Eine Gerätekonfiguration ist ein JSON-Dokument, mit dem der gewünschte Zustand Ihrer Geräte sowie ein Satz mit Metriken angegeben wird.

Speichern Sie die folgende Konfiguration als Datei mit dem Namen **firmware-update.json** auf Ihrem lokalen Computer. Ersetzen Sie die Platzhalter `YOURSTRORAGEACCOUNTNAME`, `YOURCHECKSUM` und `YOURPACKAGESIZE` durch die Werte, die Sie sich zuvor notiert haben:

```json
{
  "id": "firmware-update",
  "schemaVersion": null,
  "labels": {
    "Version": "1.0.1",
    "Devices": "IoT DevKit"
  },
  "content": {
    "deviceContent": {
      "properties.desired.firmware": {
        "fwVersion": "1.0.1",
        "fwPackageURI": "https://YOURSTRORAGEACCOUNTNAME.blob.core.windows.net/firmware/FirmwareOTA.ino.bin",
        "fwPackageCheckValue": "YOURCHECKSUM",
        "fwSize": YOURPACKAGESIZE
      }
    }
  },
  "targetCondition": "",
  "priority": 10,
  "systemMetrics": {
    "results": {
      "targetedCount": 0,
      "appliedCount": 0
    },
    "queries": {
      "targetedCount": "",
      "appliedCount": "select deviceId from devices where configurations.[[firmware-update]].status = 'Applied'"
    }
  },
  "metrics": {
    "results": {
      "Current": 1
    },
    "queries": {
      "Current": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Current' AND properties.reported.firmware.type='IoTDevKit'",
      "Downloading": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Downloading' AND properties.reported.firmware.type='IoTDevKit'",
      "Verifying": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Verifying' AND properties.reported.firmware.type='IoTDevKit'",
      "Applying": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Applying' AND properties.reported.firmware.type='IoTDevKit'",
      "Error": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Error' AND properties.reported.firmware.type='IoTDevKit'"
    }
  }
}
```

Sie verwenden diese Konfigurationsdatei im folgenden Abschnitt.

## <a name="import-a-configuration"></a>Importieren einer Konfiguration

In diesem Abschnitt importieren Sie die Gerätekonfiguration als Paket in den Solution Accelerator für die Remoteüberwachung. Diese Aufgabe wird in der Regel von einem Bediener durchgeführt.

1. Navigieren Sie in der Webbenutzeroberfläche für die Remoteüberwachung zur Seite **Pakete**, und klicken Sie auf **+ Neues Paket**:

    ![Neues Paket](media/iot-accelerators-remote-monitoring-bulk-configuration-update/packagepage.png)

1. Wählen Sie im Bereich **Neues Paket** die Option **Gerätekonfiguration** als Pakettyp und **Firmware** als Konfigurationstyp. Klicken Sie auf **Durchsuchen**, um auf Ihrem lokalen Computer nach der Datei **firmware-update.json** zu suchen, und klicken Sie anschließend auf **Hochladen**:

    ![Paket hochladen](media/iot-accelerators-remote-monitoring-bulk-configuration-update/uploadpackage.png)

1. Die Liste mit den Paketen enthält jetzt das Paket **firmware-update**.

## <a name="deploy-the-configuration-to-your-devices"></a>Bereitstellen der Konfiguration für Ihre Geräte

In diesem Abschnitt erstellen Sie eine Bereitstellung, bei der die Gerätekonfiguration auf Ihre IoT DevKit-Geräte angewendet wird, und führen sie aus.

1. Navigieren Sie in der Webbenutzeroberfläche für die Remoteüberwachung zur Seite **Bereitstellungen**, und klicken Sie auf **+ Neue Bereitstellung**:

    ![Neue Bereitstellung](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentpage.png)

1. Erstellen Sie im Bereich **Neue Bereitstellung** eine Bereitstellung mit den folgenden Einstellungen:

    |Option|Wert|
    |---|---|
    |Name|Bereitstellen des Firmwareupdates|
    |Pakettyp|Gerätekonfiguration|
    |Konfigurationstyp|Firmware|
    |Paket|firmware-update.json|
    |Gerätegruppe|IoT DevKit-Geräte|
    |Priorität|10|

    ![Bereitstellung erstellen](media/iot-accelerators-remote-monitoring-bulk-configuration-update/newdeployment.png)

    Klicken Sie auf **Anwenden**. Auf der Seite **Bereitstellungen** wird eine neue Bereitstellung mit den folgenden Metriken angezeigt:

    * **Ziel** zeigt die Anzahl von Geräten in der Gerätegruppe.
    * Unter **Angewendet** wird die Anzahl von Geräten angezeigt, die mit dem Inhalt der Konfiguration aktualisiert wurden.
    * Unter **Erfolgreich** wird die Anzahl von Geräten der Bereitstellung angezeigt, für die „Erfolgreich“ gemeldet wurde.
    * Unter **Fehler** wird die Anzahl von Geräten der Bereitstellung angezeigt, für die „Fehler“ gemeldet wurde.

## <a name="monitor-the-deployment"></a>Überwachen der Bereitstellung

Nach einigen Minuten ruft das IoT DevKit die neuen Firmwareinformationen ab und beginnt mit dem Herunterladen auf das Gerät:

![ota-2](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-2.jpg)

Je nach Geschwindigkeit Ihres Netzwerks kann der Download bis zu zwei Minuten dauern. Nachdem die Firmware heruntergeladen wurde, überprüft das Gerät die Dateigröße und den CRC-Wert. Auf dem MXChip-Bildschirm wird **Passed** (Erfolgreich) angezeigt, wenn die Überprüfung erfolgreich ist.

![ota-3](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-3.jpg)

Wenn die Überprüfung erfolgreich ist, wird das Gerät neu gestartet. Vor Beginn des Neustarts wird von **5** bis **0** heruntergezählt.

![ota-4](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-4.jpg)

Nach dem Neustart aktualisiert das IoT DevKit-Startladeprogramm die Firmware auf die neue Version. Das Upgrade kann einige Sekunden dauern. Während dieser Phase leuchtet die RGB-LED des Geräts rot, und der Bildschirm ist leer.

![ota-5](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-5.jpg)

Nach Abschluss des Neustarts wird auf Ihrem IoT DevKit-Gerät Version 1.0.1 der Firmware ausgeführt.

![ota-6](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-6.jpg)

Klicken Sie auf der Seite **Bereitstellungen** auf eine Bereitstellung, um den Status Ihrer Geräte während des Updatevorgangs zu verfolgen. Sie können den Status für jedes Gerät Ihrer Gerätegruppe und die benutzerdefinierten Metriken sehen.

![Bereitstellungsdetails](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentstatus.png)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde veranschaulicht, wie Sie die Firmware für eine Gruppe mit Geräten aktualisieren, die mit Ihrer Lösung verbunden sind. Für die Aktualisierung der Geräte nutzt Ihre Lösung die automatische Geräteverwaltung. Weitere Informationen zur automatischen Geräteverwaltung im zugrunde liegenden IoT-Hub Ihrer Lösung finden Sie unter [Bedarfsgerechtes Konfigurieren und Überwachen von IoT-Geräten mit dem Azure-Portal](../iot-hub/iot-hub-auto-device-config.md).
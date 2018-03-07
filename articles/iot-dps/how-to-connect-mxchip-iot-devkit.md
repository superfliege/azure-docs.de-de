---
title: Verwenden des MXChip IoT DevKit zum Herstellen einer Verbindung mit dem Azure IoT Hub Device Provisioning-Dienst | Microsoft Docs
description: Verwenden des MXChip IoT DevKit zum Herstellen einer Verbindung mit dem Azure IoT Hub Device Provisioning-Dienst
services: iot-dps
keywords: 
author: liydu
ms.author: liydu
ms.date: 02/20/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 77c8a6a5e384d27dca2582cc0fedc2bd23c0592e
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-hub-device-provisioning-service"></a>Verbinden des MXChip IoT DevKit mit dem Azure IoT Hub Device Provisioning-Dienst

In diesem Artikel wird beschrieben, wie Sie das DevKit für die automatische Registrierung für IoT Hub mit dem Device Provisioning-Dienst konfigurieren. In diesem Lernprogramm lernen Sie Folgendes:

* Konfigurieren des globalen DPS-Endpunkts auf dem Gerät
* Verwenden eines eindeutigen geheimen Geräteschlüssels (Unique Device Secret, UDS) zum Generieren eines X.509-Zertifikats
* Registrieren eines einzelnen Geräts
* Sicherstellen, dass das Gerät registriert ist

Das [MXChip IoT DevKit](https://aka.ms/iot-devkit) ist ein mit Arduino kompatibles All-in-One-Board mit umfangreichen Peripheriegeräten und Sensoren. Sie können dafür mithilfe der [Visual Studio Code-Erweiterung für Arduino](https://aka.ms/arduino) entwickeln. Im Lieferumfang enthalten ist ein wachsender [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), der Sie durch das Erstellen eines Prototyps von Lösungen für das Internet der Dinge (Internet of Things, IoT) führt, die Microsoft Azure-Dienste nutzen.

## <a name="before-you-begin"></a>Voraussetzungen

Zum Durchführen der Schritte in diesem Tutorial benötigen Sie Folgendes:

* Bereiten Sie Ihr DevKit mithilfe des [Leitfadens zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) vor.
* Führen Sie anhand des Tutorials [Aktualisieren von Firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) ein Upgrade auf die aktuelle Firmware (>= 1.3.0) aus.
* Erstellen und verknüpfen Sie IoT Hub mit der Instanz des Device Provisioning-Diensts anhand der Beschreibung unter [Einrichten der automatischen Bereitstellung](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision).

## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Einrichten der Device Provisioning Service-Dienstkonfiguration auf dem Gerät

Gehen Sie wie folgt vor, damit das DevKit eine Verbindung mit der von Ihnen erstellten Instanz des Device Provisioning-Diensts herstellen kann:

1. Wählen Sie im Azure-Portal die **Übersicht** Ihres Device Provisioning-Diensts aus, und notieren Sie sich die Werte von **Globaler Geräteendpunkt** und **ID-Bereich**.
  ![Globaler Endpunkt und ID-Bereich von DPS](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Vergewissern Sie sich, dass `git` auf Ihrem Computer installiert ist und den Umgebungsvariablen hinzugefügt wurde, auf die das Befehlsfenster Zugriff hat. Navigieren Sie zu [Software Freedom Conservancy's Git-Clienttools](https://git-scm.com/download/), und prüfen Sie, ob die aktuelle Version installiert ist.

3. Öffnen Sie eine Eingabeaufforderung. Klonen Sie das GitHub-Repository für den DPS-Beispielcode:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Starten Sie Visual Studio Code, verbinden Sie das DevKit mit dem Computer, und öffnen Sie dann den Ordner, der den von Ihnen geklonten Code enthält.

5. Öffnen Sie **DevKitDPS.ino**, suchen Sie `[Global Device Endpoint]` und `[ID Scope]`, und ersetzen Sie die Werte durch die Werte, die Sie sich zuvor notiert hatten.
  ![DPS-Endpunkt](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) Sie können die Registrierungs-ID (**registrationId**) leer lassen. Die ID wird von der Anwendung basierend auf der MAC-Adresse und der Firmware-Version generiert. Wenn Sie diese anpassen möchten, beachten Sie, dass die Registrierungs-ID nur aus alphanumerischen Kleinbuchstaben- und Bindestrichkombinationen bestehen und maximal 128 Zeichen umfassen darf. Weitere Informationen finden Sie unter [Verwalten von Geräteregistrierungen mit dem Azure-Portal](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments).

6. Verwenden Sie **Quick Open** in Visual Studio Code (Windows: `Ctrl+P`, MacOS: `Cmd+P`), und geben Sie **task device-upload** ein, um den Code zu erstellen und in das DevKit hochzuladen.

7. Beobachten Sie die erfolgreiche Ausführung der Aufgabe im Ausgabefenster.

## <a name="save-unique-device-secret-on-stsafe-security-chip"></a>Speichern des eindeutigen Geräteschlüssels (Unique Device Secret, UDS) auf dem STSAFE-Sicherheitschip

Der Device Provisioning-Dienst kann basierend auf dem zugehörigen [Hardwaresicherheitsmodul (HSM)](https://azure.microsoft.com/en-us/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) auf dem Gerät konfiguriert werden. DevKit verwendet [Device Identity Composition Engine (DICE)](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) der [Trusted Computing Group (TCG)](https://trustedcomputinggroup.org). Ein **eindeutiger geheimer Geräteschlüssel (Unique Device Secret, UDS)**, der auf dem STSAFE-Sicherheitschip im DevKit gespeichert ist, wird zum Generieren des eindeutigen [x. 509](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#select-a-hardware-security-module)-Zertifikats für das Gerät verwendet. Das Zertifikat kann später für den Registrierungsprozess im Device Provisioning-Dienst verwendet werden.

Ein typischer **eindeutiger geheimer Geräteschlüssel (Unique Device Secret, UDS)** ist eine aus 64 Zeichen bestehende Zeichenfolge. Nachfolgend sehen Sie einen Beispiel-UDS:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Jedes der beiden Zeichen wird als Hexadezimalwert in der Sicherheitsberechnung verwendet. Daher wird der obige Beispiel-UDS wie folgt aufgelöst: „`0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16`, `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`“.

Gehen Sie wie folgt vor, um den eindeutigen geheimen Geräteschlüssel (Unique Device Secret, UDS) im DevKit zu speichern:

1. Öffnen Sie den seriellen Monitor mit einem Tool wie Putty. Ausführliche Informationen finden Sie unter [Verwenden des Konfigurationsmodus](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/).

2. Wenn das DevKit mit dem Computer verbunden ist, halten Sie Taste A gedrückt, und drücken Sie dann die Reset-Taste, um in den Konfigurationsmodus zu wechseln. Auf dem Bildschirm werden die DevKit-ID und **„Konfiguration“** angezeigt.

3. Verwenden Sie die oben aufgeführt lange UDS-Beispielzeichenfolge, und ändern Sie ein oder mehrere Zeichen in andere Werte zwischen `0` und `f`. Diese Zeichenfolge wird als Ihr eigener UDS verwendet.

4. Geben Sie im Fenster des seriellen Monitors den Befehl **set_dps_uds [Ihr_eigener_UDS-Wert]** ein, und drücken Sie zum Speichern die EINGABETASTE.
  > [!NOTE]
  > Wenn Sie beispielsweise Ihren eigenen UDS durch Ändern der letzten beiden Zeichen in `f` festgelegt haben, müssen Sie den Befehl **set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff** eingeben.

5. Drücken Sie auf dem DevKit die Reset-Taste, ohne das Fenster des seriellen Monitors zu schließen.

6. Notieren Sie sich die Werte von **DevKit-MAC-Adresse** und **DevKit-Firmwareversion**.
  ![Firmwareversion](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-x509-certificate"></a>Generieren eines X.509-Zertifikats

### <a name="windows"></a>Windows

1. Öffnen Sie den Datei-Explorer, und wechseln Sie zu dem Ordner, der den von Ihnen geklonten DSP-Beispielcode enthält. Dort befindet sich ein **BUILD**-Ordner, suchen Sie nach **DPS.ino.bin** und **DPS.ino.map**, und kopieren Sie diese Dateien in diesen Ordner.
  ![Generierte Dateien](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Wenn Sie die Konfiguration von `built.path` für Arduino in einen anderen Ordner geändert haben, müssen Sie diese Dateien in dem von Ihnen konfigurierten Ordner suchen. You need to find those files in the folder you configured.

2. Fügen Sie diese beiden Dateien in den Ordner **Tools** ein, der sich auf der gleichen Ebene wie der **BUILD**-Ordner befindet.

3. Führen Sie die Datei **dps_cert_gen.exe** aus, und folgen Sie den Eingabeaufforderungen zur Eingabe Ihres **UDS**, der **MAC-Adresse** für das DevKit und der **Firmwareversion**, um das X.509-Zertifikat zu generieren.
  ![Ausführen der Datei „dps-cert-gen.exe“](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Beobachten Sie die erfolgreiche Generierung. Ein **PEM**-Zertifikat wird im gleichen Ordner gespeichert.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Erstellen eines Geräteregistrierungseintrags im Device Provisioning-Dienst

1. Navigieren Sie im Azure-Portal zu Ihrem Provisioning-Dienst. Klicken Sie auf **Registrierungen verwalten**, und wählen Sie die Registerkarte **Individuelle Registrierungen**. ![Individuelle Registrierungen](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Klicken Sie auf **Hinzufügen**.

3. Wählen Sie in **Mechanismus** die Option **X.509** aus.
  ![Hochladen des Zertifikats](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

4. Laden Sie in **PEM- oder CER-Zertifikatdatei** das entsprechende **PEM**-Zertifikat hoch.

5. Übernehmen Sie die übrigen Standardeinstellungen, und klicken Sie auf **Speichern**.

## <a name="start-the-devkit"></a>Starten des DevKit

1. Starten Sie Visual Studio Code, und öffnen Sie den seriellen Monitor.

2. Drücken Sie auf Ihrem DevKit die **Reset**-Taste.

Daraufhin sollte das DevKit die Registrierung bei Ihrem Device Provisioning-Dienst starten.

![Visual Studio Code-Ausgabe](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-the-devkit-is-registered-on-iot-hub"></a>Überprüfen der DevKit-Registrierung bei IoT Hub

Nach dem Starten Ihres Geräts sollten die folgenden Aktionen ausgeführt werden:

1. Das Gerät sendet eine Registrierungsanforderung an Ihren Device Provisioning-Dienst.
2. Der Device Provisioning-Dienst sendet eine Registrierungsaufforderung zurück, auf die Ihr Gerät reagiert.
3. Nach erfolgreicher Registrierung sendet der Device Provisioning-Dienst den IoT Hub-URI, die Geräte-ID und den verschlüsselten Schlüssel an das Gerät zurück.
4. Die IoT Hub-Clientanwendung auf dem Gerät stellt dann eine Verbindung mit Ihrem Hub her.
5. Nachdem eine Verbindung mit dem Hub erfolgreich hergestellt wurde, sollte das Gerät im Device Explorer von IoT Hub angezeigt werden.
  ![Registriertes Gerät](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-device-id"></a>Ändern der Geräte-ID

Die in Azure IoT Hub registrierte standardmäßige Geräte-ID lautet **AZ3166**. Wenn Sie diese ändern möchten, folgen Sie den [hier angegebenen Anweisungen](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt gelernt, das DevKit so vorzubereiten, dass es ein Gerät sicher mit DICE bei DPS registriert, damit es automatisch und ohne Tastendruck bei IoT Hub registriert werden kann. In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren des globalen DPS-Endpunkts auf dem Gerät
> * Verwenden eines eindeutigen geheimen Geräteschlüssels (Unique Device Secret, UDS) zum Generieren eines X.509-Zertifikats
> * Registrieren eines einzelnen Geräts
> * Sicherstellen, dass das Gerät registriert ist

Wechseln Sie zu den anderen Tutorials, um Folgendes zu lernen:

> [!div class="nextstepaction"]
> [Erstellen und Bereitstellen eines simulierten Geräts](./quick-create-simulated-device.md)


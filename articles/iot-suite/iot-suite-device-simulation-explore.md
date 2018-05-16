---
title: Erste Schritte mit der Gerätesimulationslösung – Azure | Microsoft-Dokumentation
description: Die IoT Solution Accelerators-Simulationslösung ist ein Tool, das Sie beim Entwickeln und Testen einer IoT-Lösung unterstützen kann. Dieser Simulationsdienst ist ein eigenständiges Angebot, das zusammen mit anderen Solution Accelerators oder mit Ihren eigenen benutzerdefinierten Lösungen verwendet werden kann.
services: iot device simulation
suite: iot-suite
author: troyhopwood
manager: timlt
ms.author: troyhop
ms.service: iot-suite
ms.date: 12/15/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 7dfd56d06e180dd163fa84d4289e6daf4baefc51
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="device-simulation-walkthrough"></a>Exemplarische Vorgehensweise bei der Gerätesimulation

Die Azure IoT-Gerätesimulation ist ein Tool, das Sie beim Entwickeln und Testen einer IoT-Lösung unterstützen kann. Die Gerätesimulation ist ein eigenständiges Angebot, das zusammen mit anderen Solution Accelerators oder mit Ihren eigenen benutzerdefinierten Lösungen verwendet werden kann.

Dieses Tutorial veranschaulicht einige Features der Gerätesimulation. Sie erfahren, wie die Gerätesimulation funktioniert, und können das Tutorial verwenden, um Ihre eigenen IoT-Lösungen zu testen.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Konfigurieren einer Simulation
> * Starten und Anhalten einer Simulation
> * Anzeigen von Telemetriedaten

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Tutorials benötigen Sie eine bereitgestellte Instanz der Azure-IoT-Gerätesimulation in Ihrem Azure-Abonnement.

Wenn Sie die Gerätesimulation noch nicht bereitgestellt haben, sollten Sie zunächst das Tutorial [Bereitstellen der Azure-IoT-Gerätesimulation](iot-suite-device-simulation-deploy.md) durcharbeiten.

## <a name="configuring-device-simulation"></a>Konfigurieren der Gerätesimulation

Sie können die Gerätesimulation komplett im Dashboard konfigurieren und ausführen. Öffnen Sie das Dashboard auf der IoT Solution Accelerators-Seite [Bereitgestellte Lösungen](https://www.azureiotsuite.com/). Klicken Sie unterhalb Ihrer neuen Gerätesimulation auf **Starten**.

### <a name="target-iot-hub"></a>IoT-Zielhub

Sie können die Gerätesimulation mit einem vorab bereitgestellten IoT Hub oder mit einem beliebigen anderen IoT Hub verwenden:

![IoT-Zielhub](media/iot-suite-device-simulation-explore/targethub.png)

> [!NOTE]
> Die Option zur Verwendung eines vorab bereitgestellten IoT Hubs ist nur verfügbar, wenn Sie bei der Bereitstellung der Gerätesimulation die Option ausgewählt haben, dass ein neuer IoT Hub erstellt werden soll. Wenn Sie keinen IoT Hub besitzen, können Sie jederzeit im [Azure-Portal](https://portal.azure.com) einen erstellen.

Um einen bestimmten IoT Hub als Ziel festzulegen, geben Sie die Verbindungszeichenfolge **iothubowner** an. Sie können diese Verbindungszeichenfolge aus dem [Azure-Portal](https://portal.azure.com) abrufen:

1. Klicken Sie auf der IoT Hub-Konfigurationsseite im Azure-Portal auf **Richtlinien für gemeinsamen Zugriff**.
1. Klicken Sie auf **iothubowner**.
1. Kopieren Sie entweder den primären oder den sekundären Schlüssel.
1. Fügen Sie diesen Wert in das Textfeld unterhalb von „IoT-Zielhub“ ein.

![IoT-Zielhub](media/iot-suite-device-simulation-explore/connectionstring.png)

### <a name="device-model"></a>Gerätemodell

Über das Gerätemodell können Sie den Typ des Geräts auswählen, das simuliert werden soll. Sie können eins der vorkonfigurierten Gerätemodelle auswählen oder eine Liste mit Sensoren definieren, um ein benutzerdefiniertes Gerätemodell zu konfigurieren:

![Gerätemodell](media/iot-suite-device-simulation-explore/devicemodel.png)

#### <a name="pre-configured-device-models"></a>Vorkonfigurierte Gerätemodelle

Die Gerätesimulation bietet drei vorkonfigurierte Gerätemodelle. Es stehen Gerätemodelle für Aufzüge, Kältemaschinen und LKW zur Verfügung.

Vorkonfigurierte Gerätemodelle umfassen mehrere Sensoren mit erweiterten, in einer JavaScript-Datei definierten Verhaltensweisen. Diese benutzerdefinierten Verhaltensweisen werden in der Web-Benutzeroberfläche nicht unterstützt. 

Die folgende Tabelle zeigt eine Liste der Konfigurationen für jedes vorkonfigurierte Gerätemodell:

| Gerätemodell | Sensor | Unit | 
| -------------| ------ | -----| 
| Kältemaschine | Luftfeuchtigkeit | % |
| | pressure | psig | 
| | Temperatur | F | 
| Aufzug | Etage | 
| | Vibration | MM | 
| | Temperatur | F | 
| LKW | Breitengrad | |
| | Längengrad | | 
| | Geschwindigkeit | mph | 
| | Temperatur Ladung | F | 

#### <a name="custom-device-model"></a>Benutzerdefiniertes Gerätemodell

Mit benutzerdefinierten Gerätemodellen können Sie Sensoren modellieren, die Ihre eigenen Geräte genauer darstellen. Ein benutzerdefiniertes Gerät kann bis zu 10 benutzerdefinierte Sensoren enthalten.

Wenn Sie den Modelltyp für das benutzerdefinierte Gerät auswählen, können Sie neue Sensoren hinzufügen, indem Sie auf **+ Sensor hinzufügen** klicken.

![Sensoren hinzufügen](media/iot-suite-device-simulation-explore/customsensors.png)

Benutzerdefinierte Sensoren verfügen über folgende Eigenschaften:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Name des Sensors | Ein Anzeigename für den Sensor wie z.B. **Temperatur** oder **Geschwindigkeit**. |
| Verhalten | Die Einstellung „Verhalten“ ermöglicht es, dass Telemetriedaten zwischen den verschiedenen Meldungen variieren, um reale Daten zu simulieren. **Erhöhen** startet beim Mindestwert und erhöht den Wert in jeder gesendeten Meldung um eins. Wenn der Höchstwert erreicht ist, beginnt dieses Verhalten beim Mindestwert wieder von vorne. **Verringern** verhält sich genauso wie **Erhöhen**, zählt jedoch abwärts. Das Verhalten **Zufällig** generiert einen zufälligen Wert zwischen dem Mindest- und den Höchstwerten. |
| Mindestwert | Die niedrigste Zahl im akzeptablen Bereich. |
| Höchstwert | Die größte Zahl im akzeptablen Bereich. |
| Unit | Die Maßeinheit für den Sensor, z.B. „%“ oder „mm“. |

### <a name="number-of-devices"></a>Anzahl von Geräten

Mit der Gerätesimulation können Sie derzeit bis zu 20.000 Geräte simulieren.

![Anzahl von Geräten](media/iot-suite-device-simulation-explore/numberofdevices.png)

### <a name="telemetry-frequency"></a>Telemetriefrequenz

Mit der Telemetriefrequenz können Sie angeben, wie häufig Ihre simulierten Geräte Daten an den IoT Hub senden sollen. Sie können Daten sehr häufig, z.B. alle 10 Sekunden, oder eher selten, z.B. alle 99 Stunden, 59 Minuten und 59 Sekunden, senden.

![Telemetriefrequenz](media/iot-suite-device-simulation-explore/frequency.png)

> [!NOTE]
> Wenn Sie nicht den vorab bereitgestellten, sondern einen anderen IoT Hub verwenden, sollten Sie Meldungsgrenzwerte für Ihren IoT-Zielhub erwägen. Wenn Sie z.B. über 1.000 simulierte Geräte verfügen, die alle 10 Sekunden Telemetriedaten an einen S1-Hub senden, wird das Telemetrielimit für den Hub in etwas mehr als einer Stunde erreicht.

### <a name="simulation-duration"></a>Simulationsdauer

Sie können Ihre Simulation über eine bestimmte Zeitspanne oder bis zur ausdrücklichen Beendigung ausführen. Sie können jede beliebige Zeitspanne zwischen 10 Minuten und 99 Stunden, 59 Minuten und 59 Sekunden auswählen.

![Simulationsdauer](media/iot-suite-device-simulation-explore/duration.png)

### <a name="start-and-stop-the-simulation"></a>Starten und Beenden der Simulation

Wenn Sie alle erforderlichen Konfigurationsdaten im Formular eingegeben haben, ist die Schaltfläche **Simulation starten** aktiviert. Klicken Sie auf diese Schaltfläche, um die Simulation zu starten.

![Simulation starten](media/iot-suite-device-simulation-explore/start.png)

Wenn Sie eine bestimmte Dauer für Ihre Simulation angegeben haben, wird sie automatisch beendet, wenn der entsprechende Zeitpunkt erreicht ist. Sie können die Simulation jederzeit vorher anhalten, indem Sie auf **Simulation beenden** klicken.

Wenn Sie keine zeitliche Begrenzung für Ihre Simulation ausgewählt haben, wird sie ausgeführt, bis Sie auf **Simulation beenden** klicken. Sie können den Browser schließen und jederzeit zur Seite „Gerätesimulation“ zurückkehren, um Ihre Simulation zu beenden.

![Simulation beenden](media/iot-suite-device-simulation-explore/stop.png)

> [!NOTE]
> Es kann immer nur eine Simulation gleichzeitig ausgeführt werden. Bevor Sie eine neue Simulation starten, müssen Sie die aktuell ausgeführte Simulation beenden.

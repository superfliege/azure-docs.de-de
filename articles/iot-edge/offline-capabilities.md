---
title: Azure IoT Edge-Offlinefunktionen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie IoT Edge-Geräte und -Module über längere Zeiträume offline betrieben werden können und wie IoT Edge auch den Offlinebetrieb normaler IoT-Geräte ermöglichen kann.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c4ab33f4d706eb677b2b790ff871c1fb900846ff
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235631"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices-preview"></a>Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte (Vorschau)

Azure IoT Edge unterstützt erweiterte Offlinevorgänge auf Ihren IoT Edge-Geräten und ermöglicht Offlinevorgänge auch auf untergeordneten Nicht-Edge-Geräten. Solange ein IoT Edge-Gerät die Möglichkeit besitzt, eine Verbindung mit IoT Hub herzustellen, kann es zusammen mit allen untergeordneten Geräten auch mit unregelmäßiger oder ohne Internetverbindung funktionieren. 

>[!NOTE]
>Die Offlineunterstützung für IoT Edge ist als [öffentliche Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) verfügbar.

## <a name="how-it-works"></a>So funktioniert's

Wenn ein IoT Edge-Gerät in den Offlinemodus wechselt, übernimmt der Edge Hub drei Rollen. Zunächst erfasst er alle für die Upstream-Übermittlung bestimmten Nachrichten und speichert sie, bis das Gerät wieder eine Verbindung herstellt. Zweitens handelt er im Auftrag von IoT Hub und authentifiziert Module und untergeordnete Geräte, sodass diese weiterhin ausgeführt werden können. Schließlich ermöglicht er die Kommunikation zwischen untergeordneten Geräten, die normalerweise über IoT Hub abgewickelt würde. 

Das folgende Beispiel veranschaulicht ein IoT Edge-Szenario im Offlinemodus:

1. **Konfigurieren Sie ein IoT Edge-Gerät.**

   Für IoT Edge-Geräte werden automatisch Offlinefunktionen aktiviert. Um diese Funktionen auf andere IoT-Geräte auszuweiten, müssen Sie eine Beziehung über- und untergeordneter Elemente zwischen den Geräten in IoT Hub deklarieren. 

2. **Führen Sie die Synchronisierung mit IoT Hub aus.**

   Mindestens einmal nach der Installation der IoT Edge-Runtime muss das IoT Edge-Gerät online sein, damit die Synchronisierung mit IoT Hub ausgeführt wird. Bei dieser Synchronisierung empfängt das IoT Edge-Gerät Details zu allen zugewiesenen untergeordneten Geräten. Das IoT Edge-Gerät aktualisiert zudem auf sichere Weise seinen lokalen Cache, um Offlinevorgänge zu aktivieren und ruft Einstellungen für das lokale Speichern von Telemetrienachrichten. 

3. **Wechseln Sie in den Offlinemodus.**

   Während sie von IoT Hub getrennt sind, können das IoT Edge-Gerät, seine bereitgestellten Module und alle untergeordneten IoT-Geräte ohne zeitliche Beschränkung betrieben werden. Module und untergeordneten Geräte gestartet und neu gestartet werden, indem im Offlinemodus die Authentifizierung mit dem Edge Hub durchgeführt wird. Telemetriedaten für die Upstreamübermittlung an IoT Hub werden lokal gespeichert. Die Kommunikation zwischen Modulen und untergeordneten IoT-Geräten wird durch direkte Methoden oder Nachrichten aufrechterhalten. 

4. **Stellen Sie erneut eine Verbindung her, und führen Sie erneut die Synchronisierung mit IoT Hub aus.**

   Nach dem Wiederherstellen der Verbindung mit IoT Hub wird die Synchronisierung des IoT Edge-Geräts wieder ausgeführt. Lokal gespeicherte Nachrichten werden in der Reihenfolge übermittelt, in der sie gespeichert wurden. Unterschiede zwischen den gewünschten und gemeldeten Eigenschaften der Module und Geräte werden ausgeglichen. Das IoT Edge-Gerät aktualisiert seine Gruppe von zugewiesenen untergeordneten IoT-Geräten mit den Änderungen.

## <a name="restrictions-and-limits"></a>Einschränkungen

Die in diesem Artikel beschriebenen erweiterten Offlinefunktionen sind unter [IoT Edge-Version 1.0.4 oder höher](https://github.com/Azure/azure-iotedge/releases) verfügbar. Frühere Versionen verfügen nur über einen Teil dieser Offlinefunktionen. Für vorhandene IoT Edge-Geräte, die über keine erweiterten Offlinefunktionen verfügen, kann kein Upgrade durch Ändern der Runtime-Version ausgeführt werden. Stattdessen sind sie mit einer neuen IoT Edge-Geräteidentität zu konfigurieren, damit diese Funktionen verfügbar werden. 

Erweiterte Offlinefunktionen werden in allen Regionen unterstützt, in denen IoT Hub verfügbar ist, mit Ausnahme von USA (Osten) und Europa, Westen. 

Lediglich Nicht-Edge IoT-Geräte können als untergeordnete Geräte hinzugefügt werden. 

IoT Edge-Geräte und deren zugewiesene untergeordnete Geräte können nach der ersten, einmaligen Synchronisierung unbefristet im Offlinemodus betrieben werden. Die Speicherung von Nachrichten hängt jedoch von der Einstellung für die TTL (Gültigkeitsdauer) und dem verfügbaren Speicherplatz zum Ablegen der Nachrichten ab. 

## <a name="set-up-an-edge-device"></a>Einrichten eines IoT Edge-Geräts

Damit ein IoT Edge-Gerät seine erweiterten Offlinefunktionen auf untergeordnete IoT-Geräte ausweiten kann, müssen Sie die Beziehungen über- und untergeordneter Elemente im Azure-Portal deklarieren.

### <a name="assign-child-devices"></a>Zuweisen von untergeordneten Geräten

Untergeordnete Geräte können beliebige Nicht-Edge-Geräte sein, die beim selben IoT Hub registriert sind. Sie können die Beziehung über- und untergeordneter Geräte beim Erstellen eines neuen Geräts verwalten, oder auf der Gerätedetailseite des übergeordneten IoT Edge-Geräts oder des untergeordneten IoT-Geräts. 

   ![Verwalten untergeordneter Geräte auf der Detailseite des IoT Edge-Geräts](./media/offline-capabilities/manage-child-devices.png)

Übergeordnete Geräte können über mehrere untergeordnete Geräte verfügen, ein untergeordnetes Gerät hingegen kann nur ein übergeordnetes Gerät besitzen.

## <a name="optional-offline-settings"></a>Optionale Offlineeinstellungen

Wenn Sie längere Offlinezeiträume für Ihre Geräte erwarten, nach denen Sie alle generierten Nachrichten erfassen möchten, konfigurieren Sie den Edge Hub so, dass alle Nachrichten gespeichert werden können. Es gibt zwei Änderungen, die Sie an Edge Hub vornehmen können, um die langfristige Speicherung von Nachrichten zu ermöglichen. Vergrößern Sie zunächst die Einstellung für die Gültigkeitsdauer, und fügen Sie dann zusätzlichen Speicherplatz zum Speichern der Nachrichten hinzu. 

### <a name="time-to-live"></a>Gültigkeitsdauer

Die Gültigkeitsdauer entspricht der Zeit (in Sekunden), die eine Nachricht auf die Übermittlung warten kann, ehe sie abläuft. Der Standardwert ist 7.200 Sekunden (zwei Stunden). 

Diese Einstellung ist eine gewünschte Eigenschaft des Edge Hub, die im Modulzwilling gespeichert wird. Sie können sie im Azure-Portal im Abschnitt **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** oder direkt im Bereitstellungsmanifest konfigurieren. 

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="additional-offline-storage"></a>Zusätzlicher Offlinespeicher

Nachrichten werden standardmäßig im Dateisystem des Edge Hub-Containers gespeichert. Wenn der Speicherplatz für die Offlineanforderungen nicht ausreicht, können Sie lokalen Speicher auf dem IoT Edge-Gerät reservieren. Sie müssen eine Umgebungsvariable für den Edge Hub erstellen, die auf einen Speicherordner im Container zeigt. Binden Sie diesen Speicherordner anschließend mit den Erstellungsoptionen an einem Ordner auf dem Hostcomputer. 

Sie können Umgebungsvariablen und die Erstellungsoptionen für das Edge Hub-Modul im Azure-Portal im Abschnitt **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** konfigurieren. Sie können sie jedoch auch direkt im Bereitstellungsmanifest konfigurieren. 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": "{\"HostConfig\":{\"Binds\":[\"<HostStoragePath>:<ModuleStoragePath>\"],\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}]}}}"
    },
    "env": {
        "storageFolder": {
            "value": "<ModuleStoragePath>"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Ersetzen Sie `<HostStoragePath>` und `<ModuleStoragePath>` durch den Speicherpfad für Ihren Host und Ihr Modul; bei beiden muss es sich um absolute Pfade handeln.  Beispielsweise bedeutet `\"Binds\":[\"/etc/iotedge/storage/:/iotedge/storage/"`, dass der Hostpfad `/etc/iotedge/storage` dem Containerpfad `/iotedge/storage/` zugeordnet ist.  Weitere Details zu „createOptions“ finden Sie in der [Docker-Dokumentation](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="next-steps"></a>Nächste Schritte

Aktivieren Sie erweiterte Offlinevorgänge in Ihren [transparenten Gatewayszenarien](how-to-create-transparent-gateway.md).

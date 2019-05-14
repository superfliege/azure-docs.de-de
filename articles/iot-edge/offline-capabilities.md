---
title: Offlinebetrieb von Geräten – Azure IoT Edge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie IoT Edge-Geräte und -Module über längere Zeiträume ohne Internetverbindung betrieben werden können und wie IoT Edge auch den Offlinebetrieb normaler IoT-Geräte ermöglichen kann.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 74d2601c2319ccad9cc980b83894a3242705aa46
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148121"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte

Azure IoT Edge unterstützt erweiterte Offlinevorgänge auf Ihren IoT Edge-Geräten und ermöglicht Offlinevorgänge auch auf untergeordneten Nicht-Edge-Geräten. Solange ein IoT Edge-Gerät die Möglichkeit besitzt, eine Verbindung mit IoT Hub herzustellen, kann es zusammen mit allen untergeordneten Geräten auch mit unregelmäßiger oder ohne Internetverbindung funktionieren. 


## <a name="how-it-works"></a>So funktioniert's

Wenn ein IoT Edge-Gerät in den Offlinemodus wechselt, übernimmt der IoT Edge-Hub drei Rollen. Zunächst erfasst er alle für die Upstream-Übermittlung bestimmten Nachrichten und speichert sie, bis das Gerät wieder eine Verbindung herstellt. Zweitens handelt er im Auftrag von IoT Hub und authentifiziert Module und untergeordnete Geräte, sodass diese weiterhin ausgeführt werden können. Schließlich ermöglicht er die Kommunikation zwischen untergeordneten Geräten, die normalerweise über IoT Hub abgewickelt würde. 

Das folgende Beispiel veranschaulicht ein IoT Edge-Szenario im Offlinemodus:

1. **Konfigurieren Sie ein IoT Edge-Gerät.**

   Für IoT Edge-Geräte werden automatisch Offlinefunktionen aktiviert. Um diese Funktionen auf andere IoT-Geräte auszuweiten, müssen Sie eine Beziehung über- und untergeordneter Elemente zwischen den Geräten in IoT Hub deklarieren. 

2. **Führen Sie die Synchronisierung mit IoT Hub aus.**

   Mindestens einmal nach der Installation der IoT Edge-Runtime muss das IoT Edge-Gerät online sein, damit die Synchronisierung mit IoT Hub ausgeführt wird. Bei dieser Synchronisierung empfängt das IoT Edge-Gerät Details zu allen zugewiesenen untergeordneten Geräten. Das IoT Edge-Gerät aktualisiert zudem auf sichere Weise seinen lokalen Cache, um Offlinevorgänge zu aktivieren und ruft Einstellungen für das lokale Speichern von Telemetrienachrichten. 

3. **Wechseln Sie in den Offlinemodus.**

   Während sie von IoT Hub getrennt sind, können das IoT Edge-Gerät, seine bereitgestellten Module und alle untergeordneten IoT-Geräte ohne zeitliche Beschränkung betrieben werden. Module und untergeordnete Geräte können gestartet und neu gestartet werden, indem im Offlinemodus die Authentifizierung mit dem IoT Edge-Hub durchgeführt wird. Telemetriedaten für die Upstreamübermittlung an IoT Hub werden lokal gespeichert. Die Kommunikation zwischen Modulen und untergeordneten IoT-Geräten wird durch direkte Methoden oder Nachrichten aufrechterhalten. 

4. **Stellen Sie erneut eine Verbindung her, und führen Sie die Synchronisierung mit IoT Hub erneut aus.**

   Nach dem Wiederherstellen der Verbindung mit IoT Hub wird die Synchronisierung des IoT Edge-Geräts wieder ausgeführt. Lokal gespeicherte Nachrichten werden in der Reihenfolge übermittelt, in der sie gespeichert wurden. Unterschiede zwischen den gewünschten und gemeldeten Eigenschaften der Module und Geräte werden ausgeglichen. Das IoT Edge-Gerät aktualisiert seine Gruppe von zugewiesenen untergeordneten IoT-Geräten mit den Änderungen.

## <a name="restrictions-and-limits"></a>Einschränkungen

Die in diesem Artikel beschriebenen erweiterten Offlinefunktionen sind unter [IoT Edge-Version 1.0.4 oder höher](https://github.com/Azure/azure-iotedge/releases) verfügbar. Frühere Versionen verfügen nur über einen Teil dieser Offlinefunktionen. Für vorhandene IoT Edge-Geräte, die über keine erweiterten Offlinefunktionen verfügen, kann kein Upgrade durch Ändern der Runtime-Version ausgeführt werden. Stattdessen sind sie mit einer neuen IoT Edge-Geräteidentität zu konfigurieren, damit diese Funktionen verfügbar werden. 

Erweiterte Offlinefunktionen werden in allen Regionen unterstützt, in denen IoT Hub verfügbar ist (**Ausnahme:** USA, Osten).

Lediglich Nicht-Edge IoT-Geräte können als untergeordnete Geräte hinzugefügt werden. 

IoT Edge-Geräte und deren zugewiesene untergeordnete Geräte können nach der ersten, einmaligen Synchronisierung unbefristet im Offlinemodus betrieben werden. Die Speicherung von Nachrichten hängt jedoch von der Einstellung für die TTL (Gültigkeitsdauer) und dem verfügbaren Speicherplatz zum Ablegen der Nachrichten ab. 

## <a name="set-up-an-iot-edge-device"></a>Einrichten eines IoT Edge-Geräts

Damit ein IoT Edge-Gerät seine erweiterten Offlinefunktionen auf untergeordnete IoT-Geräte ausweiten kann, müssen Sie die Beziehungen über- und untergeordneter Elemente im Azure-Portal deklarieren.

### <a name="assign-child-devices"></a>Zuweisen von untergeordneten Geräten

Untergeordnete Geräte können beliebige Nicht-Edge-Geräte sein, die beim selben IoT Hub registriert sind. Übergeordnete Geräte können über mehrere untergeordnete Geräte verfügen, ein untergeordnetes Gerät hingegen kann nur ein übergeordnetes Gerät besitzen. Es gibt drei Optionen für das Festlegen von untergeordneten Geräten auf einem Edge-Gerät:

#### <a name="option-1-iot-hub-portal"></a>Option 1: IoT Hub-Portal

 Sie können die Beziehung über- und untergeordneter Geräte beim Erstellen eines neuen Geräts verwalten, oder auf der Gerätedetailseite des übergeordneten IoT Edge-Geräts oder des untergeordneten IoT-Geräts. 

   ![Verwalten untergeordneter Geräte auf der Detailseite des IoT Edge-Geräts](./media/offline-capabilities/manage-child-devices.png)


#### <a name="option-2-use-the-az-command-line-tool"></a>Option 2: Verwenden des `az`Befehlszeilentools

Mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) mit [IoT-Erweiterung](https://github.com/azure/azure-iot-cli-extension) (v0.7.0 oder höher) können Sie Beziehungen zwischen über- und untergeordneten Geräten über die [device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)-Unterbefehle verwalten. Im folgenden Beispiel führen wir eine Abfrage durch, um alle Nicht-IoT Edge-Geräte auf dem Hub als untergeordnete Geräte eines IoT Edge-Geräts zuzuweisen. 

```shell
# Set IoT Edge parent device
egde_device="edge-device1"

# Get All IoT Devices
device_list=$(az iot hub query \
        --hub-name replace-with-hub-name \
        --subscription replace-with-sub-name \
        --resource-group replace-with-rg-name \
        -q "SELECT * FROM devices WHERE capabilities.iotEdge = false" \
        --query 'join(`, `, [].deviceId)' -o tsv)

# Add all IoT devices to IoT Edge (as child)
az iot hub device-identity add-children \
  --device-id $egde_device \
  --child-list $device_list \
  --hub-name replace-with-hub-name \
  --resource-group replace-with-rg-name \
  --subscription replace-with-sub-name 
```

Sie können die [Abfrage](../iot-hub/iot-hub-devguide-query-language.md) so ändern, dass eine andere Teilmenge von Geräten ausgewählt wird. Der Befehl kann mehrere Sekunden in Anspruch nehmen, wenn Sie eine große Gruppe von Geräten angeben.

#### <a name="option-3-use-iot-hub-service-sdk"></a>Option 3: Verwenden des IoT Hub Service SDK 

Beziehungen zwischen über- und untergeordneten Geräten können Sie außerdem programmgesteuert mit einem C#-, Java- oder Node.js-IoT Hub Service SDK verwalten. Dies ist ein [Beispiel für die Zuordnung eines untergeordneten Geräts](https://aka.ms/set-child-iot-device-c-sharp) mit dem C#-SDK.

### <a name="specifying-dns-servers"></a>Angeben von DNS-Servern 

Zur Verbesserung der Stabilität empfiehlt es sich dringend, die in Ihrer Umgebung verwendeten DNS-Serveradressen anzugeben. Beachten Sie die [beiden im Artikel zur Problembehandlung aufgeführten diesbezüglichen Möglichkeiten](troubleshoot.md#resolution-7).

## <a name="optional-offline-settings"></a>Optionale Offlineeinstellungen

Wenn Sie alle Nachrichten erfassen möchten, die Ihre Geräte während langer Offlinezeiträume generieren, konfigurieren Sie den IoT Edge-Hub so, dass alle Nachrichten gespeichert werden können. Es gibt zwei Änderungen, die Sie an IoT Edge-Hub vornehmen können, um die langfristige Speicherung von Nachrichten zu ermöglichen. Legen Sie zuerst einen höheren Wert für die Einstellung „Gültigkeitsdauer“ fest. Fügen Sie dann zusätzlichen Speicherplatz für den Nachrichtenspeicher hinzu. 

### <a name="time-to-live"></a>Gültigkeitsdauer

Die Gültigkeitsdauer entspricht der Zeit (in Sekunden), die eine Nachricht auf die Übermittlung warten kann, ehe sie abläuft. Der Standardwert ist 7.200 Sekunden (zwei Stunden). Der Höchstwert ist nur durch den Höchstwert einer ganzzahligen Variablen begrenzt und beträgt ca. 2 Milliarden. 

Diese Einstellung ist eine gewünschte Eigenschaft des IoT Edge-Hub, die im Modulzwilling gespeichert wird. Sie können sie im Azure-Portal im Abschnitt **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** oder direkt im Bereitstellungsmanifest konfigurieren. 

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

Nachrichten werden standardmäßig im Dateisystem des IoT Edge-Hub-Containers gespeichert. Wenn der Speicherplatz für die Offlineanforderungen nicht ausreicht, können Sie lokalen Speicher auf dem IoT Edge-Gerät reservieren. Erstellen Sie eine Umgebungsvariable für den IoT Edge-Hub, die auf einen Speicherordner im Container zeigt. Binden Sie diesen Speicherordner anschließend mit den Erstellungsoptionen an einem Ordner auf dem Hostcomputer. 

Sie können Umgebungsvariablen und die Erstellungsoptionen für das IoT Edge-Hub-Modul im Azure-Portal im Abschnitt **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** konfigurieren. Sie können sie jedoch auch direkt im Bereitstellungsmanifest konfigurieren. 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": {
            "HostConfig": {
                "Binds": ["<HostStoragePath>:<ModuleStoragePath>"],
                "PortBindings": {
                    "8883/tcp": [{"HostPort":"8883"}],
                    "443/tcp": [{"HostPort":"443"}],
                    "5671/tcp": [{"HostPort":"5671"}]
                }
            }
        }
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

Ersetzen Sie `<HostStoragePath>` und `<ModuleStoragePath>` durch den Speicherpfad für Ihren Host und Ihr Modul; bei beiden muss es sich um absolute Pfade handeln. Binden Sie in den Erstellungsoptionen die Speicherpfade für Host und Modul zusammen. Erstellen Sie dann eine Umgebungsvariable, die auf den Speicherpfad für das Modul zeigt.  

Beispielsweise bedeutet `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]`, dass das Verzeichnis **/etc/iotedge/storage** in Ihrem Hostsystem dem Verzeichnis **/iotedge/storage/** für den Container zugeordnet ist. In einem weiteren Beispiel zu Windows-Systemen bedeutet `"Binds":["C:\\temp:C:\\contemp"]`, dass das Verzeichnis **C:\\temp** in Ihrem Hostsystem dem Verzeichnis **C:\\contemp** für den Container zugeordnet ist. 

Weitere Details zu Erstellungsoptionen finden Sie in der [Docker-Dokumentation](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="next-steps"></a>Nächste Schritte

Aktivieren Sie erweiterte Offlinevorgänge in Ihren [transparenten Gatewayszenarien](how-to-create-transparent-gateway.md).

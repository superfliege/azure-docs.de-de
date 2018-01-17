---
title: "Bereitstellen von ModBus für Azure IoT Edge | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie es Geräten mit Verwendung von ModBus-TCP die Kommunikation mit Azure IoT Hub ermöglichen, indem Sie ein IoT Edge-Gatewaygerät erstellen."
services: iot-Edge
documentationcenter: 
author: kgremban
manager: timlt
editor: chrisgmsft
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: kgremban
ms.custom: 
ms.openlocfilehash: e239bde48c3da0d899e3c78bdd39f520c4128b95
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway---preview"></a>Verbinden von ModBus-TCP-Geräten über ein IoT Edge-Gerätegateway – Vorschauversion

Wenn Sie IoT-Geräte, für die ModBus-TCP- oder -RTU-Protokolle genutzt werden, mit einem Azure IoT Hub verbinden möchten, sollten Sie ein IoT Edge-Gerät als Gateway einsetzen. Das Gatewaygerät liest Daten von Ihren ModBus-Geräten und kommuniziert sie dann über ein unterstütztes Protokoll weiter an die Cloud. 

![Verbindung von ModBus-Geräten mit dem IoT Hub per Edge-Gateway](./media/deploy-modbus-gateway/diagram.png)

In diesem Artikel wird beschrieben, wie Sie Ihr eigenes Containerimage für ein ModBus-Modul erstellen (Sie können auch ein vorgefertigtes Beispiel verwenden) und dann auf dem IoT Edge-Gerät bereitstellen, das als Ihr Gateway fungiert. 

In diesem Artikel wird davon ausgegangen, dass Sie das ModBus-TCP-Protokoll verwenden. Weitere Informationen zur Konfiguration des Moduls zur Unterstützung von ModBus-RTU finden Sie im Projekt vom Typ [ModBus-Modul für Azure IoT Edge](https://github.com/Azure/iot-edge-modbus) auf GitHub. 

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure IoT Edge-Gerät. Eine exemplarische Vorgehensweise zur Einrichtung finden Sie unter [Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter Windows](tutorial-simulate-device-windows.md) oder [Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter Linux](tutorial-simulate-device-linux.md). 
* Die Verbindungszeichenfolge des primären Schlüssels für das IoT Edge-Gerät
* Ein physisches oder simuliertes ModBus-Gerät, das ModBus-TCP unterstützt.

## <a name="prepare-a-modbus-container"></a>Vorbereiten eines ModBus-Containers

Wenn Sie die Funktionalität des ModBus-Gateways testen möchten, können Sie ein von Microsoft bereitgestelltes Beispielmodul verwenden. Navigieren Sie hierfür zum Abschnitt [Ausführen der Lösung](#run-the-solution), und geben Sie Folgendes als Image-URI ein: 

```URL
microsoft/azureiotedge-modbus-tcp:1.0-preview
```

Falls Sie Ihr eigenes Modul erstellen und für Ihre Umgebung anpassen möchten, können Sie das Open-Source-Projekt [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) (ModBus-Modul für Azure IoT Edge) auf GitHub nutzen. Befolgen Sie die Anweisungen in diesem Projekt, um Ihr eigenes Containerimage zu erstellen. Bei der Erstellung Ihres eigenen Containerimages helfen Ihnen die Anleitungen zum Veröffentlichen von Containerimages in einer Registrierung und Bereitstellen eines benutzerdefinierten Moduls auf Ihrem Gerät unter [Entwickeln und Bereitstellen eines C#-IoT Edge-Moduls für Ihr simuliertes Gerät – Vorschau](tutorial-csharp-module.md) weiter. 


## <a name="run-the-solution"></a>Ausführen der Lösung
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrem IoT Hub.
2. Wechseln Sie zu **IoT Edge (Vorschau)**, und wählen Sie Ihr IoT Edge-Gerät aus.
3. Wählen Sie **Module festlegen** aus.
4. Fügen Sie das ModBus-Modul hinzu:
   1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
   2. Geben Sie im Feld **Name** den Text „modbus“ ein.
   3. Geben Sie im Feld **Image** den Image-URI des Beispielcontainers ein: `microsoft/azureiotedge-modbus-tcp:1.0-preview`.
   4. Aktivieren Sie das Kontrollkästchen **Aktivieren**, um die gewünschten Eigenschaften des Modulzwillings zu aktualisieren.
   5. Kopieren Sie den folgenden JSON-Code in das Textfeld. Ändern Sie den Wert von **SlaveConnection** in die IPv4-Adresse Ihres ModBus-Geräts.

      ```JSON
      {  
        "properties.desired":{  
          "PublishInterval":"2000",
          "SlaveConfigs":{  
            "Slave01":{  
              "SlaveConnection":"<IPV4 address>",
              "HwId":"PowerMeter-0a:01:01:01:01:01",
              "Operations":{  
                "Op01":{  
                  "PollingInterval": "1000",
                  "UnitId":"1",
                  "StartAddress":"400001",
                  "Count":"2",
                  "DisplayName":"Voltage"
                }
              }
            }
          }
        }
      }
      ```

   6. Wählen Sie **Speichern** aus.
5. Wenn Sie zum Schritt **Module hinzufügen** zurückgekehrt sind, wählen Sie **Weiter** aus.
7. Kopieren Sie im Schritt **Routen angeben** den unten angegebenen JSON-Code in das Textfeld. Für diese Route werden alle Nachrichten, die vom ModBus-Modul gesammelt wurden, an den IoT Hub gesendet. Bei dieser Route ist „modbusOutput“ der Endpunkt, der vom ModBus-Modul zum Ausgeben von Daten verwendet wird, und „upstream“ ist ein spezielles Ziel, das dem Edge-Hub zum Senden von Nachrichten an den IoT Hub anweist. 
   ```JSON
   {
    "routes": {
      "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
    }
   }
   ```

8. Klicken Sie auf **Weiter**. 
9. Wählen Sie im Schritt **Vorlage überprüfen** die Option **Senden** aus. 
10. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie **Aktualisieren** aus. Es sollte angezeigt werden, dass das neue Element **modbus** zusammen mit der IoT Edge-Runtime ausgeführt wird.

## <a name="view-data"></a>Anzeigen von Daten
Zeigen Sie die Daten an, die über das ModBus-Modul eingehen:
```cmd/sh
docker logs -f modbus
```

Mithilfe des [IoT Hub-Explorer-Tools](https://github.com/azure/iothub-explorer) können Sie auch die vom Gerät gesendeten Telemetriedaten anzeigen. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Nutzung von IoT Edge-Geräten als Gateways finden Sie unter [Erstellen eines IoT Edge-Geräts, das als transparentes Gateway fungiert](how-to-create-transparent-gateway.md).
- Weitere Informationen zur Funktionsweise von IoT Edge-Modulen finden Sie unter [Grundlegendes zu Azure IoT Edge-Modulen](iot-edge-modules.md).
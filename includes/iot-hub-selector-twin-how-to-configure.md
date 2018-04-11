> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-how-to-configure.md)
> * [Python](../articles/iot-hub/iot-hub-python-python-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a>Einführung

In [Erste Schritte mit Gerätezwillingen][lnk-twin-tutorial] haben Sie erfahren, wie Sie mithilfe von *Tags* Gerätemetadaten festlegen. Sie haben Gerätezustände mithilfe von *gemeldeten Eigenschaften* von einer Geräte-App erhalten und diese Informationen mithilfe einer SQL-ähnlichen Sprache abgefragt.

Dieses Tutorial beschreibt, wie Sie *gewünschte Eigenschaften* und *gemeldete Eigenschaften* des Gerätezwillings verwenden, um Geräte-Apps per Remotezugriff zu konfigurieren. Gemeldete und gewünschte Eigenschaften eines Gerätezwillings ermöglichen die Konfiguration einer Geräteanwendung in mehreren Schritten und machen den Status dieses Vorgangs über alle Geräte hinweg sichtbar. Weitere Informationen zur Rolle der Gerätekonfiguration finden Sie unter [Übersicht über die Geräteverwaltung mit IoT Hub][lnk-dm-overview].

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Ganz allgemein kann das Lösung-Back-End bei Verwendung von Gerätezwillingen die gewünschten Konfiguration für die verwalteten Geräte angeben, anstatt bestimmte Befehle zu senden. Das Gerät kann selbst die optimale Aktualisierungsmethode für seine Konfiguration einrichten (wichtig in IoT-Szenarien, bei denen sich bestimmte Gerätebedingungen auf die Fähigkeit auswirken, bestimmte Befehle sofort auszuführen), während der aktuelle Zustand und potenzielle Fehlerbedingungen des Aktualisierungsprozesses kontinuierlich gemeldet werden. Dieses Muster ist hilfreich bei der Verwaltung großer Mengen an Geräten, da das Lösungs-Back-End so über alle Geräte hinweg über einen vollständigen Einblick in den Zustand des Konfigurationsprozesses verfügt.

> [!TIP]
> In Szenarien, in denen Geräte auf interaktivere Weise gesteuert werden (z.B. das Einschalten eines Ventilators über eine vom Benutzer gesteuerte App), können Sie auch [direkte Methoden][lnk-methods] verwenden.

In diesem Tutorial ändert das Lösungs-Back-End die Telemetriekonfiguration eines Zielgeräts, sodass die Geräte-App ein Konfigurationsupdate anwendet. Beispiel: Ein Konfigurationsupdate erfordert einen Neustart eines Softwaremoduls. Dies wird im vorliegenden Tutorial mit einer einfachen Verzögerung simuliert.

Das Lösungs-Back-End speichert die Konfiguration wie folgt in den gewünschten Eigenschaften des Gerätezwillings:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

Da Konfigurationen komplexe Objekte sein können, werden ihnen eindeutige IDs zugewiesen (Hashes oder [GUIDs][lnk-guid]).


Die Geräte-App meldet ihre aktuelle Konfiguration und spiegelt die gewünschte Eigenschaft **telemetryConfig** in den gemeldeten Eigenschaften:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "configId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Beachten Sie, dass die gemeldete Eigenschaft **telemetryConfig** über eine zusätzliche **status**-Eigenschaft verfügt. Sie wird verwendet, um den Zustand des Aktualisierungsprozesses für die Konfiguration zu melden.

Wenn eine neue gewünschte Konfiguration empfangen wird, meldet die Geräte-App eine ausstehende Konfiguration, indem der Status geändert wird:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "configId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "configId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Später meldet die Geräte-App dann mittels Aktualisierung der Eigenschaft, ob der Vorgang erfolgreich war. Das Lösungs-Back-End kann den Status des Konfigurationsprozesses über alle Geräte hinweg jederzeit abfragen.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen Sie eine simulierte Geräte-App, die Konfigurationsupdates vom Lösungs-Back-End erhält und mehrere Updates als *gemeldete Eigenschaften* für den Aktualisierungsprozess der Konfiguration meldet.
* Erstellen Sie eine Back-End-App, mit der die gewünschte Konfiguration eines Geräts aktualisiert und anschließend der Aktualisierungsprozess der Konfiguration abgefragt wird.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

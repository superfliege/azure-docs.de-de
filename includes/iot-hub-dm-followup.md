## <a name="customize-and-extend-the-device-management-actions"></a>Passen Sie an und erweitern Sie das Gerät Verwaltungsaktionen

IoT-Lösungen können erweitern Sie den definierten Satz von Geräte-Management-Mustern oder benutzerdefinierte Muster mit dem Gerät und und Cloud-zu-Gerät-Methode primitive aktivieren. Weitere Beispiele für das Gerät Verwaltungsaktionen enthalten, auf die werkseinstellungen zurückgesetzt, Firmwareupdate Softwareupdate, energieverwaltung, Netzwerk- und Verbindungsproblemen Management und die Verschlüsselung von Daten.

## <a name="device-maintenance-windows"></a>Wartungsfenster für Gerät

In der Regel konfigurieren Sie die Geräte aus, um Aktionen zu einem Zeitpunkt ausführen, die Unterbrechung und Ausfallzeiten minimiert. Gerät Wartungsfenster sind ein häufig verwendete Muster für die Zeiträume, wenn ein Gerät ihre Konfiguration aktualisiert werden soll. Back-End-Lösungen können die Geräte und die gewünschten Eigenschaften definieren, und aktivieren Sie eine Richtlinie auf Ihrem Gerät, das ein Wartungsfenster ermöglicht. Wenn ein Gerät die wartungsfensterrichtlinie empfängt, können es die gemeldete Eigenschaft von dem Gerät und um den Status der Richtlinie zu melden. Die Back-End-app kann dann Gerät und Abfragen verwendet, der Kompatibilität von Geräten und jede Richtlinie auf kryptografische Weise nachweisen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm verwendet haben Sie eine direkte Methode zum Auslösen von eines remote-Neustart auf einem Gerät. Sie die gemeldeten Eigenschaften verwendet, um den Zeitpunkt der letzten Neustart vom Gerät zu melden und abgefragt, das Gerät und zum Zeitpunkt der letzten Neustart des Geräts aus der Cloud zu ermitteln.

Um den Vorgang fortzusetzen, erste Schritte mit IoT Hub und Device Management-Muster wie Remote über die Firmware-Aktualisierung per Funk, finden Sie unter:

[Exemplarische Vorgehensweise: So führen Sie ein Firmwareupdate][lnk-fwupdate]

Der IoT Erweitern der Lösung und Zeitplan auf mehreren Geräten Methodenaufrufe finden Sie unter der [Zeitplan und broadcast Aufträge] [ lnk-tutorial-jobs] Lernprogramm.

Um den Vorgang fortzusetzen, erste Schritte mit IoT Hub, finden Sie unter [erste Schritte mit IoT-Edge][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/iot-hub-node-node-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
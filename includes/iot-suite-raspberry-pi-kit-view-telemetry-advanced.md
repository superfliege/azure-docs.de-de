## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Die Brombeere Pi sendet Telemetrie ist jetzt an der remote-überwachungslösung. Sie können die Telemetrie im Projektmappen-Dashboard anzeigen. Sie können auch Nachrichten an Ihre Brombeere Pi über das Dashboard Solution senden.

- Navigieren Sie zu der Projektmappe-Dashboard.
- Wählen Sie Ihr Gerät bei der **Gerät anzuzeigende** Dropdownliste.
- Die Telemetriedaten aus der Brombeere Pi werden auf dem Dashboard angezeigt.

![Anzeigen von Telemetriedaten aus der Brombeere Pi][img-telemetry-display]

## <a name="initiate-the-firmware-update"></a>Initiieren Sie die Aktualisierung der firmware

Aktualisierung der Firmware zu heruntergeladen und installiert eine aktualisierte Version der Clientanwendung Gerät auf die Brombeere Pi. Weitere Informationen zur Aktualisierung der Firmware zu finden in der Beschreibung des updatemusters Firmware in [Überblick über Gerätemanagement und IoT Hub][lnk-update-pattern].

Initiieren Sie die Aktualisierung der Firmware zu durch Aufrufen einer Methode auf dem Gerät. Diese Methode ist asynchron und gibt zurück, sobald der Aktualisierungsvorgang beginnt. Das Gerät verwendet gemeldete Eigenschaften, um die Projektmappe über den Fortschritt des Updates zu benachrichtigen.

Methoden werden auf Ihre Brombeere Pi über das Dashboard Projektmappen aufrufen. Wenn die Brombeere Pi zuerst mit der remote-überwachungslösung verbunden ist, sendet er Informationen zu den unterstützten Methoden aus. 

[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md

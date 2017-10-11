## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Die Brombeere Pi sendet Telemetrie ist jetzt an der remote-überwachungslösung. Sie können die Telemetrie im Projektmappen-Dashboard anzeigen. Sie können auch Nachrichten an Ihre Brombeere Pi über das Dashboard Solution senden.

- Navigieren Sie zu der Projektmappe-Dashboard.
- Wählen Sie Ihr Gerät bei der **Gerät anzuzeigende** Dropdownliste.
- Die Telemetriedaten aus der Brombeere Pi werden auf dem Dashboard angezeigt.

![Anzeigen von Telemetriedaten aus der Brombeere Pi][img-telemetry-display]

## <a name="act-on-the-device"></a>Wirken sich auf dem Gerät

Über das Dashboard Lösung können Sie auf Ihre Brombeere Pi Methoden aufrufen. Wenn mit der remote-überwachungslösung der Brombeere Pi verbunden ist, sendet er Informationen zu den unterstützten Methoden aus.

- Klicken Sie im Projektmappen-Dashboard auf **Geräte** Besuchen der **Geräte** Seite. Wählen Sie Ihre Brombeere Pi in der **Geräteliste**. Wählen Sie dann **Methoden**:

    ![Liste der Geräte im dashboard][img-list-devices]

- Auf der **Aufrufmethode** Seite **LightBlink** in der **Methode** Dropdownliste.

- Wählen Sie **InvokeMethod**. Die LED, die mit der Pi Brombeere blinkt mehrmals verbunden. Die app auf die Brombeere Pi sendet eine Bestätigung zurück zum Dashboard Lösung:

    ![Verlauf der anzeigen-Methode][img-method-history]

- Können Sie wechseln die LED ein- und ausschalten mithilfe der **ChangeLightStatus** Methode mit einem **LightStatusValue** festgelegt **1** für auf oder **0** für deaktiviert.

> [!WARNING]
> Wenn Sie die remote-überwachungslösung in Ihrem Azure-Konto ausführen lassen, sind Sie für die Zeit in Rechnung gestellt, die er ausgeführt wird. Weitere Informationen zu reduzieren Verbrauch, während der remote-überwachungslösung ausgeführt wird, finden Sie unter [Konfigurieren von Azure IoT Suite vorkonfigurierter Lösungen zu Demonstrationszwecken][lnk-demo-config]. Löschen Sie die vorkonfigurierte Lösung aus Ihrem Azure-Konto ein, wenn Sie ihn nicht mehr benötigen.


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
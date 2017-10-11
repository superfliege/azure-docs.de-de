## <a name="view-device-telemetry-in-the-dashboard"></a>Gerätetelemetrie im Dashboard anzeigen
Das Dashboard in der remote-überwachungslösung können Sie die Telemetrie anzeigen, die von die Ihren Geräten mit IoT Hub gesendet.

1. Klicken Sie in Ihrem Browser zurück an die remote Lösung-überwachungsdashboard **Geräte** im linken Bereich, zu dem navigiert der **Geräteliste**.
2. In der **Geräteliste**, sehen Sie, dass der Status Ihres Geräts **ausführen**. Wenn dies nicht der Fall, klicken Sie auf **Gerät aktivieren** in der **Gerätedetails** Bereich.
   
    ![Anzeigen von Gerätestatus][18]
3. Klicken Sie auf **Dashboard** um zum Dashboard zurückzukehren, wählen Sie Ihr Gerät bei der **Gerät anzuzeigende** Dropdown-Liste für die Telemetrie anzeigen. Die Telemetriedaten aus der beispielanwendung ist 50 Einheiten für die interne Temperatur, 55 Einheiten für externe Temperatur und Luftfeuchtigkeit 50 Einheiten.
   
    ![Gerätetelemetrie anzeigen][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>Aufrufen einer Methode auf Ihrem Gerät
Das Dashboard in der remote-überwachungslösung können Sie zum Aufrufen von Methoden auf Ihren Geräten durch IoT Hub. Beispielsweise können Sie eine Methode zum Simulieren ein Geräts Neustart in der remote-überwachungslösung aufrufen.

1. Klicken Sie in der remote Lösung-überwachungsdashboard auf **Geräte** im linken Bereich zu navigieren der **Geräteliste**.
2. Klicken Sie auf **Geräte-ID** für Ihr Gerät in der **Geräteliste**.
3. In der **Gerätedetails** auf **Methoden**.
   
    ![Geräte-Methoden][13]
4. In der **Methode** Dropdownliste, wählen **InitiateFirmwareUpdate**, und klicken Sie dann im **FWPACKAGEURI** Geben Sie eine dummy-URL. Klicken Sie auf **Aufrufmethode** zum Aufrufen der Methode auf dem Gerät.
   
    ![Rufen Sie eine Geräte-Methode][14]
   

5. Sie sehen eine Meldung in der Konsole Codes Gerät ausführen, wenn das Gerät von der Methode verarbeitet. Der Verlauf im Portal Lösung werden die Ergebnisse der Methode hinzugefügt:

    ![Verlauf der anzeigen-Methode][img-method-history]

## <a name="next-steps"></a>Nächste Schritte
Der Artikel [anpassen vorkonfigurierter Lösungen] [ lnk-customize] Beschreibung einiger Unterschiede, die Sie erweitern können Sie dieses Beispiel. Mögliche Erweiterungen sind die Verwendung von realer Sensoren und zusätzliche Befehle implementieren.

Sie können erfahren Sie mehr über die [Berechtigungen für die Website azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md

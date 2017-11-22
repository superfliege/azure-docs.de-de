Eine der wichtigen Funktionen von Azure IoT Edge ist die Möglichkeit, Module aus der Cloud auf IoT Edge-Geräten bereitzustellen. Ein IoT-Edge-Modul ist ein ausführbares Paket, das als Container implementiert wird. In diesem Abschnitt stellen Sie ein Modul bereit, das Telemetrie für das simulierte Gerät generiert. 

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.
1. Wechseln Sie zu **IoT Edge (Vorschau)**, und wählen Sie Ihr IoT Edge-Gerät aus.
1. Wählen Sie **Module festlegen** aus.
1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
1. Geben Sie im Feld **Name** die Zeichenfolge `tempSensor` ein. 
1. Geben Sie im Feld **Image-URI** die Zeichenfolge `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` ein. 
1. Behalten Sie die restlichen Einstellungen unverändert bei, und wählen Sie **Speichern** aus.
1. Wenn Sie zum Schritt **Module hinzufügen** zurückgekehrt sind, wählen Sie **Weiter** aus.
1. Wählen Sie im Schritt **Routen angeben** die Option **Weiter** aus.
1. Wählen Sie im Schritt **Vorlage überprüfen** die Option **Senden** aus.
1. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie **Aktualisieren** aus. Daraufhin sollte das neue tempSensor-Modul angezeigt werden, das zusammen mit der IoT Edge-Runtime ausgeführt wird. 

   ![Anzeigen von tempSensor in der Liste der bereitgestellten Module][1]

<!-- Images -->
[1]: ../articles/iot-edge/media/tutorial-simulate-device-windows/view-module.png
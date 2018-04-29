## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Nachdem Sie nun einen IoT Hub erstellt haben, können Sie nach den wichtigen Informationen suchen, die Sie zum Herstellen einer Verbindung für Geräte und Anwendungen mit Ihrem IoT Hub nutzen. 

1. Klicken Sie im Dashboard nach Abschluss des Erstellungsvorgangs auf den erstellten IoT Hub. Notieren Sie sich den **Hostnamen**, und klicken Sie anschließend auf **Richtlinien für gemeinsamen Zugriff**.

   ![Abrufen des Hostnamens Ihres IoT Hubs](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

1. Klicken Sie im Bereich **Freigegebene Zugriffsrichtlinien** auf die Richtlinie **iothubowner**, und kopieren und notieren Sie dann die **Verbindungszeichenfolge** Ihres IoT Hubs. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf IoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
Die iothubowner-Verbindungszeichenfolge wird für dieses Tutorial nicht benötigt. Sie benötigen sie allerdings unter Umständen für einige Tutorials zu anderen IoT-Szenarien, nachdem Sie diese Einrichtung abgeschlossen haben.

   ![Abrufen der IoT Hub-Verbindungszeichenfolge](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Registrieren eines Geräts beim IoT Hub für Ihr Gerät

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) Ihren IoT-Hub.

2. Klicken Sie auf **IoT-Geräte**.
3. Klicken Sie im Bereich „IoT-Geräte“ auf **Hinzufügen**, um Ihrem IoT Hub ein Gerät hinzuzufügen. Gehen Sie wie folgt vor:

   **Geräte-ID**: Geben Sie die ID des neuen Geräts ein. Bei Geräte-IDs wird die Groß-/Kleinschreibung beachtet.

   **Authentifizierungstyp**: Wählen Sie die Option **Symmetrischer Schlüssel**.

   **Schlüssel automatisch generieren**: Aktivieren Sie dieses Kontrollkästchen.

   **Connect device to IoT Hub** (Gerät mit IoT Hub verbinden): Klicken Sie auf **Aktivieren**.

   ![Hinzufügen eines Geräts im Bereich „IoT-Geräte“ Ihres IoT Hubs](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-iot-devices-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Klicken Sie auf **Speichern**.
5. Öffnen Sie das Gerät nach der Erstellung im Bereich **IoT-Geräte**.

   ![Liste der IoT-Geräte in IoT Hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_device-list-in-iot-devices-portal.png)

6. Notieren Sie sich den Primärschlüssel der Verbindungszeichenfolge.

   ![Abrufen der Geräte-Verbindungszeichenfolge](../articles/iot-hub/media/iot-hub-create-hub-and-device/8_get-device-connection-string-in-iot-devices-portal.png)

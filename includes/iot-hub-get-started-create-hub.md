## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs
Erstellen Sie einen IoT-Hub, mit dem Ihre simulierte Geräte-App verbunden werden kann. Die folgenden Schritte veranschaulichen, wie Sie diese Aufgabe mit dem Azure-Portal ausführen.

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Nachdem Sie nun einen IoT Hub erstellt haben, können Sie nach den wichtigen Informationen suchen, die Sie zum Herstellen einer Verbindung für Geräte und Anwendungen mit Ihrem IoT Hub nutzen. 

1. Wenn der IoT Hub erfolgreich erstellt wurde, klicken Sie im Azure-Portal auf die neue Kachel für Ihren IoT Hub, um das Eigenschaftenfenster für den neuen IoT Hub zu öffnen. Notieren Sie sich den **Hostnamen**, und klicken Sie anschließend auf **Richtlinien für gemeinsamen Zugriff**.
   
    ![Neues IoT Hub-Fenster][4]
1. Klicken Sie unter **Richtlinien für gemeinsamen Zugriff** auf die Richtlinie **iothubowner**, kopieren Sie die im Fenster **iothubowner** angegebene IoT Hub-Verbindungszeichenfolge, und notieren Sie sie. Weitere Informationen finden Sie unter [Zugriffssteuerung][lnk-access-control] im „Entwicklungsleitfaden für Azure IoT Hub“.
   
    ![Freigegebene Zugriffsrichtlinien][5]

<!-- Images. -->
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md

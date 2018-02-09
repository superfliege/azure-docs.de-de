## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs
Erstellen Sie einen IoT-Hub, mit dem Ihre simulierte Geräte-App verbunden werden kann. Die folgenden Schritte veranschaulichen, wie Sie diese Aufgabe mit dem Azure-Portal ausführen.

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an.

1. Klicken Sie auf **Neu** > **Internet der Dinge** > **IoT Hub**.
   
    ![Navigationsleiste im Azure-Portal][1]

1. Geben Sie im Bereich **IoT Hub** die folgenden Informationen für Ihren IoT Hub ein:

   * **Name**: Erstellen Sie einen Namen für Ihren IoT-Hub. Wenn der eingegebene Name gültig ist, wird ein grünes Häkchen angezeigt.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   * **Tarif und Skalierung**: Wählen Sie für dieses Tutorial den Tarif **F1 – Free** aus. Weitere Informationen hierzu finden Sie unter [Tarif und Skalierung][lnk-pricing].

   * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe zum Hosten des IoT Hubs, oder verwenden Sie eine vorhandene. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen][lnk-resource-groups].

   * **Standort**: Wählen Sie den nächstgelegenen Standort aus.

   * **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.

    ![Fenster „IoT Hub“][2]

1. Klicken Sie auf **Create**. Die Erstellung des IoT Hubs kann mehrere Minuten dauern. Sie können den Fortschritt im Bereich **Benachrichtigungen** überwachen.

1. Wenn Ihre neue IoT Hub-Instanz bereit ist, klicken Sie im Azure-Portal auf die entsprechende Kachel, um das Eigenschaftenfenster zu öffnen. Nachdem Sie nun einen IoT Hub erstellt haben, können Sie nach den wichtigen Informationen suchen, die Sie zum Herstellen einer Verbindung für Geräte und Anwendungen mit Ihrem IoT Hub nutzen. Notieren Sie sich den **Hostnamen**, und klicken Sie anschließend auf **Richtlinien für gemeinsamen Zugriff**.
   
    ![Neues IoT Hub-Fenster][4]

1. Klicken Sie unter **Richtlinien für gemeinsamen Zugriff** auf die Richtlinie **iothubowner**, und notieren Sie sich die im Fenster **iothubowner** angegebene IoT Hub-Verbindungszeichenfolge. Weitere Informationen finden Sie unter [Zugriffssteuerung][lnk-access-control] im „Entwicklungsleitfaden für Azure IoT Hub“.
   
    ![Freigegebene Zugriffsrichtlinien][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md

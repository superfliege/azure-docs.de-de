## <a name="view-the-solution-dashboard"></a>Zeigen Sie das Dashboard für die Lösung

Die Lösung Dashboard können Sie die bereitgestellte Projektmappe verwalten. Sie können z. B. Telemetrie anzeigen, Hinzufügen von Geräten und Methoden aufrufen.

1. Wenn die Bereitstellung abgeschlossen ist, und gibt an, die Kachel für Ihre vorkonfigurierte Lösung **bereit**, wählen Sie **starten** Ihre Überwachung Lösung Remoteportal in einer neuen Registerkarte geöffnet.

    ![Starten Sie die vorkonfigurierte Lösung][img-launch-solution]

1. Standardmäßig zeigt das Portal für die Lösung die *Dashboard*. Sie können zu anderen Bereichen des Portals Lösung mithilfe des Menüs auf der linken Seite der Seite navigieren.

    ![Remote vorkonfigurierte Lösung überwachungsdashboard][img-menu]

## <a name="add-a-device"></a>Hinzufügen eines Geräts

Für ein Gerät zum Verbinden mit der vorkonfigurierte Lösung muss er sich gegenüber IoT Hub mit gültigen Anmeldeinformationen identifizieren. Sie können die Anmeldeinformationen des Geräts über das Dashboard Solution abrufen. Sie enthalten die Anmeldeinformationen des Geräts in Ihrer Clientanwendung weiter unten in diesem Lernprogramm.

Wenn Sie dies nicht bereits getan haben Hinzufügen eines benutzerdefinierten Geräts zur Projektmappe remote überwachen. Die folgenden Schritte im Projektmappen-Dashboard:

1. Klicken Sie in der unteren linken Ecke des Dashboards auf **Hinzufügen eines Geräts**.

   ![Hinzufügen eines Geräts][1]

1. In der **benutzerdefinierte Gerät** auf **Add new**.

   ![Hinzufügen eines benutzerdefinierten Geräts][2]

1. Wählen Sie **ich möchte meine eigene Geräte-ID definieren**. Geben Sie eine Geräte-ID z. B. **Rasppi**, klicken Sie auf **überprüfen ID** überprüfen Sie den Namen noch nicht bereits in der Projektmappe verwendet, und klicken Sie dann auf **erstellen** auf das Gerät bereitstellen.

   ![Geräte-ID hinzufügen][3]

1. Notieren Sie sich die Anmeldeinformationen des Geräts (**Geräte-ID**, **IoT Hub-Hostname**, und **Geräteschlüssel**). Die Clientanwendung auf die Brombeere Pi benötigt diese Werte zur Verbindung mit der remote-überwachungslösung. Klicken Sie dann auf **Fertig**.

    ![Anmeldeinformationen des Geräts anzeigen][4]

1. Wählen Sie Ihr Gerät in der Geräteliste im Projektmappen-Dashboard. Klicken Sie auf die **Gerätedetails** auf **Gerät aktivieren**. Der Status Ihres Geräts ist jetzt **ausführen**. Der remote-überwachungslösung kann jetzt erhalten von Ihrem Gerät Telemetrie und Aufrufen von Methoden auf dem Gerät.

[img-launch-solution]: media/iot-suite-raspberry-pi-kit-view-solution/launch.png
[img-menu]: media/iot-suite-raspberry-pi-kit-view-solution/menu.png
[1]: media/iot-suite-raspberry-pi-kit-view-solution/suite0.png
[2]: media/iot-suite-raspberry-pi-kit-view-solution/suite1.png
[3]: media/iot-suite-raspberry-pi-kit-view-solution/suite2.png
[4]: media/iot-suite-raspberry-pi-kit-view-solution/suite3.png
> [!div class="op_single_selector"]
> * [C unter Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C unter Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a>Szenarioübersicht
In diesem Szenario erstellen Sie ein Gerät, das die folgenden Telemetrie an die Remoteüberwachung sendet [vorkonfigurierte Lösung][lnk-what-are-preconfig-solutions]:

* Externe Temperatur
* Temperatur
* Luftfeuchtigkeit

Der Einfachheit halber Beispielwerte durch den Code auf dem Gerät wird, aber wir empfehlen Ihnen, das Beispiel zu erweitern, indem Sie echte Sensoren auf Ihrem Gerät verbinden und Senden von Telemetriedaten real.

Das Gerät kann auch auf Methoden, die aufgerufen wird, aus der Lösung Dashboard und die gewünschten Werte Eigenschaftensatz im Projektmappen-Dashboard zu reagieren.

Um dieses Lernprogramm abzuschließen, benötigen Sie ein aktives Azure-Konto. Wenn Sie keines Konto haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion][lnk-free-trial].

## <a name="before-you-start"></a>Vorbereitung
Bevor Sie für Ihr Gerät keinen Code schreiben, müssen Sie Ihre remote überwachen vorkonfigurierte Lösung bereitstellen und Bereitstellen ein neues benutzerdefiniertes Geräts in dieser Projektmappe.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Bereitstellen Sie Ihrer remote vorkonfigurierten überwachungslösung
Das Gerät, das Sie in diesem Lernprogramm erstellen sendet Daten an eine Instanz von der [Remoteüberwachung] [ lnk-remote-monitoring] vorkonfigurierte Lösung. Wenn Sie in Ihrem Azure-Konto bereits remote vorkonfigurierten überwachungslösung bereitgestellt dies nicht getan haben, verwenden Sie die folgenden Schritte aus:

1. Auf der <https://www.azureiotsuite.com/> auf  **+**  um eine Projektmappe zu erstellen.
2. Klicken Sie auf **wählen** auf die **Remoteüberwachung** Bereich, um die Projektmappe zu erstellen.
3. Auf der **Remote überwachungslösung erstellen** geben eine **Projektmappenname** Ihrer Wahl ein, wählen Sie aus der **Region** soll, und wählen Sie das Azure-Abonnement verwendet werden soll. Klicken Sie dann auf **Lösung erstellen**.
4. Warten Sie, bis der Bereitstellungsvorgang abgeschlossen ist.

> [!WARNING]
> Die vorkonfigurierten Lösungen werden abrechenbare Azure-Dienste verwenden. Achten Sie darauf, dass Sie die vorkonfigurierte Lösung aus dem Abonnement entfernen, Sie abschließend mit ihm alle unnötige Gebühren zu vermeiden. Sie können eine vorkonfigurierte Lösung vollständig aus dem Abonnement entfernen, auf die <https://www.azureiotsuite.com/> Seite.
> 
> 

Wenn im Rahmen des Bereitstellungsprozesses für die remote-überwachungslösung abgeschlossen ist, klicken Sie auf **starten** um das Dashboard für die Lösung in Ihrem Browser zu öffnen.

![Projektmappen-dashboard][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Bereitstellen Sie Ihres Geräts bei der remote-überwachungslösung
> [!NOTE]
> Wenn Sie in der Projektmappe bereits ein Gerät bereitgestellt haben, können Sie diesen Schritt überspringen. Sie müssen die Anmeldeinformationen des Geräts kennen, wenn Sie die Clientanwendung erstellen.
> 
> 

Für ein Gerät zum Verbinden mit der vorkonfigurierte Lösung muss er sich gegenüber IoT Hub mit gültigen Anmeldeinformationen identifizieren. Sie können die Anmeldeinformationen des Geräts über das Dashboard Solution abrufen. Sie enthalten die Anmeldeinformationen des Geräts in Ihrer Clientanwendung weiter unten in diesem Lernprogramm.

Um ein Gerät remote überwachen Projektmappe hinzuzufügen, führen Sie die folgenden Schritte aus, in der Projektmappe Dashboard:

1. Klicken Sie in der unteren linken Ecke des Dashboards auf **Hinzufügen eines Geräts**.
   
   ![Hinzufügen eines Geräts][1]
2. In der **benutzerdefinierte Gerät** auf **Add new**.
   
   ![Hinzufügen eines benutzerdefinierten Geräts][2]
3. Wählen Sie **ich möchte meine eigene Geräte-ID definieren**. Geben Sie eine Geräte-ID z. B. **Mydevice**, klicken Sie auf **überprüfen ID** diesem Namen ist bereits in Verwendung nicht überprüfen, und klicken Sie dann auf **erstellen** auf das Gerät bereitstellen.
   
   ![Geräte-ID hinzufügen][3]
4. Notieren Sie sich die Anmeldeinformationen des Geräts (Geräte-ID, IoT Hub-Hostname und Geräteschlüssel). Ihre Clientanwendung benötigt diese Werte zur Verbindung mit der remote-überwachungslösung. Klicken Sie dann auf **Fertig**.
   
    ![Anmeldeinformationen des Geräts anzeigen][4]
5. Wählen Sie Ihr Gerät in der Geräteliste im Projektmappen-Dashboard. Klicken Sie auf die **Gerätedetails** auf **Gerät aktivieren**. Der Status Ihres Geräts ist jetzt **ausführen**. Der remote-überwachungslösung kann jetzt erhalten von Ihrem Gerät Telemetrie und Aufrufen von Methoden auf dem Gerät.

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
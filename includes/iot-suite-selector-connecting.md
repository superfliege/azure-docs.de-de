> [!div class="op_single_selector"]
> * [C unter Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C unter Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js (generisch)](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> * [Node.js auf Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-node.md)
> * [C auf Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-c.md)

In diesem Tutorial implementieren Sie ein **Kühlgerät**, das die folgenden Telemetriedaten an die [vorkonfigurierte Lösung](../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md) für die Remoteüberwachung sendet:

* Temperatur
* Pressure
* Luftfeuchtigkeit

Der Einfachheit halber generiert der Code Beispieltelemetriewerte für den **Kühler**. Sie können das Beispiel noch erweitern, indem Sie echte Sensoren an Ihr Gerät anschließen und echte Telemetriedaten senden.

Das Beispielgerät führt auch folgende Vorgänge durch:

* Sendet Metadaten an die Lösung, um die jeweiligen Funktionen zu beschreiben
* Antwortet auf Aktionen, die in der Lösung von der Seite **Geräte** ausgelöst wurden
* Antwortet auf Konfigurationsänderungen, die von der Seite **Geräte** in der Lösung gesendet werden

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Konto. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Vorbereitung

Bevor Sie Code für Ihr Gerät schreiben, stellen Sie Ihre vorkonfigurierte Remoteüberwachungslösung bereit, und fügen Sie der Lösung ein neues physisches Gerät hinzu.

### <a name="deploy-your-remote-monitoring-preconfigured-solution"></a>Bereitstellen der vorkonfigurierten Remoteüberwachungslösung

Das von Ihnen in diesem Tutorial erstellte **Kühlgerät** sendet Daten an eine Instanz der vorkonfigurierten Lösung für die [Remoteüberwachung](../articles/iot-suite/iot-suite-remote-monitoring-explore.md). Wenn Sie die vorkonfigurierte Lösung für die Remoteüberwachung noch nicht in Ihrem Azure-Konto bereitgestellt haben, lesen Sie den Artikel [Deploy the remote monitoring preconfigured solution](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md) (Bereitstellen der vorkonfigurierten Lösung für die Remoteüberwachung).

Wenn der Bereitstellungsvorgang für die Remoteüberwachungslösung abgeschlossen ist, klicken Sie auf **Starten**, um das Lösungsdashboard im Browser zu öffnen.

![Das Lösungsdashboard](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Hinzufügen des Geräts zur Remoteüberwachungslösung

> [!NOTE]
> Wenn Sie der Lösung bereits ein Gerät hinzugefügt haben, können Sie diesen Schritt überspringen. Für den nächsten Schritt ist jedoch die Verbindungszeichenfolge Ihres Geräts erforderlich. Sie können die Verbindungszeichenfolge eines Geräts im [Azure-Portal](https://portal.azure.com) oder über das CLI-Tool [az iot](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) abrufen.

Damit ein Gerät eine Verbindung mit der vorkonfigurierten Lösung herstellen kann, muss es sich mit gültigen Anmeldeinformationen bei IoT Hub identifizieren können. Sie haben die Möglichkeit, die Verbindungszeichenfolge mit den Anmeldeinformationen zu speichern, wenn Sie das Gerät der Lösung hinzufügen. Die Geräte-Verbindungszeichenfolge wird zu einem späteren Zeitpunkt in diesem Lernprogramm in Ihre Clientanwendung einbezogen.

Um Ihrer Lösung für die Remoteüberwachung ein Gerät hinzuzufügen, führen Sie in der Lösung auf der Seite **Geräte** die folgenden Schritte durch:

1. Wählen Sie **+ Neues Gerät** und dann **Physisch** als **Gerätetyp** aus:

    ![Hinzufügen eines physischen Geräts](media/iot-suite-selector-connecting/devicesprovision.png)

1. Geben Sie als Geräte-ID **Physical-chiller** ein. Wählen Sie die Optionen **Symmetrischer Schlüssel** und **Schlüssel automatisch generieren**:

    ![Auswählen von Geräteoptionen](media/iot-suite-selector-connecting/devicesoptions.png)

1. Wählen Sie **Anwenden** aus. Notieren Sie sich dann die Werte in den Feldern **Geräte-ID**, **Primärschlüssel** und **Verbindungszeichenfolge – Primärschlüssel**:

    ![Abrufen der Anmeldeinformationen](media/iot-suite-selector-connecting/credentials.png)

Sie haben jetzt der vorkonfigurierten Remoteüberwachungslösung ein physisches Gerät hinzugefügt und dessen Verbindungszeichenfolge notiert. In den folgenden Abschnitten implementieren Sie die Clientanwendung, die mithilfe der Verbindungszeichenfolge des Geräts eine Verbindung mit Ihrer Lösung herstellt.

Die Clientanwendung implementiert das integrierte Gerätemodell des **Kühlers**. Das Gerätemodell einer vorkonfigurierten Lösung gibt folgende Informationen im Zusammenhang mit einem Gerät an:

* Die Eigenschaften, die das Gerät der Lösung meldet. Beispiel: Ein **Kühlgerät** meldet Informationen zur Firmware und zum Standort.
* Die Arten von Telemetriedaten, die das Gerät an die Lösung sendet. Beispiel: Ein **Kühlgerät** sendet Temperatur-, Luftfeuchtigkeits- und Druckwerte.
* Die Methoden, die Sie mit der Lösung zur Ausführung auf dem Gerät planen können. Beispiel: Ein **Kühlgerät** muss die Methoden **Reboot**, **FirmwareUpdate**, **EmergencyValveRelease** und  **IncreasePressure** implementieren.
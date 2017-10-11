## <a name="configure-the-nodejs-simulated-device"></a>Konfigurieren Sie das Node.js simulierte Gerät
1. Klicken Sie auf das entfernte überwachungsdashboard auf **+ Hinzufügen eines Geräts** und fügen Sie dann eine *benutzerdefinierte*. Notieren Sie sich den IoT Hub Hostname, Geräte-Id und Geräteschlüssel. Sie benötigen sie später in diesem Lernprogramm, wenn Sie die Clientanwendung für remote_monitoring.js-Gerät vorbereiten.
2. Stellen Sie sicher, dass Version Node.js 0.12.x oder höher auf dem Entwicklungscomputer installiert ist. Führen Sie `node --version` an der Eingabeaufforderung oder in einer Shell auf die Version zu überprüfen. Informationen zur Verwendung von Paket-Manager zum Installieren von Node.js unter Linux finden Sie unter [Installation von Node.js über Paket-Manager][node-linux].
3. Bei der Installation von Node.js Klonen Sie die neueste Version der [Azure Iot-Sdk-Knoten] [ lnk-github-repo] Repository auf Ihrem Entwicklungscomputer. Verwenden Sie immer die **master** Verzweigung für die neueste Version der Bibliotheken und Beispiele.
4. Aus der lokalen Kopie der [Azure Iot-Sdk-Knoten] [ lnk-github-repo] Repository, die folgenden beiden Dateien kopieren von Knoten/Gerät/Beispielordner zu einem leeren Ordner auf dem Entwicklungscomputer:
   
   * Packages.JSON
   * remote_monitoring.js
5. Öffnen Sie die Datei remote_monitoring.js, und suchen Sie nach der Definition der folgenden Variablen:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Ersetzen Sie **[IoT Hub Gerät-Verbindungszeichenfolge]** mit Ihrem Gerät-Verbindungszeichenfolge. Verwenden Sie die Werte für Ihre IoT Hub-Hostname, Geräte-Id und Geräteschlüssel, die Sie in Schritt 1 notiert vorgenommen. Eine Verbindungszeichenfolge Gerät weist das folgende Format:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Wenn Ihre IoT Hub-Hostname ist **Contoso** und die Geräte-Id ist **Mydevice**, sieht wie folgt der folgende Ausschnitt aus die Verbindungszeichenfolge:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Speichern Sie die Datei. Führen Sie die folgenden Befehle in einer Shell oder eine Eingabeaufforderung in den Ordner mit diesen Dateien zum Installieren der erforderlichen Pakete und führen Sie die beispielanwendung aus:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Beachten Sie die dynamische Telemetrie in Aktion
Das Dashboard zeigt die Temperatur und Luftfeuchtigkeit Telemetrie aus den vorhandenen simulierten Geräten:

![Der Standard-dashboard][image1]

Wenn Sie das Node.js simulierte Gerät, das Sie im vorherigen Abschnitt ausgeführt haben auswählen, sehen Sie Temperatur, Luftfeuchtigkeit und externen Temperatur Telemetrie:

![Externe Temperatur zum Dashboard hinzufügen.][image2]

Der remote-überwachungslösung automatisch erkennt den zusätzlichen externen Temperatur Telemetrie-Typ und das Diagramm auf dem Dashboard hinzugefügt.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png
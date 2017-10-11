## <a name="build-iot-edge"></a>Erstellen des IoT-Edge

Dieses Lernprogramm verwendet benutzerdefinierte IoT-Edge-Module für die Kommunikation mit der remote vorkonfigurierten überwachungslösung. Aus diesem Grund müssen Sie den Rand IoT Module aus benutzerdefinierten Quellcode erstellen. In den folgenden Abschnitten wird beschrieben, wie zum Installieren von IoT Rand, und erstellen Sie das benutzerdefinierte IoT-Edge-Modul.

### <a name="install-iot-edge"></a>Installieren Sie IoT-Edge

Die folgenden Schritte beschreiben, wie die vorkompilierte IoT-Edge-Software auf der Intel-NUC installieren:

1. Konfigurieren Sie den erforderlichen intelligenten Paket Repositorys durch Ausführen der folgenden Befehle auf der Intel-NUC:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Geben Sie `y` Wenn der Befehl fordert Sie auf **über diesen Kanal enthalten?**.

1. Aktualisieren Sie den intelligenten Paket-Manager durch Ausführen des folgenden Befehls:

    ```bash
    smart update
    ```

1. Installieren Sie das Azure IoT-Edge-Paket, durch den folgenden Befehl ausführen:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. Überprüfen Sie die Installation durch Ausführen des Beispiels "Hello World" ein. In diesem Beispiel schreibt eine Hello World-Nachricht in die Datei log.txT alle fünf Sekunden. Die folgenden Befehle führen Sie das Beispiel "Hello World":

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Alle ignorieren **Ungültiges Argument** Nachrichten, wenn Sie das Beispiel zu beenden.

    Verwenden Sie zum Anzeigen des Inhalts der Protokolldatei den folgenden Befehl ein:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Problembehandlung

Wenn der Fehler "kein Paket bietet Util-Linux-Dev" angezeigt wird, wiederholen Sie den Neustart der Intel-NUC.

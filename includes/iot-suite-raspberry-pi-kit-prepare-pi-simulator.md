## <a name="prepare-your-raspberry-pi"></a>Vorbereiten Sie Ihrer Brombeere Pi

### <a name="install-raspbian"></a>Installieren von Raspbian

Ist dies das erste Mal Sie Ihre Brombeere Pi verwenden, müssen Sie zum Installieren der Raspbian Betriebssystems mit NOOBS auf die SD-Karte im Kit enthalten. Die [Brombeere Pi Software Handbuch] [ lnk-install-raspbian] wird beschrieben, wie ein Betriebssystem auf Ihre Brombeere Pi zu installieren. In diesem Lernprogramm wird davon ausgegangen, dass Sie das Betriebssystem Raspbian auf Ihre Brombeere Pi installiert haben.

> [!NOTE]
> Die SD-Karte in enthalten die [Microsoft Azure IoT Starter Kit für Brombeere Pi 3] [ lnk-starter-kits] bereits NOOBS installiert wurde. Sie können starten die Brombeere Pi aus dieser Smartcard und so installieren Sie das Betriebssystem Raspbian auswählen.

Um die Hardware-Einrichtung abzuschließen, müssen Sie:

- Verbinden Sie Ihre Brombeere Pi die Stromversorgung erneut in das Kit enthalten.
- Verbinden Sie Ihre Brombeere Pi mit Ihrem Netzwerk mit der Ethernet-Kabel, die in Ihrem Kit enthalten. Alternativ können Sie einrichten [Wireless-Konnektivität] [ lnk-pi-wireless] für Ihre Brombeere Pi.

Sie haben jetzt die Hardware-Einrichtung Ihrer Brombeere Pi abgeschlossen.

### <a name="sign-in-and-access-the-terminal"></a>Melden Sie an und Zugreifen auf den Terminalserver

Sie haben zwei Möglichkeiten, die auf eine Terminaldienste-Umgebung auf Ihre Brombeere Pi zuzugreifen:

- Wenn Sie eine Tastatur und einen Monitor, die mit der Pi Brombeere verbunden haben, können Sie die Raspbian GUI verwenden, auf ein terminal-Fenster zuzugreifen.

- Auf zugreifen Sie der Befehlszeile auf Ihre Brombeere Pi mithilfe von SSH vom desktop-Computer.

#### <a name="use-a-terminal-window-in-the-gui"></a>Verwenden Sie ein terminal-Fenster auf der grafischen Benutzeroberfläche

Die Standardanmeldeinformationen für Raspbian werden Benutzername **Pi** und das Kennwort **Brombeere**. In der Taskleiste in der Benutzeroberfläche, starten Sie den **Terminaldienste** Dienstprogramm mit dem Symbol, das einen Monitor aussieht.

#### <a name="sign-in-with-ssh"></a>Melden Sie sich mit SSH

Sie können SSH für befehlszeilengestützten Zugriff auf Ihre Brombeere Pi verwenden. Der Artikel [SSH (Secure Shell)] [ lnk-pi-ssh] beschreibt, wie zum Konfigurieren von SSH auf Ihre Brombeere Pi und Herstellen einer Verbindung über [Windows] [ lnk-ssh-windows] oder [Linux und Mac OS][lnk-ssh-linux].

Melden Sie sich mit Benutzernamen **Pi** und das Kennwort **Brombeere**.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>Optional: Freigabeordner auf Ihre Brombeere Pi

Optional: Sie möchten einen Ordner auf Ihrem Brombeere Pi mit Ihrer desktop-Umgebung gemeinsam nutzen. Freigeben eines Ordners können Sie Ihre bevorzugte desktop Texteditor (z. B. [Visual Studio Code](https://code.visualstudio.com/) oder [Sublime Text](http://www.sublimetext.com/)) zum Bearbeiten von Dateien auf Ihrem Brombeere Pi anstatt `nano` oder `vi`.

Konfigurieren Sie zum Freigeben eines Ordners in Windows einen Samba-Server, auf die Brombeere Pi. Alternativ können Sie verwenden die integrierte [SFTP](https://www.raspberrypi.org/documentation/remote-access/) Server mit einem SFTP-Client auf dem Desktop.

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
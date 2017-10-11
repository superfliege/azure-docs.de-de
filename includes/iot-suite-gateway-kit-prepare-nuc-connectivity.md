## <a name="prepare-your-intel-nuc"></a>Vorbereiten Sie Ihrer Intel NUC

Um die Hardware-Einrichtung abzuschließen, müssen Sie:

- Verbinden Sie Ihre Intel NUC die Stromversorgung erneut in das Kit enthalten.
- Verbinden Sie Ihre Intel NUC mit Ihrem Netzwerk über ein Ethernetkabel.

Sie haben jetzt die Hardwarekonfiguration des Geräts Gateway Intel NUC abgeschlossen.

### <a name="sign-in-and-access-the-terminal"></a>Melden Sie an und Zugreifen auf den Terminalserver

Sie haben zwei Möglichkeiten, die auf eine Terminaldienste-Umgebung auf Ihre Intel NUC zuzugreifen:

- Wenn Sie eine Tastatur und einen Monitor, die mit der Intel-NUC verbunden haben, können Sie die Shell direkt zugreifen. Die Standardanmeldeinformationen sind Benutzernamen **Root** und das Kennwort **Root**.

- Zugriff auf die Befehlsshell auf Ihrem Intel NUC mithilfe von SSH vom desktop-Computer.

#### <a name="sign-in-with-ssh"></a>Melden Sie sich mit SSH

Um sich mit SSH anmelden, benötigen Sie die IP-Adresse Ihrer NUC Intel. Wenn Sie eine Tastatur und einen Monitor, die mit der Intel-NUC verbunden haben, verwenden Sie die `ifconfig` Befehl aus, um die IP-Adresse gefunden. Verbinden Sie alternativ mit Ihren Router, um die Adressen von Geräten in Ihrem Netzwerk aufzulisten.

Melden Sie sich mit Benutzernamen **Root** und das Kennwort **Root**.

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a>Optional: Freigabeordner auf Ihre Intel NUC

Optional: Sie möchten einen Ordner auf Ihrem Intel NUC mit Ihrer desktop-Umgebung gemeinsam nutzen. Freigeben eines Ordners können Sie Ihre bevorzugte desktop Texteditor (z. B. [Visual Studio Code](https://code.visualstudio.com/) oder [Sublime Text](http://www.sublimetext.com/)) zum Bearbeiten von Dateien auf Ihrem Intel NUC statt `nano` oder `vi`.

Um einen Ordner mit Windows freizugeben, konfigurieren Sie einen Samba-Server, auf der Intel-NUC. Alternativ verwenden Sie die SFTP-Server, auf der Intel-NUC mit einem SFTP-Client auf dem Desktopcomputer.

## <a name="overview"></a>Übersicht

In diesem Lernprogramm führen Sie die folgenden Schritte aus:

- Stellen Sie eine Instanz der remote überwachen vorkonfigurierte Lösung, mit der Azure-Abonnement. Dieser Schritt wird automatisch bereitgestellt und konfiguriert mehrere Azure-Dienste.
- Richten Sie Ihr Gerät aus, für die Kommunikation mit Ihrem Computer und der remote-überwachungslösung.
- Aktualisieren Sie den Beispielcode für die Geräte zur Verbindung mit der remote-überwachungslösung und Telemetriedaten Sie simulierte, die Sie im Projektmappen-Dashboard anzeigen können.

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Lernprogramm abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

> [!NOTE]
> Wenn Sie keines Konto haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion][lnk-free-trial].

### <a name="required-software"></a>Erforderliche Software

Sie benötigen die SSH-Client auf dem Desktopcomputer, für den Remotezugriff von der Befehlszeile auf die Brombeere Pi zu können.

- Windows umfasst einen SSH-Client nicht. Es wird empfohlen, [PuTTY](http://www.putty.org/).
- Die meisten Linux-Distributionen und Mac OS enthalten die SSH-Befehlszeile-Hilfsprogramm. Weitere Informationen finden Sie unter [SSH mithilfe von Linux oder Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-hardware"></a>Erforderliche hardware

Ein Desktopcomputer, um die Befehlszeile auf die Brombeere Pi herstellen können.

[Microsoft IoT Starter Kit für Brombeere Pi 3] [ lnk-starter-kits] oder entsprechende Komponenten. Dieses Lernprogramm verwendet die folgenden Elemente aus dem Kit:

- Brombeere Pi 3
- MicroSD-Karte (mit NOOBS)
- Ein USB-Mini-Kabel
- Ein Ethernet-Kabel

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
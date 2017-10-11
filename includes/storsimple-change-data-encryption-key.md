<!--author=SharS last changed: 12/01/15-->

### <a name="step-1-authorize-a-device-to-change-the-service-data-encryption-key-in-the-azure-classic-portal"></a>Schritt 1: Autorisieren eines Geräts so ändern Sie den Verschlüsselungsschlüssel für Dienstdaten im klassischen Azure-portal
In der Regel wird der Geräteadministrator angefordert, dass der Dienstadministrator ein Gerät so ändern Sie den Verschlüsselungsschlüssel für Dienstdaten zu autorisieren. Anschließend autorisiert der Dienstadministrator das Gerät den Schlüssel zu ändern.

Dieser Schritt wird im klassischen Azure-Portal ausgeführt. Der Dienstadministrator kann ein Gerät aus der angezeigten Liste der Geräte auswählen, die für die Autorisierung geeignet sind. Das Gerät wird dann autorisiert, um den Dienst Daten Änderungsvorgang des Verschlüsselungsschlüssels zu starten.

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Welche Geräte so ändern Sie den Verschlüsselungsschlüssel für Dienstdaten autorisiert werden können?
Ein Gerät muss die folgenden Kriterien erfüllen, bevor es autorisiert werden kann, initiieren der Dienst Daten Verschlüsselungsschlüssel wird geändert:

* Das Gerät muss online sein, um für Dienstdaten Verschlüsselung schlüsseländerung autorisiert berechtigt zu sein.
* Sie können das gleiche Gerät nach 30 Minuten erneut autorisieren, falls die schlüsseländerung nicht initiiert wurde.
* Sie können ein anderes Gerät autorisieren, vorausgesetzt, dass die schlüsseländerung nicht vom zuvor autorisierten Gerät initiiert wurde. Nachdem das neue Gerät autorisiert wurde, kann die Änderung nicht alte Gerät initiieren.
* Ein Gerät kann nicht autorisiert werden, während das Rollover des der Verschlüsselungsschlüssel für Dienstdaten ausgeführt wird.
* Sie können ein Gerät autorisieren, wenn einige der mit dem Dienst registrierten Geräte über die Verschlüsselung durchgeführt haben, während andere Benutzer keinen. In solchen Fällen sind die berechtigten Geräte an, dass diejenigen, die den Verschlüsselungsschlüssel für Dienstdaten abgeschlossen wurden geändert.

> [!NOTE]
> Im klassischen Azure-Portal werden virtuelle StorSimple-Geräte nicht in der Liste der Geräte angezeigt, die die schlüsseländerung autorisiert werden kann.
> 
> 

Führen Sie die folgenden Schritte aus, um auszuwählen, und Autorisieren eines Geräts, um die Verschlüsselung Änderung des Dienstdaten zu initiieren.

#### <a name="to-authorize-a-device-to-change-the-key"></a>Um ein Gerät so ändern Sie den Schlüssel zu autorisieren.
1. Klicken Sie auf der Dashboardseite auf **Verschlüsselungsschlüssel für Dienstdaten ändern**.
   
    ![Dienst-Verschlüsselungsschlüssel ändern](./media/storsimple-change-data-encryption-key/HCS_ChangeServiceDataEncryptionKey-include.png)
2. In der **Verschlüsselungsschlüssel für Dienstdaten ändern** Dialogfeld Feld, wählen Sie ein Gerät und Autorisieren der Verschlüsselung Änderung des Dienstdaten zu initiieren. Die Dropdown Liste weist allen qualifizierten Geräten, die autorisiert werden kann.
3. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-change-data-encryption-key/HCS_CheckIcon-include.png).

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Schritt 2: Verwenden von Windows PowerShell für StorSimple, um die Verschlüsselung Änderung des Dienstdaten zu initiieren.
Dieser Schritt wird in der Windows PowerShell für StorSimple-Schnittstelle auf dem autorisierten StorSimple-Gerät ausgeführt.

> [!NOTE]
> Bis der schlüsselrollover abgeschlossen ist, können keine Vorgänge im klassischen Azure-Portal von Ihrem StorSimple Manager-Dienst ausgeführt werden.
> 
> 

Wenn Sie die serielle Konsole des Geräts für die Verbindung zur Windows PowerShell-Schnittstelle verwenden, führen Sie die folgenden Schritte aus.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Um die Verschlüsselung Änderung des Dienstdaten zu initiieren.
1. Wählen Sie Option 1 mit Vollzugriff anzumelden.
2. Geben Sie in die Eingabeaufforderung Folgendes ein:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Nachdem das Cmdlet erfolgreich abgeschlossen wurde, erhalten Sie einen neuen Verschlüsselungsschlüssel für Dienstdaten. Kopieren Sie und speichern Sie diesen Schlüssel für die Verwendung in Schritt 3 dieses Prozesses. Dieser Schlüssel dienen zum Aktualisieren aller übrigen Geräte, die beim StorSimple Manager-Dienst registriert.
   
   > [!NOTE]
   > Dieser Vorgang muss innerhalb von vier Stunden nach der Autorisierung eines StorSimple-Geräts initiiert werden.
   > 
   > 
   
   Dieser neue Schlüssel wird dann an den Dienst gesendet, für alle Geräte verschoben werden, die mit dem Dienst registriert sind. Eine Warnung wird auf dem Service-Dashboard angezeigt. Der Dienst deaktiviert alle Vorgänge auf den registrierten Geräten und der Geräteadministrator müssen die Verschlüsselungsschlüssel für Dienstdaten auf anderen Geräten zu aktualisieren. Der e/a (Hosts, die Senden von Daten in der Cloud) wird jedoch nicht unterbrochen werden.
   
   Wenn Sie ein einzelnes Gerät beim Dienst registriert haben, der rolloverprozess jetzt abgeschlossen ist, und Sie können den nächsten Schritt überspringen. Wenn Sie mehrere Geräte beim Dienst registriert haben, fahren Sie mit Schritt 3 fort.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Schritt 3: Aktualisieren Sie den Verschlüsselungsschlüssel für Dienstdaten auf anderen StorSimple-Geräten
Diese Schritte müssen in der Windows PowerShell-Schnittstelle Ihres StorSimple-Geräts ausgeführt werden, wenn Sie mehrere Geräte für Ihr StorSimple Manager-Dienst registriert haben. Der Schlüssel, der in Schritt2 erfassten Clusterdatenträger: Verwenden von Windows PowerShell für StorSimple zum Initiieren der Verschlüsselung Änderung des Dienstdaten muss zum Aktualisieren aller übrigen StorSimple-Geräts mit StorSimple Manager-Dienst registriert.

Führen Sie die folgenden Schritte aus, um die dienstdatenverschlüsselung auf Ihrem Gerät zu aktualisieren.

#### <a name="to-update-the-service-data-encryption-key"></a>Den Dienst-Datenverschlüsselungsschlüssel aktualisieren
1. Verwenden Sie Windows PowerShell für StorSimple, um die Verbindung zur Konsole. Wählen Sie Option 1 mit Vollzugriff anzumelden.
2. Geben Sie in die Eingabeaufforderung Folgendes ein:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Geben Sie den Dienstdaten-Verschlüsselungsschlüssel, die Sie in erhalten [Schritt2: Verwenden von Windows PowerShell für StorSimple zum Initiieren der Verschlüsselung Änderung des Dienstdaten](#to-initiate-the-service-data-encryption-key-change).


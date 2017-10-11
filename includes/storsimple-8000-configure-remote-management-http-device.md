
#### <a name="to-configure-remote-management-on-cloud-appliance"></a>So konfigurieren die Remoteverwaltung auf Cloud-Anwendung

1. Klicken Sie in Ihrem StorSimple-Geräte-Manager-Dienst auf **Geräte**. Wählen Sie aus, und klicken Sie auf Ihre Cloud-Anwendung aus der Liste der Geräte, die mit dem Dienst verbunden.
    ![StorSimple wählen Cloud-Anwendung](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage1.png)

2. Wechseln Sie zu **Einstellungen > Sicherheit** So öffnen die **Sicherheitseinstellungen** Blatt.

     ![Sicherheitseinstellungen für StorSimple](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage2.png)

3. Wechseln Sie zu der **Remoteverwaltung** Abschnitt. Klicken Sie auf **Remoteverwaltung** Feld.
     ![StorSimple remote Verwaltungsdienstprogramms verwendet](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage3.png)

4. In der **Remoteverwaltung** Blatt:

    1. Stellen Sie sicher **Aktivieren der Remoteverwaltung** aktiviert ist.
    2. Der Standardwert ist für die Verbindung über HTTPS. Sie können auswählen, um eine Verbindung über HTTP. Herstellen einer Verbindung über HTTP ist nur in vertrauenswürdigen Netzwerken akzeptabel. Stellen Sie sicher, dass HTTP aktiviert ist.
    3. Klicken Sie auf der Befehlsleiste am oberen Rand Blatt auf **... Weitere** , und klicken Sie dann auf **Download Zertifikat** auf einem remote-Verwaltungszertifikat herunterladen. Sie können einen Speicherort zum Speichern dieser Datei angeben. Dieses Zertifikat sollte auf dem Computer Client- oder Hostcomputer installiert werden, mit denen Sie in der Cloud-Anwendung eine Verbindung herstellen.

        ![Blatt "Remote Verwaltungsdienstprogramms verwendet"](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage4.png)
5. Klicken Sie auf **speichern** und bestätigen Sie die Änderungen, wenn Sie dazu aufgefordert werden.
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-connect-through-the-serial-console"></a>Herstellen einer Verbindung über die serielle Konsole
1. Schließen Sie das serielle Kabel mit dem Gerät (direkt oder über einen seriellen USB-Adapter).
2. Öffnen Sie die **Systemsteuerung**, und öffnen Sie dann die **Geräte-Manager**.
3. Identifizieren Sie die COM-Port an, wie in der folgenden Abbildung dargestellt.
   
     ![Herstellen einer Verbindung über die serielle Konsole](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. Starten Sie PuTTY. 
5. Ändern Sie im rechten Bereich die **Verbindungstyp** auf **serielle**.
6. Geben Sie im rechten Bereich den entsprechenden COM-Port ein. Stellen Sie sicher, dass die Parameter für die serielle Konfiguration wie folgt festgelegt sind:
   
   * Geschwindigkeit: 115,200
   * Datenbits: 8
   * Stoppbits: 1
   * Parität: keine
   * Flusssteuerung: keine
     
     Diese Einstellungen werden in der folgenden Abbildung gezeigt.
     
     ![PuTTY-Einstellungen](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Wenn der Standardwert für die flusssteuerung nicht funktioniert, versuchen Sie, die flusssteuerung auf XON/XOFF festlegen.
     > 
     > 
7. Klicken Sie auf **öffnen** eine serielle Sitzung starten.


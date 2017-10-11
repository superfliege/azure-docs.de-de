1. Starten Sie den Azure Site Recovery-UnifiedSetup.exe
2. In **Vorbereitungen**Option **zusätzliche Prozessserver Bereitstellung für horizontales Skalieren hinzufügen**.

  ![Process-Server hinzufügen](./media/site-recovery-add-process-server/ps-page-1.png)

3. In **Konfigurationsserverdetails**, geben Sie die IP-Adresse von Konfigurationsserver und die Passphrase.

  ![Prozessserver 2 hinzufügen](./media/site-recovery-add-process-server/ps-page-2.png)
4. In **Interneteinstellungen**, angeben, wie der Anbieter auf dem Konfigurationsserver ausgeführt wird eine Verbindung mit Azure Site Recovery über das Internet herstellt.

  ![Prozessserver 3 hinzufügen](./media/site-recovery-add-process-server/ps-page-3.png)

   * Wenn die Verbindung mit dem Proxy, der derzeit auf dem Computer, wählen Sie eingerichtet werden sollen **Herstellen einer Verbindung mit vorhandenen Proxyeinstellungen**.
   * Wenn Sie den Anbieter, um direkt eine Verbindung herstellen möchten, wählen Sie **verbinden direkt ohne einen Proxy**.
   * Wenn die vorhandene Proxy eine Authentifizierung erfordert oder wenn Sie einen benutzerdefinierten Proxy für den Anbieterverbindungspunkt verwenden möchten **Verbinden mit benutzerdefinierten Proxyeinstellungen**.

     * Wenn Sie einen benutzerdefinierten Proxy verwenden, müssen Sie die Adresse, Port und Anmeldeinformationen angeben.
     * Wenn Sie einen Proxy verwenden, sollten haben Sie bereits Zugriff auf die Dienst-Urls zulässig.

5. In **Voraussetzungsprüfung**, Setup führt eine Überprüfung aus, um sicherzustellen, dass die Installation ausgeführt werden kann. Wenn eine Warnung angezeigt, über wird die **Überprüfung der globalen Zeit**, überprüfen Sie, ob die Zeit auf der Systemzeit (**Datum und Uhrzeit** Einstellungen) ist identisch mit der Zeitzone.

     ![Prozessserver 4 hinzufügen](./media/site-recovery-add-process-server/ps-page-4.png)

6. In **Umgebungsdetails**aus, ob die VMware-VMs repliziert werden sollen. Wenn Sie sind, klicken Sie dann Setup überprüft, ob PowerCLI 6.0 installiert ist.

     ![Prozessserver 5 hinzufügen](./media/site-recovery-add-process-server/ps-page-5.png)

7. In **Installationspfad**wählen, wobei Sie die Binärdateien installieren und den Cache speichern möchten. Das Laufwerk, das Sie auswählen, muss mindestens 5 GB freien Speicherplatz haben, aber ein Cachelaufwerk mit mindestens 600 GB freier Speicherplatz empfohlen.
     ![Prozessserver 5 hinzufügen](./media/site-recovery-add-process-server/ps-page-6.png)

8. In **Netzwerkauswahl**, geben Sie den Listener (Netzwerkadapter und SSL-Port) auf dem Konfigurationsserver sendet und empfängt Daten der Replikation. Port 9443 ist der Standardport zum Senden und Empfangen von Replikations-Datenverkehr verwendet, aber Sie können diese Portnummer entsprechend Ihrer Umgebung Anforderungen ändern. Zusätzlich zu den Port 9443 öffnen Sie auch über Port 443, der von einem Webserver verwendet wird, Replikationsvorgängen orchestriert. Verwenden Sie zum Senden oder Empfangen von Replikations-Datenverkehr nicht Port 443.

     ![Prozessserver 6 hinzufügen](./media/site-recovery-add-process-server/ps-page-7.png)
9. In **Zusammenfassung**, überprüfen Sie die Informationen, und klicken Sie auf **installieren**. Wenn die Installation abgeschlossen ist, wird eine Passphrase generiert. Sie benötigen diese, wenn Sie Aktivieren der Replikation, sodass ihn kopieren und an einem sicheren Ort aufbewahren.

     ![Prozessserver 7 hinzufügen](./media/site-recovery-add-process-server/ps-page-8.png)

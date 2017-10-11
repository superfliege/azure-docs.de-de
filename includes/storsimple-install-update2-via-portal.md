<!--author=alkohli last changed: 02/06/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>So installieren Sie ein Update aus dem Azure-portal

1. Wählen Sie auf der Seite StorSimple Ihres Geräts aus. Navigieren Sie zu **Geräte** > **Wartung**.
2. Klicken Sie am unteren Rand der Seite, auf **Updates Scannen**. Ein Auftrag wird erstellt, um nach verfügbaren Updates suchen. Sie werden benachrichtigt, wenn der Auftrag erfolgreich abgeschlossen wurde.
3. In der **Softwareupdates** Abschnitt auf derselben Seite der neue Softwareupdates verfügbar sind. Es wird empfohlen, dass Sie die Anmerkungen zu dieser Version sorgfältig lesen, bevor Sie ein Update auf Ihrem Gerät installieren.
4. Klicken Sie am unteren Rand der Seite, auf **Updates installieren**, und klicken Sie dann **OK**.
5. In der **Updates installieren** (Dialogfeld), stellen Sie sicher, dass Sie die Empfehlungen befolgt haben, und wählen Sie dann **ich mir die oben genannten Voraussetzung bewusst und zum upgrade meines Geräts bereit** , und klicken Sie auf die Schaltfläche "Überprüfen".
   
    ![Bestätigungsnachricht](./media/storsimple-install-update2-via-portal/InstallUpdate12_2M.png)
6. Eine Reihe von erforderlichen Komponenten überprüft gestartet wird. Diese Prüfungen enthalten:
   
   * **Controller Systemdiagnosen** sicherstellen, dass der Gerätecontroller fehlerfrei ist und die online sind.
   * **Hardware-Komponente Systemdiagnosen** , stellen Sie sicher, dass alle Hardwarekomponenten in Ihrem StorSimple-Gerät fehlerfrei sind.
   * **Hier wird überprüft, ob DATA 0** zu überprüfen, ob DATA 0 auf Ihrem Gerät aktiviert ist. Wenn diese Schnittstelle nicht aktiviert ist, müssen Sie sie aktivieren, und wiederholen Sie dann.
   * **DATA 2- und DATA 3-Überprüfungen** sicher, dass DATA 2- und DATA 3-Netzwerkschnittstellen nicht konfiguriert werden. Wenn diese Schnittstellen aktiviert sind, müssen Sie deaktivieren diese und dann versuchen, Ihr Gerät zu aktualisieren. Diese Überprüfung erfolgt nur, wenn Sie von einem Gerät ausgeführte GA-Software aktualisieren. Geräte mit Versionen 0,1 oder 0,2, 0,3 ist diese Überprüfung nicht erforderlich.
   * **Gateway-Kontrollkästchen** auf jedem Gerät eine Version vor Update 1. Diese Überprüfung erfolgt auf dem Gerät, die vor dem Update 1-Software ausgeführt jedoch ein Fehler auftritt, auf den Geräten, die ein Gateway für eine Netzwerkschnittstelle als DATA 0 konfiguriert haben.
     
     Das Update wird angewendet, wenn alle Überprüfungen erfolgreich abgeschlossen wurden. Sie werden benachrichtigt, wenn die Prüfungen ausgeführt werden.
     
     ![Bei der Vorprüfung-Benachrichtigung](./media/storsimple-install-update2-via-portal/InstallUpdate12_3M.png)
     
     Im folgenden finden ein Beispiel, in dem der Fehler bei Überprüfung. Sie müssen sicherstellen, dass der Gerätecontroller fehlerfrei und online sind. Sie müssen auch die Integrität der Hardwarekomponenten überprüfen. In diesem Beispiel Aufmerksamkeit Controller 0 und Controller 1-Komponenten. Möglicherweise müssen Sie den Microsoft Support wenden, wenn Sie diese Probleme von selbst lösen können.
     
       ![Fehler bei Überprüfung](./media/storsimple-install-update2-via-portal/HCS_PreUpgradeChecksFailed-include.png)
7. Wenn die Überprüfungen erfolgreich abgeschlossen wurden, wird ein updateauftrag erstellt. Sie werden benachrichtigt, wenn das Aktualisieren von Auftrag erfolgreich erstellt wurde.
   
    ![Aktualisieren von Auftrag erstellen](./media/storsimple-install-update2-via-portal/InstallUpdate12_44M.png)
   
    Das Update wird auf Ihrem Gerät angewendet.
    
8. Um den Fortschritt des Auftrags Update überwachen zu können, klicken Sie auf **Auftrag anzeigen**. Auf der **Aufträge** Seite sehen Sie den Status der Aktualisierung.
9. Das Update kann einige Stunden dauern. Wählen Sie das Update aus, und klicken Sie auf **Details** um die Details des Auftrags zu einem beliebigen Zeitpunkt anzuzeigen.
10. Nachdem der Auftrag abgeschlossen ist, wechseln Sie in der **Wartung** Seite, und führen Sie einen Bildlauf nach unten bis zum **Softwareupdates**.

